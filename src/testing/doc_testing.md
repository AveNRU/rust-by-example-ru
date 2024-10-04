# Проверка пособия

Основной способ документирования проекта на Ржавчина - это 
изложение исходной рукописи. Документационные примечания 
пишутся с использованием [markdown](https://daringfireball.net/projects/markdown/) и позволяют 
использовать внутри разделы рукописи. Ржавчина заботится о правильности, так 
что эти разделы рукописи могут собираться и использоваться в 
качестве проверок.

```rust,ignore
/// Первая строка - это краткое описание функции.
///
/// Следующие строки представляют детальную пособие. Разделы рукописи /// начинаются трёх обратных кавычек и внутри содержат неявные
/// `fn main()` и `extern crate <cratename>`. Предположим, мы
/// проверяем крейт `doccomments`:
///
/// ```
/// let result = doccomments::add(2, 3);
/// assert_eq!(result, 5);
/// ```
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}

/// Ообычно документационные примечания могут содержат разделе "Examples", "Panics" and "Failures".
///
/// Следующая функция делит два числа.
///
/// # Examples
///
/// ```
/// let result = doccomments::div(10, 2);
/// assert_eq!(result, 5);
/// ```
///
/// # Panics
///
/// Функция вызывает сбой, если второй переменная равен нулю.
///
/// ```rust,should_panic
/// // вызывает сбой при делении на 0
/// doccomments::div(10, 0);
/// ```
pub fn div(a: i32, b: i32) -> i32 {
    if b == 0 {
        panic!("Ошибка деления на 0");
    }

    a / b
}
```

Проверки могут быть запущены при помощи `cargo test`:

```shell
$ cargo test
running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

   Doc-tests doccomments

running 3 tests
test src/lib.rs - add (line 7) ... ok
test src/lib.rs - div (line 21) ... ok
test src/lib.rs - div (line 31) ... ok

test result: ok. 3 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

## Мотивация для документационных проверок

Главная цель документационных проверок - служить примерами 
предоставляемой возможности, что является одной из самых 
важных [советов](https://rust-lang-nursery.github.io/api-guidelines/documentation.html#examples-use--not-try-not-unwrap-c-question-mark). Это позволяет использовать примеры из пособия в качестве полных отрывков рукописи. Но использование `?` приведёт к ошибке сборки, так как функция `main` возвращает `()` 
(`unit`). На помощь приходит возможность скрыть из пособия 
некоторые строки исходной рукописи: можно написать 
`fn try_main() -> Result<(), ErrorType>`, скрыть 
её и вызвать её в скрытом `main` с 
`unwrap`. Звучит сложно? Вот пример:

```rust,ignore
/// Использование скрытой `try_main` в документационных проверках.
///
/// ```
/// # // скрытые строки начинаются с знака `#`, но они всё ещё могут собраться!
/// # fn try_main() -> Result<(), String> { // эта линия оборачивает тело функции, которое отображается в пособии
/// let res = try::try_div(10, 2)?;
/// # Ok(()) // возвращается из try_main
/// # }
/// # fn main() { // начало `main` которая выполняет `unwrap()`
/// #    try_main().unwrap(); // вызов `try_main` и извлечение итога
/// #                         // так что в случае ошибки этот проверка вызывает сбой
/// # }
pub fn try_div(a: i32, b: i32) -> Result<i32, String> {
    if b == 0 {
        Err(String::from("Деление на 0"))
    } else {
        Ok(a / b)
    }
}
```

## Смотрите также:

- [RFC505](https://github.com/rust-lang/rfcs/blob/master/text/0505-api-comment-conventions.md) по исполнению пособия
- [Советы для API](https://rust-lang-nursery.github.io/api-guidelines/documentation.html) по документационному проверке
