# `Option`

Иногда желательно перехватить ошибку в какой-либо части программы
вместо вызова сбоя с помощью макроса `panic!`. Это можно сделать
с помощью перечисления `Option`.

Перечисление `Option<T>` имеет два исхода:

* `None`, указывающий о наличии ошибки или отсутствия значений
* `Some(value)`, сложный вид данных, обёртка для `значения` вида `T`.

```rust,editable,ignore,mdbook-runnable
// Целочисленное деление, которое не вызывает `panic!`
fn checked_division(dividend: i32, divisor: i32) -> Option<i32> {
    if divisor == 0 {
        // В случае ошибки возвращаем `None`
        None
    } else {
        // Итог деления возвращаем в исходе `Some`
        Some(dividend / divisor)
    }
}

// Эта функция обрабатывает деление, которое может выполнится с ошибкой
fn try_division(dividend: i32, divisor: i32) {
    // Значение вида `Option` могут быть сопоставлены по образцу
    match checked_division(dividend, divisor) {
        None => println!("{} / {} вызвало ошибку!", dividend, divisor),
        Some(quotient) => {
            println!("{} / {} = {}", dividend, divisor, quotient)
        },
    }
}

fn main() {
    try_division(4, 2);
    try_division(1, 0);

    // Привязка `None` к переменной должна быть изложеной по виду
    let none: Option<i32> = None;
    let _equivalent_none = None::<i32>;

    let optional_float = Some(0f32);

    // Распаковка исхода `Some` будет извлекать данные, которые в нем находятся.
    println!("{:?} распаковывается в {:?}", optional_float, optional_float.unwrap());

    // Распаковка исхода `None` вызовет `panic!`
    println!("{:?} распаковывается в {:?}", none, none.unwrap());
}
```
