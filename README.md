Superstore Data Analysis: Optimizing Delivery & Targeting VIPs
Concept: Turning messy retail data into a Logistics & Customer Optimization Dashboard using Python and Power BI.

คอนเซปต์: เปลี่ยนข้อมูลการขายที่ยุ่งเหยิง ให้เป็น Dashboard สำหรับวางแผนขนส่งและเจาะกลุ่มลูกค้าด้วย Python และ Power BI
https://app.powerbi.com/view?r=eyJrIjoiOGQwMDVjZDItMzk5Mi00NDc3LWFiMDYtMTNiNjA5OWE4MDRkIiwidCI6ImZkMjA2NzE1LTc1MDktNGFlNS05Yjk2LTc2YmI5Nzg4NmE4NCIsImMiOjEwfQ%3D%3D
(Click button above to interact with the dashboard / คลิกปุ่มด้านบนเพื่อเล่นแดชบอร์ดจริง)

Project Objective
โปรเจกต์นี้มีจุดประสงค์เพื่อนำข้อมูล Transaction ของ Superstore มาวิเคราะห์เพื่อแก้ปัญหาทางธุรกิจ 2 ด้านหลัก:

Logistics Optimization: วิเคราะห์รูปแบบการขนส่งเพื่อลดความล่าช้าและต้นทุน

Customer Segmentation (RFM): แบ่งกลุ่มลูกค้าเพื่อหา VIP สำหรับทำการตลาดที่แม่นยำ

1. Data Cleaning (Python & Pandas)
จัดการข้อมูลสูญหาย (Missing Values)

แปลงรูปแบบวันที่ (Date formatting) ให้พร้อมวิเคราะห์

2. Feature Engineering (RFM Analysis)
สร้างตัวแปรใหม่ (New Columns) เพื่อวัดมูลค่าลูกค้าตามหลักการ RFM Model:

Recency (R): คำนวณจำนวนวันจากวันที่ซื้อครั้งล่าสุด (ยิ่งน้อยยิ่งดี)

Frequency (F): นับจำนวนครั้งที่ลูกค้ากลับมาซื้อซ้ำ

Monetary (M): ผลรวมยอดใช้จ่ายทั้งหมดของลูกค้า

Segment: ตัดเกรดและจัดกลุ่มลูกค้า (e.g., Champions, Hibernating)

File Description

| File Name | Description (EN) | คำอธิบาย (TH) |
|---|---|---|
| `train.csv` | The original dataset containing sales transactions. |ข้อมูลดิบ Transaction การขายทั้งหมด |
| `analysis_code.ipynb` | Jupyter Notebook for data cleaning & RFM calculation. |โค้ด Python สำหรับคลีนข้อมูลและคำนวณ RFM |
| `cleaned_superstore.csv` |Processed data, ready for visualization. |ไฟล์ที่ผ่านการคลีนและสร้างฟีเจอร์ใหม่แล้ว |
| `Logistics_Dashboard.pbix` |The Power BI file ready to play with. |  ไฟล์ Power BI Dashboard ฉบับสมบูรณ์ |

---
Data Dictionary

### 1. Original Columns (Raw Data)
| Column Name | Description |
|---|---|
| `Row ID` | ลำดับข้อมูล (Unique identifier for each row) |
| `Order ID` | รหัสใบสั่งซื้อ (Transaction ID) |
| `Order Date` | วันที่ทำการสั่งซื้อ |
| `Ship Date` | วันที่ทำการจัดส่งสินค้า |
| `Ship Mode` | รูปแบบการขนส่ง (e.g., Second Class, Standard Class) |
| `Customer ID` | รหัสลูกค้า |
| `Customer Name` | ชื่อลูกค้า |
| `Segment` | ประเภทลูกค้า (e.g., Consumer, Corporate) |
| `Country` / `City` / `State` / `Postal Code` / `Region` | ข้อมูลที่อยู่ในการจัดส่งสินค้า |
| `Product ID` | รหัสสินค้า |
| `Category` | หมวดหมู่สินค้าหลัก (e.g., Furniture, Technology) |
| `Sub-Category` | หมวดหมู่สินค้าย่อย |
| `Product Name` | ชื่อสินค้า |
| `Sales` | ยอดขายต่อรายการสินค้า |

### 2. Engineered Features (New Columns)
คอลัมน์ใหม่ที่ถูกสร้างขึ้นจากการเขียน Python และ DAX เพื่อวิเคราะห์เจาะลึก:

| Column Name | Description & Business Goal |
|---|---|
| **Delivery Days** | **Logistics:** ระยะเวลาจัดส่ง (Ship Date - Order Date) ใช้หา Pattern ความล่าช้า |
| **Day Name** | **Logistics:** ชื่อวัน (จันทร์-อาทิตย์) ดูช่วงเวลา Peak Time ของสัปดาห์ |
| **RFM_Score** | **Marketing:** คะแนนรวม RFM (เช่น '555') ใช้แบ่งเกรดลูกค้าตามพฤติกรรม |
| **Customer_Grade** | **Marketing:** ชื่อกลุ่มลูกค้า (e.g., Platinum, Gold) จากการตัดเกรด RFM ระบุ VIP |
| **Order Total Sales** | **Sales:** ยอดรวมบิลต่อ Order ID ดูมูลค่าต่อตระกร้า (Basket Value) |
| **Items per Order** | **Logistics:** จำนวนชิ้นสินค้าต่อการสั่งซื้อ 1 ครั้ง ประเมินขนาดพัสดุ (Parcel Size) |
| **Basket Size Group** | **Logistics:** การจัดกลุ่มขนาดตระกร้า (Small, Large) เพื่อวางแผนรถขนส่ง |

---

## DAX Formulas Logic
ตัวอย่างสูตร DAX สำคัญที่ใช้ในการสร้าง Features ใน Power BI:

**1. Order Total Sales (ยอดขายรวมต่อออเดอร์)**
```dax
Order Total Sales = 
CALCULATE(
    SUM('superstore_final_for_powerbi'[Sales]), 
    ALLEXCEPT('superstore_final_for_powerbi', 'superstore_final_for_powerbi'[Order ID])
)
Basket Size Group = SWITCH(TRUE(),
    'superstore_final_for_powerbi'[Order Total Sales] < 200, "Small (<$200)", 
    'superstore_final_for_powerbi'[Order Total Sales] < 1000, "Medium ($200-$1000)", 
    "Large (>$1000)"
)
Items per Order = 
CALCULATE(
    COUNTROWS('superstore_final_for_powerbi'), 
    ALLEXCEPT('superstore_final_for_powerbi', 'superstore_final_for_powerbi'[Order ID])
)
