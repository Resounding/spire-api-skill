# Database: Sales Orders

Schema: `public` (v3)

### sales_orders

Table for Sales Orders not yet posted

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| order_no | character varying(10) |  |  | Sales Order number assigned from Company Settings, Sequence numbers or manually entered by the user |
| cust_no | character varying(20) |  |  | Customer number - links to customers.cust_no |
| cust_name | character varying(60) |  |  | Customer name |
| status | character varying(1) |  |  | Sales Order status; C - Closed (used by Batch Invoicing) L - Deposit O - Open P - Processed S - Shipped |
| order_date | date |  |  | Sales Order date |
| division | character varying(3) |  |  | Division assigned to Sales Order |
| location | character varying(24) | Yes |  | General Ledger Location segment for companies using Location accounting |
| profit_center | character varying(24) |  |  | General Ledger Profit Center segment for companies using Profit Center accounting |
| territory_code | character varying(10) |  |  | Territory code assigned to this Sales Order - links to territories.code |
| salesperson_no | character varying(10) |  |  | Salesperson code assigned to this Sales Order - links to salespeople.salesperson_no |
| cust_po_no | character varying(20) |  |  | Customer Purchase Order number |
| ship_code | character varying(10) |  |  | Ship via code - links to shipping_methods.code |
| required_date | date |  |  | Required date for Sales Order |
| ship_carrier | character varying(20) |  |  | Ship carrier on the Sales Order |
| ship_track_id | character varying(50) |  |  | Carrier tracking number from the Sales Order |
| terms_code | character varying(10) |  |  | Payment terms code on the Sales Order - links to payment_terms.terms_code |
| terms_description | character varying(60) |  |  | Payment terms description |
| terms_days_before_due | smallint |  |  | Number of days from then Invoice date that the payment is due |
| terms_days_allowed | smallint |  |  | Number of days from the Invoice date for which a discount is allowed |
| terms_discount_rate | numeric(5,2) | Yes |  | Rate of the discount |
| hold | boolean |  |  | Sales Order On Hold flag; FALSE - Not Held TRUE= Held |
| fob | character varying(20) |  |  | Free on board location |
| incoterms | character varying(3) |  |  | Incoterms code |
| incoterms_place | character varying |  |  | Incoterms place |
| ref_no | character varying(20) |  |  | Reference number added by the user |
| invoice_no | character varying(10) |  |  | The Invoice number to be assigned to this Sales Order |
| invoice_date | date |  |  | Invoice date, also used for transaction date |
| inv_date_rrule | character varying(120) |  |  | The recurring rules for this Standing Order |
| batch_no | bigint |  |  | The batch number for this Sales Order, only used with Batch Invoicing |
| was_quote_no | character varying(10) |  |  | Original Quote number before being converted to a Sales Order |
| quote_expires | date |  |  | Quote expiry date |
| ship_id | character varying(20) | Yes |  | Customers Shipto ID selected for this Sales Order - links to addresses.ship_id |
| currency | character varying(3) |  |  | Customer currency; blank = base currency - links to currencies.code |
| currency_rate_method | character varying(1) |  |  | Currency rate conversion method used; / = direct * = indirect |
| currency_rate | numeric(13,7) | Yes |  | Currency rate used |
| discount | numeric(5,2) | Yes |  | Discount percentage on the entire Sales Order |
| subtotal | numeric(15,2) | Yes |  | Subtotal before taxes, discount and freight |
| total_discount | numeric(15,2) | Yes |  | Sales Order discount amount given on total, does not include line discounts |
| freight | numeric(15,2) | Yes |  | Total freight on the Sales Order. **Note:** a value written here only persists if `user_freight` is also set to `TRUE`; otherwise Spire will recalculate/overwrite it. |
| sales_tax | numeric(15,2) | Yes |  | Total Sales tax on this Sales Order |
| gross_profit | numeric(15,2) | Yes |  | Gross profit for this Sales Order |
| total_current_cost | numeric(15,2) | Yes |  | Total current cost of items on this Sales Order |
| total_average_cost | numeric(15,2) | Yes |  | Total average cost of items on this Sales Order |
| total | numeric(15,2) | Yes |  | Sales Order total |
| total_payments | numeric(15,2) | Yes |  | Total payments made on this Sales Order including deposits |
| backordered | boolean |  |  | Sales Order has back orders; False = no backorders True = has 1 or more backorders |
| total_surcharge | numeric(15,2) | Yes |  | Total surcharges on Sales Order |
| user_surcharge | boolean |  |  | Flag that indicates if the user edited the total surcharge amount; FALSE - Surcharge was not edited and will continue to be automatically calculated TRUE - Surcharge was edited and will no longer be automatically calculated. |
| pack_date | date |  |  | Date Ship was last clicked on the Sales Order |
| pack_init | character varying(3) |  |  | Users initials that last clicked Ship onthe Sales Order |
| processed_user | character varying(3) |  |  | Initials of user that clicked Process on the Sales Order |
| invoiced_user | character varying(3) |  |  | Initials of user that clicked Invoice on the Sales Order, used when set to batch invoicing |
| subtotal_ordered | numeric(15,2) | Yes |  | Subtotal before taxes, discount and freight, based on order quantity |
| total_discount_ordered | numeric(15,2) | Yes |  | Sales Order discount amount given on total, does not include line discounts, based on order quantity |
| freight_ordered | numeric(15,2) | Yes |  | Total freight amount based on order quantity |
| sales_tax_ordered | numeric(15,2) | Yes |  | Total sales taxes based on order quantity |
| gross_profit_ordered | numeric(15,2) | Yes |  | Gross profit based on order quantity |
| current_cost_ordered | numeric(15,2) | Yes |  | Total current cost based on order quantity |
| average_cost_ordered | numeric(15,2) | Yes |  | Total average cost based on order quantity |
| total_ordered | numeric(15,2) | Yes |  | Sales Order total based on order quantity |
| backorder_created | boolean |  |  | A new Sales Order for the backorders has been created; FALSE - No TRUE - Yes |
| cr_approve_amt | numeric(15,2) |  |  | Credit approved amount on this Sales Order |
| cr_approve_date | date |  |  | The last date credit amount was approved |
| cr_approve_user | character varying(3) |  |  | The initials of the user that approved the credit amount on this Sales Order |
| job_no | character varying(10) |  |  | Job cost number - links to jobs.job_no |
| job_acct_no | character varying(10) |  |  | Job cost account number - links to jobs.account |
| phase_id | character varying(20) |  |  | The current Phase ID of this Sales Order - links to phases.phase_id |
| contact_name | character varying(60) |  |  | deprecated |
| contact_phone | character varying(30) |  |  | deprecated |
| contact_fax | character varying(30) |  |  | deprecated |
| last_inv_no | character varying(10) |  |  | Not used |
| last_inv_date | date |  |  | Not used |
| contact_phone_type | smallint |  |  | deprecated |
| contact_fax_type | smallint |  |  | deprecated |
| contact_email | character varying(254) |  |  | deprecated |
| bill_contact_id | integer |  |  |  |
| ship_contact_id | integer |  |  |  |
| total_weight | numeric(15,2) |  |  | Total weight of the items on the Sales Order |
| order_type | character varying(1) |  |  | The Sales Order type; B - Booking Order O - Sales Order Q - Quote R - RMA S - Standing Order W - Work Order |
| ship_date | date |  |  | Date Ship was last clicked on the Sales Order |
| standard_cost_ordered | numeric(15,5) | Yes |  | Total standard cost based on order quantity |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| sales_tax_rate | numeric(7,4)[] | Yes |  | Tax 1 - 4 rates used on the Sales Order |
| sales_tax_applicable | numeric(15,2)[] | Yes |  | Tax 1 - 4 applicable amounts on the Sales Order |
| sales_tax_total | numeric(15,2)[] | Yes |  | Tax 1 - 4 totals for each tax on the Sales Order |
| sales_tax_applicable_ordered | numeric(15,2)[] | Yes |  | Tax 1 - 4 applicable amounts on the Sales Order, based on order quantity |
| sales_tax_total_ordered | numeric(15,2)[] | Yes |  | Tax 1 - 4 totals for each tax on the Sales Order, based on order quantity |
| processed_date | timestamp without time zone |  |  | Date Process was last clicked on the Sales Order |
| invoiced_date | timestamp without time zone |  |  | Date that Invoice was clicked on the Sales Order, used when set to batch invoicing |
| user_freight | boolean | Yes |  | Flag to indicate that the user did a freight override; FALSE = No TRUE = Yes |
| udf_data | hstore |  |  | User defined data for this record |
| partial_tax_rate | numeric(7,4) |  |  | Partial tax rate used on the Sales Order |
| partial_tax_total | numeric(15,2) |  |  | Partial tax total on the Sales Order |
| _deleted | timestamp without time zone |  |  | UTC Date and time this record was deleted by user, blank if not deleted |
| _deleted_by | character varying(3) |  |  | User initials that deleted this Sales Order, blank if not deleted |
| total_standard_cost | numeric(15,2) | Yes |  | Total standard cost of items on this Sales Order |
| total_surcharge_ordered | numeric(15,2) | Yes |  | Total surcharges on Sales Order based on order quantity |
| whse | character varying(6) |  |  | Default ship from warehouse |

**Constraints:**
- `bve_order_pkey` (Primary key): (id)

### sales_order_items

Table for Sales Orders lines not yet posted

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| order_no | character varying(10) |  |  | Sales Order number for the line - links to sales_orders.order_no |
| sequence | smallint |  |  | Sequential number for order line |
| parent_item | smallint |  |  | Link to sales_order_items.sequence for the parent of this group, used by kits, accessories and job items |
| whse | character varying(6) |  |  | Inventory warehouse for this record - links to inventory.whse |
| part_no | character varying(34) |  |  | Inventory part number for this record - links to inventory.part_no |
| description | character varying(80) |  |  | Description of the Inventory item or first 80 characters of the comment |
| product_code | character varying(10) |  |  | Product Code of the item - links to inventory_product_codes.product_code |
| uom_inventory | character varying(10) |  |  | Unit of measure used by this item for inventory committed quantity - links to inventory_uoms |
| uom_sales | character varying(10) |  |  | Sales unit of measure - links to inventory_uoms |
| uom_sales_factor | numeric(11,5) |  |  | Unit of Measure conversion factor to use for this record, 0 or 1 = no conversion |
| sell_direct_factor | boolean |  |  | Use UOM direct factor for conversion; TRUE= multiply FALSE = divide |
| order_qty | numeric(15,5) | Yes |  | Quantity that was ordered |
| committed_qty | numeric(15,5) | Yes |  | Quantity that was shipped |
| backorder_qty | numeric(15,5) | Yes |  | Quantity that was backordered |
| inventory_order_qty | numeric(15,5) | Yes |  | Quantity ordered converted to Inventory unit of measure |
| inventory_committed_qty | numeric(15,5) | Yes |  | Quantity committed converted to Inventory unit of measure |
| inventory_backorder_qty | numeric(15,5) | Yes |  | Quantity backordered converted to Inventory unit of measure |
| retail_price | numeric(15,5) | Yes |  | Sell price on this line before discount, user can override |
| status | smallint |  |  | Price method used; 0 = Price from Inventory 2 = Price from Price Matrix 9 = User override price 10 = Copied price from previous Invoice |
| unit_price | numeric(15,5) | Yes |  | Sell price after line discount, user can override |
| current_cost | numeric(15,5) | Yes |  | Current cost of the item |
| average_cost | numeric(15,5) | Yes |  | Average cost of the item |
| discountable | boolean |  |  | Item discountable flag; FALSE = No TRUE = Yes |
| line_disc | numeric(5,2) | Yes |  | Line discount percentage |
| line_disc_amt | numeric(15,2) | Yes |  | Line discount amount |
| serialized_qty | numeric(11) | Yes |  | Quantity of serialized/lot numbered items using Inventory unit of measure |
| employee_no | character varying(6) |  |  | Employee code added by user - links to employees.employee_no |
| user_cost | boolean |  |  | User edited cost flag; FALSE = No (cost may be updated by system) TRUE = Yes (cost will not be updated) |
| user_current_cost | boolean |  |  | User edited current cost flag; FALSE = No (cost may be updated by system) TRUE = Yes (cost will not be updated) |
| user_standard_cost | boolean |  |  | User edited standard cost flag; FALSE = No (cost may be updated by system) TRUE = Yes (cost will not be updated) |
| price_matrix_promo_code | character varying(25) |  |  | Promotion code from price_matrix record used to set the price |
| price_matrix_score | smallint |  |  | Value from inventory_price_matrix.score |
| levy_code | character varying(3) |  |  | Levy code - links to inventory_levy_codes.code |
| levy_amount | numeric(9,3) | Yes |  | Levy amount on the line |
| price_matrix_id | integer |  |  | Price Matrix record used to set the retail price on the line - links to inventory_price_matrix.id |
| guid | character varying(32) |  |  | Unique identifier for linking to other tables; inventory_serial_transactions.link_guid inventory_receipts.link_guid inventory_requisitions_src_guid |
| item_type | smallint | Yes |  | Item type for line; 1 = Inventory item 2 = Non-Inventory 3 = Comment 4 = Job header |
| required_date | date |  |  | Required date for this line |
| inventory_gl | character varying(24) |  |  | General Ledger Inventory asset account used, defaults from sales department but user can edit with permission setting |
| revenue_gl | character varying(24) |  |  | General Ledger Inventory Revenue/Sales account used, defaults from sales department but user can edit with permission setting |
| cost_gl | character varying(24) |  |  | General Ledger Inventory Cost of Goods account used, defaults from sales department but user can edit with permission setting |
| vendor_no | character varying(20) |  |  | Vendor number from inventory.preferred_vendor or entered by user |
| ref_no | character varying(20) |  |  | Reference number user entered on the line |
| req_no | character varying(10) |  |  | Requisition number that was created by this record - links to inventory_requisitions.requisition_no |
| upc_code | character varying(40) |  |  | UPC code that was scanned or entered for this line, blank if part number was used to populate the line |
| customer_part_no | character varying |  |  | Customer part no |
| po_number | character varying(10) |  |  | Purchase Order number created by Inventory Requisitions for this record |
| job_no | character varying(10) |  |  | Job cost number - links to jobs.job_no Blank will use sales_orders.job_no |
| job_acct_no | character varying(10) |  |  | Job cost account number - links to jobs.account Blank will use sales_history.job_account |
| backorder_todate_qty | numeric(15,5) | Yes |  | Not used |
| weight | numeric(15,5) |  |  | Per unit weight from Sales unit of measure unless overridden by user |
| standard_cost | numeric(15,5) | Yes |  | Standard cost price |
| comment | text |  |  | The comment text |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| tax_applicable | boolean[] | Yes |  | Tax 1 - 4 applicable flags |
| levy_tax_applicable | boolean[] | Yes |  | Tax 1 - 4 applicable to Levy flag |
| udf_data | hstore |  |  | User defined data for this record |
| partial_tax | boolean |  |  | Partial tax flag; FALSE = No TRUE = Yes |
| suppress | boolean |  |  | Suppress view/print flag as set by user on kit components; FALSE = No TRUE = Yes |
| kit | boolean |  |  | Item is kitted; FALSE = No TRUE = Yes |
| kit_component | boolean |  |  | Item is a kit component; FALSE = No TRUE = Yes |
| kit_average_cost | numeric(15,5) |  |  | Total average cost of kit |
| kit_current_cost | numeric(15,5) |  |  | Total current cost of kit |
| kit_standard_cost | numeric(15,5) |  |  | Total standard cost of kit |
| _deleted | timestamp without time zone |  |  | UTC Date and time this record was deleted by user, blank if not deleted |
| _deleted_by | character varying(3) |  |  | User initials that deleted this Sales Order, blank if not deleted |

**Constraints:**
- `bve_order_dtl_pkey` (Primary key): (id)
- `sales_order_items_order_no_sequence_key` (Unique): (order_no, sequence) DEFERRABLE INITIALLY IMMEDIATE

### sales_order_payments

Table for Deposits and Payments made on Sales Orders

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| order_no | character varying(10) |  |  | Sales Order number that payment was for - link to sales_orders.order_no |
| sequence | smallint |  |  | Sequential number for payments, used when more than one payment exists on a Sales Order |
| payment_method | integer | Yes |  | Payment method for this record - links to payment_methods.id |
| currency | character varying(3) | Yes |  | Payment method currency; blank = base currency - links to currencies.code |
| division | character varying(3) | Yes |  | General Ledger Division for payment |
| gl_account | character varying(24) | Yes |  | General Ledger account for payment |
| amount | numeric(15,2) | Yes |  | Payment amount |
| auth_code | character varying |  |  | Credit card authorization recorded at payment time |
| ccard_postal_code | character varying(16) |  |  | Credit card postal code |
| cust_no | character varying(20) |  |  | Customer numberfor payment - links to customers.cust_no |
| payment_date | date |  |  | Date payment was made |
| layaway_flag | boolean |  |  | This payment is a deposit on the Sales Order; FALSE = No TRUE = Yes |
| trans_no | character varying(10) |  |  | General Ledger transaction number for payment - links to gl_transactions.trans_no |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| change | numeric(15,2) | Yes |  | Change given on payment |
| last_four | character varying(4) |  |  | Last four characters of the credit card number |
| _deleted | timestamp without time zone |  |  | UTC Date and time this record was deleted by user, blank if not deleted |
| _deleted_by | character varying(3) |  |  | User initials that deleted this Sales Order, blank if not deleted |

**Constraints:**
- `bve_order_pmt_pkey` (Primary key): (id)
- `sales_order_payments_order_no_sequence_key` (Unique): (order_no, sequence) DEFERRABLE INITIALLY IMMEDIATE

### sales_order_equipment

Table for equipment attached to open Sales Orders

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| order_no | character varying(10) |  |  | Sales order number for this record - links to sales_orders.order_no |
| equipment_no | character varying(10) |  |  | Equipment number - links to equipment.equipment_no |
| unit_no | character varying(80) |  |  | Unit number from equipment.unit_no |
| odometer | integer |  |  | Current reading |
| estimate | numeric(15,2) |  |  | Repair estimate |
| reference_no | character varying(20) |  |  | Reference number |
| _dbversion | integer |  |  | Program version that last modified this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |

**Constraints:**
- `bve_order_vehicle_pkey` (Primary key): (id)

### sales_order_batches

Table for for Sales Order batches

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| batch_no | bigint |  |  | Batch number created when Invoice is Posted (Live Invoice mode) or when Invoice batch is created by user (Batch Invoice mode) |
| type | character varying(1) |  |  | Record type; I = Invoice posted to Sales History O = Order exists in Sales Order List |
| record_no | character varying(10) |  |  | Invoice number (type = 'I') or Sales Order number (type = 'O') |
| status | character varying(1) |  |  | Batch record status; C = Closed, records posted to Sales History O = Open, records in Sales Order List |
| division | character varying(3) |  |  | General Ledger division for batch |
| gl_trans_no | character varying(10) |  |  | General Ledger transaction number for posted Invoice - links to gl_transactions.trans_no |
| create_user | character varying(3) |  |  | User initials that created the batch (Batch Invoice mode) or posted the Invoice (Live Invoice mode) |
| post_user | character varying(3) |  |  | User initials that posted the batch (Batch Invoice mode) or posted the Invoice (Live Invoice mode) |
| location | character varying(24) |  |  | General Ledger Location segment for companies using Location accounting |
| profit_center | character varying(24) |  |  | General Ledger Profit Center segment for companies using Profit Center accounting |
| create_date | timestamp without time zone |  |  | UTC Date and time record was created |
| post_date | timestamp without time zone |  |  | UTC Date and time record was posted |

**Constraints:**
- `bve_batch_pkey` (Primary key): (id)

### sales_order_credit_authorizations

Table for credit authorizations made for Sales Orders

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| order_id | integer | Yes |  | Sales Order id - link to sales_orders.id |
| payment_method_id | integer | Yes |  | Payment Method id - link to payment_methods.id |
| amount | numeric(15,2) | Yes |  | Authorization amount |
| auth_id | character varying | Yes |  | Authorization id (third-party primary key) |
| auth_no | character varying |  |  | Authorization no |
| last_four | character varying(4) |  |  | Last four characters of the credit card number |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |

**Constraints:**
- `sales_order_credit_authorizations_pkey` (Primary key): (id)

### sales_departments

Table for Inventory Sales Departments

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| revenue | character varying(24) |  |  | General Ledger account for revenue |
| cost_of_goods | character varying(24) |  |  | General LEdger account for cost of goods |
| inventory | character varying(24) |  |  | General LEdger account for inventory asset |
| code | character varying(10) |  |  | Sales department code |
| description | character varying(30) |  |  | Sales department description |

**Constraints:**
- `sales_dept_pkey` (Primary key): (id)

### salespeople

Table for Salespeople

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| salesperson_no | character varying(10) |  |  | Salesperson code |
| name | character varying(60) |  |  | Salesperson name |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| udf_data | hstore |  |  | User defined data for this record |

**Constraints:**
- `salesperson_pkey` (Primary key): (id)

### territories

Table of Sales Territories

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| code | character varying(10) |  |  | Territory code |
| name | character varying(80) |  |  | Territory name |
| description | text |  |  | Territory description |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| udf_data | hstore |  |  | User defined data for this record |

**Constraints:**
- `territory_pkey` (Primary key): (id)

### sales_tills

Table for Sales Tills

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| code | character varying(20) |  | Yes | Sales Till code |
| description | character varying |  |  | Description |
| location | character varying(24) |  |  | Location |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
