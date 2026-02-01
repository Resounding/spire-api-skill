# Sales Orders API

Base Path: `/api/v2/companies/{company-name}`

---

## Sales Root

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/sales` | Get sales root |

---

## Sales Orders

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/sales-orders` | Get list of sales orders |
| POST | `/sales-orders` | Create a sales order |
| GET | `/sales-orders/{id}` | Get sales order by ID |
| PUT | `/sales-orders/{id}` | Update sales order by ID |
| DELETE | `/sales-orders/{id}` | Delete sales order by ID |

---

## POST /sales-orders - Create Sales Order

### Request Body

The API handles defaults automatically - you only need to provide minimal required fields.

```json
{
  "customer": {
    "customerNo": "ACTTEC"
  },
  "customerPO": "PO-12345",
  "address": {
    "line1": "123 Main St",
    "line2": "",
    "city": "Vancouver",
    "provState": "BC",
    "postalCode": "V6B 1A1",
    "country": "Canada"
  },
  "shippingAddress": {
    "line1": "456 Shipping Ave",
    "city": "Vancouver",
    "provState": "BC",
    "postalCode": "V6B 2B2"
  },
  "orderDate": "2024-01-15",
  "requiredDate": "2024-01-22",
  "status": "O",
  "salesperson": {
    "code": "JD"
  },
  "territory": {
    "code": "WEST"
  },
  "items": [
    {
      "inventory": {
        "whse": "00",
        "partNo": "WIDGET-A"
      },
      "description": "Widget Type A",
      "orderQty": "5.0000",
      "committedQty": "5.0000",
      "unitPrice": "29.9900",
      "discountPercent": "0.00"
    },
    {
      "inventory": {
        "whse": "00",
        "partNo": "GADGET-B"
      },
      "description": "Gadget Type B",
      "orderQty": "10.0000",
      "committedQty": "10.0000",
      "unitPrice": "15.0000"
    }
  ],
  "payments": [
    {
      "method": 1,
      "amount": "50.00"
    }
  ]
}
```

### Sales Order Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `customer` | object | Yes | Customer reference |
| `customer.customerNo` | string | Yes* | Customer number (alternative to id) |
| `customer.id` | integer | Yes* | Customer ID (alternative to customerNo) |
| `customerPO` | string | No | Customer's purchase order number |
| `address` | object | No | Billing address |
| `shippingAddress` | object | No | Shipping address (if different from billing) |
| `orderDate` | string | No | Order date (YYYY-MM-DD), defaults to today |
| `requiredDate` | string | No | Required/ship date |
| `status` | string | No | Order status (O=Open, default) |
| `salesperson` | object | No | Salesperson reference |
| `salesperson.code` | string | No | Salesperson code |
| `territory` | object | No | Territory reference |
| `territory.code` | string | No | Territory code |
| `items` | array | Yes | Array of line items |
| `payments` | array | No | Array of payments/deposits |
| `freight` | decimal | No | Freight amount |
| `subtotal` | decimal | No | Order subtotal (calculated) |
| `total` | decimal | No | Order total (calculated) |

### Sales Order Item Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `inventory` | object | Yes | Inventory item reference |
| `inventory.whse` | string | Yes | Warehouse code |
| `inventory.partNo` | string | Yes | Part number |
| `inventory.id` | integer | No | Inventory item ID (alternative) |
| `description` | string | No | Line item description |
| `orderQty` | decimal | Yes | Quantity ordered |
| `committedQty` | decimal | No | Quantity committed/allocated |
| `backorderQty` | decimal | No | Quantity on backorder |
| `unitPrice` | decimal | No | Unit price (uses default if not specified) |
| `discountPercent` | decimal | No | Discount percentage |
| `comment` | string | No | Line item comment |

### Payment Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `method` | integer | Yes | Payment method ID |
| `amount` | decimal | Yes | Payment amount |

### Order Status Values

| Status | Description |
|--------|-------------|
| `O` | Open - New order, editable |
| `L` | Layaway |
| `P` | Processed/Picking |
| `S` | Shipped |
| `C` | Closed/Complete |

---

## PUT /sales-orders/{id} - Update Sales Order

Use the same structure as POST. Include only fields you want to update.

```json
{
  "requiredDate": "2024-01-25",
  "items": [
    {
      "id": 1001,
      "orderQty": "10.0000"
    }
  ]
}
```

---

## Sales Order Communications

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/sales-orders/{id}/communications` | Get list of sales order communications |

---

## Sales Order Email Messages

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/sales-orders/{id}/email-messages` | Get list of sales order email messages |
| POST | `/sales-orders/{id}/email-messages` | Create a sales order email message |

### POST Email Message

```json
{
  "to": "customer@example.com",
  "subject": "Order Confirmation",
  "body": "Your order has been received..."
}
```

---

## Sales Order Notes

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/sales-orders/{id}/notes` | Get list of sales order notes |
| POST | `/sales-orders/{id}/notes` | Create a sales order note |

### POST Note

```json
{
  "noteType": {"id": 1},
  "note": "Customer requested express shipping."
}
```

---

## Sales Order Invoice Conversion

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/sales-orders/{id}/invoice` | Convert sales order to invoice |

Convert an open sales order to an invoice. Send empty JSON body or specify options:

```json
{
  "invoiceDate": "2024-01-20"
}
```

---

## Sales Orders UDFs

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/sales-orders/udf` | Get list of sales order UDFs |
| GET | `/sales-orders/udf-fields` | Get list of sales order UDF fields |
| GET | `/sales-orders/udf-fields/{field-name}` | Get sales order UDF field by name |
| PUT | `/sales-orders/udf-fields/{field-name}` | Update sales order UDF field by name |
| DELETE | `/sales-orders/udf-fields/{field-name}` | Delete sales order UDF field by name |
| GET | `/sales-orders/udf-pages` | Get list of sales order UDF pages |
| POST | `/sales-orders/udf-pages` | Create a sales order UDF page |
| GET | `/sales-orders/udf-pages/{id}` | Get sales order UDF page by ID |
| PUT | `/sales-orders/udf-pages/{id}` | Update sales order UDF page by ID |
| DELETE | `/sales-orders/udf-pages/{id}` | Delete sales order UDF page by ID |

### Including UDFs in Sales Order

```json
{
  "customer": {"customerNo": "ABC123"},
  "items": [...],
  "udf": {
    "custom_field_1": "value",
    "custom_field_2": true
  }
}
```

---

## Example Queries

```bash
# Get all sales orders
GET /sales-orders

# Get sales order by ID
GET /sales-orders/12345

# Get with specific fields
GET /sales-orders/12345?fields=id,orderNo,customer.name,total

# Filter orders by status (Open)
GET /sales-orders?filter={"status":"O"}

# Filter by customer
GET /sales-orders?filter={"customer.customerNo":"ACTTEC"}

# Filter by date range
GET /sales-orders?filter={"orderDate":{"$gte":"2024-01-01","$lte":"2024-01-31"}}

# Get orders sorted by date descending
GET /sales-orders?sort=-orderDate

# Include UDF fields
GET /sales-orders?udf=1

# Paginate results
GET /sales-orders?start=0&limit=50

# Convert order to invoice
POST /sales-orders/12345/invoice
```
