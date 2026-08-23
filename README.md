# Rex

**Rex** คือชุด instruction (คำสั่งกำหนดพฤติกรรม) สำหรับสร้างผู้ช่วยส่วนตัวที่เน้นการตอบอย่างมีเหตุผล ค้นคว้า วิเคราะห์ ช่วยตัดสินใจ และให้ second opinion (ความเห็นที่สอง) โดยสามารถเพิ่ม skill (ทักษะเฉพาะทาง) ได้จากไฟล์ใน repository นี้

Repository นี้ตั้งใจให้คนอื่น **fork/clone แล้วนำไปผูกกับ AI agent/runtime ของตัวเอง** จากนั้นปรับ `USER.md` และ `MEMORY.md` ให้เป็นของเจ้าของแต่ละคน

> **สำคัญ:** การ clone repository อย่างเดียวไม่ได้ทำให้ ChatGPT, Claude, Codex หรือ AI runtime อื่นรู้จัก Rex โดยอัตโนมัติ ตัว runtime ต้องถูกตั้งค่าให้โหลดไฟล์ instruction เหล่านี้ และถ้าต้องการเรียก Rex ผ่านผู้ช่วยหลัก ก็ต้องมี routing (การส่งต่อ) ตามที่อธิบายด้านล่าง
>
> **คุณTop owner architecture:** เมื่อใช้งานใน Rex Project/เครื่องของคุณTop และพบ canonical `Agent-Team` Repo ให้ใช้ `/a <staff>`, `/p <project>`, `/s [project]` จาก Agent-Team ตาม bridge ใน `AGENTS.md`. ไม่ต้องสร้าง ChatGPT Project แยกต่อ code project และไม่ต้องติดตั้ง/คัดลอก Sara, Dex หรือ Staff อื่นเข้า repository นี้

## โครงสร้าง

```text
Agent_rex/
├── AGENTS.md
├── SOUL.md
├── USER.md
├── MEMORY.md
└── skill/
    └── adviser.md
```

### `AGENTS.md`

Entry point (จุดเริ่มต้น) ของ Rex บอก runtime ว่าต้องโหลดไฟล์อะไร, เลือก skill อย่างไร และควรตีความคำสั่งเรียก Rex แบบภาษาธรรมชาติอย่างไร

### `SOUL.md`

ตัวตนหลักของ Rex — Rex คือใคร, มีหน้าที่อะไร, ตอบแบบไหน, ใช้ source อย่างไร และต้องระบุ confidence (ความมั่นใจ) อย่างไร

### `USER.md`

ข้อมูลของเจ้าของ Rex

ไฟล์ต้นฉบับตั้งใจปล่อยว่าง ผู้ที่นำ Rex ไปใช้ควรกรอกเฉพาะข้อมูลที่อยากให้ Rex ใช้เป็น context (บริบท) เช่น:

```md
# User

- ชื่อที่อยากให้เรียก:
- ภาษาในการตอบ:
- งาน/บทบาท:
- เป้าหมายหลัก:
- ความชอบในการตัดสินใจ:
- ข้อจำกัดที่ควรรู้:
```

ไม่ควรใส่ password, API key, token หรือ secret (ข้อมูลลับ) ลง repository

### `MEMORY.md`

ความรู้ระยะยาวที่ Rex ควรจำและนำมาใช้ซ้ำในอนาคต

ไฟล์ต้นฉบับตั้งใจปล่อยว่างเช่นกัน แต่ละคนกำหนด policy (นโยบาย) เองได้ว่าจะให้ Rex เพิ่ม memory เมื่อใด เช่น เมื่อเจ้าของสั่งให้จำ หรือเมื่อ runtime มีระบบ memory ที่ได้รับอนุญาตให้บันทึก

### `skill/adviser.md`

Skill **ที่ปรึกษา** สำหรับสองกลุ่มงานที่เชื่อมกัน:

- ช่วยคิด ชั่งน้ำหนักทางเลือก วิเคราะห์ข้อดี–ข้อเสีย และเสนอ recommendation (ข้อเสนอแนะ)
- review/audit (ตรวจทาน/ตรวจสอบ) plan, design, PR, diff และ code change แบบ end-to-end (ตั้งแต่ต้นจนจบ)

สำหรับงาน technical review (การตรวจเชิงเทคนิค) Rex จะไม่ดูแค่ diff แต่จะตั้งคำถามก่อนว่าสิ่งที่กำลังทำจำเป็นหรือไม่ มีวิธีที่ง่ายกว่าหรือไม่ แล้วจึง trace (ไล่เส้นทาง) code path จริงเพื่อยืนยันว่า behavior ที่อ้างไว้เกิดขึ้นจริง

---

# วิธีนำ Rex ไปใช้

## 1. Fork หรือ clone repository

```bash
git clone https://github.com/Dreamfinal/Agent_rex.git
cd Agent_rex
```

ถ้าต้องการปรับ Rex เป็นของตัวเอง แนะนำให้ fork repository ก่อน เพื่อเก็บ `USER.md`, `MEMORY.md` และ skill เพิ่มเติมของตัวเองแยกจากต้นฉบับ

## 2. กรอก `USER.md`

ใส่เฉพาะ context ที่อยากให้ Rex รู้เกี่ยวกับเจ้าของ เช่น รูปแบบการทำงาน เป้าหมาย เกณฑ์ตัดสินใจ หรือ preference (ความชอบ)

ไม่จำเป็นต้องกรอกทุกอย่างตั้งแต่วันแรก สามารถค่อย ๆ เพิ่มได้

## 3. กำหนดวิธีใช้ `MEMORY.md`

เลือกอย่างใดอย่างหนึ่งตาม runtime ที่ใช้:

- **Manual memory** — เจ้าของแก้ `MEMORY.md` เอง
- **Agent-managed memory** — อนุญาตให้ agent เพิ่มเฉพาะข้อมูลที่ควรจำระยะยาวตามกติกาที่กำหนด
- **External memory** — runtime มี memory/database ของตัวเอง ก็สามารถใช้ `MEMORY.md` เป็น seed (ข้อมูลตั้งต้น) หรือไม่ใช้ไฟล์นี้ก็ได้

ควรหลีกเลี่ยงการ commit ข้อมูลส่วนตัวหรือ secret ที่ไม่ควรอยู่บน GitHub โดยเฉพาะ repository แบบ public

## 4. ให้ AI runtime โหลด Rex

ลำดับ context ที่แนะนำคือ:

```text
AGENTS.md
  ├─ SOUL.md
  ├─ USER.md
  ├─ MEMORY.md
  └─ skill/<relevant-skill>.md
```

ถ้า runtime รองรับ `AGENTS.md` เป็น project instruction (คำสั่งระดับโปรเจกต์) อยู่แล้ว ให้เปิด workspace ที่ repository นี้และให้ runtime อ่าน `AGENTS.md`

ถ้า runtime ไม่รองรับโดยตรง ให้ตั้ง system/project prompt (คำสั่งระดับระบบ/โปรเจกต์) ของผู้ช่วยหลักให้โหลดเนื้อหาจาก `AGENTS.md` และไฟล์ที่อ้างถึง

---

# การเรียก Rex ตอนแชท

เป้าหมายคือ **ผู้ใช้ไม่ควรต้องจำ command พิเศษ** สามารถพูดกับผู้ช่วยหลักแบบธรรมชาติได้เลย

ตัวอย่าง:

```text
ลองถาม Rex หน่อย ว่ามีความเห็นยังไงบ้าง
```

หรือ:

```text
ถาม Rex ให้หน่อย
ให้ Rex ช่วยคิดเรื่องนี้ที
Rex คิดว่ายังไง
ขอความเห็นจาก Rex
ลองให้ Rex review แผนนี้หน่อย
```

คำเหล่านี้ควรถูกตีความตาม **intent (เจตนา)** ไม่ใช่ match แค่ประโยคตรงตัว

## พฤติกรรมที่ผู้ช่วยหลักควรทำ

สมมติว่าก่อนหน้านี้ผู้ใช้กำลังคุยเรื่อง architecture, แผนงาน, code หรือการตัดสินใจบางอย่าง แล้วพิมพ์ว่า:

```text
ลองถาม Rex หน่อย ว่ามีความเห็นยังไงบ้าง
```

ผู้ช่วยหลักควร:

1. ใช้เรื่องที่กำลังคุยอยู่เป็น subject (หัวข้อ) โดยไม่บังคับให้ผู้ใช้พิมพ์ซ้ำ
2. รวม context ที่จำเป็น เช่น ข้อความก่อนหน้า, code, plan หรือ artifact ที่กำลังพิจารณา
3. โหลด `SOUL.md`, `USER.md`, `MEMORY.md`
4. ถ้าเป็นการขอคำปรึกษา, second opinion, review หรือช่วยตัดสินใจ ให้โหลด `skill/adviser.md`
5. ส่งคำถามและ context เข้า Rex
6. คืนคำตอบของ Rex ให้ผู้ใช้

ถ้าบทสนทนามีหลายเรื่องจนไม่รู้จริง ๆ ว่า "เรื่องนี้" หมายถึงอะไร จึงค่อยถาม clarification (คำถามเพื่อความชัดเจน) สั้น ๆ

---

# วิธีเชื่อม Rex กับผู้ช่วยหลัก

มีสองรูปแบบหลัก

## แบบ A — Runtime รองรับ named agent / subagent

วิธีนี้ตรงกับประสบการณ์ใช้งานที่ตั้งใจไว้มากที่สุด

1. Register (ลงทะเบียน) agent ชื่อ **Rex**
2. ใช้ `SOUL.md` เป็น instruction หลักของ Rex
3. แนบ `USER.md` และ `MEMORY.md` เป็น persistent context (บริบทถาวร) ตามความสามารถของ runtime
4. โหลด skill ตาม intent ของคำถาม
5. ตั้ง routing rule ให้คำสั่งที่มีเจตนา เช่น `ถาม Rex`, `ให้ Rex`, `Rex คิดว่า...` เรียก named agent `Rex`
6. ส่ง conversation context ที่เกี่ยวข้องไปพร้อมกับคำถาม

แนวคิดของ routing สามารถเขียนเป็น pseudocode (โค้ดจำลอง) ได้ประมาณนี้:

```text
if user_intent asks_for("Rex"):
    subject = explicit_subject_or_current_conversation_topic()
    skills = select_relevant_skills(subject)

    answer = invoke_agent(
        name="Rex",
        instructions=[SOUL, USER, MEMORY, skills],
        context=subject
    )

    return answer
```

ไม่จำเป็นต้อง match ตัวพิมพ์เล็ก/ใหญ่หรือประโยคแบบ exact string (ข้อความตรงตัว) สิ่งสำคัญคือเข้าใจว่าผู้ใช้กำลัง **ขอความเห็นจาก Rex**

## แบบ B — Runtime ไม่มี subagent

ยังใช้ Rex ได้ โดยให้ผู้ช่วยหลักสลับเข้า Rex persona (บทบาท Rex) ภายใน conversation เดิม:

1. ผู้ช่วยหลักตรวจพบ intent ว่าผู้ใช้กำลังเรียก Rex
2. โหลด instruction/context ของ Rex
3. ใช้ skill ที่เกี่ยวข้อง
4. ตอบตามกติกาของ Rex

ภายนอกผู้ใช้ยังสามารถพูดว่า:

```text
ลองถาม Rex หน่อย ว่ามีความเห็นยังไงบ้าง
```

ได้เหมือนเดิม แม้ implementation ภายในจะไม่ได้สร้าง process/model แยกจริง

---

# ตัวอย่าง flow

ผู้ใช้กำลังคุยกับผู้ช่วยหลัก:

```text
ฉันคิดว่าจะย้ายระบบ cache ทั้งหมดไป Redis แล้วแก้ API ให้เขียน cache ทุก request
```

จากนั้นผู้ใช้พิมพ์:

```text
ลองถาม Rex หน่อย ว่ามีความเห็นยังไงบ้าง
```

ผู้ช่วยหลักควรส่งแผน Redis ข้างต้นเป็น context ให้ Rex และเลือก `skill/adviser.md`

Rex ควรเริ่มจากถามว่าเป้าหมายจริงของการย้ายคืออะไร/ถ้าข้อมูลเพียงพอก็สรุป intent, ตรวจว่ามีวิธีที่เล็กกว่าหรือใช้ของเดิมได้หรือไม่, วิเคราะห์ trade-off, ตรวจ assumption และจึงให้ recommendation พร้อม confidence

ผู้ใช้ไม่ควรต้อง copy แผนเดิมหรือพิมพ์ `/adviser` ซ้ำอีกครั้ง

---

# การเพิ่ม skill ใหม่

เพิ่ม Markdown file ใน `skill/` และกำหนดอย่างน้อย:

```yaml
---
name: skill-name
description: "อธิบายว่า skill นี้ทำอะไร และควรถูกเรียกเมื่อผู้ใช้มี intent แบบไหน"
---
```

จากนั้นเขียน workflow, operating rules และ output format ของ skill ให้ชัด

ควรเขียน `description` ให้ครอบคลุมภาษาที่ผู้ใช้มีแนวโน้มจะพูดจริง เพื่อให้ runtime เลือก skill จาก intent ได้ง่ายขึ้น

---

# แนวคิดหลักของ Rex

Rex ไม่ได้ถูกออกแบบมาให้เห็นด้วยกับเจ้าของทุกเรื่อง หน้าที่ของ Rex คือช่วยให้เจ้าของ **คิดได้ดีขึ้นและตัดสินใจบนข้อมูลที่ดีขึ้น**

ดังนั้น Rex ควร:

- เริ่มจากข้อสรุปก่อน แล้วอธิบายเหตุผล
- แสดงข้อดี–ข้อเสียเมื่อมีหลายทางเลือก
- ตั้งคำถามกับ assumption เมื่อจำเป็น
- ตรวจ source เมื่อข้อเท็จจริงเปลี่ยนแปลงตามเวลา
- ไม่เดาเมื่อไม่มีหลักฐาน
- ระบุ confidence ทุกคำตอบตาม `SOUL.md`
- สำหรับแผน/PR/code change ให้ถามเสมอว่ามีวิธีที่ง่ายกว่าและเสี่ยงน้อยกว่าหรือไม่ ก่อนลงรายละเอียด

## License / การนำไปต่อ

ผู้ที่ fork/clone repository ควรตรวจสอบและกำหนด license (สัญญาอนุญาต) ที่เหมาะสมกับการแจกจ่ายหรือดัดแปลงต่อ หาก repository ต้นฉบับยังไม่มีไฟล์ `LICENSE` จะยังไม่มี license แบบ open-source ที่ระบุสิทธิ์การนำไปใช้ไว้อย่างชัดเจน
