# Core Resources API

Base Path: `/api/v2/companies/{company-name}`

---

## Companies

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/v2/companies` | Get list of companies |
| GET | `/api/v2/companies/{company-name}` | Get company by name |
| PUT | `/api/v2/companies/{company-name}` | Create or update company |
| DELETE | `/api/v2/companies/{company-name}` | Delete company |

---

## Users

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/users` | Get list of users |
| GET | `/users/{uuid}` | Get user by UUID |
| PUT | `/users/{uuid}` | Update user by UUID |
| GET | `/users/username/{username}` | Get user by username |

### Users UDFs

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/users/udf` | Get list of user UDFs |
| GET | `/users/udf/fields` | Get list of user UDF fields |

---

## Addresses

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/addresses` | Get list of addresses |
| GET | `/addresses/{id}` | Get address by ID |
| PUT | `/addresses/{id}` | Update address by ID |
| DELETE | `/addresses/{id}` | Delete address by ID |

---

## Contacts

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/contacts` | Get list of contacts |
| GET | `/contacts/{id}` | Get contact by ID |
| PUT | `/contacts/{id}` | Update contact by ID |
| DELETE | `/contacts/{id}` | Delete contact by ID |

---

## Contact Types

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/contact-types` | Get list of contact types |
| POST | `/contact-types` | Create a contact type |
| GET | `/contact-types/{id}` | Get contact type by ID |
| PUT | `/contact-types/{id}` | Update contact type by ID |
| DELETE | `/contact-types/{id}` | Delete contact type by ID |

---

## Currencies

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/currencies` | Get list of currencies |
| POST | `/currencies` | Create a currency |
| GET | `/currencies/{id}` | Get currency by ID |
| PUT | `/currencies/{id}` | Update currency by ID |
| DELETE | `/currencies/{id}` | Delete currency by ID |

---

## Payment Methods

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/payment-methods` | Get list of payment methods |
| POST | `/payment-methods` | Create a payment method |
| GET | `/payment-methods/{id}` | Get payment method by ID |
| PUT | `/payment-methods/{id}` | Update payment method by ID |
| DELETE | `/payment-methods/{id}` | Delete payment method by ID |

---

## Payment Terms

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/payment-terms` | Get list of payment terms |
| POST | `/payment-terms` | Create a payment term |
| GET | `/payment-terms/{id}` | Get payment term by ID |
| PUT | `/payment-terms/{id}` | Update payment term by ID |
| DELETE | `/payment-terms/{id}` | Delete payment term by ID |

---

## Shipping Methods

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/shipping-methods` | Get list of shipping methods |

### Shipping Methods UDFs

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/shipping-methods-udf` | Get list of shipping methods UDFs |
| GET | `/shipping-methods-udf-fields` | Get list of UDF fields |
| GET | `/shipping-methods-udf-fields/{field-name}` | Get UDF field by name |
| PUT | `/shipping-methods-udf-fields/{field-name}` | Update UDF field by name |
| DELETE | `/shipping-methods-udf-fields/{field-name}` | Delete UDF field by name |
| GET | `/shipping-methods-udf-pages` | Get list of UDF pages |
| POST | `/shipping-methods-udf-pages` | Create UDF page |
| GET | `/shipping-methods-udf-pages/{id}` | Get UDF page by ID |
| PUT | `/shipping-methods-udf-pages/{id}` | Update UDF page by ID |
| DELETE | `/shipping-methods-udf-pages/{id}` | Delete UDF page by ID |

---

## Filters

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/filters` | Get list of filters |
| GET | `/filters-window` | Get list of filters (windowed) |

---

## Reports

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/reports` | Get list of reports |
| GET | `/reports/{section}` | Get reports by section |

---

## Integrations

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/integrations` | Get list of integrations |

---

## User Defined Types

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/user-defined-types` | Get list of user defined types |

---

## Phases

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/phase` | Get phase root |
| GET | `/phases` | Get list of phases |
| GET | `/phase-history` | Get list of phase history |

---

## Equipment (Service)

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/service-equipment` | Get list of service equipment |
| POST | `/service-equipment` | Create service equipment |
| GET | `/service-equipment/{id}` | Get service equipment by ID |
| PUT | `/service-equipment/{id}` | Update service equipment by ID |
| DELETE | `/service-equipment/{id}` | Delete service equipment by ID |
| GET | `/service-sales-history-equipment` | Get list of sales history equipment |

---

## Example Queries

```bash
# Get companies
GET /api/v2/companies

# Get users
GET /users

# Get user by username
GET /users/username/admin

# Get currencies
GET /currencies

# Get payment methods
GET /payment-methods

# Get shipping methods
GET /shipping-methods

# Get reports
GET /reports

# Get reports by section
GET /reports/sales
```
