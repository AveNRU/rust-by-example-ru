# Свойство `Derive`

Сборщик способен предоставить основные реализации для некоторых сущностей
с помощью [свойства](../attribute.md) `#[derive]`. Эти сущности могут быть
реализованы вручную, если необходимо более сложное поведение.

Ниже приводится список выводимых сущностей:

- Сущности сравнения:[`Eq`](https://doc.rust-lang.org/std/cmp/trait.Eq.html), [`PartialEq`](https://doc.rust-lang.org/std/cmp/trait.PartialEq.html), [`Ord`](https://doc.rust-lang.org/std/cmp/trait.Ord.html), [`PartialOrd`](https://doc.rust-lang.org/std/cmp/trait.PartialOrd.html)
- [`Clone`](https://doc.rust-lang.org/std/clone/trait.Clone.html), для создания `T` из `&T` с помощью копии.
- [`Copy`](https://doc.rust-lang.org/core/marker/trait.Copy.html), чтобы создать вид семантикой копирования, вместо семантики перемещения.
- [`Hash`](https://doc.rust-lang.org/std/hash/trait.Hash.html), чтобы вычислить хеш из `&T`.
- [`Default`](https://doc.rust-lang.org/std/default/trait.Default.html), чтобы создать пустой экземпляр вида данных.
- [`Debug`](https://doc.rust-lang.org/std/fmt/trait.Debug.html), чтобы отизмененять значение с помощью `{:?}`.

```rust,editable
// `Centimeters`, упорядоченная стопка, которую можно сравнить
#[derive(PartialEq, PartialOrd)]
struct Centimeters(f64);

// `Inches`, упорядоченная стопка, которую можно навыводить
#[derive(Debug)]
struct Inches(i32);

impl Inches {
    fn to_centimeters(&self) -> Centimeters {
        let &Inches(inches) = self;

        Centimeters(inches as f64 * 2.54)
    }
}

// `Seconds`, упорядоченная стопка без дополнительных свойств
struct Seconds(i32);

fn main() {
    let _one_second = Seconds(1);

    // Ошибка: `Seconds` не может быть выведена; не реализован сущность `Debug`
    //println!("Одна секунда выглядит как: {:?}", _one_second);
    // ЗАДАНИЕ ^ Попробуйте убрать примечания эту строку

    // Ошибка: `Seconds` нельзя сравнить; не реализован сущность `PartialEq`
    //let _this_is_true = (_one_second == _one_second);
    // ЗАДАНИЕ ^ Попробуйте убрать примечания эту строку

    let foot = Inches(12);

    println!("Один фут равен {:?}", foot);

    let meter = Centimeters(100.0);

    let cmp =
        if foot.to_centimeters() < meter {
            "меньше"
        } else {
            "больше"
        };

    println!("Один фут {} одного метра.", cmp);
}
```

### Смотрите также:

[`derive`](https://doc.rust-lang.org/reference/attributes.html#derive)
