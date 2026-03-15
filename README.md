📦 Logistics Intelligence Engine: Automated Stock & Inbound Tracker
🎯 Project Overview
This project was engineered to solve a critical visibility gap in warehouse operations: the disconnect between current stock levels and inbound supply chain data. By integrating fractured datasets (Warehouse CSVs and Inbound PDF reports) into a unified Power BI dashboard, I built an automated "Early Warning System" for stockouts and dynamic ETA forecasting.

🛠️ The Challenge
Data Silos: Warehouse inventory data lived in daily CSV exports, while inbound shipment details were locked in weekly PDF reports.

Zero Visibility: Critical low-stock SKUs with no incoming orders were indistinguishable from those with pending deliveries.

Manual Reporting: Compiling accurate stock status required hours of manual data manipulation.

🚀 The Solution: A Unified Data Pipeline
Python ETL: Custom scripts using pandas and pdfplumber to extract and clean SKU data from PDFs.

Star Schema Modeling: Engineered a resilient data model with a central Master_SKU dimension table.

Advanced DAX Analytics: Developed custom measures using VAR and SWITCH for real-time flagging.

🧠 Core DAX Logic
1. Stock Intelligence Status (Stock_Status)
Code snippet

Stock_Status = 
VAR CurrentInventory = SUM('Cleaned_Stock'[Quantity])
VAR PlannedInbound = SUM('Wk_11'[Quantity])

RETURN
SWITCH(
    TRUE(),
    CurrentInventory > 0, "🟢 In Stock",
    CurrentInventory <= 0 && PlannedInbound > 0, "🟡 OOS - Inbound Pending",
    "🔴 Critical Shortage"
)
2. Logistics Forecasting (ETA_Week)
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
100% Critical Shortage Visibility: Instantly isolates SKUs with zero stock and zero inbound shipments.

Data-Driven ETA: Supply chain coordinators use automated forecasting to optimize inbound windows.

💻 Technical Stack
Data Engineering: Python (pandas, pdfplumber)

Analytics Platform: Microsoft Power BI Desktop

Language: DAX & M Query

👤 Author
Hubert Kowalski

LinkedIn Profile

Email: kowalskihubert343@gmail.com
