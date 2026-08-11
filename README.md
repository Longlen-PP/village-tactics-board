# Village Tactics Board

กระดานแทคติกฟุตบอล — จัดผู้เล่นสองทีม เปลี่ยนแผนการเล่น วาดเส้นแทคติก มาร์กโซนบนสนาม
และตอนนี้ **sync แบบ real-time ผ่าน Firebase** ให้หลายคนเปิดดูพร้อมกันได้ มีระบบล็อกการแก้ไขไว้เฉพาะแอดมิน

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
