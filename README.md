# 📦 Logistics Intelligence Engine: Automated Stock & Inbound Tracker

[![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://www.python.org/)
[![Power BI](https://img.shields.io/badge/Power_BI-Data_Modeling-yellow.svg)](https://powerbi.microsoft.com/)
[![DAX](https://img.shields.io/badge/DAX-Advanced_Analytics-orange.svg)](https://docs.microsoft.com/en-us/dax/)

## 🎯 Project Overview

This project was engineered to solve a critical visibility gap in warehouse operations: **the disconnect between current stock levels and inbound supply chain data.** By integrating fractured datasets (Warehouse CSVs and Inbound PDF reports) into a unified Power BI dashboard, I built an automated "Early Warning System" for stockouts and dynamic ETA forecasting.

## 🛠️ The Challenge

* **Data Silos:** Warehouse inventory data lived in daily CSV exports, while inbound shipment details were locked in weekly PDF reports.
* **Zero Visibility:** Critical low-stock SKUs with no incoming orders were indistinguishable from those with pending deliveries, leading to reactive decision-making.
* **Manual Reporting:** Compiling accurate stock status required hours of manual data manipulation in Excel.

## 🚀 The Solution: A Unified Data Pipeline

I designed and implemented a scalable ETL (Extract, Transform, Load) and data modeling architecture:

1. **Python ETL (Data Extraction):** Custom scripts using `pandas` and `pdfplumber` to extract, clean, and standardize SKU data from fractured sources.
2. **Star Schema Modeling:** Engineered a resilient data model in Power BI with a central **Master_SKU1** dimension table to ensure 100% SKU visibility across all datasets.
3. **Advanced DAX Analytics:** Developed custom measures using `VAR` and `SWITCH` for real-time stock status flagging and ETA logic.

[Insert Image of Power BI Data Model / Star Schema]

## 🧠 Core DAX Logic

Here are the key formulas that power the intelligence of this dashboard:

### 1. Stock Intelligence Status (`Stock_Status`)
This measure dynamically prioritizes inventory risk using robust `VAR` and `SWITCH` logic.

```dax
Stock_Status = 
VAR CurrentInventory = SUM('Cleaned_Stock'[Quantity])
VAR PlannedInbound = SUM('Wk_11'[Quantity])

RETURN
SWITCH(
    TRUE(),
    CurrentInventory > 0, "🟢 In Stock",
    CurrentInventory <= 0 && PlannedInbound > 0, "🟡 OOS - Inbound Pending",
    "🔴 Critical Shortage" -- 0 Stock & 0 Inbound
)
2. Logistics Forecasting (ETA_Week)
An automated ETA tracker that estimates arrival weeks based on data ingestion timestamps.

Code snippet

ETA_Week = 
VAR NextWeekArrival = MAX('Wk_11'[Download_Date]) + 7
RETURN
IF(
    [Dostawa_Inbound] > 0, 
    "Wk " & WEEKNUM(NextWeekArrival), 
    BLANK()
)
✨ Key Business Benefits
100% Critical Shortage Visibility: Instantly isolates SKUs with zero stock and zero inbound shipments, reducing manual analysis time by 90%.

Proactive Inventory Management: Operational teams can now plan promotions or substitute products based on precise "Inbound Pending" data.

Data-Driven ETA: Supply chain coordinators use the automated ETA_Week forecasting to optimize inbound logistics windows.

💻 Technical Stack
Data Engineering: Python (pandas, pdfplumber)

Analytics Platform: Microsoft Power BI Desktop

Language: DAX (Data Analysis Expressions) & M Query

👤 Author
Hubert Kowalski

www.linkedin.com/in/hubert-kowalski-081189398

kowalskihubert343@gmail.com
