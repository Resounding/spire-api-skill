# Production API

Base Path: `/api/v2/companies/{company-name}`

---

## Production Root

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/production` | Get production root |

---

## Production Categories

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/production/categories` | Get list of production categories |

---

## Production Orders

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/production/orders` | Get list of production orders |
| POST | `/production/orders` | Create a production order |
| GET | `/production/orders/{id}` | Get production order by ID |
| PUT | `/production/orders/{id}` | Update production order by ID |
| DELETE | `/production/orders/{id}` | Delete production order by ID |

---

## POST /production/orders - Create Production Order

### Request Body

```json
{
  "template": {
    "id": 1
  },
  "finishedGood": {
    "partNo": "ASSEMBLED-WIDGET"
  },
  "qty": "100",
  "whse": "00",
  "date": "2024-01-15",
  "requiredDate": "2024-01-22",
  "status": "O",
  "reference": "WO-12345",
  "items": [
    {
      "inventory": {
        "partNo": "COMPONENT-A"
      },
      "qty": "200",
      "unitCost": "5.00"
    },
    {
      "inventory": {
        "partNo": "COMPONENT-B"
      },
      "qty": "100",
      "unitCost": "10.00"
    }
  ],
  "udf": {
    "priority": "High",
    "machine_id": "M-001"
  }
}
```

### Production Order Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `template` | object | No | Production template reference |
| `template.id` | integer | No | Template ID |
| `finishedGood` | object | Yes | Finished product reference |
| `finishedGood.partNo` | string | Yes | Part number of finished good |
| `qty` | decimal | Yes | Quantity to produce |
| `whse` | string | No | Warehouse code |
| `date` | string | No | Order date |
| `requiredDate` | string | No | Required completion date |
| `status` | string | No | Order status |
| `reference` | string | No | Reference number |
| `items` | array | No | Component items (auto-populated from template) |
| `udf` | object | No | User defined fields |

### Production Order Item Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `inventory` | object | Yes | Component item reference |
| `inventory.partNo` | string | Yes | Part number |
| `qty` | decimal | Yes | Quantity required |
| `unitCost` | decimal | No | Unit cost |

### Production Order Status Values

| Status | Description |
|--------|-------------|
| `O` | Open |
| `I` | In Progress |
| `C` | Complete |

---

## PUT /production/orders/{id} - Update Production Order

```json
{
  "qty": "150",
  "requiredDate": "2024-01-25",
  "status": "I"
}
```

---

## Production Order Notes

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/production/orders/{id}/notes` | Get list of production order notes |

---

## Production Orders UDFs

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/production/orders/udf` | Get list of production order UDFs |
| GET | `/production/orders/udf-fields` | Get list of UDF fields |
| GET | `/production/orders/udf-fields/{field-name}` | Get UDF field by name |
| PUT | `/production/orders/udf-fields/{field-name}` | Update UDF field by name |
| DELETE | `/production/orders/udf-fields/{field-name}` | Delete UDF field by name |
| GET | `/production/orders/udf-pages` | Get list of UDF pages |
| POST | `/production/orders/udf-pages` | Create UDF page |
| GET | `/production/orders/udf-pages/{id}` | Get UDF page by ID |
| PUT | `/production/orders/udf-pages/{id}` | Update UDF page by ID |
| DELETE | `/production/orders/udf-pages/{id}` | Delete UDF page by ID |

---

## Production Templates

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/production/templates` | Get list of production templates |
| POST | `/production/templates` | Create a production template |
| GET | `/production/templates/{id}` | Get production template by ID |
| PUT | `/production/templates/{id}` | Update production template by ID |
| DELETE | `/production/templates/{id}` | Delete production template by ID |

---

## POST /production/templates - Create Production Template

### Request Body

```json
{
  "name": "Widget Assembly Template",
  "finishedGood": {
    "partNo": "ASSEMBLED-WIDGET"
  },
  "category": {
    "id": 1
  },
  "description": "Standard widget assembly process",
  "defaultQty": "100",
  "items": [
    {
      "inventory": {
        "partNo": "COMPONENT-A"
      },
      "qtyPer": "2"
    },
    {
      "inventory": {
        "partNo": "COMPONENT-B"
      },
      "qtyPer": "1"
    }
  ]
}
```

### Production Template Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | Template name |
| `finishedGood` | object | Yes | Finished product reference |
| `category` | object | No | Category reference |
| `description` | string | No | Template description |
| `defaultQty` | decimal | No | Default production quantity |
| `items` | array | No | Component items |
| `items[].inventory` | object | Yes | Component item reference |
| `items[].qtyPer` | decimal | Yes | Quantity per finished unit |

---

## Production Template Notes

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/production/templates/{id}/notes` | Get list of production template notes |

---

## Production Template Items

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/production/template-items` | Get list of production template items |

---

## Production Items

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/production-items` | Get list of production items |

---

## Production History

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/production/history` | Get list of production history |

---

## Production History Items

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/production/history-items` | Get list of production history items |

---

## Example Queries

```bash
# Get production root
GET /production

# Get all production orders
GET /production/orders

# Get production order by ID
GET /production/orders/123

# Filter orders by status (Open)
GET /production/orders?filter={"status":"O"}

# Filter by date range
GET /production/orders?filter={"date":{"$gte":"2024-01-01","$lte":"2024-01-31"}}

# Sort by required date
GET /production/orders?sort=requiredDate

# Include UDF fields
GET /production/orders?udf=1

# Create a production order from template
POST /production/orders
Content-Type: application/json
{
  "template": {"id": 1},
  "qty": "100",
  "requiredDate": "2024-01-22"
}

# Create a production order with custom items
POST /production/orders
Content-Type: application/json
{
  "finishedGood": {"partNo": "ASSEMBLED-WIDGET"},
  "qty": "50",
  "items": [
    {"inventory": {"partNo": "COMP-A"}, "qty": "100"},
    {"inventory": {"partNo": "COMP-B"}, "qty": "50"}
  ]
}

# Update production order status
PUT /production/orders/123
Content-Type: application/json
{
  "status": "I"
}

# Get production templates
GET /production/templates

# Get production history
GET /production/history

# Get production template items
GET /production/template-items
```
