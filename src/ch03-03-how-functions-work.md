## Functions

ฟังก์ชันเป็นสิ่งที่พบได้ทั่วไปในโค้ด Rust คุณได้เห็นฟังก์ชันที่สำคัญที่สุดฟังก์ชันหนึ่งในภาษาแล้ว นั่นคือฟังก์ชัน `main` ซึ่งเป็นจุดเริ่มต้นของโปรแกรมจำนวนมาก คุณยังได้เห็น Keyword `fn` ซึ่งช่วยให้คุณประกาศฟังก์ชันใหม่ได้

โค้ด Rust ใช้ _Snake Case_ เป็นรูปแบบการตั้งชื่อที่เป็นธรรมเนียมสำหรับชื่อฟังก์ชันและตัวแปร โดยตัวอักษรทั้งหมดเป็นตัวพิมพ์เล็ก และใช้ Underscore คั่นระหว่างคำ นี่คือโปรแกรมที่มีตัวอย่างการกำหนดฟังก์ชัน:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-16-functions/src/main.rs}}
```

เรากำหนดฟังก์ชันใน Rust โดยการพิมพ์ `fn` ตามด้วยชื่อฟังก์ชันและชุดของวงเล็บ เครื่องหมายปีกกาบอก Compiler ว่าส่วนใดคือจุดเริ่มต้นและจุดสิ้นสุดของ Body ฟังก์ชัน

เราสามารถเรียกใช้ฟังก์ชันใดๆ ที่เรากำหนดไว้ได้โดยการพิมพ์ชื่อของมันตามด้วยชุดของวงเล็บ เนื่องจาก `another_function` ถูกกำหนดไว้ในโปรแกรม จึงสามารถเรียกใช้ได้จากภายในฟังก์ชัน `main` โปรดทราบว่าเรากำหนด `another_function` ไว้หลังฟังก์ชัน `main` ใน Source Code เราสามารถกำหนดไว้ก่อนก็ได้ Rust ไม่สนใจว่าคุณจะกำหนดฟังก์ชันของคุณไว้ที่ใด เพียงแค่ให้แน่ใจว่ามันถูกกำหนดไว้ใน Scope ที่ Caller สามารถมองเห็นได้

มาเริ่มต้น Binary Project ใหม่ชื่อ _functions_ เพื่อสำรวจฟังก์ชันเพิ่มเติมกันครับ ใส่ตัวอย่าง `another_function` ใน _src/main.rs_ แล้วรัน คุณควรเห็น Output ดังนี้:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-16-functions/output.txt}}
```
บรรทัดต่างๆ จะถูก Execute ตามลำดับที่ปรากฏในฟังก์ชัน `main` ก่อนอื่น ข้อความ "Hello, world!" จะถูกพิมพ์ออกมา จากนั้น `another_function` จะถูกเรียกใช้และข้อความของมันก็จะถูกพิมพ์ออกมาครับ

### Parameters

เราสามารถกำหนดให้ฟังก์ชันมี _Parameters_ ซึ่งเป็นตัวแปรพิเศษที่เป็นส่วนหนึ่งของ Signature ของฟังก์ชัน เมื่อฟังก์ชันมี Parameters คุณสามารถให้ค่าที่เป็นรูปธรรมสำหรับ Parameters เหล่านั้นได้ ในทางเทคนิคแล้ว ค่าที่เป็นรูปธรรมเหล่านั้นเรียกว่า _Arguments_ แต่ในการสนทนาทั่วไป ผู้คนมักใช้คำว่า _Parameter_ และ _Argument_ สลับกัน ไม่ว่าจะเป็นตัวแปรใน Definition ของฟังก์ชัน หรือค่าที่เป็นรูปธรรมที่ส่งเข้าไปเมื่อคุณเรียกใช้ฟังก์ชัน



ใน another_function Version นี้ เราได้เพิ่ม Parameter เข้าไป:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-17-functions-with-parameters/src/main.rs}}
```

เมื่อคุณรันโปรแกรมนี้ Output จะเป็นดังนี้:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-17-functions-with-parameters/output.txt}}
```


การประกาศ `another_function` มี Parameter หนึ่งชื่อ `x` Type ของ `x` ถูกระบุเป็น `i32` เมื่อเราส่งค่า `5` เข้าไปใน `another_function` Macro `println!` จะใส่ `5` แทนที่วงเล็บปีกกาคู่ที่บรรจุ x ไว้ใน Format String

ใน Function Signatures คุณ _ต้อง_ ประกาศ Type ของ Parameter แต่ละตัว นี่เป็นการตัดสินใจโดยเจตนาในการออกแบบของ Rust การกำหนดให้มี Type Annotations ใน Function Definitions หมายความว่า Compiler แทบจะไม่ต้องการให้คุณใช้ Type Annotations ที่อื่นในโค้ดเพื่อระบุ Type ที่คุณต้องการ Compiler ยังสามารถให้ Error Messages ที่เป็นประโยชน์มากขึ้นได้ หากทราบว่าฟังก์ชันนั้นคาดหวัง Type ใด

เมื่อกำหนด Parameters หลายตัว ให้คั่น Declaration ของ Parameter ด้วยเครื่องหมายจุลภาค ดังนี้:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-18-functions-with-multiple-parameters/src/main.rs}}
```
ตัวอย่างนี้สร้างฟังก์ชันชื่อ `print_labeled_measurement` ที่มีสอง Parameters Parameter แรกชื่อ `value` และเป็น Type `i32` Parameter ที่สองชื่อ `unit_label` และเป็น Type `char` จากนั้นฟังก์ชันจะพิมพ์ข้อความที่มีทั้ง `value` และ `unit_label`

ลองรันโค้ดนี้กันครับ แทนที่โปรแกรมปัจจุบันในไฟล์ _src/main.rs_ ของ Project _functions_ ด้วยตัวอย่างก่อนหน้า แล้วรันโดยใช้ cargo run:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-18-functions-with-multiple-parameters/output.txt}}
```
เนื่องจากเราเรียกใช้ฟังก์ชันด้วย `5` เป็นค่าสำหรับ `value` และ `'h'` เป็นค่าสำหรับ `unit_label` Output ของโปรแกรมจึงมีค่าเหล่านั้นครับ

### Statements and Expressions

Body ของฟังก์ชันประกอบด้วยชุดของ Statements ที่อาจจบลงด้วย Expression ฟังก์ชันที่เรากล่าวถึงไปก่อนหน้านี้ยังไม่มี Ending Expression แต่คุณได้เห็น Expression เป็นส่วนหนึ่งของ Statement แล้ว เนื่องจาก Rust เป็นภาษาที่ใช้ Expression เป็นหลัก การทำความเข้าใจความแตกต่างนี้จึงมีความสำคัญ ภาษาอื่นๆ ไม่มีความแตกต่างเช่นนี้ ดังนั้นเรามาดูกันว่า Statements และ Expressions คืออะไร และความแตกต่างของมันส่งผลต่อ Body ของฟังก์ชันอย่างไร

- **Statements** คือคำสั่งที่ดำเนินการบางอย่างและไม่ Return ค่า
- **Expressions** ประเมินค่าและให้ผลลัพธ์เป็นค่าหนึ่งค่า มาดูตัวอย่างกันครับ

จริงๆ แล้วเราได้ใช้ Statements และ Expressions ไปแล้ว การสร้างตัวแปรและกำหนดค่าให้มันด้วย Keyword `let` คือ Statement ใน Listing 3-1, `let y = 6;` คือ Statement ครับ

<Listing number="3-1" file-name="src/main.rs" caption="A `main` function declaration containing one statement">

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-01/src/main.rs}}
```

</Listing>

การกำหนดฟังก์ชันก็เป็น Statement เช่นกัน ตัวอย่างทั้งหมดที่เราเพิ่งดูกันไปนั้นเป็น Statement ในตัวมันเอง (ดังที่เราจะเห็นต่อไป การ เรียกใช้ ฟังก์ชันไม่ใช่ Statement)

Statements ไม่ Return ค่า ดังนั้นคุณจึงไม่สามารถกำหนด `let` Statement ให้กับตัวแปรอื่นได้ ดังที่โค้ดต่อไปนี้พยายามทำ ซึ่งจะทำให้เกิด Error:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-19-statements-vs-expressions/src/main.rs}}
```

เมื่อคุณรันโปรแกรมนี้ Error ที่คุณจะได้รับจะมีลักษณะดังนี้:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-19-statements-vs-expressions/output.txt}}
```

Statement `let y = 6` ไม่ Return ค่า ดังนั้นจึงไม่มีอะไรให้ `x` ผูกค่าด้วย นี่แตกต่างจากสิ่งที่เกิดขึ้นในภาษาอื่นๆ เช่น C และ Ruby ซึ่งการกำหนดค่าจะ Return ค่าของการกำหนด ในภาษาเหล่านั้น คุณสามารถเขียน `x =y = 6 `และทำให้ทั้ง `x` และ `y` มีค่าเป็น `6` ได้ ซึ่งไม่ใช่กรณีใน Rust

Expressions ประเมินค่าและให้ผลลัพธ์เป็นค่าหนึ่งค่า และเป็นส่วนประกอบหลักของโค้ดส่วนใหญ่ที่คุณจะเขียนใน Rust พิจารณาการดำเนินการทางคณิตศาสตร์ เช่น `5 + 6` ซึ่งเป็น Expression ที่ประเมินค่าได้เป็น `11` Expressions สามารถเป็นส่วนหนึ่งของ Statements ได้ ใน Listing 3-1 ค่า 6 ใน Statement `let y = 6;` คือ Expression ที่ประเมินค่าได้เป็น 6 การเรียกใช้ฟังก์ชันคือ Expression การเรียกใช้ Macro คือ Expression Scope Block ใหม่ที่สร้างด้วยเครื่องหมายปีกกาคือ Expression ตัวอย่างเช่น:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-20-blocks-are-expressions/src/main.rs}}
```

This expression:

```rust,ignore
{
    let x = 3;
    x + 1
}
```


คือ Block ที่ในกรณีนี้ประเมินค่าได้เป็น `4` ค่านี้จะถูกผูกไว้กับ `y` ซึ่งเป็นส่วนหนึ่งของ Statement `let` โปรดสังเกตว่าบรรทัด `x + 1` ไม่มีเครื่องหมายเซมิโคลอนที่ส่วนท้าย ซึ่งแตกต่างจากบรรทัดส่วนใหญ่ที่คุณเห็นมาจนถึงตอนนี้ Expressions จะไม่มีเครื่องหมายเซมิโคลอนที่ส่วนท้าย หากคุณเพิ่มเครื่องหมายเซมิโคลอนที่ส่วนท้ายของ Expression คุณจะเปลี่ยนให้มันกลายเป็น Statement และมันจะไม่ Return ค่า โปรดจำสิ่งนี้ไว้ในขณะที่คุณสำรวจค่า Return ของฟังก์ชันและ Expressions ในหัวข้อถัดไป

### Functions with Return Values


ฟังก์ชันสามารถ Return ค่าให้กับ Code ที่เรียกใช้ได้ เราไม่ได้ตั้งชื่อให้กับค่า Return แต่เราต้องประกาศ Type ของมันหลังจากเครื่องหมายลูกศร (`->`) ใน Rust ค่า Return ของฟังก์ชันจะมีความหมายเหมือนกันกับค่าของ Expression สุดท้ายใน Block ของ Body ฟังก์ชัน คุณสามารถ Return ค่าออกจากฟังก์ชันก่อนหน้านี้ได้โดยใช้ Keyword `return` และระบุค่า แต่ฟังก์ชันส่วนใหญ่จะ Return Expression สุดท้ายโดยปริยาย นี่คือตัวอย่างของฟังก์ชันที่ Return ค่า:


<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-21-function-return-values/src/main.rs}}
```

ในฟังก์ชัน five ไม่มี Function Calls, Macros หรือแม้แต่ `let` Statements มีเพียงตัวเลข `5` โดดๆ เท่านั้น นั่นเป็นฟังก์ชันที่ถูกต้องสมบูรณ์แบบใน Rust โปรดสังเกตว่า Type ของค่า Return ของฟังก์ชันก็ถูกระบุด้วยเช่นกัน คือ `-> i32` ลองรันโค้ดนี้ดูครับ Output ควรจะเป็นดังนี้:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-21-function-return-values/output.txt}}
```

ค่า `5` ในฟังก์ชัน `five` คือค่า Return ของฟังก์ชัน ซึ่งเป็นเหตุผลว่าทำไม Return Type จึงเป็น `i32` มาพิจารณาสิ่งนี้ในรายละเอียดกันครับ มีสองส่วนที่สำคัญ: ประการแรก บรรทัด `let x = five();` แสดงให้เห็นว่าเรากำลังใช้ค่า Return ของฟังก์ชันเพื่อ Initialize ตัวแปร เนื่องจากฟังก์ชัน `five` Return ค่า 5 บรรทัดนั้นจึงเหมือนกับบรรทัดต่อไปนี้:

```rust
let x = 5;
```
ประการที่สอง ฟังก์ชัน `five` ไม่มี Parameters และกำหนด Type ของค่า Return แต่ Body ของฟังก์ชันมีเพียง `5` โดดๆ โดยไม่มีเครื่องหมายเซมิโคลอน เพราะมันเป็น Expression ที่เราต้องการ Return ค่า

มาดูอีกตัวอย่างกันครับ:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-22-function-parameter-and-return/src/main.rs}}
```

การรันโค้ดนี้จะพิมพ์ `The value of x is: 6` แต่ถ้าเราใส่เครื่องหมายเซมิโคลอนที่ท้ายบรรทัดที่มี `x + 1` เปลี่ยนจาก Expression เป็น Statement เราจะได้รับ Error:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-23-statements-dont-return-values/src/main.rs}}
```

Compiling this code produces an error, as follows:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-23-statements-dont-return-values/output.txt}}
```

ข้อความ Error หลักคือ `mismatched types` (ประเภทไม่ตรงกัน) เผยให้เห็นปัญหาหลักของโค้ดนี้ การกำหนดของฟังก์ชัน `plus_one` ระบุว่าจะ Return ค่า `i32` แต่ Statements ไม่ได้ประเมินค่าเป็นค่าใดๆ ซึ่งแสดงด้วย `()` หรือ Unit Type ดังนั้นจึงไม่มีอะไรถูก Return ซึ่งขัดแย้งกับการกำหนดของฟังก์ชันและส่งผลให้เกิด Error ใน Output นี้ Rust ให้ข้อความที่อาจช่วยแก้ไขปัญหานี้ โดยแนะนำให้ลบเครื่องหมายเซมิโคลอน ซึ่งจะแก้ไข Error ได้ครับ
