# General Ledger API

Base Path: `/api/v2/companies/{company-name}`

---

## General Ledger Root

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/gl` | Get general ledger root |

---

## GL Accounts

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/gl-accounts` | Get list of GL accounts |

### GL Accounts UDFs

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/gl-accounts-udf` | Get list of GL account UDFs |
| GET | `/gl-accounts-udf-fields` | Get list of UDF fields |
| GET | `/gl-accounts-udf-fields/{field-name}` | Get UDF field by name |
| PUT | `/gl-accounts-udf-fields/{field-name}` | Update UDF field by name |
| DELETE | `/gl-accounts-udf-fields/{field-name}` | Delete UDF field by name |
| GET | `/gl-accounts-udf-pages` | Get list of UDF pages |
| POST | `/gl-accounts-udf-pages` | Create UDF page |

---

## GL Divisions

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/gl-divisions` | Get list of GL divisions |

---

## GL Fiscal Periods

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/gl-periods` | Get list of fiscal periods |

---

## GL Groups & Subgroups

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/gl-groups` | Get list of GL groups |
| GET | `/gl-subgroups` | Get list of GL subgroups |

---

## GL Segments

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/gl-segments` | Get list of GL segments |

---

## GL Special Accounts

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/gl-special-accounts` | Get list of special GL accounts |

---

## GL History

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/gl-history` | Get GL history root |
| GET | `/gl-history-transactions` | Get list of historical GL transactions |

---

## GL Transactions

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/gl-transactions` | Get list of GL transactions |
| POST | `/gl-transactions` | Create a GL transaction |
| GET | `/gl-transactions/{id}` | Get GL transaction by ID |

---

## POST /gl-transactions - Create GL Transaction (Journal Entry)

### Request Body

```json
{
  "date": "2024-01-15",
  "reference": "JE-001",
  "memo": "Monthly adjustment",
  "entries": [
    {
      "account": {
        "accountNo": "1000"
      },
      "debit": "1000.00",
      "credit": "0.00",
      "memo": "Cash adjustment"
    },
    {
      "account": {
        "accountNo": "3000"
      },
      "debit": "0.00",
      "credit": "1000.00",
      "memo": "Revenue adjustment"
    }
  ]
}
```

### GL Transaction Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `date` | string | No | Transaction date |
| `reference` | string | No | Reference number |
| `memo` | string | No | Transaction memo |
| `entries` | array | Yes | Array of journal entry lines |

### GL Entry Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `account` | object | Yes | GL account reference |
| `account.accountNo` | string | Yes | Account number |
| `account.id` | integer | No | Account ID (alternative) |
| `debit` | decimal | No | Debit amount |
| `credit` | decimal | No | Credit amount |
| `memo` | string | No | Line memo |
| `entity` | string | No | Customer/vendor code |
| `document` | string | No | Document reference |

**Note:** Total debits must equal total credits for the transaction to be valid.

---

## GL Recurring Transactions

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/gl-recurring` | Get list of recurring GL transactions |

---

## GL Reconciliations

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/gl-reconciliations` | Get list of GL reconciliations |

---

## GL Year End

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/gl-year-end` | Execute year-end closing procedures |

---

## Example Queries

```bash
# Get GL root
GET /gl

# Get all GL accounts
GET /gl-accounts

# Get GL transactions
GET /gl-transactions

# Create a GL transaction (journal entry)
POST /gl-transactions
Content-Type: application/json
{
  "date": "2024-01-15",
  "entries": [
    {"account": {"id": 1000}, "debit": 100.00},
    {"account": {"id": 2000}, "credit": 100.00}
  ]
}

# Get fiscal periods
GET /gl-periods

# Get GL history transactions
GET /gl-history-transactions

# Filter accounts by type
GET /gl-accounts?filter={"type":"A"}

# Perform year-end closing
POST /gl-year-end
```
