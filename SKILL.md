---
name: spire-api
description: "C#/.NET development against the Spire ERP REST API and underlying PostgreSQL database. Use when building Spire integrations, querying inventory, customers, sales, purchasing, or accounting."
---

# Spire API - C#/.NET Development Skill

You are an expert at building C#/.NET applications that integrate with the Spire ERP REST API. When helping users write code against this API, follow the conventions and patterns below.

## API Fundamentals

- **Base URL:** `https://{server}:{port}/api/v2/companies/{company-name}`
- **Auth:** HTTP Basic Authentication (`Authorization: Basic base64(username:password)`)
- **Content-Type:** `application/json` for all request/response bodies
- **Date Format:** ISO 8601 (`YYYY-MM-DD`)

## C# HttpClient Setup Pattern

Always configure the Spire API client using this pattern:

```csharp
using System.Net.Http.Headers;
using System.Text;
using System.Text.Json;

// Spire often uses self-signed certs in on-premise deployments.
// IMPORTANT: Disable auto-redirect because Spire may redirect requests,
// and .NET's HttpClient strips the Authorization header on redirect.
var handler = new HttpClientHandler
{
    ServerCertificateCustomValidationCallback =
        HttpClientHandler.DangerousAcceptAnyServerCertificateValidator,
    AllowAutoRedirect = false
};

var auth = new AuthenticationHeaderValue(
    "Basic",
    Convert.ToBase64String(Encoding.UTF8.GetBytes("username:password"))
);

var client = new HttpClient(handler);
client.BaseAddress = new Uri("https://your-server:10880/api/v2/companies/yourcompany/");
client.DefaultRequestHeaders.Authorization = auth;
```

**Trailing slash on collection URLs (CRITICAL):** All collection endpoints (e.g. `customers`, `inventory/items`, `sales/orders`) **must** have a trailing slash before the query string, or Spire will respond with a 301/302 redirect to the slash-suffixed URL. Combined with the auto-redirect behavior, this will cause the `Authorization` header to be stripped and the retried request will return 401 Unauthorized.

```csharp
// WRONG — Spire will redirect, auth header gets stripped
var response = await client.GetAsync("customers?limit=10");

// CORRECT — trailing slash before the query string
var response = await client.GetAsync("customers/?limit=10");
var response = await client.GetAsync("inventory/items/?filter=...");
var response = await client.GetAsync("sales/orders/?start=0&limit=100");
```

This applies to all collection GETs. Single-resource URLs (e.g. `customers/123`) do not need a trailing slash.

**Redirect handling:** Since `AllowAutoRedirect = false`, you must follow redirects manually and re-attach the `Authorization` header. Use this helper:

```csharp
async Task<HttpResponseMessage> SendWithAuthAsync(HttpClient client, string url,
    AuthenticationHeaderValue auth)
{
    var response = await client.GetAsync(url);
    int remaining = 5;
    while (remaining-- > 0 && IsRedirect(response.StatusCode)
        && response.Headers.Location is not null)
    {
        var redirectUri = response.Headers.Location;
        response.Dispose();
        var request = new HttpRequestMessage(HttpMethod.Get, redirectUri);
        request.Headers.Authorization = auth;
        response = await client.SendAsync(request);
    }
    return response;
}

bool IsRedirect(System.Net.HttpStatusCode code) =>
    code is System.Net.HttpStatusCode.MovedPermanently
        or System.Net.HttpStatusCode.Found
        or System.Net.HttpStatusCode.TemporaryRedirect
        or System.Net.HttpStatusCode.PermanentRedirect;
```

For production applications, extract connection settings into configuration:

```csharp
public class SpireApiSettings
{
    public string BaseUrl { get; set; } = "";
    public string Company { get; set; } = "";
    public string Username { get; set; } = "";
    public string Password { get; set; } = "";
}
```

## JSON Serialization

Use `System.Text.Json` with camelCase naming to match the API's field naming convention:

```csharp
var jsonOptions = new JsonSerializerOptions
{
    PropertyNamingPolicy = JsonNamingPolicy.CamelCase,
    PropertyNameCaseInsensitive = true,
    DefaultIgnoreCondition = JsonIgnoreCondition.WhenWritingNull
};
```

**Important:** The Spire API returns many numeric fields (quantities, prices, totals) as **JSON strings** rather than numbers (e.g. `"29.9900"` instead of `29.99`). When deserializing into C# `decimal` properties, use a custom converter:

```csharp
public class StringDecimalConverter : JsonConverter<decimal>
{
    public override decimal Read(ref Utf8JsonReader reader, Type typeToConvert,
        JsonSerializerOptions options) =>
        reader.TokenType == JsonTokenType.String
            ? decimal.Parse(reader.GetString()!)
            : reader.GetDecimal();

    public override void Write(Utf8JsonWriter writer, decimal value,
        JsonSerializerOptions options) =>
        writer.WriteNumberValue(value);
}
```

Apply it to decimal properties on your models:

```csharp
[JsonConverter(typeof(StringDecimalConverter))]
public decimal SubTotal { get; set; }
```

## API Response Shape

All list endpoints return this structure:

```csharp
public class SpireListResponse<T>
{
    public List<T> Records { get; set; } = new();
    public int Start { get; set; }
    public int Limit { get; set; }
    public int Count { get; set; }
}
```

`Count` is the total number of matching records (not the page size). Use it to determine if more pages exist.

## Pagination

- Query params: `start` (offset, default 0) and `limit` (page size, default 10, max 10000)
- To fetch all records, loop until `start >= count`

```csharp
var allRecords = new List<T>();
int start = 0;
const int limit = 100;

while (true)
{
    var response = await client.GetFromJsonAsync<SpireListResponse<T>>(
        $"customers?start={start}&limit={limit}", jsonOptions);
    allRecords.AddRange(response.Records);
    start += limit;
    if (start >= response.Count)
        break;
}
```

## Filtering

Filters are passed as URL-encoded JSON in the `filter` query parameter.

**Simple equality:**
```csharp
var filter = Uri.EscapeDataString("""{"status":"A"}""");
var url = $"customers?filter={filter}";
```

**Operators** (prefix with `$`):

| Operator | Description | Example |
|----------|-------------|---------|
| `$eq` | Equals | `{"field": {"$eq": "value"}}` |
| `$ne` | Not equals | `{"field": {"$ne": 12}}` |
| `$gt` / `$gte` | Greater than (or equal) | `{"field": {"$gt": 100}}` |
| `$lt` / `$lte` | Less than (or equal) | `{"field": {"$lt": 100}}` |
| `$in` | In array | `{"field": {"$in": ["a","b"]}}` |
| `$like` | SQL LIKE (`%` wildcard) | `{"field": {"$like": "ABC%"}}` |
| `$contains` | Contains substring | `{"field": {"$contains": "test"}}` |
| `$icontains` | Contains (case-insensitive) | `{"field": {"$icontains": "test"}}` |
| `$startswith` | Starts with | `{"field": {"$startswith": "AB"}}` |
| `$or` | Logical OR | `{"$or": [{"f1": 1}, {"f2": 2}]}` |
| `$and` | Logical AND | `{"$and": [cond1, cond2]}` |

**Building filters in C#:**

```csharp
// Use a dictionary/anonymous object and serialize to JSON for the filter value
var filter = JsonSerializer.Serialize(new
{
    status = "A",
    territory = new { code = "WEST" }
});
var url = $"customers?filter={Uri.EscapeDataString(filter)}";
```

**Date range filter:**
```csharp
var filter = JsonSerializer.Serialize(new
{
    orderDate = new Dictionary<string, string>
    {
        ["$gte"] = "2024-01-01",
        ["$lte"] = "2024-01-31"
    }
});
```

## Sorting

- `sort=fieldName` for ascending, `sort=-fieldName` for descending
- Multiple: `sort=-orderDate&sort=customerNo`

## Field Selection

- `fields=field1,field2,field3` limits returned fields
- Nested fields use dot notation: `fields=id,name,address.city,udf.CustomField`

## User Defined Fields (UDFs)

- Add `udf=1` to include all UDF values in the response
- Select specific UDFs: `fields=name,udf.MyField`
- Set UDF values in PUT/POST by including a `udf` object:
  ```json
  {"udf": {"custom_field": "value"}}
  ```

## Common Entity Patterns

### References by ID or Code

Most entities can be referenced by ID (integer) or by their natural key:

```csharp
// By ID
new { customer = new { id = 123 } }

// By natural key
new { customer = new { customerNo = "CUST001" } }
```

### Persisting links to sales/purchase line items: prefer GUID over ID

If you are storing a foreign reference to a sales order item or purchase order item in your own table, **store the line's `guid`, not its integer `id`**.

When a sales order is invoiced (or a purchase order is received and closed), Spire moves the records from the working tables to the history tables:

| Working table | History table |
|---|---|
| `sales_order_items` | `sales_history_items` |
| `purchase_order_items` | `purchase_history_items` |

The integer `id` of the line **changes** during this move, but the `guid` column (`character varying(32)`) is **stable** across the move and is the same value Spire itself uses to link related rows (`inventory_serial_transactions.link_guid`, `inventory_receipts.link_guid`, `purchase_receipts.guid`, etc.).

A reference stored as `id` will silently break the moment the order is invoiced/received. A reference stored as `guid` keeps working — you just need to look in the history table instead of the working table.

**Important caveat:** the `guid` column is **not exposed by the Spire REST API** on sales/purchase line items. You can only read it via direct database access. Typical pattern: fetch the order from the REST API as usual, then issue a small DB query (`SELECT id, guid FROM sales_order_items WHERE id = ANY(@ids)`) to enrich the items with their guids before returning them to your callers.

### Status Codes

| Entity | Active | Inactive | Other |
|--------|--------|----------|-------|
| Customer | A | I | H (Hold) |
| Inventory Item | A | I | O (Obsolete) |
| Sales Order | O (Open) | C (Closed) | L (Layaway), P (Picking), S (Shipped) |
| Purchase Order | O (Open) | C (Closed) | I (Issued), R (Received) |
| Production Order | O (Open) | C (Complete) | I (In Progress) |
| Employee | A | I | T (Terminated) |
| Job | A | I | C (Complete) |

### POST/PUT Conventions

- **POST** creates a new record. Returns **201 Created** with a `Location` header pointing to the new resource. The response body is empty — you must GET the `Location` URL to retrieve the created record.
- **PUT** updates an existing record. Include only the fields to change.
- For items/entries arrays on PUT, include the `id` of existing line items to update them.
- **Do not send server-assigned fields** (e.g. `id`, `orderNo`, `status`) on POST. If your C# model includes these fields, exclude them from serialization using `[JsonIgnore(Condition = JsonIgnoreCondition.WhenWritingDefault)]` or use `WhenWritingNull` with nullable types. Sending `"id": null` or `"id": 0` can cause validation errors.

### POST Response Pattern (201 + Location Header)

```csharp
// POST returns 201 with empty body — follow the Location header
var response = await client.PostAsync("sales/orders/", content, ct);
// ... check for errors ...

var location = response.Headers.Location;
var getResponse = await client.GetAsync(location, ct);
var created = await getResponse.Content.ReadFromJsonAsync<SpireSalesOrder>(jsonOptions, ct);
```

**Exception:** Some action endpoints (e.g. `POST /sales/orders/{id}/invoice`) return **200** with the result directly in the response body.

### Address Fields

The `provState` field is `varchar(3)` — use short province/state codes (e.g. `"BC"`, `"KY"`), not full names. The `country` field expects short codes (e.g. `"US"`, `"CA"`), not full names like `"United States"` or `"Canada"`.

## Error Handling

Spire returns errors as JSON with this structure:

```json
{
  "type": "error",
  "message": "Customer is missing or invalid",
  "error_type": "RequiredFieldError"
}
```

- `type` is always `"error"`
- `message` describes what went wrong
- `error_type` varies (e.g. `RequiredFieldError`, `DataError`, `ValidationError`)

```csharp
var response = await client.GetAsync(url);
if (!response.IsSuccessStatusCode)
{
    var body = await response.Content.ReadAsStringAsync();
    // Parse Spire's error JSON for a meaningful message
    try
    {
        var error = JsonSerializer.Deserialize<SpireErrorResponse>(body, jsonOptions);
        var message = error?.Message ?? body;
        if (!string.IsNullOrEmpty(error?.ErrorType))
            message = $"{error.ErrorType}: {message}";
        throw new HttpRequestException(
            $"Spire API error {(int)response.StatusCode}: {message}");
    }
    catch (JsonException)
    {
        throw new HttpRequestException(
            $"Spire API error {(int)response.StatusCode}: {body}");
    }
}
```

| Code | Description |
|------|-------------|
| 200 | Success |
| 201 | Created (follow Location header to GET the new resource) |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden (insufficient permissions) |
| 404 | Not Found |
| 422 | Unprocessable Entity (validation/semantic errors) |
| 423 | Locked (record locked by another user) |
| 500 | Server Error |

## Dependency Injection Pattern

For ASP.NET Core or hosted service applications:

```csharp
builder.Services.AddHttpClient("SpireApi", (sp, client) =>
{
    var settings = sp.GetRequiredService<IOptions<SpireApiSettings>>().Value;
    client.BaseAddress = new Uri($"{settings.BaseUrl}/api/v2/companies/{settings.Company}/");
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue(
        "Basic",
        Convert.ToBase64String(Encoding.UTF8.GetBytes($"{settings.Username}:{settings.Password}"))
    );
})
.ConfigurePrimaryHttpMessageHandler(() => new HttpClientHandler
{
    ServerCertificateCustomValidationCallback =
        HttpClientHandler.DangerousAcceptAnyServerCertificateValidator,
    AllowAutoRedirect = false // Spire may redirect; redirects strip the Authorization header
});
```

**Note:** With `AllowAutoRedirect = false`, your service layer must follow redirects manually and re-attach the `Authorization` header on each hop. See the redirect handling helper in the HttpClient Setup Pattern section above.

## API Reference Documentation

Consult these files for endpoint details, request/response schemas, and field definitions:

- [docs/00-api-overview.md](docs/00-api-overview.md) - Authentication, filtering, pagination, sorting, UDFs, error codes
- [docs/01-customers.md](docs/01-customers.md) - Customer CRUD, addresses, contacts, payments, credit, notes
- [docs/02-sales-orders.md](docs/02-sales-orders.md) - Sales orders, line items, payments, order-to-invoice conversion
- [docs/03-sales-invoices.md](docs/03-sales-invoices.md) - Sales invoice retrieval, communications, notes
- [docs/04-sales-batches-taxes.md](docs/04-sales-batches-taxes.md) - Sales batches, taxes, salespeople, territories
- [docs/05-inventory.md](docs/05-inventory.md) - Inventory items, adjustments, counts, transfers, warehouses, UOMs, UPCs, serials
- [docs/06-purchasing.md](docs/06-purchasing.md) - Purchase orders, issue/receive actions, vendor management, history
- [docs/07-accounts-payable.md](docs/07-accounts-payable.md) - AP aged, transactions (invoice/payment/credit), vendors
- [docs/08-accounts-receivable.md](docs/08-accounts-receivable.md) - AR aged, transactions (payment/credit/debit)
- [docs/09-general-ledger.md](docs/09-general-ledger.md) - GL accounts, journal entries, fiscal periods, year-end
- [docs/10-production.md](docs/10-production.md) - Production orders, templates, BOM, components
- [docs/11-payroll.md](docs/11-payroll.md) - Employees, timecards, departments, T4s, ROEs
- [docs/12-job-costing.md](docs/12-job-costing.md) - Jobs, cost entries, accounts, budgets
- [docs/13-crm.md](docs/13-crm.md) - Notes, note types, attachments
- [docs/14-email.md](docs/14-email.md) - Email messages, batches, templates, attachments
- [docs/15-core-resources.md](docs/15-core-resources.md) - Companies, users, currencies, payment methods/terms, shipping

For C# code examples covering common workflows, see [examples.md](examples.md).

## Database Direct Access

When the REST API does not support a required operation (e.g. complex reporting queries, bulk reads, cross-table joins), you can query the underlying PostgreSQL database directly. The database schema is `public` (v3).

**Connection:** Use `Npgsql` for .NET PostgreSQL access:

```csharp
using Npgsql;

var connStr = "Host=your-server;Port=5432;Database=yourcompany;Username=user;Password=pass";
await using var conn = new NpgsqlConnection(connStr);
await conn.OpenAsync();
```

**Key conventions:**
- All tables have an `id` (integer, PK) and audit columns: `_created`, `_created_by`, `_modified`, `_modified_by`, `_dbversion`
- Timestamps are UTC (`timestamp without time zone`)
- User Defined Fields are stored in `udf_data` columns (PostgreSQL `hstore` type)
- Periodic financial data (sales by period, GP by period) uses PostgreSQL arrays: `numeric(15,2)[]` with 13 elements (periods 1-13)
- Foreign key relationships are documented in column comments (e.g. "links to payment_terms.terms_code")

### Database Data Dictionary

Consult these files for complete column definitions, data types, and constraints:

- [docs/db-addresses.md](docs/db-addresses.md) - addresses, address_contacts, address_contact_types
- [docs/db-customers.md](docs/db-customers.md) - customers, customer_code_changes, customer_part_nos
- [docs/db-sales-orders.md](docs/db-sales-orders.md) - sales_orders, sales_order_items, sales_order_payments, sales_order_equipment, batches, departments, salespeople, territories, tills
- [docs/db-sales-history.md](docs/db-sales-history.md) - sales_history, sales_history_items, sales_history_payments, sales_history_equipment, sales_taxes
- [docs/db-inventory.md](docs/db-inventory.md) - inventory, adjustments, comments, components, counts, images, labels, levies, lot trace, price levels, price matrix, product codes, promo codes, receipts, requisitions, sell prices, serial numbers, statistics, transfers, UOMs, UPC codes, warehouses
- [docs/db-purchasing.md](docs/db-purchasing.md) - purchase_orders, purchase_order_items, purchase_history, purchase_receipts, vendors, vendor_pricing
- [docs/db-accounts-payable.md](docs/db-accounts-payable.md) - ap_batches, ap_batch_items, ap_transactions, ap_transaction_links
- [docs/db-accounts-receivable.md](docs/db-accounts-receivable.md) - ar_batches, ar_batch_items, ar_transactions, ar_transaction_links
- [docs/db-general-ledger.md](docs/db-general-ledger.md) - gl_accounts, gl_allocations, gl_divisions, gl_groups, gl_periods, gl_segments, gl_subgroups, gl_transactions, gl_reconciliations, gl_recurring, gl_special_accounts, plus all gl_history_* tables
- [docs/db-production.md](docs/db-production.md) - production_orders, production_order_items, production_templates, production_history, bom_categories, bom_item_replacements
- [docs/db-payroll.md](docs/db-payroll.md) - employees, employee_roes, employee_t4s, payroll_depts, payroll_schedules, payroll_timecards, payroll_timecard_entries
- [docs/db-jobs.md](docs/db-jobs.md) - jobs, job_items, phases, phase_history
- [docs/db-notes.md](docs/db-notes.md) - notes, note_attachments, note_types
- [docs/db-system.md](docs/db-system.md) - system_settings, system_users_base, system_filters, record_types, udf_fields, udf_pages, countries, currencies, email_templates, equipment, payment_methods, payment_terms, shipping_methods, cash_outs, integrations
