# Database: Customers

Schema: `public` (v3)

### customers

Table for Customers

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique indentifier |
| cust_no | character varying(20) |  |  | Customer code |
| name | character varying(60) |  |  | Customer name |
| currency | character varying(3) | Yes |  | Customer currency; blank = base currency - links to currencies.code |
| reference | character varying(60) |  |  | Customer reference number |
| last_invoice_no | character varying(25) |  |  | Customer's last invoice number posted |
| last_invoice_date | date |  |  | Customer's last date invoice date |
| present_bal | numeric(15,2) |  |  | Customer's present accounts receivable balance |
| no_credit | smallint |  |  | Customer’s Credit Limit Type; 0 = No Credit 1 = Unlimited Credit 2 = Limited Credit |
| credit_line | numeric(13) |  |  | Customer's credit limit |
| disc_pct | numeric(5,2) |  |  | Customer's default invoice discount % that is applied after line discounts |
| terms_code | character varying(10) |  |  | Customer's terms code - links to payment_terms.terms_code |
| notes | character varying(30) |  |  | Customer's note that is displayed on top of Sales Order entry screen. Can be set in Edit Customer, General, User Defined Fields |
| misc1 | character varying(40) |  |  | Customer type field that is used in price matrix for group pricing. Can be set in Edit Customer, General, User Defined Fields - links to record_types.user_type where record_types.link_tbl='CUST' |
| spec_handling | character varying(1) |  |  | Customer's special field . Can be set in Edit Customer, General, User Defined Fields. A-Z for use only in reporting and filtering. |
| statement_code | boolean | Yes |  | Customer's statement setting as set in Edit Customer, Billing, Satements and Invoices; FALSE = Not required TRUE = Statement required |
| svce_chg_code | boolean | Yes |  | Customer's Service / Finance charge flag; FALSE = No TRUE = Yes |
| tax_prompt | boolean | Yes |  | Not used |
| dflt_ship_to | character varying(20) |  |  | Customer's default ship to code - used as the default ship to on all new sales orders for this customer |
| upload | boolean | Yes |  | Not used by Spire - populated with 'FALSE' |
| last_sale_amt | numeric(15,2) |  |  | Customer's last sale amount |
| last_pmt_amt | numeric(15,2) |  |  | Customer's last payment amount |
| statement_type | character varying(1) |  |  | Customer's statement setting as set in Edit Customer, Billing, Satements and Invoices; B = Both form and email E = Email F = Form N = Not required |
| invoice_type | character varying(1) |  |  | Customer's invoice setting as set in Edit Customer, Billing, Satements and Invoices; B = Both form and email E = Email F = Form N = Not required |
| po_no_required | boolean | Yes |  | Customer's purchase order number required flag; FALSE = No TRUE = Yes |
| ar_account | character varying(24) |  |  | General Ledger control account used for Accounts Receivable - links to gl_accounts.account_no |
| avg_days_to_pay | numeric(15,2) |  |  | Customer's average days to pay accounts receivable |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User initials that modified this record |
| approved_by | character varying(3) |  |  | User initials that approved customer's credit limit change |
| approved_date | date |  |  | Date that the customer's credit limit was changed |
| color_text | bigint | Yes |  | Text colour code for display of name |
| color_back | bigint | Yes |  | Background colour code for display of name |
| website | character varying(125) |  |  | Customer's website address |
| open_ord | numeric(15,2) |  |  | Customer's un-invoiced Sales Orders value |
| bank_institution | character varying(3) |  |  | Customer's bank institution number used for EFT |
| bank_transit | character varying(9) |  |  | Customer's bank transit number used for EFT |
| bank_account | character varying(31) |  |  | Customer's bank account number used for EFT |
| status | character varying(1) | Yes |  | Customer's current status; A = Active I = Inactive P = Prospect |
| levy_exempt | boolean | Yes |  | Customer's status for charging inventory levies; FALSE = Not exempt, charge levies TRUE = Exempt, do not charge levies |
| surcharge_exempt | boolean | Yes |  | Customer's status for charging service / finance charges; FALSE = Not exempt, charge service fee TRUE = Exempt, do not charge fee |
| last_year_sales | numeric(15,2)[] | Yes |  | Customer's total sales last year by period |
| this_year_sales | numeric(15,2)[] | Yes |  | Customer's total sales this year by period |
| next_year_sales | numeric(15,2)[] | Yes |  | Customer's total sales next year by period |
| last_year_gp | numeric(15,2)[] | Yes |  | Customer's total gross profit last year by period |
| this_year_gp | numeric(15,2)[] | Yes |  | Customer's total gross profit this year by period |
| next_year_gp | numeric(15,2)[] | Yes |  | Customer's total gross profit next year by period |
| hold | boolean | Yes |  | Customer's on hold flag; FALSE = Not on hold TRUE = On hold |
| last_pmt_date | date |  |  | Customer's last payment date in Accounts Receivable |
| udf_data | hstore |  |  | User defined data for this record |
| tax_id_type | character varying(1) |  |  | Type of identifier specified by tax_id |
| tax_id | character varying(60) |  |  | Customer tax identifier (typically VAT identification number) |
| last_modified | timestamp without time zone |  |  | UTC Date and time record was last modified by a Spire process, not a user edit |

**Constraints:**
- `customer_pkey` (Primary key): (id)

### customer_code_changes

Table for Customer Code changes

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique indentifier |
| old_customer_no | character varying(34) |  |  | Customer code to be replaced |
| new_customer_no | character varying(34) |  |  | New customer code used for replacement |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User initials that modified this record |

**Constraints:**
- `bve_customer_change_pkey` (Primary key): (id)

### customer_part_nos

Table for Customer Part Numbers

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| customer_id | integer | Yes |  | Customer |
| inventory_id | integer | Yes |  | Inventory |
| inventory_uom_id | integer |  |  | Unit of Measure |
| customer_part_no | character varying |  |  | Customer part number |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
