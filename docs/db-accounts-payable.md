# Database: Accounts Payable

Schema: `public` (v3)

### ap_batches

Table for saved Accounts Payable Batches

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Uniique identifier |
| currency | character varying(3) |  |  | Currency code for batch - links to currencies.code |
| date | date |  |  | Batch payment date |
| due_by | date |  |  | Date due by filter used for batch |
| total | numeric(15,2) |  |  | Batch total |
| payment_account | character varying(24) |  |  | General Ledger account for payments |
| note | character varying(60) |  |  | User added note |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User intitals that created the record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User intitals that last modified the record |
| payment_method | integer |  |  | Payment method identified - Links to payments_methods.id |
| terms_code | character varying(10) |  |  | Payment terms filter used |
| status | character varying(1) | Yes |  |  |

**Constraints:**
- `ap_batches_pkey` (Primary key): (id)

### ap_batch_items

Table for saved Accounts Payable payment batch items

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| batch_id | integer | Yes |  | Batch identifier - links to ap_batches.id |
| transaction_id | integer |  |  | AP transaction identifier - links to ap_transactions.id |
| pay | boolean |  |  | Payment flag; FALSE = No TRUE = Yes |
| take_discount | boolean |  |  | Discount flag; FALSE = No TRUE = Yes |
| discount | numeric(15,2) |  |  | Discount amount |
| total | numeric(15,2) |  |  | Total payment |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| payment_id | integer |  |  |  |
| void_id | integer |  |  |  |

**Constraints:**
- `ap_batch_items_pkey` (Primary key): (id)
- `ap_batch_items_batch_id_fkey` (Foreign key): (batch_id) REFERENCES public.ap_batches (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE CASCADE
- `ap_batch_items_payment_id_fkey` (Foreign key): (payment_id) REFERENCES public.ap_transactions (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION
- `ap_batch_items_transaction_id_fkey` (Foreign key): (transaction_id) REFERENCES public.ap_transactions (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION
- `ap_batch_items_void_id_fkey` (Foreign key): (void_id) REFERENCES public.ap_transactions (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION

### ap_transactions

Table for Accounts Payable Transactions

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| vendor_no | character varying(20) |  |  | Vendor code - links to vendors.vendor_no |
| date | date |  |  | Transaction date |
| trans_no | character varying(10) |  |  | General Ledger transaction number |
| recno | integer |  |  | Record number for transactions with multiple records |
| credit_amt | numeric(15,2) |  |  | Credit amount |
| debit_amt | numeric(15,2) |  |  | Debit amount |
| balance | numeric(15,2) |  |  | Balance outstanding on transaction |
| trans_no | character varying(10) |  |  | General Ledger transaction number - links to gl_transactions.trans_no |
| ref_no | character varying |  |  | Reference number - Invoice or Cheque number |
| code | character varying(1) |  |  | Transaction code; C = Credit memo D = Debit memo I = Invoice P = Payment |
| open_close_flag | character varying(1) |  |  | Transaction status flag; C = Closed O = Open |
| item_hold_flag | boolean |  |  | Transaction hold flag; FALSE or empty = No TRUE = Yes |
| due_date | date |  |  | Transaction due date |
| terms_code | character varying(10) |  |  | Terms code for transaction - links to table payment_terms.terms_code |
| terms_description | character varying(60) |  |  | Payment terms description |
| terms_days_before_due | smallint |  |  | Number of days before the payment is due calculated from transaction date |
| terms_days_allowed | smallint |  |  | Number of days allowed for discount |
| terms_discount_rate | numeric(5,2) |  |  | Early payment discount percentage rate |
| apf_user | character varying(3) |  |  | User initials that posted the transaction |
| rate_method | character varying(1) |  |  | Currency rate conversion method used; / = direct * = indirect |
| rate | numeric(13,7) |  |  | Currency rate used |
| base_debit_amt | numeric(15,2) |  |  | Debit amount in base currency |
| base_credit_amt | numeric(15,2) |  |  | Credit amount in base currency |
| po_no | character varying(10) |  |  | Purchase Order number |
| cheque_void_flag | character varying(1) |  |  | Payment Void flag; 'Y' = payment was voided |
| note | character varying(60) |  |  | Note or memo added to transaction |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User initials that modified this record |
| sales_tax_no | smallint[] | Yes |  | Sales tax codes 1 - 4 used on transaction |
| sales_tax_rate | numeric(7,4)[] | Yes |  | Sales tax 1 - 4 rates used on transaction |
| sales_tax_total | numeric(15,2)[] | Yes |  | Sales tax 1 - 4 totals on transaction |
| freight | numeric(15,2) |  |  | Freight amount on transaction |

**Constraints:**
- `ap_transactions_pkey` (Primary key): (id)

### ap_transaction_links

Table to link Accounts Payable Debit transactions to Credit transactions

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| debit_id | integer |  |  | Debit record identifier - links to ap_transactions.id |
| credit_id | integer |  |  | Credit record identifier - links to ap_transactions.id |
| applied_date | date |  |  | Apply date used |
| applied_amt | numeric(15,2) |  |  | Apply amount |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| folio | character varying(32) |  |  | Folio value to group tranasactions that were selected by the user during a session |

**Constraints:**
- `ap_transaction_links_pkey` (Primary key): (id)
