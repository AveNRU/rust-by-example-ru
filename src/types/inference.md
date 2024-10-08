# Вывод видов

Движок вывода видов весьма умён. Он делает куда больше,
чем просто смотрит на вид [r-value][rvalue] при объявления.
Он также смотрит, как используется значение после объявления, чтобы
вывести его вид. Вот расширенный пример вывода видов:

```rust,editable
fn main() {
    // Благодаря выведению видов сборщик знает, `elem` имеет вид - u8.
    let elem = 5u8;

    // Создадим пустой вектор (расширяемый массив).
    let mut vec = Vec::new();
    // В данном месте сборщик не знает точный вид `vec`, он лишь знает,
    // что это вектор чего-то там (`Vec<_>`).

    // Добавляем `elem` в вектор.
    vec.push(elem);
    // Ага! Теперь сборщик знает, что `vec` - это вектор, который хранит в себе вид `u8`
    // (`Vec<u8>`)
    // ЗАДАНИЕ ^ Попробуйте накрыть примечаниями строку `vec.push(elem)`

    println!("{:?}", vec);
}
```

Не потребовалось никакого изложения видов переменных, сборщик счастлив, как и программист!

[rvalue]: https://en.wikipedia.org/wiki/Value_%28computer_science%29#lrvalue
