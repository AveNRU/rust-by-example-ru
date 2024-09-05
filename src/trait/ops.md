# Перегрузка приказчиков

В Rust, множество приказчиков могут быть перегружены с помощью сущностей. То есть, некоторые
приказчики могут использоваться для выполнения различных задач на основе вводимых переменных.
Это возможно, потому что приказчики являются синтаксическим сахаром для вызова способов. Например,
приказчик `+` в `a + b` вызывает способ `add` (как в `a.add(b)`).
Способ `add` является частью сущности `Add`.
Следовательно, приказчик `+` могут использовать все, кто реализуют сущность `Add`.

Список сущностей, таких как `Add`, которые перегружают приказчики, доступен [здесь](https://doc.rust-lang.org/core/ops/).

```rust,editable
use std::ops;

struct Foo;
struct Bar;

#[derive(Debug)]
struct FooBar;

#[derive(Debug)]
struct BarFoo;

// Сущность `std::ops::Add` используется для указания возможности `+`.
// Здесь мы объявим `Add<Bar>` - сущность сложения, со вторым
// операндом вида `Bar`.
// Следующий раздел реализует операцию: Foo + Bar = FooBar
impl ops::Add<Bar> for Foo {
    type Output = FooBar;

    fn add(self, _rhs: Bar) -> FooBar {
        println!("> Вызвали Foo.add(Bar)");

        FooBar
    }
}

// Если мы поменяем местами виды, то получим реализацию некоммутативного сложения.
// Здесь мы объявим `Add<Foo>` - сущность сложения, со вторым
// операндом вида `Foo`.
// Этот раздел реализует операцию: Bar + Foo = BarFoo
impl ops::Add<Foo> for Bar {
    type Output = BarFoo;

    fn add(self, _rhs: Foo) -> BarFoo {
        println!("> Вызвали Bar.add(Foo)");

        BarFoo
    }
}

fn main() {
    println!("Foo + Bar = {:?}", Foo + Bar);
    println!("Bar + Foo = {:?}", Bar + Foo);
}
```

### Смотрите также:

[Add](https://doc.rust-lang.org/core/ops/trait.Add.html), [Syntax Index](https://doc.rust-lang.org/book/second-edition/appendix-02-operators.html)
