A complete data analysis pipeline for an e-commerce business (ENIAC), covering data cleaning, quality control, and business intelligence — built with Python and pandas in Google Colab.

---

## Project Structure

```
├── data_cleaning.ipynb       # Raw data preparation and cleaning
├── quality_assessment.ipynb  # Data quality control and filtering
├── business_analysis.ipynb   # Product categorization and business insights


---

## 📓 Notebook Descriptions

### 1. `data_cleaning.ipynb` — Data Cleaning with Pandas

Prepares the raw `orders`, `orderlines`, and `products` datasets for analysis.

**Key steps:**
- **Duplicate detection** — checks for duplicate rows across all DataFrames using `.duplicated()`
- **Missing value handling** — identifies and removes 5 missing `total_paid` values in `orders` (~0.001% of data)
- **Datatype corrections:**
  - Converts `created_date` and `date` columns to `datetime`
  - Converts `unit_price` from string to float in `orderlines`
- **Double-decimal price fix** — identifies and removes ~12.3% of `orderlines` rows affected by malformed prices (e.g. `1.137.99`), retaining 216,250 clean rows

---

### 2. `quality_assessment.ipynb` — Data Quality Assessment

Takes cleaned DataFrames and applies further quality control to ensure a consistent, analysis-ready dataset.

**Key steps:**
- **Display configuration** — sets pandas options for float formatting and max row display
- **Order state filtering** — retains only `Completed` orders, excluding shopping cart and cancelled entries
- **Cross-table consistency** — performs an inner merge between `orders` and `orderlines` to keep only order IDs present in both tables
- **Unknown product removal** — identifies and removes entire orders containing products not found in the `products` table (left-join strategy)
- **Revenue validation** — calculates `unit_price * product_quantity` per order, merges with `total_paid`, and computes the average difference to assess pricing consistency across tables

---

### 3. `business_analysis.ipynb` — Business Analysis & Insights

Performs product categorization and business intelligence on the quality-controlled data.

**Key steps:**
- **Smartphone detection** — identifies smartphone products using brand name matching (Apple iPhone, Samsung Galaxy, Google Pixel, Xiaomi, OnePlus, Huawei) and fills missing `type` values accordingly
- **Product categorization** — maps product `type` codes to 28 human-readable categories (e.g. *MacBook Pro Laptops*, *NAS Servers and Storage*, *Smartphones*, *iPad Cases and Covers*)
- **"Other" reclassification** — reduces uncategorized products from 2,158 by applying keyword-based rules on `name` and `desc` fields; achieves 96% categorization coverage
- **Order line merging** — joins `orderlines` with categorized products for downstream analysis
- **Revenue visualization** — uses `seaborn` and `matplotlib` to visualize order counts by product category

---

## Datasets

The project uses three core datasets, loaded from Google Drive:

| Dataset | Description |
|---|---|
| `orders.csv` | Order-level data including `order_id`, `total_paid`, `state`, `created_date` |
| `orderlines.csv` | Line-item data with `id_order`, `sku`, `unit_price`, `product_quantity`, `date` |
| `products.csv` | Product catalog with `sku`, `name`, `desc`, `type` |

Cleaned and quality-controlled versions are saved as `_cl` and `_qu` suffixed files respectively.

---

## 🔧 Requirements

- Python 3.x
- pandas
- matplotlib
- seaborn
- numpy

Install dependencies:

```bash
pip install pandas matplotlib seaborn numpy
```

---

## Getting Started

1. **Clone the repository:**
   ```bash
   git clone https://github.com/yourusername/eniac-data-analysis.git
   cd eniac-data-analysis
   ```

2. **Open notebooks in Google Colab** *(recommended)* or run locally in Jupyter:
   - [Google Colab](https://colab.research.google.com/)

3. **Run notebooks in order:**
   ```
   1. data_cleaning.ipynb
   2. quality_assessment.ipynb
   3. business_analysis.ipynb
   ```

> Each notebook loads data from Google Drive. Make sure the Drive links are accessible or update the file paths to your local copies.

---

## Key Findings

- **~12.3%** of orderlines had malformed price values and were removed
- **216,250** clean orderlines retained after data cleaning
- **28 product categories** identified, covering **96%** of the product catalog
- Order-level revenue cross-validated between `orders.total_paid` and computed `unit_price × quantity`

---
