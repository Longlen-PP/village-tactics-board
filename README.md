# Village Tactics Board

กระดานแทคติกฟุตบอล — จัดผู้เล่นสองทีม เปลี่ยนแผนการเล่น วาดเส้นแทคติก มาร์กโซนบนสนาม
และตอนนี้ **sync แบบ real-time ผ่าน Firebase** ให้หลายคนเปิดดูพร้อมกันได้ มีระบบล็อกการแก้ไขไว้เฉพาะแอดมิน

## Tabs
- **⚽ Match Board** — สนามแทคติก: Starting XI/Bench แต่ละช่องเป็น **dropdown เลือกจาก Squad** เท่านั้น
  (แก้ชื่อ/เบอร์ตรงนี้ไม่ได้แล้ว ต้องไปแก้ที่ Squad tab) ตำแหน่งบนสนามยังลากปรับเองได้ วาดเส้น/มาร์กโซนได้เหมือนเดิม
- **🧑‍🤝‍🧑 Squad** — ทำเนียบนักเตะถาวรของทีม (เลขเสื้อ/ชื่อ/ตำแหน่ง) เพิ่ม/แก้/ลบได้ — ใครถูก add เข้ามาจะไปโผล่เป็น
  ตัวเลือกใน dropdown ของ Match Board และ Lineup tab ทันที
- **🖼️ Lineup** — การ์ดสรุปตัวจริงวันนี้ (แนวตั้ง สไตล์ตามภาพตัวอย่าง) **แยกอิสระจาก Match Board เต็มตัว** —
  มี formation dropdown + Starting XI + Bench เป็นของตัวเอง (เลือกจาก Squad คนละชุดกับ Match Board, สลับ
  ตัวจริง/สำรองแบบเดียวกัน) เปลี่ยนอะไรในนี้ไม่กระทบสนามแทคติกเลย

ทุก tab เก็บอยู่ใน Firebase node เดียวกับ `board/current` เลยไม่ต้องแก้ Security Rules เพิ่ม

## โลโก้ทีม
โลโก้ขึ้นอยู่ 2 ที่: หัวเว็บ (ทุก tab ข้างชื่อทีม, `#teamLogo`) และหัวการ์ด Lineup (`#lineupLogo`) —
ทั้งคู่อ้างอิงไฟล์เดียวกันที่ path `village-esport-logo.png` (โฟลเดอร์เดียวกับ `index.html`) ต้องเซฟไฟล์โลโก้
(PNG พื้นหลังโปร่งใส/ขาว) ไว้ที่นี่ด้วยชื่อนี้เป๊ะๆ — ถ้ายังไม่มีไฟล์ ระบบจะ fallback เป็นวงกลม "VE" สีทองแทนอัตโนมัติ
(ไม่ขึ้นไอคอนรูปหัก และไม่ทำให้ Save Image พังด้วย เพราะ broken `<img>` เคยเป็นสาเหตุที่ html2canvas capture ไม่ผ่าน)

## Save as Image
แยกปุ่มตาม tab ของตัวเอง:
- **📸 Save Board Image** (ใน Match Board tab) — capture เฉพาะสนาม (ตำแหน่ง+เส้น+โซน) เป็น PNG
- **📸 Save Lineup Image** (ใน Lineup tab) — capture การ์ด Lineup ทั้งใบ (โลโก้+รายชื่อ+ผังสนาม+แถบตัวสำรอง) เป็น PNG

ทั้งสองปุ่มใช้ไลบรารี [html2canvas](https://github.com/niklasvh/html2canvas) โหลดจาก CDN (jsdelivr) —
ต้องมีอินเทอร์เน็ตตอนกดเซฟ ถ้าเซฟไม่ผ่านให้เปิด browser console (F12) ดู error ที่ log ไว้ (`Save Board/Lineup Image failed: ...`)

## ลิงก์
- **Live site (GitHub Pages):** https://longlen-pp.github.io/village-tactics-board/
- **GitHub repo:** https://github.com/Longlen-PP/village-tactics-board
- **Claude Artifact (ต้นทาง เวอร์ชันแรกก่อนแยกมาเป็น GitHub):** https://claude.ai/code/artifact/f3bb64d5-5dd9-4d9e-b02c-fa0bf0921f46

## ระบบแอดมิน (Firebase)
- Backend: Firebase project `village-tactics-board` (Realtime Database + Authentication)
- ใครก็เปิดลิงก์ดูได้ (real-time, view only) แต่แก้ไขได้เฉพาะคนที่ล็อกอินด้วย **PIN**
- รองรับสูงสุด **5 PIN แยกกัน** (คนละบัญชี Firebase Auth: `admin1@village-tactics.local` ถึง `admin5@village-tactics.local`) — พิมพ์แค่ PIN ที่หน้าเว็บ ระบบจับคู่บัญชีให้อัตโนมัติ ไม่ต้องรู้ว่าเป็นของใคร
- จัดการ/เพิ่ม-ลบ PIN: Firebase Console → Authentication → Users
- Security Rules: Realtime Database → Rules → จำกัดสิทธิ์เขียนด้วย regex `^admin[1-5]@village-tactics\.local$`
- ใช้ฟรีตลอด — ไม่ได้ผูกบัตรเครดิต (Spark Plan) เกินโควต้าแค่ใช้งานไม่ได้ชั่วคราว ไม่มีบิลเรียกเก็บ

## แก้ไขต่อ
แก้ไฟล์ `index.html` ในโฟลเดอร์นี้ได้เลย (เป็น git repo อยู่แล้ว ไม่มีสำเนาแยกที่อื่นแล้ว) แล้ว:
```
git add index.html
git commit -m "..."
git push
```
GitHub Pages จะ deploy เว็บให้อัตโนมัติภายในไม่กี่นาที
