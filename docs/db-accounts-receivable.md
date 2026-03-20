# Database: Accounts Receivable

Schema: `public` (v3)

### ar_batches

Table for saved Accounts Receivable payment batches

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| currency | character varying(3) |  |  | Currency code for batch - links to currencies-code |
| date | date |  |  | Batch payment date |
| due_by | date |  |  | Date due by filter used for batch |
| terms_code | character varying(10) |  |  | Payment terms filter used for batch |
| total | numeric(15,2) |  |  | Batch total |
| payment_method | integer |  |  | Payment method selected for batch |
| note | character varying(60) |  |  | User added note |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User initials that modified this record |

**Constraints:**
- `ar_batches_pkey` (Primary key): (id)

### ar_batch_items

Table for saved Accounts Receivable payment batch items

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| batch_id | integer |  |  | Batch identifier - links to ar_batches.id |
| transaction_id | integer |  |  | Accounts Receivable transaction identifier - links to ar_transactions.id |
| pay | boolean |  |  | Pay flag; FALSE = No TRUE = Yes |
| give_discount | boolean |  |  | Discount flag; FALSE = No TRUE = Yes |
| discount | numeric(15,2) |  |  | Discount amount |
| total | numeric(15,2) |  |  | Total payment |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User initials that modified this record |
| reference_no | character varying(25) |  |  | Reference number |

**Constraints:**
- `ar_batch_items_pkey` (Primary key): (id)
- `ar_batch_items_batch_id_fkey` (Foreign key): (batch_id) REFERENCES public.ar_batches (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION

### ar_transactions

Table for Accounts Receivable transactions

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| cust_no | character varying(20) |  |  | Customer code - links to customers.cust_no |
| recno | integer |  |  | Record number for transactions with multiple records |
| date | date |  |  | Date of transaction |
| debit_amt | numeric(15,2) |  |  | Debit amoount |
| credit_amt | numeric(15,2) |  |  | Credit amount |
| balance | numeric(15,2) |  |  | Balance outstanding |
| trans_no | character varying(10) |  |  | Transaction number - links to gl_transactions.trans_no |
| ref_no | character varying |  |  | Reference or invoice number |
| code | character varying(1) |  |  | Transaction type code; C = Credit memo D = Debit memo I = Invoice P = Payment S = Service Charge |
| open_close_flag | character varying(1) |  |  | Transaction status flag; O = Open C = Closed |
| item_hold_flag | boolean |  |  | Transaction hold flag; FALSE or empty = No TRUE = Yes |
| due_date | date |  |  | Transaction due date |
| terms_code | character varying(10) |  |  | Terms code for transaction - links to table payment_terms.terms_code |
| terms_description | character varying(60) |  |  | Payment terms description |
| terms_days_before_due | smallint |  |  | Number of days before the payment is due calculated from transaction date |
| terms_days_allowed | smallint |  |  | Number of days allowed for discount |
| terms_discount_rate | numeric(5,2) |  |  | Early payment discount percentage rate |
| arf_user | character varying(3) |  |  | User initials that posted the transaction |
| rate_method | character varying(1) |  |  | Currency rate conversion method used; / = direct * = indirect |
| rate | numeric(13,7) |  |  | Currency rate used |
| base_debit_amt | numeric(15,2) |  |  | Debit amount in base currency |
| base_credit_amt | numeric(15,2) |  |  | Debit amount in base currency |
| note | character varying(60) |  |  | Transaction memo entered by user or Ship to ID and Location code from Sales Iinvoice |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User initials that modified this record |
| sales_tax_no | smallint[] | Yes |  | Sales tax codes 1 - 4 used on transaction |
| sales_tax_rate | numeric(7,4)[] | Yes |  | Sales tax rates 1 - 4 used on transaction |
| sales_tax_total | numeric(15,2)[] | Yes |  | Sales tax 1 - 4 amounts on transaction |
| freight | numeric(15,2) |  |  | Freight amount on transaction |
| cust_po_no | character varying(20) |  |  | Customer Purchase Order number |
| payment_account | character varying(24) |  |  | General Ledger account used for payment - links to gl_accounts.account_no |
| last_four | character varying(4) |  |  | Last 4 characters from credit card |
| payment_method | integer |  |  | Payment method - links to payment_methods.id |

**Constraints:**
- `ar_transactions_pkey` (Primary key): (id)

### ar_transaction_links

Table to link Accounts Receivable Credit transactions to Debit transactions

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| debit_id | integer |  |  | Debit record identifier - links to ar_transactions.id |
| credit_id | integer |  |  | Credit record identifier - links to ar_transactions.id |
| applied_date | date |  |  | Apply date used |
| applied_amt | numeric(15,2) |  |  | Apply amount |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| folio | character varying(32) |  |  | Folio value to group tranasactions that were selected by the user during a session |

**Constraints:**
- `ar_transaction_links_pkey` (Primary key): (id)
