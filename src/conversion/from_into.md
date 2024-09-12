# `From` и `Into`

Сущности [`From`](https://doc.rust-lang.org/std/convert/trait.From.html) и [`Into`](https://doc.rust-lang.org/std/convert/trait.Into.html) связаны по своей сути, и это стало частью их реализации. Если вы можете преобразовать вид `А` в вид `В`, то будет легко предположить, что мы должны быть в состоянии преобразовать вид `В` в вид `А`.

## `From`

Сущность [`From`](https://doc.rust-lang.org/std/convert/trait.From.html) позволяет виду определить, как он будет создаваться из другого вида, что предоставляет очень простой рычаг преобразования между несколькими видами. Есть несколько реализаций этот сущности во встроенной библиотеке для преобразования простейших и общих видов.

Для примера, мы можем легко преобразовать `str` в `String`

```rust
let my_str = "привет";
let my_string = String::from(my_str);
```

Мы можем сделать нечто похожее для определения преобразования для нашего собственного вида.

```rust,editable
use std::convert::From;

#[derive(Debug)]
struct Number {
    value: i32,
}

impl From<i32> for Number {
    fn from(item: i32) -> Self {
        Number { value: item }
    }
}

fn main() {
    let num = Number::from(30);
    println!("Мой номер {:?}", num);
}
```

## `Into`

Сущность [`Into`](https://doc.rust-lang.org/std/convert/trait.Into.html) является полной противоположностью сущности `From`. Так что если вы реализовали для вашего вида сущность  `From`, реализацию сущности `Into` вы получите бесплатно.

Использование сущности `Into` обычно требует спецификации вида, в который мы собираемся преобразовать, так как сборщик чаще всего не может это вывести.
Однако это небольшой компромисс, учитывая, что данную возможности мы получаем бесплатно.

```rust,editable
use std::convert::From;

#[derive(Debug)]
struct Number {
    value: i32,
}

impl From<i32> for Number {
    fn from(item: i32) -> Self {
        Number { value: item }
    }
}

fn main() {
    let int = 5;
    // Попробуйте убрать изложение вида
    let num: Number = int.into();
    println!("Мой номер {:?}", num);
}
```
