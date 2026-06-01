# Data

โฟลเดอร์นี้เก็บข้อมูลของโปรเจกต์ ไฟล์ข้อมูลจริง **ไม่ถูกเก็บใน Git** (อยู่ใน `.gitignore`)
เนื่องจากมีขนาดเบิ้ม (Parquet ~715 MB)(CSV ต้นฉบับ ~2 GB)

```
data/
├── raw/         # ไฟล์ CSV ต้นฉบับรายเดือน 1/1/2024 - 31/3/2026 (input ของ notebook 01)
└── processed/   # ไฟล์ที่ clean แล้ว: cyclistic_cleaned_data.parquet (output ของ notebook 01)
```

## Data source

ข้อมูลเป็นชุดข้อมูลสาธารณะของ **Divvy / Cyclistic bike-share (Chicago)**
เผยแพร่โดย Lyft Bikes and Scooters ภายใต้ [Data License Agreement](https://divvybikes.com/data-license-agreement) เป็นข้อมูลจริงแต่ถูกดัดแปลง เช่น ลบข้อมูลส่วนตัวออก ลบข้อมูลความลับบริษัทออก เหลือแค่การขับขี่ที่ให้เราสามารถนำไปใช้ได้

ดาวน์โหลดไฟล์ได้ที่: https://divvy-tripdata.s3.amazonaws.com/index.html (ไฟล์มีการ update ทุกเดือน)

## Data dictionary

ดูรายละเอียดคอลัมน์ทั้งหมดได้ที่ [`docs/data_dictionary.md`](../docs/data_dictionary.md)
