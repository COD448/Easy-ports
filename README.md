# Ezport

> เว็บแอปสร้างพอร์ตโฟลิโอสำหรับยื่นเข้ามหาวิทยาลัย — กรอกข้อมูลครั้งเดียว เลือกคณะที่จะยื่น
> แล้วระบบจัดหน้า เลือกสี และเลือกฟอนต์ให้เข้ากับคณะนั้น ได้ออกมาเป็นเล่ม 14 หน้าพร้อมพิมพ์ A4

> A portfolio builder for Thai university applications. Students fill in one form, pick the
> faculty they are applying to, and the app lays out a print-ready 14-page A4 book in that
> faculty's design system.

**โปรเจกต์จบ / Final-year project.** ไม่ต้องติดตั้งอะไร เปิด `index.html` ได้เลย

---

## ทำอะไรได้บ้าง / Features

- **6 ชุดดีไซน์ตามคณะ** — วิศวกรรมศาสตร์, ศิลปกรรมศาสตร์, บริหารธุรกิจ, สถาปัตยกรรมศาสตร์,
  นิเทศศาสตร์, วิทยาศาสตร์ แต่ละคณะมีชุดสี ฟอนต์ และลวดลายของตัวเอง
- **7 หน้าจอ** — หน้าแรก → เลือกสไตล์ → ข้อมูลผู้ใช้ → โปรไฟล์และการศึกษา → กิจกรรม →
  ผลงานและเกียรติบัตร → พอร์ตโฟลิโอ
- **เล่ม 14 แผ่น** — ปกหน้า + คำนำ + สารบัญ + เนื้อหา 10 หน้า + ปกหลัง
- **แก้ไขก่อนพิมพ์** — ลากย้าย ลบ และดับเบิลคลิกเพื่อพิมพ์ทับข้อความได้ทุกชิ้น
- **อัปโหลดรูป** — รูปถ่าย ตราโรงเรียน รูปกิจกรรม ผลงาน และเกียรติบัตร
  (ย่อขนาดอัตโนมัติในเบราว์เซอร์)
- **ดาวน์โหลด / พิมพ์** — ได้ไฟล์ HTML ไฟล์เดียวที่มีทุกอย่างอยู่ในตัว สั่งพิมพ์ A4 ได้ทันที
- รองรับธีมสว่างและมืด และปรับตามขนาดจอ

## เริ่มใช้งาน / Getting started

เปิดไฟล์ `index.html` ด้วยเบราว์เซอร์ได้เลย — ไม่ต้องมี build step ไม่ต้องลง dependency

Or serve it locally if you prefer a real origin:

```bash
python -m http.server 8000
```

Then open <http://localhost:8000>.

## โครงสร้างไฟล์ / Project structure

```
.
├── index.html                      # the entire app — markup, styles and script in one file
├── docs/
│   └── SPEC.md                     # product specification (screens, page structure, design rules)
├── .github/workflows/
│   └── deploy-pages.yml            # publishes the site to GitHub Pages on every push to main
├── .gitignore
└── README.md
```

`index.html` is intentionally a single self-contained file: it has no build step, no bundler and no
runtime dependencies, so it runs from `file://`, from any static host, and from inside an offline
classroom machine. The only external request is the Google Fonts stylesheet.

## เผยแพร่ขึ้น GitHub Pages / Deploy

The included workflow publishes the site automatically. After pushing:

1. Go to **Settings → Pages** in the repository
2. Under **Build and deployment → Source**, choose **GitHub Actions**
3. Push to `main` — the site goes live at `https://<username>.github.io/<repo>/`

## หมายเหตุทางเทคนิค / Technical notes

**หน่วยเป็น `cqw` ทั้งเล่ม.** `.sheet` ตั้ง `container-type: inline-size` และ
`aspect-ratio: 210/297` ทุกขนาดข้างในหน้าใช้หน่วย `cqw` ทั้งหมด ทำให้กฎ CSS ชุดเดียว
แสดงผลเหมือนกันทั้งบนจอ ในแถบรูปย่อ และตอนพิมพ์จริงที่ 210 มม. ตำแหน่งที่ผู้ใช้ลากย้าย
ก็เก็บเป็น `cqw` ด้วยเหตุผลเดียวกัน

**ฟอนต์ไทย.** ใช้ฟอนต์ที่รองรับภาษาไทยทั้งหมด — Mitr และ Sarabun สำหรับตัวเว็บ ส่วน
Chakra Petch, Trirong, Bai Jamjuree, Kanit และ IBM Plex Sans Thai เป็นฟอนต์หัวเรื่องของแต่ละคณะ

**รูปภาพ.** ย่อเหลือด้านยาวสุด 1100px แปลงเป็น JPEG คุณภาพ 0.82 ก่อนเก็บเป็น data URI
(ตราโรงเรียนเก็บเป็น PNG เพื่อรักษาพื้นหลังโปร่งใส)

**การบันทึกไฟล์.** หน้าเว็บตรวจว่าตัวเองรันอยู่ที่ไหน ถ้าอยู่ใน Claude artifact จะเรียก
`downloads` capability ของโฮสต์ ถ้าเป็นเว็บธรรมดาจะใช้ Blob download ปกติ

## ข้อจำกัดที่ยังมี / Known limitations

- **ยังไม่บันทึกข้อมูล** — ปิดหน้าเว็บแล้วข้อมูลหาย ยังไม่มี backend และไม่ได้ใช้
  `localStorage` ถ้าจะใช้งานจริงควรเพิ่มส่วนนี้ก่อน
- **จำนวนหน้าคงที่ 10 หน้า** — คนที่มี 2 กิจกรรมกับคนที่มี 12 กิจกรรมได้จำนวนหน้าเท่ากัน
  ช่องที่ไม่มีข้อมูลจะแสดงเป็นกรอบว่างไว้
- **ปุ่มพิมพ์** — เรียก `window.print()` ของเบราว์เซอร์ ซึ่งอาจถูกบล็อกเมื่อฝังอยู่ใน iframe
  แนะนำให้กดดาวน์โหลดแล้วเปิดไฟล์เพื่อสั่งพิมพ์
- แสดงผลได้ดีกับเบราว์เซอร์รุ่นใหม่ (ใช้ CSS container queries และ `aspect-ratio`)

## License

ยังไม่ได้กำหนดสัญญาอนุญาต — เพิ่มไฟล์ `LICENSE` เองได้ถ้าต้องการเผยแพร่แบบโอเพนซอร์ส

No license has been chosen yet. Add a `LICENSE` file if you intend to open-source this.
