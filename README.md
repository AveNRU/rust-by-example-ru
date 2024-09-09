# Ржачина на примерах

[![Build Status][travis-image]][travis-link]

Изучение Ржачины на примерах (Рукопись доступна для изменения)

## От переводчиков

Перевод `Ржачина by examples` находится в процессе выполнения. За ходом перевода можно наблюдать [тут](https://github.com/ruRust/rust-by-example-ru/issues/1).

Отдельное спасибо @suhr и @AKhranovskiy. Перевод основан на их работе.

Будем рады помощи.

Проверка правописания: [Яндекс.Спеллер][yaspeller]

## Что это за проект?

Это исходный рукопись сайта [Ржачина by Example][website], переведённый на русский язык! Перевод можно найти по адресу https://rurust.github.io/rust-by-example-ru

## Как помочь проекту?

Смотри [CONTRIBUTING.md](CONTRIBUTING.md).

## Использование

Для сборки местной копии книги Вам нужно [установить сборщик Rust][install Rust]
и выполнить следующие приказы:

```bash
$ git clone https://github.com/ruRust/rust-by-example-ru
$ cd rust-by-example-ru
$ cargo install mdbook
$ mdbook build
$ mdbook serve
```

[install Rust]: https://www.rust-lang.org/ru-RU/install.html

Для запуска примеров приведенных в книге Вам необходимо подключение к сети интернет;
Однако вы можете читать все содержимое без подключения к сети интернет (автономно)!

## Перевод на другие языки

* [Chinese](https://github.com/rust-lang-cn/rust-by-example-cn)
* [Japanese](https://github.com/rust-lang-ja/rust-by-example-ja)
* [French](https://github.com/Songbird0/FR_RBE)

## Лицензия

`Ржачина на примерах` распространяется по двойной лицензии - лицензия Apache 2.0 и лицензия MIT.
Более подробную сведения можно найти в файлах LICENSE-APACHE и LICENSE-MIT соответственно.

 * Apache License, Version 2.0, ([LICENSE-APACHE](LICENSE-APACHE) or
   http://www.apache.org/licenses/LICENSE-2.0)
 * MIT license ([LICENSE-MIT](LICENSE-MIT) or
   http://opensource.org/licenses/MIT)

Если вы явно не указываете иное, любой вклад преднамеренно представлен
для включения в `Ржачина на примерах`, как определено в лицензии Apache-2.0, необходима
двойная лицензия, как указано выше, без каких-либо дополнительных условий.

[travis-image]: https://travis-ci.org/ruRust/rust-by-example-ru.svg?branch=master
[travis-link]: https://travis-ci.org/ruRust/rust-by-example-ru
[yaspeller]: https://tech.yandex.ru/speller/
