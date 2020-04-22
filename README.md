## CN210
This repository is created to be a guidance and summary of what I have learned from CN210 computer architecture course.
Repository นี้ถูกสร้างขึ้นเพื่อสรุปเเละเเนะนำเนื้อหาวิชาที่ได้เรียนในวิชา CN210 สถาปัตยกรรมคอมพิวเตอร์

## เนื้อหาวิชา
พื้นฐานการทำงานของคอมพิวเตอร์ ชุดคำสั่งต่างๆของ CPU (โดยในวิชานี้จะใช้ MIPS ในการศึกษา ดังนั้นชุดคำสั่งที่กล่าวถึงจะหมายถึง MIPS instruction ทั้งสิ้น) `ทุกชุดคำสั่งของ MIPS จะมีขนาด 32 bit` 

#### MIPS Instruction format
ชุดคำสั่งของ MIPS มีสามชนิด

|type/bits|31 - 26|25 - 21|20 - 16|15 - 11|10 - 6|5 - 0|
|---------|-------|-------|-------|-------|------|-----|
|R-type   |op code|$rs    |$rt    |$rd    |shamt | func|
|I-type   |op code|$rs    |$rt    |ims(15 - 0)|
|J-type   |op code|       |       |address(25 - 0)|

#### R-type 
ชุดคำสั่งสำหรับการคำนวนทางตรรกศาสตร์ ทุกคำสั่ง R-type จะใช้ `opcode 000000` โดยเเต่ละคำสั่งจะใช้ Function ในการระบุการทำงานของเเต่ละชุดคำสั่ง
```
op-code   Funct
  add     100000  บวก
  sub     100010  ลบ
  and     100100  และ
  or      100101  หรือ
  slt     101010  Signed comparison
```
โดย R-type มีรูปแบบดังนี้


|R-type |       |      |       |       |      |
|-------|-------|------|-------|-------| ---- |
|op code|$rs     |$rt    |$rd     |shamt  | func |
| 6 bits|5 bits |5 bits|5 bits |5 bits |6 bits|

2. I-type
3. J-type
## ส่งการบ้าน

#### CLIP1
ในคลิปที่ 1 จะเป็นการพูดถึงการทำงานของชุดคำสั่ง `ADD`
<br>[clip1]()

#### CLIP2

#### CLIP3

#### CLIP4
พูดถึงการทำงานของคำสั่ง lw [clip4](https://drive.google.com/open?id=1MWs46dEnK21W6binPPvU7I_iTcDpInNu)

#### CLIP5
พูดถึงการทำงานของคำสั่ง beq [clip5](https://drive.google.com/open?id=1-XlfTLiHj0VFS1AFilVCNCJtwjmj0-P_)
#### CLIP6
พูดถึงสัญญาณที่ใช้ในการทำงานชุดคำสั่งแบบ R-type [clip6](https://drive.google.com/open?id=1GWHg1gYD5LIL0P6IzxH3C_XdkvpbKLST)
#### CLIP7
ระบบคอมพิวเตอร์แบบ pipelining
