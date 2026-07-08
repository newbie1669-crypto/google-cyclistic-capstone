# Cyclistic Bike Share - Google Data Analytics Capstone Project

By Pluemprach Dangdee (Ne).

<img src="docs/image/victor_victor.png" alt="bike">

โปรเจคนี้ คือ Capstone ของ **Google Data Analytics Professional Certificate** ที่ผมเคยเรียนจบเมื่อนานมาแล้ว ตั้งแต่ 2024 แต่ผมทำโปรเจคเดิมหาย เลยทำใหม่เป็นตัวนี้ ;-;

โจทย์ คือ วิเคราะห์ข้อมูลการใช้งานจักรยานของลูกค้า Cyclistic ใน Chicago เพื่อหาความแตกต่างของพฤติกรรม
ระหว่าง **สมาชิกรายปี (member)** กับ **ผู้ใช้ทั่วไป (casual)**
เพื่อคิดแผนการตลาดในการเปลี่ยน casual ให้เป็น member

---

## Background

Cyclistic เป็นบริษัทให้บริการเช่าจักรยานในเมือง Chicago ทีมการตลาดและการเงินเชื่อว่ากุญแจสำคัญของการเติบโต
คือการเปลี่ยน **ผู้ใช้ทั่วไป (casual rider)** ให้กลายเป็น **สมาชิกรายปี (annual member)** โปรเจกต์นี้จึงวิเคราะห์ข้อมูลการเดินทางจริง
กว่า **11.7 ล้านรายการ (ปี 2024–2026)** เพื่อทำความเข้าใจว่าผู้ใช้ทั้งสองกลุ่มใช้บริการต่างกันอย่างไร

## Business Task

ในโจทย์เราจะต้องตอบคำถามสำคัญที่โจทย์ไกด์ให้เรานี้ให้ได้ ได้แก่

> *" สมาชิกรายปีและผู้ใช้ทั่วไปใช้บริการจักรยาน Cyclistic แตกต่างกันอย่างไร ? "*

> *" ทำไมผู้ใช้ทั่วไปถึงจะตัดสินใจซื้อสมาชิกรายปีของ Cyclistic ? "*

> *" Cyclistics จะสามารถใช้สื่อดิจิตัลเพื่อจูงใจให้ผู้ใช้งานทั่วไปเปลี่ยนมาเป็นสมาชิกได้อย่างไร ? "*

ผลการวิเคราะห์จะนำไปสู่การตอบคำถามเหล่านี้ และช่วยในการแนะนำกลยุทธ์การตลาดเพื่อเปลี่ยน casual ให้เป็น member

## Data Source

ข้อมูลเป็นชุดข้อมูลสาธารณะของ Divvy / Cyclistic bike-share (Chicago) เผยแพร่โดย Lyft Bikes and Scooters ภายใต้ Data License Agreement เป็นข้อมูลจริงแต่ถูกดัดแปลง เช่น ลบข้อมูลส่วนตัวออก ลบข้อมูลความลับบริษัทออก เหลือแค่การขับขี่ที่ให้เราสามารถนำไปใช้ได้

ดาวน์โหลดไฟล์ : <https://divvy-tripdata.s3.amazonaws.com/index.html> (ไฟล์มีการ update ให้ทุกเดือน มีจำนวนมหาศาลมากสามารถต่อยอดในการฝึกหรือทำโปรเจคได้อีกเยอะ)

รายละเอียดข้อมูลและคอลัมน์ : [`docs/data_dictionary.md`](docs/data_dictionary.md)

## Project Structure

```
google-cyclistic-capstone/
├── data/                 # folder มีไว้เป็นพิธี เพราะไฟล์จริงเอาขึ้นมาไม่ได้ ;-;
│   ├── raw/              # ไฟล์ CSV ต้นฉบับรายเดือน (ไม่เก็บใน Git)
│   ├── processed/        # ไฟล์ที่ทำความสะอาดแล้วเป็น parquet (ไม่เก็บใน Git)
│   └── README.md         # ที่มาของข้อมูล
├── notebooks/
│   ├── 01_data_preparation.ipynb           # รวมไฟล์ + data cleaning + add features
│   └── 02_exploratory_data_analysis.ipynb  # วิเคราะห์ + dataviz
├── reports/
│   └── cyclistic_capstone_report.pdf       # รายงาน pdf
├── docs/
│   └── data_dictionary.md                  # คำอธิบายข้อมูล
├── .gitignore
├── LICENSE
└── README.md
```

## Workflow

### เตรียมข้อมูล - `01_data_preparation.ipynb`

- รวมไฟล์ CSV รายเดือนหลายไฟล์เป็น DataFrame เดียว
- ตรวจสอบและจัดการคุณภาพข้อมูล:
  - ลบ `ride_id` ที่ซ้ำกัน
  - ลบการเดินทางที่ระยะเวลาติดลบ (`ended_at < started_at`)
  - ลบการเดินทางที่สั้นกว่า 1 นาที (ไม่น่าจะเป็นการใช้งานจริง)
  - จัดการค่าว่างของสถานี (`On-street`) และเติมพิกัดที่ขาดด้วยค่าเฉลี่ยของสถานี
- เพิ่ม feature ที่เป็นประโยชน์ต่อการวิเคราะห์: `day_of_week`, `month`, `season`, `holiday_name`,
  `duration_min`, `duration_hour`
- บันทึกผลลัพธ์เป็นไฟล์ **Parquet** (มีประสิทธิภาพกว่า CSV สำหรับข้อมูลขนาดใหญ่)

### วิเคราะห์ - `02_data_analysis.ipynb`

เปรียบเทียบพฤติกรรมระหว่าง **member** และ **casual** :

- **จำนวนเที่ยว & ระยะเวลา** — ภาพรวมและการเติบโตแบบ year-on-year
- **ประเภทจักรยาน** (`rideable_type`) ที่แต่ละกลุ่มเลือกใช้
- **วันในสัปดาห์** — เปรียบเทียบวันธรรมดากับวันหยุดสุดสัปดาห์
- **เดือนและฤดูกาล** — รูปแบบตามฤดู (seasonality)
- **ชั่วโมงในแต่ละวัน** — ช่วงเวลาเร่งด่วน (commute) vs ช่วงพักผ่อน
- **สถานียอดนิยม** และ **เส้นทางการเดินทาง**

## Report

**รายงานวิเคราะห์พร้อมภาพ :**
[`reports/cyclistic_capstone_report.pdf`](reports/cyclistic_capstone_report.pdf)

สามารถนำไปดูและต่อยอดได้เลย

## Tools

| Category | Tool |
| -------- | -------- |
| Language | Python |
| Data wrangling | pandas, numpy |
| Data storing | Parquet (pyarrow) |
| Visualization | matplotlib, seaborn, plotly |

**Note** : ถ้าเป็นโจทย์จากต้นฉบับของคอร์สจริงๆ จะให้เราใช้ Spreadsheet, SQL + Tableau หรือ ภาษา R ที่ผมเลือกใช้ Python เพราะมันสามารถทำงานกับข้อมูล CSV ดิบๆ หลักหลายล้าน rows ได้ดีกว่า ฟรี ยืดหยุ่น และชอบเป็นการส่วนตัว (จริงๆทางคอร์สไม่ได้กะจะให้เราทำเยอะขนาดนี้อยู่แล้ว555)

## 📄 License

Repo : [`LICENSE`](LICENSE)

Data : [Divvy Data License Agreement](https://divvybikes.com/data-license-agreement)
