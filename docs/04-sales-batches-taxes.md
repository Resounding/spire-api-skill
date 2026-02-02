# Sales Batches & Taxes API

Base Path: `/api/v2/companies/{company-name}`

---

## Sales Batches

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/sales/batches` | Get list of sales batches |
| POST | `/sales/batches` | Create a sales batch |
| GET | `/sales/batches/{id}` | Get sales batch by ID |
| PUT | `/sales/batches/{id}` | Update sales batch by ID |
| POST | `/sales/batches/{id}` | Post/finalize sales batch |

---

## Sales Items

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/sales/items` | Get list of sales items |

### Sales Items UDFs

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/sales/items/udf` | Get list of sales items UDFs |
| GET | `/sales/items/udf-fields` | Get list of sales items UDF fields |
| GET | `/sales/items/udf-fields/{field-name}` | Get sales items UDF field by name |
| PUT | `/sales/items/udf-fields/{field-name}` | Update sales items UDF field by name |
| DELETE | `/sales/items/udf-fields/{field-name}` | Delete sales items UDF field by name |
| GET | `/sales/items/udf-pages` | Get list of sales items UDF pages |
| POST | `/sales/items/udf-pages` | Create a sales items UDF page |
| GET | `/sales/items/udf-pages/{id}` | Get sales items UDF page by ID |
| PUT | `/sales/items/udf-pages/{id}` | Update sales items UDF page by ID |
| DELETE | `/sales/items/udf-pages/{id}` | Delete sales items UDF page by ID |

---

## Sales Taxes

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/sales/taxes` | Get list of sales taxes |
| POST | `/sales/taxes` | Create a sales tax |
| GET | `/sales/taxes/{id}` | Get sales tax by ID |
| PUT | `/sales/taxes/{id}` | Update sales tax by ID |
| DELETE | `/sales/taxes/{id}` | Delete sales tax by ID |

### Sales Taxes UDFs

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/sales/taxes/udf` | Get list of sales taxes UDFs |
| GET | `/sales/taxes/udf-fields` | Get list of sales taxes UDF fields |
| GET | `/sales/taxes/udf-fields/{field-name}` | Get sales taxes UDF field by name |
| PUT | `/sales/taxes/udf-fields/{field-name}` | Update sales taxes UDF field by name |
| DELETE | `/sales/taxes/udf-fields/{field-name}` | Delete sales taxes UDF field by name |
| GET | `/sales/taxes/udf-pages` | Get list of sales taxes UDF pages |
| POST | `/sales/taxes/udf-pages` | Create a sales taxes UDF page |
| GET | `/sales/taxes/udf-pages/{id}` | Get sales taxes UDF page by ID |
| PUT | `/sales/taxes/udf-pages/{id}` | Update sales taxes UDF page by ID |
| DELETE | `/sales/taxes/udf-pages/{id}` | Delete sales taxes UDF page by ID |

---

## Salespeople

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/salespeople` | Get list of salespeople |
| POST | `/salespeople` | Create a salesperson |
| GET | `/salespeople/{id}` | Get salesperson by ID |
| PUT | `/salespeople/{id}` | Update salesperson by ID |
| DELETE | `/salespeople/{id}` | Delete salesperson by ID |

### Salespeople UDFs

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/salespeople-udf` | Get list of salespeople UDFs |
| GET | `/salespeople/udf-fields` | Get list of salespeople UDF fields |
| GET | `/salespeople/udf-fields/{field-name}` | Get salespeople UDF field by name |
| PUT | `/salespeople/udf-fields/{field-name}` | Update salespeople UDF field by name |
| DELETE | `/salespeople/udf-fields/{field-name}` | Delete salespeople UDF field by name |
| GET | `/salespeople/udf-pages` | Get list of salespeople UDF pages |
| POST | `/salespeople/udf-pages` | Create a salespeople UDF page |
| GET | `/salespeople/udf-pages/{id}` | Get salespeople UDF page by ID |
| PUT | `/salespeople/udf-pages/{id}` | Update salespeople UDF page by ID |
| DELETE | `/salespeople/udf-pages/{id}` | Delete salespeople UDF page by ID |

---

## Territories

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/territories` | Get list of territories |
| POST | `/territories` | Create a territory |
| GET | `/territories/{id}` | Get territory by ID |
| PUT | `/territories/{id}` | Update territory by ID |
| DELETE | `/territories/{id}` | Delete territory by ID |
