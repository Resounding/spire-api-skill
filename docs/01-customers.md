# Customers API

Base Path: `/api/v2/companies/{company-name}`

---

## Core Customer Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/customers` | Get list of customers |
| POST | `/customers` | Create a customer |
| GET | `/customers/{id}` | Get customer by ID |
| PUT | `/customers/{id}` | Update customer by ID |
| DELETE | `/customers/{id}` | Delete customer by ID |

---

## POST /customers - Create Customer

### Request Body

```json
{
  "customerNo": "CUST001",
  "name": "Acme Corporation",
  "status": "A",
  "address": {
    "line1": "123 Main Street",
    "line2": "Suite 100",
    "city": "Vancouver",
    "provState": "BC",
    "postalCode": "V6B 1A1",
    "country": "Canada"
  },
  "shippingAddresses": [
    {
      "shipId": "WAREHOUSE",
      "name": "Acme Warehouse",
      "line1": "456 Industrial Ave",
      "city": "Richmond",
      "provState": "BC",
      "postalCode": "V7A 2B2",
      "country": "Canada"
    }
  ],
  "contact": {
    "name": "John Smith",
    "email": "john.smith@acme.com",
    "phone": "604-555-1234",
    "fax": "604-555-1235"
  },
  "currency": {
    "code": "CAD"
  },
  "territory": {
    "code": "WEST"
  },
  "salesperson": {
    "code": "JD"
  },
  "termsCode": "NET30",
  "creditLimit": "50000.00",
  "taxCode": "BC",
  "discount": "5.00",
  "priceLevel": {
    "id": 1
  },
  "applyFinanceCharges": true,
  "receiveStatements": true,
  "receiveBackorderReleases": true,
  "udf": {
    "industry": "Manufacturing",
    "referral_source": "Trade Show"
  }
}
```

### Customer Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `customerNo` | string | Yes | Unique customer number |
| `name` | string | Yes | Customer/company name |
| `status` | string | No | Status (A=Active, I=Inactive, H=Hold) |
| `address` | object | No | Primary billing address |
| `shippingAddresses` | array | No | Array of shipping addresses |
| `contact` | object | No | Primary contact information |
| `currency` | object | No | Currency reference |
| `currency.code` | string | No | Currency code (CAD, USD, etc.) |
| `territory` | object | No | Territory reference |
| `salesperson` | object | No | Salesperson reference |
| `termsCode` | string | No | Payment terms code |
| `creditLimit` | decimal | No | Credit limit amount |
| `taxCode` | string | No | Tax code |
| `discount` | decimal | No | Default discount percentage |
| `priceLevel` | object | No | Price level reference |
| `applyFinanceCharges` | boolean | No | Apply finance charges |
| `receiveStatements` | boolean | No | Receive statements |
| `receiveBackorderReleases` | boolean | No | Receive backorder releases |
| `udf` | object | No | User defined fields |

### Address Object Fields

| Field | Type | Description |
|-------|------|-------------|
| `line1` | string | Address line 1 |
| `line2` | string | Address line 2 |
| `line3` | string | Address line 3 |
| `city` | string | City |
| `provState` | string | Province/State |
| `postalCode` | string | Postal/ZIP code |
| `country` | string | Country |

### Contact Object Fields

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Contact name |
| `email` | string | Email address |
| `phone` | string | Phone number |
| `fax` | string | Fax number |

### Customer Status Values

| Status | Description |
|--------|-------------|
| `A` | Active |
| `I` | Inactive |
| `H` | Hold |

---

## PUT /customers/{id} - Update Customer

Use the same structure as POST. Include only fields you want to update.

```json
{
  "name": "Acme Corporation Ltd.",
  "creditLimit": "75000.00",
  "contact": {
    "email": "newemail@acme.com"
  }
}
```

---

## Customer Addresses

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/customers/{id}/addresses` | Get list of customer addresses |
| POST | `/customers/{id}/addresses` | Create a customer address |
| GET | `/customers/{customer-id}/addresses/{address-id}` | Get customer address by ID |
| PUT | `/customers/{customer-id}/addresses/{address-id}` | Update customer address by ID |
| DELETE | `/customers/{customer-id}/addresses/{address-id}` | Delete customer address by ID |

### POST /customers/{id}/addresses - Create Address

```json
{
  "shipId": "BRANCH2",
  "name": "Acme Branch Office",
  "line1": "789 Commerce Way",
  "city": "Surrey",
  "provState": "BC",
  "postalCode": "V3T 4W9",
  "country": "Canada",
  "contact": {
    "name": "Jane Doe",
    "phone": "604-555-9999"
  }
}
```

---

## Customer Contacts & Communications

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/customers/{id}/contacts` | Get list of customer contacts |
| GET | `/customers/{id}/communications` | Get list of customer communications |

---

## Customer Payments

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/customers/{id}/payments` | Get list of customer payments |
| POST | `/customers/{id}/payments` | Create payment |

### POST /customers/{id}/payments - Create Payment

```json
{
  "date": "2024-01-15",
  "amount": "1500.00",
  "method": {
    "id": 1
  },
  "reference": "CHQ-12345",
  "applyTo": [
    {
      "invoice": {"id": 1001},
      "amount": "1000.00"
    },
    {
      "invoice": {"id": 1002},
      "amount": "500.00"
    }
  ]
}
```

### Payment Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `date` | string | No | Payment date |
| `amount` | decimal | Yes | Payment amount |
| `method` | object | Yes | Payment method reference |
| `reference` | string | No | Check number or reference |
| `applyTo` | array | No | Array of invoices to apply payment |

---

## Customer Credit Authorizations

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/customers/{id}/credit-authorizations` | Create customer credit authorization |

### POST Credit Authorization

```json
{
  "amount": "500.00",
  "authorizationCode": "AUTH123456",
  "expiryDate": "2024-02-15"
}
```

---

## Customer Payment Methods

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/customers/{id}/payment-methods` | Get list of customer payment methods |
| POST | `/customers/{id}/payment-methods` | Create customer payment method |
| DELETE | `/customers/{id}/payment-methods` | Delete customer payment methods |
| GET | `/customers/{customer-id}/payment-methods/{payment-id}` | Get customer payment method by ID |
| PUT | `/customers/{customer-id}/payment-methods/{payment-id}` | Update customer payment method by ID |
| DELETE | `/customers/{customer-id}/payment-methods/{payment-id}` | Delete customer payment method by ID |

### POST Payment Method

```json
{
  "paymentType": "C",
  "cardholderName": "John Smith",
  "cardNumber": "************1234",
  "expiryMonth": "12",
  "expiryYear": "2025",
  "default": true
}
```

---

## Customer Payment Provider

| Method | Endpoint | Description |
|--------|----------|-------------|
| PUT | `/customers/{id}/payment-provider` | Update customer payment provider |

### PUT Payment Provider

```json
{
  "providerId": "stripe",
  "customerId": "cus_12345abcdef"
}
```

---

## Customer Part Numbers

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/customers/{id}/part-nos` | Get list of customer part numbers |
| GET | `/customers/{customer-id}/part-nos/{partno-id}` | Get customer part number by ID |
| DELETE | `/customers/{customer-id}/part-nos/{partno-id}` | Delete customer part number by ID |

---

## Customer Notes

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/customers/{id}/notes` | Get list of customer notes |
| POST | `/customers/{id}/notes` | Create a customer note |

### POST Note

```json
{
  "noteType": {"id": 1},
  "note": "Customer prefers email communication.",
  "date": "2024-01-15"
}
```

---

## Customer Email Messages

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/customers/{id}/email-messages` | Get list of customer email messages |
| POST | `/customers/{id}/email-messages` | Create a customer email message |

### POST Email Message

```json
{
  "to": "john.smith@acme.com",
  "cc": "accounts@acme.com",
  "subject": "Account Statement",
  "body": "Please find attached your account statement..."
}
```

---

## Customer Billing UDFs

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/customers/udf-billing` | Get list of customer billing UDFs |
| GET | `/customers/udf-billing-fields` | Get list of customer billing UDF fields |
| GET | `/customers/udf-billing-fields/{field-name}` | Get customer billing UDF field by name |
| PUT | `/customers/udf-billing-fields/{field-name}` | Update customer billing UDF field by name |
| DELETE | `/customers/udf-billing-fields/{field-name}` | Delete customer billing UDF field by name |
| GET | `/customers/udf-billing-pages` | Get list of customer billing UDF pages |
| POST | `/customers/udf-billing-pages` | Create a customer billing UDF page |
| GET | `/customers/udf-billing-pages/{id}` | Get customer billing UDF page by ID |
| PUT | `/customers/udf-billing-pages/{id}` | Update customer billing UDF page by ID |
| DELETE | `/customers/udf-billing-pages/{id}` | Delete customer billing UDF page by ID |

### POST UDF Page

```json
{
  "label": "Custom Billing Info"
}
```

### PUT UDF Field

```json
{
  "label": "Tax Exempt Number",
  "pageId": 1,
  "sequence": 0
}
```

---

## Customer Shipping UDFs

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/customers/udf-shipping` | Get list of customer shipping UDFs |
| GET | `/customers/udf-shipping-fields` | Get list of customer shipping UDF fields |
| GET | `/customers/udf-shipping-fields/{field-name}` | Get customer shipping UDF field by name |
| PUT | `/customers/udf-shipping-fields/{field-name}` | Update customer shipping UDF field by name |
| DELETE | `/customers/udf-shipping-fields/{field-name}` | Delete customer shipping UDF field by name |
| GET | `/customers/udf-shipping-pages` | Get list of customer shipping UDF pages |
| POST | `/customers/udf-shipping-pages` | Create a customer shipping UDF page |
| GET | `/customers/udf-shipping-pages/{id}` | Get customer shipping UDF page by ID |
| PUT | `/customers/udf-shipping-pages/{id}` | Update customer shipping UDF page by ID |
| DELETE | `/customers/udf-shipping-pages/{id}` | Delete customer shipping UDF page by ID |

---

## Example Queries

```bash
# Get all customers
GET /customers

# Get customer with specific ID
GET /customers/123

# Get customer by customer number
GET /customers?filter={"customerNo":"CUST001"}

# Get customers with pagination
GET /customers?start=0&limit=50

# Get specific fields only
GET /customers?fields=id,name,customerNo,address.city,creditLimit

# Filter active customers
GET /customers?filter={"status":"A"}

# Filter by territory
GET /customers?filter={"territory.code":"WEST"}

# Search by name (case-insensitive contains)
GET /customers?filter={"name":{"$icontains":"acme"}}

# Sort by name
GET /customers?sort=name

# Sort by credit limit descending
GET /customers?sort=-creditLimit

# Include UDF fields
GET /customers?udf=1

# Create a minimal customer
POST /customers
Content-Type: application/json
{
  "customerNo": "NEWCUST",
  "name": "New Customer Inc."
}

# Update customer credit limit
PUT /customers/123
Content-Type: application/json
{
  "creditLimit": "100000.00"
}

# Get customer addresses
GET /customers/123/addresses

# Get customer payments
GET /customers/123/payments

# Create customer payment
POST /customers/123/payments
Content-Type: application/json
{
  "amount": "1500.00",
  "method": {"id": 1},
  "reference": "CHQ-12345"
}
```
