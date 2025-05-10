## Data Types

ทุกค่าใน Rust มี Data Type ที่เจาะจง ซึ่งบอก Rust ว่าข้อมูลประเภทใดกำลังถูกระบุ เพื่อให้ Rust รู้ว่าจะทำงานกับข้อมูลนั้นอย่างไร เราจะดู Data Type สองกลุ่มย่อยคือ Scalar และ Compound

โปรดทราบว่า Rust เป็นภาษา _Statically Typed_ ซึ่งหมายความว่า Rust จะต้องทราบ Type ของตัวแปรทั้งหมดใน Compile Time โดยปกติแล้ว Compiler สามารถอนุมาน Type ที่เราต้องการใช้ได้จากค่าและวิธีการใช้งานของเรา ในกรณีที่มี Type ที่เป็นไปได้หลาย Type เช่น เมื่อเราแปลง `String` เป็น Type ตัวเลขโดยใช้ `parse` ในส่วน [“Comparing the Guess to the Secret
Number”][comparing-the-guess-to-the-secret-number]<!-- ignore -->  เราจำเป็นต้องเพิ่ม Type Annotation ดังนี้:

```rust
let guess: u32 = "42".parse().expect("Not a number!");
```

ในกรณีนี้ เราได้เพิ่ม Type Annotation `: u32` เพื่อบอก Rust ว่าเราต้องการให้ตัวแปร guess เป็น Integer ไม่มีเครื่องหมายขนาด 32 บิต

```console
{{#include ../listings/ch03-common-programming-concepts/output-only-01-no-type-annotations/output.txt}}
```

คุณจะเห็น Type Annotation ที่แตกต่างกันสำหรับ Data Types อื่นๆ

### Scalar Types

Scalar Type แสดงถึงค่าเดียว Rust มี Scalar Type หลักสี่ประเภท: Integers (จำนวนเต็ม), Floating-point numbers (จำนวนทศนิยม), Booleans (ค่าความจริง) และ Characters (ตัวอักษร) คุณอาจคุ้นเคยกับ Types เหล่านี้จากภาษา Programming อื่นๆ มาแล้ว มาดูวิธีการทำงานของ Types เหล่านี้ใน Rust กันครับ

#### Integer Types

Integer คือตัวเลขที่ไม่มีส่วนที่เป็นเศษส่วน เราได้ใช้ Integer Type หนึ่งไปแล้วใน บทที่ 2 คือ Type `u32` การประกาศ Type นี้บ่งชี้ว่าค่าที่มันเชื่อมโยงด้วยควรเป็น Integer ไม่มีเครื่องหมาย (Signed Integer Types จะเริ่มต้นด้วย `i` แทน `u`) ที่ใช้พื้นที่ 32 Bits ตารางที่ 3-1 แสดง Integer Types Built-in ใน Rust เราสามารถใช้ Variant ใดก็ได้เหล่านี้เพื่อประกาศ Type ของค่า Integer ครับ

<span class="caption">Table 3-1: Integer Types in Rust</span>

| Length  | Signed  | Unsigned |
| ------- | ------- | -------- |
| 8-bit   | `i8`    | `u8`     |
| 16-bit  | `i16`   | `u16`    |
| 32-bit  | `i32`   | `u32`    |
| 64-bit  | `i64`   | `u64`    |
| 128-bit | `i128`  | `u128`   |
| arch    | `isize` | `usize`  |

แต่ละ Variant สามารถเป็นได้ทั้ง _Signed_ หรือ _Unsigned_ และมีขนาดที่ชัดเจน Signed และ Unsigned หมายถึงความเป็นไปได้ที่ตัวเลขจะเป็นค่าลบ กล่าวคือ ตัวเลขจำเป็นต้องมีเครื่องหมาย (Signed) หรือไม่ หรือตัวเลขนั้นจะเป็นค่าบวกเสมอและสามารถแสดงได้โดยไม่มีเครื่องหมาย (Unsigned) เหมือนกับการเขียนตัวเลขบนกระดาษ: เมื่อเครื่องหมายมีความสำคัญ ตัวเลขจะแสดงด้วยเครื่องหมายบวกหรือลบ อย่างไรก็ตาม เมื่อสามารถสันนิษฐานได้อย่างปลอดภัยว่าตัวเลขเป็นบวก จะแสดงโดยไม่มีเครื่องหมาย ตัวเลข Signed จะถูกจัดเก็บโดยใช้การแทนค่าแบบ [two’s complement][twos-complement]<!-- ignore
-->

แต่ละ Variant ที่เป็น Signed สามารถจัดเก็บตัวเลขได้ตั้งแต่ -(2&lt;sup>n - 1&lt;/sup>) ถึง 2&lt;sup>n - 1&lt;/sup> - 1 โดยรวม โดยที่ n คือจำนวน Bits ที่ Variant นั้นใช้ ดังนั้น i8 สามารถจัดเก็บตัวเลขได้ตั้งแต่ -(2&lt;sup>7&lt;/sup>) ถึง 2&lt;sup>7&lt;/sup> - 1 ซึ่งเท่ากับ -128 ถึง 127 Variant ที่เป็น Unsigned สามารถจัดเก็บตัวเลขได้ตั้งแต่ 0 ถึง 2&lt;sup>n&lt;/sup> - 1 ดังนั้น u8 สามารถจัดเก็บตัวเลขได้ตั้งแต่ 0 ถึง 2&lt;sup>8&lt;/sup> - 1 ซึ่งเท่ากับ 0 ถึง 255

นอกจากนี้ Type `isize` และ `usize` ยังขึ้นอยู่กับ Architecture ของ Computer ที่โปรแกรมของคุณกำลังรันอยู่ ซึ่งระบุไว้ในตารางเป็น "arch": 64 Bits หากคุณใช้ Architecture 64-bit และ 32 Bits หากคุณใช้ Architecture 32-bit

คุณสามารถเขียน Integer Literals ได้หลายรูปแบบดังแสดงในตารางที่ 3-2 โปรดทราบว่า Number Literals ที่สามารถเป็น Numeric Types ได้หลายประเภท อนุญาตให้มี Type Suffix เช่น `57u8` เพื่อระบุ Type Number Literals ยังสามารถใช้ _ เป็นตัวคั่นเพื่อให้อ่านตัวเลขได้ง่ายขึ้น เช่น `1_000` ซึ่งจะมีค่าเท่ากับ `1000`

<span class="caption">Table 3-2: Integer Literals in Rust</span>

| Number literals  | Example       |
| ---------------- | ------------- |
| Decimal          | `98_222`      |
| Hex              | `0xff`        |
| Octal            | `0o77`        |
| Binary           | `0b1111_0000` |
| Byte (`u8` only) | `b'A'`        |

แล้วคุณจะรู้ได้อย่างไรว่าควรใช้ Integer Type ไหน? หากคุณไม่แน่ใจ ค่า Default ของ Rust โดยทั่วไปก็เป็นจุดเริ่มต้นที่ดี: Integer Types จะ Default เป็น `i32` สถานการณ์หลักที่คุณจะใช้ `isize` หรือ `usize` คือเมื่อทำการ Indexing Collection บางประเภทครับ


> ##### Integer Overflow
>
>สมมติว่าคุณมีตัวแปร Type `u8` ซึ่งสามารถเก็บค่าได้ระหว่าง 0 ถึง 255 หากคุณพยายามเปลี่ยนตัวแปรนั้นให้มีค่าที่อยู่นอกช่วง เช่น 256 จะเกิดสิ่งที่เรียกว่า Integer Overflow ซึ่งอาจส่งผลให้เกิดพฤติกรรมได้สองแบบ
เมื่อคุณ Compile ใน Debug Mode Rust จะมีการตรวจสอบ _Integer Overflow_ ซึ่งจะทำให้โปรแกรมของคุณ _Panic_ (หยุดทำงานพร้อม Error) ใน Runtime หากเกิดพฤติกรรมนี้ Rust ใช้คำว่า _Panicking_ เมื่อโปรแกรม Exit พร้อม Error ซึ่งเราจะกล่าวถึง Panics ในรายละเอียดเพิ่มเติมในส่วน[“Unrecoverable Errors with
> `panic!`”][unrecoverable-errors-with-panic]<!-- ignore --> ในบทที่ 9
>เมื่อคุณ Compile ใน Release Mode ด้วย Flag `--release` Rust _จะไม่_ มีการตรวจสอบ Integer Overflow ที่ทำให้เกิด Panics แทนที่ หากเกิด Overflow Rust จะทำการ _Two's Complement Wrapping_ กล่าวโดยย่อคือ ค่าที่มากกว่าค่าสูงสุดที่ Type นั้นสามารถเก็บได้ จะ "Wrap Around" กลับไปยังค่าต่ำสุดที่ Type นั้นสามารถเก็บได้ ในกรณีของ `u8` ค่า 256 จะกลายเป็น 0 ค่า 257 จะกลายเป็น 1 และอื่นๆ โปรแกรมจะไม่ Panic แต่ตัวแปรจะมีค่าที่ไม่น่าจะเป็นไปตามที่คุณคาดหวัง การพึ่งพาพฤติกรรมการ Wrapping ของ Integer Overflow ถือเป็น Error
>
>เพื่อจัดการกับความเป็นไปได้ของการ Overflow อย่างชัดเจน คุณสามารถใช้ Method กลุ่มต่างๆ เหล่านี้ที่ Standard Library จัดเตรียมไว้ให้สำหรับ Primitive Numeric Types:
> - Wrap ในทุก Mode ด้วย Methods `wrapping_*` เช่น `wrapping_add`
> - คืนค่า `None` หากมีการ Overflow ด้วย Methods `checked_*` 
> - คืนค่าและ Boolean ที่ระบุว่ามีการ Overflow หรือไม่ ด้วย Methods `overflowing_*`
> - Saturate ที่ค่าต่ำสุดหรือสูงสุดของ Type นั้นๆ ด้วย Methods `saturating_*`

#### Floating-Point Types

Rust ยังมี Primitive Types สองประเภทสำหรับ _Floating-Point Numbers_ ซึ่งเป็นตัวเลขที่มีจุดทศนิยม Floating-Point Types ของ Rust คือ `f32` และ `f64` ซึ่งมีขนาด 32 Bits และ 64 Bits ตามลำดับ Type Default คือ `f64` เนื่องจากบน CPU สมัยใหม่นั้น มีความเร็วใกล้เคียงกับ `f32` แต่มีความแม่นยำมากกว่า Floating-Point Types ทั้งหมดเป็น Signed

นี่คือตัวอย่างที่แสดงการใช้งาน Floating-Point Numbers:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-06-floating-point/src/main.rs}}
```

ตัวเลขทศนิยมถูกแสดงตามมาตรฐาน IEEE-754

#### การดำเนินการทางตัวเลข

Rust สนับสนุนการดำเนินการทางคณิตศาสตร์พื้นฐานที่คุณคาดหวังสำหรับ Number Types ทั้งหมด: การบวก การลบ การคูณ การหาร และการหาเศษ การหาร Integer จะปัดเศษลงเป็น Integer ที่ใกล้เคียงที่สุดเข้าหาศูนย์ โค้ดต่อไปนี้แสดงวิธีที่คุณจะใช้การดำเนินการทางตัวเลขแต่ละประเภทในคำสั่ง `let:`

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-07-numeric-operations/src/main.rs}}
```
แต่ละ Expression ในคำสั่งเหล่านี้ใช้ Operator ทางคณิตศาสตร์และประเมินค่าเป็นค่าเดียว ซึ่งจากนั้นจะถูกผูกไว้กับตัวแปร[Appendix
B][appendix_b]<!-- ignore -->  มีรายการ Operator ทั้งหมดที่ Rust จัดเตรียมไว้ให้ครับ

#### The Boolean Type
เช่นเดียวกับภาษา Programming อื่นๆ ส่วนใหญ่ Boolean Type ใน Rust มีค่าที่เป็นไปได้สองค่าคือ `true` และ `false` Booleans มีขนาดหนึ่ง Byte Type Boolean ใน Rust ถูกระบุโดยใช้ `bool` ตัวอย่างเช่น:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-08-boolean/src/main.rs}}
```
วิธีหลักในการใช้ค่า Boolean คือผ่าน Conditionals เช่น Expression `if` เราจะกล่าวถึงวิธีการทำงานของ Expression `if` ใน Rust ในส่วน [“Control
Flow”][control-flow]<!-- ignore -->

#### The Character Type

Type `char` ของ Rust เป็น Type ตัวอักษรพื้นฐานที่สุดของภาษา นี่คือตัวอย่างการประกาศค่า `char:`

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-09-char/src/main.rs}}
```


โปรดทราบว่าเรากำหนด `char` Literals ด้วย Single Quotes (') ซึ่งแตกต่างจาก String Literals ที่ใช้ Double Quotes (") Type `char` ของ Rust มีขนาดสี่ Bytes และแสดงถึง Unicode Scalar Value ซึ่งหมายความว่ามันสามารถแสดงอะไรได้มากกว่าแค่ ASCII มาก ตัวอักษรที่มีเครื่องหมายกำกับ ตัวอักษรจีน ญี่ปุ่น และเกาหลี Emoji และ Zero-Width Spaces ล้วนเป็นค่า char ที่ถูกต้องใน Rust Unicode Scalar Values มีช่วงตั้งแต่ `U+0000` ถึง `U+D7FF` และ `U+E000` ถึง `U+10FFFF` โดยรวม อย่างไรก็ตาม "ตัวอักษร" ไม่ใช่แนวคิดที่ชัดเจนใน Unicode ดังนั้นสิ่งที่สัญชาตญาณของมนุษย์เข้าใจว่าเป็น "ตัวอักษร" อาจไม่ตรงกับสิ่งที่ `char` เป็นใน Rust เราจะกล่าวถึงหัวข้อนี้ในรายละเอียดใน บทที่ 8 [“Storing UTF-8 Encoded Text with
Strings”][strings]<!-- ignore -->


### Compound Types

_Compound Types_ สามารถรวมค่าหลายค่าเข้าด้วยกันเป็น Type เดียว Rust มี Compound Types พื้นฐานสองประเภทคือ Tuples และ Arrays

#### The Tuple Type

_Tuple_ เป็นวิธีทั่วไปในการรวมค่าจำนวนหนึ่งที่มี Type ที่หลากหลายเข้าด้วยกันเป็น Compound Type เดียว Tuples มีความยาวคงที่: เมื่อประกาศแล้ว ขนาดของมันจะไม่สามารถเพิ่มขึ้นหรือลดลงได้

เราสร้าง Tuple โดยการเขียนรายการค่าที่คั่นด้วยเครื่องหมายจุลภาคภายในวงเล็บ แต่ละตำแหน่งใน Tuple มี Type และ Types ของค่าที่แตกต่างกันใน Tuple ไม่จำเป็นต้องเป็น Type เดียวกัน เราได้เพิ่ม Type Annotation ที่เป็น Optional ในตัวอย่างนี้:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-10-tuples/src/main.rs}}
```

ตัวแปร `tup` ถูกผูกไว้กับ Tuple ที่มีค่าแรกเป็น i32 ค่าที่สองเป็น f64 และค่าที่สามเป็น u8 Tuple ทั้งหมดถือเป็น Single Compound Element

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-11-destructuring-tuples/src/main.rs}}
```

โปรแกรมนี้เริ่มต้นด้วยการสร้าง Tuple และผูกไว้กับตัวแปร `tup` จากนั้นใช้ Pattern กับ `let` เพื่อนำ `tup` มาแยกออกเป็นสามตัวแปรที่แตกต่างกันคือ `x`,`y` และ `z` สิ่งนี้เรียกว่า _Destructuring_ เพราะมันแบ่ง Tuple เดียวออกเป็นสามส่วน สุดท้าย โปรแกรมจะพิมพ์ค่าของ `y` ซึ่งคือ `6.4` ครับ


เรายังสามารถเข้าถึง Element ของ Tuple ได้โดยตรงโดยใช้เครื่องหมายจุด (`.`) ตามด้วย Index ของค่าที่เราต้องการเข้าถึง ตัวอย่างเช่น:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-12-tuple-indexing/src/main.rs}}
```

โปรแกรมนี้สร้าง Tuple `x` และจากนั้นเข้าถึงแต่ละ Element ของ Tuple โดยใช้ Index ที่เกี่ยวข้อง เช่นเดียวกับภาษา Programming ส่วนใหญ่ Index แรกใน Tuple คือ 0

Tuple ที่ไม่มีค่าใดๆ มีชื่อพิเศษว่า _Unit_ ค่านี้และ Type ที่สอดคล้องกันถูกเขียนแทนด้วย `()` และแสดงถึงค่าว่างเปล่าหรือ Type Return ที่ว่างเปล่า Expressions จะ Return ค่า Unit โดยปริยาย หากไม่ได้ Return ค่าอื่นใด

#### The Array Type

อีกวิธีหนึ่งในการมี Collection ของค่าหลายค่าคือการใช้ _Array_ ซึ่งแตกต่างจาก Tuple ตรงที่ Elements ทุกตัวใน Array จะต้องมี Type เดียวกัน และแตกต่างจาก Arrays ในภาษาอื่นๆ ตรงที่ Arrays ใน Rust มีความยาวคงที่

เราเขียนค่าใน Array เป็นรายการที่คั่นด้วยเครื่องหมายจุลภาคภายในวงเล็บเหลี่ยม:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-13-arrays/src/main.rs}}
```


Arrays มีประโยชน์เมื่อคุณต้องการให้ Data ของคุณถูก Allocate บน Stack เช่นเดียวกับ Types อื่นๆ ที่เราได้เห็นไปแล้ว แทนที่จะเป็น Heap (เราจะกล่าวถึง Stack และ Heap ในรายละเอียดเพิ่มเติมใน บทที่ 4 [Chapter 4][stack-and-heap]<!-- ignore --> ) หรือเมื่อคุณต้องการให้แน่ใจว่าคุณมีจำนวน Elements ที่คงที่เสมอ อย่างไรก็ตาม Array ไม่มีความยืดหยุ่นเท่ากับ _Vector_ Type Vector เป็น Collection Type ที่คล้ายกันซึ่งจัดเตรียมโดย Standard Library และ สามารถ เพิ่มหรือลดขนาดได้ หากคุณไม่แน่ใจว่าจะใช้อาร์เรย์หรือ Vector โอกาสที่คุณควรใช้ Vector มีมากกว่า บทที่ 8 [Chapter 8][vectors]<!-- ignore -->  กล่าวถึง Vectors ในรายละเอียดเพิ่มเติม


อย่างไรก็ตาม Arrays มีประโยชน์มากกว่าเมื่อคุณทราบว่าจำนวน Elements จะไม่จำเป็นต้องเปลี่ยนแปลง ตัวอย่างเช่น หากคุณกำลังใช้ชื่อเดือนในโปรแกรม คุณอาจจะใช้ Array แทนที่จะเป็น Vector เพราะคุณรู้ว่ามันจะมี 12 Elements เสมอ:

```rust
let months = ["January", "February", "March", "April", "May", "June", "July",
              "August", "September", "October", "November", "December"];
```

คุณเขียน Type ของ Array โดยใช้วงเล็บเหลี่ยม โดยมี Type ของแต่ละ Element คั่นด้วยเครื่องหมายเซมิโคลอน และตามด้วยจำนวน Elements ใน Array ดังนี้:

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```

`i32` คือ Type ของแต่ละ Element ใน Array และเลข 5 ที่อยู่หลังเครื่องหมายเซมิโคลอนบ่งชี้ว่า Array นี้มี 5 Elements

และดังที่คุณกล่าวถึง คุณยังสามารถสร้าง Array ที่มีค่าเริ่มต้นเดียวกันสำหรับทุก Elements ได้โดยการระบุค่าเริ่มต้น ตามด้วยเครื่องหมายเซมิโคลอน และความยาวของ Array ภายในวงเล็บเหลี่ยม ดังตัวอย่าง:

```rust
let a = [3; 5];
```

ในกรณีนี้ `a` จะเป็น Array ที่มี Integer จำนวน `5` ตัว โดยแต่ละตัวมีค่าเริ่มต้นเป็น `3` ซึ่งเทียบเท่ากับการเขียน `let a = [3, 3, 3, 3, 3];`

##### การเข้าถึง Elements ของ Array

Array คือ Memory Block เดียวที่มีขนาดคงที่และทราบขนาด ซึ่งสามารถ Allocate บน Stack ได้ คุณสามารถเข้าถึง Elements ของ Array ได้โดยใช้ Index ดังนี้:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-14-array-indexing/src/main.rs}}
```
ในตัวอย่างนี้ ตัวแปรที่ชื่อว่า `first` จะได้รับค่า `1` เนื่องจากเป็นค่าที่ Index `[0]` ใน Array และตัวแปรที่ชื่อว่า `second` จะได้รับค่า `2` จาก Index `[1]` ใน Array

##### การเข้าถึง Element ของ Array ที่ไม่ถูกต้อง

มาดูกันว่าจะเกิดอะไรขึ้นถ้าคุณพยายามเข้าถึง Element ของ Array ที่อยู่นอกขอบเขตของ Array สมมติว่าคุณรันโค้ดนี้ ซึ่งคล้ายกับเกมทายตัวเลขใน บทที่ 2 เพื่อรับ Index ของ Array จากผู้ใช้:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,panics
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-15-invalid-array-access/src/main.rs}}
```
โค้ดนี้ Compile ได้สำเร็จ หากคุณรันโค้ดนี้โดยใช้ `cargo run` และป้อน `0`, `1`, `2`, `3` หรือ `4` โปรแกรมจะพิมพ์ค่าที่สอดคล้องกันที่ Index นั้นใน Array ออกมา แต่ถ้าคุณป้อนตัวเลขที่เกินขอบเขตของ Array เช่น `10` คุณจะเห็น Output ดังนี้:


<!-- manual-regeneration
cd listings/ch03-common-programming-concepts/no-listing-15-invalid-array-access
cargo run
10
-->

```console
thread 'main' panicked at src/main.rs:19:19:
index out of bounds: the len is 5 but the index is 10
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

โปรแกรมส่งผลให้เกิด Error ใน _Runtime_ ในขณะที่ใช้ค่าที่ไม่ถูกต้องในการดำเนินการ Indexing โปรแกรม Exit พร้อม Error Message และไม่ได้ Execute คำสั่ง `println!` สุดท้าย เมื่อคุณพยายามเข้าถึง Element โดยใช้ Indexing Rust จะตรวจสอบว่า Index ที่คุณระบุนั้นน้อยกว่าความยาวของ Array หรือไม่ หาก Index มากกว่าหรือเท่ากับความยาว Rust จะ Panic การตรวจสอบนี้จำเป็นต้องเกิดขึ้นใน Runtime โดยเฉพาะอย่างยิ่งในกรณีนี้ เนื่องจาก Compiler ไม่สามารถทราบค่าที่ผู้ใช้จะป้อนเมื่อรันโค้ดในภายหลังได้

นี่เป็นตัวอย่างของหลักการ Memory Safety ของ Rust ในการทำงาน ในภาษา Low-Level หลายภาษา การตรวจสอบประเภทนี้ไม่ได้ทำ และเมื่อคุณระบุ Index ที่ไม่ถูกต้อง Memory ที่ไม่ถูกต้องอาจถูกเข้าถึงได้ Rust ปกป้องคุณจาก Error ประเภทนี้โดยการ Exit ทันทีแทนที่จะอนุญาตให้เข้าถึง Memory และดำเนินการต่อไป บทที่ 9 กล่าวถึงการจัดการ Error ของ Rust และวิธีที่คุณสามารถเขียนโค้ดที่อ่านง่าย ปลอดภัย ซึ่งทั้งไม่ Panic และไม่อนุญาตให้เข้าถึง Memory ที่ไม่ถูกต้องครับ

[comparing-the-guess-to-the-secret-number]: ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number
[twos-complement]: https://en.wikipedia.org/wiki/Two%27s_complement
[control-flow]: ch03-05-control-flow.html#control-flow
[strings]: ch08-02-strings.html#storing-utf-8-encoded-text-with-strings
[stack-and-heap]: ch04-01-what-is-ownership.html#the-stack-and-the-heap
[vectors]: ch08-01-vectors.html
[unrecoverable-errors-with-panic]: ch09-01-unrecoverable-errors-with-panic.html
[appendix_b]: appendix-02-operators.md
