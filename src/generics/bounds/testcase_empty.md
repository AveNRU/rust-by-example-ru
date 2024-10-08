# Пример: пустые ограничения

Следствием того, как работают ограничения по сущности, является то,
что даже если `сущность` не включает в себя какие-либо функциональные
возможности, вы все равно можете использовать его в качестве ограничения.
Примерами таких сущностей являются `Eq` и `Ord` из встроенной библиотеки.

```rust,editable
struct Cardinal;
struct BlueJay;
struct Turkey;

trait Red {}
trait Blue {}

impl Red for Cardinal {}
impl Blue for BlueJay {}

// Эти функции действительны только для видов реализующих эти сущности.
// То, что сущности пусты, не имеет значения.
fn red<T: Red>(_: &T)   -> &'static str { "красная" }
fn blue<T: Blue>(_: &T) -> &'static str { "синяя" }

fn main() {
    let cardinal = Cardinal;
    let blue_jay = BlueJay;
    let _turkey   = Turkey;

    // `red()` не будет работать для blue_jay, ни наоборот,
    // из-за ограничений по сущности.
    println!("Кардинал {} птица", red(&cardinal));
    println!("Голубая сойка {} птица", blue(&blue_jay));
    //println!("Индюк {} птица", red(&_turkey));
    // ^ TODO: Попробуйте убрать примечания эту строку.
}
```

### Смотрите также:

[`std::cmp::Eq`](https://doc.rust-lang.org/std/cmp/trait.Eq.html), [`std::cmp::Ord`](https://doc.rust-lang.org/std/cmp/trait.Ord.html) и [`trait`](../../trait.md)
