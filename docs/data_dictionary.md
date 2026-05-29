# Data Dictionary — `cyclistic_cleaned_data.parquet`

ไฟล์ข้อมูลที่ผ่านการทำความสะอาดและเพิ่ม feature แล้ว ประกอบด้วย **11,774,329 แถว** และ **21 คอลัมน์**
ครอบคลุมข้อมูลการเดินทางตั้งแต่ปี **2024–2026**

| # | คอลัมน์ | ชนิดข้อมูล | คำอธิบาย |
|---|---------|-----------|----------|
| 1 | `ride_id` | string | รหัสการเดินทาง (unique identifier) |
| 2 | `rideable_type` | string | ประเภทจักรยาน เช่น `electric_bike`, `classic_bike` |
| 3 | `started_at` | timestamp | วันเวลาที่เริ่มการเดินทาง |
| 4 | `ended_at` | timestamp | วันเวลาที่สิ้นสุดการเดินทาง |
| 5 | `day_of_week` | int | วันในสัปดาห์ (0 = จันทร์ … 6 = อาทิตย์) |
| 6 | `holiday_name` | string | ชื่อวันหยุดของรัฐ Illinois หรือ `None` ถ้าเป็นวันปกติ |
| 7 | `season` | string | ฤดูกาลตามหลักดาราศาสตร์ (`Spring`, `Summer`, `Fall`, `Winter`) |
| 8 | `month` | int | เดือน (1–12) |
| 9 | `year` | int | ปี |
| 10 | `start_station_name` | string | ชื่อสถานีต้นทาง (`On-street` = ไม่มีสถานี ปล่อย/รับบนถนนผ่าน GPS) |
| 11 | `start_station_id` | string | รหัสสถานีต้นทาง (`STREET` = ไม่มีสถานี) |
| 12 | `end_station_name` | string | ชื่อสถานีปลายทาง |
| 13 | `end_station_id` | string | รหัสสถานีปลายทาง |
| 14 | `start_lat` | double | ละติจูดต้นทาง |
| 15 | `start_lng` | double | ลองจิจูดต้นทาง |
| 16 | `end_lat` | double | ละติจูดปลายทาง |
| 17 | `end_lng` | double | ลองจิจูดปลายทาง |
| 18 | `member_casual` | string | ประเภทผู้ใช้: `member` (สมาชิกรายปี) หรือ `casual` (ผู้ใช้ทั่วไป) |
| 19 | `duration` | duration | ระยะเวลาการเดินทาง (timedelta) |
| 20 | `duration_min` | double | ระยะเวลาการเดินทาง (นาที) |
| 21 | `duration_hour` | double | ระยะเวลาการเดินทาง (ชั่วโมง) |

## หมายเหตุการทำความสะอาดข้อมูล (Data cleaning notes)

- ลบแถวที่ `ride_id` ซ้ำกัน
- ลบการเดินทางที่ `ended_at < started_at` (ระยะเวลาติดลบ)
- ลบการเดินทางที่สั้นกว่า 1 นาที (ไม่น่าจะเป็นการใช้งานจริง)
- แทนค่าว่างของสถานีด้วย `On-street` / `STREET` (จักรยานไฟฟ้าที่จอดบนถนนผ่าน GPS)
- เติมพิกัด lat/lng ที่ขาดด้วยค่าเฉลี่ยของสถานีนั้น ๆ (mean imputation)
- เพิ่ม feature: `day_of_week`, `month`, `holiday_name`, `season`
