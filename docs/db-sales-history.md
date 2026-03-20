# Database: Sales History (Invoices)

Schema: `public` (v3)

### sales_history

Table for posted Sales Invoices

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| invoice_no | character varying(10) |  |  | Invoice number |
| cust_no | character varying(20) |  |  | Customer number - links to customers.cust_no |
| cust_name | character varying(60) |  |  | Customer name |
| cust_po_no | character varying(20) |  |  | Customer Purchase Order number |
| order_date | date |  |  | Sales Order date |
| invoice_date | date |  |  | Invoice date, also used for transaction date |
| required_date | date |  |  | Required date from Sales Order |
| ref_no | character varying(20) |  |  | Reference number |
| territory_code | character varying(10) |  |  | Territory code assigned to Invoice - links to territories.code |
| salesperson_no | character varying(10) |  |  | Salesperson code assigned to Invoice - links to salespeople.salesperson_no |
| discount | numeric(5,2) | Yes |  | Discount percentage on entire Invoice |
| terms_code | character varying(10) |  |  | Payment terms code on Invoice - links to payment_terms.terms_code |
| terms_description | character varying(60) |  |  | Payment terms description |
| terms_days_before_due | smallint |  |  | Number of days from then Invoice date that the payment is due |
| terms_days_allowed | smallint |  |  | Number of days from the Invoice date for which a discount is allowed |
| terms_discount_rate | numeric(5,2) | Yes |  | Rate of the discount |
| ship_id | character varying(20) | Yes |  | Customers Shipto ID selected for this Invoice - links to addresses.ship_id |
| currency | character varying(3) | Yes |  | Customer currency; blank = base currency - links to currencies.code |
| currency_rate_method | character varying(1) |  |  | Currency rate conversion method used; / = direct * = indirect |
| currency_rate | numeric(13,7) | Yes |  | Currency rate used |
| ship_code | character varying(10) |  |  | Ship via code - links to shipping_methods.code |
| pack_date | date |  |  | Date Ship was last clicked on the Sales Order |
| pack_init | character varying(3) |  |  | Users initials that last clicked Ship onthe Sales Order |
| fob | character varying(20) |  |  | Free on board location |
| incoterms | character varying(3) |  |  | Incoterms code |
| incoterms_place | character varying |  |  | Incoterms place |
| was_quote_no | character varying(10) |  |  | Original Sales Quote number |
| quote_expires | date |  |  | Quote expiry date |
| subtotal | numeric(15,2) | Yes |  | Subtotal before taxes, discount and freight |
| total_discount | numeric(15,2) | Yes |  | Invoice discount amount given on total, does not include line discounts |
| freight | numeric(15,2) | Yes |  | Total freight |
| sales_tax | numeric(15,2) | Yes |  | Total of all sales taxes on Invoice |
| gross_profit | numeric(15,2) | Yes |  | Gross profit on Invoice |
| total_current_cost | numeric(15,2) | Yes |  | Total current cost of all items on the Invoice |
| total_average_cost | numeric(15,2) | Yes |  | Total average cost of all items on the Invoice |
| total | numeric(15,2) | Yes |  | Invoice total |
| division | character varying(3) |  |  | Division assigned to Invoice |
| order_no | character varying(10) |  |  | The Sales Order number that this Invoice originated from |
| trans_no | character varying(10) |  |  | General Ledger transaction number of this Invoice - links to gl_transactions.trans_no |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| processed_user | character varying(3) |  |  | Initials of user that clicked Process on the Sales Order |
| invoiced_user | character varying(3) |  |  | Initials of user that clicked Invoice on the Sales Order |
| ship_date | date |  |  | Ship date from the Sales Order |
| ship_carrier | character varying(20) |  |  | Ship carrier from the Sales Order |
| ship_track_id | character varying(50) |  |  | Carrier tracking number from the Sales Order |
| location | character varying(24) | Yes |  | General Ledger Location segment for companies using Location accounting |
| profit_center | character varying(24) |  |  | General Ledger Profit Center segment for companies using Profit Center accounting |
| job_no | character varying(10) |  |  | Job cost number - links to jobs.job_no |
| job_acct_no | character varying(10) |  |  | Job cost account number - links to jobs.account |
| bill_contact_id | integer |  |  | address_contacts.id reference for sales order buyer (billing address) |
| ship_contact_id | integer |  |  | address_contacts.id reference for sales order receiver (shipping address) |
| total_weight | numeric(15,2) |  |  | Total weight of the items on the Invoice |
| sales_tax_rate | numeric(7,4)[] | Yes |  | Tax 1 - 4 rates used on the Invoice |
| sales_tax_applicable | numeric(15,2)[] | Yes |  | Tax 1 - 4 applicable amounts on the Invoice |
| sales_tax_total | numeric(15,2)[] | Yes |  | Tax 1 - 4 totals for each tax |
| processed_date | timestamp without time zone |  |  | Date Process was last clicked on the Sales Order |
| invoiced_date | timestamp without time zone |  |  | Date that Invoice was clicked on the Sales Order |
| udf_data | jsonb |  |  | User defined data for this record |
| partial_tax_rate | numeric(7,4) |  |  | Partial tax rate used on the Invoice |
| partial_tax_total | numeric(15,2) |  |  | Partial tax total on the Invoice |
| phase_id | character varying(20) |  |  | Phase ID of the Sales Order when it was Invoiced - links to phases.phase_id |
| total_surcharge | numeric(15,2) | Yes |  | Total surcharges on the Invoice |
| user_freight | boolean | Yes |  | Flag to indicate that the user did a freight override; FALSE = No TRUE = Yes |
| user_surcharge | boolean |  |  | Flag to indicate that the user did a surcharge override; FALSE = No TRUE = Yes |
| was_standing_no | character varying(10) |  |  | Standing Sales Order number if this Invoice came from a Standing Order |
| total_standard_cost | numeric(15,2) | Yes |  | Total standard cost of all items on the Invoice |
| whse | character varying(6) |  |  | Default ship from warehouse |

**Constraints:**
- `sales_history_header_pkey` (Primary key): (id)

### sales_history_items

Table for posted Sales Invoice lines

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| invoice_no | character varying(10) |  |  | Invoice number - links to sales_history.invoice_no |
| sequence | smallint |  |  | Sequential number for order line |
| whse | character varying(6) |  |  | Inventory warehouse for this record - links to inventory.whse |
| part_no | character varying(34) |  |  | Inventory part number for this record - links to inventory.part_no |
| description | character varying(80) |  |  | Description of the Inventory item or first 80 characters of the comment |
| product_code | character varying(10) |  |  | Product Code of the item - links to inventory_product_codes.product_code |
| uom_inventory | character varying(10) |  |  | Unit of measure used by this item for inventory committed quantity - links to inventory_uoms |
| uom_sales | character varying(10) |  |  | Sales unit of measure - links to inventory_uoms |
| order_qty | numeric(15,5) | Yes |  | Quantity that was ordered |
| committed_qty | numeric(15,5) | Yes |  | Quantity that was shipped |
| backorder_qty | numeric(15,5) | Yes |  | Quantity that was backordered |
| inventory_order_qty | numeric(15,5) | Yes |  | Quantity ordered converted to Inventory unit of measure |
| inventory_committed_qty | numeric(15,5) | Yes |  | Quantity committed converted to Inventory unit of measure |
| inventory_backorder_qty | numeric(15,5) | Yes |  | Quantity backordered converted to Inventory unit of measure |
| serialized_qty | numeric(11) | Yes |  | Quantity of serialized/lot numbered items using Inventory unit of measure |
| retail_price | numeric(15,5) | Yes |  | Sell price on this line before discount |
| unit_price | numeric(15,5) | Yes |  | Sell price after line discount |
| current_cost | numeric(15,5) | Yes |  | Current cost of the item at time of Invoicing |
| average_cost | numeric(15,5) | Yes |  | Average cost of the item at time of Invoicing |
| status | smallint |  |  | Price method used; 0 = Price from Inventory 2 = Price from Price Matrix 9 = User override price 10 = Copied price from previous Invoice |
| discountable | boolean |  |  | Item discountable flag; FALSE = No TRUE = Yes |
| line_disc | numeric(5,2) | Yes |  | Line discount percentage |
| line_disc_amt | numeric(15,2) | Yes |  | Line discount amount |
| required_date | date |  |  | Required date for this line |
| guid | character varying(32) |  |  | Unique identifier for linking to other tables; inventory_serial_transactions.link_guid inventory_receipts.link_guid inventory_requisitions_src_guid |
| item_type | smallint | Yes |  | Item type for line; 1 = Inventory item 2 = Non-Inventory 3 = Comment 4 = Job header |
| invoice_date | date |  |  | Invoice date from sales_history |
| comment | text |  |  | Comment text |
| po_number | character varying(10) |  |  | Purchase Order number created by Inventory Requisitions for this record |
| _dbversion | integer |  |  | Program version that last modified this record (Invoiced it) |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| employee_no | character varying(6) |  |  | Employee code added by user - links to employees.employee_no |
| parent_item | smallint |  |  | Link to sales_history_items.sequence for the parent of this group, used by kits, accessories and job items |
| price_matrix_promo_code | character varying(25) |  |  | Promotion code from price_matrix record used to set the price |
| inventory_gl | character varying(24) |  |  | General Ledger Inventory asset account used |
| revenue_gl | character varying(24) |  |  | General Ledger Inventory Revenue/Sales account used |
| cost_gl | character varying(24) |  |  | General Ledger Inventory Cost of Goods account used |
| ref_no | character varying(20) |  |  | Reference number user entered on the line |
| upc_code | character varying(40) |  |  | UPC code that was scanned or entered for this line, blank if part number was used to populate the line |
| customer_part_no | character varying |  |  | Customer part no |
| job_no | character varying(10) |  |  | Job cost number - links to jobs.job_no Blank used sales_history.job_no |
| job_acct_no | character varying(10) |  |  | Job cost account number - links to jobs.account Blank used sales_history.job_account |
| weight | numeric(15,5) |  |  | Weight of item on this line |
| tax_applicable | boolean[] | Yes |  | Tax 1 - 4 applicable flags |
| udf_data | jsonb |  |  | User defined data for this record |
| standard_cost | numeric(15,5) | Yes |  | Standard cost of the item at time of Invoicing |
| partial_tax | boolean |  |  | Partial tax flag; FALSE = No TRUE = Yes |
| levy_amount | numeric(9,3) | Yes |  | Levy amount on the line |
| levy_code | character varying(3) |  |  | Levy code - links to inventory_levy_codes.code |
| levy_tax_applicable | boolean[] | Yes |  | Tax 1 - 4 applicable to Levy flag |
| price_matrix_id | integer |  |  | Price Matrix record used to set the retail price on the line - links to inventory_price_matrix.id |
| price_matrix_score | smallint |  |  | Value from inventory_price_matrix.score |
| sell_direct_factor | boolean |  |  |  |
| uom_sales_factor | numeric(11,5) |  |  |  |
| user_cost | boolean |  |  | User edited cost flag; FALSE = No TRUE = Yes |
| user_current_cost | boolean |  |  | User edited cost flag; FALSE = No TRUE = Yes |
| user_standard_cost | boolean |  |  | User edited cost flag; FALSE = No TRUE = Yes |
| vendor_no | character varying(20) |  |  | Vendor number from inventory.preferred_vendor or entered by user |
| suppress | boolean |  |  | Suppress view/print flag as set by user on kit components; FALSE = No TRUE = Yes |
| kit | boolean |  |  | Item is kitted; FALSE = No TRUE = Yes |
| kit_component | boolean |  |  | Item is a kit component; FALSE = No TRUE = Yes |
| kit_average_cost | numeric(15,5) |  |  | Total average cost of kit |
| kit_current_cost | numeric(15,5) |  |  | Total current cost of kit |
| kit_standard_cost | numeric(15,5) |  |  | Total standard cost of kit |

**Constraints:**
- `sales_history_detail_pkey` (Primary key): (id)

### sales_history_payments

Table for Payments on posted Sales Invoices

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| invoice_no | character varying(10) |  |  | Invoice number that payment was for - link to sales_history.invoice_no |
| payment_method | integer | Yes |  | Payment method for this record - links to payment_methods.id |
| currency | character varying(3) | Yes |  | Payment method currency; blank = base currency - links to currencies.code |
| amount | numeric(15,2) | Yes |  | Payment amount |
| division | character varying(3) | Yes |  | General Ledger Division for payment |
| gl_account | character varying(24) | Yes |  | General Ledger account for payment |
| auth_code | character varying |  |  | Credit card authorization recorded at payment time |
| _dbversion | integer |  |  | UTC Date and time record was created |
| _created | timestamp without time zone |  |  | User initials that created this record |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| sequence | smallint |  |  | Sequential number for payments, used when more than one payment exists on an Invoice |
| payment_date | date |  |  | Date payment was made |
| change | numeric(15,2) | Yes |  | Change given on payment |
| last_four | character varying(4) |  |  | Last four characters of the credit card number |

**Constraints:**
- `payment_history_pkey` (Primary key): (id)

### sales_history_equipment

Table for equipment attached to posted Invoices

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| invoice_no | character varying(10) |  |  | Invoice number for this record - links to sales_history.invoice_no |
| cust_no | character varying(20) |  |  | Customer number - links to customers.cust_no |
| equipment_no | character varying(10) |  |  | Equipment number - links to equipment.equipment_no |
| previous_odometer | integer |  |  | Previous reading |
| odometer | integer |  |  | Current reading |
| order_no | character varying(10) |  |  | Sales order number |
| cust_po_no | character varying(20) |  |  | Customer Purchase Order number |
| invoice_date | date |  |  | Invoice date |
| total_net | numeric(15,2) |  |  | Invoice subtotal |
| total_current_cost | numeric(15,2) |  |  | Total current cost |
| total_average_cost | numeric(15,2) |  |  | Total average cost |
| followup_date | date |  |  | Date to follow-up |
| reference_no | character varying(20) |  |  | Reference number |
| _dbversion | integer |  |  | Program version that last modified this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |

**Constraints:**
- `bve_vehicle_history_pkey` (Primary key): (id)

### sales_taxes

Table for Sales Taxes

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| tax_no | smallint |  |  | Tax code created by user |
| name | character varying(60) |  |  | Tax name or description |
| rate | numeric(7,4) |  | Yes | Tax rate to apply |
| gl_account | character varying(24) |  |  | General Ledger tax liability accouunt |
| short_name | character varying(20) |  |  | Tax short name to use on forms |
| use_partial_rate | boolean |  |  | Partial tax rate allowed flag; TRUE = Yes FALSE = No |
| partial_rate | numeric(7,4) |  |  | Partial tax rate |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| udf_data | hstore |  |  | User defined data for this record |
| freight | boolean |  |  | Tax applicable to freight flag; FALSE = No TRUE = Yes |
| surcharge | boolean |  |  | Tax applicable to surcharges flag; FALSE = No TRUE = Yes |
| include_landed | boolean |  |  | Include tax to Purchase Order landed cost flag; FALSE = No (tax is recoverable) TRUE = Yes (tax is added to purchase cost) |
| gl_credit_account | character varying(24) |  |  | General Ledger account for recoverable tax - Input Tax Credit account |
| integrated | boolean |  |  | Sales tax integration flag |
| customer_default | boolean |  |  | Default on new customer billing address |
| vendor_default | boolean |  |  | Default on new vendor billing address |

**Constraints:**
- `sales_tax_pkey` (Primary key): (id)
