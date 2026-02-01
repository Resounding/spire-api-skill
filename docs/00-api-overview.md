# Spire API Overview

## Base URL
```
https://{server}:{port}/api/v2/companies/{company-name}
```

## Authentication

**Method:** HTTP Basic Authentication

**Header Format:**
```
Authorization: Basic base64(username:password)
```

**cURL Example:**
```bash
curl -u "username:password" https://your-server:10880/api/v2/companies/yourcompany/customers
```

---

## Common Query Parameters

### Pagination

| Parameter | Type | Default | Max | Description |
|-----------|------|---------|-----|-------------|
| `start` | integer | 0 | - | Record offset for pagination |
| `limit` | integer | 10 | 10,000 | Number of records to return |

**Example:**
```
GET /customers?start=0&limit=100
GET /customers?start=100&limit=100  # Page 2
```

**Response Structure:**
```json
{
  "records": [...],
  "start": 0,
  "limit": 100,
  "count": 1500
}
```

### Sorting

| Parameter | Syntax | Description |
|-----------|--------|-------------|
| `sort` | `fieldname` | Ascending order |
| `sort` | `-fieldname` | Descending order (prefix with `-`) |

**Examples:**
```
GET /inventory/items?sort=partNo           # Ascending
GET /inventory/items?sort=-partNo          # Descending
GET /inventory/items?sort=-availableQty&sort=partNo  # Multi-level
```

### Field Selection

| Parameter | Syntax | Description |
|-----------|--------|-------------|
| `fields` | `field1,field2,field3` | Comma-separated list of fields to return |

**Example:**
```
GET /inventory/items?fields=partNo,description,udf.Kosher
```

Nested fields use dot notation: `udf.FieldName`

### Filtering

| Parameter | Format | Description |
|-----------|--------|-------------|
| `filter` | URL-encoded JSON | Filter conditions |

**Basic Filter:**
```json
{"customerNo": "12"}
```
URL: `?filter=%7B%22customerNo%22%3A%2212%22%7D`

**Multiple Conditions (AND):**
```json
{"column1": 12, "column2": "test"}
```

### Filter Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `$eq` | Equals | `{"field": {"$eq": "value"}}` |
| `$ieq` | Equals (case-insensitive) | `{"field": {"$ieq": "VALUE"}}` |
| `$ne` | Not equals | `{"field": {"$ne": 12}}` |
| `$not` | Negation | `{"field": {"$not": 12}}` |
| `$gt` | Greater than | `{"field": {"$gt": 100}}` |
| `$gte` | Greater than or equal | `{"field": {"$gte": 100}}` |
| `$lt` | Less than | `{"field": {"$lt": 100}}` |
| `$lte` | Less than or equal | `{"field": {"$lte": 100}}` |
| `$in` | In array | `{"field": {"$in": ["a", "b"]}}` |
| `$nin` | Not in array | `{"field": {"$nin": ["a", "b"]}}` |
| `$like` | SQL LIKE pattern | `{"field": {"$like": "ABC%"}}` |
| `$ilike` | LIKE (case-insensitive) | `{"field": {"$ilike": "abc%"}}` |
| `$contains` | Contains substring | `{"field": {"$contains": "test"}}` |
| `$icontains` | Contains (case-insensitive) | `{"field": {"$icontains": "TEST"}}` |
| `$startswith` | Starts with | `{"field": {"$startswith": "ABC"}}` |
| `$istartswith` | Starts with (case-insensitive) | `{"field": {"$istartswith": "abc"}}` |

**Wildcards for $like/$ilike:**
- `%` matches any number of characters
- `?` matches a single character

### Logical Operators

| Operator | Description | Example |
|----------|-------------|---------|
| `$or` | Logical OR | `{"$or": [{"field": 1}, {"field": 2}]}` |
| `$and` | Logical AND | `{"$and": [{"field": {"$gt": 10}}, {"field": {"$lt": 100}}]}` |

---

## User Defined Fields (UDFs)

### Retrieving UDFs

**All UDFs:**
```
GET /customers?udf=1
```

**Specific UDF Fields:**
```
GET /customers?fields=name,udf.CustomField
```

### UDF Management Endpoints

Most resources support UDF configuration via these endpoint patterns:
- `GET /{resource}-udf` - List UDF data
- `GET /{resource}-udf-fields` - List UDF field definitions
- `GET/PUT/DELETE /{resource}-udf-fields/{field-name}` - Manage specific field
- `GET/POST /{resource}-udf-pages` - List/create UDF pages
- `GET/PUT/DELETE /{resource}-udf-pages/{id}` - Manage specific page

### Creating UDF Fields

1. **Create UDF Page:**
```
POST /udf/pages
{"label": "Custom Page"}
```

2. **Define UDF Field:**
```
PUT /udf/fields/{field_name}
{"label": "Field Label", "pageId": 1, "sequence": 0}
```

3. **Set UDF Value:**
```
PUT /inventory/items/{id}
{"udf": {"field_name": "value"}}
```

---

## HTTP Methods

| Method | Description |
|--------|-------------|
| GET | Retrieve resource(s) |
| POST | Create new resource |
| PUT | Update existing resource |
| DELETE | Remove resource |

---

## Response Codes

| Code | Description |
|------|-------------|
| 200 | Success |
| 201 | Created |
| 400 | Bad Request |
| 401 | Unauthorized |
| 404 | Not Found |
| 500 | Server Error |

---

## API Resources

| Category | Resources |
|----------|-----------|
| **Core** | Companies, Users, Addresses, Contacts, Contact Types |
| **Customers** | Customers, Addresses, Contacts, Payments, Payment Methods, Part Numbers, Notes |
| **Sales** | Sales Orders, Sales Invoices, Sales Items, Sales Batches, Sales Taxes |
| **Inventory** | Items, Adjustments, Counts, Transfers, Warehouses, Groups, Price Levels, UPCs, Serial Numbers |
| **Purchasing** | Purchase Orders, Purchase History, Purchase Items |
| **Accounting** | Accounts Payable, Accounts Receivable, General Ledger |
| **Production** | Production Orders, Templates, Categories, History |
| **Payroll** | Employees, Timecards, Departments, Schedules |
| **Other** | Job Costing, CRM, Email, Currencies, Payment Methods/Terms, Shipping Methods |
