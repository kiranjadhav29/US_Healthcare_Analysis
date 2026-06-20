# US_Healthcare_Analysis
# US Healthcare Analytics Dashboard

This project analyzes US healthcare data to understand:

- Patient demographics and treatment patterns
- Hospital performance and financial metrics
- Payer-provider financial interactions
- Healthcare expenses and billing trends
- Regional healthcare impact analysis

The analysis is performed using **Power BI** for visualization and **DAX** for advanced calculations, with data modeling in **Star Schema** architecture.

<img width="1125" height="748" alt="Executive-Summary" src="https://github.com/user-attachments/assets/7fcb9d0e-231c-4b5c-bf79-d8e0c2122d4d" />
<img width="1122" height="752" alt="Hospital-Analysis" src="https://github.com/user-attachments/assets/a2a3e756-0654-4e75-aff8-1934e3812c77" />
<img width="1122" height="748" alt="Patient-Analysis" src="https://github.com/user-attachments/assets/f7574551-4fd5-444f-b8d5-bd78617adc8e" />
![Uploading Payer-Provider-Analysis.png…]()

---

## 🎯 Business Problem

The healthcare dataset highlights key challenges:

- **High Accounts Receivable (AR)** in certain hospitals
- **Inefficient insurance payment ratios**
- **Regional disparities** in patient care and expenses
- Need to identify **billing optimization opportunities**
- Understanding **payer contributions** to medical expenses
- Tracking **treatment trends** and **procedure dominance**

---

## 🛠️ Tools Used

- **Power BI Desktop** – Dashboard development and visualization
- **DAX** – Custom measures and calculations
- **Power Query** – Data cleaning and transformation
- **Star Schema** – Data modeling with 8 dimension tables and 1 fact table

---

## 🔍 Key Insights

### 📌 Hospital Performance Analysis

- **Hospitals with highest accounts receivable** identified
- **Insurance payment ratios** evaluated across facilities
- **Procedure volumes** analyzed to recognize dominant hospitals
- **Financial ratios** tracked for cost optimization

### 📌 Patient Analysis

- **Patient demographics** (Age, Gender, Region, Habits)
- **Treatment patterns** and service utilization
- **Patient habits** (Tobacco, Alcohol, Exercise, Diet) analyzed
- **Regional patient concentration** mapped

### 📌 Payer Provider Analysis

- **Payer expense contributions** examined
- **Reimbursement rates** evaluated
- **Service utilization patterns** identified
- **Cost-sharing arrangements** assessed

### 📌 Regional Impact Analysis

- **Regions severely affected by pandemic** identified
- **Patient count and expense distribution** mapped
- **Resource allocation** strategies recommended

### 📌 Trend Analysis

- **Healthcare expenses trends** over time
- **Patient admission patterns** analyzed
- **Monthly expense trends** tracked
- **Seasonal variations** identified

### 📌 Financial Metrics

| Metric | Description |
|--------|-------------|
| Gross Charges | Total charges including contractual adjustments |
| Net Revenue | Gross charges minus adjustments |
| Payment Ratio | Insurance payment percentage |
| AR Aging | Outstanding receivables (30/60/90+ days) |
| Collection Rate | Payment collection efficiency |

---

## 📊 DAX Measures Developed

```dax
// Financial Measures
Total Gross Charges = SUM(FactTable[GrossCharge])
Total Payments = SUM(FactTable[Payment])
Total Adjustments = SUM(FactTable[Adjustment])
Net Revenue = [Total Payments] - [Total Adjustments]
Total AR = SUM(FactTable[AR])

// Payment Ratios
Insurance Payment Ratio = DIVIDE([Total Payments], [Total Gross Charges], 0)
Collection Rate = DIVIDE([Total Payments], [Total Gross Charges] - [Total Adjustments], 0)

// AR Aging
AR Aging 30 Days = CALCULATE([Total AR], dimDate[DaysSinceService] <= 30)
AR Aging 60 Days = CALCULATE([Total AR], dimDate[DaysSinceService] BETWEEN 31 AND 60)
AR Aging 90+ Days = CALCULATE([Total AR], dimDate[DaysSinceService] > 90)

// Patient Metrics
Total Patients = DISTINCTCOUNT(FactTable[PatientNumber])
Average Patient Age = AVERAGE(dimPatient[PatientAge])
Patient Gender Distribution = DIVIDE(CALCULATE([Total Patients], dimPatient[PatientGender] = "Male"), [Total Patients], 0)

// Procedure Analysis
Total Procedures = SUM(FactTable[CPTUnits])
Average Charges Per Procedure = DIVIDE([Total Gross Charges], [Total Procedures], 0)

// Regional Analysis
Regional Patient Count = CALCULATE([Total Patients], dimPatient[Region] = SELECTEDVALUE(dimPatient[Region]))
Regional Expense Share = DIVIDE(CALCULATE([Total Gross Charges], dimPatient[Region] = SELECTEDVALUE(dimPatient[Region])), [Total Gross Charges], 0)
