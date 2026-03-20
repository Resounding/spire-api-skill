# Database: Addresses & Contacts

Schema: `public` (v3)

### addresses

Table for all Address records

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| link_table | character varying(4) |  |  | Field to use for module linking; COMP = Company CUST = Customer DIV = Division EMP = Employee JC = Job Cost PHIS = Purchase History PORD = Purchase Order SLSP = Salesperson SHIS = Sales History SORD = Sales Order VEND = Vendor WHSE = Warehouse |
| link_no | character varying(20) |  |  | Field to store join value for link to; customers.cust_no, gl_divisions.division, employees.employee_no, jobs.job_no, purchase_history.purchase_no , purchase_orders.purchase_no, sales_history.invoice_no, sales_orders_order_no, vendors.vendor_no, inventory_warehouses.whse |
| source_address_id | integer |  |  | Address ID this record was sourced from |
| addr_type | character varying(1) |  |  | Address type flag; B = Billing address S = Ship To address |
| ship_id | character varying(20) | Yes |  | ShipTo ID from; Customer, Sales Order, Sales History, Purchase History, Purchase Order, Vendor |
| name | character varying(60) |  |  | Name for; Company, Customer, Division, Employee, Job Cost, Purchase History, Purchase Order, Sales History, Sales Order, Vendor, Warehouse |
| city | character varying(45) |  |  | City for this record |
| prov_state | character varying(3) |  |  | Province/State for this record |
| postal_zip | character varying(16) |  |  | Postal Code / Zip Code for this record |
| country_code | character varying(3) |  |  | Country for this record |
| phone_type | smallint |  |  | deprecated |
| phone | character varying(30) |  |  | deprecated - See address_contacts.phone for contact_type = 0 (main) |
| fax_type | smallint |  |  | deprecated |
| fax | character varying(30) |  |  | deprecated - See address_contacts.fax for contact_type = 0 (main) |
| email | character varying(254) |  |  | deprecated - See address_contacts.email for contact_type = 0 (main) |
| webpage | character varying(80) |  |  | Web page address for this record |
| hold | boolean |  |  | Hold flag; FALSE - Not On Hold TRUE - On Hold |
| sales_terr | character varying(10) |  |  | Territory code for this record - links to territories.code |
| sales_terr_desc | character varying(80) |  |  | Territory Name for this record |
| sales_person | character varying(10) |  |  | Salesperson code for this record - links to salespeople.salesperson_no |
| slspsn_name | character varying(60) |  |  | Salesperson Name for this record |
| ship_code | character varying(10) |  |  | Ship code for this record |
| ship_desc | character varying(60) |  |  | Ship description |
| dflt_whse | character varying(6) |  |  | Default warehouse code for customer |
| rv_account | character varying(24) |  |  | Revenue General Ledger account for Customer Expense General Ledger account for Vendor |
| sell_no | smallint |  |  | Customers sell level 1 - 20 |
| ecommerce | boolean |  |  | Used for ecommerce integration |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| address | character varying(45)[] |  |  | Street Address for this record - address[1] = address line 1, address[2] = address line 2, address[3] = address line 3, address[4] = address line 4 |
| contact_name | character varying(60)[] | Yes |  | deprecated |
| contact_phone_type | smallint[] | Yes |  | deprecated |
| contact_phone | character varying(30)[] | Yes |  | deprecated |
| contact_fax_type | smallint[] | Yes |  | deprecated |
| contact_fax | character varying(30)[] | Yes |  | deprecated |
| contact_email | character varying(254)[] | Yes |  | deprecated |
| sales_tax_no | smallint[] | Yes |  | Sales tax codes charged to be used witht his address - links to sales_taxes.id |
| sales_tax_exempt_no | character varying(20)[] | Yes |  | Sales tax exemption numbers 1 - 4 for this record |
| udf_data | jsonb |  |  | User defined data for address record |

**Constraints:**
- `address_pkey` (Primary key): (id)

### address_contacts

Address contacts

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| address_id | integer | Yes |  | addresses.id relation |
| source_contact_id | integer | Yes |  | address_contacts.id relation |
| contact_type_id | integer |  |  | address_contact_types.id relation |
| name | character varying(80) | Yes |  | Name of contact type |
| phone | character varying(30) | Yes |  | Phone number in e164 format |
| cell | character varying(30) | Yes |  | Mobile number in e164 format |
| fax | character varying(30) | Yes |  | Fax/other number in e164 format |
| email | character varying(254) | Yes |  | Contact email address |

### address_contact_types

Contact types for address contacts

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| name | character varying(80) | Yes |  | Name of contact type |
