# marketing-campaingn-analytics

# 🎯 Marketing Campaign Performance Analysis  
### (SQL + Excel Dashboard Project)

---

## 👨‍💻 Author  
**SK**  
Data Analyst | SQL & Excel Specialist  
📍 Pune, India  
🌐 GitHub Portfolio: [sklaps.github.io](https://sklaps.github.io)

---

## 🩵 1. Project Overview  
This project analyzes the effectiveness of marketing campaigns conducted by a Portuguese banking institution.  
The goal is to identify which customer segments and contact strategies yield the highest term-deposit conversion rates.

**Tools Used:**  
- 🗄️ MySQL for Data Cleaning & Analysis  
- 📊 Excel for Visualization & KPI Dashboard  

---

## 🩵 2. Dataset Summary  
**Source:** [UCI Machine Learning Repository — Bank Marketing Data](https://archive.ics.uci.edu/ml/datasets/Bank+Marketing)  
**Records:** ~45,211 customers  
**Target Variable:** `y` → indicates if the client subscribed to a term deposit  

| Column | Description |
|---------|-------------|
| age | Age of the client |
| job | Occupation type |
| education | Education level |
| balance | Average yearly account balance |
| contact | Communication channel used |
| poutcome | Outcome of previous marketing campaign |
| y | Term deposit subscription (yes / no) |

---

## 🩵 3. Key Metrics Summary  

| Metric | Value |
|---------|-------|
| 🧍 Total Customers | 45,211 |
| ✅ Total Conversions | 5,289 |
| 📈 Conversion Rate | 11.7% |
| 💼 Top Job Type | student (28.7%) |
| 🎓 Top Education | tertiary (16.4%) |
| ☎️ Top Contact Type | cellular (13.6%) |
| 🔁 Top Previous Outcome | success (50%) |

---

## 🩵 4. SQL Analysis Highlights  

**Queries Performed:**  
- Conversion Rate by Job  
- Conversion Rate by Education  
- Conversion Rate by Contact Type  
- Conversion Rate by Previous Outcome  

**Example Insight (Job):**
| Job | Conversion Rate (%) |
|------|---------------------|
| student | 28.7 |
| retired | 22.8 |
| unemployed | 15.5 |
| management | 13.8 |

---

## 🩵 5. Excel Dashboard Overview  

### Dashboard Components  
- KPI Summary Cards  
- Conversion Rate by Job (Column Chart)  
- Conversion Rate by Education (Bar Chart)  
- Conversion Rate by Contact Type (Donut Chart)  
- Previous Campaign Outcome (Column Chart)  
- Top 10 Segments by Conversion Rate (Horizontal Bar Chart)

> The dashboard provides a visual summary of campaign performance, enabling marketing teams to instantly identify high-response segments and improve ROI.

---

## 🩵 6. Business Insights Summary  

| # | Category | Key Finding | Business Impact |
|---|-----------|--------------|-----------------|
| 1 | Job Type | Students (28.7%) & Retired (22.8%) perform best | Focus marketing budget on these segments |
| 2 | Education | Tertiary education = 16.4% conversion | Educated clients are more investment-prone |
| 3 | Contact | Cellular contacts outperform telephone (13.6% vs 2.8%) | Prioritize mobile campaigns |
| 4 | Previous Outcome | Past successes = 50% future success rate | Maintain client engagement history |
| 5 | Age | 30–50 age range most stable | Mid-career customers are reliable audience |
| 6 | Balance | Not a strong predictor | Trust & interaction > wealth indicator |

---

## 🩵 7. Strategic Recommendations  

1. 🎯 Target **students and retired** clients in upcoming campaigns.  
2. ☎️ Use **cellular** as the main contact channel.  
3. 🔁 Re-engage previously successful clients with loyalty offers.  
4. 📚 Invest in **educational marketing content** to build trust.  
5. 🤖 Develop a **predictive model** to forecast client conversions.  

---

## 🧾 SQL Queries 

### 1. Overall conversion
```sql
-- sql/overall_conversion.sql
SELECT 
  COUNT(*) AS total_customers,
  SUM(CASE WHEN y = 'yes' THEN 1 ELSE 0 END) AS total_converted,
  ROUND(SUM(CASE WHEN y = 'yes' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS conversion_rate
FROM bank_data.bankdata_clean;
```
### 2. Conversion by job
```sql
-- sql/conversion_by_job.sql
SELECT 
  job,
  COUNT(*) AS total_customers,
  SUM(CASE WHEN y = 'yes' THEN 1 ELSE 0 END) AS converted,
  ROUND(SUM(CASE WHEN y = 'yes' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS conversion_rate
FROM bank_data.bankdata_clean
GROUP BY job
ORDER BY conversion_rate DESC;
```
### 3. Conversion by education
```
-- sql/conversion_by_education.sql
SELECT 
  education,
  COUNT(*) AS total_customers,
  SUM(CASE WHEN y = 'yes' THEN 1 ELSE 0 END) AS converted,
  ROUND(SUM(CASE WHEN y = 'yes' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS conversion_rate
FROM bank_data.bankdata_clean
GROUP BY education
ORDER BY conversion_rate DESC;
```
### 4. Conversion by contact
```
-- sql/conversion_by_contact.sql
SELECT 
  contact,
  COUNT(*) AS total_customers,
  SUM(CASE WHEN y = 'yes' THEN 1 ELSE 0 END) AS converted,
  ROUND(SUM(CASE WHEN y = 'yes' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS conversion_rate
FROM bank_data.bankdata_clean
GROUP BY contact
ORDER BY conversion_rate DESC;
```
### 5. Conversion by previous outcome (poutcome)
```
-- sql/conversion_by_poutcome.sql
SELECT 
  poutcome,
  COUNT(*) AS total_customers,
  SUM(CASE WHEN y = 'yes' THEN 1 ELSE 0 END) AS converted,
  ROUND(SUM(CASE WHEN y = 'yes' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS conversion_rate
FROM bank_data.bankdata_clean
GROUP BY poutcome
ORDER BY conversion_rate DESC;
```

### 6. Conversion by duration groups
```
-- sql/conversion_by_duration.sql
SELECT 
  CASE 
    WHEN duration < 100 THEN '<100s'
    WHEN duration BETWEEN 100 AND 300 THEN '100-300s'
    WHEN duration BETWEEN 301 AND 600 THEN '301-600s'
    ELSE '>600s'
  END AS duration_group,
  COUNT(*) AS total_customers,
  SUM(CASE WHEN y = 'yes' THEN 1 ELSE 0 END) AS converted,
  ROUND(SUM(CASE WHEN y = 'yes' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS conversion_rate
FROM bank_data.bankdata_clean
GROUP BY duration_group
ORDER BY conversion_rate DESC;
```
### 7. Top segments view (job + education + contact + poutcome)
```
-- sql/top_segments_view.sql
CREATE OR REPLACE VIEW top_segments AS
SELECT 
  job,
  education,
  contact,
  poutcome,
  COUNT(*) AS total_customers,
  ROUND(SUM(CASE WHEN y = 'yes' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS conversion_rate
FROM bank_data.bankdata_clean
GROUP BY job, education, contact, poutcome
HAVING COUNT(*) > 50
ORDER BY conversion_rate DESC
LIMIT 50;
```











