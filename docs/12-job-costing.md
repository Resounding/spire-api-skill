# Job Costing API

Base Path: `/api/v2/companies/{company-name}`

---

## Job Costing Root

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/job-costing` | Get job costing root |

---

## Job Costing Accounts

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/job-costing-accounts` | Get list of job costing accounts |
| POST | `/job-costing-accounts` | Create a job costing account |
| GET | `/job-costing-accounts/{id}` | Get job costing account by ID |
| PUT | `/job-costing-accounts/{id}` | Update job costing account by ID |
| DELETE | `/job-costing-accounts/{id}` | Delete job costing account by ID |

---

## Job Costing Entries

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/job-costing-entries` | Get list of job costing entries |
| POST | `/job-costing-entries` | Create a job costing entry |
| GET | `/job-costing-entries/{id}` | Get job costing entry by ID |

---

## Job Costing Jobs

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/job-costing-jobs` | Get list of job costing jobs |
| POST | `/job-costing-jobs` | Create a job costing job |
| GET | `/job-costing-jobs/{id}` | Get job costing job by ID |
| PUT | `/job-costing-jobs/{id}` | Update job costing job by ID |
| DELETE | `/job-costing-jobs/{id}` | Delete job costing job by ID |

---

## POST /job-costing-jobs - Create Job

### Request Body

```json
{
  "jobNo": "JOB-001",
  "name": "Customer Project Alpha",
  "customer": {
    "customerNo": "CUST001"
  },
  "status": "A",
  "startDate": "2024-01-15",
  "endDate": "2024-06-30",
  "budget": "50000.00",
  "description": "Custom software development project",
  "udf": {
    "project_manager": "John Smith",
    "department": "Engineering"
  }
}
```

### Job Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `jobNo` | string | Yes | Unique job number |
| `name` | string | Yes | Job name |
| `customer` | object | No | Customer reference |
| `status` | string | No | Status (A=Active, I=Inactive, C=Complete) |
| `startDate` | string | No | Project start date |
| `endDate` | string | No | Project end date |
| `budget` | decimal | No | Budget amount |
| `description` | string | No | Job description |
| `udf` | object | No | User defined fields |

---

## POST /job-costing-entries - Create Entry

### Request Body

```json
{
  "job": {
    "jobNo": "JOB-001"
  },
  "account": {
    "id": 1
  },
  "date": "2024-01-20",
  "amount": "500.00",
  "description": "Labor costs",
  "reference": "INV-12345"
}
```

### Entry Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `job` | object | Yes | Job reference |
| `account` | object | Yes | Cost account reference |
| `date` | string | No | Entry date |
| `amount` | decimal | Yes | Amount |
| `description` | string | No | Description |
| `reference` | string | No | Document reference |

---

## Job Costing Jobs UDFs

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/job-costing-jobs-udf` | Get list of job UDFs |
| GET | `/job-costing-jobs-udf-fields` | Get list of UDF fields |
| GET | `/job-costing-jobs-udf-fields/{field-name}` | Get UDF field by name |
| PUT | `/job-costing-jobs-udf-fields/{field-name}` | Update UDF field by name |
| DELETE | `/job-costing-jobs-udf-fields/{field-name}` | Delete UDF field by name |
| GET | `/job-costing-jobs-udf-pages` | Get list of UDF pages |
| POST | `/job-costing-jobs-udf-pages` | Create UDF page |
| GET | `/job-costing-jobs-udf-pages/{id}` | Get UDF page by ID |
| PUT | `/job-costing-jobs-udf-pages/{id}` | Update UDF page by ID |
| DELETE | `/job-costing-jobs-udf-pages/{id}` | Delete UDF page by ID |

---

## Example Queries

```bash
# Get job costing root
GET /job-costing

# Get all jobs
GET /job-costing-jobs

# Create a job
POST /job-costing-jobs
Content-Type: application/json
{
  "name": "Project Alpha",
  "customer": {"id": 100},
  ...
}

# Get job costing entries
GET /job-costing-entries

# Create an entry
POST /job-costing-entries
Content-Type: application/json
{
  "job": {"id": 1},
  "account": {"id": 10},
  "amount": 500.00,
  ...
}

# Filter jobs by status
GET /job-costing-jobs?filter={"status":"A"}
```
