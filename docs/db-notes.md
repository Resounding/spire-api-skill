# Database: Notes & Attachments

Schema: `public` (v3)

### notes

Table for Communication Notes

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes |  |
| link_table | character varying(4) |  |  | Field to use for module linking; AP = Accounts Payable AR = Accounts Receivable BORD = Production Orders CUST = Customer EMP = Employee IADJ = Inventory Adjustment INV = Inventory JC = Job Cost NOTE = Communiction PORD = Purchase Order SORD = Sales Order TRAN = General Ledger Transaction VEND = Vendor |
| link_no | character varying(60) |  |  | Field to store join value for link to; vendor_no, cust_no, employee_no, job_no, purchase_no, order_no, trans_no, part_no |
| subject | character varying(60) |  |  | Subject of the communication |
| attachment_flag | boolean |  |  | (DEPRECATED) Has an attachment; FALSE = No TRUE = Yes |
| attachment_type | character varying(50) |  |  | (DEPRECATED) Type of attachment |
| attachment_path | character varying(280) |  |  | (DEPRECATED) Network path to the attachment |
| detail | text |  |  | Note text |
| due_date | date |  |  | Due date set by user |
| completed_date | timestamp without time zone |  |  | Completed date set by user |
| display_flag | character(1) |  |  | Display setting; A = Alert popup B = Both P = Print on form |
| type | character(1) |  |  | Document to print this note on; A = Packing Slip B = Booking Order C = Picking Slip I = Invoice O = Order Confirmation P = Purchase Order Q = Quote S = Sales Order W = Work Order |
| group_type | character varying(10) |  |  | Note type - links to note_types.type |
| qty | numeric(21,5) |  |  | Numeric reference value |
| _dbversion | integer |  |  | Program version that last modified this record |
| _created | timestamp without time zone |  |  | UTC Date and time record was created |
| _created_by | character varying(3) |  |  | User initials that created this record |
| _modified | timestamp without time zone |  |  | UTC Date and time this record was last modified |
| _modified_by | character varying(3) |  |  | User initials that last modified this record |
| due_time | time without time zone |  |  | Due time set by user |
| assigned_user_id | integer |  |  | User note is assigned to |

**Constraints:**
- `notes_pkey` (Primary key): (id)
- `notes_assigned_user_id_fkey` (Foreign key): (assigned_user_id) REFERENCES public.system_users_base (id) MATCH SIMPLE ON UPDATE NO ACTION ON DELETE NO ACTION

### note_attachments

Links and files attached to communication notes.

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| note_id | integer | Yes |  | Primary key of related note |
| sequence | integer | Yes |  | Logical ordering of attachments |
| name | character varying | Yes |  | Attachment filename |
| url | character varying | Yes |  | URL to attachment (if applicable) |

**Constraints:**
- `note_attachments_pkey` (Primary key): (id)

### note_types

Table for Communication Note types

| Column | Type | Not Null | PK | Comment |
|--------|------|----------|----|---------|
| id | integer | Yes | Yes | Unique identifier |
| type | character varying(10) |  |  | User set Code for the note type |
| link_table | character varying(80) |  |  | Field to use for module linking; AP = Accounts Payable AR = Accounts Receivable BORD = Production Orders CUST = Customer EMP = Employee IADJ = Inventory Adjustment INV = Inventory JC = Job Cost NOTE = Communiction PORD = Purchase Order SORD = Sales Order TRAN = General Ledger Transaction VEND = Vendor |
| description | character varying(35) |  |  | Description for the note type |

**Constraints:**
- `bve_note_type_pkey` (Primary key): (id)
