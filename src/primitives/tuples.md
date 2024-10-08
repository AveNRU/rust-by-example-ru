# Упорядоченные ряды

Упорядоченные ряды - собрание, которая хранит в себе переменные разных видов. Упорядоченные ряды
создаются с помощью круглых скобок `()`, и каждый упорядоченный ряд является переменной
с описанием видов `(T1, T2, ...)`, где `T1`, `T2` вид члена упорядоченного ряда.
Функции могут использовать упорядоченные ряды для возвращения нескольких значений,
так упорядоченные ряды могут хранить любое количество значений.

```rust,editable
// Упорядоченные ряды могут быть использованы как переменные функции
// и как возвращаемые значения
fn reverse(pair: (i32, bool)) -> (bool, i32) {
    // `let` можно использовать для создания связи между упорядоченным рядом и переменной
    let (integer, boolean) = pair;

    (boolean, integer)
}

// Это стопка используется для задания
#[derive(Debug)]
struct Matrix(f32, f32, f32, f32);

fn main() {
    // Упорядоченный ряд с множеством различных видов данных
    let long_tuple = (1u8, 2u16, 3u32, 4u64,
                      -1i8, -2i16, -3i32, -4i64,
                      0.1f32, 0.2f64,
                      'a', true);

    // К значениям переменных внутри упорядоченного ряда можно обратиться по порядковому указателю
    println!("первое значение длинного упорядоченного ряда: {}", long_tuple.0);
    println!("второе значение длинного упорядоченного ряда: {}", long_tuple.1);

    // Упорядоченные ряды могут содержать в себе упорядоченные ряды
    let tuple_of_tuples = ((1u8, 2u16, 2u32), (4u64, -1i8), -2i16);

    // Упорядоченные ряды можно навыводить
    println!("упорядоченный ряд из упорядоченных рядов: {:?}", tuple_of_tuples);
    
    // Но длинные Упорядоченные ряды не могут быть выведены
    // let too_long_tuple = (1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13);
    // println!("слишком длинный кортеж: {:?}", too_long_tuple);
    // TODO ^ Снимите примечание выше 2 строки, чтобы увидеть ошибку сборщика

    let pair = (1, true);
    println!("pair хранит в себе {:?}", pair);

    println!("перевёрнутая pair будет {:?}", reverse(pair));

    // Для создания упорядоченного ряда, содержащего один элемент, необходимо написать элемент и
    // поставить запятую внутри круглых скобок.
    println!("упорядоченный ряд из одного элемента: {:?}", (5u32,));
    println!("просто целочисленное значение: {:?}", (5u32));

    // Упорядоченные ряды можно разобрать на части (разбирать) для создания связи
    let tuple = (1, "привет", 4.5, true);

    let (a, b, c, d) = tuple;
    println!("{:?}, {:?}, {:?}, {:?}", a, b, c, d);

    let matrix = Matrix(1.1, 1.2, 2.1, 2.2);
    println!("{:?}", matrix);

}
```

### Задание

 1. *Повторение*: Добавьте реализацию сущности `fmt::Display` для `стопки`
    Matrix в примерах выше,
    чтобы, когда вы измените вид вывода с `{:?}` на `{}`
    на окно вывода вывелось:

    ```text
    ( 1.1 1.2 )
    ( 2.1 2.2 )
    ```

    Вы можете вернуться на пример [print display][print_display].
 2. Добавьте функцию `transpose`, используя функцию `reverse`, как пример, которая принимает
    матрицу, как переменная и возвращает матрицу, в которой два элемента поменялись местами.
    Например:

    ```rust,ignore
    println!("Matrix:\n{}", matrix);
    println!("Transpose:\n{}", transpose(matrix));
    ```

    Итог:

    ```text
    Matrix:
    ( 1.1 1.2 )
    ( 2.1 2.2 )
    Transpose:
    ( 1.1 2.1 )
    ( 1.2 2.2 )
    ```

[print_display]: hello/print/print_display.html
