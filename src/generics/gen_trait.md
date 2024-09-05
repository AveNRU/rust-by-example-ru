# Сущности

Конечно `сущности` тоже могут быть обобщёнными. Здесь мы определяем, тот
который повторно реализует `сущность` `Drop` как обобщённый способ, чтобы
удалить себя и входные данные.

```rust,editable
// Некопируемые виды.
struct Empty;
struct Null;

// Обобщённый сущность от `T`.
trait DoubleDrop<T> {
    // Определим способ для вида вызывающего объекта,
    // который принимает один дополнительный параметр `T` и ничего с ним не делает.
    fn double_drop(self, _: T);
}

// Реализация `DoubleDrop<T>` для любого общего параметра `T` и
// вызывающего объекта `U`.
impl<T, U> DoubleDrop<T> for U {
    // Этот способ получает право владения на оба переданных переменной и
    // освобождает их.
    fn double_drop(self, _: T) {}
}

fn main() {
    let empty = Empty;
    let null  = Null;

    // Освободить `empty` и `null`.
    empty.double_drop(null);

    //empty;
    //null;
    // ^ TODO: Попробуйте убрать примечания эти строки.
}
```

### Смотрите также:

[`Drop`](https://doc.rust-lang.org/std/ops/trait.Drop.html), [`struct`](../custom_types/structs.md) и [`trait`](../trait.md)
