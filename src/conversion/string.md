# `FromStr` и `ToString`

## Преобразование в строку

Преобразовать любой вид в `String` так же просто, как и реализовать для него сущность [`ToString`](https://doc.rust-lang.org/std/string/trait.ToString.html). Вместо того, чтобы делать это напрямую, вы должны реализовать сущность [`fmt::Display`](https://doc.rust-lang.org/std/fmt/trait.Display.html), который самостоятельно предоставляет реализацию [`ToString`](https://doc.rust-lang.org/std/string/trait.ToString.html), а 
также позволяет вывести вид, как обсуждалось в разделе [`print!`](../hello/print.md).

```rust,editable
use std::fmt;

struct Circle {
    radius: i32
}

impl fmt::Display for Circle {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "Круг радиусом {}", self.radius)
    }
}

fn main() {
    let circle = Circle { radius: 6 };
    println!("{}", circle.to_string());
}
```

## Разбор строки

Один из наиболее общим видов преобразования - это преобразование строки в число. Идиоматический подход это сделать при помощи функции [`parse`](https://doc.rust-lang.org/std/primitive.str.html#method.parse) и указания вида, в который будем преобразовывать, что можно сделать либо через выведение вида, либо при помощи 'turbofish'-правил написания.

Это преобразует строку в указанный вид при условии, что для этого вида реализован сущность [`FromStr`](https://doc.rust-lang.org/std/str/trait.FromStr.html). 
Он реализован для множества видов обычной библиотеки. 
Чтобы получить эту возможности для пользовательского вида, надо просто реализовать для этого вида сущность [`FromStr`](https://doc.rust-lang.org/std/str/trait.FromStr.html).

```rust
fn main() {
    let parsed: i32 = "5".parse().unwrap();
    let turbo_parsed = "10".parse::<i32>().unwrap();

    let sum = parsed + turbo_parsed;
    println!("Сумма: {:?}", sum);
}
```
