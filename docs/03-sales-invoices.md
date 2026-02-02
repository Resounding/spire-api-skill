# Sales Invoices API

Base Path: `/api/v2/companies/{company-name}`

---

## Sales Invoices

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/sales/invoices` | Get list of sales invoices |
| GET | `/sales/invoices/{id}` | Get sales invoice by ID |
| PUT | `/sales/invoices/{id}` | Update sales invoice by ID |

---

## Sales Invoice Communications

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/sales/invoices/{id}/communications` | Get list of sales invoice communications |

---

## Sales Invoice Contacts

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/sales/invoices/{id}/contacts` | Get list of sales invoice contacts |

---

## Sales Invoice Email Messages

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/sales/invoices/{id}/email-messages` | Get list of sales invoice email messages |
| POST | `/sales/invoices/{id}/email-messages` | Create a sales invoice email message |

---

## Sales Invoice Notes

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/sales/invoices/{id}/notes` | Get list of sales invoice notes |
| POST | `/sales/invoices/{id}/notes` | Create a sales invoice note |

---

## Sales Invoice Items

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/sales/invoice-items` | Get list of all sales invoice items |

---

## Example Queries

```bash
# Get all invoices
GET /sales/invoices

# Get invoice by ID
GET /sales/invoices/12345

# Get invoice items
GET /sales/invoice-items

# Filter invoices by customer
GET /sales/invoices?filter={"customer.id":100}

# Get invoices sorted by date
GET /sales/invoices?sort=-invoiceDate
```
