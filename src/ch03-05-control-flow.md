## Control Flow

ความสามารถในการรันโค้ดบางส่วนขึ้นอยู่กับว่าเงื่อนไขเป็น `true` หรือไม่ และการรันโค้ดบางส่วนซ้ำๆ ในขณะที่เงื่อนไขเป็น`true` เป็นพื้นฐานสำคัญในภาษา Programming ส่วนใหญ่ โครงสร้างที่พบบ่อยที่สุดที่ช่วยให้คุณควบคุม Flow การ Execute ของโค้ด Rust คือ `If` Expressions และ Loops ครับ

### `if` Expressions


`if` Expression ช่วยให้คุณสามารถแตกแขนงโค้ดของคุณได้ตามเงื่อนไข คุณระบุเงื่อนไข จากนั้นกล่าวว่า "ถ้าเงื่อนไขนี้เป็นจริง ให้รัน Block ของโค้ดนี้ ถ้าเงื่อนไขไม่เป็นจริง อย่ารัน Block ของโค้ดนี้"

สร้าง Project ใหม่ชื่อ _branches_ ใน Directory _projects_ ของคุณเพื่อสำรวจ `if` Expression ในไฟล์ _src/main.rs_ ให้ป้อนโค้ดต่อไปนี้:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-26-if-true/src/main.rs}}
```

If Expressions ทั้งหมดเริ่มต้นด้วย Keyword `if` ตามด้วยเงื่อนไข ในกรณีนี้ เงื่อนไขตรวจสอบว่าตัวแปร `number` มีค่าน้อยกว่า 5 หรือไม่ เราวาง Block ของโค้ดที่จะ Execute หากเงื่อนไขเป็น `true` ทันทีหลังเงื่อนไขภายในเครื่องหมายปีกกา Block ของโค้ดที่เกี่ยวข้องกับเงื่อนไขใน If Expressions บางครั้งเรียกว่า Arms เช่นเดียวกับ Arms ใน `Match Expressions` ที่เรากล่าวถึงในส่วน [“Comparing
the Guess to the Secret Number”][comparing-the-guess-to-the-secret-number]<!--
ignore --> ของ บทที่ 2

นอกจากนี้ เรายังสามารถใส่ `Else` Expression ซึ่งเราเลือกที่จะทำในที่นี้ เพื่อให้โปรแกรมมี Block ของโค้ดทางเลือกที่จะ Execute หากเงื่อนไขประเมินค่าเป็น `false` หากคุณไม่ใส่ `Else Expression` และเงื่อนไขเป็น `false` โปรแกรมก็จะข้าม If Block ไปและดำเนินการต่อกับโค้ดส่วนถัดไป

ลองรันโค้ดนี้ดูครับ คุณควรจะเห็น Output ดังนี้:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-26-if-true/output.txt}}
```

มาลองเปลี่ยนค่าของตัวแปร `number` เป็นค่าที่ทำให้เงื่อนไขเป็น `false` เพื่อดูว่าจะเกิดอะไรขึ้นครับ:

```rust,ignore
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-27-if-false/src/main.rs:here}}
```

รันโปรแกรม และดูสิ่งที่เกิดขึ้น

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-27-if-false/output.txt}}
```

นอกจากนี้ สิ่งสำคัญที่ควรทราบคือเงื่อนไขในโค้ดนี้ ต้อง เป็น Type `bool` หากเงื่อนไขไม่ใช่ `bool` เราจะได้รับ Error ตัวอย่างเช่น ลองรันโค้ดต่อไปนี้:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-28-if-condition-must-be-bool/src/main.rs}}
```

เงื่อนไข `if` ประเมินค่าเป็น `3` ในครั้งนี้ และ Rust จะแสดง Error:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-28-if-condition-must-be-bool/output.txt}}
```

Error ชี้ให้เห็นว่า Rust คาดหวัง Type `bool` แต่ได้รับ Integer ซึ่งแตกต่างจากภาษาอย่าง Ruby และ JavaScript ที่ Rust จะไม่พยายามแปลง Type ที่ไม่ใช่ Boolean เป็น Boolean โดยอัตโนมัติ คุณต้องระบุอย่างชัดเจนและให้ if มี Boolean เป็นเงื่อนไขเสมอ หากเราต้องการให้ Block ของโค้ด `if` ทำงานเฉพาะเมื่อตัวเลขไม่เท่ากับ `0` ตัวอย่างเช่น เราสามารถเปลี่ยน `if` Expression เป็นดังนี้:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-29-if-not-equal-0/src/main.rs}}
```

การรันโค้ดนี้จะพิมพ์ `number was something other than zero`

#### การจัดการหลายเงื่อนไขด้วย  `else if`

คุณสามารถใช้หลายเงื่อนไขได้โดยการรวม `if` และ `else` เข้าด้วยกันใน `else if` ตัวอย่างเช่น:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-30-else-if/src/main.rs}}
```

โปรแกรมนี้มีสี่เส้นทางที่เป็นไปได้ หลังจากรันแล้ว คุณควรจะเห็น Output ดังนี้:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-30-else-if/output.txt}}
```

เมื่อโปรแกรมนี้ Execute มันจะตรวจสอบแต่ละ `if` ตามลำดับ และ Execute Body แรกที่เงื่อนไขประเมินค่าเป็น `true` โปรดสังเกตว่าแม้ว่า `6 จะหารด้วย 2 ลงตัว` เราก็ไม่เห็น Output number is divisible by 2 และเราก็ไม่เห็นข้อความ `number is not divisible by 4, 3, or 2` จาก `else` Block นั่นเป็นเพราะ Rust จะ Execute Block สำหรับเงื่อนไข `true` แรกเท่านั้น และเมื่อพบแล้ว มันจะไม่ตรวจสอบส่วนที่เหลืออีก

การใช้ `else if` มากเกินไปอาจทำให้โค้ดของคุณรก ดังนั้นหากคุณมีมากกว่าหนึ่ง คุณอาจต้องการ Refactor โค้ดของคุณ บทที่ 6 อธิบายโครงสร้างการแตกแขนงที่ทรงพลังของ Rust ที่เรียกว่า `match` สำหรับกรณีเหล่านี้

#### การใช้ `if` ใน `let` Statement

เนื่องจาก `if` เป็น Expression เราจึงสามารถใช้มันทางด้านขวาของ `let` Statement เพื่อกำหนดผลลัพธ์ให้กับตัวแปรได้ ดังใน Listing 3-2:

<Listing number="3-2" file-name="src/main.rs" caption="Assigning the result of an `if` expression to a variable">

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-02/src/main.rs}}
```

</Listing>


ตัวแปร `number` จะถูกผูกไว้กับค่าที่ขึ้นอยู่กับผลลัพธ์ของ `if` ลองรันโค้ดนี้เพื่อดูว่าจะเกิดอะไรขึ้นครับ:

```console
{{#include ../listings/ch03-common-programming-concepts/listing-03-02/output.txt}}
```

โปรดจำไว้ว่า Block ของ Code จะประเมินค่าเป็น Expression สุดท้ายใน Block นั้น และตัวเลขเองก็เป็น Expression เช่นกัน ในกรณีนี้ ค่าของ `if` Expression ทั้งหมดจะขึ้นอยู่กับ Block ของ Code ใดที่ถูก Execute ซึ่งหมายความว่าค่าที่มีศักยภาพที่จะเป็นผลลัพธ์จากแต่ละ Arm ของ `if` จะต้องมี Type เดียวกัน ใน Listing 3-2 ผลลัพธ์ของทั้ง `if` Arm และ `else` Arm เป็น Integer Type `i32` หาก Type ไม่ตรงกัน ดังในตัวอย่างต่อไปนี้ เราจะได้รับ Error:


<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-31-arms-must-return-same-type/src/main.rs}}
```

เมื่อเราพยายาม Compile โค้ดนี้ เราจะได้รับ Error ซึ่ง `if` Arm และ `else` Arm มี Type ของค่าที่ไม่เข้ากัน และ Rust จะระบุตำแหน่งที่แน่นอนของปัญหาในโปรแกรม:


```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-31-arms-must-return-same-type/output.txt}}
```

Expression ใน Block `if` ประเมินค่าเป็น Integer และ Expression ใน Block `else` ประเมินค่าเป็น String ซึ่งจะไม่สามารถทำงานได้ เนื่องจากตัวแปรต้องมี Type เดียว และ Rust จำเป็นต้องทราบ Type ที่แน่นอนของตัวแปร number ณ Compile Time การทราบ Type ของ number ช่วยให้ Compiler สามารถตรวจสอบได้ว่า Type นั้นถูกต้องในทุกๆ ที่ที่เราใช้ `number` Rust จะไม่สามารถทำเช่นนั้นได้หาก Type ของ `number` ถูกกำหนดใน Runtime เท่านั้น Compiler จะมีความซับซ้อนมากขึ้น และจะให้การรับประกันเกี่ยวกับ Code น้อยลง หากต้องติดตาม Type ที่เป็นไปได้หลายแบบสำหรับตัวแปรใดๆ

### การทำซ้ำด้วย Loops

บ่อยครั้งที่เป็นประโยชน์ที่จะ Execute Block ของ Code มากกว่าหนึ่งครั้ง สำหรับงานนี้ Rust มี _Loops_ หลายประเภท ซึ่งจะรัน Code ภายใน Loop Body ไปจนจบ แล้วเริ่มใหม่ทันทีที่จุดเริ่มต้น เพื่อทดลองกับ Loops มาสร้าง Project ใหม่ชื่อ _loops_ กันครับ

Rust มี Loops สามประเภท: `loop`, `while` และ `for` มาลองแต่ละแบบกันครับ

#### การทำซ้ำ Code ด้วย `loop`

Keyword `loop` บอกให้ Rust Execute Block ของ Code ซ้ำแล้วซ้ำเล่าไปเรื่อยๆ หรือจนกว่าคุณจะสั่งให้หยุดอย่างชัดเจน

ตัวอย่างเช่น เปลี่ยนไฟล์ _src/main.rs_ ใน Directory _loops_ ของคุณให้มีลักษณะดังนี้:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-32-loop/src/main.rs}}
```

เมื่อเรารันโปรแกรมนี้ เราจะเห็นคำว่า `again!` พิมพ์ซ้ำแล้วซ้ำเล่าอย่างต่อเนื่องจนกว่าเราจะหยุดโปรแกรมด้วยตัวเอง Terminal ส่วนใหญ่รองรับ Keyboard Shortcut <kbd>ctrl</kbd>-<kbd>c</kbd> เพื่อ Interrupt โปรแกรมที่ติดอยู่ใน Loop ที่ไม่มีวันสิ้นสุด ลองทำดูครับ:

<!-- manual-regeneration
cd listings/ch03-common-programming-concepts/no-listing-32-loop
cargo run
CTRL-C
-->

```console
$ cargo run
   Compiling loops v0.1.0 (file:///projects/loops)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.08s
     Running `target/debug/loops`
again!
again!
again!
again!
^Cagain!
```

สัญลักษณ์ `^C` แสดงถึงตำแหน่งที่คุณกด <kbd>ctrl</kbd>-<kbd>c</kbd> คุณอาจเห็นหรือไม่เห็นคำว่า `again!` พิมพ์ออกมาหลังจาก `^C` ก็ได้ ขึ้นอยู่กับว่า Code อยู่ส่วนใดของ Loop เมื่อได้รับสัญญาณ Interrupt

โชคดีที่ Rust ยังมีวิธีออกจาก Loop โดยใช้ Code คุณสามารถใส่ Keyword `break` ภายใน Loop เพื่อบอกให้โปรแกรมหยุดการ Execute Loop เมื่อใดก็ได้ จำได้ไหมว่าเราทำเช่นนี้ในเกมทายตัวเลขในส่วน [“Quitting After a Correct Guess”][quitting-after-a-correct-guess]<!-- ignore
--> ของ บทที่ 2 เพื่อออกจากโปรแกรมเมื่อผู้ใช้ชนะเกมโดยการทายตัวเลขที่ถูกต้อง

เรายังใช้ `continue` ในเกมทายตัวเลขด้วย ซึ่งใน Loop จะบอกให้โปรแกรมข้าม Code ที่เหลืออยู่ในการวนรอบปัจจุบัน และไปเริ่มการวนรอบถัดไป

#### การ Return ค่าจาก Loops

ประโยชน์อย่างหนึ่งของ `loop` คือการลองทำ Operation ที่คุณรู้ว่าอาจล้มเหลวซ้ำ เช่น การตรวจสอบว่า Thread ทำงานเสร็จสิ้นแล้วหรือไม่ คุณอาจจำเป็นต้องส่งผลลัพธ์ของ Operation นั้นออกจาก Loop ไปยังส่วนอื่นๆ ของ Code ของคุณ เพื่อทำสิ่งนี้ คุณสามารถเพิ่มค่าที่คุณต้องการ Return หลังจาก Expression `break` ที่คุณใช้เพื่อหยุด Loop ค่านั้นจะถูก Return ออกมาจาก Loop เพื่อให้คุณสามารถใช้งานได้ ดังที่แสดงไว้ในตัวอย่างนี้:

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-33-return-value-from-loop/src/main.rs}}
```


ก่อน Loop เราประกาศตัวแปรชื่อ `counter` และ Initialize ให้มีค่าเป็น `0` จากนั้นเราประกาศตัวแปรชื่อ `result` เพื่อเก็บค่าที่ Return ออกมาจาก Loop ในทุกๆ รอบของการวน Loop เราจะเพิ่ม 1 ให้กับตัวแปร `counter` แล้วตรวจสอบว่า `counter` มีค่าเท่ากับ 10 หรือไม่ เมื่อเป็นเช่นนั้น เราจะใช้ Keyword break พร้อมกับค่า `counter * 2` หลังจาก Loop เราใช้เครื่องหมายเซมิโคลอนเพื่อจบ Statement ที่กำหนดค่าให้กับ `result` สุดท้าย เราพิมพ์ค่าใน result ซึ่งในกรณีนี้คือ `20`

คุณยังสามารถ `return` จากภายใน Loop ได้ด้วย ในขณะที่ `break` จะออกจาก Loop ปัจจุบันเท่านั้น แต่ `return` จะออกจากฟังก์ชันปัจจุบันเสมอ

#### Loop Labels เพื่อแยกความแตกต่างระหว่าง Loops หลายชั้น

หากคุณมี Loops ซ้อนกัน Keyword `break` และ `continue` จะมีผลกับ Loop ที่อยู่ชั้นในสุด ณ จุดนั้น คุณสามารถเลือกที่จะระบุ _Loop Label_ บน Loop ได้ จากนั้นคุณสามารถใช้ Label นี้กับ `break` หรือ `continue` เพื่อระบุว่า Keyword เหล่านั้นมีผลกับ Loop ที่มี Label แทนที่จะเป็น Loop ที่อยู่ชั้นในสุด Loop Labels ต้องเริ่มต้นด้วยเครื่องหมาย Single Quote นี่คือตัวอย่างที่มี Loops ซ้อนกันสองชั้น:

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-32-5-loop-labels/src/main.rs}}
```

Loop นอกมี Label `'counting_up'` และจะนับขึ้นจาก 0 ถึง 2 Loop ในที่ไม่มี Label จะนับลงจาก 10 ถึง 9 คำสั่ง `break` แรกที่ไม่ได้ระบุ Label จะออกจาก Loop ในสุดเท่านั้น ส่วนคำสั่ง break 'counting_up; จะออกจาก Loop นอก โค้ดนี้จะพิมพ์:

```console
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-32-5-loop-labels/output.txt}}
```

#### Conditional Loops ด้วย `while`


บ่อยครั้งที่โปรแกรมจำเป็นต้องประเมินเงื่อนไขภายใน Loop ในขณะที่เงื่อนไขเป็น `true` Loop จะทำงาน เมื่อเงื่อนไขไม่เป็น `true` โปรแกรมจะเรียก `break` เพื่อหยุด Loop คุณสามารถ Implement ลักษณะการทำงานแบบนี้ได้โดยใช้การรวมกันของ `loop`, `if`, `else` และ `break` คุณสามารถลองทำดูในโปรแกรมตอนนี้ก็ได้ หากต้องการ อย่างไรก็ตาม Pattern นี้เป็นเรื่องปกติมาก จน Rust มี Construct ภาษา Built-in สำหรับมัน เรียกว่า `While` Loop ใน Listing 3-3 เราใช้ `while` เพื่อวน Loop โปรแกรมสามครั้ง โดยนับถอยหลังในแต่ละครั้ง และหลังจาก Loop แล้ว จะพิมพ์ข้อความและ Exit

<Listing number="3-3" file-name="src/main.rs" caption="Using a `while` loop to run code while a condition evaluates to `true`">

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-03/src/main.rs}}
```

</Listing>

โครงสร้างนี้ช่วยลดการซ้อนกันของ Code ที่จำเป็นหากคุณใช้ `loop`, `if`, `else` และ `break` และยังมีความชัดเจนกว่า ในขณะที่เงื่อนไขประเมินค่าเป็น `true` Code จะทำงาน มิฉะนั้นจะออกจาก Loop



#### การวน Loop ผ่าน Collection ด้วย `for`

คุณยังสามารถใช้ Construct `while` เพื่อวน Loop ผ่าน Elements ของ Collection ได้ เช่น Array ตัวอย่างเช่น Loop ใน Listing 3-4 จะพิมพ์แต่ละ Element ใน Array `a`

<Listing number="3-4" file-name="src/main.rs" caption="Looping through each element of a collection using a `while` loop">

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-04/src/main.rs}}
```

</Listing>

ใน Code นี้จะมีการนับวนไปตาม Elements ใน Array โดยเริ่มต้นที่ Index `0` แล้ววน Loop ไปเรื่อยๆ จนกระทั่งถึง Index สุดท้ายใน Array (นั่นคือเมื่อ `index < 5` ไม่เป็น `true` อีกต่อไป) การรัน Code นี้จะพิมพ์ทุก Element ใน Array:

```console
{{#include ../listings/ch03-common-programming-concepts/listing-03-04/output.txt}}
```
ค่าทั้งห้าของ Array ปรากฏใน Terminal ตามที่คาดไว้ แม้ว่า `index` จะมีค่าถึง `5` ในบางจุด แต่ Loop จะหยุดการทำงานก่อนที่จะพยายามดึงค่าที่หกจาก Array

อย่างไรก็ตาม วิธีนี้มีโอกาสเกิด Error ได้ง่าย เราอาจทำให้โปรแกรม Panic ได้หากค่า Index หรือเงื่อนไขการทดสอบไม่ถูกต้อง ตัวอย่างเช่น หากคุณเปลี่ยน Definition ของ Array `a` ให้มีสี่ Elements แต่ลืมอัปเดตเงื่อนไขเป็น `while index < 4` โค้ดจะ Panic นอกจากนี้ยังช้าด้วย เนื่องจาก Compiler จะเพิ่ม Runtime Code เพื่อทำการตรวจสอบเงื่อนไขว่า Index อยู่ภายในขอบเขตของ Array หรือไม่ ในทุกๆ รอบของการวน Loop

เพื่อเป็นทางเลือกที่กระชับกว่า คุณสามารถใช้ `for` Loop และ Execute Code บางอย่างสำหรับแต่ละ Item ใน Collection `for` Loop มีลักษณะเหมือน Code ใน Listing 3-5:

<Listing number="3-5" file-name="src/main.rs" caption="Looping through each element of a collection using a `for` loop">

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-05/src/main.rs}}
```

</Listing>

เมื่อเรารันโค้ดนี้ เราจะเห็น Output เดียวกันกับใน Listing 3-4 ที่สำคัญกว่านั้นคือ ตอนนี้เราได้เพิ่มความปลอดภัยของโค้ดและขจัดโอกาสเกิด Bugs ที่อาจเกิดขึ้นจากการเข้าถึงเกินขอบเขตของ Array หรือเข้าถึงไม่เพียงพอและพลาดบาง Items ไป

การใช้ `for` Loop ทำให้คุณไม่จำเป็นต้องจำที่จะต้องแก้ไข Code ส่วนอื่น หากคุณเปลี่ยนจำนวนค่าใน Array เหมือนกับวิธีที่ใช้ใน Listing 3-4

ความปลอดภัยและความกระชับของ `for` Loops ทำให้มันเป็น Construct Loop ที่ใช้บ่อยที่สุดใน Rust แม้ในสถานการณ์ที่คุณต้องการรัน Code จำนวนครั้งที่แน่นอน เช่น ตัวอย่างการนับถอยหลังที่ใช้ `while` Loop ใน Listing 3-3 Rustaceans ส่วนใหญ่จะใช้ `for` Loop วิธีการทำเช่นนั้นคือการใช้ `Range` ที่ Standard Library จัดเตรียมไว้ให้ ซึ่งจะสร้างตัวเลขทั้งหมดตามลำดับ โดยเริ่มต้นจากตัวเลขหนึ่งและสิ้นสุดก่อนอีกตัวเลขหนึ่ง

นี่คือลักษณะของการนับถอยหลังโดยใช้ `for` Loop และอีก Method ที่เรายังไม่ได้พูดถึงคือ `rev` ซึ่งใช้เพื่อกลับลำดับของ Range:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-34-for-range/src/main.rs}}
```

โค้ดนี้ดูดีกว่าหน่อยใช่ไหมครับ?

## สรุป

คุณทำสำเร็จแล้ว! นี่เป็นบทที่ค่อนข้างยาว: คุณได้เรียนรู้เกี่ยวกับตัวแปร, Data Types แบบ Scalar และ Compound, ฟังก์ชัน, Comments, `If` Expressions และ Loops! เพื่อฝึกฝนแนวคิดที่กล่าวถึงในบทนี้ ลองสร้างโปรแกรมเพื่อทำสิ่งต่อไปนี้:

- แปลงอุณหภูมิระหว่างฟาเรนไฮต์และเซลเซียส
- สร้างเลข Fibonacci ลำดับที่ n
- พิมพ์เนื้อเพลงของเพลงคริสต์มาส "The Twelve Days of Christmas" โดยใช้ประโยชน์จากการทำซ้ำในเพลง

เมื่อคุณพร้อมที่จะก้าวไปข้างหน้า เราจะพูดถึงแนวคิดใน Rust ที่ ไม่มี อยู่ทั่วไปในภาษา Programming อื่นๆ นั่นคือ Ownership ครับ

[comparing-the-guess-to-the-secret-number]: ch02-00-guessing-game-tutorial.html#comparing-the-guess-to-the-secret-number
[quitting-after-a-correct-guess]: ch02-00-guessing-game-tutorial.html#quitting-after-a-correct-guess
