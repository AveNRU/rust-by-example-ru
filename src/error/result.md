# `Result`

[`Result`](https://doc.rust-lang.org/std/result/enum.Result.html) является более богатой версией вида [`Option`](https://doc.rust-lang.org/std/option/enum.Option.html), вид который описывает возможную *ошибку* вместо возможного её *отсутствия*.

`Result<T, E>` имеет два возможных значения:

- `Ok<T>`: Значение вида `T`
- `Err<E>`: Ошибка обработки элемента, вида `E`

По соглашению, ожидаемый итог `Ok`, тогда как не ожидаемый - `Err`.

Подобно `Option`, `Result` имеет множество ассоциированных с ним способов. Например, `unwrap()` или возвращает `T`, или вызывает `panic`. 
Для обработки итога у `Result` существует множество комбинаторов, которые совпадают с комбинаторами `Option`.

При работе с Ржачина вы, скорее всего, столкнётесь с способами, которые возвращают вид Result, например способ parse(). Не всегда
можно разобрать строку в другой вид, поэтому parse() возвращает Result, указывающий на возможный сбой.

Давайте посмотрим, что происходит, когда мы успешно и безуспешно попытаемся преобразовать строку с помощью `parse()`:

```rust,editable,ignore,mdbook-runnable
fn multiply(first_number_str: &str, second_number_str: &str) -> i32 {
    // Давайте попробуем использовать `unwrap()` чтобы получить число. Он нас укусит?
    let first_number = first_number_str.parse::<i32>().unwrap();
    let second_number = second_number_str.parse::<i32>().unwrap();
    first_number * second_number
}

fn main() {
    let twenty = multiply("10", "2");
    println!("удовоенное {}", twenty);

    let tt = multiply("t", "2");
    println!("удвоенное {}", tt);
}
```

При неудаче, `parse()` оставляет на с ошибкой, с 
которой `unwrap()` вызывает `panic`. Дополнительно, `panic` завершает нашу программу и 
предоставляет неприятное сообщение об ошибке.

Для повышения качества наших сообщений об ошибка, мы должны более явно указать возвращаемый вид и рассмотреть возможной явной обработки ошибок.

## Использование `Result` в `main`

Также `Result` может быть возвращаемым видом функции `main`, если это указано явно. Обычно функция `main` имеют следующую форму:

```rust
fn main() {
    println!("Hello World!");
}
```

Однако `main` также может и возвращать вид `Result`. Если ошибка происходит в пределах функции `main`, то она возвращает рукопись ошибки и выводит отладочное представление ошибки (используя сущность [`Debug`](https://doc.rust-lang.org/std/fmt/trait.Debug.html)). Следующий пример показывает такой сценарий и затрагивает аспекты, описанные в [последующем разделе](result/early_returns.md).

```rust,editable
use std::num::ParseIntError;

fn main() -> Result<(), ParseIntError> {
    let number_str = "10";
    let number = match number_str.parse::<i32>() {
        Ok(number)  => number,
        Err(e) => return Err(e),
    };
    println!("{}", number);
    Ok(())
}
```
