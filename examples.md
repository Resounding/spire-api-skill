# Spire API - C#/.NET Code Examples

## Setup (Common to All Examples)

```csharp
using System.Net;
using System.Net.Http.Headers;
using System.Net.Http.Json;
using System.Text;
using System.Text.Json;
using System.Text.Json.Serialization;

// IMPORTANT: Disable auto-redirect because Spire may redirect requests,
// and .NET's HttpClient strips the Authorization header on redirect.
var handler = new HttpClientHandler
{
    ServerCertificateCustomValidationCallback =
        HttpClientHandler.DangerousAcceptAnyServerCertificateValidator,
    AllowAutoRedirect = false
};

var auth = new AuthenticationHeaderValue(
    "Basic", Convert.ToBase64String(Encoding.UTF8.GetBytes("username:password")));

var client = new HttpClient(handler)
{
    BaseAddress = new Uri("https://your-server:10880/api/v2/companies/yourcompany/")
};
client.DefaultRequestHeaders.Authorization = auth;

var json = new JsonSerializerOptions
{
    PropertyNamingPolicy = JsonNamingPolicy.CamelCase,
    PropertyNameCaseInsensitive = true,
    DefaultIgnoreCondition = JsonIgnoreCondition.WhenWritingNull
};

// Helper: follow redirects while preserving the Authorization header
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

bool IsRedirect(HttpStatusCode code) =>
    code is HttpStatusCode.MovedPermanently
        or HttpStatusCode.Found
        or HttpStatusCode.TemporaryRedirect
        or HttpStatusCode.PermanentRedirect;
```

## Response Model

```csharp
public class SpireListResponse<T>
{
    public List<T> Records { get; set; } = new();
    public int Start { get; set; }
    public int Limit { get; set; }
    public int Count { get; set; }
}
```

---

## Customers

### Get Customers with Filtering and Pagination

```csharp
// Get active customers in the WEST territory, page by page
var filter = Uri.EscapeDataString(
    JsonSerializer.Serialize(new { status = "A", territory = new { code = "WEST" } }));

var result = await client.GetFromJsonAsync<SpireListResponse<JsonElement>>(
    $"customers?filter={filter}&start=0&limit=50&sort=name", json);

foreach (var customer in result.Records)
{
    Console.WriteLine($"{customer.GetProperty("customerNo")} - {customer.GetProperty("name")}");
}
Console.WriteLine($"Showing {result.Records.Count} of {result.Count} total");
```

### Search Customers by Name

```csharp
var filter = Uri.EscapeDataString(
    JsonSerializer.Serialize(new { name = new { ["\u0024icontains"] = "acme" } }));

var result = await client.GetFromJsonAsync<SpireListResponse<JsonElement>>(
    $"customers?filter={filter}&fields=id,customerNo,name,address.city", json);
```

### Create a Customer

```csharp
var newCustomer = new
{
    customerNo = "NEWCUST",
    name = "New Customer Inc.",
    status = "A",
    address = new
    {
        line1 = "123 Main Street",
        city = "Vancouver",
        provState = "BC",
        postalCode = "V6B 1A1",
        country = "Canada"
    },
    contact = new
    {
        name = "John Smith",
        email = "john@newcustomer.com",
        phone = "604-555-1234"
    },
    termsCode = "NET30",
    creditLimit = "50000.00"
};

var response = await client.PostAsJsonAsync("customers", newCustomer, json);
response.EnsureSuccessStatusCode();
var created = await response.Content.ReadFromJsonAsync<JsonElement>(json);
Console.WriteLine($"Created customer ID: {created.GetProperty("id")}");
```

### Update a Customer

```csharp
var update = new
{
    creditLimit = "75000.00",
    contact = new { email = "newemail@customer.com" }
};

var response = await client.PutAsJsonAsync("customers/123", update, json);
response.EnsureSuccessStatusCode();
```

---

## Inventory

### Get Inventory Items with UDFs

```csharp
var result = await client.GetFromJsonAsync<SpireListResponse<JsonElement>>(
    "inventory/items?filter=%7B%22status%22%3A%22A%22%7D&udf=1&start=0&limit=100&sort=partNo",
    json);

foreach (var item in result.Records)
{
    var partNo = item.GetProperty("partNo").GetString();
    var desc = item.GetProperty("description").GetString();
    Console.WriteLine($"{partNo}: {desc}");

    if (item.TryGetProperty("udf", out var udf))
    {
        foreach (var field in udf.EnumerateObject())
            Console.WriteLine($"  UDF {field.Name} = {field.Value}");
    }
}
```

### Create an Inventory Item

```csharp
var newItem = new
{
    whse = "00",
    partNo = "WIDGET-NEW",
    description = "New Widget Product",
    type = "N",
    status = "A",
    defaultSellPrice = "29.99",
    currentCost = "14.75",
    sellMeasure = "EA",
    purchaseMeasure = "EA",
    allowBackorders = true,
    udf = new { country_of_origin = "Canada" }
};

var response = await client.PostAsJsonAsync("inventory/items", newItem, json);
response.EnsureSuccessStatusCode();
```

### Create an Inventory Adjustment and Post It

```csharp
// Step 1: Create the adjustment
var adjustment = new
{
    whse = "00",
    date = "2024-01-15",
    reference = "ADJ-001",
    items = new[]
    {
        new { inventory = new { partNo = "WIDGET-A" }, qty = "10", unitCost = "15.00" },
        new { inventory = new { partNo = "GADGET-B" }, qty = "-5", unitCost = "25.00" }
    }
};

var createResp = await client.PostAsJsonAsync("inventory/adjustments", adjustment, json);
createResp.EnsureSuccessStatusCode();
var created = await createResp.Content.ReadFromJsonAsync<JsonElement>(json);
var adjId = created.GetProperty("id").GetInt32();

// Step 2: Post it to update inventory quantities
var postResp = await client.PostAsJsonAsync($"inventory/adjustments/{adjId}/post",
    new { }, json);
postResp.EnsureSuccessStatusCode();
```

### Transfer Inventory Between Warehouses

```csharp
var transfer = new
{
    fromWhse = "00",
    toWhse = "01",
    date = DateTime.Today.ToString("yyyy-MM-dd"),
    reference = "TRF-001",
    items = new[]
    {
        new { inventory = new { partNo = "WIDGET-A" }, qty = "25" }
    }
};

var createResp = await client.PostAsJsonAsync("inventory/transfers", transfer, json);
createResp.EnsureSuccessStatusCode();
var created = await createResp.Content.ReadFromJsonAsync<JsonElement>(json);
var transferId = created.GetProperty("id").GetInt32();

// Post the transfer
await client.PostAsJsonAsync($"inventory/transfers/{transferId}/post", new { }, json);
```

---

## Sales Orders

### Create a Sales Order

```csharp
var order = new
{
    customer = new { customerNo = "ACTTEC" },
    orderDate = "2024-01-15",
    requiredDate = "2024-01-22",
    items = new[]
    {
        new
        {
            inventory = new { whse = "00", partNo = "WIDGET-A" },
            description = "Widget Type A",
            orderQty = "5.0000",
            unitPrice = "29.9900"
        },
        new
        {
            inventory = new { whse = "00", partNo = "GADGET-B" },
            description = "Gadget Type B",
            orderQty = "10.0000",
            unitPrice = "15.0000"
        }
    }
};

var response = await client.PostAsJsonAsync("sales/orders", order, json);
response.EnsureSuccessStatusCode();
var created = await response.Content.ReadFromJsonAsync<JsonElement>(json);
Console.WriteLine($"Order ID: {created.GetProperty("id")}, Order #: {created.GetProperty("orderNo")}");
```

### Get Open Sales Orders for a Customer

```csharp
var filter = Uri.EscapeDataString(JsonSerializer.Serialize(new
{
    status = "O",
    customer = new { customerNo = "ACTTEC" }
}));

var result = await client.GetFromJsonAsync<SpireListResponse<JsonElement>>(
    $"sales/orders?filter={filter}&sort=-orderDate&fields=id,orderNo,orderDate,total", json);
```

### Convert a Sales Order to Invoice

```csharp
var response = await client.PostAsJsonAsync("sales/orders/12345/invoice",
    new { invoiceDate = DateTime.Today.ToString("yyyy-MM-dd") }, json);
response.EnsureSuccessStatusCode();
```

---

## Purchasing

### Create a Purchase Order

```csharp
var po = new
{
    vendor = new { vendorNo = "ACMSYS" },
    date = DateTime.Today.ToString("yyyy-MM-dd"),
    requiredDate = DateTime.Today.AddDays(7).ToString("yyyy-MM-dd"),
    whse = "00",
    items = new[]
    {
        new
        {
            inventory = new { whse = "00", partNo = "WIDGET-A" },
            orderQty = "100",
            purchaseMeasure = "EA",
            unitPrice = "12.50"
        }
    }
};

var response = await client.PostAsJsonAsync("purchasing/orders", po, json);
response.EnsureSuccessStatusCode();
var created = await response.Content.ReadFromJsonAsync<JsonElement>(json);
var poId = created.GetProperty("id").GetInt32();
```

### Issue and Receive a Purchase Order

```csharp
// Issue (send to vendor)
await client.PostAsJsonAsync($"purchasing/orders/{poId}/issue", new { }, json);

// Receive items (partial or full)
var receivePayload = new
{
    items = new[]
    {
        new { id = 1001, receiveQty = "100" }
    }
};
await client.PostAsJsonAsync($"purchasing/orders/{poId}/receive", receivePayload, json);
```

---

## Accounts Payable

### Create an AP Invoice with GL Distribution

```csharp
var apInvoice = new
{
    type = "I",
    vendor = new { vendorNo = "VENDOR001" },
    date = "2024-01-15",
    dueDate = "2024-02-14",
    invoiceNo = "INV-12345",
    amount = "1500.00",
    description = "Office supplies",
    glEntries = new[]
    {
        new { account = new { accountNo = "5000" }, amount = "1500.00" }
    }
};

var response = await client.PostAsJsonAsync("ap/transactions", apInvoice, json);
response.EnsureSuccessStatusCode();
```

### Apply a Payment to an AP Invoice

```csharp
// Create the payment
var payment = new
{
    type = "P",
    vendor = new { vendorNo = "VENDOR001" },
    date = "2024-02-14",
    amount = "1500.00",
    reference = "CHQ-5678",
    paymentMethod = new { id = 1 }
};
var payResp = await client.PostAsJsonAsync("ap/transactions", payment, json);
var payResult = await payResp.Content.ReadFromJsonAsync<JsonElement>(json);
var paymentId = payResult.GetProperty("id").GetInt32();

// Apply payment to invoice
await client.PostAsync($"ap/transactions/{paymentId}/apply/{invoiceId}", null);
```

---

## General Ledger

### Create a Journal Entry

```csharp
// Debits must equal credits
var journalEntry = new
{
    date = "2024-01-15",
    reference = "JE-001",
    memo = "Monthly adjustment",
    entries = new[]
    {
        new { account = new { accountNo = "1000" }, debit = "1000.00", credit = "0.00" },
        new { account = new { accountNo = "3000" }, debit = "0.00", credit = "1000.00" }
    }
};

var response = await client.PostAsJsonAsync("gl/transactions", journalEntry, json);
response.EnsureSuccessStatusCode();
```

---

## Production

### Create a Production Order from Template

```csharp
var prodOrder = new
{
    template = new { id = 1 },
    qty = "100",
    whse = "00",
    requiredDate = "2024-01-22"
};

var response = await client.PostAsJsonAsync("production/orders", prodOrder, json);
response.EnsureSuccessStatusCode();
```

### Create a Production Order with Custom BOM

```csharp
var prodOrder = new
{
    finishedGood = new { partNo = "ASSEMBLED-WIDGET" },
    qty = "50",
    whse = "00",
    date = DateTime.Today.ToString("yyyy-MM-dd"),
    items = new[]
    {
        new { inventory = new { partNo = "COMPONENT-A" }, qty = "100", unitCost = "5.00" },
        new { inventory = new { partNo = "COMPONENT-B" }, qty = "50", unitCost = "10.00" }
    }
};

var response = await client.PostAsJsonAsync("production/orders", prodOrder, json);
response.EnsureSuccessStatusCode();
```

---

## Payroll

### Create an Employee

```csharp
var employee = new
{
    employeeNo = "EMP001",
    firstName = "John",
    lastName = "Smith",
    status = "A",
    department = new { id = 1 },
    hireDate = "2024-01-15",
    payType = "S",
    payRate = "50000.00",
    payFrequency = "B",
    address = new
    {
        line1 = "123 Main St",
        city = "Vancouver",
        provState = "BC",
        postalCode = "V6B 1A1",
        country = "Canada"
    }
};

var response = await client.PostAsJsonAsync("payroll/employees", employee, json);
response.EnsureSuccessStatusCode();
```

---

## Generic Pagination Helper

```csharp
public static async Task<List<JsonElement>> GetAllRecordsAsync(
    HttpClient client,
    string endpoint,
    string? filter = null,
    string? sort = null,
    string? fields = null,
    int pageSize = 100,
    JsonSerializerOptions? jsonOptions = null)
{
    var allRecords = new List<JsonElement>();
    int start = 0;

    while (true)
    {
        var queryParts = new List<string>
        {
            $"start={start}",
            $"limit={pageSize}"
        };
        if (filter != null)
            queryParts.Add($"filter={Uri.EscapeDataString(filter)}");
        if (sort != null)
            queryParts.Add($"sort={sort}");
        if (fields != null)
            queryParts.Add($"fields={fields}");

        var url = $"{endpoint}?{string.Join("&", queryParts)}";
        var page = await client.GetFromJsonAsync<SpireListResponse<JsonElement>>(
            url, jsonOptions);

        allRecords.AddRange(page!.Records);
        start += pageSize;
        if (start >= page.Count)
            break;
    }

    return allRecords;
}

// Usage
var allActiveCustomers = await GetAllRecordsAsync(
    client,
    "customers",
    filter: """{"status":"A"}""",
    sort: "name",
    fields: "id,customerNo,name"
);
```

---

## Typed Model Example

For strongly-typed access, define C# record/class models.

**Important:** The Spire API returns many numeric fields as JSON strings (e.g. `"29.9900"` instead of `29.99`). Use a custom `JsonConverter` for `decimal` properties:

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

Then apply it to your models:

```csharp
public record Customer
{
    public int Id { get; init; }
    public string CustomerNo { get; init; } = "";
    public string Name { get; init; } = "";
    public string Status { get; init; } = "";
    public Address? Address { get; init; }
    public Contact? Contact { get; init; }

    [JsonConverter(typeof(StringDecimalConverter))]
    public decimal CreditLimit { get; init; }

    public string? TermsCode { get; init; }
    public Dictionary<string, JsonElement>? Udf { get; init; }
}

public record Address
{
    public string? Line1 { get; init; }
    public string? Line2 { get; init; }
    public string? City { get; init; }
    public string? ProvState { get; init; }
    public string? PostalCode { get; init; }
    public string? Country { get; init; }
}

public record Contact
{
    public string? Name { get; init; }
    public string? Email { get; init; }
    public string? Phone { get; init; }
}

// Usage
var result = await client.GetFromJsonAsync<SpireListResponse<Customer>>(
    "customers?start=0&limit=50", json);

foreach (var c in result.Records)
    Console.WriteLine($"{c.CustomerNo}: {c.Name} ({c.Status})");
```
