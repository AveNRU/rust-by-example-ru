# Дополнения

Свойство `crate_type` используется, чтобы сказать сборщику,
какой дополнение является библиотекой (и каким видом библиотеки),
а какой исполняемым файлом. Свойство `crate_name` используется для указания имени дополнения.

Однако важно отметить, что свойства `crate_type` и `create_name` **не имеют значения** при использовании пакетного управленца Cargo.
В виду того, что Cargo используется для большинства проектов на Rust,
это значит в реальном мире использование `crate_type` и `crate_name`
достаточно ограничено.

```rust,editable
// Это дополнение - библиотека
#![crate_type = "lib"]
// Эта библиотека называется "rary"
#![crate_name = "rary"]

pub fn public_function() {
    println!("вызвана `public_function()` библиотеки `rary`");
}

fn private_function() {
    println!("вызвана `private_function()` библиотеки `rary`");
}

pub fn indirect_access() {
    print!("вызвана `indirect_access()` библиотеки `rary`, и в ней\n> ");

    private_function();
}
```

Если мы используем свойство `crate_type`,
то нам больше нет необходимости передавать флаг `--crate-type` сборщику.

```bash
$ rustc lib.rs
$ ls lib*
library.rlib
```
