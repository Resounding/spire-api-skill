# Accounts Payable API

Base Path: `/api/v2/companies/{company-name}`

---

## Accounts Payable Root

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/ap` | Get accounts payable root |

---

## Accounts Payable Aged

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/ap/aged` | Get list of accounts payable aged |

### AP Aged UDFs

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/ap/aged/udf` | Get list of AP aged UDFs |
| GET | `/ap/aged/udf/fields` | Get list of UDF fields |
| GET | `/ap/aged/udf/fields/{field-name}` | Get UDF field by name |
| PUT | `/ap/aged/udf/fields/{field-name}` | Update UDF field by name |
| DELETE | `/ap/aged/udf/fields/{field-name}` | Delete UDF field by name |
| GET | `/ap/aged/udf/pages` | Get list of UDF pages |
| POST | `/ap/aged/udf/pages` | Create UDF page |
| GET | `/ap/aged/udf/pages/{id}` | Get UDF page by ID |
| PUT | `/ap/aged/udf/pages/{id}` | Update UDF page by ID |
| DELETE | `/ap/aged/udf/pages/{id}` | Delete UDF page by ID |

---

## Accounts Payable Batches

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/ap/batches` | Get list of accounts payable batches |

---

## Accounts Payable Transactions

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/ap/transactions` | Get list of accounts payable transactions |
| POST | `/ap/transactions` | Create an accounts payable transaction |
| GET | `/ap/transactions/{id}` | Get accounts payable transaction by ID |

---

## POST /ap/transactions - Create AP Transaction

### Request Body (Invoice)

```json
{
  "type": "I",
  "vendor": {
    "vendorNo": "VENDOR001"
  },
  "date": "2024-01-15",
  "dueDate": "2024-02-14",
  "invoiceNo": "INV-12345",
  "amount": "1500.00",
  "description": "Office supplies",
  "glEntries": [
    {
      "account": {"accountNo": "5000"},
      "amount": "1500.00"
    }
  ]
}
```

### Request Body (Payment)

```json
{
  "type": "P",
  "vendor": {
    "vendorNo": "VENDOR001"
  },
  "date": "2024-02-14",
  "amount": "1500.00",
  "reference": "CHQ-5678",
  "paymentMethod": {
    "id": 1
  }
}
```

### AP Transaction Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `type` | string | Yes | Transaction type (I=Invoice, P=Payment, C=Credit) |
| `vendor` | object | Yes | Vendor reference |
| `date` | string | No | Transaction date |
| `dueDate` | string | No | Due date (for invoices) |
| `invoiceNo` | string | No | Vendor invoice number |
| `amount` | decimal | Yes | Transaction amount |
| `description` | string | No | Description |
| `reference` | string | No | Reference (check number for payments) |
| `paymentMethod` | object | No | Payment method (for payments) |
| `glEntries` | array | No | GL distribution entries |

### AP Transaction Types

| Type | Description |
|------|-------------|
| `I` | Invoice |
| `P` | Payment |
| `C` | Credit Memo |
| `D` | Debit Memo |

---

## Apply AP Transaction

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/ap/transactions/{transaction-id}/apply/{apply-transaction-id}` | Apply an AP transaction |

---

## Vendors

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/vendors` | Get list of vendors |

---

## Example Queries

```bash
# Get AP root
GET /ap

# Get aged payables
GET /ap/aged

# Get AP transactions
GET /ap/transactions

# Create AP transaction
POST /ap/transactions
Content-Type: application/json
{
  "vendor": {"id": 100},
  "amount": 1000.00,
  ...
}

# Apply a payment to an invoice
POST /ap/transactions/12345/apply/67890

# Get vendors list
GET /vendors

# Filter aged payables by vendor
GET /ap/aged?filter={"vendor.id":100}
```
