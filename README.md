# แผนที่น้ำท่วมสูงสุด หาดใหญ่ 2568 · 9 กรณีฝน (Longdo Map)

เว็บแผนที่ (Longdo Map 3D) แสดง polygon ความลึกน้ำท่วมสูงสุดจากแบบจำลอง SWE-Flood2D FV2
กรณีฐาน = น้ำท่วมหาดใหญ่ 2568 (ฝนจริง 15–30 พ.ย. 2568) และกรณีฝน −90, −50, −25, −10, +10, +25, +50, +100 %

## โครงสร้าง
```
index.html                       หน้าเว็บ (ใส่ Longdo API key ไว้ในตัวแปร API_KEY)
catchments.js                    ขอบเขตลุ่มน้ำ 10 ลุ่ม + ตำแหน่งชื่อ (สร้างจาก tools/make_catchments.py)
scenarios.js                     ข้อมูลฝนเฉลี่ย + สถิติน้ำท่วม + กรณีที่มีผลแล้ว (สร้างจากสคริปต์)
vec/flood_<case>.js              polygon น้ำท่วมสูงสุดของแต่ละกรณี (สร้างจากสคริปต์)
results/DWmax_<case>.asc         ผล max depth จากแบบจำลอง (input ของสคริปต์)
tools/make_vector_scenarios.py   สคริปต์สร้าง vec/ และ scenarios.js
tools/make_catchments.py         python tools/make_catchments.py results/HadYai_inflow_catchments.asc
```
`<case>` = `base`, `Rm90`, `Rm50`, `Rm25`, `Rm10`, `Rp10`, `Rp25`, `Rp50`, `Rp100`

## เพิ่มผลกรณีใหม่
1. รัน `Run_HadYai_Rain_Scenarios_FV2.bat` แล้วคัดลอก `Results_FV2_<case>/A1LDWmax.asc`
   มาเป็น `results/DWmax_<case>.asc`
2. `pip install numpy scipy contourpy opencv-python`
3. `python tools/make_vector_scenarios.py results`
4. commit `scenarios.js` และ `vec/` ขึ้น GitHub – ปุ่มกรณีนั้นจะใช้งานได้ทันที
   (ไฟล์ `results/*.asc` ใหญ่ ไม่จำเป็นต้อง commit ใส่ไว้ใน `.gitignore` ได้)

## เปิดบน GitHub Pages
Settings → Pages → Source: branch `main`, folder `/ (root)` → เปิดที่ `https://<user>.github.io/<repo>/`
หรือเปิด `index.html` ตรงจากเครื่องก็ได้ (ไฟล์ข้อมูลโหลดด้วย `<script>`)

## Longdo API key
key อยู่ใน `index.html` (`const API_KEY`) ซึ่งทุกคนที่เปิดหน้าเว็บจะเห็นได้
ควรจำกัด key ให้ใช้ได้เฉพาะโดเมน `<user>.github.io` ในหน้าจัดการ key ของ Longdo Map
เปลี่ยน key ชั่วคราวได้ด้วย `?key=...` ต่อท้าย URL

## ข้อมูลฝน
ฝนเฉลี่ยพื้นที่รับน้ำต้นน้ำของจุดน้ำเข้า 10 จุด จากสถานีโทรมาตร สสน. (HII) รายชั่วโมง
(SLA002, SLA003, SLA007, TSDO, NPCT, KAOP, STU001, JNA002, FOP069) แบบ Thiessen
กรณีอื่น = ฝนเดิม × (1 + %) · ฝนรวมถ่วงตามพื้นที่ 1,825 ตร.กม.

## ลุ่มน้ำ
1 คลองเรียน · 2 คลองหวะ · 3 คลองมอ · 4 คลองโคกพยอม · 5 คลองอู่ตะเภา ·
6 คลองต้นยุง · 7 คลองหอยโข่ง · 8 คลองทอน · 9 คลองต่ำ · 10 คลองวาด
(ลากจาก HadYai_DIR.asc ทิศทางการไหล D8 ขนาดเซลล์ ~90 ม.) คลิกชื่อบนแผนที่หรือแถวในตารางเพื่อเน้น/ซูมลุ่มน้ำ

## การแสดงผล
น้ำท่วมเป็น polygon 3D (fill-extrusion) สูงตามความลึกจริง × ตัวคูณ "ความสูงน้ำ" วางใต้ชั้นอาคารของแผนที่ฐาน
อาคาร 3D ของ Longdo ที่สูงกว่าผิวน้ำจึงโผล่พ้นน้ำ · ปุ่ม 3D/2D สลับมุมมอง · ปุ่ม ⟳ หมุนกล้องอัตโนมัติ
หมุน/เอียงเอง: คลิกขวาลาก หรือ Ctrl+ลาก (มือถือใช้สองนิ้ว)

## ความเรียบของ polygon (tools/make_vector_scenarios.py)
`UP = 4` ขยายกริด 4 เท่า · `SIGMA = 0.7` Gaussian (หน่วยเซลล์เดิม) · `CHAIKIN = 2` ตัดมุมให้โค้งมน
polygon เป็นแบบสะสม (ลึก ≥ ค่าช่วง) วาดซ้อนจากตื้นไปลึก จึงไม่มีรอยแยกระหว่างช่วงสี
SIGMA มาก = เรียบขึ้นแต่ลำน้ำแคบจะเบลอ · ลด SIGMA เหลือ 0.4–0.5 ถ้าต้องการรายละเอียดลำน้ำ
