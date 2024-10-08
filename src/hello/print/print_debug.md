# Debug

Все виды, которые будут использовать `сущности (traits)` изменения `std::fmt` требуют
их реализации для возможности вывода. Автоматическая реализация предоставлена только для
видов из `встроенной библиотеки (std)`. Все остальные виды *должны* иметь собственную реализацию.

C помощью `сущности` `fmt::Debug` это сделать очень просто. *Все* виды могут
`выводить` (по умолчанию создавать) реализацию `fmt::Debug`.
Сделать подобное с `fmt::Display` невозможно, он должен быть реализован вручную.

```rust
// Эта стопка не может быть выведена с помощью `fmt::Display`
// или с помощью `fmt::Debug`
struct UnPrintable(i32);

// Свойство `derive` самостоятельно реализует
// необходимые способы, чтобы была возможность
// выводить данную `стопку` с помощью `fmt::Debug`.
#[derive(Debug)]
struct DebugPrintable(i32);
```

Все виды в `встроенной библиотеке (std)` могут быть выведены с `{:?}`:

```rust,editable
// Вывод и реализация `fmt::Debug` для `Structure`.
// `Structure` - это стопка, которая содержит в себе один `i32`.
#[derive(Debug)]
struct Structure(i32);

// Добавим стопку `Structure` в стопку `Deep`.
// Реализуем для `Deep` возможность вывода с помощью fmt::Debug`.
#[derive(Debug)]
struct Deep(Structure);

fn main() {
    // Вывод с помощью `{:?}` аналогичен `{}`.
    println!("{:?} месяцев в году.", 12);
    println!("{1:?} {0:?} - это имя {actor:?}.",
             "Слэйтер",
             "Кристиан",
             actor="актёра");

    // `Structure` теперь можно навыводить!
    println!("Теперь {:?} будет выведена на экран!", Structure(3));

    // Неполадка с `выводом (derive)`, в том, что у нас не будет контроля
    // над тем, как будет выглядеть итог.
    // Что если мы хотим навыводить просто `7`?
    println!("А теперь выведем {:?}", Deep(Structure(7)));
}
```

Так что 'fmt:: Debug' определенно делает это для вывода, но жертвует некоторыми
изящество. Ржавчина также обеспечивает "красивую печать " с помощью" {:#?}'.

```rust,editable
#[derive(Debug)]
struct Person<'a> {
    name: &'a str,
    age: u8
}

fn main() {
    let name = "Peter";
    let age = 27;
    let peter = Person { name, age };

    // Pretty print
    println!("{:#?}", peter);
}
```

Можно вручную реализовать 'fmt:: Display' для управления отображением.

### Смотрите также

[attributes](https://doc.rust-lang.org/reference/attributes.html), [`derive`](../../trait/derive.md), [`std::fmt`](https://doc.rust-lang.org/std/fmt/),
и [`struct`](../../custom_types/structs.md)
