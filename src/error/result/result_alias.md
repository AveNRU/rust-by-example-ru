# Псевдонимы для `Result`

Как насчёт случая, когда мы хотим использовать определенный вид `Result` много раз?
Напомним, что Ржавчина позволяет нам создавать [псевдонимы][typealias]. Мы можем
удобно объявить псевдоним для определенного `Result`.

Особенно полезным может быть создание псевдонимов на уровне разделов. Ошибки,
найденные в определенном разделе, часто имеют один и тот же вид `Err`, поэтому один
псевдоним может коротко объявить *все* ассоциированные `Results`.
Это настолько полезно, что библиотека `std` обеспечивает даже один: `io::Result`!

Ниже приведён краткий пример для отображения правил написания:

```rust,editable
use std::num::ParseIntError;

// Объявим обобщённый псевдоним для `Result` с видом ошибки `ParseIntError`.
type AliasedResult<T> = Result<T, ParseIntError>;

// Используем вышеуказанный псевдоним для обозначения
// нашего определенного вида `Result`.
fn multiply(first_number_str: &str, second_number_str: &str) -> AliasedResult<i32> {
    first_number_str.parse::<i32>().and_then(|first_number| {
        second_number_str.parse::<i32>().map(|second_number| first_number * second_number)
    })
}

// Здесь псевдоним снова позволяет нам сэкономить место.
fn print(result: AliasedResult<i32>) {
    match result {
        Ok(n)  => println!("n это {}", n),
        Err(e) => println!("Ошибка: {}", e),
    }
}

fn main() {
    print(multiply("10", "2"));
    print(multiply("t", "2"));
}
```

### Смотрите также:

[`io::Result`][io_result]

[typealias]: types/alias.html
[io_result]: https://doc.rust-lang.org/std/io/type.Result.html
