\## Fact Tables



\### 1. `fact\_sales`

\* \*\*Description:\*\* Contains line-item level transaction details for all completed customer orders.

\* \*\*Granularity:\*\* One record per order line item.



| Column Name | Data Type | Key Type | Description |

| :--- | :--- | :--- | :--- |

| `order\_id` | Integer | FK | Unique identifier for the order. |

| `line\_id` | Integer | PK | Line item identifier within the order. |

| `product\_key` | Integer | FK | Link to `dim\_product.product\_key`. |

| `customer\_id` | Integer | FK | Link to `dim\_customer.customer\_id`. |

| `bill\_to\_city\_key`| Integer | FK | Link to `dim\_geo.geo\_key`. |

| `order\_flag\_key` | Integer | FK | Link to `dim\_order\_flags.flag\_key`. |

| `order\_date` | Date | FK | Date the order was placed. |

| `discount\_pct` | Decimal | Attribute | Applied discount percentage. |

| `order\_total` | Currency | Measure | Net sales value for the line item. |



\---



\### 2. `fact\_inventory`

\* \*\*Description:\*\* Tracks daily snapshot quantities for products in stock.

\* \*\*Granularity:\*\* One record per product per date.



| Column Name | Data Type | Key Type | Description |

| :--- | :--- | :--- | :--- |

| `date` | Date | FK | Inventory snapshot date. |

| `product\_key` | Integer | FK | Link to `dim\_product.product\_key`. |

| `quantity` | Integer | Measure | Total available stock quantity. |



\---



\### 3. `fact\_campaign\_log`

\* \*\*Description:\*\* Logs marketing performance metrics per campaign and channel over time.

\* \*\*Granularity:\*\* Daily aggregated metrics per campaign.



| Column Name | Data Type | Key Type | Description |

| :--- | :--- | :--- | :--- |

| `camp\_key` | Integer | FK | Link to `dim\_campaign.camp\_key`. |

| `date` | Date | FK | Date of metric recording. |

| `impressions` | Integer | Measure | Total ad impressions served. |

| `clicks` | Integer | Measure | Total ad clicks generated. |

| `spend` | Currency | Measure | Total monetary advertising spend. |



\---



\### 4. `fact\_promotion\_coverage`

\* \*\*Description:\*\* Bridge table linking promotional campaigns to specific targeted SKUs.



| Column Name | Data Type | Key Type | Description |

| :--- | :--- | :--- | :--- |

| `camp\_key` | Integer | FK | Link to `dim\_campaign.camp\_key`. |

| `product\_key` | Integer | FK | Link to `dim\_product.product\_key`. |



\---



\### 5. `fact\_order\_process`

\* \*\*Description:\*\* Tracks order operational lifecycle dates from order placement to final payment.



| Column Name | Data Type | Key Type | Description |

| :--- | :--- | :--- | :--- |

| `order\_id` | Integer | PK/FK | Order reference number. |

| `customer\_id` | Integer | FK | Link to `dim\_customer.customer\_id`. |

| `order\_date` | Date | Attribute | Date order was placed. |

| `ship\_date` | Date | Attribute | Date order was dispatched. |

| `delivery\_date` | Date | Attribute | Date customer received shipment. |

| `invoice\_date` | Date | Attribute | Date invoice was generated. |

| `pay\_date` | Date | Attribute | Date payment was settled. |



\---



\### 6. `fact\_sales\_target`

\* \*\*Description:\*\* Stores periodic sales targets for comparative performance tracking.



| Column Name | Data Type | Key Type | Description |

| :--- | :--- | :--- | :--- |

| `period` | Date / Text | PK | Target time period (Year-Month). |

| `target\_revenue` | Currency | Measure | Targeted sales revenue goal. |



\---



\## Dimension Tables



\### 1. `dim\_product`

| Column Name | Key Type | Description |

| :--- | :--- | :--- |

| `product\_key` | PK | Surrogate key for product. |

| `product\_code` | Attribute | Business code for SKU. |

| `product\_name` | Attribute | Name of the product. |

| `brand` | Attribute | Associated product brand. |

| `category` | Attribute | Top-level product category. |

| `subcategory` | Attribute | Low-level product subcategory. |

| `primary\_supplier`| Attribute | Supplier name. |

| `unit\_price` | Attribute | Standard list price. |



\---



\### 2. `dim\_customer`

| Column Name | Key Type | Description |

| :--- | :--- | :--- |

| `customer\_id` | PK | Unique customer identification. |

| `customer\_name` | Attribute | Registered business/customer name. |

| `account\_manager`| Attribute | Assigned sales representative. |

| `contact\_name` | Attribute | Primary contact person. |

| `email` | Attribute | Contact email address. |

| `city` | Attribute | Primary billing city. |

| `credit\_limit` | Attribute | Max credit allocation. |



\---



\### 3. `dim\_geo`

| Column Name | Key Type | Description |

| :--- | :--- | :--- |

| `geo\_key` | PK | Geographic location surrogate key. |

| `city` | Attribute | City name. |

| `region` | Attribute | Geographical region/territory. |



\---



\### 4. `dim\_campaign`

| Column Name | Key Type | Description |

| :--- | :--- | :--- |

| `camp\_key` | PK | Campaign surrogate key. |

| `camp\_name` | Attribute | Marketing campaign title. |

| `budget` | Attribute | Total allocated budget. |

| `channel` | Attribute | Media channel (Social, Email, Search, etc.). |

| `start\_date` | Attribute | Campaign launch date. |

| `end\_date` | Attribute | Campaign conclusion date. |



\---



\### 5. `dim\_order\_flags`

| Column Name | Key Type | Description |

| :--- | :--- | :--- |

| `flag\_key` | PK | Order attribute surrogate key. |

| `channel\_code` | Attribute | Order source system code. |

| `channel\_name` | Attribute | Order channel description (Web, Mobile, B2B Portal). |

| `priority` | Attribute | Delivery priority status (High, Medium, Low). |



\---



\## Security \& Utility Tables



\### 1. `security`

| Column Name | Description |

| :--- | :--- |

| `user\_email` | User login domain/email for dynamic RLS context. |

| `region` | Assigned geographic region filter. |



\### 2. `\_measures`

\* Single repository table housing enterprise DAX measures (`\[Total Sales]`, `\[Target Attainment %]`, `\[ROAS]`, `\[AOV]`, etc.).

