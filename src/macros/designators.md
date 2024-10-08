# Указатели

переменные макроса имеют приставка знака доллара `$` и вид аннотируется
с помощью *указателей отрывка*:

```rust,editable
macro_rules! create_function {
    // Этот макрос принимает переменная определителя `ident` и
    // создаёт функцию с именем `$func_name`.
    // Определитель `ident` используют для обозначения имени переменной/функции.
    ($func_name:ident) => (
        fn $func_name() {
            // Макрос `stringify!` преобразует `ident` в строку.
            println!("Вызвана функция {:?}()",
                     stringify!($func_name))
        }
    )
}

// Создадим функции с именами `foo` и `bar` используя макрос, указанный выше.
create_function!(foo);
create_function!(bar);

macro_rules! print_result {
    // Этот макрос принимает выражение вида `expr` и навыводит
    // его как строку вместе с итогом.
    // Указатель `expr` используют для обозначения выражений.
    ($expression:expr) => (
        // `stringify!` преобразует выражение в строку *без изменений*.
        println!("{:?} = {:?}",
                 stringify!($expression),
                 $expression);
    )
}

fn main() {
    foo();
    bar();

    print_result!(1u32 + 1);

    // Напомним, что разделы тоже являются выражениями!
    print_result!({
        let x = 1u32;

        x * x + 2 * x - 1
    });
}
```

Это список всех указателей:

- `block`
- `expr` используют для обозначения выражений
- `ident` используют для обозначения имени переменной/функции
- `item`
- `literal` используется для записных постоянных переменных
- `pat` (*образец*)
- `path`
- `stmt` (*единственный приказчик*)
- `tt` (*единственное дерево лексем*)
- `ty` (*вид*)
- `vis` (*спецификатор видимости*)

Полный список указателей, вы можете увидеть в [Ржавчина Reference](https://doc.rust-lang.org/reference/macros-by-example.html).
