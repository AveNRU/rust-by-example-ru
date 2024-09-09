# Сущности

Изложение времён жизни для способов сущностей в основном 
похоже на изложение в функциях. Обратите внимание, что 
`impl` также может иметь изложение времени жизни.

```rust,editable
// Стопка с изложенным временем жизни.
#[derive(Debug)]
 struct Borrowed<'a> {
     x: &'a i32,
 }

// Изложенное время жизни для реализации.
impl<'a> Default for Borrowed<'a> {
    fn default() -> Self {
        Self {
            x: &10,
        }
    }
}

fn main() {
    let b: Borrowed = Default::default();
    println!("b равно {:?}", b);
}
```

### Смотрите также:

[`trait`](../../trait.md)
