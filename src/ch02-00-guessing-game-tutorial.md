# การเขียนโปรแกรมเกมทายตัวเลข

มาเริ่มต้นการเรียนรู้ Rust อย่างจริงจังด้วยการทำโปรเจกต์ไปพร้อมๆ กันเลยครับ! บทนี้จะแนะนำแนวคิดพื้นฐานของ Rust บางส่วน โดยแสดงให้เห็นวิธีการใช้งานในโปรแกรมจริง คุณจะได้เรียนรู้เกี่ยวกับ `let`, `match`, methods, associated functions, external crates และอื่นๆ อีกมากมาย! ในบทต่อๆ ไป เราจะสำรวจแนวคิดเหล่านี้ในรายละเอียดมากขึ้น แต่ในบทนี้ เราจะเน้นการฝึกฝนพื้นฐานครับ

เราจะสร้างโปรแกรมแก้ปัญหาคลาสสิกสำหรับผู้เริ่มต้นเขียนโปรแกรม นั่นคือเกมทายตัวเลข หลักการทำงานของมันคือ: โปรแกรมจะสร้างตัวเลขสุ่มจำนวนเต็มระหว่าง 1 ถึง 100 จากนั้นจะให้ผู้เล่นป้อนตัวเลขที่คาดเดา หลังจากป้อนตัวเลขแล้ว โปรแกรมจะบอกว่าตัวเลขที่ทายนั้นต่ำไปหรือสูงไป หากตัวเลขที่ทายถูกต้อง เกมจะแสดงข้อความแสดงความยินดีและจบการทำงาน

## การตั้งค่าโปรเจกต์ใหม่

ในการตั้งค่าโปรเจกต์ใหม่ ให้ไปยังไดเรกทอรี _projects_ ที่คุณสร้างไว้ในบทที่ 1 และสร้างโปรเจกต์ใหม่โดยใช้ Cargo ดังนี้:

```console
$ cargo new guessing_game
$ cd guessing_game
```

คำสั่งแรก `cargo new guessing_game` จะสร้างไดเรกทอรีใหม่ชื่อ `guessing_game` พร้อมกับไฟล์พื้นฐานที่จำเป็นสำหรับโปรเจกต์ Cargo จากนั้น cd guessing_game จะเปลี่ยนไปยังไดเรกทอรีนั้น

ดูที่ _Cargo.toml_ file:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial
rm -rf no-listing-01-cargo-new
cargo new no-listing-01-cargo-new --name guessing_game
cd no-listing-01-cargo-new
cargo run > output.txt 2>&1
cd ../../..
-->

<span class="filename">Filename: Cargo.toml</span>

```toml
{{#include ../listings/ch02-guessing-game-tutorial/no-listing-01-cargo-new/Cargo.toml}}
```

อย่างที่คุณเห็นในบทที่ 1 คำสั่ง `cargo new` จะสร้างโปรแกรม "Hello, world!" ให้คุณ ลองดูไฟล์ _src/main.rs_:


<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-01-cargo-new/src/main.rs}}
```

ตอนนี้เรามาคอมไพล์และรันโปรแกรม "Hello, world!" นี้ในขั้นตอนเดียวโดยใช้คำสั่ง `cargo run` กันครับ:

```console
{{#include ../listings/ch02-guessing-game-tutorial/no-listing-01-cargo-new/output.txt}}
```

คุณควรจะเห็นข้อความ "Hello, world!" ปรากฏใน Terminal ของคุณครับ ตอนนี้เราจะเริ่มแก้ไขไฟล์ src/main.rs เพื่อเขียนโค้ดเกมทายตัวเลขของเรากันแล้วครับ

## การประมวลผลคำทาย

ส่วนแรกของโปรแกรมเกมทายตัวเลขคือการขอ Input จากผู้ใช้ ประมวลผล Input นั้น และตรวจสอบว่า Input อยู่ในรูปแบบที่คาดไว้ เพื่อเริ่มต้น เราจะอนุญาตให้ผู้เล่นป้อนตัวเลขที่ทาย ใส่โค้ดใน Listing 2-1 ลงใน _src/main.rs_ ครับ

<Listing number="2-1" file-name="src/main.rs" caption="Code that gets a guess from the user and prints it">

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:all}}
```

</Listing>

โค้ดนี้มีข้อมูลมากมายเลยทีเดียว ดังนั้นเรามาดูกันทีละบรรทัดนะครับ ในการรับ Input จากผู้ใช้แล้วพิมพ์ผลลัพธ์ออกมา เราจำเป็นต้องนำ Input/Output Library (`io`) เข้ามาใน Scope Library `io` นี้มาจาก Standard Library ซึ่งรู้จักกันในชื่อ `std`:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:io}}
```

โดยค่าเริ่มต้น Rust มีชุดของ Items ที่ถูกกำหนดไว้ใน Standard Library ซึ่งจะถูกนำเข้ามาใน Scope ของทุกโปรแกรม ชุดนี้เรียกว่า prelude และคุณสามารถดูทุกสิ่งที่มีอยู่ในนั้นได้ใน [in the standard library documentation][prelude].

หาก Type ที่คุณต้องการใช้ไม่ได้อยู่ใน prelude คุณจะต้องนำ Type นั้นเข้ามาใน Scope อย่างชัดเจนด้วยคำสั่ง `use` การใช้ Library `std::io` ช่วยให้คุณมีคุณสมบัติที่มีประโยชน์มากมาย รวมถึงความสามารถในการรับ Input จากผู้ใช้

อย่างที่คุณเห็นในบทที่ 1 ฟังก์ชัน `main` เป็นจุดเริ่มต้นการทำงานของโปรแกรม:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:main}}
```

ไวยากรณ์ `fn` ประกาศฟังก์ชันใหม่ วงเล็บ `()` บ่งชี้ว่าไม่มีพารามิเตอร์ และวงเล็บปีกกา `{` เปิดส่วนเนื้อหาของฟังก์ชัน

และอย่างที่คุณได้เรียนรู้ในบทที่ 1 `println!` เป็น Macro ที่พิมพ์ String ออกทางหน้าจอ:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:print}}
```

โค้ดนี้กำลังพิมพ์ข้อความแจ้ง (prompt) ที่ระบุว่าเกมคืออะไร และขอให้ผู้ใช้ป้อนข้อมูล (input)

### การจัดเก็บค่าด้วยตัวแปร

ต่อไป เราจะสร้าง ตัวแปร เพื่อจัดเก็บ Input ที่ผู้ใช้ป้อนเข้ามา ดังนี้:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:string}}
```

ตอนนี้โปรแกรมเริ่มน่าสนใจแล้ว! มีหลายสิ่งเกิดขึ้นในบรรทัดเล็กๆ นี้ เราใช้คำสั่ง `let` เพื่อสร้างตัวแปร นี่คืออีกตัวอย่างหนึ่ง:

```rust,ignore
let apples = 5;
```

บรรทัดนี้สร้างตัวแปรใหม่ชื่อ `apples` และผูกมันกับค่า 5 ใน Rust ตัวแปรเป็น Immutable โดยค่าเริ่มต้น ซึ่งหมายความว่าเมื่อเรากำหนดค่าให้กับตัวแปรแล้ว ค่าจะไม่เปลี่ยนแปลง เราจะพูดถึงแนวคิดนี้ในรายละเอียดในส่วน  [“Variables and Mutability”][variables-and-mutability]<!-- ignore -->
ในบทที่ 3 หากต้องการทำให้ตัวแปรเป็น Mutable เราจะเพิ่ม `mut` ก่อนชื่อตัวแปร:

```rust,ignore
let apples = 5; // immutable
let mut bananas = 5; // mutable
```

> หมายเหตุไวยากรณ์ // เริ่มต้น Comment ซึ่งจะดำเนินต่อไปจนกระทั่งสิ้นสุดบรรทัด Rust จะละเว้นทุกสิ่งที่อยู่ใน Comment เราจะพูดถึง Comment ในรายละเอียดเพิ่มเติมใน[Chapter 3][comments]<!-- ignore -->.


กลับมาที่โปรแกรมเกมทายตัวเลข ตอนนี้คุณรู้แล้วว่า `let mut guess` จะประกาศตัวแปร Mutable ชื่อ `guess` เครื่องหมายเท่ากับ (`=`) บอก Rust ว่าเราต้องการผูกบางสิ่งเข้ากับตัวแปรนี้ ตอนด้านขวาของเครื่องหมายเท่ากับคือค่าที่ `guess` ถูกผูกไว้ ซึ่งเป็นผลลัพธ์ของการเรียก `String::new` ซึ่งเป็นฟังก์ชันที่คืนค่า Instance ใหม่ของ `String`.
[`String`][string]<!-- ignore --> String Type ที่ Standard Library จัดเตรียมไว้ให้ ซึ่งสามารถขยายขนาดได้และเป็นลำดับของข้อความที่เข้ารหัสแบบ UTF-8

ไวยากรณ์ `::` ในบรรทัด `::new` บ่งชี้ว่า `new` เป็น Associated Function ของ Type `String` _Associated Function_ คือฟังก์ชันที่ถูก Implement บน Type ในกรณีนี้คือ `String` ฟังก์ชัน `new` นี้จะสร้าง String ว่างเปล่า Instance ใหม่ คุณจะพบฟังก์ชัน `new` ในหลายๆ Type เนื่องจากเป็นชื่อทั่วไปสำหรับฟังก์ชันที่สร้างค่าใหม่ของ Type นั้นๆ

โดยรวมแล้ว บรรทัด `let mut guess = String::new();` ได้สร้างตัวแปร Mutable ที่ปัจจุบันถูกผูกไว้กับ Instance ใหม่ที่เป็น `String` ว่างเปล่า ว้าว!

### การรับ Input จากผู้ใช้

จำได้ไหมว่าเราได้รวมฟังก์ชันการทำงานด้าน Input/Output จาก Standard Library เข้ามาด้วย `use std::io;` ในบรรทัดแรกของโปรแกรม ตอนนี้เราจะเรียกใช้ฟังก์ชัน stdin จาก Module `io` ซึ่งจะช่วยให้เราจัดการกับ Input จากผู้ใช้ได้:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:read}}
```

หากเราไม่ได้ Import Library `io` ด้วย `use std::io;` ที่ส่วนเริ่มต้นของโปรแกรม เราก็ยังสามารถใช้ฟังก์ชันนี้ได้โดยเขียนการเรียกใช้ฟังก์ชันเป็น `std::io::stdin` ฟังก์ชัน `stdin` คืนค่า Instance ของ [`std::io::Stdin`][iostdin]<!-- ignore -->, ซึ่งเป็น Type ที่ Represent Handle ไปยัง Standard Input ของ Terminal ของคุณ

ต่อไป บรรทัด `.read_line(&mut guess)` เรียกใช้ Method [`read_line`][read_line]<!--
ignore --> บน Standard Input Handle เพื่อรับ Input จากผู้ใช้ เรายังส่ง `&mut guess` เป็น Argument ให้กับ `read_line` เพื่อบอกว่าควรเก็บ Input ของผู้ใช้ไว้ใน String ใด หน้าที่ทั้งหมดของ `read_line` คือการนำสิ่งที่ผู้ใช้พิมพ์ลงใน Standard Input มาต่อท้าย String (โดยไม่เขียนทับเนื้อหาเดิม) ดังนั้นเราจึงส่ง String นั้นเป็น Argument String ที่เป็น Argument จะต้องเป็น Mutable เพื่อให้ Method สามารถเปลี่ยนแปลงเนื้อหาของ String ได้

เครื่องหมาย & บ่งชี้ว่า Argument นี้เป็น _Reference_ ซึ่งเป็นวิธีที่ช่วยให้โค้ดหลายส่วนสามารถเข้าถึงข้อมูลชิ้นเดียวกันได้โดยไม่จำเป็นต้อง Copy ข้อมูลนั้นลงใน Memory หลายๆ ครั้ง References เป็น Feature ที่ซับซ้อน และหนึ่งในข้อได้เปรียบหลักของ Rust คือความปลอดภัยและความง่ายในการใช้งาน References คุณไม่จำเป็นต้องรู้รายละเอียดเหล่านั้นมากนักเพื่อทำโปรแกรมนี้ให้เสร็จ สำหรับตอนนี้ สิ่งที่คุณต้องรู้คือ References ก็เป็น Immutable โดยค่าเริ่มต้นเช่นเดียวกับตัวแปร ดังนั้นคุณจึงต้องเขียน `&mut guess` แทนที่จะเป็น `&guess` เพื่อทำให้มันเป็น Mutable (บทที่ 4 จะอธิบายเรื่อง References อย่างละเอียดมากขึ้น)

<!-- Old heading. Do not remove or links may break. -->

<a id="handling-potential-failure-with-the-result-type"></a>

### การจัดการกับความเป็นไปได้ที่จะเกิดข้อผิดพลาดด้วย `Result`

เรายังคงทำงานกับโค้ดบรรทัดเดิมนี้อยู่ ตอนนี้เรากำลังพูดถึงข้อความบรรทัดที่สาม แต่โปรดสังเกตว่ามันยังคงเป็นส่วนหนึ่งของโค้ดบรรทัดเดียวในเชิงตรรกะ ส่วนถัดไปคือ Method นี้:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:expect}}
```

เราสามารถเขียนโค้ดนี้ได้ดังนี้:

```rust,ignore
io::stdin().read_line(&mut guess).expect("Failed to read line");
```

อย่างไรก็ตาม บรรทัดที่ยาวเกินไปนั้นอ่านยาก ดังนั้นจึงควรแบ่งบรรทัด การขึ้นบรรทัดใหม่และการใช้ Whitespace อื่นๆ เพื่อช่วยแบ่งบรรทัดที่ยาวเมื่อคุณเรียก Method ด้วยไวยากรณ์ `.method_name()` มักจะเป็นสิ่งที่ควรทำ ตอนนี้เรามาพูดถึงสิ่งที่บรรทัดนี้ทำกันครับ

ดังที่กล่าวไว้ก่อนหน้านี้ `read_line` จะนำสิ่งที่ผู้ใช้ป้อนเข้าไปใส่ใน String ที่เราส่งให้ แต่ก็จะคืนค่า Result ด้วย `Result` คือ ignore --> is an [_enumeration_][enums]<!-- ignore --> ซึ่งมักจะเรียกว่า _Enum_ 
ซึ่งเป็น Type ที่สามารถมีสถานะที่เป็นไปได้หลายสถานะ เราเรียกแต่ละสถานะที่เป็นไปได้ว่า _Variant_

[Chapter 6][enums]<!-- ignore --> จะกล่าวถึง Enums ในรายละเอียดมากขึ้น จุดประสงค์ของ Type Result เหล่านี้คือการเข้ารหัสข้อมูลการจัดการ Error

Variants ของ `Result` คือ `Ok` และ `Err` Variant `Ok` บ่งชี้ว่าการดำเนินการสำเร็จ และมีค่าที่สร้างขึ้นสำเร็จอยู่ภายใน Variant `Err` หมายความว่าการดำเนินการล้มเหลว และมีข้อมูลเกี่ยวกับวิธีการหรือเหตุผลที่การดำเนินการล้มเหลวอยู่ภายใน

ค่าของ Type `Result` เช่นเดียวกับค่าของ Type อื่นๆ มี Methods ที่ถูกกำหนดไว้ Instance ของ Result มี Method expect ที่คุณสามารถเรียกใช้ได้ หาก Instance ของ `Result` นี้เป็นค่า Err expect จะทำให้โปรแกรม Crash และแสดงข้อความที่คุณส่งเป็น Argument ให้กับ expect หาก Method read_line คืนค่า Err สาเหตุก็น่าจะมาจาก Error ที่มาจากระบบปฏิบัติการเบื้องล่าง หาก Instance ของ Result นี้เป็นค่า Ok expect จะนำค่าที่ Ok เก็บไว้และคืนค่านั้นให้คุณ เพื่อให้คุณสามารถนำไปใช้งานได้ ในกรณีนี้ ค่าดังกล่าวคือจำนวน Byte ใน Input ของผู้ใช้

หากคุณไม่เรียกใช้ expect โปรแกรมจะ Compile ได้ แต่คุณจะได้รับ Warning:

```console
{{#include ../listings/ch02-guessing-game-tutorial/no-listing-02-without-expect/output.txt}}
```
Rust เตือนว่าคุณยังไม่ได้ใช้ค่า `Result` ที่ถูกคืนมาจาก `read_line` ซึ่งบ่งชี้ว่าโปรแกรมยังไม่ได้จัดการกับ Error ที่อาจเกิดขึ้น

วิธีที่ถูกต้องในการไม่ให้แสดง Warning คือการเขียนโค้ดจัดการ Error จริงๆ แต่ในกรณีของเรา เราแค่อยากให้โปรแกรม Crash เมื่อเกิดปัญหา ดังนั้นเราจึงสามารถใช้ expect ได้ คุณจะได้เรียนรู้เกี่ยวกับการ Recover จาก Error ใน [Chapter
9][recover]<!-- ignore -->.

### การพิมพ์ค่าด้วย Placeholders ของ `println!`

นอกเหนือจากวงเล็บปีกกาปิดแล้ว ยังมีโค้ดอีกบรรทัดเดียวที่เราต้องพูดถึงในโค้ดที่เราเขียนไปแล้ว:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:print_guess}}
```

บรรทัดนี้พิมพ์ String ที่ตอนนี้มี Input ของผู้ใช้อยู่ ชุดวงเล็บปีกกา `{}` เป็น Placeholder: ลองนึกภาพ `{}` เหมือนกับก้ามปูเล็กๆ ที่คอยจับค่าไว้ เมื่อพิมพ์ค่าของตัวแปร ชื่อตัวแปรสามารถอยู่ภายในวงเล็บปีกกาได้ เมื่อพิมพ์ผลลัพธ์ของการประเมิน Expression ให้วางวงเล็บปีกกาเปล่าๆ ใน Format String จากนั้นตามด้วยรายการของ Expression ที่คั่นด้วยเครื่องหมายจุลภาคเพื่อพิมพ์ในแต่ละ Placeholder วงเล็บปีกกาเปล่าๆ ในลำดับเดียวกัน การพิมพ์ตัวแปรและผลลัพธ์ของ Expression ในการเรียก `println!` ครั้งเดียวจะมีลักษณะดังนี้:

```rust
let x = 5;
let y = 10;

println!("x = {x} and y + 2 = {}", y + 2);
```

จะแสดงข้อความ `x = 5 and y + 2 = 12`.

### การทดสอบส่วนแรก

มาทดสอบส่วนแรกของเกมทายตัวเลขกันครับ รันโดยใช้คำสั่ง `cargo run`:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-01/
cargo clean
cargo run
input 6 -->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 6.44s
     Running `target/debug/guessing_game`
Guess the number!
Please input your guess.
6
You guessed: 6
```
ตอนนี้โปรแกรมทำงานตามที่เราคาดหวังแล้ว มันพิมพ์ข้อความ Prompt และรอให้เราป้อน Input เมื่อเราป้อนตัวเลข (ในตัวอย่างนี้คือ 6) แล้วกด Enter โปรแกรมก็จะพิมพ์ข้อความ "You guessed: 6" ออกมา

## การสร้างตัวเลขลับ

ต่อไป เราจำเป็นต้องสร้างตัวเลขลับที่ผู้ใช้จะพยายามทาย ตัวเลขลับควรจะแตกต่างกันไปในแต่ละครั้งเพื่อให้เกมสนุกที่จะเล่นมากกว่าหนึ่งครั้ง เราจะใช้ตัวเลขสุ่มระหว่าง 1 ถึง 100 เพื่อให้เกมไม่ยากเกินไป Rust ยังไม่ได้รวมฟังก์ชันการสร้างตัวเลขสุ่มไว้ใน Standard Library อย่างไรก็ตาม ทีม Rust ได้จัดเตรียม [`rand` crate][randcrate] ซึ่งมีฟังก์ชันดังกล่าวไว้ให้

### การใช้ Crate เพื่อเพิ่มความสามารถ

จำได้ไหมว่า Crate คือชุดของไฟล์ Source Code ของ Rust โปรเจกต์ที่เรากำลังสร้างอยู่นี้คือ _Binary Crate_ ซึ่งเป็นไฟล์ Executable ส่วน Crate rand เป็น _Library Crate_ ซึ่งมีโค้ดที่ตั้งใจให้ถูกใช้งานในโปรแกรมอื่นๆ และไม่สามารถ Executable ได้ด้วยตัวเอง

การจัดการ External Crate ของ Cargo เป็นจุดที่ Cargo โดดเด่นอย่างแท้จริง ก่อนที่เราจะสามารถเขียนโค้ดที่ใช้ `rand` ได้ เราจำเป็นต้องแก้ไขไฟล์ _Cargo.toml_ เพื่อรวม Crate `rand` เป็น Dependency เปิดไฟล์นั้นตอนนี้และเพิ่มบรรทัดต่อไปนี้ที่ด้านล่าง ใต้ส่วนหัว `[dependencies]` ที่ Cargo สร้างไว้ให้คุณ โปรดระบุ `rand` ให้ตรงตามที่เรามีที่นี่ โดยมีหมายเลขเวอร์ชันนี้ หรือตัวอย่างโค้ดในบทช่วยสอนนี้อาจใช้งานไม่ได้:


<!-- When updating the version of `rand` used, also update the version of
`rand` used in these files so they all match:
* ch07-04-bringing-paths-into-scope-with-the-use-keyword.md
* ch14-03-cargo-workspaces.md
-->

<span class="filename">Filename: Cargo.toml</span>

```toml
{{#include ../listings/ch02-guessing-game-tutorial/listing-02-02/Cargo.toml:8:}}
```

ในไฟล์ _Cargo.tom_l_ ทุกสิ่งทุกอย่างที่อยู่หลังส่วนหัวจะเป็นส่วนหนึ่งของ Section นั้นๆ ต่อไปจนกว่าจะเริ่มต้น Section อื่น ใน `[dependencies]` คุณจะบอก Cargo ว่าโปรเจกต์ของคุณขึ้นอยู่กับ External Crate ใดบ้าง และต้องการ Crate เหล่านั้นในเวอร์ชันใด ในกรณีนี้ เราได้ระบุ Crate `rand` ด้วยตัวระบุเวอร์ชันแบบ Semantic Versioning คือ `0.8.5` Cargo เข้าใจ Semantic Versioning (บางครั้งเรียกว่า _SemVer_) ซึ่งเป็นมาตรฐานสำหรับการเขียนหมายเลขเวอร์ชัน ตัวระบุ 0.8.5 จริงๆ แล้วเป็นรูปแบบย่อของ ^0.8.5 ซึ่งหมายถึงเวอร์ชันใดๆ ที่มีค่าตั้งแต่ 0.8.5 ขึ้นไป แต่ต่ำกว่า 0.9.0

Cargo พิจารณาว่าเวอร์ชันเหล่านี้มี Public API ที่เข้ากันได้กับเวอร์ชัน 0.8.5 และข้อกำหนดนี้จะช่วยให้คุณได้รับการ Patch Release ล่าสุดที่จะยังคงสามารถ Compile ได้กับโค้ดในบทนี้ เวอร์ชัน 0.9.0 หรือสูงกว่านั้นไม่รับประกันว่าจะมี API เหมือนกับที่ตัวอย่างต่อไปนี้ใช้

ตอนนี้ โดยที่ยังไม่ได้เปลี่ยนแปลงโค้ดใดๆ มา Build โปรเจกต์กันดังที่แสดงใน Listing 2-2 ครับ

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-02/
rm Cargo.lock
cargo clean
cargo build -->

<Listing number="2-2" caption="The output from running `cargo build` after adding the rand crate as a dependency">

```console
$ cargo build
  Updating crates.io index
   Locking 15 packages to latest Rust 1.85.0 compatible versions
    Adding rand v0.8.5 (available: v0.9.0)
 Compiling proc-macro2 v1.0.93
 Compiling unicode-ident v1.0.17
 Compiling libc v0.2.170
 Compiling cfg-if v1.0.0
 Compiling byteorder v1.5.0
 Compiling getrandom v0.2.15
 Compiling rand_core v0.6.4
 Compiling quote v1.0.38
 Compiling syn v2.0.98
 Compiling zerocopy-derive v0.7.35
 Compiling zerocopy v0.7.35
 Compiling ppv-lite86 v0.2.20
 Compiling rand_chacha v0.3.1
 Compiling rand v0.8.5
 Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
  Finished `dev` profile [unoptimized + debuginfo] target(s) in 2.48s
```

</Listing>

คุณอาจเห็นหมายเลขเวอร์ชันที่แตกต่างกัน (แต่ทั้งหมดจะเข้ากันได้กับโค้ด ต้องขอบคุณ SemVer!) และบรรทัดที่แตกต่างกัน (ขึ้นอยู่กับระบบปฏิบัติการ) และลำดับของบรรทัดอาจแตกต่างกัน

เมื่อเรารวม External Dependency เข้ามา Cargo จะดึงเวอร์ชันล่าสุดของทุกสิ่งที่ Dependency นั้นต้องการจาก _Registry_ ซึ่งเป็นสำเนาข้อมูลจาก [Crates.io][cratesio]. เป็นที่ที่ผู้คนในระบบนิเวศ Rust โพสต์โปรเจกต์ Open Source Rust ของตนเพื่อให้ผู้อื่นใช้งาน

หลังจากอัปเดต Registry แล้ว Cargo จะตรวจสอบส่วน `[dependencies]` และดาวน์โหลด Crate ใดๆ ที่ระบุไว้ซึ่งยังไม่ได้ดาวน์โหลด ในกรณีนี้ แม้ว่าเราจะระบุเพียง `rand` เป็น Dependency แต่ Cargo ก็ยังดึง Crate อื่นๆ ที่ `rand` ขึ้นอยู่กับการทำงานมาด้วย หลังจากดาวน์โหลด Crate แล้ว Rust จะคอมไพล์ Crate เหล่านั้น จากนั้นจึงคอมไพล์โปรเจกต์โดยมี Dependencies พร้อมใช้งาน

หากคุณรัน `cargo build` อีกครั้งทันทีโดยไม่มีการเปลี่ยนแปลงใดๆ คุณจะไม่เห็น Output ใดๆ นอกเหนือจากบรรทัด `Finished` Cargo รู้ว่าได้ดาวน์โหลดและคอมไพล์ Dependencies แล้ว และคุณไม่ได้เปลี่ยนแปลงอะไรเกี่ยวกับ Dependencies เหล่านั้นในไฟล์ _Cargo.toml_ ของคุณ Cargo ยังรู้ว่าคุณไม่ได้เปลี่ยนแปลงอะไรเกี่ยวกับโค้ดของคุณ ดังนั้นจึงไม่ได้ Recompile โค้ดนั้นด้วย เมื่อไม่มีอะไรต้องทำ Cargo ก็จะ Exit ไป

หากคุณเปิดไฟล์ _src/main.rs_ ทำการเปลี่ยนแปลงเล็กน้อย จากนั้นบันทึกและ Build อีกครั้ง คุณจะเห็น Output เพียงสองบรรทัด:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-02/
touch src/main.rs
cargo build -->

```console
$ cargo build
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
```

บรรทัดเหล่านี้แสดงให้เห็นว่า Cargo อัปเดตการ Build เฉพาะกับการเปลี่ยนแปลงเล็กน้อยที่คุณทำกับไฟล์ _src/main.rs_ เท่านั้น Dependencies ของคุณไม่ได้เปลี่ยนแปลง ดังนั้น Cargo จึงรู้ว่าสามารถนำสิ่งที่ได้ดาวน์โหลดและคอมไพล์ไว้แล้วสำหรับ Dependencies เหล่านั้นกลับมาใช้ใหม่ได้

#### การรับประกันการ Build ที่ทำซ้ำได้ด้วยไฟล์ _Cargo.lock_

Cargo มีกลไกที่รับประกันว่าคุณจะสามารถ Rebuild Artifact เดิมได้ทุกครั้งที่คุณหรือใครก็ตาม Build โค้ดของคุณ: Cargo จะใช้เฉพาะเวอร์ชันของ Dependencies ที่คุณระบุไว้เท่านั้น จนกว่าคุณจะระบุเป็นอย่างอื่น ตัวอย่างเช่น สมมติว่าสัปดาห์หน้า Crate `rand` เวอร์ชัน 0.8.6 ออกมา และเวอร์ชันนั้นมีการแก้ไข Bug ที่สำคัญ แต่ก็มี Regression ที่จะทำให้โค้ดของคุณเสียหาย เพื่อจัดการกับปัญหานี้ Rust จะสร้างไฟล์ _Cargo.lock_ ในครั้งแรกที่คุณรัน `cargo build` ดังนั้นตอนนี้เราจึงมีไฟล์นี้อยู่ในไดเรกทอรี guessing_game

เมื่อคุณ Build โปรเจกต์เป็นครั้งแรก Cargo จะพิจารณาทุกเวอร์ชันของ Dependencies ที่ตรงตามเกณฑ์ และจากนั้นจะเขียนเวอร์ชันเหล่านั้นลงในไฟล์ _Cargo.lock_ เมื่อคุณ Build โปรเจกต์ของคุณในอนาคต Cargo จะเห็นว่ามีไฟล์ _Cargo.lock_ อยู่แล้ว และจะใช้เวอร์ชันที่ระบุไว้ในนั้น แทนที่จะต้องทำงานทั้งหมดในการพิจารณาเวอร์ชันอีกครั้ง นี่จะช่วยให้คุณมีการ Build ที่ทำซ้ำได้โดยอัตโนมัติ กล่าวอีกนัยหนึ่ง โปรเจกต์ของคุณจะยังคงอยู่ที่เวอร์ชัน 0.8.5 จนกว่าคุณจะอัปเกรดอย่างชัดเจน ต้องขอบคุณไฟล์ _Cargo.lock_ เนื่องจากไฟล์ _Cargo.lock_ มีความสำคัญต่อการ Build ที่ทำซ้ำได้ จึงมักจะถูก Commit เข้า Source Control พร้อมกับโค้ดส่วนอื่นๆ ในโปรเจกต์ของคุณ

#### การอัปเดต Crate เพื่อให้ได้เวอร์ชันใหม่

เมื่อคุณต้องการอัปเดต Crate Cargo มีคำสั่ง `update` ซึ่งจะละเว้นไฟล์ _Cargo.lock_ และพิจารณาทุกเวอร์ชันล่าสุดที่ตรงตามข้อกำหนดของคุณใน _Cargo.toml_ จากนั้น Cargo จะเขียนเวอร์ชันเหล่านั้นลงในไฟล์ Cargo.lock ในกรณีนี้ Cargo จะมองหาเฉพาะเวอร์ชันที่สูงกว่า 0.8.5 และต่ำกว่า 0.9.0 เท่านั้น หาก Crate `rand` ได้เผยแพร่สองเวอร์ชันใหม่คือ 0.8.6 และ 0.9.0 คุณจะเห็นผลลัพธ์ดังต่อไปนี้หากคุณรัน `cargo update`:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-02/
cargo update
assuming there is a new 0.8.x version of rand; otherwise use another update
as a guide to creating the hypothetical output shown here -->

```console
$ cargo update
    Updating crates.io index
     Locking 1 package to latest Rust 1.85.0 compatible version
    Updating rand v0.8.5 -> v0.8.6 (available: v0.9.0)
```

Cargo ละเว้น Release 0.9.0 ณ จุดนี้ คุณจะสังเกตเห็นการเปลี่ยนแปลงในไฟล์ _Cargo.lock_ ของคุณ ซึ่งระบุว่าเวอร์ชันของ Crate `rand` ที่คุณกำลังใช้อยู่คือ 0.8.6 หากต้องการใช้ `rand` เวอร์ชัน 0.9.0 หรือเวอร์ชันใดๆ ใน Series 0.9._x_ คุณจะต้องอัปเดตไฟล์ _Cargo.toml_ ให้มีลักษณะดังนี้แทน:

```toml
[dependencies]
rand = "0.9.0"
```


เมื่อคุณทำการเปลี่ยนแปลงนี้และรัน cargo update อีกครั้ง Cargo จะดึงและใช้เวอร์ชัน 0.9.0 (หรือเวอร์ชันล่าสุดใน Series 0.9.x หากมี) ของ Crate rand และอัปเดตไฟล์ Cargo.lock ตามไปด้วย

ตอนนี้เราได้เพิ่ม Dependency rand เข้าไปใน Cargo.toml แล้ว เรามาเริ่มใช้มันเพื่อสร้างตัวเลขลับกันครับ

### การสร้างตัวเลขสุ่ม

มาเริ่มใช้ `rand` เพื่อสร้างตัวเลขที่เราจะให้ผู้เล่นทายกันครับ ขั้นตอนต่อไปคือการอัปเดต _src/main.rs_ ดังที่แสดงใน Listing 2-3 ครับ

<Listing number="2-3" file-name="src/main.rs" caption="Adding code to generate a random number">

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-03/src/main.rs:all}}
```

</Listing>

ขั้นแรกเราเพิ่มบรรทัด `use rand::Rng;` Trait Rng กำหนด Methods ที่ Random Number Generator Implement และ Trait นี้จะต้องอยู่ใน Scope เพื่อให้เราสามารถใช้ Methods เหล่านั้นได้ บทที่ 10 จะกล่าวถึง Traits ในรายละเอียด

ต่อไป เรากำลังเพิ่มสองบรรทัดตรงกลาง ในบรรทัดแรก เราเรียกใช้ฟังก์ชัน `rand::thread_rng` ซึ่งให้ Random Number Generator ที่เราจะใช้ นั่นคือ Generator ที่ Local กับ Thread ปัจจุบันของการ Execution และถูก Seed โดยระบบปฏิบัติการ จากนั้นเราเรียก Method `gen_range` บน Random Number Generator Method นี้ถูกกำหนดโดย Trait `Rng` ที่เรานำเข้ามาใน Scope ด้วยคำสั่ง `use rand::Rng;` Method `gen_range` รับ Range Expression เป็น Argument และสร้างตัวเลขสุ่มภายใน Range นั้น Range Expression ที่เราใช้อยู่ในที่นี้มีรูปแบบ `start..=end` และรวมทั้งขอบเขตล่างและขอบเขตบน ดังนั้นเราจึงต้องระบุ `1..=100` เพื่อขอตัวเลขระหว่าง 1 ถึง 100

> หมายเหตุ: คุณจะไม่สามารถทราบได้ทันทีว่าจะต้องใช้ Trait ใด และต้องเรียกใช้ Methods และ Functions ใดจาก Crate ดังนั้นแต่ละ Crate จึงมีเอกสารประกอบพร้อมคำแนะนำสำหรับการใช้งาน อีก Feature ที่ยอดเยี่ยมของ Cargo คือการรันคำสั่ง cargo doc --open จะสร้างเอกสารประกอบที่ Crate Dependencies ทั้งหมดของคุณจัดเตรียมไว้ให้ในเครื่องของคุณ และเปิดมันใน Browser หากคุณสนใจฟังก์ชันอื่นๆ ใน Crate rand ตัวอย่างเช่น ให้รัน cargo doc --open และคลิก rand ใน Sidebar ด้านซ้าย

บรรทัดใหม่ที่สองพิมพ์ตัวเลขลับ ซึ่งมีประโยชน์ในขณะที่เรากำลังพัฒนาโปรแกรมเพื่อให้สามารถทดสอบได้ แต่เราจะลบทิ้งในเวอร์ชันสุดท้าย คงไม่สนุกเท่าไหร่ถ้าโปรแกรมพิมพ์คำตอบออกมาทันทีที่เริ่ม!

ลองรันโปรแกรมสองสามครั้ง:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-03/
cargo run
4
cargo run
5
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.02s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 7
Please input your guess.
4
You guessed: 4

$ cargo run
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.02s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 83
Please input your guess.
5
You guessed: 5
```

เยี่ยมไปเลยครับ! คุณควรจะได้รับตัวเลขสุ่มที่แตกต่างกัน และตัวเลขเหล่านั้นทั้งหมดควรอยู่ระหว่าง 1 ถึง 100 ครับ ทำได้ดีมาก!

ตอนนี้เราได้สร้างตัวเลขลับและให้ผู้เล่นป้อนคำทายแล้ว ขั้นตอนต่อไปคือการเปรียบเทียบคำทายของผู้เล่นกับตัวเลขลับ และให้ Feedback แก่ผู้เล่นครับ

## การเปรียบเทียบคำทายกับตัวเลขลับ

ตอนนี้เรามี Input จากผู้ใช้และตัวเลขสุ่มแล้ว เราสามารถนำมาเปรียบเทียบกันได้ ขั้นตอนนั้นแสดงอยู่ใน Listing 2-4 โปรดทราบว่าโค้ดนี้จะยัง Compile ไม่ได้ในตอนนี้ ซึ่งเราจะอธิบายต่อไปครับ

<Listing number="2-4" file-name="src/main.rs" caption="Handling the possible return values of comparing two numbers">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-04/src/main.rs:here}}
```

</Listing>

ขั้นแรก เราเพิ่มคำสั่ง `use` อีกหนึ่งคำสั่ง โดยนำ Type ที่ชื่อ `std::cmp::Ordering` เข้ามาใน Scope จาก Standard Library Type Ordering เป็น Enum อีกประเภทหนึ่ง และมี Variants คือ `Less`, `Greater` และ `Equal` นี่คือสามผลลัพธ์ที่เป็นไปได้เมื่อคุณเปรียบเทียบค่าสองค่า

จากนั้นเราเพิ่มโค้ดใหม่ห้าบรรทัดที่ด้านล่าง ซึ่งใช้ Type `Ordering` Method `cmp` เปรียบเทียบค่าสองค่า และสามารถเรียกใช้ได้กับทุกสิ่งทุกอย่างที่สามารถเปรียบเทียบได้ โดยรับ Reference ไปยังสิ่งที่คุณต้องการเปรียบเทียบด้วย ในที่นี้คือการเปรียบเทียบ `guess` กับ `secret_number` จากนั้น Method นี้จะคืนค่า Variant หนึ่งของ Enum Ordering ที่เรานำเข้ามาใน Scope ด้วยคำสั่ง `use` เราใช้ Expression [`match`][match]<!-- ignore --> เพื่อตัดสินใจว่าจะทำอะไรต่อไป โดยอิงจาก Variant ใดของ `Ordering `ที่ถูกคืนค่ามาจากการเรียกใช้ `cmp` ด้วยค่าใน `guess` และ `secret_number`

Expression `match` ประกอบด้วย _Arms_ แต่ละ Arm ประกอบด้วย _Pattern_ ที่จะนำมา Match และโค้ดที่จะถูกรันหากค่าที่ให้กับ match ตรงกับ Pattern ของ Arm นั้น Rust จะนำค่าที่ให้กับ `match` มาพิจารณาและตรวจสอบ Pattern ของแต่ละ Arm ตามลำดับ Patterns และ Construct `match` เป็น Feature ที่ทรงพลังของ Rust ซึ่งช่วยให้คุณสามารถแสดงสถานการณ์ต่างๆ ที่โค้ดของคุณอาจพบเจอ และทำให้มั่นใจได้ว่าคุณได้จัดการกับสถานการณ์เหล่านั้นทั้งหมด Feature เหล่านี้จะถูกกล่าวถึงในรายละเอียดใน บทที่ 6 และ บทที่ 19 ตามลำดับ

มาดูตัวอย่างการทำงานของ Expression `match` ที่เราใช้ในที่นี้ สมมติว่าผู้ใช้ทาย 50 และตัวเลขลับที่ถูกสร้างขึ้นแบบสุ่มในครั้งนี้คือ 38

เมื่อโค้ดเปรียบเทียบ 50 กับ 38 Method `cmp` จะคืนค่า `Ordering::Greater` เนื่องจาก 50 มากกว่า 38 Expression `match` จะได้รับค่า `Ordering::Greater` และเริ่มตรวจสอบ Pattern ของแต่ละ Arm มันจะดู Pattern ของ Arm แรกคือ `Ordering::Less` และพบว่าค่า `Ordering::Greater` ไม่ตรงกับ `Ordering::Less` ดังนั้นจึงข้ามโค้ดใน Arm นั้นไปและไปยัง Arm ถัดไป Pattern ของ Arm ถัดไปคือ Ordering::Greater ซึ่งตรงกับ Ordering::Greater! โค้ดที่เกี่ยวข้องใน Arm นั้นจะถูก Execute และพิมพ์ `Too big!` ออกทางหน้าจอ Expression `match` จะสิ้นสุดหลังจาก Match สำเร็จครั้งแรก ดังนั้นจึงจะไม่พิจารณา Arm สุดท้ายในสถานการณ์นี้

อย่างไรก็ตาม โค้ดใน Listing 2-4 จะยัง Compile ไม่ได้ ลอง Compile ดูครับ:

<!--
The error numbers in this output should be that of the code **WITHOUT** the
anchor or snip comments
-->

```console
{{#include ../listings/ch02-guessing-game-tutorial/listing-02-04/output.txt}}
```

ใจความสำคัญของ Error ระบุว่ามี Type ไม่ตรงกัน Rust มีระบบ Type ที่แข็งแกร่งและเป็น Static อย่างไรก็ตาม มันก็มีการอนุมาน Type ด้วย เมื่อเราเขียน `let mut guess = String::new()` Rust สามารถอนุมานได้ว่า `guess` ควรจะเป็น `String` และไม่ได้บังคับให้เราเขียน Type เอง ในทางกลับกัน `secret_number` เป็น Type ตัวเลข Type ตัวเลขบางประเภทของ Rust สามารถมีค่าระหว่าง 1 ถึง 100 ได้ เช่น `i32` (เลขจำนวนเต็ม 32 บิต), `u32` (เลขจำนวนเต็มไม่มีเครื่องหมาย 32 บิต), `i64` (เลขจำนวนเต็ม 64 บิต) และอื่นๆ หากไม่ได้ระบุเป็นอย่างอื่น Rust จะ Default เป็น i32 ซึ่งเป็น Type ของ `secret_number` เว้นแต่คุณจะเพิ่มข้อมูล Type ที่อื่นที่จะทำให้ Rust อนุมาน Type ตัวเลขที่แตกต่างออกไป เหตุผลของ Error คือ Rust ไม่สามารถเปรียบเทียบ String กับ Type ตัวเลขได้

ท้ายที่สุด เราต้องการแปลง `String` ที่โปรแกรมอ่านเป็น Input ให้เป็น Type ตัวเลข เพื่อให้เราสามารถเปรียบเทียบเชิงตัวเลขกับตัวเลขลับได้ เราทำได้โดยการเพิ่มบรรทัดนี้ในส่วน Body ของฟังก์ชัน `main`:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-03-convert-string-to-number/src/main.rs:here}}
```

ที่บรรทัด:

```rust,ignore
let guess: u32 = guess.trim().parse().expect("Please type a number!");
```

เราสร้างตัวแปรชื่อ `guess` แต่เดี๋ยวก่อน โปรแกรมมีตัวแปรชื่อ `guess` อยู่แล้วไม่ใช่เหรอ? ใช่ครับ แต่ Rust อนุญาตให้เรา Shadow ค่าเดิมของ `guess` ด้วยค่าใหม่ได้อย่างมีประโยชน์ การ _Shadow_ ช่วยให้เราสามารถใช้ชื่อตัวแปร `guess` ซ้ำได้ แทนที่จะบังคับให้เราสร้างตัวแปรที่ไม่ซ้ำกันสองตัว เช่น `guess_str` และ `guess` เป็นต้น เราจะกล่าวถึงเรื่องนี้ในรายละเอียดเพิ่มเติมใน [Chapter 3][shadowing]<!-- ignore -->, แต่สำหรับตอนนี้ ให้ทราบว่า Feature นี้มักใช้เมื่อคุณต้องการแปลงค่าจาก Type หนึ่งไปเป็นอีก Type หนึ่ง

เราผูกตัวแปรใหม่นี้กับ Expression `guess.trim().parse()` ตัวแปร `guess` ใน Expression อ้างถึงตัวแปร `guess` เดิมที่เก็บ Input เป็น String Method `trim` บน Instance ของ `String` จะลบ Whitespace ใดๆ ที่อยู่ด้านหน้าและด้านหลัง ซึ่งเราจำเป็นต้องทำก่อนที่เราจะสามารถแปลง String เป็น u32 ซึ่งสามารถมีได้เฉพาะข้อมูลตัวเลขเท่านั้น ผู้ใช้จะต้องกด <kbd>enter</kbd> เพื่อให้ read_line ทำงานเสร็จสิ้นและป้อนคำทาย ซึ่งจะเพิ่ม Character ขึ้นบรรทัดใหม่ (\n) เข้าไปใน String ตัวอย่างเช่น หากผู้ใช้พิมพ์ 5 แล้วกด enter guess จะมีลักษณะดังนี้: 5\n ตัว \n แทน "ขึ้นบรรทัดใหม่" (บน Windows การกด enter จะส่งผลให้มี Carriage Return และ Line Feed คือ \r\n) Method trim จะลบ \n หรือ \r\n ออก ทำให้เหลือเพียง 5

Method [`parse` method on strings][parse]<!-- ignore --> บน String แปลง String เป็น Type อื่น ในที่นี้ เราใช้เพื่อแปลงจาก String เป็นตัวเลข เราจำเป็นต้องบอก Rust ถึง Type ตัวเลขที่แน่นอนที่เราต้องการโดยใช้ `let guess: u32` เครื่องหมาย Colon (`:`) หลัง `guess` บอก Rust ว่าเราจะระบุ Type ของตัวแปร Rust มี Type ตัวเลข Built-in อยู่สองสามประเภท `u32` ที่เห็นในที่นี้คือ Integer ไม่มีเครื่องหมายขนาด 32 บิต มันเป็นตัวเลือก Default ที่ดีสำหรับตัวเลขบวกขนาดเล็ก คุณจะได้เรียนรู้เกี่ยวกับ Type ตัวเลขอื่นๆ ใน [Chapter 3][integers]<!-- ignore -->

นอกจากนี้ การระบุ Type `u32` ในโปรแกรมตัวอย่างนี้และการเปรียบเทียบกับ `secret_number` หมายความว่า Rust จะอนุมานว่า `secret_number` ควรเป็น `u32` ด้วยเช่นกัน ดังนั้นตอนนี้การเปรียบเทียบจะเป็นระหว่างค่าสองค่าที่มี Type เดียวกัน!

Method `parse` จะทำงานได้เฉพาะกับ Character ที่สามารถแปลงเป็นตัวเลขได้อย่างสมเหตุสมผลเท่านั้น ดังนั้นจึงสามารถทำให้เกิด Error ได้ง่าย หาก String มี `A👍%` ตัวอย่างเช่น จะไม่มีทางแปลงเป็นตัวเลขได้ เนื่องจากอาจล้มเหลว Method `parse` จึงคืนค่า Type `Result` เช่นเดียวกับ Method `read_line` (กล่าวถึงก่อนหน้านี้ใน "การจัดการกับความเป็นไปได้ที่จะเกิดข้อผิดพลาดด้วย Result") เราจะจัดการกับ Result นี้ในลักษณะเดียวกันโดยใช้ Method expect อีกครั้ง หาก parse คืนค่า Variant Err ของ `Result` เนื่องจากไม่สามารถสร้างตัวเลขจาก String ได้ การเรียก `expect` จะทำให้เกม Crash และพิมพ์ข้อความที่เราให้ไว้ หาก parse สามารถแปลง String เป็นตัวเลขได้สำเร็จ มันจะคืนค่า Variant `Ok` ของ `Result` และ `expect` จะคืนค่าตัวเลขที่เราต้องการจากค่า `Ok`


ลองรันโปรแกรมดู:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/no-listing-03-convert-string-to-number/
touch src/main.rs
cargo run
  76
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.26s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 58
Please input your guess.
  76
You guessed: 76
Too big!
```

เยี่ยมไปเลย! แม้ว่าจะมีการเพิ่ม Space ก่อนคำทาย โปรแกรมก็ยังคงเข้าใจว่าผู้ใช้ทาย 76 ลองรันโปรแกรมสองสามครั้งเพื่อตรวจสอบพฤติกรรมที่แตกต่างกันเมื่อมี Input ประเภทต่างๆ: ทายตัวเลขถูกต้อง ทายตัวเลขที่สูงเกินไป และทายตัวเลขที่ต่ำเกินไป

ตอนนี้เกมของเราทำงานได้เกือบทั้งหมดแล้ว แต่ผู้ใช้สามารถทายได้เพียงครั้งเดียวเท่านั้น มาเปลี่ยนแปลงสิ่งนั้นโดยการเพิ่ม Loop กันครับ!

## อนุญาตให้ทายหลายครั้งด้วยการวนซ้ำ

Keyword `loop` สร้าง Loop ที่ไม่มีวันสิ้นสุด เราจะเพิ่ม Loop เพื่อให้ผู้ใช้มีโอกาสทายตัวเลขมากขึ้น:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-04-looping/src/main.rs:here}}
```

อย่างที่คุณเห็น เราได้ย้ายทุกอย่างตั้งแต่ Prompt ให้ป้อนคำทายลงไปใน Loop โปรดตรวจสอบให้แน่ใจว่าคุณได้ Indent บรรทัดที่อยู่ภายใน Loop เพิ่มอีกสี่ Space และรันโปรแกรมอีกครั้ง ตอนนี้โปรแกรมจะถามคำทายใหม่ไปเรื่อยๆ ซึ่งจริงๆ แล้วทำให้เกิดปัญหาใหม่ ดูเหมือนว่าผู้ใช้จะไม่สามารถออกจากโปรแกรมได้!

ผู้ใช้สามารถ Interrupt โปรแกรมได้เสมอโดยใช้ Keyboard Shortcut <kbd>ctrl</kbd>-<kbd>c</kbd> แต่มีอีกวิธีในการหลีกหนีจากสัตว์ประหลาดที่ไม่รู้จักอิ่มนี้ ดังที่กล่าวไว้ในการอภิปรายเรื่อง parse ใน "การเปรียบเทียบคำทายกับตัวเลขลับ" หากผู้ใช้ป้อนคำตอบที่ไม่ใช่ตัวเลข โปรแกรมจะ Crash เราสามารถใช้ประโยชน์จากสิ่งนั้นเพื่อให้ผู้ใช้สามารถออกจากโปรแกรมได้ ดังที่แสดงไว้ที่นี่:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/no-listing-04-looping/
touch src/main.rs
cargo run
(too small guess)
(too big guess)
(correct guess)
quit
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.23s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 59
Please input your guess.
45
You guessed: 45
Too small!
Please input your guess.
60
You guessed: 60
Too big!
Please input your guess.
59
You guessed: 59
You win!
Please input your guess.
quit

thread 'main' panicked at src/main.rs:28:47:
Please type a number!: ParseIntError { kind: InvalidDigit }
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

การพิมพ์ quit จะออกจากเกม แต่ดังที่คุณจะสังเกตเห็น การป้อน Input ที่ไม่ใช่ตัวเลขอื่นๆ ก็จะทำให้เป็นเช่นเดียวกัน นี่ไม่ใช่สิ่งที่เหมาะสมเลย อย่างน้อยที่สุด เราต้องการให้เกมหยุดเมื่อมีการทายตัวเลขที่ถูกต้องด้วย

### การออกจากเกมหลังทายถูก

มาเขียนโปรแกรมให้เกมออกจาก Loop เมื่อผู้ใช้ทายถูก โดยการเพิ่มคำสั่ง `break` เข้าไปครับ

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-05-quitting/src/main.rs:here}}
```

การเพิ่มบรรทัด `break` หลัง `"You win!"` ทำให้โปรแกรมออกจาก Loop เมื่อผู้ใช้ทายตัวเลขลับได้อย่างถูกต้อง การออกจาก Loop ก็หมายถึงการออกจากโปรแกรมด้วย เนื่องจาก Loop เป็นส่วนสุดท้ายของฟังก์ชัน `main`

ตอนนี้เกมทายตัวเลขของเราก็สมบูรณ์แล้ว! ผู้ใช้สามารถป้อนคำทายได้เรื่อยๆ โปรแกรมจะบอกว่าคำทายนั้นสูงหรือต่ำเกินไป และเมื่อผู้ใช้ทายถูก โปรแกรมจะแสดงข้อความแสดงความยินดีและจบการทำงานครับ

### การจัดการ Input ที่ไม่ถูกต้อง

เพื่อปรับปรุงพฤติกรรมของเกมให้ดียิ่งขึ้น แทนที่จะให้โปรแกรม Crash เมื่อผู้ใช้ป้อน Input ที่ไม่ใช่ตัวเลข เราจะทำให้เกมละเว้น Input ที่ไม่ใช่ตัวเลขนั้น เพื่อให้ผู้ใช้สามารถทายต่อไปได้ เราสามารถทำได้โดยการแก้ไขบรรทัดที่แปลง `guess` จาก `String` เป็น `u32` ดังที่แสดงใน Listing 2-5 ครับ

<Listing number="2-5" file-name="src/main.rs" caption="Ignoring a non-number guess and asking for another guess instead of crashing the program">

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-05/src/main.rs:here}}
```

</Listing>

เราเปลี่ยนจากการเรียกใช้ Method `expect` ไปเป็นการใช้ Expression `match` เพื่อเปลี่ยนจากการ Crash เมื่อเกิด Error ไปเป็นการจัดการ Error จำได้ไหมว่า `parse` คืนค่า Type `Result` และ `Result` เป็น Enum ที่มี Variants คือ `Ok` และ `Err` เรากำลังใช้ Expression `match` ที่นี่ เช่นเดียวกับที่เราใช้กับผลลัพธ์ `Ordering` ของ Method `cmp`

หาก `parse` สามารถแปลง String เป็นตัวเลขได้อย่างสำเร็จ มันจะคืนค่า `Ok` ที่มีตัวเลขผลลัพธ์อยู่ภายใน ค่า `Ok` นั้นจะตรงกับ Pattern ของ Arm แรก และ Expression `match` จะคืนค่า num ที่ parse สร้างขึ้นและใส่ไว้ในค่า `Ok` ตัวเลขนั้นจะไปอยู่ในตำแหน่งที่เราต้องการในตัวแปร `guess` ใหม่ที่เรากำลังสร้าง

หาก `parse` ไม่สามารถแปลง String เป็นตัวเลขได้ มันจะคืนค่า Err ที่มีข้อมูลเพิ่มเติมเกี่ยวกับ Error ค่า Err ไม่ตรงกับ Pattern `Ok(num)` ใน Arm แรกของ `match` แต่ตรงกับ Pattern `Err(_)` ใน Arm ที่สอง Underscore (_) เป็นค่าที่จับคู่ทุกกรณี ในตัวอย่างนี้ เรากำลังบอกว่าเราต้องการ Match ค่า `Err` ทั้งหมด ไม่ว่าจะมีข้อมูลอะไรอยู่ภายในก็ตาม ดังนั้นโปรแกรมจะ Execute โค้ดของ Arm ที่สองคือ `continue` ซึ่งบอกให้โปรแกรมไปยัง Iteration ถัดไปของ `loop` และขอคำทายอีกครั้ง ดังนั้น โดยพื้นฐานแล้ว โปรแกรมจะละเว้น Error ทั้งหมดที่ `parse` อาจพบ!

ตอนนี้ทุกอย่างในโปรแกรมควรทำงานได้ตามที่คาดหวัง ลองรันดูครับ:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-05/
cargo run
(too small guess)
(too big guess)
foo
(correct guess)
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 61
Please input your guess.
10
You guessed: 10
Too small!
Please input your guess.
99
You guessed: 99
Too big!
Please input your guess.
foo
Please input your guess.
61
You guessed: 61
You win!
```

เยี่ยมไปเลย! ด้วยการปรับแต่งเล็กน้อยครั้งสุดท้าย เราก็จะทำให้เกมทายตัวเลขเสร็จสมบูรณ์ จำได้ไหมว่าโปรแกรมยังคงพิมพ์ตัวเลขลับออกมา ซึ่งใช้ได้ดีสำหรับการทดสอบ แต่ทำให้เกมหมดสนุก มาลบคำสั่ง `println!` ที่แสดงตัวเลขลับออกกันครับ Listing 2-6 แสดงโค้ดสุดท้ายครับ

<Listing number="2-6" file-name="src/main.rs" caption="Complete guessing game code">

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-06/src/main.rs}}
```

</Listing>

ครับ! ในที่สุดเราก็สร้างเกมทายตัวเลขได้สำเร็จ ขอแสดงความยินดีด้วยนะครับ!

## สรุป

โปรเจกต์นี้เป็นวิธีลงมือปฏิบัติจริงเพื่อแนะนำ Concepts ใหม่ๆ ของ Rust มากมายให้กับคุณ ไม่ว่าจะเป็น `let`, `match`, ฟังก์ชัน การใช้ External Crate และอื่นๆ ในบทต่อไป คุณจะได้เรียนรู้เกี่ยวกับ Concepts เหล่านี้ในรายละเอียดมากขึ้น บทที่ 3 กล่าวถึง Concepts ที่ภาษา Programming ส่วนใหญ่มี เช่น ตัวแปร Type ข้อมูล และฟังก์ชัน และแสดงวิธีใช้งานใน Rust บทที่ 4 จะสำรวจ Ownership ซึ่งเป็น Feature ที่ทำให้ Rust แตกต่างจากภาษาอื่นๆ บทที่ 5 กล่าวถึง Structs และ Method Syntax และ บทที่ 6 อธิบายวิธีการทำงานของ Enums ครับ

[prelude]: ../std/prelude/index.html
[variables-and-mutability]: ch03-01-variables-and-mutability.html#variables-and-mutability
[comments]: ch03-04-comments.html
[string]: ../std/string/struct.String.html
[iostdin]: ../std/io/struct.Stdin.html
[read_line]: ../std/io/struct.Stdin.html#method.read_line
[result]: ../std/result/enum.Result.html
[enums]: ch06-00-enums.html
[expect]: ../std/result/enum.Result.html#method.expect
[recover]: ch09-02-recoverable-errors-with-result.html
[randcrate]: https://crates.io/crates/rand
[semver]: http://semver.org
[cratesio]: https://crates.io/
[doccargo]: https://doc.rust-lang.org/cargo/
[doccratesio]: https://doc.rust-lang.org/cargo/reference/publishing.html
[match]: ch06-02-match.html
[shadowing]: ch03-01-variables-and-mutability.html#shadowing
[parse]: ../std/primitive.str.html#method.parse
[integers]: ch03-02-data-types.html#integer-types
