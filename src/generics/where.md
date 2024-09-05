# Утверждения where

Ограничение сущности также может быть выражено с помощью утверждения `where`
непосредственно перед открытием `{`, а не при первом упоминании вида.
Кроме того, утверждения `where` могут применять ограничения сущностей к 
произвольным видам, а не только к параметрам вида.

В некоторых случаях утверждение `where` является полезным:

* При указании обобщённых видов и ограничений сущностей отдельно,
рукопись становится более ясным:

```rust,ignore
impl <A: TraitB + TraitC, D: TraitE + TraitF> MyTrait<A, D> for YourType {}

// Выражение ограничений сущностей через утверждение `where`
impl <A, D> MyTrait<A, D> for YourType where
    A: TraitB + TraitC,
    D: TraitE + TraitF {}
```

* Использование утверждения `where` более выразительно, чем использование
обычного правил написания. В этом примере `impl` не может быть непосредственно
выражен без утверждения `where`:

```rust,editable
use std::fmt::Debug;

trait PrintInOption {
    fn print_in_option(self);
}

// Потому что в противном случае мы должны были бы выразить это как
// `T: Debug` или использовать другой способ косвенного подхода,
// для этого требуется утверждение `where`:
impl<T> PrintInOption for T where
    Option<T>: Debug {
    // Мы хотим использовать `Option<T>: Debug` как наше ограничение
    // сущности, потому то это то, что будет выведено. В противном случае
    // использовалось бы неправильное ограничение сущности.
    fn print_in_option(self) {
        println!("{:?}", Some(self));
    }
}

fn main() {
    let vec = vec![1, 2, 3];

    vec.print_in_option();
}
```

### Смотрите также:

[RFC][where], [`структуры`][struct], и [`сущности`][trait]

[struct]: custom_types/structs.html
[trait]: trait.html
[where]: https://github.com/rust-lang/rfcs/blob/master/text/0135-where.md
