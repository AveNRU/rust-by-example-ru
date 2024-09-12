# Повторение по `Result`

При работе способа `Iter::map` может случиться ошибка, например:

```rust,editable
fn main() {
    let strings = vec!["tofu", "93", "18"];
    let numbers: Vec<_> = strings
        .into_iter()
        .map(|s| s.parse::<i32>())
        .collect();
    println!("Итоги: {:?}", numbers);
}
```

Давайте рассмотрим стратегии обработки этого.

## Пренебрежение неудачными элементами с `filter_map()`

`filter_map` вызывает функцию и отфильтровывает итоги, вернувшие `None`.

```rust,editable
fn main() {
    let strings = vec!["tofu", "93", "18"];
    let numbers: Vec<_> = strings
        .into_iter()
        .map(|s| s.parse::<i32>())
        .filter_map(Result::ok)
        .collect();
    println!("Итоги: {:?}", numbers);
}
```

## Сбой всей действия с `collect()`

`Result` реализует `FromIter` так что вектор из итогов (`Vec<Result<T, E>>`)
может быть преобразован в итог с вектором (`Result<Vec<T>, E>`). Если будет найдена хотя бы одна `Result::Err`, повторение завершится.

```rust,editable
fn main() {
    let strings = vec!["tofu", "93", "18"];
    let numbers: Result<Vec<_>, _> = strings
        .into_iter()
        .map(|s| s.parse::<i32>())
        .collect();
    println!("Итоги: {:?}", numbers);
}
```

Та же самая техника может использоваться с `Option`.

## Сбор всех правильных значений и ошибок с помощью `partition()`

```rust,editable
fn main() {
    let strings = vec!["tofu", "93", "18"];
    let (numbers, errors): (Vec<_>, Vec<_>) = strings
        .into_iter()
        .map(|s| s.parse::<i32>())
        .partition(Result::is_ok);
    println!("Числа: {:?}", numbers);
    println!("Ошибки: {:?}", errors);
}
```

Если вы посмотрите на итоги работы, вы заметите, что они всё ещё обёрнуты в `Result`. Потребуется немного больше образцовой рукописи, чтобы получить нужный итог.

```rust,editable
fn main() {
    let strings = vec!["tofu", "93", "18"];
    let (numbers, errors): (Vec<_>, Vec<_>) = strings
        .into_iter()
        .map(|s| s.parse::<i32>())
        .partition(Result::is_ok);
    let numbers: Vec<_> = numbers.into_iter().map(Result::unwrap).collect();
    let errors: Vec<_> = errors.into_iter().map(Result::unwrap_err).collect();
    println!("Числа: {:?}", numbers);
    println!("Ошибки: {:?}", errors);
}
```
