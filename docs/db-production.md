# Database: Production & BOM

Schema: `public` (v3)

### production_orders

Table for Production Orders not yet fully built

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| order_no | character varying(10) |  |  | Production Order number |
| whse | character varying(6) |  |  | Warehouse of the manufactured item to be built - links to inventory.whse |
| part_no | character varying(34) |  |  | Part number of the manufactured item to be built - links to inventory.part_no |
| description | character varying(80) |  |  | Description of the manufactured item to be built, can be user edited |
| status | character varying(1) |  |  | Status of the Production Order; A = Pending I = In Progress U = Open |
| lead | numeric(5) |  |  | Lead time to build item as set by user |
| order_date | date |  |  | Production Order date |
| required_date | date |  |  | Required date on the Production Order |
| required_qty | numeric(13,5) |  |  | Quantity required of the manufactured item |
| backorder_qty | numeric(13,5) |  |  | Quantity still left to build on this Production Order |
| parts_cost | numeric(17,3) |  |  | Total unit cost of components on this Production Order |
| cust_no | character varying(20) |  |  | Customer that was assigned to this Production Order - links to customer.cust_no |
| category | character varying(6) |  |  | Category assigned to this build - links to bom_categories.category |
| priority | smallint |  |  | Priority that the Production Order was as set by user; 0 - Highest 1 - High 2 - Normal 3 - Low 4 - Lowest |
| expected_yield_pct | numeric(5,2) |  |  | Expected yield percentage |
| actual_yield_qty | numeric(13,5) |  |  | Actual yield quantity |
| assemble_qty | numeric(17,5) |  |  | Quantity to assemble using expected yield |
| assembled_qty | numeric(17,5) |  |  | Quantity already built |
| sales_order_no | character varying(10) |  |  | Sales Order number that this Production Order is for - links to sales_orders.order_no |
| cust_po_no | character varying(20) |  |  | Customer Purchase Order number |
| phase_date | date |  |  | The Phase Date on the Production Order |
| guid | character varying(32) |  |  | Uniique identifier for linking to other tables; sales_order_items.guid inventory_serial_transactions.link_guid inventory_receipts.link_guid |
| reference | character varying(30) |  |  | User added reference number |
| revision | character varying(8) |  |  | The revision of the Production Template when the Production Order was created or updated |
| phase_id | character varying(20) |  |  | Phase ID on the Production Order |
| req_no | character varying(10) |  |  | Requisition number that created this Production Order - links to inventory_requisitions.requisition_no |
| employee | character varying(6) |  |  | Employee number added by user - links to employees.employee_no |
| template_no | character varying(10) |  |  | Production Template that was used to create this Production Order |
| template_guid | character varying(32) |  |  | The Template GUID linked to this Production Order - links to production_templates.guid |
| user_flag | boolean |  |  | User edited flag; FALSE = No, record still same as populated from template TRUE = User modified this record since it was created from the template |
| committed_qty | numeric(21,5) |  |  | The Inventory committed quantity by this Production Order - effects inventory.purchase_qty |
| weight | numeric(15, 2) |  |  | Unit weight |
| user_weight | boolean |  |  | User modified weight |
| instructions | text |  |  | Manufacturing instructions for the Production Order |
| mfg_notes | text |  |  | Manufacturing notes for the Production Order |
| division | character varying(3) |  |  | Division code |
| udf_data | jsonb |  |  | User defined data for this record |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |

**Constraints:**
- `bve_bombuild_pkey` (Primary key): (id)

### production_order_items

Table for Prodution Order component details

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| sequence | smallint |  |  | Sequential number for order line |
| parent | smallint |  |  | Link to production_order_items.id for the parent of this record |
| whse | character varying(6) |  |  | Inventory warehouse for this record - links to inventory.whse |
| part_no | character varying(34) |  |  | Inventory part number for this record - links to inventory.part_no |
| description | character varying(80) |  |  | Description of the component |
| unit_qty | numeric(15,5) |  |  | Quantity required of this component to build 1 of the sub-assembly or assembly, 1 level above |
| scrap_qty | numeric(21,5) |  |  | Actual scrap quantity |
| extended_qty | numeric(21,5) |  |  | Extended quantity of the component |
| committed_qty | numeric(21,5) |  |  | Committed quantity of the component, affects inventory.committed_qty |
| consumed_qty | numeric(21,5) |  |  | Consumed quantity of the component as per partial builds |
| unit_cost | numeric(15,5) |  |  | Cost of the component |
| inventory_cost | numeric(15,5) |  |  | Inventory cost of the component |
| user_cost_flag | boolean |  |  | User edited cost flag; FALSE = No TRUE = Yes |
| uom_usage | character varying(10) |  |  | Unit of Measure to use for this item |
| uom_usage_factor | numeric(13,5) |  |  | Unit of Measure conversion factor to use for this record, 0 or 1 = no conversion |
| direct_factor | boolean |  |  | Use UOM direct factor for conversion; TRUE= multiply FALSE = divide |
| lead_time | numeric(5) |  |  | Lead time of the item as set by user |
| date | date |  |  | Order date, editable by user |
| category | character varying(6) |  |  | Category assigned to this component - links to bom_categories.category |
| expected_scrap_pct | numeric(5,2) |  |  | Expected scrap percentage of the component |
| guid | character varying(32) |  |  | Uniique identifier for linking to other tables; inventory_serial_transactions.link_guid inventory_receipts.link_guid inventory_requisitions_targ_guid |
| req_no | character varying(10) |  |  | Requisition number that was created by this record - links to inventory_requisitions.requisition_no |
| vend_no | character varying(20) |  |  | Vendor code that will be used for Inventory Requisition |
| employee | character varying(6) |  |  | Employee number added to the line - links to employees.employee_no |
| template_no | character varying(10) |  |  | Template that was used when creating this record |
| template_guid | character varying(32) |  |  | Template GUID that was used to create this line - links to production_template_items.guid |
| wip_account_no | character varying(24) |  |  | Work in process GL account no |
| wip_built | date |  |  | Work in process build date |
| phase_id | character varying(20) |  |  | Work in process phase |
| user_flag | boolean |  |  | Not used |
| weight | numeric(15, 2) |  |  | Unit weight |
| user_weight | boolean |  |  | User modified weight |
| revision | character varying(8) |  |  | The revision number of the template that created or last updated this record |
| _dbversion | integer |  |  | Unique identifier |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| instructions | text |  |  | Component instruction notes |
| order_id | integer | Yes |  | Link to production_order.id |
| udf_data | jsonb |  |  | User defined data for this record |

**Constraints:**
- `bve_bombuild_dtl_pkey` (Primary key): (id)
- `production_order_items_order_id_fkey` (Foreign key): (order_id) REFERENCES public.production_orders (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE CASCADE

### production_templates

Table for Production Templates

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| template_no | character varying(10) |  |  | Template number |
| whse | character varying(6) |  |  | Warehouse of the manufactured item - links to inventory.whse |
| part_no | character varying(34) |  |  | Part number of the manufactured item - links to inventory.part_no |
| description | character varying(80) |  |  | Description of the manufactureded item, can be edited by user |
| status | character varying(1) |  |  | Not used |
| lead | numeric(5) |  |  | Lead time to build item as set by user |
| cust_no | character varying(20) |  |  | Customer that was assigned to this Production Template - links to customer.cust_no |
| category | character varying(6) |  |  | Category assigned to this build - links to bom_categories.category |
| priority | smallint |  |  | Priority that the Production Order was as set by user; 0 - Highest 1 - High 2 - Normal 3 - Low 4 - Lowest |
| expected_yield_pct | numeric(5,2) |  |  | Expected yield percentage |
| required_qty | numeric(13,5) |  |  | Default quantity required of the manufactured item |
| guid | character varying(32) |  |  | Uniique identifier for linking to production_orders |
| reference | character varying(30) |  |  | Default reference number |
| revision | character varying(8) |  |  | The revision number set by user when the Template was last edited and saved |
| employee | character varying(6) |  |  | Default employee number set on the line - links to employees.employee_no |
| phase_id | character varying(20) |  |  | Starting phase |
| weight | numeric(15, 2) |  |  | Unit weight |
| user_weight | boolean |  |  | User modified weight |
| instructions | text |  |  | Manufacturing instructions for the manufactured item |
| mfg_notes | text |  |  | Manufacturing notes for the manufactured item |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| default_flag | boolean |  |  | Flag to assign this template as default when manufactured item is set on a Production Order; FALSE = No TRUE = Yes |
| udf_data | hstore |  |  | User defined data for this record |

**Constraints:**
- `production_templates_pkey` (Primary key): (id)

### production_template_items

Table for Prodution Template component details

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| template_id | integer | Yes |  | Link to production_templates.id |
| child_template_id | integer |  |  | Link to production.template_id for sub assemblies |
| sequence | smallint |  |  | Sequential number for order line |
| whse | character varying(6) |  |  | Inventory warehouse for this record - links to inventory.whse |
| part_no | character varying(34) |  |  | Inventory part number for this record - links to inventory.part_no |
| description | character varying(80) |  |  | Description of component |
| unit_qty | numeric(15,5) |  |  | Quantity required of this component to build 1 of the sub-assembly or assembly, 1 level abov |
| unit_cost | numeric(15,5) |  |  | Cost of the component |
| uom_usage | character varying(10) |  |  | Unit of Measure to use for this item |
| lead_time | numeric(5) |  |  | Lead time of the item as set by user |
| category | character varying(6) |  |  | Category assigned to this component - links to bom_categories.category |
| expected_scrap_pct | numeric(5,2) |  |  | Expected scrap percentage of the component |
| guid | character varying(32) |  |  | Uniique identifier for linking to production_order_items |
| vend_no | character varying(20) |  |  | Vendor code that will be used for Inventory Requisition |
| employee | character varying(6) |  |  | Employee number added to the line - links to employees.employee_no |
| revision | character varying(8) |  |  | The revision number set by user when the Template was last edited and saved |
| instructions | text |  |  | Component instruction notes |
| wip_account_no | character varying(24) |  |  | Work in process GL account no |
| phase_id | character varying(20) |  |  | Work in process phase |
| weight | numeric(15, 2) |  |  | Unit weight |
| user_weight | boolean |  |  | User modified weight |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| parent_item_id | integer |  |  | Link to production_order_items.id for the parent of this record |
| udf_data | jsonb |  |  | User defined data for this record |

**Constraints:**
- `production_template_items_pkey` (Primary key): (id)
- `production_template_items_child_template_fkey` (Foreign key): (child_template_id) REFERENCES public.production_templates (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE SET NULL
- `production_template_items_parent_id_fkey` (Foreign key): (template_id, parent_item_id) REFERENCES public.production_template_items (template_id, id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE CASCADE
- `production_template_items_template_id_fkey` (Foreign key): (template_id) REFERENCES public.production_templates (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE CASCADE
- `production_template_items_template_id_id_key` (Unique): (template_id, id)

### production_history

Table for Production Orders that have been built

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| order_no | character varying(10) |  |  | Production Order number |
| history_no | character varying(10) |  |  | Build number, note that each Production Order can be partially built more than once |
| trans_no | character varying(10) |  |  | Build transaction no |
| build_date | date |  |  | Date item was built |
| whse | character varying(6) |  |  | Warehouse of the manufactured item that was built - links to inventory.whse |
| part_no | character varying(34) |  |  | Part number of the manufactured item that was built - links to inventory.part_no |
| description | character varying(80) |  |  | Description of item built, may be user edited |
| status | character varying(1) |  |  | Status of the Production Order at build time; A = Pending I = In Progress U = Open |
| lead | numeric(5) |  |  | Lead time of the item as set on the Production Order |
| order_date | date |  |  | Production Order date |
| required_date | date |  |  | Required date on the Production Order |
| required_qty | numeric(13,5) |  |  | Quantity required on the Production Order |
| backorder_qty | numeric(13,5) |  |  | Quantity still left to build on this Production Order |
| parts_cost | numeric(17,3) |  |  | Total unit cost of components on this Production Order |
| cust_no | character varying(20) |  |  | Customer that was assigned to this Production Order - links to customer.cust_no |
| category | character varying(6) |  |  | Category assigned to this build - links to bom_categories.category |
| priority | smallint |  |  | Priority that the Production Order was set to when it was built; 0 - Highest 1 - High 2 - Normal 3 - Low 4 - Lowest |
| expected_yield_pct | numeric(5,2) |  |  | Expected yield percentage |
| actual_yield_qty | numeric(13,5) |  |  | Actual yield quantity |
| assembled_qty | numeric(17,5) |  |  | Quantity that were built |
| sales_order_no | character varying(10) |  |  | Sales Order number that this Production Order is for - links to sales_orders.order_no or sales_history.order_no |
| cust_po_no | character varying(20) |  |  | Customer Purchase Order number |
| phase_date | date |  |  | The Phase Date on the Production Order when built |
| guid | character varying(32) |  |  | Uniique identifier for linking to other tables; sales_order_items.guid inventory_serial_transactions.link_guid inventory_receipts.link_guid |
| reference | character varying(30) |  |  | User added reference number |
| weight | numeric(15, 2) |  |  | Unit weight |
| user_weight | boolean |  |  | User modified weight |
| revision | character varying(8) |  |  | The revision of the Production Template when the Production Order was created or updated |
| phase_id | character varying(20) |  |  | Phase ID on the Production Order when built |
| req_no | character varying(10) |  |  | Requisition number that created this Production Order - links to inventory_requisitions.requisition_no |
| employee | character varying(6) |  |  | Employee number added by user - links to employees.employee_no |
| template_no | character varying(10) |  |  | Production Template that was used to create this Production Order |
| template_guid | character varying(32) |  |  | The Template GUID linked to this Production Order |
| user_flag | boolean |  |  | User edited flag; FALSE = No, record still same as populated from template TRUE = User modified this record since it was created from the template |
| committed_qty | numeric(21,5) |  |  | The Inventory committed quantity at build time |
| instructions | text |  |  | Instructions for the top assembly |
| mfg_notes | text |  |  | Manufacturing notes added by user |
| division | character varying(3) |  |  | Division code |
| udf_data | jsonb |  |  | User defined data for this record |
| _dbversion | integer |  |  | Program version that last modified this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was created |
| _modified_by | character varying(3) |  |  | User initials that created this record |
| _created | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _created_by | character varying(3) |  |  | User initials that last modified this record |

**Constraints:**
- `production_history_pkey` (Primary key): (id)

### production_history_items

Table for built Prodution Order component details

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| order_id | integer | Yes |  | Link to production_history.id |
| sequence | smallint |  |  | Sequential number for order line |
| parent_item_id | integer |  |  | Link to production_history_items.id for the parent of this record |
| whse | character varying(6) |  |  | Inventory warehouse for this record - links to inventory.whse |
| part_no | character varying(34) |  |  | Inventory part number for this record - links to inventory.part_no |
| description | character varying(80) |  |  | Inventory description for this record |
| unit_qty | numeric(15,5) |  |  | Unit quantity |
| scrap_qty | numeric(21,5) |  |  | Actual scrap quantity |
| extended_qty | numeric(21,5) |  |  | Extended quantity consumed for build |
| consumed_qty | numeric(21,5) |  |  | Consumed quantity of this item |
| unit_cost | numeric(15,5) |  |  | Cost of the component |
| user_cost_flag | boolean |  |  | User edited cost flag; FALSE = No TRUE = Yes |
| uom_usage | character varying(10) |  |  | Unit of Measure used for this item |
| uom_usage_factor | numeric(13,5) |  |  | Unit of measure conversion factor used |
| direct_factor | boolean |  |  | UOM direct factor used for conversion; TRUE= multiply FALSE = divide |
| lead_time | numeric(5) |  |  | Lead time of the item as set by user |
| date | date |  |  | Order date |
| category | character varying(6) |  |  | Category assigned to this component - links to bom_categories.category |
| expected_scrap_pct | numeric(5,2) |  |  | Expected scrap percentage |
| guid | character varying(32) |  |  | Uniique identifier for linking to other tables; sales_order_items.guid inventory_serial_transactions.link_guid inventory_receipts.link_guid |
| req_no | character varying(10) |  |  | Requisition number that was created by this record - links to inventory_requisitions.requisition_no |
| vend_no | character varying(20) |  |  | Vendor code that was to be used for Inventory Requisition |
| employee | character varying(6) |  |  | Employee number added to the line - links to employees.employee_no |
| template_no | character varying(10) |  |  | Template that was used when creating this record |
| template_guid | character varying(32) |  |  | Template GUID that was used to create this line - links to production_template_items.guid |
| wip_account_no | character varying(24) |  |  | Work in process GL account no |
| wip_built | date |  |  | Work in process build date |
| phase_id | character varying(20) |  |  | Work in process phase |
| user_flag | boolean |  |  | User edited flag; FALSE = No, record still same as populated from template TRUE = User modified this record since it was created from the template |
| weight | numeric(15, 2) |  |  | Unit weight |
| user_weight | boolean |  |  | User modified weight |
| revision | character varying(8) |  |  | The revision number of the template that created or last updated this record |
| instructions | text |  |  | Component instruction notes |
| _dbversion | integer |  |  | Program version that last modified this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was created |
| _modified_by | character varying(3) |  |  | User initials that created this record |
| _created | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _created_by | character varying(3) |  |  | User initials that last modified this record |
| udf_data | jsonb |  |  | User defined data for this record |

**Constraints:**
- `production_history_items_pkey` (Primary key): (id)
- `production_history_items_order_id_fkey` (Foreign key): (order_id) REFERENCES public.production_history (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION
- `production_history_items_parent_id_fkey` (Foreign key): (order_id, parent_item_id) REFERENCES public.production_history_items (order_id, id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION
- `production_history_items_order_id_id_key` (Unique): (order_id, id)

### bom_categories

Table for Bill of Materials categories

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| category | character varying(6) |  |  | Category code |
| description | character varying(60) |  |  | Category description |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User initials that modified this record |

**Constraints:**
- `bom_categories_pkey` (Primary key): (id)

### bom_item_replacements

Table for saved Production Order and Template Components replacements

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| status | character varying(1) |  |  | Status flag; A = Applied U = Unapplied |
| old_whse | character varying(6) |  |  | Warehouse for old (original) part number |
| old_part_no | character varying(34) |  |  | Old part number to be replaced |
| new_whse | character varying(6) |  |  | Warehouse for new (replacement) part number |
| new_part_no | character varying(34) |  |  | New replacement part number |
| update_bom | boolean |  |  | Update templates flag; FALSE = No TRUE = Yes |
| update_bo | boolean |  |  | Update Production Orders flag; FALSE = No TRUE = Yes |
| include_allocated | boolean |  |  | Update committed Production Orders components; FALSE = No TRUE = Yes |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time record was last modified |
| _modified_by | character varying(3) |  |  | User initials that modified this record |

**Constraints:**
- `bve_replace_components_pkey` (Primary key): (id)
