# 🛒 Amazon Product Analysis — 2025

> **A 5-page interactive Power BI dashboard analyzing 1,465 Amazon India product listings across pricing, discounts, brand performance, ratings, and quality flags.**

---

## 📊 Dashboard Preview

### Page 1 — Overview
![Overview Dashboard]([https://github.com/rohithap0819/Amazon-Product-Analysis-2025/blob/main/Amazon%20product%20analysis-2025.pdf](https://github.com/rohithap0819/Amazon-Product-Analysis-2025/blob/main/Images))

> The Overview page shows the complete product landscape — category distribution, price tier split, discount tier breakdown, and 4 headline KPI cards.

| KPI | Value |
|-----|-------|
| Total Products | 1,465 |
| Avg Discounted Price | ₹3,125 |
| Avg Rating | 4.10 |
| Avg Discount | 48% |

---

### Page 2 — Pricing & Discount Analysis
> Deep dive into how Amazon prices and discounts products across categories and price tiers.

**Key Findings:**
- Value-tier products carry the highest average discount at **52%**
- Budget-tier: **40%** · Premium-tier: **45%**
- **96.5% of listed discounts are mismatched** — Amazon's displayed % doesn't match the actual price difference
- Electronics has the highest average actual price (~₹10.1K)

---

### Page 3 — Brand Analysis
> Compares 400+ brands across product count, pricing, discount strategy, and ratings.

**Key Findings:**
- **Boat** leads with 68 products; **AmazonBasics** has the highest avg rating at **4.31**
- **Fire-Boltt** offers the highest avg discount at **80%** yet maintains a strong **4.23** rating
- **Redmi** and **Samsung** are the highest-priced brands at ~₹13,787 avg

---

### Page 4 — Rating & Reviews
> Explores how products are rated, reviewed, and perceived by customers.

**Key Findings:**
- **61.4%** of products are rated "Good" (4.0–4.4 range)
- Only **2.9%** fall in the "Poor" category
- **64.9%** of reviews are medium length — customers write moderately detailed feedback
- Office Products lead category ratings at **4.3**

---

### Page 5 — Quality & Flags
> Identifies risk products — those with high discounts but poor ratings.

**Key Findings:**
- **1,434** products are flagged as OK (97.9%)
- **29 risk products** have high discounts but low ratings
- **Electronics** carries the highest risk count (**11 products**)
- The scatter plot reveals the "sweet spot" — products rated **4.0–4.3** accumulate the most reviews

---

## 📁 Repository Structure

```
Amazon-Product-Analysis-2025/
│
├── Amazon product analysis-2025.pbix    ← Power BI dashboard file
├── Amazon product analysis-2025.pdf     ← Dashboard export (all 5 pages)
├── Rohith A P Amazon Data Analysis.xlsx ← Cleaned dataset with derived columns
├── README.md
└── LICENSE
```

---

## 🗂️ Dataset Overview

**Source:** Amazon India product listings (Kaggle)  
**Sheet used:** `amazon cleaned` — 1,465 rows × 34 columns

| Column | Description |
|--------|-------------|
| `product_id` | Unique product identifier |
| `product_name` | Full product name |
| `category L1–L4` | 4-level category hierarchy |
| `Final_Category` | Most granular category label |
| `discounted_price` | Listed selling price (₹) |
| `actual_price` | Original MRP (₹) |
| `discount_percentage` | Amazon's listed discount % |
| `recomputed_percentage` | Actual calculated discount % |
| `discount_validation` | `Valid` or `Mismatch` flag |
| `rating` | Product rating (0–5) |
| `rating_count` | Number of reviews |
| `Brand` | Derived brand name |
| `Price_Brand` | `Budget` / `Value` / `Permium` tier |
| `Discount_Brand` | `High` / `Medium` / `Low` tier |
| `Rating_Brand` | `Excellent` / `Good` / `Average` / `Poor` |
| `Quality_Flag` | `OK` or `High Discount Low Rating` |
| `Review_Type` | `Short` / `Medium` / `Long` |

---

## 🧩 Dashboard Pages & Features

| Page | Title | Slicers | Charts |
|------|-------|---------|--------|
| 1 | Overview | category L1, Price_Brand, Discount_Brand, Rating_Brand | Donut (category), Bar (top 15 sub-cats), Pie (price tier), Donut (discount tier) |
| 2 | Pricing & Discount | category L1, Price_Brand | Clustered bar (actual vs disc. price), Histogram (disc. %), Stacked bar (validation), Bar (avg disc. by tier) |
| 3 | Brand Analysis | Brand, Price_Brand | Bar (top 10 brands), Scatter (rating vs discount), Bar (avg price), 100% stacked bar (tier mix) |
| 4 | Rating & Reviews | Rating_Brand, Review_Type | Column histogram (rating dist.), Donut (rating tier), Bar (avg by category), Donut (review length) |
| 5 | Quality & Flags | Quality_Flag, category L1 | Stacked bar (flag by category), Clustered bar (discount vs rating tier), Scatter (rating count vs score) |

---

## 🔑 Key DAX Measures Used

```dax
-- Total Products
Total Products = DISTINCTCOUNT('amazon cleaned'[product_id])

-- Avg Discount %
Avg Discount % = AVERAGE('amazon cleaned'[discount_percentage])

-- Discount Mismatch Rate
Mismatch Rate = 
DIVIDE(
    COUNTROWS(FILTER('amazon cleaned', 'amazon cleaned'[discount_validation] = "Mismatch")),
    COUNTROWS('amazon cleaned')
)

-- Good Rated Products %
Good Rated % = 
DIVIDE(
    COUNTROWS(FILTER('amazon cleaned', 'amazon cleaned'[Rating_Brand] = "Good")),
    COUNTROWS('amazon cleaned')
)

-- Risk Products (High Discount + Low Rating)
Risk Products = 
CALCULATE(
    COUNTROWS('amazon cleaned'),
    'amazon cleaned'[Quality_Flag] = "High Discount Low Rating"
)

-- Budget Tier Avg Discount
Budget Disc % = 
CALCULATE(
    AVERAGE('amazon cleaned'[discount_percentage]),
    'amazon cleaned'[Price_Brand] = "Budget"
)
```

---

## 💡 Key Insights

1. **96.5% discount mismatch** — Amazon's listed discount percentages are calculated against a different base price than `actual_price`, making the listed % misleading for nearly all products.

2. **Discounts don't determine quality** — Fire-Boltt offers 80% average discount but maintains a 4.23 avg rating. High discounts are a marketing tactic, not a quality signal.

3. **Price ≠ quality** — Premium tier products still have 194 "Average" rated products. Paying more does not guarantee a better rating.

4. **The review sweet spot is 4.0–4.3** — Products in this rating band attract the most reviews (highest `rating_count`). Going above 4.5 often correlates with fewer reviews.

5. **Electronics carries the most risk** — 11 of the 29 risk-flagged products (high discount, low rating) belong to Electronics, making it the category most requiring quality scrutiny.

6. **Cables dominate the catalog** — With 267 products (18%), Cables are 3× the size of the next largest sub-category. A dedicated Cables filter would significantly improve exploration.

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|------|---------|
| **Power BI Desktop** | Dashboard development & visualization |
| **Microsoft Excel** | Data cleaning, derived columns, KPI tables |
| **DAX** | Custom measures and calculated columns |
| **Power Query** | Data transformation and type formatting |

---

## 🚀 How to Use

1. **Clone or download** this repository
   ```bash
   git clone https://github.com/rohithap0819/Amazon-Product-Analysis-2025.git
   ```

2. **Open the dashboard**  
   Open `Amazon product analysis-2025.pbix` in **Power BI Desktop** (free download from Microsoft)

3. **Explore the pages**  
   Use the arrow-shaped navigation buttons at the top of each page to switch between the 5 dashboard pages

4. **Filter the data**  
   Use the dropdown slicers on each page to filter by Category, Brand, Price Tier, Discount Tier, or Quality Flag

5. **View without Power BI**  
   Open `Amazon product analysis-2025.pdf` for a static snapshot of all 5 dashboard pages

---

## 📌 Assumptions & Limitations

- `discount_percentage` in the dataset is Amazon's listed value — it does not match the mathematically derived value from `actual_price` and `discounted_price` in 96.5% of rows
- The `Price_Brand` column contains a typo: **"Permium"** instead of "Premium" — this is preserved as-is from the source data
- Analysis covers products listed at a single point in time; prices and ratings change dynamically on Amazon
- No time-series data is available — trend analysis over time is not possible with this dataset

---

## 👤 Author

**Rohith A P**  
Data Analytics Enthusiast  
[GitHub Profile](https://github.com/rohithap0819)

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

## ⭐ If you found this useful

Give the repo a **star** ⭐ and feel free to **fork** it to build on top of the analysis!
