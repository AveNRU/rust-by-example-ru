# Повторители

Сущность [`Iterator`](https://doc.rust-lang.org/core/iter/trait.Iterator.html) используется для повтора
по собранием, таким как массивы.

Сущность требует определить способ `next`, для получения следующего элемента.
Данный способ в разделе `impl` может быть определён
вручную или самостоятельно (как в массивах и диапазонах).

Для удобства использования, например в круговороте `for`, некоторые собрания
превращаются в Повторители с помощью способа [`.into_iterator()`](https://doc.rust-lang.org/std/iter/trait.IntoIterator.html).

```rust,editable
struct Fibonacci {
    curr: u32,
    next: u32,
}

// Реализация `Iterator` для `Fibonacci`.
// Для реализации сущности `Iterator` требуется реализовать способ `next`.
impl Iterator for Fibonacci {
    type Item = u32;
    
    // Здесь мы определяем последовательность, используя `.curr` и `.next`.
    // Возвращаем вид `Option<T>`:
    //     * Когда в `Iterator` больше нет значений, будет возвращено `None`.
    //     * В противном случае следующее значение оборачивается в `Some` и возвращается.
    fn next(&mut self) -> Option<u32> {
        let new_next = self.curr + self.next;

        self.curr = self.next;
        self.next = new_next;

        // Поскольку последовательность Фибоначчи бесконечна,
        // то `Iterator` никогда не вернет `None`, и всегда будет
        // возвращаться `Some`.
        Some(self.curr)
    }
}

// Возвращается породитель последовательности Фибоначчи.
fn fibonacci() -> Fibonacci {
    Fibonacci { curr: 0, next: 1 }
}

fn main() {
    // `0..3` это `Iterator`, который создает : 0, 1, и 2.
    let mut sequence = 0..3;

    println!("Четыре подряд вызова `next`на 0..3");
    println!("> {:?}", sequence.next());
    println!("> {:?}", sequence.next());
    println!("> {:?}", sequence.next());
    println!("> {:?}", sequence.next());

    // `for` работает через `Iterator` пока тот не вернет `None`.
    // каждое значение `Some` распаковывается  и привязывается к переменной (здесь это `i`).
    println!("Повторение по 0..3 используя `for`");
    for i in 0..3 {
        println!("> {}", i);
    }

    // Способ `take(n)` уменьшает `Iterator` до его первых `n` членов.
    println!("Первые четыре члена последовательности Фибоначчи: ");
    for i in fibonacci().take(4) {
        println!("> {}", i);
    }

    // Способ `skip(n)` сокращает `Iterator`, отбрасывая его первые `n` членов.
    println!("Следующие четыре члена последовательности Фибоначчи: ");
    for i in fibonacci().skip(4).take(4) {
        println!("> {}", i);
    }

    let array = [1u32, 3, 3, 7];

    // Способ `iter` превращает `Iterator` в массив/срез.
    println!("Повторение по массиву {:?}", &array);
    for i in array.iter() {
        println!("> {}", i);
    }
}
```
