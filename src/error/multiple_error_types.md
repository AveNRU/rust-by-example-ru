# Несколько видов ошибок

Предыдущие примеры всегда были очень удобны:  `Result` взаимодействовали с другими `Result`, а `Option` - с другими `Option`.

Иногда `Option` необходимо взаимодействовать с 
`Result`, или `Result<T, Error1>` с 
`Result<T, Error2>`. В этих случаях, нам нужно 
управлять этими разными видами ошибок таким образом, чтобы 
можно было их компоновать и легко взаимодействовать с ними.

В следующем рукописи, два исхода `unwrap` 
создает разные виды ошибок. `Vec::first` 
возвращает `Option`, в то время как 
`parse::<i32>` возвращает 
`Result<i32, ParseIntError>`:

```rust,editable,ignore,mdbook-runnable
fn double_first(vec: Vec<&str>) -> i32 {
    let first = vec.first().unwrap(); // Создает ошибку 1
    2 * first.parse::<i32>().unwrap() // Создает ошибку 2
}

fn main() {
    let numbers = vec!["42", "93", "18"];
    let empty = vec![];
    let strings = vec!["tofu", "93", "18"];

    println!("Первое удвоенное {}", double_first(numbers));

    println!("Первое удвоенное {}", double_first(empty));
    // Ошибка 1: входной вектор пустой

    println!("Первое удвоенное {}", double_first(strings));
    // Ошибка 2: элемент не может быть преобразован в число
}
```

В следующих главах мы рассмотрим различные стратегии обработки этих видов неполадок.
