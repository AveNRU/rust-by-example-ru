# Другие способы использования `?`

Вы обратили внимание, что сразу же после вызова 
`parse`, мы в `map_err` упаковали ошибку 
из библиотеки?

```rust,ignore
.and_then(|s| s.parse::<i32>()
    .map_err(|e| e.into())
```

Это простая и распространённая действие и было бы не плохо, 
если бы мы могли её опустить. Но из-за того, что 
`and_then` недостаточно гибок, мы не можем этого 
сделать. Однако, тут нам может помочь `?`.

Ранее `?` был рассмотрен как `unwrap` 
или `return Err(err)`. По большей части это правда: на 
самом деле `?` означает `unwrap` или 
`return Err(From::from(err))`. Поскольку 
`From::from` используется для преобразования между 
разными видами, применение `?` к ошибке 
по умолчанию преобразует её в возвращаемый вид (при условии, 
что исходная ошибка может быть в него преобразована).

Теперь мы перепишем наш предыдущий пример с использованием 
`?`. В итоге у нас пропал `map_err`, 
так как для нашего вида реализован `From::from`:

```rust,editable
use std::error;
use std::fmt;

// Создадим псевдоним с видом ошибки `Box<error::Error>`.
type Result<T> = std::result::Result<T, Box<error::Error>>;

#[derive(Debug)]
struct EmptyVec;

impl fmt::Display for EmptyVec {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "неверный первый элемент")
    }
}

impl error::Error for EmptyVec {
    fn description(&self) -> &str {
        "неверный первый элемент"
    }

    fn cause(&self) -> Option<&error::Error> {
        // Общая ошибка, основная причина не отслеживается.
        None
    }
}

// Такая же последовательность, как и раньше, но вместо объединения 
// всех `Result` и `Option`, мы используем `?` чтобы незамедлительно 
// получить внутреннее значение.
fn double_first(vec: Vec<&str>) -> Result<i32> {
    let first = vec.first().ok_or(EmptyVec)?;
    let parsed = first.parse::<i32>()?;
    Ok(2 * parsed)
}

fn print(result: Result<i32>) {
    match result {
        Ok(n)  => println!("Удвоенный первый элемент: {}", n),
        Err(e) => println!("Ошибка: {}", e),
    }
}

fn main() {
    let numbers = vec!["42", "93", "18"];
    let empty = vec![];
    let strings = vec!["tofu", "93", "18"];

    print(double_first(numbers));
    print(double_first(empty));
    print(double_first(strings));
}
```

Сейчас рукопись выглядит довольно чисто. По сравнению с 
`panic`, это похоже на замену вызова 
`unwrap` на `?` за исключением того, что 
возвращаемый вид будет `Result`. В итоге, он 
может быть обработан уровнем выше.

### Смотрите также:

[`From::from`](https://doc.rust-lang.org/std/convert/trait.From.html) и [`?`](https://doc.rust-lang.org/reference/expressions/operator-expr.html#the-question-mark-operator)
