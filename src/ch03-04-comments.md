## Comments

นัก Programming ทุกคนพยายามทำให้โค้ดของตนเองเข้าใจง่าย แต่บางครั้งคำอธิบายเพิ่มเติมก็มีความจำเป็น ในกรณีเหล่านี้ นัก Programming จะใส่ Comments ใน Source Code ซึ่ง Compiler จะ Ignore แต่ผู้ที่อ่าน Source Code อาจพบว่ามีประโยชน์ครับ

นี่คือ Comment อย่างง่าย:

```rust
// hello, world
```

ใน Rust รูปแบบ Comment ที่เป็นมาตรฐานจะเริ่มต้น Comment ด้วยเครื่องหมายสแลชสองตัว (//) และ Comment จะดำเนินต่อไปจนสุดบรรทัด สำหรับ Comment ที่ยาวเกินหนึ่งบรรทัด คุณจะต้องใส่ `//` ในแต่ละบรรทัด ดังนี้:

```rust
// So we're doing something complicated here, long enough that we need
// multiple lines of comments to do it! Whew! Hopefully, this comment will
// explain what's going on.
```

Comments สามารถวางไว้ที่ท้ายบรรทัดที่มี Code ได้เช่นกัน:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-24-comments-end-of-line/src/main.rs}}
```

แต่คุณจะเห็นการใช้งานในรูปแบบนี้บ่อยกว่า โดยมี Comment อยู่ในบรรทัดแยกต่างหากเหนือ Code ที่ Comment นั้นอธิบาย:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-25-comments-above-line/src/main.rs}}
```

Rust ยังมี Comment อีกประเภทหนึ่งคือ Documentation Comments ซึ่งเราจะกล่าวถึงในส่วน [“Publishing a Crate to Crates.io”][publishing]<!-- ignore --> ของ บทที่ 14 ครับ


[publishing]: ch14-02-publishing-to-crates-io.html
