# Purchasing API

Base Path: `/api/v2/companies/{company-name}`

---

## Purchasing Root

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/purchasing` | Get purchasing root |

---

## Purchase Orders

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/purchasing-orders` | Get list of purchase orders |
| POST | `/purchasing-orders` | Create a purchase order |
| GET | `/purchasing-orders/{id}` | Get purchase order by ID |
| PUT | `/purchasing-orders/{id}` | Update purchase order by ID |
| DELETE | `/purchasing-orders/{id}` | Delete purchase order by ID |

---

## POST /purchasing-orders - Create Purchase Order

### Request Body

```json
{
  "vendor": {
    "vendorNo": "ACMSYS"
  },
  "status": "O",
  "date": "2024-01-15",
  "requiredDate": "2024-01-22",
  "whse": "00",
  "termsCode": "NET30",
  "trackingNo": "TRK-12345",
  "referenceNo": "REF-001",
  "location": {
    "id": 1
  },
  "items": [
    {
      "inventory": {
        "whse": "00",
        "partNo": "WIDGET-A"
      },
      "orderQty": "100",
      "purchaseMeasure": "EA",
      "unitPrice": "12.50"
    },
    {
      "inventory": {
        "whse": "00",
        "partNo": "GADGET-B"
      },
      "orderQty": "50",
      "purchaseMeasure": "EA",
      "unitPrice": "25.00"
    }
  ]
}
```

### Purchase Order Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `vendor` | object | Yes | Vendor reference |
| `vendor.vendorNo` | string | Yes* | Vendor number (alternative to id) |
| `vendor.id` | integer | Yes* | Vendor ID (alternative to vendorNo) |
| `status` | string | No | Order status (O=Open, default) |
| `date` | string | No | Order date (YYYY-MM-DD), defaults to today |
| `requiredDate` | string | No | Required delivery date |
| `whse` | string | No | Default warehouse code |
| `termsCode` | string | No | Payment terms code (e.g., NET30, NET60) |
| `trackingNo` | string | No | Tracking number |
| `referenceNo` | string | No | Reference number |
| `location` | object | No | Location reference |
| `location.id` | integer | No | Location ID |
| `items` | array | Yes | Array of line items |
| `subtotal` | decimal | No | Order subtotal (calculated) |
| `total` | decimal | No | Order total (calculated) |

### Purchase Order Item Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `inventory` | object | Yes | Inventory item reference |
| `inventory.whse` | string | Yes | Warehouse code |
| `inventory.partNo` | string | Yes | Part number |
| `inventory.id` | integer | No | Inventory item ID (alternative) |
| `orderQty` | decimal | Yes | Quantity ordered |
| `receiveQty` | decimal | No | Quantity received (for receiving) |
| `purchaseMeasure` | string | No | Unit of measure (EA, CS, etc.) |
| `unitPrice` | decimal | No | Unit cost |
| `comment` | string | No | Line item comment |

### Purchase Order Status Values

| Status | Description |
|--------|-------------|
| `O` | Open - New PO, editable, not sent to vendor |
| `I` | Issued - Sent to vendor, limited editing |
| `R` | Received - Some or all items received |
| `C` | Closed/Complete |

---

## PUT /purchasing-orders/{id} - Update Purchase Order

Use the same structure as POST. Include only fields you want to update.

```json
{
  "requiredDate": "2024-01-25",
  "items": [
    {
      "id": 1001,
      "orderQty": "150"
    }
  ]
}
```

---

## Purchase Order Actions

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/purchasing-orders/{id}/issue` | Issue a purchase order |
| POST | `/purchasing-orders/{id}/receive` | Receive a purchase order |

### POST /purchasing-orders/{id}/issue - Issue PO

Issues the purchase order to the vendor. Changes status from Open to Issued.

Send empty JSON body:
```json
{}
```

### POST /purchasing-orders/{id}/receive - Receive PO

Receive items on a purchase order. Updates inventory quantities.

```json
{
  "items": [
    {
      "id": 1001,
      "receiveQty": "100"
    },
    {
      "id": 1002,
      "receiveQty": "25"
    }
  ]
}
```

---

## Purchase Order Communications

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/purchasing-orders/{id}/communications` | Get list of order communications |

---

## Purchase Order Contacts

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/purchasing-orders/{id}/contacts` | Get list of order contacts |

---

## Purchase Order Email Messages

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/purchasing-orders/{id}/email-messages` | Get list of order email messages |
| POST | `/purchasing-orders/{id}/email-messages` | Create order email message |

### POST Email Message

```json
{
  "to": "vendor@example.com",
  "subject": "Purchase Order #12345",
  "body": "Please find attached our purchase order..."
}
```

---

## Purchase Order Notes

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/purchasing-orders/{id}/notes` | Get list of order notes |
| POST | `/purchasing-orders/{id}/notes` | Create order note |

### POST Note

```json
{
  "noteType": {"id": 1},
  "note": "Vendor confirmed delivery date."
}
```

---

## Purchase Orders UDFs

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/purchasing-orders/udf` | Get list of order UDFs |
| GET | `/purchasing-orders/udf-fields` | Get list of UDF fields |
| GET | `/purchasing-orders/udf-fields/{field-name}` | Get UDF field by name |
| PUT | `/purchasing-orders/udf-fields/{field-name}` | Update UDF field by name |
| DELETE | `/purchasing-orders/udf-fields/{field-name}` | Delete UDF field by name |
| GET | `/purchasing-orders/udf-pages` | Get list of UDF pages |
| POST | `/purchasing-orders/udf-pages` | Create UDF page |
| GET | `/purchasing-orders/udf-pages/{id}` | Get UDF page by ID |
| PUT | `/purchasing-orders/udf-pages/{id}` | Update UDF page by ID |
| DELETE | `/purchasing-orders/udf-pages/{id}` | Delete UDF page by ID |

### Including UDFs in Purchase Order

```json
{
  "vendor": {"vendorNo": "ACMSYS"},
  "items": [...],
  "udf": {
    "project_code": "PRJ-001",
    "approved_by": "John Smith"
  }
}
```

---

## Purchasing History

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/purchasing-history` | Get list of purchasing history |
| GET | `/purchasing-history/{id}` | Get purchasing history record by ID |

---

## Purchasing History Communications

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/purchasing-history/{id}/communications` | Get list of history communications |

---

## Purchasing History Contacts

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/purchasing-history/{id}/contacts` | Get list of history contacts |

---

## Purchasing History Email Messages

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/purchasing-history/{id}/email-messages` | Get list of history email messages |
| POST | `/purchasing-history/{id}/email-messages` | Create history email message |

---

## Purchasing History Notes

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/purchasing-history/{id}/notes` | Get list of history notes |
| POST | `/purchasing-history/{id}/notes` | Create history note |

---

## Purchasing History Items

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/purchasing-history-items` | Get list of all purchasing history items |

---

## Purchasing Items

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/purchasing-items` | Get list of purchasing items |

### Purchasing Items UDFs

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/purchasing-items/udf` | Get list of item UDFs |
| GET | `/purchasing-items/udf-fields` | Get list of UDF fields |
| GET | `/purchasing-items/udf-fields/{field-name}` | Get UDF field by name |
| PUT | `/purchasing-items/udf-fields/{field-name}` | Update UDF field by name |
| DELETE | `/purchasing-items/udf-fields/{field-name}` | Delete UDF field by name |
| GET | `/purchasing-items/udf-pages` | Get list of UDF pages |
| POST | `/purchasing-items/udf-pages` | Create UDF page |
| GET | `/purchasing-items/udf-pages/{id}` | Get UDF page by ID |
| PUT | `/purchasing-items/udf-pages/{id}` | Update UDF page by ID |
| DELETE | `/purchasing-items/udf-pages/{id}` | Delete UDF page by ID |

---

## Example Queries

```bash
# Get all purchase orders
GET /purchasing-orders

# Get purchase order by ID
GET /purchasing-orders/12345

# Get with specific fields
GET /purchasing-orders/12345?fields=id,poNo,vendor.name,total

# Create a new purchase order
POST /purchasing-orders
Content-Type: application/json
{
  "vendor": {"vendorNo": "ACMSYS"},
  "whse": "00",
  "items": [
    {"inventory": {"partNo": "WIDGET-A"}, "orderQty": "100", "unitPrice": "12.50"}
  ]
}

# Issue a purchase order
POST /purchasing-orders/12345/issue
Content-Type: application/json
{}

# Receive a purchase order
POST /purchasing-orders/12345/receive
Content-Type: application/json
{
  "items": [{"id": 1001, "receiveQty": "100"}]
}

# Filter orders by status (Open)
GET /purchasing-orders?filter={"status":"O"}

# Filter by vendor
GET /purchasing-orders?filter={"vendor.vendorNo":"ACMSYS"}

# Filter by date range
GET /purchasing-orders?filter={"date":{"$gte":"2024-01-01","$lte":"2024-01-31"}}

# Get orders sorted by date descending
GET /purchasing-orders?sort=-date

# Include UDF fields
GET /purchasing-orders?udf=1

# Get purchasing history
GET /purchasing-history

# Get purchase order items
GET /purchasing-items
```
