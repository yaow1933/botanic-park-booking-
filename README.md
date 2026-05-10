[README.md](https://github.com/user-attachments/files/27565840/README.md)
# ระบบจองกิจกรรม Botanic Park - GitHub Pages

ระบบจองกิจกรรมอุทยานพฤกษศาสตร์ ม.วลัยลักษณ์ — แยกส่วน Frontend (HTML) ขึ้น GitHub Pages โดยใช้ Apps Script เป็น Backend API เท่านั้น

## ✅ ข้อดีของวิธีนี้

- **ไม่มีหน้าแจ้งเตือน Google** ปรากฏให้ผู้ใช้เห็น
- โหลดเร็วขึ้น (Static Hosting จาก GitHub)
- URL สวยกว่า เช่น `https://yourname.github.io/botanic-park`
- ผูก Custom Domain ได้ฟรี

---

## 🏗️ สถาปัตยกรรม

```
[ผู้ใช้]
    ↓
[GitHub Pages] ← ผู้ใช้เห็นแค่หน้านี้ (ไม่มี warning)
    ↓ fetch()
[Apps Script API] ← ทำงานเงียบๆ อยู่เบื้องหลัง
    ↓
[Google Sheets]
```

---

## 📋 ขั้นตอนติดตั้ง (15 นาที)

### ขั้นที่ 1: อัปเดต Apps Script

1. เปิด [script.google.com](https://script.google.com) → เปิดโปรเจกต์เดิมของคุณ
2. **ลบโค้ดเก่าทั้งหมด** ในไฟล์ `Code.gs`
3. คัดลอกโค้ดจากไฟล์ `Code.gs` (ที่ผมส่งให้) ไปวางแทน
4. **ลบไฟล์ `Index.html`** ในโปรเจกต์ Apps Script (ไม่ใช้แล้ว — Frontend ย้ายไป GitHub แทน)
5. กด **Save** (💾)

### ขั้นที่ 2: Deploy Apps Script ใหม่

1. คลิก **Deploy** → **Manage deployments**
2. คลิกไอคอนดินสอ ✏️ ที่ Deployment เดิม
3. ตั้งค่า:
   - **Version**: New version
   - **Execute as**: `Me (your email)` ⚠️ สำคัญ
   - **Who has access**: `Anyone` ⚠️ สำคัญ — เลือก **"Anyone"** ไม่ใช่ "Anyone with Google account"
4. คลิก **Deploy**
5. **คัดลอก Web App URL** เก็บไว้ (จะเอาไปใช้ขั้นถัดไป)

> ⚠️ ถ้าครั้งแรกขอ Authorize ให้กดอนุญาตทุกอย่าง — เป็นการให้สิทธิ์โค้ดเข้าถึง Sheet/Mail ของ**คุณ** ไม่ใช่ของผู้ใช้ทั่วไป

### ขั้นที่ 3: ตั้งค่า index.html

1. เปิดไฟล์ `index.html`
2. ค้นหาบรรทัด:
   ```javascript
   const API_URL = 'PASTE_YOUR_APPS_SCRIPT_WEB_APP_URL_HERE';
   ```
3. แทนที่ด้วย URL ที่ได้จากขั้นที่ 2 เช่น:
   ```javascript
   const API_URL = 'https://script.google.com/macros/s/AKfycbxxxxxxx/exec';
   ```
4. บันทึกไฟล์

### ขั้นที่ 4: อัปโหลดขึ้น GitHub

#### ผ่านหน้าเว็บ (ง่ายสุด)

1. ไปที่ [github.com](https://github.com) → **New repository**
2. ตั้งชื่อ เช่น `botanic-park-booking` → กด **Create**
3. คลิก **uploading an existing file**
4. ลากไฟล์ `index.html` เข้าไป → กด **Commit changes**

#### ผ่าน Git CLI (สำหรับคนที่คุ้นเคย)

```bash
git init
git add index.html
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/botanic-park-booking.git
git push -u origin main
```

### ขั้นที่ 5: เปิด GitHub Pages

1. ใน Repository → **Settings** → **Pages**
2. ที่ **Source** เลือก **Deploy from a branch**
3. เลือก Branch: `main` / Folder: `/ (root)` → **Save**
4. รอ 1-2 นาที จะได้ URL: `https://YOUR_USERNAME.github.io/botanic-park-booking/`

---

## 🎉 เสร็จแล้ว!

เปิด URL ของ GitHub Pages → **ไม่มีหน้าแจ้งเตือน Google อีกต่อไป** 🎊

---

## 🔧 การปรับแก้ภายหลัง

| อยากแก้อะไร | แก้ที่ไหน |
|---|---|
| หน้าตา/ดีไซน์เว็บ | `index.html` แล้ว push ขึ้น GitHub |
| รหัสผ่าน Admin | `Code.gs` บรรทัด `ADMIN_PASSWORD = "1234"` แล้ว Deploy ใหม่ |
| เพิ่ม/แก้กิจกรรม | แก้ในชีท `Settings` ตามปกติ — รีเฟรชเว็บก็เห็นเลย |
| รูปแบบอีเมล | `Code.gs` ฟังก์ชัน `sendQuoteEmail` แล้ว Deploy ใหม่ |

> ⚠️ **เมื่อแก้ `Code.gs` ทุกครั้ง** ต้อง Deploy → Manage deployments → ✏️ → New version → Deploy
> URL จะเหมือนเดิม ไม่ต้องไปแก้ใน `index.html`

---

## 🛠️ Troubleshooting

### ❌ เปิดเว็บแล้วเห็น "⚠️ ยังไม่ได้ตั้งค่า API_URL"
→ ลืมแก้ตัวแปร `API_URL` ใน `index.html`

### ❌ เห็น "โหลดข้อมูลไม่สำเร็จ" / Error
1. ตรวจสอบว่า Apps Script Deploy เป็น **Anyone** (ไม่ใช่ Anyone with Google account)
2. ลองเปิด `API_URL` ตรงๆ ในเบราว์เซอร์ — ถ้าเห็น JSON `{"ok":true,...}` แสดงว่า API ทำงาน
3. เปิด DevTools (F12) → Console ดู error

### ❌ ส่งจองแล้วไม่ขึ้นใน Sheet
1. เช็คชื่อ Sheet ตรงกับใน `Code.gs` (ปกติคือ `Sheet1`)
2. ดู Apps Script → Executions ว่ามี error อะไรไหม

### ❌ Calendar ไม่แสดงงานที่ยืนยัน
- เช็คว่าคอลัมน์ B มีคำว่า "ยืนยันแล้ว" (ตรงเป๊ะ ไม่มีช่องว่างเกิน)
- เช็คคอลัมน์ E (วันที่) เป็น Date format ที่ถูกต้อง

---

## 🔒 ข้อแนะนำด้านความปลอดภัย

1. **เปลี่ยนรหัส Admin** จาก `1234` เป็นรหัสที่แข็งแรง (ใน `Code.gs`)
2. **อย่า Commit URL Apps Script ขึ้น Public Repo** หากกังวล (แต่ตัว Apps Script เองเช็คสิทธิ์ผู้ใช้ที่ระดับฟังก์ชัน Admin อยู่แล้ว)
3. หากต้องการความปลอดภัยสูง พิจารณาใช้ Firebase Auth + Cloud Functions แทน

---

## 📝 ไฟล์ในโปรเจกต์

```
botanic-park-booking/
├── index.html      ← ขึ้น GitHub Pages
├── Code.gs         ← ใส่ใน Apps Script Editor
└── README.md       ← คู่มือนี้
```

---

มีคำถามถามได้เลยครับ! 🌿
