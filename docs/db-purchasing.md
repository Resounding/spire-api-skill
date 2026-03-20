# Database: Purchasing

Schema: `public` (v3)

### purchase_orders

Table for open Purchase Orders

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes |  |
| po_number | character varying(10) |  |  | Purchase Order number |
| vendor_no | character varying(20) |  |  | Vendor code - links to vendors.vendor_no |
| vendor_name | character varying(60) |  |  | Vendor name |
| vendor_po | character varying(20) |  |  | Vendor Order number |
| date | date |  |  | Purchase Order date |
| status | character varying(1) |  |  | Purchase Order status; C= Closed (only exists if Purchase Order failed move to history) I = Issued H = Hold O = Open R = Received S = Standing |
| required_date | date |  |  | Required date on Purchase Order |
| received_date | date |  |  | Last received date on Purchase Order |
| buyer_name | character varying(60) |  |  | Buyer name on Purchase Order |
| discount | numeric(5,2) |  |  | Doscount % on the Purchase Order |
| terms_code | character varying(10) |  |  | Terms code for Purchase Order, will be used to populate the Vendor Invoice - Links to payment_terms.terms_code |
| terms_description | character varying(60) |  |  | Payment Terms description, will be used to populate the Vendor Invoice |
| terms_days_before_due | smallint |  |  | Number of days before the payment is due calculated from transaction date, will be used to populate the Vendor Invoice |
| terms_days_allowed | smallint |  |  | Number of days allowed for discount, will be used to populate the Vendor Invoice |
| terms_discount_rate | numeric(5,2) |  |  | Early payment discount percentage rate, will be used to populate the Vendor Invoice |
| ship_id | character varying(20) | Yes |  | Not used |
| currency | character varying(3) |  |  | Vendor currency; blank = base currency - links to currencies.code |
| currency_rate_method | character varying(1) |  |  | Currency rate conversion method used; / = direct * = indirect |
| currency_rate | numeric(13,7) |  |  | Currency rate used |
| fob | character varying(20) |  |  | Free on board location |
| incoterms | character varying(3) |  |  | Incoterms code |
| incoterms_place | character varying |  |  | Incoterms place |
| freight_balance | numeric(15,2) |  |  | Freight balance accrued, not invoiced |
| ship_via_code | character varying(10) |  |  | Ship Via code - links to shipping_methods.code |
| ref_no | character varying(20) |  |  | Reference number as set by user |
| serialized_items | boolean |  |  | Purchase Order has serialized items |
| subtotal | numeric(15,2) |  |  | Subtotal of items to be received before taxes, freight and discount |
| total_discount | numeric(15,2) |  |  | Total discount amount |
| freight | numeric(15,2) |  |  | Vendor Freight on entire Purchase Order |
| sales_tax | numeric(15,2) |  |  | Total sales tax for Purchase Order on received items |
| gross_profit | numeric(15,2) |  |  | Gross Profit for Purchase Order if Status = Issued, Gross Profit on received items if Status = Received |
| total_current_cost | numeric(15,2) |  |  | Not used |
| total_average_cost | numeric(15,2) |  |  | Not used |
| total | numeric(15,2) |  |  | Total of items to be received |
| po_amount | numeric(15,2) |  |  | Total value of products ordered |
| amount_received | numeric(15,2) |  |  | Total Amount received to date |
| landed_freight | numeric(15,5) |  |  | Landed freight to date |
| landed_duty | numeric(15,5) |  |  | Landed duty to date |
| issued_amt | numeric(15,5) |  |  | Issued amount |
| accrued_to_date | numeric(15,5) |  |  | Accrued receipts to date |
| invoiced_bal | numeric(15,2) |  |  | Amount to send to Accounts payable |
| invoiced_net | numeric(15,2) |  |  | Net amount for account payable |
| deposit | numeric(15,5) | Yes |  | Prepaid deposit |
| deposit_consumed | numeric(15,5) | Yes |  | Prepaid deposit consumed by receipts |
| print_date | date |  |  | Date Purchase Order was Issued and Print dialog presented |
| print_init | character varying(3) |  |  | User initials that Issued the Purchase Order and had the oppurtunity to Print |
| subtotal_issued | numeric(15,2) |  |  | Subtotal of items when ordered or issued |
| total_discount_issued | numeric(15,2) |  |  | Total discount when ordered or issued |
| freight_issued | numeric(15,2) |  |  | Freight when ordered or issued |
| sales_tax_issued | numeric(15,2) |  |  | Total sales tax on items when ordered or issued |
| gross_profit_issued | numeric(15,2) |  |  | Not used |
| current_cost_issued | numeric(15,2) |  |  | Not used |
| average_cost_issued | numeric(15,2) |  |  | Not used |
| total_issued | numeric(15,2) |  |  | Total of items when ordered or issued |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| whse | character varying(6) |  |  | Purcahse Order warehouse, used as default for items to order - links to inventory_warehouses.whse |
| ship_customer | character varying(20) |  |  | Customer selected for drop ship - links to customers.cust_no |
| location | character varying(24) |  |  | General Ledger Location segment of the warehouse for companies using Location accounting |
| division | character varying(3) |  |  | Division code for Purchase Order as per divisions in the company; 000 = non divisionalized company - links to gl_divisions.code |
| phase_id | character varying(20) |  |  | Current phase ID of the Purchase Order - links to phases.phase_id |
| job_no | character varying(10) |  |  | Job cost number - links to jobs.job_no |
| job_acct_no | character varying(10) |  |  | Job cost account number - links to jobs.account |
| weight | numeric(15,2) |  |  | Total weight on Purchase Order |
| sales_tax_rate | numeric(7,4)[] | Yes |  | Sales tax rates 1 - 4 used on Purchase Order |
| sales_tax_applicable | numeric(15,2)[] | Yes |  | Sales tax applicable flags 1 - 4 used on Purchase Order |
| sales_tax_total | numeric(15,2)[] | Yes |  | Sales tax totals 1 - 4 on Purchase Order |
| sales_tax_applicable_issued | numeric(15,2)[] | Yes |  | Sales tax applicable flags 1 - 4 used on Purchase Order when Issued |
| sales_tax_total_issued | numeric(15,2)[] | Yes |  | Sales tax totals 1 - 4 on Purchase Order when Issued |
| udf_data | jsonb |  |  | User defined data for this record |

**Constraints:**
- `purchase_order_headr_pkey` (Primary key): (id)

### purchase_order_items

Table for items on open Purchase Orders

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| po_number | character varying(10) |  |  | Purchase Order number |
| sequence | smallint |  |  | Sequential number for order line |
| whse | character varying(6) |  |  | Inventory warehouse for this record - links to inventory.whse |
| part_no | character varying(34) |  |  | Inventory part number for this record - links to inventory.part_no |
| description | character varying(80) |  |  | Description of the item |
| product_code | character varying(10) |  |  | Product Code assigned to the item - links to inventory_product_codes.product_code |
| uom_purchase | character varying(10) |  |  | Purchase unit of measure - links to inventory_uoms |
| uom_inventory | character varying(10) |  |  | Inventory unit of measure - links to inventory_uoms |
| uom_sales | character varying(10) |  |  | Sales unit of measure - links to inventory_uoms |
| order_qty | numeric(15,5) |  |  | Quantity ordered |
| received_qty | numeric(15,5) |  |  | Quantity received to date |
| inventory_order_qty | numeric(15,5) |  |  | Quantity ordered converted to Inventory unit of measure |
| inventory_received_qty | numeric(15,5) |  |  | Quantity received to date converted to Inventory unit of measure |
| last_received_qty | numeric(15,5) |  |  | Last received quantity |
| serialized_qty | numeric(11) |  |  | Serialized quantity received |
| retail_price | numeric(15,5) |  |  | Price before line discount |
| unit_price | numeric(15,5) |  |  | Price after line discount |
| status | smallint |  |  | Pricing status; 0 = price taken from Inventory current cost 5 = Vendor Price used 9 = user typed price |
| whse_location | character varying(20) |  |  | Physical location in warehouse for item |
| vendor_part_no | character varying(34) |  |  | Vendor part number from Vendor Price table |
| guid | character varying(32) |  |  | Unique identifier for linking to other tables; inventory_serial_transactions.link_guid inventory_receipts.link_guid inventory_requisitions_targ_guid purchase_receipts.guid |
| item_type | smallint |  |  | Item record type for this line; 1 = Inventory item 2 = Non Inventory item 3 = Comment 4 = Serial/Lot numbered item 5 = Non Physical item |
| last_received_date | date |  |  | Date Purchase Order line was last received |
| required_date | date |  |  | Required date for this item |
| comment | text |  |  | Comment on line |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| reference | character varying(20) |  |  | Reference number on the line |
| receive_qty | numeric(11,3) |  |  | Receive quantity saved on Purchase Order before posting Receipt |
| inventory_receive_qty | numeric(11,3) |  |  | Receive quantity saved on Purchase Order converted to Inventory unit of measure, before posting Receipt |
| duty_by_pct | boolean |  |  | Calculate duty by percent flag; FALSE = No TRUE = Yes |
| duty_rate | numeric(5,2) |  |  | Duty percentage |
| duty_amount | numeric(15,5) |  |  | Duty amount |
| freight_by_pct | boolean |  |  | Calculate freight by percent flag; FALSE = No TRUE = Yes |
| freight_rate | numeric(5,2) |  |  | Freight percentage |
| freight_amount | numeric(15,5) |  |  | Freight amount |
| gl_account | character varying(24) |  |  | General Ledger Inventory Asset account to post receipt to, defaults to sales department but is editable by user with permission |
| sell_price | numeric(15,5) |  |  | Inventory Sell price 1 |
| user_sell_price | boolean |  |  | User edited sell price flag; FALSE = No TRUE = Yes |
| pack_size | numeric(9,3) |  |  | Normal pack size |
| requisition_no | character varying(10) |  |  | Requisition number that created this record - links to inventory_requisitions.requisition_no |
| employee | character varying(6) |  |  | Employee code added by user - links to employees.employee_no |
| ship_to | character varying(20) |  |  | Ship to text as entered by user |
| job_no | character varying(10) |  |  | Job cost number - links to jobs.job_no |
| job_acct | character varying(10) |  |  | Job cost account number - links to jobs.account |
| weight | numeric(15,2) |  |  | Weight per each of order quantity of the item |
| tax_applicable | boolean[] | Yes |  | Tax 1 - 4 applicable flags; False = No True = Yes |
| vendor_price_id | integer |  |  | Link to vendor_pricing.id |
| udf_data | jsonb |  |  | User defined data for this record |
| discountable | boolean |  |  | Discountable flag; FALSE = No TRUE = Yes |
| line_disc | numeric(5,2) | Yes |  | Line discount percentage |
| line_disc_amt | numeric(15,2) | Yes |  | Line discount amount |

**Constraints:**
- `purchase_order_dtail_pkey` (Primary key): (id)
- `purchase_order_items_vendor_price_id_fkey` (Foreign key): (vendor_price_id) REFERENCES public.vendor_pricing (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION
- `purchase_order_items_po_number_sequence_key` (Unique): (po_number, sequence) DEFERRABLE INITIALLY IMMEDIATE

### purchase_history

Table for closed Purchase Orders

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| po_number | character varying(10) |  |  | Purchase Order number |
| vendor_no | character varying(20) |  |  | Vendor code - links to vendors.vendor_no |
| vendor_name | character varying(60) |  |  | Vendor name |
| vendor_po | character varying(20) |  |  | Vendor Order number |
| date | date |  |  | Purchase Order date |
| status | character varying(1) |  |  | Purchase Order status - Always 'C' - Closed |
| required_date | date |  |  | Required date on Purchase Order |
| received_date | date |  |  | Last received date on Purchase Order |
| buyer_name | character varying(60) |  |  | Buyer name on Purchase Order |
| discount | numeric(5,2) |  |  | Doscount % on the Purchase Order |
| terms_code | character varying(10) |  |  | Terms code for Purchase Orderwhich was used to populate the Vendor Invoice - links to payment_terms.terms_code |
| terms_description | character varying(60) |  |  | Payment Terms description which was used to populate the Vendor Invoice |
| terms_days_before_due | smallint |  |  | Number of days before the payment is due calculated from transaction date which was used to populate the Vendor Invoice |
| terms_days_allowed | smallint |  |  | Number of days allowed for discount which was used to populate the Vendor Invoice |
| terms_discount_rate | numeric(5,2) |  |  | Early payment discount percentage rate which was used to populate the Vendor Invoice |
| ship_id | character varying(20) | Yes |  | Not used |
| currency | character varying(3) |  |  | Vendor currency; blank = base currency - links to currencies.code |
| currency_rate_method | character varying(1) |  |  | Currency rate conversion method used; / = direct * = indirect |
| currency_rate | numeric(13,7) |  |  | Currency rate used |
| fob | character varying(20) |  |  | FOB - Free on board location |
| incoterms | character varying(3) |  |  | Incoterms code |
| incoterms_place | character varying |  |  | Incoterms place |
| freight_balance | numeric(15,2) |  |  | Freight balance |
| ship_via_code | character varying(10) |  |  | Ship Via code - links to shipping_methods.code |
| ref_no | character varying(20) |  |  | Reference number as set by user |
| serialized_items | boolean |  |  | Purchase Order has serialized items |
| subtotal | numeric(15,2) |  |  | Subtotal before taxes, freight and discount |
| total_discount | numeric(15,2) |  |  | Total discount amount |
| freight | numeric(15,2) |  |  | Vendor Freight amount |
| sales_tax | numeric(15,2) |  |  | Total sales tax amount on received amount |
| gross_profit | numeric(15,2) |  |  | Gross Profit on items received |
| total_current_cost | numeric(15,2) |  |  | Total current cost of items on the Purchase Order |
| total_average_cost | numeric(15,2) |  |  | Total average cost of items on the Purchase Order |
| total | numeric(15,2) |  |  | PO total |
| print_date | date |  |  | Date Purchase Order was Issued and Print dialog presented |
| print_init | character varying(3) |  |  | User initials that Issued the Purchase Order and had the oppurtunity to Print |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| whse | character varying(6) |  |  | Purcahse Order warehouse - links to inventory_warehouses.whse |
| ship_customer | character varying(20) |  |  | Customer selected for drop ship - links to customers.cust_no |
| location | character varying(24) |  |  | General Ledger Location segment of the warehouse for companies using Location accounting |
| division | character varying(3) |  |  | Division code for Purchase Order as per divisions in the company; 000 = non divisionalized company - links to gl_divisions.code |
| phase_id | character varying(20) |  |  | Phase ID of the Purchase Order when Closed - links to phases.phase_id |
| job_no | character varying(10) |  |  | Job cost number - links to jobs.job_no |
| job_acct_no | character varying(10) |  |  | Job cost account number - links to jobs.account |
| weight | numeric(15,2) |  |  | Total weight on Purchase Order |
| sales_tax_rate | numeric(7,4)[] | Yes |  | Sales tax rates 1 - 4 used on Purchase Order |
| sales_tax_applicable | numeric(15,2)[] | Yes |  | Sales tax applicable flags 1 - 4 used on Purchase Order |
| sales_tax_total | numeric(15,2)[] | Yes |  | Sales tax totals 1 - 4 on Purchase Order |
| udf_data | jsonb |  |  | User defined data for this record |

**Constraints:**
- `purchase_history_hdr_pkey` (Primary key): (id)

### purchase_history_items

Table for items on closed Purchase Orders

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| po_number | character varying(10) |  |  | Purchase Order number |
| sequence | smallint |  |  | Sequential number for order line |
| whse | character varying(6) |  |  | Inventory warehouse for this record - links to inventory.whse |
| part_no | character varying(34) |  |  | Inventory part number for this record - links to inventory.part_no |
| description | character varying(80) |  |  | Description of item |
| product_code | character varying(10) |  |  | Product Code assigned to the item - links to inventory_product_codes.product_code |
| uom_purchase | character varying(10) |  |  | Purchase unit of measure - links to inventory_uoms |
| uom_inventory | character varying(10) |  |  | Inventory unit of measure - links to inventory_uoms |
| uom_sales | character varying(10) |  |  | Sales unit of measure - links to inventory_uoms |
| order_qty | numeric(15,5) |  |  | Quantity ordered |
| received_qty | numeric(15,5) |  |  | Quantity received |
| inventory_order_qty | numeric(15,5) |  |  | Quantity ordered converted to Inventory unit of measure |
| inventory_received_qty | numeric(15,5) |  |  | Quantity received converted to Inventory unit of measure |
| last_received_qty | numeric(15,5) |  |  | Last received quantity |
| serialized_qty | numeric(11) |  |  | Serialized quantity received |
| retail_price | numeric(15,5) |  |  | Price before line discount |
| unit_price | numeric(15,5) |  |  | Price after line discount |
| status | smallint |  |  | Pricing status; 0 = price taken from Inventory current cost 5 = Vendor Price used 9 = user typed price |
| whse_location | character varying(20) |  |  | Physical location in warehouse for item |
| vendor_part_no | character varying(34) |  |  | Vendor part number from Vendor Price table |
| guid | character varying(32) |  |  | Unique identifier for linking to other tables; inventory_serial_transactions.link_guid inventory_receipts.link_guid inventory_requisitions_targ_guid purchase_receipts.guid |
| item_type | smallint |  |  | Item record type for this line; 1 = Inventory item 2 = Non Inventory item 3 = Comment 4 = Serial/Lot numbered item 5 = Non Physical item |
| last_received_date | date |  |  | Date Purchase Order line was last received |
| required_date | date |  |  | Required date for this item |
| comment | text |  |  | Comment on line |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| reference | character varying(20) |  |  | Reference number on the line |
| src_type | character varying(4) |  |  | Source type from Requisition; INV = Inventory SORD = Sales Order BORD = Production Order |
| src_no | character varying(40) |  |  | Source Sales Order number or Production Order number from Requisition |
| src_cust_no | character varying(20) |  |  | Source Customer number from Requisition |
| employee | character varying(6) |  |  | Employee code added by user - links to employees.employee_no |
| weight | numeric(15,2) |  |  | Weight per each of order quantity of the item |
| job_no | character varying(10) |  |  | Job cost number - links to jobs.job_no |
| job_acct | character varying(10) |  |  | Job cost account number - links to jobs.account |
| tax_applicable | boolean[] | Yes |  | Tax 1 - 4 applicable flags; False = No True = Yes |
| udf_data | jsonb |  |  | User defined data for this record |
| discountable | boolean |  |  | Discountable flag; FALSE = No TRUE = Yes |
| line_disc | numeric(5,2) | Yes |  | Line discount percentage |
| line_disc_amt | numeric(15,2) | Yes |  | Line discount amount |

**Constraints:**
- `purchase_history_dtl_pkey` (Primary key): (id)

### purchase_receipts

Table for all receipts in Purchase Orders

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | bigint | Yes | Yes | Unique identifier |
| order_id | integer |  |  | Refers to the open Purchase Order for this record - links to purchase_orders.id |
| history_id | integer |  |  | Refers to the closed Purchase Order for this record - links to purchase_history.id |
| inventory_id | integer |  |  | Refers to the Inventory item on this record - links to inventory.id |
| inventory_receipt_id | bigint |  |  | Refers to the Inventory Receipt record - links to inventory_receipts.id |
| item_type | smallint |  |  | Item record type for this line; 1 = Inventory item 2 = Non Inventory item 4 = Serial/Lot numbered item 5 = Non Physical item |
| whse | character varying(6) | Yes |  | Inventory warehouse for this record - links to inventory.whse |
| part_no | character varying(34) | Yes |  | Inventory part number for this record - links to inventory.part_no |
| description | character varying(80) |  |  | Description of the item |
| guid | character varying(32) |  |  | GUID of the Purchase Order line that this record was received on |
| receive_date | date |  |  | Date iem was received |
| whse_location | character varying(20) |  |  | Physical location in warehouse for item |
| receive_qty | numeric(15,5) |  |  | Quantity received at this time |
| receive_uom | character varying(10) |  |  | Unit of measure received - links to inventory_uoms |
| receive_cost | numeric(15,5) |  |  | Cost of item received |
| currency | character varying(3) |  |  | Vendor currency; blank = base currency - links to currencies.code |
| currency_rate_method | character varying(1) |  |  | Currency rate conversion method used; / = direct * = indirect |
| currency_rate | numeric(13,7) |  |  | Currency rate used |
| duty | numeric(15,5) |  |  | Duty amount on this line |
| vendor_freight | numeric(15,5) |  |  | Vendor freight allocated to this line |
| freight | numeric(15,5) |  |  | Landed freight on this line |
| weight | numeric(15,5) |  |  | Item weight |
| inventory_qty | numeric(15,5) |  |  | Quantity received converted to Inventory unit of measure |
| inventory_uom | character varying(10) |  |  | Inventory unit of measure - links to inventory_uoms |
| inventory_cost | numeric(15,5) |  |  | Cost converted to Inventory unit of measure |
| sell | numeric(15,5) |  |  | Sell Price 1 |
| ref_no | character varying(20) |  |  | Reference number from the Purchase Order line |
| tax_applicable | numeric(15,2)[] | Yes |  | Tax 1 - 4 applicable flags |
| _dbversion | integer |  |  | Program version that last modified this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |

**Constraints:**
- `purchase_receipts_pkey` (Primary key): (id)
- `purchase_receipts_history_id_fkey` (Foreign key): (history_id) REFERENCES public.purchase_history (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION
- `purchase_receipts_inventory_id_fkey` (Foreign key): (inventory_id) REFERENCES public.inventory (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION
- `purchase_receipts_inventory_receipt_id_fkey` (Foreign key): (inventory_receipt_id) REFERENCES public.inventory_receipts (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION
- `purchase_receipts_order_id_fkey` (Foreign key): (order_id) REFERENCES public.purchase_orders (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION

### vendors

Table for Vendors

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| vendor_no | character varying(20) |  |  | Vendor code |
| currency | character varying(3) | Yes |  | Vendor currency; blank = base currency - links to currencies.code |
| name | character varying(60) |  |  | Vendor's name |
| account_no | character varying(20) | Yes |  | Vendor assigned account number |
| last_po_number | character varying(10) |  |  | Last Purchase Order issued to this Vendor |
| last_payment_ref | character varying(25) |  |  | Last payment reference number |
| last_payment_date | date |  |  | Last payment date |
| terms_code | character varying(10) |  |  | Default Terms code - links to payment_terms.terms_code |
| present_bal | numeric(15,2) |  |  | Present balance owing |
| no_credit | smallint |  |  | Not used |
| credit_line | numeric(13) |  |  | Credit limit |
| notes | character varying(30) |  |  | Vendor note |
| buyer_name | character varying(60) |  |  | Buyer name |
| reference | character varying(60) | Yes |  | Reference |
| tax_id_type | character varying(1) |  |  | CPRS/1099 Type flag; B = Business Number U = Social Insurance Number |
| gl_ap_no | character varying(24) |  |  | General Ledger Accounts Payable control account - links to gl_accounts.account_no |
| tax_id | character varying(60) |  |  | Business number or Social Insurance number |
| use_remit_to | boolean | Yes |  | Use Remit To address for payments; FALSE = No TRUE = Yes |
| dental_benefits | smallint |  |  | Employer-offered dental benefits |
| print_cheques | boolean |  |  | Print cheques by default; FALSE = No TRUE = Yes |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| hold | boolean | Yes |  | On hold flag; FALSE = No TRUE = Yes |
| status | character varying(1) | Yes |  | Status flag; A = Active I = Inactive |
| udf_data | jsonb |  |  | User defined data for this record |
| use_billing_taxes | boolean |  |  | Use Billing address tax flags on Purchase Orders; FALSE = No TRUE = Yes |
| last_year_purchases | numeric(15,2)[] | Yes |  | Last Year period 1 - 13 purchases by period |
| this_year_purchases | numeric(15,2)[] | Yes |  | This Year period 1 - 13 purchases by period |
| next_year_purchases | numeric(15,2)[] | Yes |  | Next Year period 1 - 13 purchases by period |
| color_text | bigint | Yes |  | Text colour code for display of name |
| color_back | bigint | Yes |  | Background colour code for display of name |
| bank_institution | character varying(3) |  |  | Bank Institution number for electronic payments |
| bank_transit | character varying(9) |  |  | Bank Transit number for electronic payments |
| bank_account | character varying(31) |  |  | Bank Account number for electronic payments |
| payment_account | character varying(24) |  |  |  |
| tax_report_type | integer |  |  | Tax report type; 1 = 1099 2 = T4A 3 = T5018 |

**Constraints:**
- `vendor_pkey` (Primary key): (id)

### vendor_code_changes

Table for saved Vendor Code changes

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| old_vendor_no | character varying(20) |  |  | Original Vendor code |
| new_vendor_no | character varying(20) |  |  | Replacement Vendor code |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |

**Constraints:**
- `vendor_code_changes_pkey` (Primary key): (id)

### vendor_pricing

Table for Vendor prices per part number

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| uom | character varying(10) |  |  | Unit of Measure for this record |
| price | numeric(15,5) |  |  | Price for this record |
| auto_erase_record | boolean |  |  | Remove record after expiry flag; FALSE = No TRUE = Yes |
| vendor_code | character varying(34) |  |  | Vendor part number |
| min_ord_qty | numeric(15,5) |  |  | Minimum order quantity permited to order |
| updatecostprice | boolean |  |  | Automatically update this record when receiving this item; FALSE = No TRUE = Yes |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| inventory_id | integer | Yes |  | ID from table inventory |
| vendor_id | integer | Yes |  | ID from table vendor |
| break_qty | numeric(11)[] | Yes |  | Minimum order quantity to qualify for this price, level 1 -15 |
| break_qty_price | numeric(15,5)[] | Yes |  | Price when ordering specified quantity, level 1 -15 |
| valid_dates | daterange |  |  | Valid date range for this record |

**Constraints:**
- `vendor_pricing_pkey` (Primary key): (id)
- `vendor_pricing_inventory_id_fkey` (Foreign key): (inventory_id) REFERENCES public.inventory (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE CASCADE
- `vendor_pricing_vendor_id_fkey` (Foreign key): (vendor_id) REFERENCES public.vendors (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE CASCADE
- `vendor_pricing_inventory_id_uom_vendor_id_valid_dates_excl` (Exclude): USING gist (inventory_id WITH =, uom WITH =, vendor_id WITH =, valid_dates WITH &amp;&amp;)

### vendor_t4as

Table for Vendor T4A records

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| vendor_id | integer | Yes |  | Vendor ID for this record - links to vendors.id |
| year | integer | Yes |  | Year of the T4 |
| last_name | character varying(30) |  |  | Vendor last name |
| first_name | character varying(30) |  |  | Vendor first name |
| initials | character varying(5) |  |  | Vendor initials |
| sin | character varying(9) |  |  | Vendor Social Insurance Number |
| account_no | character varying(15) |  |  | Vendor Business Number |
| pension | numeric(15,2) |  |  | Pension amount |
| lump_sum | numeric(15,2) |  |  | Lump sum amount |
| commission | numeric(15,2) |  |  | Commission amount |
| income_tax_deducted | numeric(15,2) |  |  | Income tax deducted amount |
| annuities | numeric(15,2) |  |  | Annuities amount |
| service_fees | numeric(15,2) |  |  | Service fees amount |
| dental_benefits | smallint |  |  | Employer-offered dental benefits |
| other_boxes | character varying[] |  |  | Array for Other Boxes codes |
| other_amounts | numeric(15,2)[] |  |  | Array for Other Boxes amounts |
| suggested_pension | numeric(15,2) |  |  | Suggested pension amount |
| suggested_lump_sum | numeric(15,2) |  |  | Suggested lump sum amount |
| suggested_commission | numeric(15,2) |  |  | Suggested commission amount |
| suggested_income_tax_deducted | numeric(15,2) |  |  | Suggested income tax amount |
| suggested_annuities | numeric(15,2) |  |  | Suggested annuity amount |
| suggested_service_fees | numeric(15,2) |  |  | Suggested service fees amount |
| suggested_other_amounts | numeric(15,2)[] |  |  | Array for suggested other amounts |
| remitted | date |  |  | Remitted date |
| status | character varying(1) |  |  | T4A status; A = Amended C = Cancelled O = Original |
| _dbversion | integer |  |  | Program version that last modified this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |

**Constraints:**
- `vendor_t4as_pkey` (Primary key): (id)
- `vendor_t4as_vendor_id_fkey` (Foreign key): (vendor_id) REFERENCES public.vendors (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION

### vendor_t5018s

Table for Vendor T5018 records

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| vendor_id | integer | Yes |  | Vendor ID for this record - links to vendors.id |
| period_ending | date | Yes |  | End date for reporting period |
| last_name | character varying(30) |  |  | Vendor last name |
| first_name | character varying(30) |  |  | Vendor first name |
| initials | character varying(5) |  |  | Vendor initials |
| account_no | character varying(15) |  |  | Vendor Business Number |
| payments | numeric(15,2) |  |  | Total payments for the reporting period |
| suggested_payments | numeric(15,2) |  |  | Sugested total payments for the reporting period |
| remitted | date |  |  | Remitted date |
| status | character varying(1) |  |  | T4A status; A = Amended C = Cancelled O = Original |
| _dbversion | integer |  |  | Program version that last modified this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |

**Constraints:**
- `vendor_t5018s_pkey` (Primary key): (id)
- `vendor_t5018s_vendor_id_fkey` (Foreign key): (vendor_id) REFERENCES public.vendors (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION
