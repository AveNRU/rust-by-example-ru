# Изменение

Мы видели, что изменение задаётся *макросом изменения*:

* `format!("{}", foo)` -> `"3735928559"`
* `format!("0x{:X}", foo)` ->
  [`"0xDEADBEEF"`][deadbeef]
* `format!("0o{:o}", foo)` -> `"0o33653337357"`

Одна и та же переменная (`foo`) может быть отображена по разному в зависимости от
используемого *вида переменной*: `X`, `o` или *неопределённый*.

Возможности изменения реализован благодаря сущности,
и для каждого вида переменной существует свой.
Наиболее распространённый сущность для изменения - `Display`,
который работает без переменных: `{}`, например.

```rust,editable
use std::fmt::{self, Formatter, Display};

struct City {
    name: &'static str,
    // Широта
    lat: f32,
    // Долгота
    lon: f32,
}

impl Display for City {
    // `f` - это буфер, данный способ должен записать в него измененную строку
    fn fmt(&self, f: &mut Formatter) -> fmt::Result {
        let lat_c = if self.lat >= 0.0 { 'N' } else { 'S' };
        let lon_c = if self.lon >= 0.0 { 'E' } else { 'W' };

        // `write!` похож на `format!`, только он запишет измененную строку
        // в буфер (первый переменная функции)
        write!(f, "{}: {:.3}°{} {:.3}°{}",
               self.name, self.lat.abs(), lat_c, self.lon.abs(), lon_c)
    }
}

#[derive(Debug)]
struct Color {
    red: u8,
    green: u8,
    blue: u8,
}

fn main() {
    for city in [
        City { name: "Дублин", lat: 53.347778, lon: -6.259722 },
        City { name: "Осло", lat: 59.95, lon: 10.75 },
        City { name: "Ванкувер", lat: 49.25, lon: -123.1 },
    ].iter() {
        println!("{}", *city);
    }
    for color in [
        Color { red: 128, green: 255, blue: 90 },
        Color { red: 0, green: 3, blue: 254 },
        Color { red: 0, green: 0, blue: 0 },
    ].iter() {
        // Поменяйте {:?} на {}, когда добавите реализацию
        // сущности fmt::Display
        println!("{:?}", *color)
    }
}
```

Вы можете посмотреть [полный список сущностей изменения][fmt_traits] и их виды переменных в пособии к [`std::fmt`][fmt].

### Задание

Добавьте реализацию сущности `fmt::Display` для стопки `Color`,
чтобы вывод отображался вот так:

```text
RGB (128, 255, 90) 0x80FF5A
RGB (0, 3, 254) 0x0003FE
RGB (0, 0, 0) 0x000000
```

Пару подсказок, если вы не знаете, что делать:
 * Вам [возможно потребуется перечислить каждый цвет несколько раз][argument_types],
 * Вы можете [добавить немного нулей][fmt_width] с `:02`.

### Смотрите также

[`std::fmt`][fmt]

[argument_types]: https://doc.rust-lang.org/std/fmt/#argument-types
[deadbeef]: https://en.wikipedia.org/wiki/Deadbeef#Magic_debug_values
[fmt]: https://doc.rust-lang.org/std/fmt/
[fmt_traits]: https://doc.rust-lang.org/std/fmt/#formatting-traits
[fmt_width]: https://doc.rust-lang.org/std/fmt/#width
