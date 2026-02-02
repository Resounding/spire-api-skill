# Accounts Receivable API

Base Path: `/api/v2/companies/{company-name}`

---

## Accounts Receivable Root

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/ar` | Get accounts receivable root |

---

## Accounts Receivable Aged

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/ar-aged` | Get list of accounts receivable aged |

### AR Aged UDFs

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/ar/aged-udf` | Get list of AR aged UDFs |
| GET | `/ar/aged-udf-fields` | Get list of UDF fields |
| GET | `/ar/aged-udf-fields/{field-name}` | Get UDF field by name |
| PUT | `/ar/aged-udf-fields/{field-name}` | Update UDF field by name |
| DELETE | `/ar/aged-udf-fields/{field-name}` | Delete UDF field by name |
| GET | `/ar/aged-udf-pages` | Get list of UDF pages |
| POST | `/ar/aged-udf-pages` | Create UDF page |
| GET | `/ar/aged-udf-pages/{id}` | Get UDF page by ID |
| PUT | `/ar/aged-udf-pages/{id}` | Update UDF page by ID |
| DELETE | `/ar/aged-udf-pages/{id}` | Delete UDF page by ID |

---

## Accounts Receivable Transactions

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/ar-transactions` | Get list of AR transactions |
| POST | `/ar-transactions` | Create an AR transaction |
| GET | `/ar-transactions/{id}` | Get AR transaction by ID |

---

## POST /ar-transactions - Create AR Transaction

### Request Body (Payment)

```json
{
  "type": "P",
  "customer": {
    "customerNo": "CUST001"
  },
  "date": "2024-01-15",
  "amount": "1500.00",
  "reference": "CHQ-12345",
  "paymentMethod": {
    "id": 1
  }
}
```

### Request Body (Credit Memo)

```json
{
  "type": "C",
  "customer": {
    "customerNo": "CUST001"
  },
  "date": "2024-01-15",
  "amount": "100.00",
  "description": "Return credit"
}
```

### AR Transaction Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `type` | string | Yes | Transaction type (P=Payment, C=Credit, D=Debit) |
| `customer` | object | Yes | Customer reference |
| `date` | string | No | Transaction date |
| `amount` | decimal | Yes | Transaction amount |
| `reference` | string | No | Reference (check number for payments) |
| `description` | string | No | Description |
| `paymentMethod` | object | No | Payment method (for payments) |

### AR Transaction Types

| Type | Description |
|------|-------------|
| `P` | Payment |
| `C` | Credit Memo |
| `D` | Debit Memo |

---

## Apply AR Transaction

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/ar-transactions/{transaction-id}/apply/{apply-transaction-id}` | Apply an AR transaction |

---

## Example Queries

```bash
# Get AR root
GET /ar

# Get aged receivables
GET /ar-aged

# Get AR transactions
GET /ar-transactions

# Create AR transaction (payment)
POST /ar-transactions
Content-Type: application/json
{
  "customer": {"id": 100},
  "amount": 500.00,
  ...
}

# Apply a payment to an invoice
POST /ar-transactions/12345/apply/67890

# Filter aged receivables by customer
GET /ar-aged?filter={"customer.id":100}

# Get aged receivables sorted by amount
GET /ar-aged?sort=-amount
```
