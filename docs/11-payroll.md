# Payroll API

Base Path: `/api/v2/companies/{company-name}`

---

## Payroll Root

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/payroll` | Get payroll root |

---

## Payroll Departments

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/payroll-departments` | Get list of payroll departments |

---

## Payroll Employees

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/payroll-employees` | Get list of payroll employees |
| POST | `/payroll-employees` | Create a payroll employee |
| GET | `/payroll-employees/{id}` | Get payroll employee by ID |
| PUT | `/payroll-employees/{id}` | Update payroll employee by ID |
| DELETE | `/payroll-employees/{id}` | Delete payroll employee by ID |

---

## POST /payroll-employees - Create Employee

### Request Body

```json
{
  "employeeNo": "EMP001",
  "firstName": "John",
  "lastName": "Smith",
  "status": "A",
  "department": {
    "id": 1
  },
  "address": {
    "line1": "123 Main Street",
    "city": "Vancouver",
    "provState": "BC",
    "postalCode": "V6B 1A1",
    "country": "Canada"
  },
  "contact": {
    "email": "john.smith@company.com",
    "phone": "604-555-1234"
  },
  "hireDate": "2024-01-15",
  "birthDate": "1985-06-20",
  "sinNumber": "123456789",
  "payType": "S",
  "payRate": "50000.00",
  "payFrequency": "B",
  "udf": {
    "emergency_contact": "Jane Smith",
    "emergency_phone": "604-555-9999"
  }
}
```

### Employee Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `employeeNo` | string | Yes | Unique employee number |
| `firstName` | string | Yes | First name |
| `lastName` | string | Yes | Last name |
| `status` | string | No | Status (A=Active, I=Inactive, T=Terminated) |
| `department` | object | No | Department reference |
| `address` | object | No | Employee address |
| `contact` | object | No | Contact information |
| `hireDate` | string | No | Hire date |
| `birthDate` | string | No | Birth date |
| `sinNumber` | string | No | Social Insurance Number |
| `payType` | string | No | Pay type (S=Salary, H=Hourly) |
| `payRate` | decimal | No | Pay rate (annual for salary, hourly for hourly) |
| `payFrequency` | string | No | Pay frequency (W=Weekly, B=Bi-weekly, M=Monthly) |
| `udf` | object | No | User defined fields |

### Employee Status Values

| Status | Description |
|--------|-------------|
| `A` | Active |
| `I` | Inactive |
| `T` | Terminated |

---

## PUT /payroll-employees/{id} - Update Employee

```json
{
  "payRate": "55000.00",
  "department": {"id": 2}
}
```

---

## Employee Communications

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/payroll-employees/{id}/communications` | Get list of employee communications |

---

## Employee Contacts

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/payroll-employees/{id}/contacts` | Get list of employee contacts |

---

## Employee Email Messages

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/payroll-employees/{id}/email-messages` | Get list of employee email messages |
| POST | `/payroll-employees/{id}/email-messages` | Create employee email message |

---

## Employee Notes

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/payroll-employees/{id}/notes` | Get list of employee notes |
| POST | `/payroll-employees/{id}/notes` | Create employee note |

---

## Payroll Employees UDFs

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/payroll-employees-udf` | Get list of employee UDFs |
| GET | `/payroll-employees-udf-fields` | Get list of UDF fields |
| GET | `/payroll-employees-udf-fields/{field-name}` | Get UDF field by name |
| PUT | `/payroll-employees-udf-fields/{field-name}` | Update UDF field by name |
| DELETE | `/payroll-employees-udf-fields/{field-name}` | Delete UDF field by name |
| GET | `/payroll-employees-udf-pages` | Get list of UDF pages |
| POST | `/payroll-employees-udf-pages` | Create UDF page |
| GET | `/payroll-employees-udf-pages/{id}` | Get UDF page by ID |
| PUT | `/payroll-employees-udf-pages/{id}` | Update UDF page by ID |
| DELETE | `/payroll-employees-udf-pages/{id}` | Delete UDF page by ID |

---

## Payroll Schedules

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/payroll-schedules` | Get list of payroll schedules |

---

## Employee ROEs (Records of Employment)

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/payroll-employee-roes` | Get list of employee ROEs |

---

## Employee T4s

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/payroll-employee-t4s` | Get list of employee T4s |

---

## Payroll Timecards

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/payroll-timecards` | Get list of payroll timecards |
| POST | `/payroll-timecards` | Create a payroll timecard |
| GET | `/payroll-timecards/{id}` | Get payroll timecard by ID |
| PUT | `/payroll-timecards/{id}` | Update payroll timecard by ID |
| DELETE | `/payroll-timecards/{id}` | Delete payroll timecard by ID |
| POST | `/payroll-timecards/{id}/post` | Post payroll timecard |

---

## Timecard Email Messages

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/payroll-timecards/{id}/email-messages` | Get list of timecard email messages |
| POST | `/payroll-timecards/{id}/email-messages` | Create timecard email message |

---

## Payroll Timecards UDFs

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/payroll-timecards-udf` | Get list of timecard UDFs |
| GET | `/payroll-timecards-udf-fields` | Get list of UDF fields |
| GET | `/payroll-timecards-udf-fields/{field-name}` | Get UDF field by name |
| PUT | `/payroll-timecards-udf-fields/{field-name}` | Update UDF field by name |
| DELETE | `/payroll-timecards-udf-fields/{field-name}` | Delete UDF field by name |
| GET | `/payroll-timecards-udf-pages` | Get list of UDF pages |
| POST | `/payroll-timecards-udf-pages` | Create UDF page |
| GET | `/payroll-timecards-udf-pages/{id}` | Get UDF page by ID |
| PUT | `/payroll-timecards-udf-pages/{id}` | Update UDF page by ID |
| DELETE | `/payroll-timecards-udf-pages/{id}` | Delete UDF page by ID |

---

## Timecard Entry Codes

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/payroll-timecard-entry-codes` | Get list of timecard entry codes |
| POST | `/payroll-timecard-entry-codes` | Create timecard entry code |
| GET | `/payroll-timecard-entry-codes/{id}` | Get entry code by ID |
| PUT | `/payroll-timecard-entry-codes/{id}` | Update entry code by ID |
| DELETE | `/payroll-timecard-entry-codes/{id}` | Delete entry code by ID |

---

## Timecard Batches

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/payroll-timecard-batches` | Get list of timecard batches |

---

## Payroll Year End

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/payroll-year-end` | Perform payroll year end |

---

## Example Queries

```bash
# Get payroll root
GET /payroll

# Get all employees
GET /payroll-employees

# Create an employee
POST /payroll-employees
Content-Type: application/json
{
  "firstName": "John",
  "lastName": "Smith",
  "department": {"id": 1},
  ...
}

# Get timecards
GET /payroll-timecards

# Post a timecard
POST /payroll-timecards/12345/post

# Get employee T4s
GET /payroll-employee-t4s

# Filter employees by department
GET /payroll-employees?filter={"department.id":1}
```
