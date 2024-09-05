# Интеграционное проверка

[Модульные проверки](unit_testing.md) проверяют по одному модулю изолированно: они малы
и могут проверить не публичный рукопись. Интеграционные проверки являются внешними для вашего пакета и используют
только его открытый внешняя оболочка, таким же образом, как и любой другой рукопись. Их цель в том, чтобы проверить, что многие части вашей библиотеки работают правильно вместе.

Cargo ищет интеграционные проверки в каталоге `tests` после каталога `src`.

Файл `src/lib.rs`:

```rust,ignore
// Предположим, что наш пакет называется `adder`, для проверки он будет внешним рукописью.
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

Файл с проверкой: `tests/integration_test.rs`:

```rust,ignore
// мы проверяем extern crate, как и любой другой рукопись.
extern crate adder;

#[test]
fn test_add() {
    assert_eq!(adder::add(3, 2), 5);
}
```

Запустить проверки можно приказом `cargo test`:

```shell
$ cargo test
running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

     Running target/debug/deps/integration_test-bcd60824f5fbfe19

running 1 test
test test_add ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out

   Doc-tests adder

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

Каждый файл с исходным рукописью в директории `tests` собирается в отдельный пакет. 
Один из путей использовать некоторый общий рукопись между интеграционными проверками - создать модуль с открытыми функциями и импортировать их в проверках.

Файл `tests/common.rs`:

```rust,ignore
pub fn setup() {
    // некоторый рукопись для настройки, создание необходимых файлов/каталогов, запуск серверов.
}
```

Файл с проверкой: `tests/integration_test.rs`

```rust,ignore
// мы проверяем extern crate, как и любой другой рукопись.
extern crate adder;

// импорт общего модуля.
mod common;

#[test]
fn test_add() {
    // использование общего рукописи.
    common::setup();
    assert_eq!(adder::add(3, 2), 5);
}
```

Модули с общим рукописью следуют обычным правилам  [модулей](../mod.md). Общий модуль можно создать как `tests/common/mod.rs`.
