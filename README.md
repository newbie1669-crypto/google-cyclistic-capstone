# Cyclistic - Google Data Analytics Capstone Project

> Capstone project ของ **Google Data Analytics Professional Certificate**
> วิเคราะห์ข้อมูลการใช้งานจักรยานสาธารณะ Cyclistic ในเมือง Chicago เพื่อหาความแตกต่างของพฤติกรรม
> ระหว่าง **สมาชิกรายปี (member)** กับ **ผู้ใช้ทั่วไป (casual)** เพื่อคิดแผนการตลาดในการเปลี่ยน casual ให้เป็น member

---

## Background

Cyclistic เป็นบริษัทให้บริการเช่าจักรยานในเมือง Chicago ทีมการตลาดเชื่อว่ากุญแจสำคัญของการเติบโต
คือการเปลี่ยน **casual rider** ให้กลายเป็น **annual member** โปรเจกต์นี้จึงวิเคราะห์ข้อมูลการเดินทางจริง
กว่า **11.7 ล้านรายการ (ปี 2024–2026)** เพื่อทำความเข้าใจว่าผู้ใช้ทั้งสองกลุ่มใช้บริการต่างกันอย่างไร

โปรเจกต์ดำเนินตามกรอบการทำงาน 6 ขั้นของ Google Data Analytics:
**Ask → Prepare → Process → Analyze → Share → Act**

## Business Task

> *"สมาชิกรายปีและผู้ใช้ทั่วไปใช้บริการจักรยาน Cyclistic แตกต่างกันอย่างไร?"*

ผลการวิเคราะห์จะนำไปสนับสนุนกลยุทธ์การตลาดเพื่อเปลี่ยน casual rider ให้เป็นสมาชิกรายปี

## Data Source

ข้อมูลสาธารณะของ **Divvy / Cyclistic bike-share** เผยแพร่โดย Lyft Bikes and Scooters
ภายใต้ [Data License Agreement](https://divvybikes.com/data-license-agreement)
ดาวน์โหลดไฟล์รายเดือนได้ที่ https://divvy-tripdata.s3.amazonaws.com/index.html

รายละเอียดข้อมูลและคอลัมน์ทั้งหมดดูได้ที่ [`docs/data_dictionary.md`](docs/data_dictionary.md)
และวิธีเตรียมไฟล์ดูได้ที่ [`data/README.md`](data/README.md)

> ไฟล์ข้อมูล (`.csv`, `.parquet`) ไม่ถูกเก็บไว้ใน repository นี้เนื่องจากมีขนาดใหญ่
> (ไฟล์ Parquet ~715 MB) — กรุณาดาวน์โหลดและเตรียมข้อมูลตามขั้นตอนใน `data/README.md`

## Project Structure

```
google-cyclistic-capstone/
├── data/
│   ├── raw/              # ไฟล์ CSV ต้นฉบับรายเดือน (ไม่เก็บใน Git)
│   ├── processed/        # ไฟล์ที่ทำความสะอาดแล้ว .parquet (ไม่เก็บใน Git)
│   └── README.md         # ที่มาของข้อมูลและวิธีเตรียมข้อมูล
├── notebooks/
│   ├── 01_data_preparation.ipynb           # รวมไฟล์ + ทำความสะอาด + เพิ่ม feature
│   └── 02_exploratory_data_analysis.ipynb  # วิเคราะห์ + สร้างกราฟ
├── reports/
│   └── cyclistic_capstone_report.pdf       # รายงานฉบับสมบูรณ์
├── docs/
│   └── data_dictionary.md                  # คำอธิบายคอลัมน์ข้อมูล
├── requirements.txt      # Python dependencies
├── .gitignore
├── LICENSE               # MIT
└── README.md
```

## Workflow

### เตรียมข้อมูล — `01_data_preparation.ipynb`

- รวมไฟล์ CSV รายเดือนหลายไฟล์เป็น DataFrame เดียว
- ตรวจสอบและจัดการคุณภาพข้อมูล:
  - ลบ `ride_id` ที่ซ้ำกัน
  - ลบการเดินทางที่ระยะเวลาติดลบ (`ended_at < started_at`)
  - ลบการเดินทางที่สั้นกว่า 1 นาที (ไม่น่าจะเป็นการใช้งานจริง)
  - จัดการค่าว่างของสถานี (`On-street`) และเติมพิกัดที่ขาดด้วยค่าเฉลี่ยของสถานี
- เพิ่ม feature ที่เป็นประโยชน์ต่อการวิเคราะห์: `day_of_week`, `month`, `season`, `holiday_name`,
  `duration_min`, `duration_hour`
- บันทึกผลลัพธ์เป็นไฟล์ **Parquet** (มีประสิทธิภาพกว่า CSV สำหรับข้อมูลขนาดใหญ่)

### EDA — `02_exploratory_data_analysis.ipynb`

เปรียบเทียบพฤติกรรมระหว่าง **member** และ **casual** ในหลายมิติ:

- **จำนวนเที่ยว & ระยะเวลา** — ภาพรวมและการเติบโตแบบ year-on-year
- **ประเภทจักรยาน** (`rideable_type`) ที่แต่ละกลุ่มเลือกใช้
- **วันในสัปดาห์** — เปรียบเทียบวันธรรมดากับวันหยุดสุดสัปดาห์
- **เดือนและฤดูกาล** — รูปแบบตามฤดู (seasonality)
- **ชั่วโมงในแต่ละวัน** — ช่วงเวลาเร่งด่วน (commute) vs ช่วงพักผ่อน
- **สถานียอดนิยม** และการเดินทางแบบ `On-street`

## Report

[`reports/cyclistic_capstone_report.pdf`](reports/cyclistic_capstone_report.pdf)

## Tools

|  | Tool |
|------|-----------|
| Language | Python |
| จัดการข้อมูล | pandas, numpy |
| จัดเก็บข้อมูล | Parquet (pyarrow) |
| visualization | matplotlib, seaborn, plotly |


## 📄 License

This Project : [`LICENSE`](LICENSE)

Data : [Divvy Data License Agreement](https://divvybikes.com/data-license-agreement)
