# Inventory API

Base Path: `/api/v2/companies/{company-name}`

---

## Inventory Root

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/inventory` | Get inventory root |

---

## Inventory Items

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/inventory-items` | Get list of inventory items |
| POST | `/inventory-items` | Create an inventory item |
| GET | `/inventory-items/{id}` | Get inventory item by ID |
| PUT | `/inventory-items/{id}` | Update inventory item by ID |
| DELETE | `/inventory-items/{id}` | Delete inventory item by ID |

---

## POST /inventory-items - Create Inventory Item

### Minimal Request Body

The API handles defaults automatically - you only need minimal fields:

```json
{
  "partNo": "WIDGET-A",
  "description": "Widget Type A"
}
```

### Full Request Body

```json
{
  "whse": "00",
  "partNo": "WIDGET-A",
  "description": "Widget Type A - Premium Edition",
  "type": "N",
  "status": "A",
  "groupNo": "WIDGETS",
  "salesDept": "100",
  "defaultSellPrice": "29.99",
  "avgCost": "15.00",
  "lastCost": "14.50",
  "stdCost": "15.00",
  "currentCost": "14.75",
  "weight": "2.5",
  "minQty": "10",
  "maxQty": "100",
  "reorderQty": "50",
  "sellMeasure": "EA",
  "purchaseMeasure": "EA",
  "serialized": false,
  "discountable": true,
  "allowBackorders": true,
  "foregroundColor": 0,
  "backgroundColor": 16777215,
  "udf": {
    "custom_field_1": "value",
    "custom_field_2": true
  }
}
```

### Inventory Item Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `whse` | string | No | Warehouse code (defaults to primary) |
| `partNo` | string | Yes | Part number (unique identifier) |
| `description` | string | Yes | Item description |
| `type` | string | No | Item type (N=Normal, S=Service, K=Kit, A=Assembly) |
| `status` | string | No | Status (A=Active, I=Inactive, O=Obsolete) |
| `groupNo` | string | No | Inventory group code |
| `salesDept` | string | No | Sales department code |
| `defaultSellPrice` | decimal | No | Default selling price |
| `avgCost` | decimal | No | Average cost |
| `lastCost` | decimal | No | Last purchase cost |
| `stdCost` | decimal | No | Standard cost |
| `currentCost` | decimal | No | Current cost |
| `weight` | decimal | No | Item weight |
| `minQty` | decimal | No | Minimum stock quantity |
| `maxQty` | decimal | No | Maximum stock quantity |
| `reorderQty` | decimal | No | Reorder quantity |
| `sellMeasure` | string | No | Selling unit of measure |
| `purchaseMeasure` | string | No | Purchasing unit of measure |
| `serialized` | boolean | No | Track serial numbers |
| `discountable` | boolean | No | Allow discounts |
| `allowBackorders` | boolean | No | Allow backorders |
| `udf` | object | No | User defined fields |

### Inventory Type Values

| Type | Description |
|------|-------------|
| `N` | Normal - Standard inventory item |
| `S` | Service - Non-stock service item |
| `K` | Kit - Bundle of items sold together |
| `A` | Assembly - Manufactured item with BOM |

### Inventory Status Values

| Status | Description |
|--------|-------------|
| `A` | Active |
| `I` | Inactive |
| `O` | Obsolete |

---

## PUT /inventory-items/{id} - Update Inventory Item

Use the same structure as POST. Include only fields you want to update.

```json
{
  "description": "Widget Type A - Updated Description",
  "defaultSellPrice": "34.99",
  "status": "A"
}
```

---

## Inventory Item Components

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/inventory-items/{id}/components` | Get list of item components |
| GET | `/inventory-components` | Get list of all inventory components |

---

## Inventory Item Images

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/inventory-items/{item-id}/images` | Get list of item images |
| POST | `/inventory-items/{item-id}/images` | Create item image |
| GET | `/inventory-items/{item-id}/images/{image-id}` | Get item image by ID |
| PUT | `/inventory-items/{item-id}/images/{image-id}` | Update item image by ID |
| DELETE | `/inventory-items/{item-id}/images/{image-id}` | Delete item image by ID |
| GET | `/inventory-items/{item-id}/images/{image-id}/data` | Get image data (binary) |

### POST Image

```json
{
  "filename": "widget-a.jpg",
  "data": "base64_encoded_image_data_here",
  "primary": true
}
```

---

## Inventory Item UOMs (Units of Measure)

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/inventory-items/{item-id}/uoms` | Get list of item UOMs |
| POST | `/inventory-items/{item-id}/uoms` | Create item UOM |
| GET | `/inventory-items/{item-id}/uoms/{uom-id}` | Get item UOM by ID |
| PUT | `/inventory-items/{item-id}/uoms/{uom-id}` | Update item UOM by ID |
| DELETE | `/inventory-items/{item-id}/uoms/{uom-id}` | Delete item UOM by ID |

### POST UOM

```json
{
  "code": "CS",
  "description": "Case",
  "conversionFactor": "12",
  "sellPrice": "299.99"
}
```

### UOM Sell Prices

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/inventory-items/{item-id}/uoms/{uom-id}/sell-prices` | Get list of UOM sell prices |
| POST | `/inventory-items/{item-id}/uoms/{uom-id}/sell-prices` | Create UOM sell price |
| GET | `/inventory-items/{item-id}/uoms/{uom-id}/sell-prices/{sell-price-id}` | Get UOM sell price by ID |
| PUT | `/inventory-items/{item-id}/uoms/{uom-id}/sell-prices/{sell-price-id}` | Update UOM sell price by ID |
| DELETE | `/inventory-items/{item-id}/uoms/{uom-id}/sell-prices/{sell-price-id}` | Delete UOM sell price by ID |

### POST UOM Sell Price

```json
{
  "priceLevel": {"id": 1},
  "price": "24.99",
  "minQty": "10"
}
```

---

## Inventory Item UPCs

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/inventory-items/{item-id}/upcs` | Get list of item UPCs |
| POST | `/inventory-items/{item-id}/upcs` | Create item UPC |
| GET | `/inventory-items/{item-id}/upcs/{upc-id}` | Get item UPC by ID |
| PUT | `/inventory-items/{item-id}/upcs/{upc-id}` | Update item UPC by ID |
| DELETE | `/inventory-items/{item-id}/upcs/{upc-id}` | Delete item UPC by ID |

### POST UPC

```json
{
  "upc": "012345678901",
  "uom": {"code": "EA"}
}
```

---

## Inventory Item Serial Numbers

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/inventory-items/{item-id}/serials` | Get list of item serial numbers |
| GET | `/inventory-items/{item-id}/serials/{serial-id}` | Get item serial by ID |

---

## Inventory Items UDFs

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/inventory-items-udf` | Get list of inventory items UDFs |
| GET | `/inventory-items-udf-fields` | Get list of inventory items UDF fields |
| GET | `/inventory-items-udf-fields/{field-name}` | Get UDF field by name |
| PUT | `/inventory-items-udf-fields/{field-name}` | Update UDF field by name |
| DELETE | `/inventory-items-udf-fields/{field-name}` | Delete UDF field by name |
| GET | `/inventory-items-udf-pages` | Get list of UDF pages |
| POST | `/inventory-items-udf-pages` | Create UDF page |
| GET | `/inventory-items-udf-pages/{id}` | Get UDF page by ID |
| PUT | `/inventory-items-udf-pages/{id}` | Update UDF page by ID |
| DELETE | `/inventory-items-udf-pages/{id}` | Delete UDF page by ID |

### POST UDF Page

```json
{
  "label": "Custom Properties"
}
```

### PUT UDF Field

```json
{
  "label": "Country of Origin",
  "pageId": 1,
  "sequence": 0
}
```

---

## Inventory Adjustments

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/inventory-adjustments` | Get list of inventory adjustments |
| POST | `/inventory-adjustments` | Create an inventory adjustment |
| GET | `/inventory-adjustments/{id}` | Get inventory adjustment by ID |
| PUT | `/inventory-adjustments/{id}` | Update inventory adjustment by ID |
| DELETE | `/inventory-adjustments/{id}` | Delete inventory adjustment by ID |
| POST | `/inventory-adjustments/{id}/post` | Post inventory adjustment |

### POST /inventory-adjustments - Create Adjustment

```json
{
  "whse": "00",
  "date": "2024-01-15",
  "reference": "ADJ-001",
  "items": [
    {
      "inventory": {
        "partNo": "WIDGET-A"
      },
      "qty": "10",
      "unitCost": "15.00",
      "reason": "Stock correction"
    },
    {
      "inventory": {
        "partNo": "GADGET-B"
      },
      "qty": "-5",
      "unitCost": "25.00",
      "reason": "Damaged goods"
    }
  ]
}
```

### Adjustment Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `whse` | string | No | Warehouse code |
| `date` | string | No | Adjustment date |
| `reference` | string | No | Reference number |
| `items` | array | Yes | Array of adjustment items |
| `items[].inventory` | object | Yes | Inventory item reference |
| `items[].qty` | decimal | Yes | Quantity (positive=increase, negative=decrease) |
| `items[].unitCost` | decimal | No | Unit cost |
| `items[].reason` | string | No | Reason for adjustment |

### POST /inventory-adjustments/{id}/post - Post Adjustment

Posts the adjustment, updating inventory quantities. Send empty JSON body:

```json
{}
```

---

## Inventory Counts

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/inventory-counts` | Get list of inventory counts |
| POST | `/inventory-counts` | Create an inventory count |
| GET | `/inventory-counts/{id}` | Get inventory count by ID |
| PUT | `/inventory-counts/{id}` | Update inventory count by ID |
| DELETE | `/inventory-counts/{id}` | Delete inventory count by ID |

### POST /inventory-counts - Create Count

```json
{
  "whse": "00",
  "date": "2024-01-15",
  "reference": "COUNT-001",
  "items": [
    {
      "inventory": {
        "partNo": "WIDGET-A"
      },
      "countQty": "95"
    }
  ]
}
```

---

## Inventory Transfers

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/inventory-transfers` | Create inventory transfer |
| GET | `/inventory-transfers/{id}` | Get inventory transfer by ID |
| PUT | `/inventory-transfers/{id}` | Update inventory transfer by ID |
| DELETE | `/inventory-transfers/{id}` | Delete inventory transfer by ID |
| POST | `/inventory-transfers/{id}/post` | Post inventory transfer |

### POST /inventory-transfers - Create Transfer

```json
{
  "fromWhse": "00",
  "toWhse": "01",
  "date": "2024-01-15",
  "reference": "TRF-001",
  "items": [
    {
      "inventory": {
        "partNo": "WIDGET-A"
      },
      "qty": "25"
    }
  ]
}
```

### Transfer Fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `fromWhse` | string | Yes | Source warehouse code |
| `toWhse` | string | Yes | Destination warehouse code |
| `date` | string | No | Transfer date |
| `reference` | string | No | Reference number |
| `items` | array | Yes | Array of transfer items |
| `items[].inventory` | object | Yes | Inventory item reference |
| `items[].qty` | decimal | Yes | Quantity to transfer |

### POST /inventory-transfers/{id}/post - Post Transfer

```json
{}
```

---

## Inventory Groups

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/inventory-groups` | Get list of inventory groups |

### Inventory Groups UDFs

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/inventory-groups-udf` | Get list of inventory groups UDFs |
| GET | `/inventory-groups-udf-fields` | Get list of UDF fields |
| GET | `/inventory-groups-udf-fields/{field-name}` | Get UDF field by name |
| PUT | `/inventory-groups-udf-fields/{field-name}` | Update UDF field by name |
| DELETE | `/inventory-groups-udf-fields/{field-name}` | Delete UDF field by name |
| GET | `/inventory-groups-udf-pages` | Get list of UDF pages |
| POST | `/inventory-groups-udf-pages` | Create UDF page |
| GET | `/inventory-groups-udf-pages/{id}` | Get UDF page by ID |
| PUT | `/inventory-groups-udf-pages/{id}` | Update UDF page by ID |
| DELETE | `/inventory-groups-udf-pages/{id}` | Delete UDF page by ID |

---

## Inventory Warehouses

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/inventory-warehouses` | Get list of inventory warehouses |

### Inventory Warehouses UDFs

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/inventory-warehouses-udf` | Get list of warehouse UDFs |
| GET | `/inventory-warehouses-udf-fields` | Get list of UDF fields |
| GET | `/inventory-warehouses-udf-fields/{field-name}` | Get UDF field by name |
| PUT | `/inventory-warehouses-udf-fields/{field-name}` | Update UDF field by name |
| DELETE | `/inventory-warehouses-udf-fields/{field-name}` | Delete UDF field by name |
| GET | `/inventory-warehouses-udf-pages` | Get list of UDF pages |
| POST | `/inventory-warehouses-udf-pages` | Create UDF page |
| GET | `/inventory-warehouses-udf-pages/{id}` | Get UDF page by ID |
| PUT | `/inventory-warehouses-udf-pages/{id}` | Update UDF page by ID |
| DELETE | `/inventory-warehouses-udf-pages/{id}` | Delete UDF page by ID |

---

## Inventory Price Levels

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/inventory-price-levels` | Get list of price levels |
| POST | `/inventory-price-levels` | Create price level |
| GET | `/inventory-price-levels/{id}` | Get price level by ID |
| PUT | `/inventory-price-levels/{id}` | Update price level by ID |
| DELETE | `/inventory-price-levels/{id}` | Delete price level by ID |

### POST Price Level

```json
{
  "code": "WHOLESALE",
  "description": "Wholesale Price Level",
  "discountPercent": "15.00"
}
```

---

## Inventory UPCs (Global)

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/inventory-upcs` | Get list of all UPCs |
| POST | `/inventory-upcs` | Create UPC |
| GET | `/inventory-upcs/{id}` | Get UPC by ID |
| PUT | `/inventory-upcs/{id}` | Update UPC by ID |
| DELETE | `/inventory-upcs/{id}` | Delete UPC by ID |

### POST UPC

```json
{
  "upc": "012345678901",
  "inventory": {"partNo": "WIDGET-A"},
  "uom": {"code": "EA"}
}
```

---

## Inventory Requisitions

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/inventory-requisitions` | Get list of requisitions |
| POST | `/inventory-requisitions` | Create requisition |
| GET | `/inventory-requisitions/{id}` | Get requisition by ID |
| PUT | `/inventory-requisitions/{id}` | Update requisition by ID |
| DELETE | `/inventory-requisitions/{id}` | Delete requisition by ID |

### POST Requisition

```json
{
  "whse": "00",
  "date": "2024-01-15",
  "requiredDate": "2024-01-22",
  "items": [
    {
      "inventory": {"partNo": "WIDGET-A"},
      "qty": "100"
    }
  ]
}
```

---

## Other Inventory Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/inventory-comments` | Get list of inventory comments |
| GET | `/inventory-levies` | Get list of inventory levies |
| GET | `/inventory-movement` | Get list of inventory movements |
| GET | `/inventory-price-matrix` | Get price matrix |
| GET | `/inventory-promotions` | Get list of inventory promotions |
| GET | `/inventory-receipts` | Get list of inventory receipts |
| GET | `/inventory-sales-departments` | Get list of sales departments |
| GET | `/inventory-sell-prices` | Get list of sell prices |
| GET | `/inventory-serials` | Get list of all serial numbers |
| GET | `/inventory-serial-transactions` | Get list of serial transactions |

---

## Inventory Serials UDFs

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/inventory-serials-udf` | Get list of serial UDFs |
| GET | `/inventory-serials-udf-fields` | Get list of UDF fields |
| GET | `/inventory-serials-udf-fields/{field-name}` | Get UDF field by name |
| PUT | `/inventory-serials-udf-fields/{field-name}` | Update UDF field by name |
| DELETE | `/inventory-serials-udf-fields/{field-name}` | Delete UDF field by name |
| GET | `/inventory-serials-udf-pages` | Get list of UDF pages |
| POST | `/inventory-serials-udf-pages` | Create UDF page |
| GET | `/inventory-serials-udf-pages/{id}` | Get UDF page by ID |
| PUT | `/inventory-serials-udf-pages/{id}` | Update UDF page by ID |
| DELETE | `/inventory-serials-udf-pages/{id}` | Delete UDF page by ID |

---

## Example Queries

```bash
# Get all inventory items
GET /inventory-items

# Get item by ID
GET /inventory-items/123

# Get item by part number
GET /inventory-items?filter={"partNo":"WIDGET-A"}

# Get items with specific fields
GET /inventory-items?fields=partNo,description,availableQty,defaultSellPrice

# Filter items by group
GET /inventory-items?filter={"groupNo":"WIDGETS"}

# Filter active items only
GET /inventory-items?filter={"status":"A"}

# Sort items by available quantity descending
GET /inventory-items?sort=-availableQty

# Get items with UDF fields
GET /inventory-items?udf=1

# Paginate results
GET /inventory-items?start=0&limit=100

# Get item images
GET /inventory-items/123/images

# Get item UOMs
GET /inventory-items/123/uoms

# Get item serial numbers
GET /inventory-items/123/serials

# Create minimal inventory item
POST /inventory-items
Content-Type: application/json
{
  "partNo": "WIDGET-A",
  "description": "Widget Type A"
}

# Create inventory adjustment
POST /inventory-adjustments
Content-Type: application/json
{
  "whse": "00",
  "items": [{"inventory": {"partNo": "WIDGET-A"}, "qty": "10"}]
}

# Post inventory adjustment
POST /inventory-adjustments/123/post
Content-Type: application/json
{}

# Create inventory transfer
POST /inventory-transfers
Content-Type: application/json
{
  "fromWhse": "00",
  "toWhse": "01",
  "items": [{"inventory": {"partNo": "WIDGET-A"}, "qty": "25"}]
}
```
