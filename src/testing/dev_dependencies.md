# `dev-dependencies`

Иногда возникает необходимость иметь зависимости только для проверок (примеры, бенчмарки). Такие зависимости добавляются в `Cargo.toml` в секцию
`[dev-dependencies]`. Эти зависимости не распространяются как зависимости на другие пакеты, которые зависят от этого пакета.

Одним из таких примеров является пакет расширяющий обычный макрос `assert!`. Файл `Cargo.toml`:

```ignore
# При обычной сборке проекта данная зависимость не будет использоваться.
[dev-dependencies]
pretty_assertions = "0.4.0"
```

Файл `src/lib.rs`:

```rust,ignore
// внешний пакет используется только для проверок
#[cfg(test)]
#[macro_use]
extern crate pretty_assertions;

pub fn add(a: i32, b: i32) -> i32 {
    a + b
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_add() {
        assert_eq!(add(2, 3), 5);
    }
}
```

## Смотрите также:

Пособие [Cargo](http://doc.crates.io/specifying-dependencies.html) по указанию зависимостей.
