# `extern crate`

Чтобы связать дополнение с новой библиотекой, нужна объявление `extern crate`.
Она не только свяжет библиотеку, но и импортирует все элементы в раздел
с тем же именем, что и сама библиотека.
Правила видимости, применимые к разделам, так же применимы и к библиотекам.

```rust,ignore
// Ссылка на `library`. Импортируем элементы, как раздел `rary`
extern crate rary;

fn main() {
    rary::public_function();

    // Ошибка! Функция `private_function` закрытая
    //rary::private_function();

    rary::indirect_access();
}
```

```bash
# Где library.rlib путь к ссобранной библиотеке. Предположим, что
# она находится в той же директории:
$ rustc executable.rs --extern rary=library.rlib && ./executable
вызвана `public_function()` библиотеки rary
вызвана `indirect_access()` библиотеки rary, и в ней
> вызвана `private_function()` библиотеки rary
```
