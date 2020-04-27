## CN210
This repository is created to be a guidance and summary of what I have learned from CN210 computer architecture course.
Repository นี้ถูกสร้างขึ้นเพื่อสรุปเเละเเนะนำเนื้อหาวิชาที่ได้เรียนในวิชา CN210 สถาปัตยกรรมคอมพิวเตอร์

## เนื้อหาวิชา
พื้นฐานการทำงานของคอมพิวเตอร์ ชุดคำสั่งต่างๆของ CPU (โดยในวิชานี้จะใช้ MIPS ในการศึกษา ดังนั้นชุดคำสั่งที่กล่าวถึงจะหมายถึง MIPS instruction ทั้งสิ้น)

#### MIPS Instruction format
ชุดคำสั่งของ MIPS มีสามชนิด

|type/bits|31 - 26|25 - 21|20 - 16|15 - 11|10 - 6|5 - 0|
|---------|-------|-------|-------|-------|------|-----|
|R-type   |op code|$rs    |$rt    |$rd    |shamt | func|
|I-type   |op code|$rs    |$rt    |ims|
|J-type   |op code|       |       |address|

> **`ทุกชุดคำสั่งของ MIPS จะมีขนาด 32 bit`**

#### R-type 
ชุดคำสั่งสำหรับการคำนวนทางตรรกศาสตร์ ทุกคำสั่ง R-type จะใช้ `opcode 000000` โดยเเต่ละคำสั่งจะใช้ Function ในการระบุการทำงานของเเต่ละชุดคำสั่ง
```
 Machine
 opcode   Funct
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

```
alu $rd, $rs, $rt
```

### I-type
ชุดคำสั่งเกี่ยวกับการจัดการข้อมูล`Data tranfer` เช่น การดึงข้อมูล การเขียนข้อมูลเป็นต้นเเละยังรวมไปถึง `beq(Branch on Equal)` ด้วย
```
  Machine
  opcode   Funct   x = don't care
    lw     xxxxxx  ดึงข้อมูลมาใช้
    sw     xxxxxx  นำข้อมูลไปเก็บ
```

โดย I-type มีรูปแบบดังนี้

|I-type|    |   |               |
|------|----|---|---------------|
|op code|$rs|&rt|Value or Offset|
| 6 bits|5 bits|5 bits|16 bits  |

```
Datatranfer lw $rt, offset($rs)
            sw $rt, offset($rs)

Branch      beq $rs, $rt, offset
```

### J-type
ชุดคำสั่งเกี่ยวกับการข้ามหรือ`jump`เพื่อย้ายที่อยู่ในการทำงาน

```
Jump
Jump&Link
```

|J-type|                  |
|------|------------------|
|opcode| absolute address |
|6 bits|      26 bits     |

```
Jump       j address
Jump&Link  jal address
```

```
คอมพิวเตอร์ไม่สามารถเข้าใจภาษาระดับสูงได้ คอมพิวเตอร์เข้าใจเพียงเลขฐานสองเท่านั้น
```

#### ALU Decoder

# ส่งการบ้าน

### [CLIP1](https://www.youtube.com/watch?v=RRHZPNz_lzE&feature=youtu.be)
ในคลิปที่ 1 จะเป็นการพูดถึงการทำงานของชุดคำสั่ง `ADD`

```
ADD $rd, $rs, $rt    ภาษา Assembly เขียนแทนภาษาเครื่องเพื่อให้มนุษย์เข้าใจได้ง่าย
$ -> สัญลักษณ์แทน Register
```

ADD เป็นชุดคำสั่งแบบ R-type ซึ่งมี `opcode = 000000` และ `Funct = 100000` ซึ่ง**การทำงานของชุดคำสั่ง ADD คือ _นำข้อมูลใน $rs บวกกับ $rt เเละนำผลลัพธ์ที่ได้ไปเก็บไว้ที่ $rd_**

```
$rd = $rs + $rt
```
Ex. ADD $7, $5, $2

> คอมพิวเตอร์เข้าใจเลขฐานสองเท่านั้น

จากภาษา Assembly ที่เขียนมาข้างต้นสามารถแปลงเป็นภาษาคอมพิวเตอร์(เลขฐานสอง) โดยการดูข้อกำหนดของชุดคำสั่ง CPU ตัวนั้นๆ(กรณีนี้ MIPS) 

> กลับขึ้นไปดู ALU decoder และ R-type Instruction format

จะทราบได้ว่า ADD นั้นเป็นชุดคำสั่ง R-type ซึ่งมี opcode = 000000 และมี Function = 100000 สามารถนำมาเขียนตาม R-type format ได้เป็นเลขฐานสองว่า

> 000000 00101 00010 00111 00000 100000

โดย 

```
bits ที่ 31-26 (6 bits แรก) คือ opcode
bits ที่ 25-21 (5 bits ถัดไป) คือ Register ตำแหน่งที่ rs
bits ที่ 20-16 (5 bits ถัดไป) คือ Register ตำแหน่งที่ rt
bits ที่ 15-11 (5 bits ถัดไป) คือ Register ตำแหน่งที่ rd
bits ที่ 10-6 (5 bits ถัดไป) คือ shamt
bits ที่ 5-0 (6 bits สุดท้าย) คือ Function
```

เราสามารถแปลงชุดคำสั่งด้านบนให้เป็นเลขฐาน 16 ได้

`0000` 0001 `1010` 0010 `0011` 1000 `0010` 0000

```
 0x01A23820 
```

> แปลงเป็นเลขฐาน 16 เพื่อให้มนุษย์สามารถอ่านได้ง่ายขึ้น

### [CLIP2](https://www.youtube.com/watch?v=EfOSx5m8Fms&feature=youtu.be)

CLIP2 พูดถึงการเปลี่ยนภาษาระดับสูง(ภาษาที่มนุษย์เข้าใจได้ง่าย) เป็นภาษาเครื่องโดยใช้ภาษา Assembly ในการอธิบาย

```java
class test{
    public static void main(String[] args){
        int a = 10;  <- คอมพิวเตอร์ทำการจองพื้นที่หนึ่งในหน่วยความจำเพื่อใส่ข้อมูลว่าหน่วยความจำนี้ถูกเรียกว่า a และใส่เลข 10 ลงไป
        int b = 20;  <-__|
        int c = a + b; <-  คอมพิวเตอร์ทำการดึงข้อมูลจากหน่วยความจำ a ขึ้นมาบวกกับข้อมูลในหน่วยความจำ b เเละนำไปเก็บที่หน่วยความจำ c
    }
}
```

```
 Memory        Hex                       Binary
00000000:    08400000    0000 1000 0100 0000 0000 0000 0000 0000
00000004:    1A000000    0001 1010 0000 0000 0000 0000 0000 0000
...
01000000:    8C090004    1000 1100 0000 1001 0000 0000 0000 0100
01000004:    8D210000    1000 1101 0010 0001 0000 0000 0000 0000
01000008:    8D220004    1000 1101 0010 0010 0000 0000 0000 0100
0100000C:    00221820    0000 0000 0000 0010 0001 1000 0010 0000
01000010:    AD230008    1010 1101 0010 0011 0000 0000 0000 1000
...
1A000000:    0000000A    0000 0000 0000 0000 0000 0000 0000 1010
1A000004:    00000014    0000 0000 0000 0000 0000 0000 0001 0100
1A000008:    0000001E    0000 0000 0000 0000 0000 0000 0001 1110
```

เพื่ออธิบายการทำงานของคอมพิวเตอร์ เราจะสมมุติให้คอมพิวเตอร์ของเราเมื่อเปิดเครื่องมาจะเริ่มทำงานที่ตำแหน่ง Register ตัวที่ 00000000 ก็คือบรรทัดเเรก

```
 Memory        Hex                       Binary
00000000:    08400000    0000 1000 0100 0000 0000 0000 0000 0000    <- คอมพิวเตอร์สมมุติของเราเมื่อเปิดเครื่องคอมพิวเตอร์ทำงานที่ตำเเหน่งนี้ก่อน(ตำแหน่ง 0)
00000004:    1A000000    0001 1010 0000 0000 0000 0000 0000 0000
...
01000000:    8C090004    1000 1100 0000 1001 0000 0000 0000 0100
01000004:    8D210000    1000 1101 0010 0001 0000 0000 0000 0000
01000008:    8D220004    1000 1101 0010 0010 0000 0000 0000 0100
0100000C:    00221820    0000 0000 0000 0010 0001 1000 0010 0000
01000010:    AD230008    1010 1101 0010 0011 0000 0000 0000 1000
...
1A000000:    0000000A    0000 0000 0000 0000 0000 0000 0000 1010
1A000004:    00000014    0000 0000 0000 0000 0000 0000 0001 0100
1A000008:    0000001E    0000 0000 0000 0000 0000 0000 0001 1110
```

เราจะเห็นว่าที่ตำแหน่งนี้ที่คอมพิวเตอร์ทำงาน มีชุดคำสั่งหนึ่งรออยู่เเล้วซึ่งเขียนเป็นเลขฐาน 16 `08400000`

> **คอมพิวเตอร์เมื่อเห็นชุดคำสั่งจะดูที่ opcode(6 bits แรก) ก่อนเพื่อดูว่าเป็นชุดคำสั่งแบบไหน**

เมื่อคอมพิวเตอร์เห็น opcode เป็น 000010 ก็ทราบทันทีว่าเป็นคำสั่ง Jump (เนื่องจาก ALU decoder) คอมพิวเตอร์จึงทราบว่าจะต้องดู bits ที่ 25-0 ต่อเพื่อนำ Address ไปใช้ในการ Jump

> Address ในการ Jump คือ 01000000 

```
 Memory        Hex                       Binary
00000000:    08400000    0000 1000 0100 0000 0000 0000 0000 0000    J 01000000  <-  Jump ไปที่ตำแหน่ง 01000000
00000004:    1A000000    0001 1010 0000 0000 0000 0000 0000 0000
...
01000000:    8C090004    1000 1100 0000 1001 0000 0000 0000 0100  <-  คอมพิวเตอร์จึง Jump ข้ามไปทำงานที่ตำแหน่ง 01000000
01000004:    8D210000    1000 1101 0010 0001 0000 0000 0000 0000
01000008:    8D220004    1000 1101 0010 0010 0000 0000 0000 0100
0100000C:    00221820    0000 0000 0000 0010 0001 1000 0010 0000
01000010:    AD230008    1010 1101 0010 0011 0000 0000 0000 1000
...
1A000000:    0000000A    0000 0000 0000 0000 0000 0000 0000 1010
1A000004:    00000014    0000 0000 0000 0000 0000 0000 0001 0100
1A000008:    0000001E    0000 0000 0000 0000 0000 0000 0001 1110
```
เมื่อ Jump เสร็จคอมพิวเตอร์ อ่านชุดคำสั่งที่อยู่ที่ Register ที่ Jump ไป
```
 Memory        Hex                       Binary
00000000:    08400000    0000 1000 0100 0000 0000 0000 0000 0000
00000004:    1A000000    0001 1010 0000 0000 0000 0000 0000 0000
...
01000000:    8C090004    1000 1100 0000 1001 0000 0000 0000 0100  <- ดู opcode ก่อน(6 bits แรกของชุดคำสั่ง)
01000004:    8D210000    1000 1101 0010 0001 0000 0000 0000 0000     
01000008:    8D220004    1000 1101 0010 0010 0000 0000 0000 0100     เมื่อทราบว่าเป็น lw จึงไปดู $rs, $rt เเละ offset เพื่อทำคำสั่ง lw ต่อไป
0100000C:    00221820    0000 0000 0000 0010 0001 1000 0010 0000     
01000010:    AD230008    1010 1101 0010 0011 0000 0000 0000 1000
...
1A000000:    0000000A    0000 0000 0000 0000 0000 0000 0000 1010
1A000004:    00000014    0000 0000 0000 0000 0000 0000 0001 0100
1A000008:    0000001E    0000 0000 0000 0000 0000 0000 0001 1110
```
เมื่อคอมพิวเตอร์เห็นชุดคำสั่งก็จะดูที่ opcode ก่อนจะได้ทราบว่าชุดคำสั่งบอกให้ทำอะไร

> 100011 เป็น opcode ของ lw

```
$rs = Register ตำแหน่งที่ 00000 (0)
$rt = Register ตำแหน่งที่ 01001 (9)
offset = 0000000000000100 (4)
```

> lw $9, $0(4)

คอมพิวเตอร์ก็จะทำการนำข้อมูลจาก $rs(offset) ไปเก็บไว้ที่ $rt และเมื่อจบก็จะทำคำสั่งต่อไป

```
 Memory        Hex                       Binary
00000000:    08400000    0000 1000 0100 0000 0000 0000 0000 0000
00000004:    1A000000    0001 1010 0000 0000 0000 0000 0000 0000
...
01000000:    8C090004    1000 1100 0000 1001 0000 0000 0000 0100    lw $9, $0(4)
01000004:    8D210000    1000 1101 0010 0001 0000 0000 0000 0000    lw $1, $5(0)   *สองบรรทัดถัดมาเป็นคำสั่งเดียวกันจึงข้อละไว้
01000008:    8D220004    1000 1101 0010 0010 0000 0000 0000 0100    lw $0, $5(4)
0100000C:    00221820    0000 0000 0000 0010 0001 1000 0010 0000  -> ที่ตำแหน่งนี้ opcode = 000000 ซึ่งเป็นของ R-type ต้องไปดูที่ Funct จึงจะทราบได้ว่าคำสั่งให้ทำอะไร  
01000010:    AD230008    1010 1101 0010 0011 0000 0000 0000 1000    Funct = 100000 ซึ่งเป็นคำสั่ง ADD
...
1A000000:    0000000A    0000 0000 0000 0000 0000 0000 0000 1010
1A000004:    00000014    0000 0000 0000 0000 0000 0000 0001 0100
1A000008:    0000001E    0000 0000 0000 0000 0000 0000 0001 1110
```
> ตำแหน่งของ Memory จะเปลี่ยนไปทีละ 32 bits เนื่องจากแต่ละชุดคำสั่งมีขนาด 32 bits พอดี

เมื่อคอมพิวเตอร์ทราบว่าชุดคำสั่งเป็นคำสั่ง ADD จึงอ่าน $rs, $rt, $rd เพื่อนำไปทำตามคำสั่งต่อไป

```
 Memory        Hex                       Binary
00000000:    08400000    0000 1000 0100 0000 0000 0000 0000 0000
00000004:    1A000000    0001 1010 0000 0000 0000 0000 0000 0000
...
01000000:    8C090004    1000 1100 0000 1001 0000 0000 0000 0100    lw $9, $0(4)
01000004:    8D210000    1000 1101 0010 0001 0000 0000 0000 0000    lw $1, $5(0)
01000008:    8D220004    1000 1101 0010 0010 0000 0000 0000 0100    lw $0, $5(4)
0100000C:    00221820    0000 0000 0010 0010 0001 1000 0010 0000    ADD $3, $1, $2
01000010:    AD230008    1010 1101 0010 0011 0000 0000 0000 1000    sw $3, $9(8)
...
1A000000:    0000000A    0000 0000 0000 0000 0000 0000 0000 1010    a
1A000004:    00000014    0000 0000 0000 0000 0000 0000 0001 0100    b
1A000008:    0000001E    0000 0000 0000 0000 0000 0000 0001 1110    c
```

### [CLIP3](https://www.youtube.com/watch?v=koQ6yo35F8g&feature=youtu.be)
ความแตกต่างระหว่าง CPU แบบ Multi-cycle และ Single-cycle

**CPU แบบ Single-Cycle**
![comp-arch-2012-choompol-49](https://user-images.githubusercontent.com/61135042/80242926-653b0200-8690-11ea-846a-03d1d3dcc801.jpg)
1. Havard Architecture
2. Mux 4 units
3. 1 ALU 2 Add
4. 2 Memory
5. Each Instruction ends in 1 Clock-cycle **ทำให้เกิดความล่าช้าเพราะต้องรอให้คำสั่งก่อนหน้าเสร็จเรียบร้อยก่อน**

**CPU แบบ Multi-Cycle**
![comp-arch-2012-choompol-60](https://user-images.githubusercontent.com/61135042/80238516-c8289b00-8688-11ea-98b5-533b6f842219.jpg)
1. Voneuman Architecture
2. Mux 5 units
3. 1 ALU unit
4. 1 Memory
5. Each Instruction has it's own clock-cycle **ทุกคำสั่งไม่มีการเสียเวลาเกิดขึ้นเพราะใช้เวลาพอดี**

### [CLIP4](https://drive.google.com/open?id=1MWs46dEnK21W6binPPvU7I_iTcDpInNu)

พูดถึงขั้นตอนการทำงานของคำสั่ง lw

##### T1 : Instruction fetch
- IR = Memory[PC]
- PC = PC + 4

![comp-arch-2012-choompol-60](https://user-images.githubusercontent.com/61135042/80243595-b5669400-8691-11ea-8735-fb508d84c4b9.jpg)
เป็นการนำชุดคำสั่งมาเก็บไว้ที่ **IR(Instruction Register)** และนำ PC ไปบวก 4 เพื่อดึงชุดคำสั่งถัดไปมารอไว้ที่ PC

##### T2 : Instruction decode / register fetch
- A = REG[IR[25-21]]
- B = REG[IR[20-15]]
- ALUOUT = PC + (sign-extend (IR[15-0] << 2))

![comp-arch-2012-choompol-61](https://user-images.githubusercontent.com/61135042/80243611-bf889280-8691-11ea-855f-2437c9c4cb7a.jpg)
ในขั้นตอนนี้ REG จะดึงข้อมูลจาก IR มาใช้งานซึ่งจะถูกแยกเป็นส่วนๆ
- bits ที่ 31-26 เป็น opcode เราจะไม่สนใจ
- bits ที่ 25-21 จะถูกอ่านนำไปเก็บไว้ใน A
- bits ที่ 20-16 จะถูกอ่านและนำไปเก็บไว้ใน B
- bits ที่ 15-0 จะถูกนำไปเพิ่ม bits(sign extend) เป็น 32 bits เเละ shift ไปทางซ้าย 2 bits จากนั้นจะถูกนำไปบวกกับ PC

> ทั้งหมดเกิดขึ้นพร้อมกันในขั้นตอนเดียว

#### T3 : Execution , address computation , branch/jump completion
- ALUOUT = A + sign-extend(IR[15-0])

![comp-arch-2012-choompol-62](https://user-images.githubusercontent.com/61135042/80243639-c8796400-8691-11ea-9efa-788932c2fc09.jpg)
ในขั้นตอนนี้ข้อมูลที่อยู่ใน A จะถูกนำมาบวกกับ IR[15-0](offset) โดย offset จะมาจาก IR[15-0] ทำการ Sign extend เเละเข้า mux เลยโดนไม่ผ่าน Shift เพื่อนำมาบวกกับ A ตามขั้นตอน

#### T4 : Memory access or R-type completion
- Load : MDR = Memory[ALUOUT] or
- Store : Memory[ALUOUT] = B

![comp-arch-2012-choompol-63](https://user-images.githubusercontent.com/61135042/80243665-d16a3580-8691-11ea-82e3-543df6eb4e2a.jpg)
ขั้นตอนนี้ค่าที่ออกมาจาก ALUOUT จะถูกนำมาเก็บไว้ใน MDR

#### T5 : Memoey read completion
- Load : REG[IR[20-16]] = MDR

![comp-arch-2012-choompol-64](https://user-images.githubusercontent.com/61135042/80243679-daf39d80-8691-11ea-8275-9ae80da3ed87.jpg)
ข้อมูลใน MDR จะถูกนำไปเขียนลงใน REG

#### [CLIP5](https://drive.google.com/open?id=1-XlfTLiHj0VFS1AFilVCNCJtwjmj0-P_)

พูดถึงการทำงานของคำสั่ง beq

##### T1 : Instruction fetch
- IR = Memory[PC]
- PC = PC + 4

![comp-arch-2012-choompol-60](https://user-images.githubusercontent.com/61135042/80243595-b5669400-8691-11ea-8735-fb508d84c4b9.jpg)
เป็นการนำชุดคำสั่งมาเก็บไว้ที่ **IR(Instruction Register)** และนำ PC ไปบวก 4 เพื่อดึงชุดคำสั่งถัดไปมารอไว้ที่ PC

##### T2 : Instruction decode / register fetch
- A = REG[IR[25-21]]
- B = REG[IR[20-15]]
- ALUOUT = PC + (sign-extend (IR[15-0] << 2))

![comp-arch-2012-choompol-61](https://user-images.githubusercontent.com/61135042/80243611-bf889280-8691-11ea-855f-2437c9c4cb7a.jpg)
ในขั้นตอนนี้ REG จะดึงข้อมูลจาก IR มาใช้งานซึ่งจะถูกแยกเป็นส่วนๆ
- bits ที่ 31-26 เป็น opcode เราจะไม่สนใจ
- bits ที่ 25-21 จะถูกอ่านนำไปเก็บไว้ใน A
- bits ที่ 20-16 จะถูกอ่านและนำไปเก็บไว้ใน B
- bits ที่ 15-0 จะถูกนำไปเพิ่ม bits(sign extend) เป็น 32 bits เเละ shift ไปทางซ้าย 2 bits จากนั้นจะถูกนำไปบวกกับ PC

#### T3 : Execution , address computation , branch/jump completion
- if(A==B) then PC = ALUOUT

![comp-arch-2012-choompol-62](https://user-images.githubusercontent.com/61135042/80274333-c9e37480-8703-11ea-87d3-b0d4901cffd0.jpg)

ในขั้นตอนนี้ ALU จะนำ A - B หาก A - B == 0 เป็นจริง PC ก็จะทำการ Jump ไปตาม Address ที่ ALUOUT ส่งไปให้ก่อนหน้า

### [CLIP6](https://drive.google.com/open?id=1GWHg1gYD5LIL0P6IzxH3C_XdkvpbKLST)

พูดถึงสัญญาณที่ใช้ในการทำงานชุดคำสั่งชนิด R-type

T1 ในขั้นตอนนี้เป็นการรับค่าจาก PC ลงมาเขียนใน IR เเละดึง PC มาบวก 4 เพื่อทำการรอคำสั่งถัดไป control unit จึงทำการส่งสัญญาณ MemRead = 1 , IRWrite = 1 เพื่อให้ IR(Instruction Register) ทำการเขียนข้อมูลลงไป ในขณะเดียวกัน control unit ทำการส่งสัญญาณ ALUSrcA = 0 , ALUSrcB = 1 , ALUOP = ADD เพื่อนำค่าจาก PC เเละให้ Mux ส่งเลข 4 ออกมาเพื่อให้ ALU นำค่าทั้งสองมาบวกกันเป็น Address สำหรับคำสั้งต่อไป
![comp-arch-2012-choompol-83](https://user-images.githubusercontent.com/61135042/80276745-dcb27500-8714-11ea-8107-c29924b3b26e.jpg)

T2 เป็นการอ่าน $rs , $rt และนำ Offset + PC control unit จึงทำการส่งสีญญาณ ALUSrcA = 0 , ALUSrcB = 3 เพื่อให้ค่าที่เข้า ALU เป็น PC และ signext << 2 และ ALUOP = 0 เพื่อให้ ALU ทำการบวกเลข

ในขณะเดียวกันก็ทำการอ่าน $rs , $rt เข้าไปใน A , B ตามลำดับ
![comp-arch-2012-choompol-85](https://user-images.githubusercontent.com/61135042/80276753-e5a34680-8714-11ea-9ad7-2b54b4dad17f.jpg)

T3 ALUOUT = A op B control unit จึงทำการส่งสัญญาณมาให้ ALUSrcA = 1 เพื่อนำข้อมูลใน A และ ALUSrcB = 0 เพื่อนำข้อมูลใน B เข้า ALU และ ALUOP = 2 เพื่อนำค่าจาก IR[28-26] เข้าไปใช้เช่นกัน
![comp-arch-2012-choompol-87](https://user-images.githubusercontent.com/61135042/80276765-edfb8180-8714-11ea-94da-a65e88fd68c2.jpg)

T4 ซึ่งเป็นขั้นตอนสุดท้ายเป็นการนำค่าที่ไดเจาก ALU ส่งออกไปเก็บที่ REG[IR[15-11]] จึงมีการส่งสัญญาณ REGWrite = 0 เพื่อให้เขียนข้อมูลจาก ALUOUT ลงไปใน REG MemtoReg = 0 เพื่อส่งค่า ALUOUT ออกมาและ RegDst = 1 เพื่อให้ Mux ส่งค่า %rd เข้าไปใน Reg
![comp-arch-2012-choompol-89](https://user-images.githubusercontent.com/61135042/80276770-f5228f80-8714-11ea-9f0a-da05eeb496d9.jpg)

### CLIP7
ระบบคอมพิวเตอร์แบบ pipelining
