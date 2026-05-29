# Data

โฟลเดอร์นี้เก็บข้อมูลของโปรเจกต์ ไฟล์ข้อมูลจริง **ไม่ถูกเก็บใน Git** (อยู่ใน `.gitignore`)
เนื่องจากมีขนาดใหญ่ (ไฟล์ Parquet ~715 MB)

```
data/
├── raw/         # ไฟล์ CSV ต้นฉบับรายเดือน (input ของ notebook 01)
└── processed/   # ไฟล์ที่ทำความสะอาดแล้ว: cyclistic_cleaned_data.parquet (output ของ notebook 01)
```

## ที่มาของข้อมูล (Data source)

ข้อมูลเป็นชุดข้อมูลสาธารณะของ **Divvy / Cyclistic bike-share (Chicago)**
เผยแพร่โดย Lyft Bikes and Scooters ภายใต้ [Data License Agreement](https://divvybikes.com/data-license-agreement)

ดาวน์โหลดไฟล์รายเดือนได้ที่: https://divvy-tripdata.s3.amazonaws.com/index.html

## วิธีเตรียมข้อมูล

1. ดาวน์โหลดไฟล์ CSV รายเดือนที่ต้องการ แล้ววางไว้ในโฟลเดอร์ `data/raw/`
2. รัน `notebooks/01_data_preparation.ipynb` เพื่อรวมไฟล์ ทำความสะอาด และเพิ่ม feature
3. notebook จะบันทึกผลลัพธ์เป็น `data/processed/cyclistic_cleaned_data.parquet`
4. รัน `notebooks/02_exploratory_data_analysis.ipynb` เพื่อทำการวิเคราะห์ต่อ

ดูรายละเอียดคอลัมน์ทั้งหมดได้ที่ [`docs/data_dictionary.md`](../docs/data_dictionary.md)
