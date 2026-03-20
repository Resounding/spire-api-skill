# Database: Payroll & Employees

Schema: `public` (v3)

### employees

Table for Employees

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| employee_no | character varying(6) |  |  | Employee number |
| name | character varying(60) |  |  | Employee name |
| sin | character varying(9) |  |  | Employee Social Insurance number |
| birth_date | date |  |  | Date of birth |
| hire_date | date |  |  | Date hired |
| term_date | date |  |  | Date terminated |
| review_date | date |  |  | Review date |
| job_title | character varying(60) |  |  | Job title |
| health_number | character varying(30) |  |  | Health number |
| rsp_deduction_type | character varying(1) |  |  | RSP deduction type; $ = Amount % = Percentage |
| union_number | character varying(30) |  |  | Union ID number |
| language_code | character varying(1) |  |  | Language code; E = English F = French |
| union_dues_type | character varying(1) |  |  | Union dues rate type; $ = Amount % = Percentage |
| tax_credits | numeric(15,2) |  |  | Federal Tax credit amount |
| other_tax_credits | numeric(15,2) |  |  | Other claim amount |
| prov_claim_amt | numeric(15,2) |  |  | Provincial claim amount |
| remote_allow | numeric(15,2) |  |  | Remote allowance |
| alimony | numeric(15,2) |  |  | Alimony amount |
| elected | numeric(15,2) |  |  | Elected extra tax amount |
| expense_claim | numeric(15,2) |  |  | Estimated expense amount for commissioned employees |
| union_dues | numeric(15,2) |  |  | Union dues amount or rate |
| rsp_deduction | numeric(15,2) |  |  | RSP deduction amount or rate |
| estimated_pay | numeric(15,2) |  |  | Estimated pay amount for commissioned employees |
| tax_table | character varying(2) |  |  | The province code for the tax table to use |
| periods | smallint |  |  | The number of pay periods that employee is paid per year |
| last_pay_period | smallint |  |  | Not used |
| wcb_rate | numeric(5,3) |  |  | WCB (Work Safe) rate for this employee |
| wcb_assessable | numeric(15,2) |  |  | Annual WCB (Work Safe) assessable amount |
| ei_rate | numeric(5,3) |  |  | Employer Employment Insurance rate for this employee. Usually 1.4 |
| retain_vacation_pay | boolean |  |  | Retain employee's vacation; TRUE = Yes FALSE = No |
| vacation_pay_rate | numeric(5,2) |  |  | Vacation percentage rate this employee receives for vacation pay |
| vacation_owed | numeric(15,2) |  |  | Accrued vacation amount owed |
| bank | character varying(3) |  |  | Employee's bank institution number for EFT |
| branch | character varying(9) |  |  | Employee's bank account branch (transit) for EFT |
| account | character varying(31) |  |  | Employee's bank account number for EFT |
| dept_id | integer |  |  | Employees payroll department - links to payroll_depts.id |
| status | character varying(1) |  |  | Employees status; A = Active T = Terminated L = On leave |
| gender | character varying(1) |  |  | Gender; M = Male F = Female |
| ytd_salary_dlrs | numeric(15,2) |  |  | Year to date salary amount |
| ytd_advance_dlrs | numeric(15,2) |  |  | Year to date advances outstanding |
| ytd_comm_dlrs | numeric(15,2) |  |  | Year to date commission amount |
| ytd_regular_dlrs | numeric(15,2) |  |  | Year to date regular pay amount |
| ytd_otime_dlrs | numeric(15,2) |  |  | Year to date overtime pay amount |
| ytd_premium_dlrs | numeric(15,2) |  |  | Year to date premium pay amount |
| ytd_sick_dlrs | numeric(15,2) |  |  | Year to date sick pay amount |
| ytd_vac_dlrs | numeric(15,2) |  |  | Year to date vacation paid to the employee. All pay where type = "V" and/or non retained vacation pay. |
| ytd_other_dlrs | numeric(15,2) |  |  | Year to date other pay amount |
| ytd_ppip_insurable | numeric(15,2) |  |  | Year to date PIPP insurable amount |
| ytd_ppip | numeric(15,2) |  |  | Year to date PPIP amount |
| ytd_fed_tax | numeric(15,2) |  |  | Year to date federal tax withheld amount, includes provincial except Quebec |
| ytd_prv_tax | numeric(15,2) |  |  | Year to date provincial tax withheld amount if not included in federal tax |
| ytd_ei | numeric(15,2) |  |  | Year to date Employment Insurance withheld amount |
| ytd_cpp | numeric(15,2) |  |  | Year to date Canada Pension Plan or Quebec Pension Plan withheld amount |
| ytd_cpp2 | numeric(15,2) |  |  | Year to date second Canada Pension Plan or Quebec Pension Plan withheld amount |
| ytd_f5b | numeric(15,2) |  |  | Year to date F5B |
| ytd_union | numeric(15,2) |  |  | Year to date Union dues withheld amount |
| ytd_rsp | numeric(15,2) |  |  | Year to date RSP contributions amount |
| ytd_wcb | numeric(15,2) |  |  | Year to date WCB amount remitted |
| ytd_qhip | numeric(15,2) |  |  | Year to date QHIP remitted |
| ytd_insurable | numeric(15,2) |  |  | Year to date insurable amount |
| ytd_vac_paid | numeric(15,2) |  |  | Year to date vacation pay calculated for the employee. Accrued for employees with retain_vacation_pay = True and paid to employees with retain_vacation_pay = False. |
| dental_benefits | smallint |  |  | Employer-offered dental benefits |
| ytd_bonus | numeric(15,2) |  |  | Year to date bonus |
| ytd_statutory | numeric(15,2) |  |  | Year to date statutory pay |
| lst_salary_dlrs | numeric(15,2) |  |  | Last year salary pay amount |
| lst_advance_dlrs | numeric(15,2) |  |  | Last year advances outstanding |
| lst_comm_dlrs | numeric(15,2) |  |  | Last year commission pay amount |
| lst_regular_dlrs | numeric(15,2) |  |  | Last year regular pay amount |
| lst_otime_dlrs | numeric(15,2) |  |  | Last year overtime pay amount |
| lst_premium_dlrs | numeric(15,2) |  |  | Last year premium pay amount |
| lst_sick_dlrs | numeric(15,2) |  |  | Last year sick pay amount |
| lst_vac_dlrs | numeric(15,2) |  |  | Last year vacation pay amount |
| lst_other_dlrs | numeric(15,2) |  |  | Last year other pay amount |
| lst_ppip_insurable | numeric(15,2) |  |  | Last year PPIP insurable amount |
| lst_ppip | numeric(15,2) |  |  | Last year PPIP amount |
| lst_fed_tax | numeric(15,2) |  |  | Last years federal tax, includes provincial except Quebec |
| lst_prv_tax | numeric(15,2) |  |  | Year to date provincial tax withheld amount if not included in federal tax |
| lst_ei | numeric(15,2) |  |  | Last year Employment Insurance withheld amount |
| lst_cpp | numeric(15,2) |  |  | Last year Canada Pension Plan or Quebec Pension Plan withheld amount |
| lst_cpp2 | numeric(15,2) |  |  | Last year second Canada Pension Plan or Quebec Pension Plan withheld amount |
| lst_f5b | numeric(15,2) |  |  | Last year F5B |
| lst_union | numeric(15,2) |  |  | Last year Union dues withheld amount |
| lst_rsp | numeric(15,2) |  |  | Last year RSP contributions amount |
| lst_wcb | numeric(15,2) |  |  | Year to date WCB amount remitted |
| lst_qhip | numeric(15,2) |  |  | Last year QHIP remitted |
| lst_insurable | numeric(15,2) |  |  | Last year insurable amount |
| lst_vac_paid | numeric(15,2) |  |  | Last year vacation pay calculated for the employee. Accrued for employees with retain_vacation_pay = True and paid to employees with retain_vacation_pay = False. |
| lst_bonus | numeric(15,2) |  |  | Last year bonus |
| lst_statutory | numeric(15,2) |  |  | Last year statutory pay |
| ly_dental_benefits | smallint |  |  | Last year employer-offered dental benefits |
| ppip_exempt | boolean | Yes |  | Employee PPIP exempt flag; TRUE = Yes FALSE or blank = No |
| cpp_exempt | boolean | Yes |  | Employee CPP exempt flag; TRUE = Yes FALSE or blank = No |
| advance_bal | numeric(15,2) |  |  | Current balance of employee advances owing |
| direct_deposit | boolean | Yes |  | Employee is paid with direct deposit; TRUE = Yes FALSE = No |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User initials that modified this record |
| deduction_tax05_flag | boolean[] |  |  | Not used |
| deduction_tax06_flag | boolean[] |  |  | Not used |
| deduction_tax07_flag | boolean[] |  |  | Not used |
| deduction_tax08_flag | boolean[] |  |  | Not used |
| deduction_tax09_flag | boolean[] |  |  | Not used |
| deduction_tax10_flag | boolean[] |  |  | Not used |
| ytd_pensionable | numeric(15,2) |  |  | Year to date pensionable amount |
| lst_pensionable | numeric(15,2) |  |  | Last year pensionable amount |
| udf_data | jsonb |  |  | User Defined Fields data |
| ytd_taxable | numeric(15,2) |  |  | Year to date taxable amount |
| ytd_taxable_benefits | numeric(15,2) |  |  | Year to date taxable benefits amount |
| lst_taxable | numeric(15,2) |  |  | Last year taxable amount |
| lst_taxable_benefits | numeric(15,2) |  |  | Last year taxable benefit amount |
| tax_exempt | boolean | Yes |  | Employee tax exempt flag; False = No True = Yes |
| ei_exempt | boolean | Yes |  | Employee Employement Insurance exmpt flag ; False = No True = Yes |
| ytd_benefits | numeric(15,2) |  |  | Total year to date benefit amount |
| lst_benefits | numeric(15,2) |  |  | Total last year benefit amount |
| ytd_deductions | numeric(15,2) |  |  | Total year to date deduction amount |
| lst_deductions | numeric(15,2) |  |  | Total last year deduction amount |

**Constraints:**
- `employee_pkey` (Primary key): (id)
- `employees_dept_code_fkey` (Foreign key): (dept_id) REFERENCES public.payroll_depts (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION

### employee_roes

Table for Record of Employment reports

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| employee_id | integer | Yes |  | Employee identifier - links to employees.id |
| serial_no | character varying(9) |  |  | Record of Employement serial number from CRA. |
| reference_no | character varying(26) |  |  | Employee reference number |
| pay_period_type | character varying(1) |  |  | Pay period type as per CRA specs; B = bi-weekly E = semi-monthly non standard H = thirteen pay periods per year M = monthly O = monthly non-standard S = semi-monthly W = weekly |
| first_name | character varying(20) |  |  | Employee first name |
| initials | character varying(4) |  |  | Employee initials |
| last_name | character varying(28) |  |  | Employee last name |
| line1 | character varying(35) |  |  | Employee address line 1 |
| city | character varying(35) |  |  | Emplayee address city |
| province_country | character varying(35) |  |  | Employee address, province and country |
| postal_code | character varying(10) |  |  | Employee address postal code |
| first_day | date |  |  | Employee first day worked |
| last_day | date |  |  | Employee last day paid |
| final_pay_period | date |  |  | Final pay period end date |
| occupation | character varying(40) |  |  | Employee occupation |
| expected_recall | character varying(1) |  |  | Expect recall code; N = Not returning R = Returning U = Unknown |
| expected_recall_date | date |  |  | Expected recall date |
| insurable_hours | integer |  |  | Insurable hours |
| separation_code | character varying(3) |  |  | Separation code as per CRA spec |
| contact_first_name | character varying(20) |  |  | Employer contact first name |
| contact_last_name | character varying(28) |  |  | Employer contact last name |
| contact_area_code | character varying(3) |  |  | Employer contact phone area code |
| contact_phone | character varying(7) |  |  | Employer contact phone number |
| contact_extension | character varying(8) |  |  | Employer contact phone extension |
| language | character varying(1) |  |  | Preferred language; E = English F = French |
| vacation_pay_code | character varying(1) |  |  | Vacation pay code as per CRA spec; 1 = included with each pay 2 = paid because no longer working 3 = paid for vacation leave period 4 = anniversary |
| vacation_start_date | date |  |  | Vacation start date |
| vacation_end_date | date |  |  | Vacation end date |
| vacation_amount | numeric(8,2) |  |  | Vacation pay amount |
| comments | character varying(160) |  |  | Report comments |
| remitted | date |  |  | Report remitted date |
| _modified_by | character varying(3) |  |  | User initials that modified this record |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |

**Constraints:**
- `employee_roes_pkey` (Primary key): (id)
- `employee_roes_employee_id_fkey` (Foreign key): (employee_id) REFERENCES public.employees (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION

### employee_roe_details

Table for Record of Employment details

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes |  |
| employee_roes_id | integer | Yes |  | Link to employee_roes.id |
| sequence | integer | Yes |  | Sequence number for display order |
| type | character varying(1) |  |  | Pay type for this record as per CRA specs; I = Insurable earnings H = Statutory holiday pay O = Other monies S = Special payments |
| code | character varying(5) |  |  | ROE pay code as per CRA specs |
| start_date | date |  |  | Start date for pay record |
| end_date | date |  |  | End date for pay record |
| amount | numeric(8,2) |  |  | Amount for pay record |
| hours | numeric(6,2) |  |  | Number of hours for pay record |
| period | character varying(1) |  |  | Period code for record; "" = Not applicable D = Per Day W = Per Week |
| _modified_by | character varying(3) |  |  | User initials that modified this record |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |

**Constraints:**
- `employee_roe_details_pkey` (Primary key): (id)
- `employee_roe_details_employee_roes_id_fkey` (Foreign key): (employee_roes_id) REFERENCES public.employee_roes (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION

### employee_t4s

Table for Employee T4 records

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| employee_id | integer | Yes |  | Employee identifier - links to employees.id |
| year | integer | Yes |  | Year of the T4 |
| province | character varying(2) |  |  | Province |
| cpp_exempt | boolean | Yes |  | CPP Exempttion flag; FALSE = No TRUE = Yes |
| ei_exempt | boolean | Yes |  | Employment Insurance exemption flag; FALSE = No TRUE = Yes |
| ppip_exempt | boolean | Yes |  | Provincial Parental Insurance Plan exemption flag; FALSE = No TRUE = Yes |
| employment_code | integer |  |  | Employement code as per CRA |
| employment_income | numeric(15,2) |  |  | Adjusted Income |
| cpp_contributions | numeric(15,2) |  |  | Adjusted Canada Pension Plan contribution |
| qpp_contributions | numeric(15,2) |  |  | Adjusted Quebec Pension Plan contribution |
| cpp2_contributions | numeric(15,2) |  |  | Employee's second CPP contributions |
| qpp2_contributions | numeric(15,2) |  |  | Employees's second QPP contributions |
| ei_premiums | numeric(15,2) |  |  | Adjusted Employment Insurance premiums |
| rpp_contributions | numeric(15,2) |  |  | Adjusted Registered Pension Plan contribution |
| pension_adjustment | numeric(15,2) |  |  | Adjusted Pension adjustment |
| ppip_premiums | numeric(15,2) |  |  | Adjusted Provincial Parental Insurance Plan premiums |
| tax_deducted | numeric(15,2) |  |  | Adjusted Income Tax deducted |
| insurable_earnings | numeric(15,2) |  |  | Adjusted Insurable earnings |
| pensionable_earnings | numeric(15,2) |  |  | Adjusted Pensionable earnings |
| union_dues | numeric(15,2) |  |  | Adjusted Union dues |
| charitable_donations | numeric(15,2) |  |  | Adjusted charitable donations |
| rpp_registration | character varying(30) |  |  | Registered Pension Plan registration number |
| ppip_insurable_earnings | numeric(15,2) |  |  | Adjusted Provincial Parental Insurance Plan insurable earnings |
| dental_benefits | smallint |  |  | Employer-offered dental benefits |
| other_boxes | character varying[] |  |  | Other field descriptions 1 - 6 when set on the T4 |
| other_amounts | numeric(15,2)[] |  |  | Other Adjusted field 1 - 6 amouts when selected on the T4 |
| remitted | date |  |  | Remitted date |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| last_name | character varying(30) |  |  | Employee's last name as printed on T4 |
| first_name | character varying(30) |  |  | Employee's first name as printied on T4 |
| initials | character varying(5) |  |  | Employee's initials as printed on T4 |
| suggested_cpp_exempt | boolean |  |  | Canada Pension Plan exempt flag as copied from employee table |
| suggested_ei_exempt | boolean |  |  | Employement Insurance exempt flag as copied from employee table |
| suggested_ppip_exempt | boolean |  |  | Provincial Parental Insrance Plan exempt flag as copied from employee table |
| suggested_employment_income | numeric(15,2) |  |  | Calculated Income from timecards |
| suggested_cpp_contributions | numeric(15,2) |  |  | Calculated Canada Pension Plan contributions |
| suggested_qpp_contributions | numeric(15,2) |  |  | Calculated Quebec Pension Plan contributions |
| suggested_cpp2_contributions | numeric(15,2) |  |  | Calculated Employee second CPP contributions |
| suggested_qpp2_contributions | numeric(15,2) |  |  | Calculated Employee second QPP contributions |
| suggested_ei_premiums | numeric(15,2) |  |  | Calculated Employment Insurance premiums |
| suggested_rpp_contributions | numeric(15,2) |  |  | Calculated Registered Pension Plan contributions |
| suggested_pension_adjustment | numeric(15,2) |  |  | Calculated pension adjustment |
| suggested_ppip_premiums | numeric(15,2) |  |  | Calculated Provincial Parential Insurance Plan premiums |
| suggested_tax_deducted | numeric(15,2) |  |  | Calculated Income tax deducted |
| suggested_insurable_earnings | numeric(15,2) |  |  | Calculated insurable earnings |
| suggested_pensionable_earnings | numeric(15,2) |  |  | Calculated pensionable earnings |
| suggested_union_dues | numeric(15,2) |  |  | Calculated union dues |
| suggested_charitable_donations | numeric(15,2) |  |  | Calculated charitabe donations, not currently used |
| suggested_ppip_insurable_earnings | numeric(15,2) |  |  | Calculated Provincial Parental Insurance Plan insurable earnings |
| suggested_other_amounts | numeric(15,2)[] |  |  | Not used |
| status | character varying(1) |  |  | Staus of T4; A = Amended C = Cancelled O = Original |

**Constraints:**
- `employee_t4s_pkey` (Primary key): (id)
- `employee_t4s_employee_id_fkey` (Foreign key): (employee_id) REFERENCES public.employees (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION

### payroll_depts

Table for Payroll Departments

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| gl_division | character varying(3) | Yes |  | General Ledger Division for this payroll department |
| wages | character varying(24) |  |  | General Ledger account to use for Wages expense |
| ei_expense | character varying(24) |  |  | General Ledger account to use for Empoyment Insurance Expense |
| cpp_expense | character varying(24) |  |  | General Ledger account to use for CPP expense |
| wcb_expense | character varying(24) |  |  | General Ledger account to use for WCB expense |
| advances_receivable | character varying(24) |  |  | General Ledger account to use for Advances paid to employees |
| ei_payable | character varying(24) |  |  | General Ledger account to use for Employement Insurance payable |
| cpp_payable | character varying(24) |  |  | General Ledger account to use for CPP payable |
| wcb_payable | character varying(24) |  |  | General Ledger account to use for WCB payable |
| income_tax_payable | character varying(24) |  |  | General Ledger account to use for Income Tax payable |
| pension_payable | character varying(24) |  |  | General Ledger account to use for Pension payable |
| union_dues_payable | character varying(24) |  |  | General Ledger account to use for Union dues payable |
| vacation_payable | character varying(24) |  |  | General Ledger account to use for Vacation payable |
| clearing | character varying(24) |  |  | General Ledger account to use for Payroll clearing or bank account |
| qpp_expense | character varying(24) |  |  | General Ledger account to use for QPP expense |
| csst_expense | character varying(24) |  |  | General Ledger account to use for CSST expense |
| qhip_expense | character varying(24) |  |  | General Ledger account to use for QHIP expense |
| qpp_payable | character varying(24) |  |  | General Ledger account to use for QPP payable |
| csst_payable | character varying(24) |  |  | General Ledger account to use for CSST payable |
| qhip_payable | character varying(24) |  |  | General Ledger account to use for QHIP payable |
| qc_tax_payable | character varying(24) |  |  | General Ledger account to use for QC tax payable |
| ppip_expense | character varying(24) |  |  | General Ledger account to use for PPIP expense |
| ppip_payable | character varying(24) |  |  | General Ledger account to use for PPIP payable |
| code | character varying(10) |  |  | User set code for this department |
| description | character varying(30) |  |  | User set description for this department |
| vacation_expense | character varying(24) |  |  | General Ledger account to use for Vacation pay expense |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |

**Constraints:**
- `payroll_dept_pkey` (Primary key): (id)

### payroll_dept_additions

Table for Payroll Departments containg default settings for benefits and deductions

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| dept_id | integer | Yes |  | Payroll department - links to payroll_depts.id |
| sequence | smallint | Yes |  | Sequence number |
| name | character varying(60) |  |  | Name or description |
| account | character varying(24) |  |  | GL Account to use |
| type | character varying(1) | Yes |  | Record type; B = Benefit D = Deduction |
| tax | boolean | Yes |  | Tax flag; False = Not taxable True = Taxable |
| ei | boolean | Yes |  | Employment Insurance flag; False = no EI calculated True = calculate EI |
| cpp | boolean | Yes |  | CPP/QPP flag; False = no pension calculated True = calculate pension |
| wcb | boolean | Yes |  | WCB flag; False = no Worker's Compensation calculated True = calculate Worker's Compensation |
| amount_type | character varying(1) | Yes |  | calculation type; $ = Amount % = Percentage |
| amount | numeric(15,2) | Yes |  | Amount of Benefit / Deduction |
| _dbversion | integer |  |  | Program version that last modified this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |

**Constraints:**
- `payroll_dept_additions_pkey` (Primary key): (id)

### payroll_employee_additions

Table for Employee settings for benefits and deductions

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| employee_id | integer | Yes |  | Employee's id - links to employees.id |
| dept_id | integer | Yes |  | Employees payroll department - links to payroll_depts.id |
| dept_addition_id | integer | Yes |  | Payroll department addition - links to payroll_dept_additions.id |
| type | character varying(1) | Yes |  | Record type; B = Benefit D = Deduction |
| tax | boolean | Yes |  | Tax flag; False = Not taxable True = Taxable |
| ei | boolean | Yes |  | Employment Insurance flag; False = no EI calculated True = calculate EI |
| cpp | boolean | Yes |  | CPP/QPP flag; False = no pension calculated True = calculate pension |
| wcb | boolean | Yes |  | WCB flag; False = no Worker's Compensation calculated True = calculate Worker's Compensation |
| amount_type | character varying(1) | Yes |  | calculation type; $ = Amount % = Percentage |
| amount | numeric(15,2) | Yes |  | Amount of Benefit / Deduction |
| use_defaults | boolean | Yes |  | Use default settings from payroll_dept_additions; False = Use Employee settings from this record True = Use settings from payroll_dept_additions |
| ytd | numeric(15,2) | Yes |  | Year to date amount for this Benefit / Deduction |
| last_year | numeric(15,2) | Yes |  | Last year total amount for this Benefit / Deduction |
| _dbversion | integer |  |  | Program version that last modified this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |

**Constraints:**
- `payroll_employee_additions_pkey` (Primary key): (id)
- `payroll_employee_additions_dept_addition_id_fkey` (Foreign key): (dept_addition_id) REFERENCES public.payroll_dept_additions (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE CASCADE

### payroll_schedules

Table for Payroll Schedules for each pay frequency

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| frequency | smallint | Yes |  | Number of pay periods per year (normally) |
| year | smallint | Yes |  | Payroll year |
| week_day | smallint |  |  | ISO week date (Monday = 1, Sunday = 7) |
| dates | date[] | Yes |  | End dates for all the pay periods using this frequency |
| descriptions | character varying[] | Yes |  | Descriptions for all the pay periods for this frequency |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |

**Constraints:**
- `payroll_schedules_pkey` (Primary key): (id)

### payroll_timecards

Table for Payroll timecards

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| year | smallint |  |  | Payroll year for timecard |
| week_no | smallint |  |  | Payroll week number |
| sub_week | smallint |  |  | Additional timecard number for the week; 0 = Original timecard 1 = First additional timecard 2 = Second additonal timecard |
| irregular | boolean | Yes |  | Irregular flag, indicates timecard earnings will be treated as bonus, retroactive pay increase, or other non-periodic payment |
| date | date |  |  | Pay date |
| cheque_no | character varying(25) |  |  | Cheque number assigned to timecard when printed |
| trans_no | character varying(10) |  |  | General Ledger transaction number - links to gl_transaction.trans_no |
| fed_tax | numeric(15,2) |  |  | Federal tax amount, includes provincial tax except in Quebec |
| prv_tax | numeric(15,2) |  |  | Quebec Provincial tax amount |
| ei | numeric(15,2) |  |  | Employment Insurance amount |
| cpp | numeric(15,2) |  |  | Canada (or Quebec) Pension Plan amount |
| cpp2 | numeric(15,2) |  |  | Second Canada (or Quebec) Pension Plan amount |
| f5b | numeric(15,2) |  |  | Deductions for CPP additional contributions deducted from the non-periodic payment |
| union_dues | numeric(15,2) |  |  | Union dues amount |
| rsp | numeric(15,2) |  |  | Registered Savings Plan amount |
| wcb_earnings | numeric(15,2) |  |  | Workers Compensation Board or Provincial Work Safe eligible earnings |
| wcb | numeric(15,2) |  |  | Workers Compensation Board or Provincial Work Safe amount |
| wcb_rate | numeric(5,3) |  |  | WCB rate used when calculating this timecard |
| qhip | numeric(15,2) |  |  | Quebec Health Insurance Plan amount |
| insurable_dlrs | numeric(15,2) |  |  | Insurable amount |
| vac_paid | numeric(15,2) |  |  | Vacation paid amount |
| vac_amount | numeric(15,2) |  |  | Vacation amount |
| ei_rate | numeric(5,3) |  |  | Employement Insurance rate |
| ei_exempt | boolean | Yes |  | Employee EI exemption |
| pensionable | numeric(15,2) |  |  | Pensionable amount |
| taxable_amt | numeric(15,2) |  |  | Taxable amount |
| ppip_insurable | numeric(15,2) |  |  | Provincial Parental Insurance Plan insurable amount |
| ppip | numeric(15,2) |  |  | Provincial Parental Insurance Plan amount |
| ppip_employer_rate | numeric(15,5) |  |  | Provincial Parental Insurance Plan employer rate |
| ppip_rate | numeric(15,5) |  |  | Provincial Parental Insurance Plan rate |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| udf_data | jsonb |  |  | User defined data for this record |
| dept_id | integer |  |  | Payroll department used by this record - links to payroll_depts.id |
| employee_id | integer |  |  | Employee on this record - links to employees.id |
| retain_vacation_pay | boolean |  |  | Vaction pay was retained on this timecard; False = Vacation pay was paid on the record True = Vacation pay was accrued on the timecard |
| reversal | boolean | Yes |  | Reversal flag; False = This is an original timecard True = This timecard is a reversal or has been reversed |
| net_pay | numeric(15,2) | Yes |  | Net pay |
| total_deductions | numeric(15,2) |  |  | Total deductions |
| total_benefits | numeric(15,2) |  |  | Total benefits |
| days_since_last_pay | smallint |  |  | Number of days since last payment for commission remunerated employee |

**Constraints:**
- `timecard_pkey` (Primary key): (id)
- `payroll_timecards_dept_id_fkey` (Foreign key): (dept_id) REFERENCES public.payroll_depts (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION
- `payroll_timecards_employee_id_fkey` (Foreign key): (employee_id) REFERENCES public.employees (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION

### payroll_timecard_entries

Table of Timecard pay records

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| timecard_id | integer |  |  | Link to timecards.id |
| sequence | smallint |  |  | Sequential number for timecard line |
| entry_code_id | integer | Yes |  | Link to timecard entry code |
| hours | numeric(5,2) |  |  | Number of hours on this record |
| rate | numeric(9,2) |  |  | Pay rate on this record |
| amount | numeric(15,2) |  |  | Pay amount on this record |
| account | character varying(24) |  |  | General Ledger Account for this record |
| _dbversion | integer |  |  | Program version that last modified this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| job_no | character varying(10) |  |  | Job cost number - links to jobs.job_no |
| job_acct_no | character varying(10) |  |  | Job cost account number - links to jobs.account |
| comment | text |  |  | Comment text |
| udf_data | hstore |  |  | User defined data for this record |

**Constraints:**
- `timecard_entries_pkey` (Primary key): (id)
- `payroll_timecard_entries_timecard_id_fkey` (Foreign key): (timecard_id) REFERENCES public.payroll_timecards (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE CASCADE
- `payroll_timecard_entry_codes_entry_code_id_fkey` (Foreign key): (entry_code_id) REFERENCES public.payroll_timecard_entry_codes (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION

### payroll_timecard_entry_codes

Table of Timecard Entry Codes

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| code | character varying(20) |  |  | Pay code |
| description | character varying(80) |  |  | Entry code description |
| pay_type | character varying(1) |  |  | Special payment type; $ = Salary A = Advance C = Commission R = Regular Pay O = Overtime P = Premium S = Sick X = Other V = Vacation B = Bonus |
| tax | boolean | Yes |  | Taxable |
| cpp | boolean | Yes |  | Pensionable |
| ei | boolean | Yes |  | Insurable |
| wcb | boolean | Yes |  | WCB applies |
| vacation | boolean | Yes |  | Vacation applies |
| hours | numeric(5,2) |  |  | Number of hours on this record |
| rate | numeric(9,2) |  |  | Pay rate on this record |
| amount | numeric(15,2) |  |  | Pay amount on this record |
| account | character varying(24) |  |  | Optional General Ledger Account for this record |
| _dbversion | integer |  |  | Program version that last modified this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |

**Constraints:**
- `payroll_timecard_entry_codes_pkey` (Primary key): (id)

### payroll_timecard_additions

Table for for benefits and deductions recorded on the timecard

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| timecard_id | integer | Yes |  | Timecard for these records - links to timecards.id |
| employee_addition_id | integer |  |  | Employee source this record - links to employee_additons.id |
| name | character varying(60) |  |  | Name or description |
| account | character varying(24) |  |  | GL Account posted to |
| type | character varying(1) | Yes |  | Record type; B = Benefit D = Deduction |
| tax | boolean | Yes |  | Tax flag; False = Not taxable True = Taxable |
| ei | boolean | Yes |  | Employment Insurance flag; False = no EI calculated True = calculate EI |
| cpp | boolean | Yes |  | CPP/QPP flag; False = no pension calculated True = calculate pension |
| wcb | boolean | Yes |  | WCB flag; False = no Worker's Compensation calculated True = calculate Worker's Compensation |
| user_flag | boolean | Yes |  | User override flag; False = Employee or Payroll department calculations used True = Employee did an override on the amounts |
| total | numeric(15,2) | Yes |  | Amount of Benefit / Deduction calculated |
| _dbversion | integer |  |  | Program version that last modified this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |

**Constraints:**
- `payroll_timecard_additions_pkey` (Primary key): (id)
- `payroll_timecard_additions_employee_addition_id_fkey` (Foreign key): (employee_addition_id) REFERENCES public.payroll_employee_additions (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE SET NULL
- `payroll_timecard_additions_timecard_id_fkey` (Foreign key): (timecard_id) REFERENCES public.payroll_timecards (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE CASCADE

### payroll_pd7as

Table of Payroll Source Deduction Remittances

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| vendor_no | character varying(20) |  |  | CRA Payroll AP Account (Vendor) |
| remitted | date |  |  | Date remitted |
| trans_no | character varying(10) |  |  | Posted remittance GL Transaction No |
| due_date | date |  |  | Date remittance is due to CRA |
| employees_paid_on | date |  |  | Remittance period ending date |
| employees_last_period | integer |  |  | Number of employees on payroll in the last period |
| gross_period_payroll | numeric(15,2) |  |  | Gross pay including benefits for remittance period |
| payment_date | date |  |  | Date payment remitted to CRA |
| payment_amount | numeric(15,2) |  |  | Amount to remit to CRA |
| tax | numeric(15,2) |  |  | Total tax deducted at source |
| ei | numeric(15,2) |  |  | Total EI deducted at source |
| employer_ei | numeric(15,2) |  |  | Total employer EI for remittance period |
| cpp | numeric(15,2) |  |  | Total CPP deducted at source |
| employer_cpp | numeric(15,2) |  |  | Total employer CPP for remittance period |
| temp_wage_subsidy | numeric(15,2) |  |  | Total Temporary Wage Subsidy claimed (deducted from payment amount) |
| _dbversion | integer |  |  | Program version that last modified this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |

**Constraints:**
- `payroll_pd7as_pkey` (Primary key): (id)

### payroll_pd7a_details

Table of Payroll Source Deduction Remittance Details

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| employee_id | integer | Yes |  | Employee ID |
| gross_pay | numeric(15,2) |  |  | Gross pay including benefits during remittance period for this employee |
| tax | numeric(15,2) |  |  | Tax deducted at source |
| ei | numeric(15,2) |  |  | EI deducted at source |
| employer_ei | numeric(15,2) |  |  | Employer EI during remittance period for this employee |
| cpp | numeric(15,2) |  |  | CPP deducted at source |
| employer_cpp | numeric(15,2) |  |  | Employer CPP during remittance period for this employee |
| temp_wage_subsidy | numeric(15,2) |  |  | Temporary Wage Subsidy claimed for this employee |
| _dbversion | integer |  |  | Program version that last modified this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |

**Constraints:**
- `payroll_pd7a_details_pkey` (Primary key): (id)
- `payroll_pd7a_details_employee_id_fkey` (Foreign key): (employee_id) REFERENCES public.employees (id) ON DELETE CASCADE
