# Unit-проверка

Проверки - это функции на Ржавчине, которые проверяют, что проверяемый 
рукопись работает ожидаемым образом. Тело проверокых функций обычно 
выполняет некоторую настройку, запускает рукопись[,] который мы 
проверяем, и затем сравнивает полученный итог с тем, что мы 
ожидаем.

Большинство модульных проверок располагается в [модуле](../mod.md) 
`tests`, помеченном [свойством](../attribute.md) 
`#[cfg(test)]`. Проверокые функции помечаются 
свойством `#[test]`.

Проверки заканчиваются неудачей, когда что-либо в проверокой функции 
вызывает [сбой](../std/panic.md). Есть несколько вспомогательных 
[макросов](../macros.md):

- `assert!(expression)` - вызывает сбой, если итог выражения равен `false`.
- `assert_eq!(left, right)` и `assert_ne!(left, right)` - сравнивает левое и правое выражения на равенство и неравенство соответственно.

```rust,ignore
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}

// Это действительно плохая функция сложения, её назначение в данном // примере - потерпеть неудачу.
#[allow(dead_code)]
fn bad_add(a: i32, b: i32) -> i32 {
    a - b
}

#[cfg(test)]
mod tests {
    // Обратите внимание на эту полезную идиому: импортирование имён из внешней (для mod - проверок) области видимости.
    use super::*;

    #[test]
    fn test_add() {
        assert_eq!(add(1, 2), 3);
    }

    #[test]
    fn test_bad_add() {
        // Это утверждение запустится и проверка не сработает.
        // Заметьте, что закрытые функции также могут быть протестированы!
        assert_eq!(bad_add(1, 2), 3);
    }
}
```

Проверки могут быть запущены при помощи приказы `cargo test`.

```shell
$ cargo test

running 2 tests
test tests::test_bad_add ... FAILED
test tests::test_add ... ok

failures:

---- tests::test_bad_add stdout ----
        thread 'tests::test_bad_add' panicked at 'assertion failed: `(left == right)`
  left: `-1`,
 right: `3`', src/lib.rs:21:8
note: Run with `RUST_BACKTRACE=1` for a backtrace.


failures:
    tests::test_bad_add

test result: FAILED. 1 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out
```

## Проверка сбоя

Для проверок функций, которые должны вызвать сбой при 
определённых обстоятельствах, используется свойство 
`#[should_panic]`. Этот свойство принимает 
необязательный параметр `expected =` с писанием 
сообщения о панике. Если ваша функция может вызвать сбой в 
разных случаях, то этот параметр поможет вам быть уверенным, 
что вы проверяете именно тот сбой, который собирались.

```rust,ignore
pub fn divide_non_zero_result(a: u32, b: u32) -> u32 {
    if b == 0 {
        panic!("Divide-by-zero error");
    } else if a < b {
        panic!("Divide result is zero");
    }
    a / b
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_divide() {
        assert_eq!(divide_non_zero_result(10, 2), 5);
    }

    #[test]
    #[should_panic]
    fn test_any_panic() {
        divide_non_zero_result(1, 0);
    }

    #[test]
    #[should_panic(expected = "Divide result is zero")]
    fn test_specific_panic() {
        divide_non_zero_result(1, 10);
    }
}
```

Запуск этих проверок даст следующее:

```shell
$ cargo test

running 3 tests
test tests::test_any_panic ... ok
test tests::test_divide ... ok
test tests::test_specific_panic ... ok

test result: ok. 3 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

   Doc-tests tmp-test-should-panic

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

## Запуск определенных проверок

Для запуска определенного проверки надо добавить имя проверки в приказ 
`cargo test`.

```shell
$ cargo test test_any_panic
running 1 test
test tests::test_any_panic ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 2 filtered out

   Doc-tests tmp-test-should-panic

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

Для запуска нескольких проверок, можно указать часть имени, 
которая есть во всех необходимых проверках.

```shell
$ cargo test panic
running 2 tests
test tests::test_any_panic ... ok
test tests::test_specific_panic ... ok

test result: ok. 2 passed; 0 failed; 0 ignored; 0 measured; 1 filtered out

   Doc-tests tmp-test-should-panic

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

## Пренебрежение проверками

Проверки могут быть помечены свойством `#[ignore]`, чтобы они были исключены из списка запускаемых приказом `cargo test`. Такие проверки можно запустить с помощью приказы `cargo test -- --ignored`.

```rust
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_add() {
        assert_eq!(add(2, 2), 4);
    }

    #[test]
    fn test_add_hundred() {
        assert_eq!(add(100, 2), 102);
        assert_eq!(add(2, 100), 102);
    }

    #[test]
    #[ignore]
    fn ignored_test() {
        assert_eq!(add(0, 0), 0);
    }
}
```

```shell
$ cargo test
running 3 tests
test tests::ignored_test ... ignored
test tests::test_add ... ok
test tests::test_add_hundred ... ok

test result: ok. 2 passed; 0 failed; 1 ignored; 0 measured; 0 filtered out

   Doc-tests tmp-ignore

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

$ cargo test -- --ignored
running 1 test
test tests::ignored_test ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

   Doc-tests tmp-ignore

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```
