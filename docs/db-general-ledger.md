# Database: General Ledger

Schema: `public` (v3)

### gl_accounts

Table for General Ledger Accounts

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| division | character varying(3) |  |  | Division code for Geneal Ledger account as per divisions in a company; 000 = non divisionalized company or consoldated division for multi division company - links to gl_divisions.code |
| account_no | character varying(24) |  |  | General Ledger account number |
| currency | character varying(3) |  |  | General Ledger account currency; blank = base currency - links to currencies.code |
| gl_group | character varying(3) |  |  | Group for General Ledger account - links to gl_groups.gl_group |
| subgroup | character varying(6) |  |  | Subgroup for General Ledger account - links to gl_subgroups.subgroup |
| name | character varying(60) |  |  | General Ledger account name |
| dr_cr_desig | character varying(1) |  |  | Normal balance for the account; C = Credit D = Debit |
| sales_acct | boolean |  |  | Sales account flag; TRUE = Yes FALSE = No |
| gifi_acct_no | character varying(4) |  |  | General Ledger account 4 digit GIFI code |
| opening_balance | numeric(15,2) |  |  | The opening balance for period 1 of last year, of the GL account, as posted by the Year End Close function. Income and expense accounts have a zero open balance. |
| foreign_opening_balance | numeric(15,2) |  |  | Opening balance in foreign currency if this is a foreign account. |
| next_cheque_no | bigint |  |  | Next cheque number, only used when this General Ledger account is a bank account |
| bank_acct | boolean |  |  | Bank account flag; TRUE = Yes FALSE = No |
| mc_revalue_acct | boolean |  |  | Multicurrency revaluation flag; TRUE = Yes FALSE = No |
| type | character varying(1) |  |  | Type of account; A = Asset L = Liability and Equity R = Revenue X = Expense |
| allocation_flag | boolean |  |  | General Ledger account allocation flag; TRUE = Is an allocation account FALSE = Is not an allocation account |
| inactive | boolean |  |  | General Ledger account inactive flag; TRUE = Account is inactive and cannot be posted to FALSE = Account is active |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User initials that modified this record |
| segments | character varying(24)[] |  |  | Account segments; segments [1] =Segment 1 segments [2] =Segment 2 segments [3] =Segment 3 segments [4] =Segment 4 |
| last_year | numeric(15,2)[] | Yes |  | Account change per period 1-13, last year; last_year [1] = last year period 1 net change last_year [2] = last year period 2 net change |
| this_year | numeric(15,2)[] | Yes |  | Account change per period 1-13, this year; this_year [1] = this year period 1 net change this_year [2] = this year period 2 net change |
| next_year | numeric(15,2)[] | Yes |  | Account change per period 1-13, next year; next_year [1] = next year period 1 net change next_year [2] = next year period 2 net change |
| last_year_budget | numeric(15,2)[] | Yes |  | Account budget per period 1-13, last year; last_year_budget [1] = last year budget period 1 last_year_budget [2] = last year budget period 2 |
| this_year_budget | numeric(15,2)[] | Yes |  | Account budget per period 1-13, this year; this_year_budget [1] = this year budget period 1 this_year_budget [2] = this year budget period 2 |
| next_year_budget | numeric(15,2)[] | Yes |  | Account budget per period 1-13, next year; next_year_budget [1] = next year budget period 1 next_year_budget [2] = next year budget period 2 |
| last_year_forecast | numeric(15,2)[] | Yes |  | Account forecast per period 1-13, last year; last_year_forecast [1] = last year forecast period 1 last_year_forecast [2] = last year forecast period 2 |
| this_year_forecast | numeric(15,2)[] | Yes |  | Account forecast per period 1-13, this year; this_year_forecast [1] = this year forecast period 1 this_year_forecast [2] = this year forecast period 2 |
| next_year_forecast | numeric(15,2)[] | Yes |  | Account forecast per period 1-13, next year; next_year_forecast [1] = next year forecast period 1 next_year_forecast [2] = next year forecast period 2 |
| foreign_last_year | numeric(15,2)[] | Yes |  | Foreign account change per period 1-13, last year; foreign_last_year [1] = last year period 1 net change foreign_last_year [2] = last year period 2 net change |
| foreign_this_year | numeric(15,2)[] | Yes |  | Foreign account change per period 1-13, this year; foreign_this_year [1] = this year period 1 net change foreign_this_year [2] = this year period 2 net change |
| foreign_next_year | numeric(15,2)[] | Yes |  | Foreign account change per period 1-13, next year; foreign_next_year [1] = next year period 1 net change foreign_next_year [2] = next year period 2 net change |
| foreign_last_year_budget | numeric(15,2)[] | Yes |  | Foreign account budget per period 1-13, last year; foreign_last_year_budget [1] = last year budget period 1 foreign_last_year_budget [2] = last year budget period 2 |
| foreign_this_year_budget | numeric(15,2)[] | Yes |  | Foreign account budget per period 1-13, this year; foreign_this_year_budget [1] = this year budget period 1 foreign_this_year_budget [2] = this year budget period 2 |
| foreign_next_year_budget | numeric(15,2)[] | Yes |  | Foreign account budget per period 1-13, next year; foreign_next_year_budget [1] = next year budget period 1 foreign_next_year_budget [2] = next year budget period 2 |
| foreign_last_year_forecast | numeric(15,2)[] | Yes |  | Foreign account forecast per period 1-13, last year; foreign_last_year_forecast [1] = last year forecast period 1 foreign_last_year_forecast [2] = last year forecast period 2 |
| foreign_this_year_forecast | numeric(15,2)[] | Yes |  | Foreign account forecast per period 1-13, this year; foreign_this_year_forecast [1] = this year forecast period 1 foreign_this_year_forecast [2] = this year forecast period 2 |
| foreign_next_year_forecast | numeric(15,2)[] | Yes |  | Foreign account forecast per period 1-13, next year; foreign_next_year_forecast [1] = next year forecast period 1 foreign_next_year_forecast [2] = next year forecast period 2 |
| last_stmt_end_date | date |  |  | Last bank statement end date. Only used for bank accounts. |
| last_stmt_balance | numeric(15,2) |  |  | Last bank statement end balance. Only used for bank accounts. |
| udf_data | jsonb |  |  | User Defined Fields data |

**Constraints:**
- `gl_chart_of_accounts_pkey` (Primary key): (id)
- `zero_allocation_balance` (Check): (this_year = '{0,0,0,0,0,0,0,0,0,0,0,0,0}'::numeric[] AND this_year = '{0,0,0,0,0,0,0,0,0,0,0,0,0}'::numeric[] AND next_year = '{0,0,0,0,0,0,0,0,0,0,0,0,0}'::numeric[] OR allocation_flag = false)

### gl_allocations

Table for General Ledger Allocation posting control

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| alloc_account_no | character varying(24) |  |  | Pseudo account number assigned to the allocation |
| rec_no | integer |  |  | Record number, to order the accounts the allocation will post to |
| account_no | character varying(24) |  |  | General Ledger account that the allocation will redirect to - links to gl_accounts.account_no |
| alloc | numeric(19,4) |  |  | Allocation percentage that will post to this account |
| _dbversion | integer |  |  | Program version that last modified this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User initials that modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |

**Constraints:**
- `gl_allocations_pkey` (Primary key): (id)

### gl_divisions

Table for General Ledger divisions. Only one record will exist for non divisionalized companies. One extra record will exist for divisionalized companies, '000', for the consolidated division.

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| division | character varying(3) |  |  | Division code; 000 = non divisionlized company or consolidated division for multi division company |
| name | character varying(60) |  |  | Division name |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User initials that modified this record |

**Constraints:**
- `gl_divisions_pkey` (Primary key): (id)

### gl_groups

Table for General Ledger Account Groups

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| gl_group | character varying(3) |  |  | Group code |
| name | character varying(60) |  |  | Group name as assigned by user |
| alias | character varying(60) |  |  | Group name to print on reports |
| acct_type | character varying(1) |  |  | Account type; A = Asset L = Liability and Equity R = Revenue X = Expense |
| total_heading | character varying(60) |  |  | Text to display beside Total on Reports |
| total | smallint |  |  | Total Flag; 0 = No total 1 = Total with reset 2 = Total with no reset |
| line_advance | character varying(1) |  |  | Line advance setting for reports; 0 = No additional lines 1 - 5 = Number of additional lines to advance P = Page advance |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User initials that modified this record |

**Constraints:**
- `gl_groups_pkey` (Primary key): (id)

### gl_periods

Table of General Ledger Periods for Last Year, This Year and Next Year

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| year | character(1) | Yes | Yes | Period fiscal year identifier; L = Last year T = This year N = Next year |
| period | smallint | Yes | Yes | Period number 1 - 13 for each for year |
| end_date | date |  |  | Period end date |
| start_date | date |  |  | Period start date |
| year_end | date |  |  | Fiscal Year End date for this period |
| locked | boolean | Yes |  | Period locked flag; FALSE = Period is not locked TRUE = Period is locked |

**Constraints:**
- `gl_periods_pkey` (Primary key): (year, period)

### gl_reconciliation_items

Table for list of Reconciliation items

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| reconciliation_id | integer | Yes |  | gl_reconciliations.id reference |
| transaction_id | integer | Yes |  | gl_transactions.id reference |
| reconciled | boolean |  |  | Previously reconciled flag; FALSE = No TRUE = Yes |
| reconcile | boolean |  |  | Reconcile flag for this session; FALSE = No TRUE = Yes |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User initials that modified this record |

**Constraints:**
- `bve_recon_detail_pkey` (Primary key): (id)

### gl_reconciliations

Table for Reconciliations saved, not yet posted

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| account_id | integer | Yes |  | gl_accounts.id reference |
| open_bal | numeric(15,2) |  |  | Opening balance for reconcilation sesison. Can carried forward from last reconciliation of this account but is editable. |
| close_bal | numeric(15,2) |  |  | Closong balance for reconcilation sesison, usually taken from bank statement |
| end_date | date |  |  | End date to filter out transactions past this date |
| status | character varying(1) | Yes |  | Reconciliation status; O = Open C = Closed X = Cancelled |
| memo | text |  |  | Memo |
| variance | numeric(15,2) |  |  | Reconciliation discrepancy |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User initials that modified this record |

**Constraints:**
- `bve_recon_pkey` (Primary key): (id)

### gl_recurring_pending

Table for Recurring transactions

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| temp | character varying(6) |  |  | Recurring transaction template number - links to gl_recuring_transactions.template_temp |
| rjep_date | date |  |  | Planned post date for Open transaction |
| status | character varying(1) |  |  | Record status; C = Closed or Posted O = Open or unposted |
| post_date | date |  |  | Posted date for closed transactions |
| folio | character varying(10) |  |  | Transaction number of posted transaction |

**Constraints:**
- `recurring_pending_pkey` (Primary key): (id)

### gl_recurring_transactions

Table of Templates defining Recurring transactions

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| template_temp | character varying(6) |  |  | Template number - links to gl_recurring_pending.temp |
| template_desc | character varying(60) |  |  | Template description |
| template_freq | character varying(1) |  |  | Template frequency; 2 = Bi-Monthly A = Annual B = Bi-Weekly D = Daily F = Fiscal period M = Monthly Q = Quarterly S = Specific date W = Weekly |
| template_stat | character varying(1) |  |  | Template status; A = Active S = Suspended U = Unpostable |
| template_folio | character varying(10) |  |  | General Ledger transaction that will be mimiced. Updated at each posting occurence |
| template_tran_date | date |  |  | Previoous date this recurring transaction was posted |
| template_arap_no | character varying(20) |  |  | Customer or vendor code for the transaction being recurred for Accounts Receivable and Accounts Payable postings only |
| ar_ref | character varying(9) |  |  | Reference to post with transaction |
| template_module | character varying(1) |  |  | Module to post that this transaction came from; G = General Ledger P = Accounts Payable R = Accounts Receivable |
| template_rec_info | character varying(10) |  |  | Information to use with template_freq to create repeat cycle; template_freq - template_rec_info 2 - the two days of the month to post A - date to post B - the day of the week to post, 1 = Monday, 2 = Tuesday etc D - 1 = Monday through Friday, 2 = Monday through Saturday, 3 = Monday through Sunday F - F= post on first day of period, L= last day of period M - F = post on first day of month, L = last day of month Q - blank S - day of the month to post W - the day of the week to post, 1 = Monday, 2 = Tuesday etc |
| template_start | date |  |  | Date to start posting recurring transaction |
| template_end | date |  |  | Date to end posting recurring transaction |
| template_userid | character varying(3) |  |  | User initials that created this record |

**Constraints:**
- `recurring_template_pkey` (Primary key): (id)

### gl_segments

Table of General Ledger Segments

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique indentifier |
| segment_no | character varying(1) |  |  | The segment number. 1 - 4 |
| code | character varying(24) |  |  | General Ledger code for the segment |
| description | character varying(60) |  |  | Segment description |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified_by | character varying(3) |  |  | User initials that modified this record |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |

**Constraints:**
- `gl_segments_pkey` (Primary key): (id)

### gl_special_accounts

Table of Special Accounts for system posting

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| key | character varying(40) | Yes | Yes | Special account key |
| account | character varying(24) |  |  | General Ledger account number as set in company settings - links to gl_accounts.account_no |
| currency | character varying(3) | Yes | Yes | General Ledger account currency - links to gl_accounts.currency |

**Constraints:**
- `gl_special_accounts_pkey` (Primary key): (key, currency)

### gl_subgroups

Table for General Ledger Account Sub Groups

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| subgroup | character varying(6) |  |  | Sub group code |
| name | character varying(60) |  |  | Sub group name or description |
| gl_group | character varying(3) |  |  | Member of this General Ledger account group - links to gl_groups.gl_group |
| suppress_flag | boolean |  |  | Supress flag for reporting; FALSE = Do not suppress TRUE = Supress |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |

**Constraints:**
- `gl_sub_groups_pkey` (Primary key): (id)

### gl_transactions

General Ledger Transactions for the fiscal last year, this year and next year

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| division | character varying(3) |  |  | Division code; 000 = non divisionlized company or consolidated division for multi division company Note: if the divsion &lt;&gt; "000" on this record then another duplicate of this record will exist where division = "000" to post the same transaction to the consolidated division - links to gl_divisions.division |
| account_no | character varying(24) |  |  | General Ledger account number - links to gl_history_accounts.account_no |
| currency | character varying(3) |  |  | General Ledger account currency; blank = base currency - links to currencies.code |
| date | date |  |  | Transaction date used to set fiscal period for transaction |
| trans_no | character varying(10) |  |  | General Ledger transaction number |
| recno | integer |  |  | Transaction record number, increments sequentially |
| where_from | character varying(4) |  |  | The module that the transaction originated from; AP = Accounts Payable AR = Accounts Receivable GL = General Ledger INVC = Inventory Count INVR = Inventory Receipts LWAY = Sales Deposit MFG = Manufacturing PAYR = Payroll PORD = Purchase Orders SORD = Sales Orders |
| gl_user | character varying(12) |  |  | User initials that posted the transaction |
| gl_memo | character varying(60) |  |  | General Ledger memo created automatically by some subledger transactions or entered manually by the user |
| mf_who | character varying(10) |  |  | Transaction type |
| mf_key | character varying(20) |  |  | The code of the associated record involved in the transaction; Vendor code Customer code Employee code Inventory code |
| mf_tran | character varying(25) |  |  | Document reference or document number for transaction, Invoice or Purchase Order number. |
| reconcile_date | date |  |  | Reconcile date for Bank Account transactions |
| reconcile_flag | boolean | Yes |  | Reconcile flag for Bank Account transactions; FALSE = Not reconciled TRUE = Reconciled |
| post_date | date |  |  | The date the transaction was posted to General Ledger as per user's log on date. |
| debit_amt | numeric(15,2) |  |  | Debit amount posted to the General Ledger |
| credit_amt | numeric(15,2) |  |  | Credit amount posted to the General Ledger |
| source_dept | character varying(3) |  |  | Source division for multi divisional companies. Field only populated on the 000 division record showing the division the posting originated from. |
| rate_method | character varying(1) |  |  | Currency rate conversion method used; / = direct * = indirect |
| rate | numeric(13,7) |  |  | Currency conversion rate used |
| frn_debit_amt | numeric(15,2) |  |  | Debit amount posted to the General Ledger in foreign currency |
| frn_credit_amt | numeric(15,2) |  |  | Credit amount posted to the General Ledger in foreign currency |
| recurr_entry | boolean |  |  | Transaction was created by a recurring General Ledger transaction; False = normal transaction True = created by recurring proocess |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User initials that modified this record |
| job_no | character varying(10) |  |  | Job cost number - links to jobs.job_no |
| job_acct_no | character varying(10) |  |  | Job cost account number - links to jobs.account |

**Constraints:**
- `gl_transactions_pkey` (Primary key): (id)

### gl_history_accounts

Table for historical General Ledger Accounts as they were when the Year End Close was performed for the Fiscal Year ending as stated in year_end.

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| division | character varying(3) |  |  | Division code for Geneal Ledger account as per divisions in a company; 000 = non divisionalized company or consoldated division for multi division company - links to gl_history_divisions.code |
| account_no | character varying(24) |  |  | General Ledger account number |
| currency | character varying(3) |  |  | General Ledger account currency; blank = base currency - links to currencies.code |
| gl_group | character varying(3) |  |  | Group for General Ledger account - links to gl_history_groups.gl_group |
| subgroup | character varying(6) |  |  | Subgroup for General Ledger account - links to gl_history_subgroups.subgroup |
| name | character varying(60) |  |  | General Ledger account name |
| dr_cr_desig | character varying(1) |  |  | Normal balance for the account; C = Credit D = Debit |
| sales_acct | boolean |  |  | Sales account flag; FALSE = No TRUE = Yes |
| gifi_acct_no | character varying(4) |  |  | General Ledger account 4 digit GIFI code |
| opening_balance | numeric(15,2) |  |  | The opening balance for the beginnig of the year, of the GL account. Income and expense accounts have a zero open balance. |
| foreign_opening_balance | numeric(15,2) |  |  | Opening balance in foreign currency if this is a foreign account |
| next_cheque_no | bigint |  |  | Next cheque number, only used when this General Ledger account is a bank account |
| bank_acct | boolean |  |  | Bank account flag; FALSE = No TRUE = Yes |
| mc_revalue_acct | boolean |  |  | Multi-currency revaluation flag; FALSE = No TRUE = Yes |
| type | character varying(1) |  |  | Type of account; A = Asset L = Liability and Equity R = Revenue X = Expense |
| allocation_flag | boolean |  |  | General Ledger account allocation flag; FALSE = Is not an allocation account TRUE = Is an allocation account |
| inactive | boolean |  |  | General Ledger account inactive flag; FALSE = Account is active TRUE = Account is inactive and cannot be posted to |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User initials that modified this record |
| segments | character varying(24)[] |  |  | Account segments; segments [1] =Segment 1 segments [2] =Segment 2 segments [3] =Segment 3 segments [4] =Segment 4 |
| this_year | numeric(15,2)[] | Yes |  | Account change per period 1 - 13, this year; this_year [1] = this year period 1 net change this_year [2] = this year period 2 net change |
| this_year_budget | numeric(15,2)[] | Yes |  | Account budget per period 1 - 13, this year; this_year_budget [1] = this year budget period 1 this_year_budget [2] = this year budget period 2 |
| this_year_forecast | numeric(15,2)[] | Yes |  | Account forecast per period 1 - 13, this year; this_year_forecast [1] = this year forecast period 1 this_year_forecast [2] = this year forecast period 2 |
| foreign_this_year | numeric(15,2)[] | Yes |  | Foreign account change per period 1 - 13, next year; foreign_next_year [1] = next year period 1 net change foreign_next_year [2] = next year period 2 net change |
| foreign_this_year_budget | numeric(15,2)[] | Yes |  | Foreign account budget per period 1 - 13, this year; foreign_this_year_budget [1] = this year budget period 1 foreign_this_year_budget [2] = this year budget period 2 |
| foreign_this_year_forecast | numeric(15,2)[] | Yes |  | Foreign account forecast per period 1 - 13, this year; foreign_this_year_forecast [1] = this year forecast period 1 foreign_this_year_forecast [2] = this year forecast period 2 |
| year_end | date |  |  | Fiscal Year End for this history record |

**Constraints:**
- `gl_history_accounts_pkey` (Primary key): (id)

### gl_history_divisions

Table for General Ledger divisions as they were when the Year End Close was performed for the Fiscal Year ending as stated in year_end. Only one record will exist for non divisionalized companies. One extra record, '000', will exist for divisionalized companies for the consolidated division.

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| division | character varying(3) |  |  | Division code; 000 = non divisionlized company or consolidated division for multi division company |
| name | character varying(60) |  |  | Division name |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User initials that modified this record |
| year_end | date |  |  | Fiscal Year End for this history record |

**Constraints:**
- `gl_history_divisions_pkey` (Primary key): (id)

### gl_history_groups

Table for General Ledger Groups as they were when the Year End Close was performed for the Fiscal Year ending as stated in year_end.

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| gl_group | character varying(3) |  |  | Group code |
| name | character varying(60) |  |  | Group name as assigned by user |
| alias | character varying(60) |  |  | Group name to print on reports |
| acct_type | character varying(1) |  |  | Account type; A = Asset L = Liability and Equity R = Revenue X = Expense |
| total_heading | character varying(60) |  |  | Text to display beside Total on Reports |
| total | smallint |  |  | Total Flag; 0 = No total 1 = Total with reset 2 = Total with no reset |
| line_advance | character varying(1) |  |  | Line advance setting for reports; 0 = No additional lines 1 - 5 = Number of additional lines to advance P = Page advance |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User initials that modified this record |
| year_end | date |  |  | Fiscal Year End for this history record |

**Constraints:**
- `gl_history_groups_pkey` (Primary key): (id)

### gl_history_periods

Table for General Ledger Fiscal Periods as they were when the Year End Close was performed for the Fiscal Year ending as stated in year_end.

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| period | smallint |  |  | Period number 1 - 13 |
| end_date | date |  |  | Period end date |
| year_end | date |  |  | Fiscal Year End date for this period |
| start_date | date |  |  | Period start date |

**Constraints:**
- `gl_history_periods_pkey` (Primary key): (id)

### gl_history_segments

Table for General Ledger segments as they were when the Year End Close was performed for the Fiscal Year ending as stated in year_end.

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique indentifier |
| segment_no | character varying(1) |  |  | General Ledger segment number for position 1 - 4 |
| code | character varying(24) |  |  | Code for the segment to be used in the General Ledger Account |
| description | character varying(60) |  |  | Segment description or name |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified_by | character varying(3) |  |  | User initials that modified this record |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| year_end | date |  |  | Fiscal Year End for this history record |

**Constraints:**
- `gl_history_segments_pkey` (Primary key): (id)

### gl_history_subgroups

Table for General Ledger subgroups as they were when the Year End Close was performed for the Fiscal Year ending as stated in year_end.

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| subgroup | character varying(6) |  |  | General Ledger subgroup code |
| name | character varying(60) |  |  | Subgroup name or description |
| gl_group | character varying(3) |  |  | General Ledger group that this subgroup is a member of - links to gl_history_groups.gl_group |
| suppress_flag | boolean |  |  | Suppress flag to use for reporting; FALSE = Do not supporess TRUE = Suppress on reports |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User initials that modified this record |
| year_end | date |  |  | Fiscal Year End for this history record |

**Constraints:**
- `gl_history_subgroups_pkey` (Primary key): (id)

### gl_history_transactions

Table for General Ledger history transactions for the fiscal year ending as stated in year_end.

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| division | character varying(3) |  |  | Division code; 000 = non divisionlized company or consolidated division for multi division company Note: if the divsion &lt;&gt; "000" on this record then another duplicate of this record will exist where division = "000" to post the same transaction to the consolidated division - links to gl_history_divisions.division |
| account_no | character varying(24) |  |  | General Ledger account number - links to gl_history_accounts.account_no |
| currency | character varying(3) |  |  | General Ledger account currency; blank = base currency - links to currencies.code |
| date | date |  |  | Transaction date used to set fiscal period for transaction |
| trans_no | character varying(10) |  |  | General Ledger transaction number |
| recno | integer |  |  | Transaction record number, increments sequentially |
| where_from | character varying(4) |  |  | The module that the transaction originated from; AP = Accounts Payable AR = Accounts Receivable GL = General Ledger INVC = Inventory Count INVR = Inventory Receipts LWAY = Sales Deposits MFG = Manufacturing PAYR = Payroll PORD = Purchase Orders SORD = Sales Orders |
| gl_user | character varying(12) |  |  | User initials that posted the transaction |
| gl_memo | character varying(60) |  |  | General Ledger memo created automatically by some subledger transactions or entered manually by the user |
| mf_who | character varying(10) |  |  | Transaction type |
| mf_key | character varying(20) |  |  | The code of the associated record involved in the transaction; Vendor code Customer code Employee code Inventory code |
| mf_tran | character varying(25) |  |  | Document reference or document number for transaction, Invoice or Purchase Order number |
| reconcile_date | date |  |  | Reconcile date for Bank Account transactions |
| reconcile_flag | boolean |  |  | Reconcile flag for Bank Account transactions; FALSE = Not reconciled TRUE = Reconciled |
| post_date | date |  |  | The date the transaction was posted to General Ledger as per user's log on date |
| debit_amt | numeric(15,2) |  |  | Debit amount posted to the General Ledger |
| credit_amt | numeric(15,2) |  |  | Credit amount posted to the General Ledger |
| source_dept | character varying(3) |  |  | Source division for multi divisional companies. Field only populated on the 000 division record showing the division the posting originated from. |
| rate_method | character varying(1) |  |  | Currency rate conversion method used; / = direct * = indirect |
| rate | numeric(13,7) |  |  | Currency conversion rate used |
| frn_debit_amt | numeric(15,2) |  |  | Debit amount posted to the General Ledger in foreign currency |
| frn_credit_amt | numeric(15,2) |  |  | Credit amount posted to the General Ledger in foreign currency |
| recurr_entry | boolean |  |  | Transaction was created by a recurring General Ledger transaction; False = normal transaction True = created by recurring process |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User initials that modified this record |
| year_end | date |  |  | Fiscal Year End for this history record |
| job_no | character varying(10) |  |  | Job cost number - links to jobs.job_no |
| job_acct_no | character varying(10) |  |  | Job cost account number - links to jobs.account |

**Constraints:**
- `gl_history_transactions_pkey` (Primary key): (id)
