# Database: System & Configuration

Schema: `public` (v3)

### system_settings

Table to control system settings and values

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| key | character varying(128) | Yes |  | Key for setting to control on this record |
| type | configdatatype | Yes |  | Internal use |
| num_data | bigint |  |  | Large number to store on this record |
| curr_data | numeric(15,5) |  |  | Decimal numeric data to store on this record |
| txt_data | character varying |  |  | Text data to store on this record |
| bin_data | bytea |  |  | Binary data to store on this record |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| id | integer | Yes | Yes | Unique identifier |
| user_id | integer |  |  | Identifier from system_users_base.id related to this record |

**Constraints:**
- `system_settings_pkey` (Primary key): (id)
- `system_settings_user_id_fkey` (Foreign key): (user_id) REFERENCES public.system_users_base (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE CASCADE

#### User Permissions in system_settings

Per-user module permissions are stored as rows in `system_settings` with `user_id` linking to `system_users_base.id`. The `key` column follows the pattern `spire.{module}.{action}` and `txt_data` is `'Y'` (allowed) or `'N'` (denied). If no row exists for a user, they inherit the system default.

**Known permission keys:**

| Key | Description |
|-----|-------------|
| `spire.requisitions.allow_view` | View requisitions |
| `spire.requisitions.allow_add` | Create requisitions |
| `spire.requisitions.allow_edit` | Edit requisitions |
| `spire.requisitions.allow_delete` | Delete requisitions |
| `spire.requisitions.allow_process` | Process (approve/convert) requisitions |
| `spire.email.allow_access` | Access email module |
| `spire.gl.allow_access` | Access General Ledger |
| `spire.gl.budgets.allow_access` | Access GL budgets |
| `spire.ap.allow_access` | Access Accounts Payable |
| `spire.ar.allow_access` | Access Accounts Receivable |
| `spire.job.allow_access` | Access Job Costing |
| `spire.employee.allow_access` | Access Employee/Payroll |
| `spire.inventory.access_adjustments` | Access inventory adjustments |
| `spire.inventory.access_transfers` | Access inventory transfers |
| `spire.inventory.count.allow_access` | Access inventory counts |

**Example — check if a user can process requisitions:**

```sql
SELECT ss.txt_data AS allowed
FROM system_settings ss
JOIN system_users_base u ON u.id = ss.user_id
WHERE u.username = 'JSMITH'
  AND ss.key = 'spire.requisitions.allow_process';
```

**Example — list all permissions for a user:**

```sql
SELECT ss.key, ss.txt_data
FROM system_settings ss
JOIN system_users_base u ON u.id = ss.user_id
WHERE u.username = 'JSMITH'
  AND ss.key LIKE 'spire.%.allow_%'
ORDER BY ss.key;
```

### system_users_base

Table for User list

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| username | character varying(12) |  |  | Logon user name |
| initials | character varying(3) |  |  | User initials |
| default_whse | character varying(6) | Yes |  | Default warehouse for user |
| division | character varying(3) | Yes |  | Default division for user |
| default_salesperson | character varying(10) | Yes |  | Default salesperson for user |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| udf_data | hstore |  |  | User defined data for this record |
| uuid | uuid | Yes |  | Universal unique identifier for user |

**Constraints:**
- `users_pkey` (Primary key): (id)

### system_filters

Tables for saved filters

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| window | character varying(100) |  |  | Spire list screen for this record |
| name | character varying(100) |  |  | Name or description of filter as set by user |
| inmenu | boolean |  |  | Display on main menu flag; FALSE = No TRUE = Yes |
| filter_state | json |  |  | Filter criteria |
| table_state | json |  |  | Column settings |
| save_table | boolean | Yes |  | Save this settings flag; FALSE = No TRUE = Yes |
| user_id | integer |  |  | Identifier from system_users_base.id related to this record, blank = all users |

**Constraints:**
- `bve_filter_pkey` (Primary key): (id)
- `system_filters_user_id_fkey` (Foreign key): (user_id) REFERENCES public.system_users_base (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE CASCADE

### system_record_locks

Table to control lock records being edited by a user

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| user_id | integer | Yes |  | Identifier from system_users_base.id related to this record |
| link_table | character varying(4) | Yes |  | Field to use for module linking; COMP = Company CUST = Customer DIV = Division EMP = Employee JC = Job Cost PHIS = Purchase History PORD = Purchase Order SHIS = Sales History SORD = Sales Order VEND = Vendor WHSE = Warehouse |
| link_no | character varying(128) | Yes |  | Field to store join value for link to; customers.cust_no, gl_divisions.division, employees.employee_no, jobs.job_no, purchase_history.purchase_no , purchase_orders.purchase_no, sales_history.invoice_no, sales_orders_order_no, vendors.vendor_no, inventory_warehouses.whse |
| acquired | timestamp without time zone | Yes |  | UTC Date and Time lock was set. |

**Constraints:**
- `bvlock_pkey` (Primary key): (id)
- `system_record_locks_user_id_fkey` (Foreign key): (user_id) REFERENCES public.system_users_base (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE CASCADE

### system_report_permissions

Table to control report permissions when Report security is enabled in Company settings or User settings

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| reportname | character varying(240) |  |  | Report file name |
| can_print | boolean |  |  | Permission to print report; FALSE = No permission TRUE = Has permission |
| can_preview | boolean |  |  | Permission to preview report; FALSE = No permission TRUE = Has permission |
| can_export | boolean |  |  | Permission to export report; FALSE = No permission TRUE = Has permission |
| can_email | boolean |  |  | Permission to email report; FALSE = No permission TRUE = Has permission |
| user_id | integer |  |  | Identifier from system_users_base.id related to this record, blank = all users |

**Constraints:**
- `bve_report_control_pkey` (Primary key): (id)
- `system_report_permissions_user_id_fkey` (Foreign key): (user_id) REFERENCES public.system_users_base (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE CASCADE

### system_sequence_numbers

Table to control all of the next sequence numbers i.e. trans_no, order_no, invoice_no etc.

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| key | character varying(128) | Yes | Yes | Key for sequence number to contol on this record |
| next_id | bigint |  |  | Next sequence number to use |
| initials | character varying(3) |  |  | User intitals that last caused sequence number to increment |
| date | timestamp without time zone |  |  | UTS date and time that sequence number was last incremented |
| time | bigint |  |  | Not used |

**Constraints:**
- `bve_sequence_pkey` (Primary key): (key)

### browser_tabs

Table for browser tab configuration

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| sequence | smallint | Yes |  | Tab order |
| title | varchar |  |  | Tab title |
| url | varchar |  |  | Tab url |
| user_id | integer |  |  | system_users.id if tab is for a specific user, null if company tab |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |

**Constraints:**
- `browser_tabs_pkey` (Primary key): (id)

### record_types

Table for Customer and Inventory types

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| link_tbl | character varying(80) |  |  | Record type; CUST = used for customers INV = used for inventory |
| user_type | character varying(40) |  |  | Type created by user |
| color_text | bigint |  |  | Text colour |
| color_back | bigint |  |  | Background colour |

**Constraints:**
- `bve_user_type_pkey` (Primary key): (id)

### udf_fields

Table of UDF field definitions

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| page_id | integer | Yes |  | Link to udf_pages.id |
| link_table | character varying(32) | Yes |  | Table this UDF definition record is for; AR = Accounts Receivable AP = Accounts Payable BOLi = Production Order Items BORD = Production Order EMP = Employees CUST = Customer INV = Inventory PORD = Purchase Orders PROD = Product Codes SHIS = Sales Sistory SHLi = Sales History Items SORD = Sales Orders SOLi = Sales Order Items VEND = Vendors |
| sequence | integer | Yes |  | Sequential number to control order of items |
| field_name | character varying(32) |  |  | Name of field |
| widget_type | character varying(32) |  |  | Field type |
| field_label | character varying(60) |  |  | Field label |
| align | smallint |  |  | Alignment setting; 0 = Left 1 = Centre 2 = Right |
| field_type | smallint |  |  | Field type; 0 = Numeric 1 = Currency 2 = Percent 3 = Date 4 = Text 5 = Masked Text |
| decimals | smallint |  |  | Number of decimals |
| thousands_sep | boolean |  |  | Use thousands separator; FALSE = No TRE = Yes |
| negative_format | smallint |  |  | Display negative format; 0 = -SYM1,234.00 1 = -1,234.00SYM 2 = SYM1,234.00- 3 = 1,234.00SYM- 4 = (SYM1,234.00) 5 = (1,234.00SYM) 6 = SYM1,234.00 (red) 7 = 1,234.00SYM (red) |
| text_mask | character varying(64) |  |  | Text mask set by user |
| mask_text_case | smallint |  |  | Text case mask; 0 = Any Case 1 = UPPERCASE 2 = lowercase |
| validate_period | boolean |  |  | Validate period flag; FALSE = No TRUE = Yes |
| validate_period_mode | smallint |  |  | Fiscal period validatation mode; 0 = Current month 1 = Current year 2 = Within fiscal periods |
| postdate_warning | boolean |  |  | Post-dated message enabled; FALSE = No TRUE = Yes |
| backdate_warning | boolean |  |  | Back-dated message enabled; FALSE = No TRUE = Yes |
| require_numeric | boolean |  |  | Require numeric data flag; FALSE = No TRUE = Yes |
| min_numeric | bigint |  |  | Minimum number permitted |
| max_numeric | bigint |  |  | Maximum number permitted |
| list_items | character varying[] |  |  | Items list as set by user |
| allow_null_date | boolean |  |  | Allow empty date enabled; FALSE = No TRUE = Yes |
| default_null_date | boolean |  |  | Default empty date enabled; FALSE = No TRUE = Yes |

**Constraints:**
- `udf_fields_pkey` (Primary key): (id)
- `udf_fields_page_id_fkey` (Foreign key): (page_id) REFERENCES public.udf_pages (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION

### udf_pages

Table for UDF page definitiions

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| link_table | character varying(32) | Yes |  | Table this UDF definition record is for; AR = Accounts Receivable AP = Accounts Payable BOLi = Production Order Items BORD = Production Order EMP = Employees CUST = Customer INV = Inventory PORD = Purchase Orders PROD = Product Codes SHIS = Sales Sistory SHLi = Sales History Items SORD = Sales Orders SOLi = Sales Order Items VEND = Vendors |
| sequence | integer | Yes |  | Sequential number to control order of pages |
| label | character varying(60) |  |  | Page label |

**Constraints:**
- `udf_pages_pkey` (Primary key): (id)
- `udf_pages_link_table_sequence_uniq` (Unique): (link_table, sequence) DEFERRABLE INITIALLY DEFERRED

### countries

Table for Countries

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| code | character varying(3) | Yes | Yes | Country code |
| name | character varying(60) |  |  | Country name |

**Constraints:**
- `countries_pkey` (Primary key): (code)

### currencies

Table for Currency rates

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| code | character varying(3) |  |  | Currency code |
| country | character varying(60) |  |  | Currency country |
| currency_desc | character varying(60) |  |  | Currency description |
| units_name | character varying(15) |  |  | Currency official name |
| fractions_name | character varying(15) |  |  | Currency fraction name |
| symbol | character varying(3) |  |  | Currency symbol |
| decimal_point_symbol | character varying(1) |  |  | Decimal point symbol |
| thousands_separator | character varying(1) |  |  | Thousands separator symbol |
| decimal_places | smallint |  |  | Number of decimal places to use |
| symbol_position | character varying(1) |  |  | Symbol position; P = Prefix S = Suffix |
| exchange_rate_method | character varying(1) | Yes |  | Conversion method; / = Direct * = Indirect |
| rate_type | character varying(1) | Yes |  | Conversion rate type to use; B = Buy/Sell F = Fixed |
| gain_loss_gl_acct | character varying(24) |  |  | GL account used for posting gain/loss on currency - links to gl_accounts.account.no |
| buy_rate | numeric(13,7) | Yes |  | Not used |
| indirect_buy_rate | numeric(13,7) | Yes |  | Not used |
| sell_rate | numeric(13,7) | Yes |  | Not used |
| indirect_sell_rate | numeric(13,7) | Yes |  | Not used |
| fixed_rate | numeric(13,7) | Yes |  | Fixed direct rate |
| indirect_fixed_rate | numeric(13,7) | Yes |  | Fixed indirect rate |
| last_year_rate01 | numeric(13,7) |  |  | Fixed rate for last year period 1 |
| last_year_rate02 | numeric(13,7) |  |  | Fixed rate for last year period 2 |
| last_year_rate03 | numeric(13,7) |  |  | Fixed rate for last year period 3 |
| last_year_rate04 | numeric(13,7) |  |  | Fixed rate for last year period 4 |
| last_year_rate05 | numeric(13,7) |  |  | Fixed rate for last year period 5 |
| last_year_rate06 | numeric(13,7) |  |  | Fixed rate for last year period 6 |
| last_year_rate07 | numeric(13,7) |  |  | Fixed rate for last year period 7 |
| last_year_rate08 | numeric(13,7) |  |  | Fixed rate for last year period 8 |
| last_year_rate09 | numeric(13,7) |  |  | Fixed rate for last year period 9 |
| last_year_rate10 | numeric(13,7) |  |  | Fixed rate for last year period 10 |
| last_year_rate11 | numeric(13,7) |  |  | Fixed rate for last year period 11 |
| last_year_rate12 | numeric(13,7) |  |  | Fixed rate for last year period 12 |
| last_year_rate13 | numeric(13,7) |  |  | Fixed rate for last year period 13 |
| this_year_rate01 | numeric(13,7) |  |  | Fixed rate for this year period 1 |
| this_year_rate02 | numeric(13,7) |  |  | Fixed rate for this year period 2 |
| this_year_rate03 | numeric(13,7) |  |  | Fixed rate for this year period 3 |
| this_year_rate04 | numeric(13,7) |  |  | Fixed rate for this year period 4 |
| this_year_rate05 | numeric(13,7) |  |  | Fixed rate for this year period 5 |
| this_year_rate06 | numeric(13,7) |  |  | Fixed rate for this year period 6 |
| this_year_rate07 | numeric(13,7) |  |  | Fixed rate for this year period 7 |
| this_year_rate08 | numeric(13,7) |  |  | Fixed rate for this year period 8 |
| this_year_rate09 | numeric(13,7) |  |  | Fixed rate for this year period 9 |
| this_year_rate10 | numeric(13,7) |  |  | Fixed rate for this year period 10 |
| this_year_rate11 | numeric(13,7) |  |  | Fixed rate for this year period 11 |
| this_year_rate12 | numeric(13,7) |  |  | Fixed rate for this year period 12 |
| this_year_rate13 | numeric(13,7) |  |  | Fixed rate for this year period 13 |
| next_year_rate01 | numeric(13,7) |  |  | Fixed rate for next year period 1 |
| next_year_rate02 | numeric(13,7) |  |  | Fixed rate for next year period 2 |
| next_year_rate03 | numeric(13,7) |  |  | Fixed rate for next year period 3 |
| next_year_rate04 | numeric(13,7) |  |  | Fixed rate for next year period 4 |
| next_year_rate05 | numeric(13,7) |  |  | Fixed rate for next year period 5 |
| next_year_rate06 | numeric(13,7) |  |  | Fixed rate for next year period 6 |
| next_year_rate07 | numeric(13,7) |  |  | Fixed rate for next year period 7 |
| next_year_rate08 | numeric(13,7) |  |  | Fixed rate for next year period 8 |
| next_year_rate09 | numeric(13,7) |  |  | Fixed rate for next year period 9 |
| next_year_rate10 | numeric(13,7) |  |  | Fixed rate for next year period 10 |
| next_year_rate11 | numeric(13,7) |  |  | Fixed rate for next year period 11 |
| next_year_rate12 | numeric(13,7) |  |  | Fixed rate for next year period 12 |
| next_year_rate13 | numeric(13,7) |  |  | Fixed rate for next year period 13 |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User initials that modified this record |
| date_format | character varying(14) |  |  | Date format used by the country of the currency |

**Constraints:**
- `multicurrency_pkey` (Primary key): (id)

### email_templates

Table for email templates

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| name | character varying(40) |  |  | Template name |
| subject | character varying(80) |  |  | Subject to be used on email |
| assigned_to | character varying(12) |  |  | Username assigned to if template was marked private, blank is available to all users |
| body | text |  |  | Content of email body |
| html_body | text |  |  | Content of email body in text/html |
| link_table | character varying(4) |  |  | Template link type |
| default_flag | boolean | Yes |  | Default template flag |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User initials that modified this record |

**Constraints:**
- `bve_emailtemplate_pkey` (Primary key): (id)

### equipment

Table for Equipment; All Equipment labels are user definable however database field names do not change

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| equipment_no | character varying(10) |  |  | Unique identifier in string format |
| cust_no | character varying(20) |  |  | Customer code for this record - links to customers.cust_no |
| unit_no | character varying(80) |  |  | Unit number |
| year | character varying(4) |  |  | Year |
| make | character varying(20) |  |  | Make |
| model | character varying(20) |  |  | Model |
| territory | character varying(10) |  |  | Territory - usually province or state |
| license_no | character varying(80) |  |  | Licence plate number |
| serial_no | character varying(80) |  |  | Serial number |
| extra01 | character varying(80) |  |  | Additional identifier 1 |
| extra02 | character varying(80) |  |  | Additional identifier 2 |
| odometer | integer |  |  | Odometer |
| followup_date | date |  |  | Followup date |
| odometer_type | character varying(1) |  |  | Odometer unit selected ; Blank = Not used K = First unit M = Second unit |
| notes | text |  |  | Equipment notes |
| _dbversion | integer |  |  | Program version that last modified this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User initials that modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| udf_data | jsonb |  |  | User Defined Fields data |
| extra03 | character varying(80) |  |  | Additional identifier 3 |
| extra04 | character varying(80) |  |  | Additional identifier 4 |
| extra05 | character varying(80) |  |  | Additional identifier 5 |

**Constraints:**
- `bve_vehicle_pkey` (Primary key): (id)

### payment_methods

Table for Payment Methods

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| code | character varying(10) | Yes |  | User set payment code |
| description | character varying(60) | Yes |  | Payment description |
| payment_type | character varying(1) | Yes |  | Payment type; C = Cash D = Debit O = Other Q - Cheque R = Credit Card |
| account_no | character varying(24) | Yes |  | General Ledger Account number to post |
| display | boolean | Yes |  | Display payment method in payment dialog; FALSE = No TRUE = Yes |
| sequence | smallint |  |  | Sequence to display in payment dialog; 1 = at the top 99 = at the bottom |
| icon | character varying(30) |  |  | Icon selected to display in payment dialog |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| shortcut | character varying(12) |  |  | Keyboard shortcut for payment method |
| integrated | boolean | Yes |  | Integration flag for payment gateway; FALSE = No TRUE = Yes |

**Constraints:**
- `payment_methods_pkey` (Primary key): (id)

### payment_terms

Table for Payment Terms for Sales and Purchases

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| terms_code | character varying(10) | Yes |  | Code set by user for this terms record |
| description | character varying(60) | Yes |  | Description for this terms record |
| days_before_due | smallint | Yes |  | Number of days from the Invoice date that the payment is due - this is used for invoices in Accounts Receivable and Accounts Payable |
| eom | boolean | Yes |  | Calculate due date from end of transaction month |
| days_allowed | smallint | Yes |  | Number of days from the Invoice date for which a discount is allowed - this is used for invoices in Accounts Receivable and Accounts Payable |
| discount_rate | numeric(5,2) | Yes |  | Rate of the discount - this is used for invoices in Accounts Receivable and Accounts Payable |
| discount_net | boolean | Yes |  | Only apply discount to Net Invoice amount; FALSE = Discount is allowed for the full invoice amount TRUE = Discount is available for the net amount of the invoice |
| discount_freight | boolean | Yes |  | Allow discount on freight amount; FALSE = Freight portion is not discountable TRUE = Freight is included in the discountable amount |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| udf_data | hstore |  |  | User defined data for this record |

**Constraints:**
- `terms_pkey` (Primary key): (id)

### shipping_methods

Table for Shipping Methods

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| code | character varying(10) |  |  | Shipping method code |
| description | character varying(60) |  |  | Shipping method description |
| tracking_url | character varying |  |  | Tracking URL |
| freight_taxable | boolean |  |  | Shipping method taxable flag; FALSE = No TRUE = Yes |
| freight_type | character varying(1) |  |  | Freight Type; A = Fixed amount P = Prompt each time R = Percentage on sales |
| freight_threshold | numeric(15,2) |  |  | Threshhold for no charge freight |
| freight_rate | numeric(5,2) |  |  | Freight rate percent or amount |
| freight_min | numeric(15,2) |  |  | Minimum freight to charge |
| freight_max | numeric(15,2) |  |  | Maximum freight to charge |
| scac | character varying |  |  | Standard Carrier Alpha Code |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| udf_data | hstore |  |  | User defined data for this record |

**Constraints:**
- `ship_via_pkey` (Primary key): (id)

### cash_outs

Table for Cash Outs performed

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| till | character varying(20) |  |  | User entered till name or description |
| start_date | date | Yes |  | Start date for reports |
| end_date | date | Yes |  | End date for reports |
| coin_count | smallint[] | Yes |  | Number of coins for each denomination, .01, .05, .10, .25, .50, 1.00, 2.00 |
| coin_total | numeric(15,2)[] | Yes |  | Value of coins for each denomination, .01, .05, .10, .25, .50, 1.00, 2.00 |
| bill_count | smallint[] | Yes |  | Number of bills for each denomination, $1.00, $2.00, $5.00, $10.00, $20.00, $50.00, $100.00 |
| bill_total | numeric(15,2)[] | Yes |  | Value of bills for each denomination, $1.00, $2.00, $5.00, $10.00, $20.00, $50.00, $100.00 |
| other_total | numeric(15,5) | Yes |  | Total value of all other payment methods |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _modified_by | character varying(3) |  |  | User initials that modified this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| location | character varying(24) |  |  | Location for this Cash count |

**Constraints:**
- `cash_outs_pkey` (Primary key): (id)

### cash_out_payment_methods

Table for Payment Methods selected in Cash Outs

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| cash_out_id | integer |  |  | Cash outs record identifier - links to cash_outs.id |
| payment_method_id | integer |  |  | Payment method record identifier - links to payment_methods.id |
| amount | numeric(15,5) | Yes |  | Payment amount for this method |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _modified_by | character varying(3) |  |  | User initials that modified this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |

**Constraints:**
- `cash_out_payment_methods_pkey` (Primary key): (id)
- `cash_out_payment_methods_cash_out_id_fkey` (Foreign key): (cash_out_id) REFERENCES public.cash_outs (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE CASCADE
- `cash_out_payment_methods_payment_method_id_fkey` (Foreign key): (payment_method_id) REFERENCES public.payment_methods (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION

### integration_providers

Table for Integration Providers

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| name | character varying | Yes |  | Provider name (user facing key) |
| identifier | character varying |  |  | Provider identifier |
| type | smallint | Yes |  | Provider type |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |

**Constraints:**
- `` (): 

### integration_associations

Table for Integration Associations

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| provider_id | integer | Yes |  | Link to integration_providers.id |
| customer_id | integer | Yes |  | Link to customers.id |
| inventory_id | integer |  |  | Link to inventory.id |
| inventory_levy_code_id | integer |  |  | Link to inventory_levy_codes.id |
| inventory_product_code_id | integer |  |  | Link to inventory_product_codes.id |
| payment_terms_id | integer |  |  | Link to payment_terms.id |
| sales_order_id | integer |  |  | Link to sales_orders.id |
| sales_history_id | integer |  |  | Link to sales_history.id |
| shipping_method_id | integer |  |  | Link to shipping_methods.id |
| external_key | character varying |  |  | External key |
| status | character varying |  |  | Integration status (by type) |
| received | character varying |  |  | Data received from third-party |
| transmitted | character varying |  |  | Data sent to third-party |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |

**Constraints:**
- `` (): 
