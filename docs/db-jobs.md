# Database: Job Costing

Schema: `public` (v3)

### jobs

Table for Jobs

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| job_no | character varying(10) |  |  | Job number as set by user |
| account | character varying(10) |  |  | Job account as set by user - blank for Job header record |
| name | character varying(60) |  |  | Job account name or description |
| start_date | date |  |  | Start date as set by user |
| end_date | date |  |  | End date as set by user |
| trans_flag | boolean |  |  |  |
| memo | character varying(60) |  |  | Reference as set by user |
| status | character varying(1) |  |  | Job status; A = Active C = Completed H = Hold |
| ranking | numeric(5) |  |  | Rank as set by user |
| contract_no | character varying(10) |  |  | Contract number |
| contract_date | date |  |  | Contract date |
| estimator | character varying(60) |  |  | Job estimator |
| estimated_income | numeric(15,2) |  |  | Estimated Income or Revenue |
| actual_income | numeric(15,2) |  |  | Actual Income or Revenue |
| estimated_expense | numeric(15,2) |  |  | Estimated Expenses |
| actual_expense | numeric(15,2) |  |  | Actual Expenses |
| actual_hours | numeric(9,2) |  |  | Actual hours |
| estimated_hours | numeric(9,2) |  |  | Estimated hours |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| udf_data | hstore |  |  | User defined data for this record |

**Constraints:**
- `job_header_pkey` (Primary key): (id)

### job_items

Table for Job transactions

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| job_no | character varying(10) |  |  | Job number - links to jobs.job_no |
| account | character varying(10) |  |  | Job account number - links to jobs.account |
| date | date |  |  |  |
| folio | character varying(10) |  |  | Job Transaction number for this record |
| recno | integer |  |  | Sequential record number |
| where_from | character varying(4) |  |  | The module that the transaction originated from; AP = Accounts Payable AR = Accounts Receivable GL = General Ledger INVC = Inventory Count INVR = Inventory Receipts LWAY = Sales Deposit MFG = Manufacturing PAYR = Payroll PORD = Purchase Orders SORD = Sales Orders |
| jcd_user | character varying(3) |  |  | User initials that posted the transaction |
| mf_who | character varying(10) |  |  | Transaction type |
| mf_key | character varying(20) |  |  | The code of the associated record involved in the transaction; Vendor code Customer code Employee code Inventory code |
| mf_tran | character varying(25) |  |  | Document reference or document number for transaction, Invoice or Purchase Order number. |
| post_date | date |  |  | The date the transaction was posted as per user's log on date. |
| tran_date | date |  |  | Transaction date used to set fiscal period for transaction |
| expense | numeric(15,2) | Yes |  | Expense amount for this transaction |
| income | numeric(15,2) | Yes |  | Income or Revenue amount for this transaction |
| hours | numeric(9,2) | Yes |  | Hours for transaction |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| gl_memo | character varying(60) |  |  | Memo created automatically by some subledger transactions or entered manually by the user |
| foreign_expense | numeric(15,2) | Yes |  | Expense in foreign currency if applicable |
| foreign_income | numeric(15,2) | Yes |  | Income or Revenue in foreign currency if applicable |
| currency | character varying(3) |  |  | Currency for transaction; blank = base currency - links to currencies.code |
| currency_rate | numeric(13,7) |  |  | Currency conversion rate used |
| currency_rate_method | character varying(1) |  |  | Currency rate conversion method used; / = direct * = indirect |

**Constraints:**
- `job_detail_pkey` (Primary key): (id)

### phases

Table for Phases available to Sales Orders, Purchase Orders and Production Orders

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| phase_type | character varying(4) |  |  | Phase type SORD = Sales order PORD = Purchae Order BORD = Production Order |
| phase_id | character varying(20) |  |  | User set code for this Phase |
| phase_desc | character varying(100) |  |  | User set Description of this Phase |
| report_tmpl | character varying(20) |  |  | Crystal Reports template used when order enters this Phase |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| next_phases | character varying(20)[] | Yes |  | The next Phase ID that the Order goes to when Next Phase is clicked |

**Constraints:**
- `bve_phase_pkey` (Primary key): (id)

### phase_history

Table to store the changes to Phases on Orders

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| phase_type | character varying(4) |  |  | Order type that Phase records are linked to; BORD = Production Order BHIS = Production History PORD = Purchase Order PHIS = Purchase History SORD = Sales Order SHIS = Sales History |
| order_no | character varying(10) |  |  | Sales, Purchase or Production order number that this record is linked to |
| phase_id | character varying(20) |  |  | User set Phase code - links to phases.phase_id |
| phase_desc | character varying(100) |  |  | Phase description |
| operator | character varying(3) |  |  | Initials of the person that moved it to this Phase |
| carrier | character varying(20) |  |  | User set Carrier name if shipping involved in this Phase change |
| ref_no | character varying(50) |  |  | User set Reference number for this Phase change |
| notes | text |  |  | User entered Notes for this Phase change |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| started | timestamp without time zone |  |  | Date/Time that the Order entered this Phase |
| ended | timestamp without time zone |  |  | Date/Time that the Order left this Phase. |

**Constraints:**
- `bve_phase_dtl_pkey` (Primary key): (id)
