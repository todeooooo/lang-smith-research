# LangSmith

## Introduction

LangSmith เป็นเครื่องมือสำหรับติดตาม วิเคราะห์ และประเมินการทำงานของระบบที่ใช้ LLM ช่วยให้ผู้พัฒนาเข้าใจพฤติกรรมของระบบได้ทั้งในระดับภาพรวมและเชิงลึก

## Tracing Overview Dashboard

ผู้พัฒนาสามารถตรวจสอบภาพรวมการทำงานของระบบได้จากหน้า **Tracing Overview Dashboard** ซึ่งแสดงข้อมูลสรุประดับ Application ในช่วง 7 วันย้อนหลัง (รูปที่ 1)

Dashboard นี้ช่วยให้ประเมินสถานะของระบบได้อย่างรวดเร็ว ทั้งด้านปริมาณการใช้งาน ประสิทธิภาพ ความเสถียร และต้นทุน ก่อนเลือกเจาะลึกไปวิเคราะห์ trace ในระดับ execution ต่อไป

![Tracing Overview Dashboard ของ LangSmith](images/tracing-overview.png)  
*รูปที่ 1: Tracing Overview Dashboard ของ LangSmith แสดงตัวชี้วัดหลักในช่วง 7 วัน*

จากรูปที่ 1 ตาราง Tracing Overview แสดงข้อมูลสำคัญของแต่ละ Application โดยมีความหมายดังนี้

- **Name**: ชื่อ Application หรือ Project ที่ส่ง trace เข้า LangSmith  
- **Most Recent Run (7D)**: เวลาที่มีการรัน trace ล่าสุดในช่วง 7 วัน  
- **Feedback (7D)**: จำนวน feedback ที่ใช้ประเมินคุณภาพผลลัพธ์  
- **Trace Count (7D)**: จำนวน trace ทั้งหมดที่เกิดขึ้น  
- **Error Rate (7D)**: สัดส่วน trace ที่เกิดข้อผิดพลาด  
- **P50 Latency (7D)**: เวลาเฉลี่ยของการประมวลผลในกรณีทั่วไป  
- **P99 Latency (7D)**: เวลาในกรณีที่ช้าที่สุด 1% (worst-case)  
- **% Streaming (7D)**: สัดส่วนการตอบกลับแบบ streaming  
- **Total Tokens (7D)**: จำนวน token ที่ใช้งานทั้งหมด  
- **Total Cost (7D)**: ค่าใช้จ่ายรวมจากการเรียกใช้ LLM  

## Summary

Tracing Overview Dashboard ช่วยให้ผู้ใช้ประเมินสุขภาพของระบบ LLM ในระดับภาพรวมได้อย่างรวดเร็ว มองเห็นแนวโน้มการใช้งาน ปัญหาที่เกิดขึ้น และจุดคอขวดของระบบ พร้อมทั้งเชื่อมโยงประสิทธิภาพเข้ากับต้นทุนการใช้งาน เพื่อใช้เป็นจุดเริ่มต้นในการเลือก trace ที่ควรนำไปวิเคราะห์เชิงลึกในระดับการทำงานแต่ละ run ต่อไป

## Runs → Traces

หน้า Runs → Traces ใช้สำหรับแสดงรายการ trace รายตัวที่เกิดขึ้นจากการทำงานของระบบ LLM โดยแต่ละแถวแทนการประมวลผลคำขอหนึ่งครั้งแบบ end-to-end ตั้งแต่รับ input จนได้ output สุดท้าย ดังแสดงในรูปที่ 2

![Runs → Traces Dashboard ของ LangSmith](images/runs-traces.png)
*รูปที่ 2: หน้า Runs → Traces ของ LangSmith แสดงตาราง trace รายตัว พร้อมแผงสถิติ (Stats) และตัวกรอง (Filter Shortcuts)*

### ตาราง Runs (Traces Table)

ตารางแสดงรายละเอียดของ trace แต่ละรายการ โดยคอลัมน์หลักมีความหมายดังนี้

- **Name**: ชื่อของ run หรือ component ที่เป็นต้นทางของ trace (เช่น LangGraph)
- **Input**: ข้อมูลหรือ prompt ที่ผู้ใช้ส่งเข้ามาในแต่ละ run
- **Output**: ผลลัพธ์ที่ระบบ LLM สร้างขึ้น
- **Error**: แสดงสถานะหรือข้อความ error หาก run นั้นล้มเหลว
- **Start Time**: เวลาที่เริ่มต้นการทำงานของ trace
- **Latency**: เวลาที่ใช้ในการประมวลผลทั้งหมดของ run
- **Dataset**: การเชื่อมโยง trace กับ dataset (ถ้ามี)
- **Annotation Queue**: สถานะการส่ง trace ไปทำ annotation หรือ review
- **Tokens**: จำนวน token ที่ใช้ใน trace นั้น
- **Cost**: ค่าใช้จ่ายที่เกิดจากการรัน trace
- **First Token**: เวลาที่ token แรกถูกส่งกลับ (สำคัญในกรณี streaming)
- **Tags**: tag ที่ใช้จัดกลุ่มหรือระบุลักษณะของ trace
- **Metadata**: ข้อมูลเพิ่มเติมของ run เช่น depth หรือ custom attribute
- **Feedback**: feedback หรือคะแนนที่ผูกกับ trace
- **Reference Example**: การเชื่อม trace กับตัวอย่างอ้างอิงสำหรับ evaluation

---

### Stats Panel (ด้านขวา)

แผง Stats แสดงข้อมูลสรุปของ trace ทั้งหมดในช่วงเวลาที่เลือก โดยประกอบด้วย

- **Run Count**: จำนวน trace ทั้งหมด
- **Total Tokens**: จำนวน token ที่ใช้รวมทั้งหมด
- **Median Tokens**: จำนวน token เฉลี่ยระดับมัธยฐานต่อ run
- **Error Rate**: อัตราส่วนของ trace ที่เกิดข้อผิดพลาด
- **% Streaming**: สัดส่วนของ trace ที่ตอบกลับแบบ streaming
- **Latency (P50 / P99)**: ค่า latency ในระดับปกติ (P50) และกรณีช้าที่สุด (P99)

ข้อมูลส่วนนี้ช่วยให้เห็นสุขภาพระบบในภาพรวมโดยไม่ต้องไล่ดู trace ทีละรายการ

---

### Filter Shortcuts

Filter Shortcuts ใช้สำหรับคัดกรอง trace ตามเงื่อนไขต่าง ๆ เพื่อช่วยในการวิเคราะห์ โดยตัวกรองหลักประกอบด้วย

- **Input**: กรอง trace จากข้อความหรือ pattern ของ input
- **Output**: กรองจากเนื้อหา output ที่ได้
- **Run Name**: กรองตามชื่อ run หรือ component
- **Run Type**: แยกประเภท run เช่น chain, LLM, tool
- **Status**: กรองตามสถานะของ run เช่น success หรือ error
- **Tag**: กรอง trace ตาม tag ที่กำหนดไว้
- **Metadata**: กรองจาก metadata หรือ attribute เฉพาะ

Filter Shortcuts ช่วยให้ผู้พัฒนาสามารถค้นหา trace ที่ต้องการได้อย่างรวดเร็ว โดยเฉพาะในระบบที่มีจำนวน run จำนวนมาก
