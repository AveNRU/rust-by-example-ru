# Ассоциированные виды

Использование "ассоциированных видов" улучшает общую 
читаемость рукописи через местное перемещение внутренних видов в 
сущность в качестве *выходных* видов. Правила написания для 
объявления `trait` будет следующим:

```rust
// `A` и `B` определены в сущности при помощи ключевого слова `type`.
// (Обратите внимание: в данном окружении `type` отличается `type`, который
// используется в псевдонимах).
trait Contains {
    type A;
    type B;

    // Обновлённый правила написания для обращения к этим двум ассоциированным видам.
    fn contains(&self, &Self::A, &Self::B) -> bool;
}
```

Обратите внимание, что функции, использующие `trait` `Contains` больше не требуют указания `A` или `B`:

```rust,ignore
// Без использования ассоциированных видов
fn difference<A, B, C>(container: &C) -> i32 where
    C: Contains<A, B> { ... }

// С использованием ассоциированных видов
fn difference<C: Contains>(container: &C) -> i32 { ... }
```

Давайте перепишем пример их предыдущего раздела с использованием ассоциированных видов:

```rust,editable
struct Container(i32, i32);

// Сущность, который проверяет, сохранены ли 2 элемента в дополнении.
// Также он может вернуть первое или последнее значение.
trait Contains {
    // Объявляем общие виды, которые будут использовать способы.
    type A;
    type B;

    fn contains(&self, _: &Self::A, _: &Self::B) -> bool;
    fn first(&self) -> i32;
    fn last(&self) -> i32;
}

impl Contains for Container {
    // Определяем, какими будут виды `A` и `B`. Если `входящий` вид
    // `Container(i32, i32)`, тогда `выходящие` виды определяются, как
    // `i32` и `i32`.
    type A = i32;
    type B = i32;

    // `&Self::A` и `&Self::B` также будут здесь уместны.
    fn contains(&self, number_1: &i32, number_2: &i32) -> bool {
        (&self.0 == number_1) && (&self.1 == number_2)
    }
    // Берём первую цифру.
    fn first(&self) -> i32 { self.0 }

    // Берём последнюю цифру.
    fn last(&self) -> i32 { self.1 }
}

fn difference<C: Contains>(container: &C) -> i32 {
    container.last() - container.first()
}

fn main() {
    let number_1 = 3;
    let number_2 = 10;

    let container = Container(number_1, number_2);

    println!("Содержатся ли в дополнении {} и {}: {}",
        &number_1, &number_2,
        container.contains(&number_1, &number_2));
    println!("Первое число: {}", container.first());
    println!("Последнее число: {}", container.last());

    println!("Разница: {}", difference(&container));
}
```
