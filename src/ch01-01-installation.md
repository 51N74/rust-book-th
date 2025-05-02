## การติดตั้ง

ขั้นตอนแรกคือการติดตั้ง Rust เราจะดาวน์โหลด Rust ผ่าน `rustup` ซึ่งเป็นเครื่องมือบรรทัดคำสั่งสำหรับจัดการเวอร์ชัน Rust และเครื่องมือที่เกี่ยวข้อง คุณจะต้องมีการเชื่อมต่ออินเทอร์เน็ตสำหรับการดาวน์โหลด

> หมายเหตุ: หากคุณไม่ต้องการใช้ `rustup` ด้วยเหตุผลบางประการ โปรดดูหน้า 
> [Other Rust Installation Methods page][otherinstall] สำหรับตัวเลือกเพิ่มเติม

ขั้นตอนต่อไปนี้จะติดตั้ง Rust compiler เวอร์ชันเสถียรล่าสุด การรับประกันความเสถียรของ Rust ทำให้มั่นใจได้ว่าตัวอย่างทั้งหมดในหนังสือเล่มนี้ที่คอมไพล์ได้จะยังคงคอมไพล์ได้กับ Rust เวอร์ชันใหม่กว่า ผลลัพธ์อาจแตกต่างกันเล็กน้อยระหว่างเวอร์ชันเนื่องจาก Rust มักจะปรับปรุงข้อความแสดงข้อผิดพลาดและคำเตือน กล่าวอีกนัยหนึ่ง Rust เวอร์ชันเสถียรใหม่กว่าที่คุณติดตั้งโดยใช้ขั้นตอนเหล่านี้ควรทำงานได้ตามที่คาดไว้กับเนื้อหาของหนังสือเล่มนี้

> ### Command Line 
>
> ในบทนี้และตลอดทั้งเล่ม เราจะแสดงคำสั่งบางคำสั่งที่ใช้ในเทอร์มินัล
บรรทัดที่คุณควรป้อนในเทอร์มินัลทั้งหมดจะเริ่มต้นด้วย $ คุณไม่จำเป็นต้องพิมพ์อักขระ $ นั่นคือพรอมต์บรรทัดคำสั่งที่แสดงเพื่อระบุจุดเริ่มต้นของแต่ละคำสั่ง บรรทัดที่ไม่ได้เริ่มต้นด้วย $ โดยทั่วไปจะแสดงผลลัพธ์ของคำสั่งก่อนหน้า นอกจากนี้ ตัวอย่างเฉพาะ PowerShell จะใช้ > แทน `$





### การติดตั้ง `rustup` บน Linux หรือ macOS

ถ้าคุณใช้งาน Linux หรือ macOS, เปิด เทอมินัล และ วางคำสั่งด้านล่าง:

```console
$ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

คำสั่งจะดาวน์โหลด และติดตั้ง `rustup` เวอร์ชั่นล่าสุดให้ คุณอาจจะต้องใส่รหัสของเครื่องคุณ หากติดตั้งสำเร็จ คุณจะเห็นข้อความว่า

```text
Rust is installed now. Great!
```

คุณจะต้องมี _linker_ ด้วย ซึ่งเป็นโปรแกรมที่ Rust ใช้เพื่อรวมผลลัพธ์ที่คอมไพล์แล้วให้เป็นไฟล์เดียว เป็นไปได้ว่าคุณมี linker อยู่แล้ว หากคุณได้รับข้อผิดพลาดเกี่ยวกับ linker คุณควรติดตั้ง C compiler ซึ่งโดยทั่วไปจะรวม linker มาด้วย C compiler ยังมีประโยชน์เนื่องจาก Rust packages ทั่วไปบางตัวขึ้นอยู่กับโค้ด C และจำเป็นต้องมี C compiler

บน macOS คุณสามารถรับ C compiler ได้โดยการรันคำสั่ง::

```console
$ xcode-select --install
```

ผู้ใช้ Linux โดยทั่วไปควรติดตั้ง GCC หรือ Clang ตามเอกสารของ Distribution ที่ใช้งาน ตัวอย่างเช่น หากคุณใช้ Ubuntu คุณสามารถติดตั้งแพ็กเกจ `build-essential` ได้

### การติดตั้ง `rustup` บน Windows

บน Windows ให้ไปที่ [https://www.rust-lang.org/tools/install][install] และทำตามคำแนะนำสำหรับการติดตั้ง Rust ในบางขั้นตอนของการติดตั้ง คุณจะได้รับแจ้งให้ติดตั้ง Visual Studio ซึ่งมี linker และไลบรารีเนทีฟที่จำเป็นสำหรับการคอมไพล์โปรแกรม หากคุณต้องการความช่วยเหลือเพิ่มเติมในขั้นตอนนี้ โปรดดูที่
[https://rust-lang.github.io/rustup/installation/windows-msvc.html][msvc]

ส่วนที่เหลือของหนังสือเล่มนี้ใช้คำสั่งที่ทำงานได้ทั้งใน _cmd.exe_ และ PowerShell หากมีความแตกต่างเฉพาะเจาะจง เราจะอธิบายว่าควรใช้อันไหน

### การแก้ไขปัญหา

เพื่อตรวจสอบว่าคุณได้ติดตั้ง Rust อย่างถูกต้องหรือไม่ ให้เปิดเชลล์และป้อนบรรทัดนี้:

```console
$ rustc --version
```

คุณควรเห็นหมายเลขเวอร์ชัน, commit hash และ commit date สำหรับเวอร์ชันเสถียรล่าสุดที่ได้รับการเผยแพร่ ในรูปแบบต่อไปนี้:

```text
rustc x.y.z (abcabcabc yyyy-mm-dd)
```

หากคุณเห็นข้อมูลนี้ แสดงว่าคุณได้ติดตั้ง Rust สำเร็จแล้ว! หากคุณไม่เห็นข้อมูลนี้ ให้ตรวจสอบว่า Rust อยู่ในตัวแปรระบบ %PATH% ของคุณดังนี้

ใน Windows CMD ให้ใช้:

```console
> echo %PATH%
```

ใน PowerShell ให้ใช้:

```powershell
> echo $env:Path
```

ใน Linux and macOS, ให้ใช้:

```console
$ echo $PATH
```

หากทุกอย่างถูกต้องและ Rust ยังคงไม่ทำงาน มีหลายแห่งที่คุณสามารถขอความช่วยเหลือได้ ค้นหาวิธีติดต่อ Rustaceans คนอื่นๆ (ชื่อเล่นตลกๆ ที่พวกเราเรียกกันเอง) ได้ที่ [the community page][community].

### การอัปเดตและการถอนการติดตั้ง

เมื่อติดตั้ง Rust ผ่าน `rustup` แล้ว การอัปเดตเป็นเวอร์ชันที่ใหม่จะทำได้ง่าย เพียงรันสคริปต์อัปเดตต่อไปนี้จากเชลล์ของคุณ:

```console
$ rustup update
```

หากต้องการถอนการติดตั้ง Rust และ `rustup` ให้รันสคริปต์ถอนการติดตั้งต่อไปนี้จากเชลล์ของคุณ:

```console
$ rustup self uninstall
```

### เอกสารประกอบในเครื่อง

การติดตั้ง Rust ยังรวมถึงสำเนาเอกสารประกอบในเครื่องด้วย เพื่อให้คุณสามารถอ่านได้แบบออฟไลน์ เพียงรันคำสั่ง `rustup doc` เพื่อเปิดเอกสารประกอบในเครื่องในเบราว์เซอร์ของคุณ

เมื่อใดก็ตามที่มี Type หรือ Function ใดๆ ที่มาจาก Standard Library และคุณไม่แน่ใจว่ามันทำงานอย่างไร หรือใช้งานอย่างไร ให้ใช้เอกสาร Application Programming Interface (API) เพื่อค้นหาข้อมูล!

### โปรแกรมแก้ไขข้อความและ Integrated Development Environments

หนังสือเล่มนี้ไม่ได้กำหนดว่าคุณต้องใช้เครื่องมือใดในการเขียนโค้ด Rust โปรแกรมแก้ไขข้อความเกือบทุกชนิดสามารถทำงานนี้ได้! อย่างไรก็ตาม โปรแกรมแก้ไขข้อความและ Integrated Development Environments (IDEs) หลายตัวมีการรองรับ Rust ในตัว คุณสามารถค้นหารายการโปรแกรมแก้ไขและ IDE ที่ค่อนข้างทันสมัยได้เสมอใน [the tools
page][tools] บนเว็บไซต์ Rust

### Working Offline with This Book

In several examples, we will use Rust packages beyond the standard library. To
work through those examples, you will either need to have an internet connection
or to have downloaded those dependencies ahead of time. To download the
dependencies ahead of time, you can run the following commands. (We’ll explain
what `cargo` is and what each of these commands does in detail later.)

```console
$ cargo new get-dependencies
$ cd get-dependencies
$ cargo add rand@0.8.5 trpl@0.2.0
```

This will cache the downloads for these packages so you will not need to
download them later. Once you have run this command, you do not need to keep the
`get-dependencies` folder. If you have run this command, you can use the
`--offline` flag with all `cargo` commands in the rest of the book to use these
cached versions instead of attempting to use the network. 

[otherinstall]: https://forge.rust-lang.org/infra/other-installation-methods.html
[install]: https://www.rust-lang.org/tools/install
[msvc]: https://rust-lang.github.io/rustup/installation/windows-msvc.html
[community]: https://www.rust-lang.org/community
[tools]: https://www.rust-lang.org/tools
