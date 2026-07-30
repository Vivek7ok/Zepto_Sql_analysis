# 🛒 Zepto Product Catalog — Data Analysis 

A SQL-based exploratory data analysis of the Zepto quick-commerce product catalog, covering pricing, discounts, inventory health, and revenue estimation across **14 product categories**.

---

## 📊 Dataset Overview

| Field | Type | Description | 
|---|---|---|
| `category` | VARCHAR | Product category (14 distinct values) |
| `name` | VARCHAR | Product display name |
| `mrp` | NUMERIC | Maximum Retail Price (in ₹, converted from paise) | 
| `discountPercent` | NUMERIC | Discount percentage at time of listing |
| `discountedSellingPrice` | NUMERIC | Actual selling price after discount |
| `availableQuantity` | INTEGER | Units currently in stock |
| `weightInGms` | INTEGER | Product weight in grams |
| `outOfStock` | BOOLEAN | Whether the product is currently unavailable |
| `quantity` | INTEGER | Listing quantity unit |

> **Size:** 3,732 rows · 1,681 unique products · 14 categories

---

## 🗂️ Project Structure

```
zepto-data-analysis/
│
├── zepto.csv                   # Raw dataset
├── query.sql                   # All SQL queries used in the analysis
├── zepto_analysis_report.docx  # Full formatted analysis report
└── README.md
```

---

## ⚙️ Setup

### 1. Create and load the database

```sql
CREATE DATABASE zepto;
USE zepto;

CREATE TABLE sales_2 (
    category               VARCHAR(120),
    name                   VARCHAR(150) NOT NULL,
    mrp                    NUMERIC(8,2),
    discountPercent        NUMERIC(5,2),
    availableQuantity      INTEGER,
    discountedSellingPrice NUMERIC(8,2),
    weightInGms            INTEGER,
    outOfStock             BOOLEAN,
    quantity               INTEGER
);
```

Import `zepto.csv` into the `sales_2` table using your preferred method (MySQL `LOAD DATA`, DBeaver import wizard, etc.).

### 2. Convert paise to rupees

Raw price values in the dataset are stored in paise. Run this one-time UPDATE to normalise them to Indian Rupees (₹):

```sql
UPDATE sales_2
SET mrp                    = mrp / 100.0,
    discountedSellingPrice = discountedSellingPrice / 100.0;
```

---

## 🔍 Analysis Questions

| # | Question |
|---|---|
| Q1 | Top 10 best-value products by discount percentage |
| Q2 | High-MRP products (> ₹300) that are out of stock |
| Q3 | Estimated revenue per category |
| Q4 | Products with MRP > ₹500 and discount < 10% |
| Q5 | Top 5 categories by average discount percentage |
| Q6 | Best value products by grams per rupee (weight ≥ 100 g) |
| Q7 | Product distribution by weight group (Low / Medium / Bulk) |
| Q8 | Total inventory weight per category |

---

## 📝 Key Findings

- **Highest discounts** are in Snacks and Dairy — Dukes Waffy products lead at **51% off**
- **Cooking Essentials** and **Munchies** each contribute an estimated **₹3.37 lakh** in revenue
- **Fruits & Vegetables** carries the highest average discount (15.5%) yet the lowest revenue — indicating a stock depth problem
- **4 premium products** (MRP > ₹300) are currently out of stock, representing direct revenue leakage
- **91% of products** weigh under 1 kg, highlighting a catalog dominated by small, unit-purchase SKUs
- Several bulk cooking oils carry a **0% discount**, consistent with commodity pricing behaviour

---

## 🛠️ Tools Used

- **MySQL** — database creation, data loading, and all SQL queries
- **SQL** — aggregation, filtering, CASE statements, window-style ranking
- **DBeaver** — CSV import and query interface

---

## 📄 Report

A full data analysis report with formatted tables and findings is available in the repository as `zepto_analysis_report.docx`.
