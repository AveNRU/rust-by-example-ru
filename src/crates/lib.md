# Библиотеки

Давайте создадим библиотеку и посмотрим, как связать её с другим дополнением.

```rust,ignore
pub fn public_function() {
    println!("вызвана `public_function()` библиотеки rary");
}

fn private_function() {
    println!("вызвана `private_function()` библиотеки rary");
}

pub fn indirect_access() {
    print!("вызвана `indirect_access()` библиотеки rary, и в ней\n> ");

    private_function();
}
```

```bash
$ rustc --crate-type=lib rary.rs
$ ls lib*
library.rlib
```

Библиотеки получают приставка «lib», и по умолчанию имеют то же имя,
что и их дополнения, но это имя можно изменить
с помощью [свойства `crate_name`](attribute/crate.html).
