# Database: Inventory

Schema: `public` (v3)

### inventory

Table for all Inventory items, one record exists for each warehouse, part number combination

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| whse | character varying(6) | Yes |  | Warehouse for this item |
| part_no | character varying(34) | Yes |  | Part number for this item |
| description | character varying(80) |  |  | Description for this item |
| product_code | character varying(10) |  |  | Product Code assigned to the item - links to inventory_product_codes.product_code |
| hold | smallint |  |  | Hold flag for item; 0 = Active 1 = On Hold 2 = Inactive |
| current_cost | numeric(15,5) |  |  | Current or last cost of the item as updated by inventory receiving however is editable by user |
| average_cost | numeric(15,5) |  |  | Average cost of the item as calculated at inventory receipt time |
| uom_purchase | character varying(10) |  |  | Default unit of measure to be used for purchasing - links to inventory_uoms |
| uom_inventory | character varying(10) |  |  | Unit of measure used by this item for On Hand quantity - links to inventory_uoms |
| uom_sales | character varying(10) |  |  | Default unit of measure to be used for sales - links to inventory_uoms |
| current_po | character varying(10) |  |  | Last Purchase Order number issued for this item |
| min_buy_qty | numeric(11) |  |  | Minimum quantity that can be purchased from vendor |
| po_due_date | date |  |  | Due date of last Puchase Order issued for this item |
| discountable | boolean |  |  | Item Discountable flag; FALSE = No TRUE = Yes |
| serialized | boolean | Yes |  | Serialized flag; FALSE = No TRUE = Yes |
| sales_acct | smallint |  |  | Sales department assigned to set General Ledger Accounts - links to sales_departments.id blank or 0 will use Company Settings, General Ledger, Special Accounts, Special Accounts, Sales, Cost of Goods and Inventory defaults |
| onhand_qty | numeric(15,5) |  |  | Quantity on hand for this item |
| reorder_qty | numeric(15,5) |  |  | Reorder Point |
| committed_qty | numeric(15,5) |  |  | Committed quantity - set by Sales Order shipped quantity, Production Order commited quantity and Inventory Transfer quantity |
| backorder_qty | numeric(15,5) |  |  | Back Ordered quantity - set by Sales Order back ordered quantity |
| purchase_qty | numeric(15,5) |  |  | Quantity on; - issued Purchase Orders - remaining quantity on Pending and In Progress Production Orders - Inventory Transfers |
| alt_part_no | character varying(40) |  |  | Alternate part numberto be used when this part number is out of stock or On Hold |
| misc_1 | character varying(40) |  |  | Inventory type - links to record_types.user_type where record_types.link_tbl='INV' |
| misc_2 | numeric(15,5) |  |  | Miscellaneous numeric field |
| type | character varying(1) |  |  | Inventory type; K - Kit, M - Manufactured, N - Normal, R - Raw material, V - Non Physical (On Hand, Committed and On Order not tracked), C - Macro |
| image_path | character varying(261) |  |  | (DEPRECATED) Path to a graphic image file |
| upload | boolean |  |  | Flag for upload to ecommerce site; TRUE = Yes FASLE = No |
| allow_back_orders | boolean |  |  | Back orders allowed flag; FALSE = No TRUE = Yes |
| allow_returns | boolean |  |  | Allow returns flag; FALSE = No TRUE = Yes |
| preferred_vendor | character varying(20) |  |  | Preferred vendor code to use for Requistions and Purchase Orders - links to vendors.vendor_no |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| pack_size | numeric(9,3) |  |  | Usual pack size - reference use only |
| color_text | bigint | Yes |  | Text colour code for display of description |
| color_back | bigint | Yes |  | Background colour code for display of description |
| levy_code | character varying(3) |  |  | Levy code assigned to this item - links to inventory_levy_codes.code |
| lot_numbered | boolean | Yes |  | Lot numbered flag; FALSE = No TRUE = Yes |
| duty_perc | numeric(7,2) |  |  | Default Duty percentage to add to Purchase Order for this item. |
| freight_perc | numeric(7,2) |  |  | Default Freight percentage to add to Purchase Order for this item. |
| std_cost | numeric(15,5) |  |  | Standard cost as sey by a user |
| last_serial | character varying(40) |  |  | Last serial number set for this item - used by Auto Generate when receiving |
| dflt_expiry_days | integer |  |  | Default number of days from today for new lot number expiry |
| expiry_required | boolean | Yes |  | Lot expiry date required flag |
| hs_code | character varying(27) |  |  | Harmonized System code for import / export use |
| mfg_country | character varying(3) |  |  | Country of manufacture/origin for import/export |
| rental_whse | character varying(6) |  |  | Used by 3rd Party development |
| rental_part_no | character varying(34) |  |  | Used by 3rd Party development |
| rental_description | character varying(80) |  |  | Used by 3rd Party development |
| lot_consume_type | smallint |  |  | Lot number consume type; 0 = User choice from selection dialog 1 = Auto consume by date received 2 = Auto consume in Alpha Numeric order 3 = Auto consume in Expiry Date order |
| tax_flags | boolean[] | Yes |  | Taxable flags for Sales tax 1 - 4; FALSE = No TRUE = Yes |
| extended_description | text |  |  | Extended description |
| udf_data | jsonb |  |  | User defined data for this record |
| last_modified | timestamp without time zone |  |  | Date/Time this record was last modified by Spire process, not user edit. |
| last_year_qty | numeric(15,5) |  |  | Total quantity sold last fiscal year |
| last_year_revenue | numeric(15,5) |  |  | Total value sold last fiscal year |
| this_year_qty | numeric(15,5) |  |  | Total quantity sold this fiscal year |
| this_year_revenue | numeric(15,5) |  |  | Total value sold this fiscal year |
| next_year_qty | numeric(15,5) |  |  | Total quantity sold next fiscal year |
| next_year_revenue | numeric(15,5) |  |  | Total value sold next fiscal year |
| last_count_date | date |  |  | Date that last Inventory Count was posted for this item |
| last_count_qty | numeric(15,5) |  |  | Quantity counted at last Inventory Count |
| last_count_variance | numeric(15,5) |  |  | Amount adjusted at last Inventory Count |
| show_options | boolean |  |  | Show options flag for Kitted items |
| last_sale_date | date |  |  | Date this items was last sold |
| last_receipt_date | date |  |  | Date this item was last received |

**Constraints:**
- `inventory_pkey` (Primary key): (id)
- `inventory_levy_code_fkey` (Foreign key): (levy_code) REFERENCES public.inventory_levy_codes (code) MATCH SIMPLE ON UPDATE CASCADE ON DELETE SET NULL
- `inventory_preferred_vendor_fkey` (Foreign key): (preferred_vendor) REFERENCES public.vendors (vendor_no) MATCH SIMPLE ON UPDATE CASCADE ON DELETE SET NULL
- `inventory_product_code_fkey` (Foreign key): (product_code) REFERENCES public.inventory_product_codes (product_code) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION
- `inventory_sales_acct_fkey` (Foreign key): (sales_acct) REFERENCES public.sales_departments (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION
- `inventory_uom_inventory_fkey` (Foreign key): (uom_inventory, id) REFERENCES public.inventory_uoms (uom, inventory_id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION DEFERRABLE INITIALLY DEFERRED
- `inventory_uom_purchase_fkey` (Foreign key): (uom_purchase, id) REFERENCES public.inventory_uoms (uom, inventory_id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION DEFERRABLE INITIALLY DEFERRED
- `inventory_uom_sales_fkey` (Foreign key): (uom_sales, id) REFERENCES public.inventory_uoms (uom, inventory_id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION DEFERRABLE INITIALLY DEFERRED
- `inventory_whse_fkey` (Foreign key): (whse) REFERENCES public.inventory_warehouses (whse) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION

### inventory_adjustments

Table for Inventory Adjustments

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| adjustment_no | character varying(10) |  |  | Adjustment number for this batch - links to inventory_adjustment_items.adjustment_no, gets written to purchase_history_items.po_number when posted. |
| location | character varying(24) |  |  | General Ledger Location segment of the Destination warehouse for companies using Location accounting |
| division | character varying(3) |  |  | Division that this adjustment is to be posted to, 000 for single division companies |
| date | date |  |  | Adjustment Date for the transaction |
| ref_no | character varying(20) |  |  | User entered reference number |
| notes | text |  |  | User notes |
| posted | boolean | Yes |  | true for posted adjustment |
| trans_no | varchar(10) |  |  | GL Transaction no for posted adjustment |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |

**Constraints:**
- `` (): 

### inventory_adjustment_items

Table for items in Inventory Adjustments

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| adjustment_id | integer | Yes |  | Link to inventory_adjustments.id |
| sequence | integer | Yes |  | Sequential line number for the adjustment batch |
| inventory_id | integer | Yes |  | Link to inventory.id |
| uom_inventory | character varying(10) |  |  | The stocking unit of measure of the item |
| uom_receive | character varying(10) |  |  | The unit of measure being used for this adjustment |
| qty | numeric(15,5) |  |  | Quantity being adjusted |
| qty_uom_inventory | numeric(15,5) |  |  | The amount that will be changed in inventory.onhand_qty using the inventory unit of measure |
| user_cost_flag | boolean |  |  | User override cost flag; FALSE = No TRUE = Yes |
| cost | numeric(15,5) |  |  | Item cost to use for the adjustment using the receive unit of measure |
| gl_account | character varying(24) |  |  | General Ledger Account that adjustment wil post to, defaults to Company Settings, Inventory, Receiving/Transfers, Default Adjustment Account |
| sell01 | numeric(15,5) |  |  | Selling Price 01 for this item. It is editable and will update the sell price in inventory_umos.sell_prices[1] when this Adjustment is posted. |
| user_sell01_flag | boolean |  |  | User override sell price flag; FALSE = No TRUE = Yes |
| guid | character varying(32) |  |  | Unique identifier for this record - links to other tables like inventory_serial_transactions.guid |
| whse_location | character varying(20) |  |  | Physical location the item is stored in the warehouse. This field is editable by user and will update table inventory_uoms.whse_location when posted |
| pack_size | numeric(9,3) |  |  | Pack size for this item, field is editable by user and will update inventory.pack_size when this Adjustment is posted |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| reference | character varying(20) |  |  | Reference number as entered by user |
| comment | text |  |  | Comment as entered by user |

**Constraints:**
- `bve_inv_adj_dtl_pkey` (Primary key): (id)
- `inventory_adjustment_fk` (Foreign key): (adjustment_no) REFERENCES public.inventory_adjustments (adjustment_no) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION
- `inventory_adjustment_items_adjustment_no_sequence_key` (Unique): (adjustment_no, sequence) DEFERRABLE INITIALLY IMMEDIATE

### inventory_comments

Table for Comments to be used on Sales Orders in the place of Inventory

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| code | character varying(20) |  |  | Code for the comment |
| description | character varying(60) |  |  | Brief description for the comment |
| comments | text |  |  | The extended comment which can be a paragraph or more |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |

**Constraints:**
- `inventory_comments_pkey` (Primary key): (id)

### inventory_components

Table for Kit and Macro Inventory components

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| sequence | integer |  |  | Sequential line number |
| whse | character varying(6) |  |  | Component warehouse - links to inventory.whse |
| part_no | character varying(34) |  |  | Component Part number - links to inventory.part_no |
| description | character varying(80) |  |  | Component description from inventory.description |
| qty | numeric(15,5) |  |  | Component quantity for this item |
| uom_usage | character varying(10) |  |  | Unit of measure used for the component |
| cost | numeric(15,5) |  |  | Not used |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | UTC User initials that last modified this record |
| parent_id | integer | Yes |  | The inventory.id of the parent item for this group of components - links to inventory.id |
| inventory_id | integer |  |  | The inventory.id of this item - links to inventory.id |
| price | numeric(15,5) |  |  | Not used |

**Constraints:**
- `bom_sub_level_pkey` (Primary key): (id)
- `inventory_components_inventory_id_fkey` (Foreign key): (inventory_id) REFERENCES public.inventory (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION
- `inventory_components_parent_id_fkey` (Foreign key): (inventory_id) REFERENCES public.inventory (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE CASCADE

### inventory_counts

Table for Inventory Count batches

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| count_no | character varying(10) | Yes |  | Batch number for this Inventory Count |
| start_date | date |  |  | Start date for count |
| finish_date | date |  |  | Finish date for count |
| total_unit_variance | numeric(15,5) |  |  | Total variance quantity for this batch |
| total_gain_loss | numeric(15,5) |  |  | The total amount of loss (negative) or gain for this batch |
| location | character varying(24) |  |  | General Ledger Location segment of the warehouse for companies using Location accounting |
| posted | boolean | Yes |  | Batch posted flag; FALSE = No TRUE = Yes |
| trans_no | character varying(10) |  |  | General Ledger transaction number for posting |
| notes | text |  |  | User added notes for this batch |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |

**Constraints:**
- `inventory_counts_pkey` (Primary key): (id)

### inventory_count_details

Table for Inventory Count item records, one record for each count entered, for every part number counted

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| sequence | integer | Yes |  | Sequencial record number |
| count_id | integer | Yes |  | Link to the counter header table - links to inventory_counts.id |
| inventory_id | integer | Yes |  | The inventory.id of this item - links to inventory.id |
| serial_id | integer |  |  | The serial number record for this item - links to inventory_serial_numbers.id |
| parent_id | integer |  |  | The id of the parent record for all of the counts belonging to a single part number |
| guid | character varying(32) |  |  | A unigue record identifier for linking to other tables like inventory_serial_transactions, inventory_receipts |
| count_time | timestamp without time zone |  |  | Date and time that this count record was recorded |
| uom | character varying(10) |  |  | Unit of measure recorded with this count record, may be different then inventory.uom_inventory |
| whse_location | character varying(20) |  |  | Physical location in the warehouse where part number was counted |
| expected_qty | numeric(15,5) |  |  | The onhand quantity that was expected to exist, inventory.onhand_qty when the Invoice Count was created (Frozen) |
| committed_qty | numeric(15,5) |  |  | The committed quantity that was in inventory.committed_qty when the Inventory Count was created (Frozen) |
| counted_qty | numeric(15,5) |  |  | The count quanitity for this record |
| inventory_qty | numeric(15,5) |  |  | The count quantity for this record in the inventory unit of measure |
| cost | numeric(15,5) |  |  | The average cost of this item |
| unit_variance | numeric(15,5) |  |  | The variance quantity for this item |
| gain_loss | numeric(15,5) |  |  | The amount of loss (negative) or gain for this item |
| notes | text |  |  | User added notes for this record |
| count_user | character varying(3) |  |  | The initials of the user that recorded the count for this record |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |

**Constraints:**
- `inventory_count_details_pkey` (Primary key): (id)
- `inventory_count_details_count_id_fkey` (Foreign key): (count_id) REFERENCES public.inventory_counts (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION
- `inventory_count_details_inventory_id_fkey` (Foreign key): (inventory_id) REFERENCES public.inventory (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION
- `inventory_count_details_serial_id_fkey` (Foreign key): (serial_id) REFERENCES public.inventory_serial_numbers (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION

### inventory_images

Images attached to an inventory item.

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| inventory_id | integer | Yes |  | Primary key of related inventory |
| sequence | integer | Yes |  | Logical ordering of images |
| name | character varying | Yes |  | Image filename |
| url | character varying | Yes |  | URL to image (if applicable) |

**Constraints:**
- `inventory_images_pkey` (Primary key): (id)

### inventory_labels

Schema for temp table created when printing Inventory labels

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| whse | character varying(6) |  |  | Warehouse - links to inventory.whse |
| part_no | character varying(34) |  |  | Part number - links to inventory.part_no |
| stock_uom | character varying(10) |  |  | Inventory stock Unit of Measure |
| sell_uom | character varying(10) |  |  | Sell Unit of Measure |
| receipt | bigint |  |  | Receipt number links to inventory_receipts.id |
| serial | character varying(40) |  |  | Serial or Lot number - links to inventory_serial_numbers.number |
| order_no | character varying(10) |  |  | Sales order number |
| customer_no | character varying(20) |  |  | Sales order customer number |
| customer_name | character varying(60) |  |  | Sales order customer name |
| price | numeric(15, 5) |  |  | Sell price |

**Constraints:**
- `inventory_labels_pkey` (Primary key): (id)

### inventory_levy_codes

Table for Levies

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| code | character varying(3) |  |  | Levy code |
| name | character varying(60) |  |  | Levy name or description |
| amt_or_pct | boolean |  |  | Levy calculation method is amount; FALSE = Percentage TRUE = Amount |
| rate | numeric(9,3) |  |  | Amount or percentage charged on each item |
| gl_account | character varying(24) |  |  | General Ledger account to use when posting Levy charges |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |

**Constraints:**
- `bve_levy_pkey` (Primary key): (id)

### inventory_lot_trace

Schema for temp table created for records located during Lot Trace Print process

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| parent | integer |  |  | Link record for the parent of this record |
| found | boolean |  |  | Found flag; FALSE = Not found TRUE = Found |
| type | character varying(4) |  |  | Trace record type; BORC = Production Order component BHIC = Production History componet PORD = Purchase Order SHIS = Sales History Invoice SORD = Sales Order |
| whse | character varying(6) |  |  | Warehouse for record - links to inventory.whse |
| part_no | character varying(34) |  |  | Part number for record - links to inventory.part_no |
| lot_no | character varying(40) |  |  | Lot number for record - links to inventory_serial_numbers.number |
| link_type | character varying(4) |  |  | Lot number found on record type; Trace record type; BORD = Production Order BHIS = Production Histort build PORD = Purchase Order SHIS = Sales History Invoice SORD = Sales Order |
| link_no | character varying(10) |  |  | Lot number found on reference number - Purchase Order number, Sales Order number, Invoice number or Production Order number |
| link_guid | character varying(32) |  |  | GUID for link to reference line |
| receipt_no | bigint |  |  | Link to inventory_receipts.id |
| receipt_date | date |  |  | Receipt date recorded on record |
| unit_cost | numeric(15,5) |  |  | Cost recorded on record |
| received_qty | numeric(15,5) |  |  | Received quantity recorded on record |
| sales_qty | numeric(15,5) |  |  | Sales quantity recorded on record |

**Constraints:**
- `inventory_lot_trace_pkey` (Primary key): (id)

### inventory_part_no_changes

Table for Part Number changes saved but not yet applied

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| whse | character varying(6) |  |  | Warehouse of item current part number to change |
| old_part_no | character varying(34) |  |  | Current part number to change |
| new_part_no | character varying(34) |  |  | New part number to assign |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |

**Constraints:**
- `bve_part_change_pkey` (Primary key): (id)

### inventory_price_levels

Table for Inventory Price Levels

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| code | character varying(20) | Yes |  | Price Level code |
| description | character varying | Yes |  | Price Level description |
| currency | character varying(3) | Yes |  | Price Level currency code |
| color_text | bigint | Yes |  | Foreground color |
| color_back | bigint | Yes |  | Background color |
| main | boolean | Yes |  | Main Price Level indicator |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |

### inventory_price_matrix

Table for Price Matrix records

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| whse | character varying(6) |  |  | Warehouse for the price, if applicable - links to inventory.whse |
| part_no | character varying(34) |  |  | Part number for price, if applicable - links to inventory.part_no |
| uom_code | character varying(10) |  |  | Unit of measure code for price, if applicable - links to inventory_uoms.uom |
| product_code | character varying(10) |  |  | Product code for price, if applicable - links to inventory.product_code |
| cust_no | character varying(20) |  |  | Customer code for price, if applicable - links to customer.cust_no |
| ship_id | character varying(20) |  |  | Customer Shipto ID for price, if applicable - links to addresses.ship_id |
| territory | character varying(10) |  |  | Territory for price, if applicable - links to addresses.sales_terr |
| cust_udef2 | character varying(40) |  |  | Customer type for price, if applicable - links to customers.misc1 |
| min_qty | numeric(15,5) |  |  | Minimum quantity required for price |
| start_date | date |  |  | Start date for this price to be applicable |
| end_date | date |  |  | End date for this price to be applicable |
| override | boolean |  |  | High Priority has been checked on this price causing this price to override all other prices, even if there is a lower one; FALSE = No TRUE = Yes |
| score | smallint |  |  | Score given to the price used to determine which price is used at Sales time |
| promo_code | character varying(25) |  |  | Promotion code or price peason added by the user |
| amount_type | character varying(1) |  |  | How the sell price is calculated; D = Discount M = Margin P = Price |
| cost_method | character varying(1) |  |  | Inventory cost to use for margin calculation |
| amount | numeric(15,5) |  |  | Used with amount_type to calculate the price |
| vendor_cost | numeric(15,5) |  |  | Vendor contract cost for this item, for this promotion |
| vendor_no | character varying(20) |  |  | Vendor number giving the promotional cost - links to vendors.vendor_no |
| currency | character varying(3) |  |  | Currency code |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |

**Constraints:**
- `inventory_price_matrix_pkey` (Primary key): (id)
- `inventory_price_matrix_uniq` (Primary key): (COALESCE(whse, ''), COALESCE(part_no, ''), COALESCE(product_code, ''), COALESCE(uom_code, ''), COALESCE(cust_no, ''), COALESCE(ship_id, ''), COALESCE(territory, ''), COALESCE(cust_udef2, ''), COALESCE(currency, ''), COALESCE(min_qty, 0), COALESCE(start_date, '-infinity'), COALESCE(end_date, 'infinity'))

### inventory_product_codes

Table for Inventory product groups

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| product_code | character varying(10) |  |  | Product code of this product group |
| description | character varying(80) |  |  | Product group description |
| default_margin | numeric(9,2) |  |  | Low margin limit to to trigger low margin alert |
| sales_acct | integer |  |  | Default sales department normally used for this product group |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| surcharge_pct | numeric(7,2) | Yes |  | Surcharge or shop supply percentage to charge for this product code |
| udf_data | jsonb |  |  | User defined data for this record |

**Constraints:**
- `product_code_pkey` (Primary key): (id)

### inventory_promo_codes

Table of promotional codes used by Price Matrix

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| promo_code | character varying(25) |  |  | Promotional or price reason code |
| description | character varying(35) |  |  | Description of promotion |
| color_text | bigint |  |  | Text colour code for display |
| color_back | bigint |  |  | Backround colour code for display |
| cumulative | boolean |  |  | Price 'Cumulative' flag; FALSE = No TRUE = Yes |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |

**Constraints:**
- `bve_promo_code_pkey` (Primary key): (id)

### inventory_receipts

Table for Inventory Receipts through Purchase Orders, Inventory Adjustments/Transfers, Production Orders, Sales Order returns, each record includes FIFO/LIFO information

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | bigint | Yes | Yes | Unique identifier and receipt number |
| receive_date | date |  |  | Date of inventory receipt |
| vendor_no | character varying(20) |  |  | Vendor code item received from- links to vendors.vendor_no |
| whse_location | character varying(20) |  |  | Warehouse physical location at time of receipt |
| qty | numeric(15,5) |  |  | Quantity received |
| cost | numeric(19,5) |  |  | Cost of part number received |
| selling | numeric(15,5) |  |  | Sell price at time of receipt |
| link_no | character varying(10) |  |  | The document number that posted receipt; Adjustment number Production Order number Purchase Order number Sales Order number (customer return) Transfer number |
| remaining_qty | numeric(15,5) |  |  | Remaining quantity available at this cost when using FIFO or LIFO cost |
| ref_no | character varying(20) |  |  | Reference number from the Purchase Order or Inventory Adjustment |
| uom_inventory | character varying(10) |  |  | Inventory or stock unit of measure |
| uom_receive | character varying(10) |  |  | Receipt unit of measure |
| qty_receive_uom | numeric(15,5) |  |  | Quantity received of the purchase unit of measure |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| link_table | character varying(4) |  |  | The module that received the item; BHIS = Production Order PORD = Purchase Order or Adjustment SORD = Sales Order (customer return) |
| inventory_id | integer | Yes |  | Inventory item for this receipt - links to inventory.id |
| link_guid | character varying(32) |  |  | Link to source record; inventory_count_items.guid production_history_items.guid production_order_items.guid purchase_history_items.guid purchase_order_items.guid sales_history_items.guid sales_order_items.guid |
| new_average_cost | numeric(19,5) |  |  | New value written to inventory.average_cost at the time of this receipt |
| new_onhand_qty | numeric(15,5) |  |  | New value written to inventory.onhand_qty at the time of this receipt |

**Constraints:**
- `inventory_receipts_pkey` (Primary key): (id)
- `inventory_receipts_inventory_id_fkey` (Foreign key): (inventory_id) REFERENCES public.inventory (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE CASCADE

### inventory_requisitions

Table for records created by Sales Orders, Production Orders or Inventory Requisitions, used to create Production Orders or Purchase Orders

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| requisition_no | character varying(10) |  |  | Requisition number |
| status | character varying(1) |  |  | Status of this record; O = Ordered on Purchase Order or Production Order R = Received on Purchase Order or Built on Production Order U = Unordered (not processed) |
| whse | character varying(6) |  |  | Warehouse of Requisition item - links to inventory.whse |
| part_no | character varying(34) |  |  | Part number of Requisition item - links to inventory.part_no |
| description | character varying(80) |  |  | Description of item. Editable description written to the Purchase Order line created |
| unit_cost | numeric(15,5) |  |  | Unit cost to use on Purchase Order - user editable |
| user_cost | boolean |  |  | User Cost Flag to indicate if user edited the cost; FALSE = No TRUE = Yes |
| ref_no | character varying(20) |  |  | Reference number from Sales Order and is editable |
| reqd_date | date |  |  | Required date that will be written to purchase order |
| reqd_qty | numeric(15,5) |  |  | Quantity required |
| received_qty | numeric(15,5) |  |  | Quantity received to date |
| uom | character varying(10) |  |  | Unit of measure to order for this item |
| src_type | character varying(4) |  |  | Source type; INV = Inventory SORD = Sales Order BORD = Production Order |
| src_no | character varying(40) |  |  | Source Production Order number or Sales Order number |
| src_guid | character varying(32) |  |  | Link to sales_ order_items.guid or production_order_items.guid from Requisition |
| src_cust_no | character varying(20) |  |  | Customer number from sales order |
| targ_type | character varying(4) |  |  | Target type; BORD = Production Order PORD = Purchase Order |
| targ_vend_no | character varying(20) |  |  | Target vendor that will be used to create Purchase Order |
| targ_no | character varying(40) |  |  | Purchase Order number or Production Order number that this requistion line created or updated |
| targ_guid | character varying(32) |  |  | Link to purchase_ order_items.guid or production_order_items.guid created by this record |
| ship_to | character varying(20) |  |  | Customer Shipping Address ID that this item was ordered for |
| drop_ship | boolean | Yes |  | Drop shipment flag |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |

**Constraints:**
- `bve_requisition_pkey` (Primary key): (id)

### inventory_sales_tax_flags

Table for Inventory Sales Tax Flags

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| inventory_id | integer |  |  | Inventory foreign key |
| levy_id | integer |  |  | Inventory foreign key |
| product_code_id | integer |  |  | Inventory foreign key |
| sales_tax_id | integer |  |  | Inventory foreign key |
| tax | boolean | Yes |  | Tax applies with a rebate |
| rebate_provinces | character varying[] | Yes |  | Provinces with provincial point-of-sale HST rebate |
| memo | character varying | Yes |  | Memo |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |

### inventory_sell_prices

Table for Inventory Sell Prices

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| inventory_id | integer | Yes |  | inventory foreign key (id) |
| uom_id | integer | Yes |  | inventory_uoms foreign key (id) |
| price_level_id | integer | Yes |  | inventory_price_levels foreign key (id) |
| price | numeric(15,5) | Yes |  | Price |
| effective_date | date |  |  | Price effective date |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |

### inventory_serial_numbers

Table for serial numbers and lot numbers

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| whse | character varying(6) | Yes |  | Warehouse for this item - links to inventory.whse |
| part_no | character varying(34) | Yes |  | Part number for this item - links to inventory.part_no |
| number | character varying(40) | Yes |  | Serial/lot number assigned |
| closed | boolean | Yes |  | Serial/lot number has been consumed by sales or production, or it has been returned to vendor |
| unit_cost | numeric(15,5) | Yes |  | Cost of this serial/lot number |
| last_received_date | date |  |  | Received date of this serial number or the last date an item with this lot number was received |
| expiry_date | date |  |  | Expiry date of this lot number |
| onhand_qty | numeric(15,5) | Yes |  | On hand quantity of this serial/lot number |
| committed_qty | numeric(15,5) | Yes |  | Commited quantity of this serial/lot number |
| temp_qty | numeric(15,5) | Yes |  | Quantity of serial or lot number that is has been selected in a Serial/Lot number dialog but the Sales Order or Production has not been saved yet |
| selling | numeric(15,5) |  |  | Sell price for this serial/lot number |
| hold | boolean | Yes |  | Serial/lot number is On Hold and cannot be selected; FALSE = Not on hold TRUE = On hold |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| pending_receipt | numeric(15,5) | Yes |  | Quantity of this serial/lot number pending receipt |
| udf_data | jsonb |  |  | User defined data for this record |

**Constraints:**
- `bve_lot_number_pkey` (Primary key): (id)

### inventory_serial_transactions

Table for serial number and lot number transactions

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| whse | character varying(6) | Yes |  | Warehouse for this record - links to inventory_serial_numbers.whse |
| part_no | character varying(34) | Yes |  | Part number for this record - links to inventory_serial_numbers.part_no |
| number | character varying(40) | Yes |  | Serial/lot number for this record - links to inventory_serial_numbers.number |
| link_type | character varying(4) | Yes |  | Source of this transaction; BORD = Production Order (Committed) BHIST = Posted Production Order (Built) PORD = Purchase Order, Inventory Adjustment SHIS = Posted Invoice SORD = Sales Order (Committed) |
| link_no | character varying(10) | Yes |  | The document number that created transaction; Adjustment number Invoice number Purchase Order number Production Order number Sales Order number |
| link_guid | character varying(32) |  |  | GUID linking to the line of the Sales, Purchse or Production Order that created this transaction |
| receipt_no | bigint |  |  | Inventory Receipt number when serial/lot was received |
| receipt_date | date |  |  | Receipt date for serial/lot number |
| unit_cost | numeric(15,5) | Yes |  | Cost of serial/lot number |
| recvd_qty | numeric(15,5) | Yes |  | Quantity of serial/lot number received, only 1 and -1 are supported for serial number records |
| sales_qty | numeric(15,5) | Yes |  | Quantity of serial/lot number sold, only 1 and -1 are supported for serial number records |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |

**Constraints:**
- `bve_lot_trans_pkey` (Primary key): (id)

### inventory_statistics

Table for Inventory Sales Statistics by finanacial period

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| whse | character varying(6) |  |  | Warehouse for the item - links to inventory.whse |
| part_no | character varying(34) |  |  | Part number for the item - links to inventory.part_no |
| year_end | date |  |  | Fiscal year end for the sales period on this record |
| period | smallint |  |  | Fiscal period (1-13) for this record |
| qty | numeric(15,3) |  |  | Quantity sold this period for this period |
| total_sell | numeric(15,3) |  |  | Total sales amount this period, for this item |
| total_cost | numeric(15,3) |  |  | Total cost for this period, for this item |

**Constraints:**
- `inventory_statistics_pkey` (Primary key): (id)

### inventory_transfer_items

Table for items in Inventory Transfers

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| transfer_id | integer | Yes |  | Link to inventory_transfers.id |
| sequence | integer | Yes |  | Sequential line number |
| src_inventory_id | integer | Yes |  | Link to inventory.id for source item |
| dst_inventory_id | integer | Yes |  | Link to inventory.id for destination item |
| uom_inventory | character varying(10) |  |  | The stocking unit of measure of the item |
| uom_receive | character varying(10) |  |  | The unit of measure being used for this transfer |
| qty | numeric(15,5) |  |  | Quantity being transferred |
| qty_uom_inventory | numeric(15,5) |  |  | The amount that will be changed in inventory.onhand_qty using the inventory unit of measure |
| user_cost_flag | boolean |  |  | User override cost flag; FALSE = No TRUE = Yes |
| cost | numeric(15,5) |  |  | Item cost to use for the transfer using the receive unit of measure |
| freight_by_pct | boolean |  |  | Future use |
| freight_pct | numeric(5,2) |  |  | Future use |
| freight_amt | numeric(15,5) |  |  | Future use |
| sell01 | numeric(15,5) |  |  | Selling Price 01 for this item. It is editable and will update the sell price in inventory_umos.sell_prices[1] when this Transfer is posted. |
| user_sell01_flag | boolean |  |  | User override sell price flag; FALSE = No TRUE = Yes |
| src_guid | character varying(32) |  |  | Unique identifier for source item - links to other tables like inventory_serial_transactions.guid |
| dst_guid | character varying(32) |  |  | Unique identifier for destination item - links to other tables like inventory_serial_transactions.guid |
| whse_location | character varying(20) |  |  | Physical location the item is stored in the warehouse. This field is editable by user and will update table inventory_uoms.whse_location when posted |
| pack_size | numeric(9,3) |  |  | Pack size for this item, field is editable by user and will update inventory.pack_size when this Transfer is posted |
| transfer_pct | numeric(9,2) |  |  | Used by Inventory Transfer to increase the value of the item in the receiving warehouse. - Defaults to the percentage set on the header of the Inventory Transfer - Posts to General Ledger account set up in Company Settings, Inventory, Receiving/Transfers, Transfer Markup Account |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| reference | character varying(20) |  |  | Reference number as entered by user |
| comment | text |  |  | Comment as entered by user |

**Constraints:**
- `` (): 

### inventory_transfers

Table for Inventory Transfers

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| transfer_no | character varying(10) |  |  | Transfer number |
| src_whse | character varying(6) |  |  | Source warehouse for Inventory Transfer |
| src_location | character varying(24) |  |  | General Ledger Location segment of the Source warehouse for companies using Location accounting |
| dst_whse | character varying(6) |  |  | Destination warehouse for Inventory Transfer |
| dst_location | character varying(24) |  |  | General Ledger Location segment of the Destination warehouse for companies using Location accounting |
| division | character varying(3) |  |  | Division that this Inventory Transfer is to be posted to, 000 for single division companies |
| date | date |  |  | Date for the transaction |
| ref_no | character varying(20) |  |  | User entered reference number |
| markup_pct | numeric(9,2) |  |  | Markup percentage to be added to the transfer - defaults to the percentage set up in Company Settings, Inventory, Receiving/Transfers, Default Markup Percentage - this will get added to each line of the transfer by default |
| required_date | date |  |  | Date required for transfer |
| shipping_method_id | integer |  |  | Shipping method ID |
| shipping_date | date |  |  | Ship date |
| tracking_no | character varying |  |  | Carrier tracking no |
| posted | boolean | Yes |  | true for posted transfer |
| trans_no | varchar(10) |  |  | GL Transaction no for posted transfer |
| notes | text |  |  | User notes |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |

**Constraints:**
- `` (): 

### inventory_uoms

Table for Inventory Unit of Measure records

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| whse | character varying(6) | Yes |  | Warehouse for item - links to inventory.whse |
| part_no | character varying(34) | Yes |  | Part number for item - links to inventory.part_no |
| uom | character varying(10) |  |  | Unit of measure code for this record |
| description | character varying(80) |  |  | Unit of Measure description for this record |
| qty_factor | numeric(11,5) |  |  | Factor of conversion for this unit of measure |
| direct_factor | boolean |  |  | Direct conversion factor flag; FALSE = Not direct (UOM is divided) TRUE = Direct (UOM is multiplied) |
| allow_fractional_qty | boolean |  |  | Fractional quantity permitted flag; FALSE = whole quantities only TRUE = fractional quantities permitted |
| buy_uom | boolean |  |  | Unit of Measure Purchase flag; FALSE = Not permited for Purchase TRUE = Permitted for Purchasing |
| sell_uom | boolean |  |  | Unit of Measure Sell flag; FALSE = Not permitted for Sales TRUE = Permitted for Sales |
| whse_location | character varying(20) | Yes |  | Warehouse physical location for this Unit of Measure |
| weight | numeric(11,5) |  |  | Weight of this Unit of Measure |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| sell_prices | numeric(15,5)[] | Yes |  | Sell Price 1 - 20 for this Unit of Measure |
| use_price_factor | boolean | Yes |  | Calculate price from Inventory Unit of Measure using Price Factor; FALSE = No TRUE = Yes |
| price_factor | numeric(11,5) |  |  | Factor used to calculate price from Inventory stock Unit of Measure |
| inventory_id | integer | Yes |  | Inventory identifier - links to inventory.id |

**Constraints:**
- `unit_of_measure_pkey` (Primary key): (id)
- `inventory_uoms_inventory_id_fkey` (Foreign key): (inventory_id) REFERENCES public.inventory (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE CASCADE

### inventory_upc_codes

Table for Universal Product Codes link to Inventory items

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| whse | character varying(6) | Yes |  | Warehouse for this record - links to inventory.whse |
| part_no | character varying(34) | Yes |  | Part number for this record - links to inventory-part_no |
| upc_code | character varying(40) | Yes |  | Universal Product Code for this record |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| uom | character varying(10) |  |  | Unit of Measure for this record |

**Constraints:**
- `bve_upc_pkey` (Primary key): (id)

### inventory_warehouses

Table of Inventory Warehouses

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| whse | character varying(6) |  |  | Code for warehouse |
| description | character varying(60) |  |  | Warehouse name or description |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| udf_data | hstore |  |  | User defined data for this record |

**Constraints:**
- `warehouse_pkey` (Primary key): (id)
