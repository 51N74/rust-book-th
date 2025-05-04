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

### Receiving User Input

Recall that we included the input/output functionality from the standard
library with `use std::io;` on the first line of the program. Now we’ll call
the `stdin` function from the `io` module, which will allow us to handle user
input:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:read}}
```

If we hadn’t imported the `io` module with `use std::io;` at the beginning of
the program, we could still use the function by writing this function call as
`std::io::stdin`. The `stdin` function returns an instance of
[`std::io::Stdin`][iostdin]<!-- ignore -->, which is a type that represents a
handle to the standard input for your terminal.

Next, the line `.read_line(&mut guess)` calls the [`read_line`][read_line]<!--
ignore --> method on the standard input handle to get input from the user.
We’re also passing `&mut guess` as the argument to `read_line` to tell it what
string to store the user input in. The full job of `read_line` is to take
whatever the user types into standard input and append that into a string
(without overwriting its contents), so we therefore pass that string as an
argument. The string argument needs to be mutable so the method can change the
string’s content.

The `&` indicates that this argument is a _reference_, which gives you a way to
let multiple parts of your code access one piece of data without needing to
copy that data into memory multiple times. References are a complex feature,
and one of Rust’s major advantages is how safe and easy it is to use
references. You don’t need to know a lot of those details to finish this
program. For now, all you need to know is that, like variables, references are
immutable by default. Hence, you need to write `&mut guess` rather than
`&guess` to make it mutable. (Chapter 4 will explain references more
thoroughly.)

<!-- Old heading. Do not remove or links may break. -->

<a id="handling-potential-failure-with-the-result-type"></a>

### Handling Potential Failure with `Result`

We’re still working on this line of code. We’re now discussing a third line of
text, but note that it’s still part of a single logical line of code. The next
part is this method:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:expect}}
```

We could have written this code as:

```rust,ignore
io::stdin().read_line(&mut guess).expect("Failed to read line");
```

However, one long line is difficult to read, so it’s best to divide it. It’s
often wise to introduce a newline and other whitespace to help break up long
lines when you call a method with the `.method_name()` syntax. Now let’s
discuss what this line does.

As mentioned earlier, `read_line` puts whatever the user enters into the string
we pass to it, but it also returns a `Result` value. [`Result`][result]<!--
ignore --> is an [_enumeration_][enums]<!-- ignore -->, often called an _enum_,
which is a type that can be in one of multiple possible states. We call each
possible state a _variant_.

[Chapter 6][enums]<!-- ignore --> will cover enums in more detail. The purpose
of these `Result` types is to encode error-handling information.

`Result`’s variants are `Ok` and `Err`. The `Ok` variant indicates the
operation was successful, and it contains the successfully generated value.
The `Err` variant means the operation failed, and it contains information
about how or why the operation failed.

Values of the `Result` type, like values of any type, have methods defined on
them. An instance of `Result` has an [`expect` method][expect]<!-- ignore -->
that you can call. If this instance of `Result` is an `Err` value, `expect`
will cause the program to crash and display the message that you passed as an
argument to `expect`. If the `read_line` method returns an `Err`, it would
likely be the result of an error coming from the underlying operating system.
If this instance of `Result` is an `Ok` value, `expect` will take the return
value that `Ok` is holding and return just that value to you so you can use it.
In this case, that value is the number of bytes in the user’s input.

If you don’t call `expect`, the program will compile, but you’ll get a warning:

```console
{{#include ../listings/ch02-guessing-game-tutorial/no-listing-02-without-expect/output.txt}}
```

Rust warns that you haven’t used the `Result` value returned from `read_line`,
indicating that the program hasn’t handled a possible error.

The right way to suppress the warning is to actually write error-handling code,
but in our case we just want to crash this program when a problem occurs, so we
can use `expect`. You’ll learn about recovering from errors in [Chapter
9][recover]<!-- ignore -->.

### Printing Values with `println!` Placeholders

Aside from the closing curly bracket, there’s only one more line to discuss in
the code so far:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:print_guess}}
```

This line prints the string that now contains the user’s input. The `{}` set of
curly brackets is a placeholder: think of `{}` as little crab pincers that hold
a value in place. When printing the value of a variable, the variable name can
go inside the curly brackets. When printing the result of evaluating an
expression, place empty curly brackets in the format string, then follow the
format string with a comma-separated list of expressions to print in each empty
curly bracket placeholder in the same order. Printing a variable and the result
of an expression in one call to `println!` would look like this:

```rust
let x = 5;
let y = 10;

println!("x = {x} and y + 2 = {}", y + 2);
```

This code would print `x = 5 and y + 2 = 12`.

### Testing the First Part

Let’s test the first part of the guessing game. Run it using `cargo run`:

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

At this point, the first part of the game is done: we’re getting input from the
keyboard and then printing it.

## Generating a Secret Number

Next, we need to generate a secret number that the user will try to guess. The
secret number should be different every time so the game is fun to play more
than once. We’ll use a random number between 1 and 100 so the game isn’t too
difficult. Rust doesn’t yet include random number functionality in its standard
library. However, the Rust team does provide a [`rand` crate][randcrate] with
said functionality.

### Using a Crate to Get More Functionality

Remember that a crate is a collection of Rust source code files. The project
we’ve been building is a _binary crate_, which is an executable. The `rand`
crate is a _library crate_, which contains code that is intended to be used in
other programs and can’t be executed on its own.

Cargo’s coordination of external crates is where Cargo really shines. Before we
can write code that uses `rand`, we need to modify the _Cargo.toml_ file to
include the `rand` crate as a dependency. Open that file now and add the
following line to the bottom, beneath the `[dependencies]` section header that
Cargo created for you. Be sure to specify `rand` exactly as we have here, with
this version number, or the code examples in this tutorial may not work:

<!-- When updating the version of `rand` used, also update the version of
`rand` used in these files so they all match:
* ch07-04-bringing-paths-into-scope-with-the-use-keyword.md
* ch14-03-cargo-workspaces.md
-->

<span class="filename">Filename: Cargo.toml</span>

```toml
{{#include ../listings/ch02-guessing-game-tutorial/listing-02-02/Cargo.toml:8:}}
```

In the _Cargo.toml_ file, everything that follows a header is part of that
section that continues until another section starts. In `[dependencies]` you
tell Cargo which external crates your project depends on and which versions of
those crates you require. In this case, we specify the `rand` crate with the
semantic version specifier `0.8.5`. Cargo understands [Semantic
Versioning][semver]<!-- ignore --> (sometimes called _SemVer_), which is a
standard for writing version numbers. The specifier `0.8.5` is actually
shorthand for `^0.8.5`, which means any version that is at least 0.8.5 but
below 0.9.0.

Cargo considers these versions to have public APIs compatible with version
0.8.5, and this specification ensures you’ll get the latest patch release that
will still compile with the code in this chapter. Any version 0.9.0 or greater
is not guaranteed to have the same API as what the following examples use.

Now, without changing any of the code, let’s build the project, as shown in
Listing 2-2.

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

You may see different version numbers (but they will all be compatible with the
code, thanks to SemVer!) and different lines (depending on the operating
system), and the lines may be in a different order.

When we include an external dependency, Cargo fetches the latest versions of
everything that dependency needs from the _registry_, which is a copy of data
from [Crates.io][cratesio]. Crates.io is where people in the Rust ecosystem
post their open source Rust projects for others to use.

After updating the registry, Cargo checks the `[dependencies]` section and
downloads any crates listed that aren’t already downloaded. In this case,
although we only listed `rand` as a dependency, Cargo also grabbed other crates
that `rand` depends on to work. After downloading the crates, Rust compiles
them and then compiles the project with the dependencies available.

If you immediately run `cargo build` again without making any changes, you
won’t get any output aside from the `Finished` line. Cargo knows it has already
downloaded and compiled the dependencies, and you haven’t changed anything
about them in your _Cargo.toml_ file. Cargo also knows that you haven’t changed
anything about your code, so it doesn’t recompile that either. With nothing to
do, it simply exits.

If you open the _src/main.rs_ file, make a trivial change, and then save it and
build again, you’ll only see two lines of output:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-02/
touch src/main.rs
cargo build -->

```console
$ cargo build
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
```

These lines show that Cargo only updates the build with your tiny change to the
_src/main.rs_ file. Your dependencies haven’t changed, so Cargo knows it can
reuse what it has already downloaded and compiled for those.

#### Ensuring Reproducible Builds with the _Cargo.lock_ File

Cargo has a mechanism that ensures you can rebuild the same artifact every time
you or anyone else builds your code: Cargo will use only the versions of the
dependencies you specified until you indicate otherwise. For example, say that
next week version 0.8.6 of the `rand` crate comes out, and that version
contains an important bug fix, but it also contains a regression that will
break your code. To handle this, Rust creates the _Cargo.lock_ file the first
time you run `cargo build`, so we now have this in the _guessing_game_
directory.

When you build a project for the first time, Cargo figures out all the versions
of the dependencies that fit the criteria and then writes them to the
_Cargo.lock_ file. When you build your project in the future, Cargo will see
that the _Cargo.lock_ file exists and will use the versions specified there
rather than doing all the work of figuring out versions again. This lets you
have a reproducible build automatically. In other words, your project will
remain at 0.8.5 until you explicitly upgrade, thanks to the _Cargo.lock_ file.
Because the _Cargo.lock_ file is important for reproducible builds, it’s often
checked into source control with the rest of the code in your project.

#### Updating a Crate to Get a New Version

When you _do_ want to update a crate, Cargo provides the command `update`,
which will ignore the _Cargo.lock_ file and figure out all the latest versions
that fit your specifications in _Cargo.toml_. Cargo will then write those
versions to the _Cargo.lock_ file. In this case, Cargo will only look for
versions greater than 0.8.5 and less than 0.9.0. If the `rand` crate has
released the two new versions 0.8.6 and 0.9.0, you would see the following if
you ran `cargo update`:

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

Cargo ignores the 0.9.0 release. At this point, you would also notice a change
in your _Cargo.lock_ file noting that the version of the `rand` crate you are
now using is 0.8.6. To use `rand` version 0.9.0 or any version in the 0.9._x_
series, you’d have to update the _Cargo.toml_ file to look like this instead:

```toml
[dependencies]
rand = "0.9.0"
```

The next time you run `cargo build`, Cargo will update the registry of crates
available and reevaluate your `rand` requirements according to the new version
you have specified.

There’s a lot more to say about [Cargo][doccargo]<!-- ignore --> and [its
ecosystem][doccratesio]<!-- ignore -->, which we’ll discuss in Chapter 14, but
for now, that’s all you need to know. Cargo makes it very easy to reuse
libraries, so Rustaceans are able to write smaller projects that are assembled
from a number of packages.

### Generating a Random Number

Let’s start using `rand` to generate a number to guess. The next step is to
update _src/main.rs_, as shown in Listing 2-3.

<Listing number="2-3" file-name="src/main.rs" caption="Adding code to generate a random number">

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-03/src/main.rs:all}}
```

</Listing>

First we add the line `use rand::Rng;`. The `Rng` trait defines methods that
random number generators implement, and this trait must be in scope for us to
use those methods. Chapter 10 will cover traits in detail.

Next, we’re adding two lines in the middle. In the first line, we call the
`rand::thread_rng` function that gives us the particular random number
generator we’re going to use: one that is local to the current thread of
execution and is seeded by the operating system. Then we call the `gen_range`
method on the random number generator. This method is defined by the `Rng`
trait that we brought into scope with the `use rand::Rng;` statement. The
`gen_range` method takes a range expression as an argument and generates a
random number in the range. The kind of range expression we’re using here takes
the form `start..=end` and is inclusive on the lower and upper bounds, so we
need to specify `1..=100` to request a number between 1 and 100.

> Note: You won’t just know which traits to use and which methods and functions
> to call from a crate, so each crate has documentation with instructions for
> using it. Another neat feature of Cargo is that running the `cargo doc
> --open` command will build documentation provided by all your dependencies
> locally and open it in your browser. If you’re interested in other
> functionality in the `rand` crate, for example, run `cargo doc --open` and
> click `rand` in the sidebar on the left.

The second new line prints the secret number. This is useful while we’re
developing the program to be able to test it, but we’ll delete it from the
final version. It’s not much of a game if the program prints the answer as soon
as it starts!

Try running the program a few times:

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

You should get different random numbers, and they should all be numbers between
1 and 100. Great job!

## Comparing the Guess to the Secret Number

Now that we have user input and a random number, we can compare them. That step
is shown in Listing 2-4. Note that this code won’t compile just yet, as we will
explain.

<Listing number="2-4" file-name="src/main.rs" caption="Handling the possible return values of comparing two numbers">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-04/src/main.rs:here}}
```

</Listing>

First we add another `use` statement, bringing a type called
`std::cmp::Ordering` into scope from the standard library. The `Ordering` type
is another enum and has the variants `Less`, `Greater`, and `Equal`. These are
the three outcomes that are possible when you compare two values.

Then we add five new lines at the bottom that use the `Ordering` type. The
`cmp` method compares two values and can be called on anything that can be
compared. It takes a reference to whatever you want to compare with: here it’s
comparing `guess` to `secret_number`. Then it returns a variant of the
`Ordering` enum we brought into scope with the `use` statement. We use a
[`match`][match]<!-- ignore --> expression to decide what to do next based on
which variant of `Ordering` was returned from the call to `cmp` with the values
in `guess` and `secret_number`.

A `match` expression is made up of _arms_. An arm consists of a _pattern_ to
match against, and the code that should be run if the value given to `match`
fits that arm’s pattern. Rust takes the value given to `match` and looks
through each arm’s pattern in turn. Patterns and the `match` construct are
powerful Rust features: they let you express a variety of situations your code
might encounter and they make sure you handle them all. These features will be
covered in detail in Chapter 6 and Chapter 19, respectively.

Let’s walk through an example with the `match` expression we use here. Say that
the user has guessed 50 and the randomly generated secret number this time is
38.

When the code compares 50 to 38, the `cmp` method will return
`Ordering::Greater` because 50 is greater than 38. The `match` expression gets
the `Ordering::Greater` value and starts checking each arm’s pattern. It looks
at the first arm’s pattern, `Ordering::Less`, and sees that the value
`Ordering::Greater` does not match `Ordering::Less`, so it ignores the code in
that arm and moves to the next arm. The next arm’s pattern is
`Ordering::Greater`, which _does_ match `Ordering::Greater`! The associated
code in that arm will execute and print `Too big!` to the screen. The `match`
expression ends after the first successful match, so it won’t look at the last
arm in this scenario.

However, the code in Listing 2-4 won’t compile yet. Let’s try it:

<!--
The error numbers in this output should be that of the code **WITHOUT** the
anchor or snip comments
-->

```console
{{#include ../listings/ch02-guessing-game-tutorial/listing-02-04/output.txt}}
```

The core of the error states that there are _mismatched types_. Rust has a
strong, static type system. However, it also has type inference. When we wrote
`let mut guess = String::new()`, Rust was able to infer that `guess` should be
a `String` and didn’t make us write the type. The `secret_number`, on the other
hand, is a number type. A few of Rust’s number types can have a value between 1
and 100: `i32`, a 32-bit number; `u32`, an unsigned 32-bit number; `i64`, a
64-bit number; as well as others. Unless otherwise specified, Rust defaults to
an `i32`, which is the type of `secret_number` unless you add type information
elsewhere that would cause Rust to infer a different numerical type. The reason
for the error is that Rust cannot compare a string and a number type.

Ultimately, we want to convert the `String` the program reads as input into a
number type so we can compare it numerically to the secret number. We do so by
adding this line to the `main` function body:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-03-convert-string-to-number/src/main.rs:here}}
```

The line is:

```rust,ignore
let guess: u32 = guess.trim().parse().expect("Please type a number!");
```

We create a variable named `guess`. But wait, doesn’t the program already have
a variable named `guess`? It does, but helpfully Rust allows us to shadow the
previous value of `guess` with a new one. _Shadowing_ lets us reuse the `guess`
variable name rather than forcing us to create two unique variables, such as
`guess_str` and `guess`, for example. We’ll cover this in more detail in
[Chapter 3][shadowing]<!-- ignore -->, but for now, know that this feature is
often used when you want to convert a value from one type to another type.

We bind this new variable to the expression `guess.trim().parse()`. The `guess`
in the expression refers to the original `guess` variable that contained the
input as a string. The `trim` method on a `String` instance will eliminate any
whitespace at the beginning and end, which we must do before we can convert the
string to a `u32`, which can only contain numerical data. The user must press
<kbd>enter</kbd> to satisfy `read_line` and input their guess, which adds a
newline character to the string. For example, if the user types <kbd>5</kbd> and
presses <kbd>enter</kbd>, `guess` looks like this: `5\n`. The `\n` represents
“newline.” (On Windows, pressing <kbd>enter</kbd> results in a carriage return
and a newline, `\r\n`.) The `trim` method eliminates `\n` or `\r\n`, resulting
in just `5`.

The [`parse` method on strings][parse]<!-- ignore --> converts a string to
another type. Here, we use it to convert from a string to a number. We need to
tell Rust the exact number type we want by using `let guess: u32`. The colon
(`:`) after `guess` tells Rust we’ll annotate the variable’s type. Rust has a
few built-in number types; the `u32` seen here is an unsigned, 32-bit integer.
It’s a good default choice for a small positive number. You’ll learn about
other number types in [Chapter 3][integers]<!-- ignore -->.

Additionally, the `u32` annotation in this example program and the comparison
with `secret_number` means Rust will infer that `secret_number` should be a
`u32` as well. So now the comparison will be between two values of the same
type!

The `parse` method will only work on characters that can logically be converted
into numbers and so can easily cause errors. If, for example, the string
contained `A👍%`, there would be no way to convert that to a number. Because it
might fail, the `parse` method returns a `Result` type, much as the `read_line`
method does (discussed earlier in [“Handling Potential Failure with
`Result`”](#handling-potential-failure-with-result)<!-- ignore-->). We’ll treat
this `Result` the same way by using the `expect` method again. If `parse`
returns an `Err` `Result` variant because it couldn’t create a number from the
string, the `expect` call will crash the game and print the message we give it.
If `parse` can successfully convert the string to a number, it will return the
`Ok` variant of `Result`, and `expect` will return the number that we want from
the `Ok` value.

Let’s run the program now:

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

Nice! Even though spaces were added before the guess, the program still figured
out that the user guessed 76. Run the program a few times to verify the
different behavior with different kinds of input: guess the number correctly,
guess a number that is too high, and guess a number that is too low.

We have most of the game working now, but the user can make only one guess.
Let’s change that by adding a loop!

## Allowing Multiple Guesses with Looping

The `loop` keyword creates an infinite loop. We’ll add a loop to give users
more chances at guessing the number:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-04-looping/src/main.rs:here}}
```

As you can see, we’ve moved everything from the guess input prompt onward into
a loop. Be sure to indent the lines inside the loop another four spaces each
and run the program again. The program will now ask for another guess forever,
which actually introduces a new problem. It doesn’t seem like the user can quit!

The user could always interrupt the program by using the keyboard shortcut
<kbd>ctrl</kbd>-<kbd>c</kbd>. But there’s another way to escape this insatiable
monster, as mentioned in the `parse` discussion in [“Comparing the Guess to the
Secret Number”](#comparing-the-guess-to-the-secret-number)<!-- ignore -->: if
the user enters a non-number answer, the program will crash. We can take
advantage of that to allow the user to quit, as shown here:

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

Typing `quit` will quit the game, but as you’ll notice, so will entering any
other non-number input. This is suboptimal, to say the least; we want the game
to also stop when the correct number is guessed.

### Quitting After a Correct Guess

Let’s program the game to quit when the user wins by adding a `break` statement:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-05-quitting/src/main.rs:here}}
```

Adding the `break` line after `You win!` makes the program exit the loop when
the user guesses the secret number correctly. Exiting the loop also means
exiting the program, because the loop is the last part of `main`.

### Handling Invalid Input

To further refine the game’s behavior, rather than crashing the program when
the user inputs a non-number, let’s make the game ignore a non-number so the
user can continue guessing. We can do that by altering the line where `guess`
is converted from a `String` to a `u32`, as shown in Listing 2-5.

<Listing number="2-5" file-name="src/main.rs" caption="Ignoring a non-number guess and asking for another guess instead of crashing the program">

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-05/src/main.rs:here}}
```

</Listing>

We switch from an `expect` call to a `match` expression to move from crashing
on an error to handling the error. Remember that `parse` returns a `Result`
type and `Result` is an enum that has the variants `Ok` and `Err`. We’re using
a `match` expression here, as we did with the `Ordering` result of the `cmp`
method.

If `parse` is able to successfully turn the string into a number, it will
return an `Ok` value that contains the resultant number. That `Ok` value will
match the first arm’s pattern, and the `match` expression will just return the
`num` value that `parse` produced and put inside the `Ok` value. That number
will end up right where we want it in the new `guess` variable we’re creating.

If `parse` is _not_ able to turn the string into a number, it will return an
`Err` value that contains more information about the error. The `Err` value
does not match the `Ok(num)` pattern in the first `match` arm, but it does
match the `Err(_)` pattern in the second arm. The underscore, `_`, is a
catch-all value; in this example, we’re saying we want to match all `Err`
values, no matter what information they have inside them. So the program will
execute the second arm’s code, `continue`, which tells the program to go to the
next iteration of the `loop` and ask for another guess. So, effectively, the
program ignores all errors that `parse` might encounter!

Now everything in the program should work as expected. Let’s try it:

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

Awesome! With one tiny final tweak, we will finish the guessing game. Recall
that the program is still printing the secret number. That worked well for
testing, but it ruins the game. Let’s delete the `println!` that outputs the
secret number. Listing 2-6 shows the final code.

<Listing number="2-6" file-name="src/main.rs" caption="Complete guessing game code">

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-06/src/main.rs}}
```

</Listing>

At this point, you’ve successfully built the guessing game. Congratulations!

## Summary

This project was a hands-on way to introduce you to many new Rust concepts:
`let`, `match`, functions, the use of external crates, and more. In the next
few chapters, you’ll learn about these concepts in more detail. Chapter 3
covers concepts that most programming languages have, such as variables, data
types, and functions, and shows how to use them in Rust. Chapter 4 explores
ownership, a feature that makes Rust different from other languages. Chapter 5
discusses structs and method syntax, and Chapter 6 explains how enums work.

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
