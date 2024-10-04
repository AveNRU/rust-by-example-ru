# Проверка

Как мы знаем, проверка является неотъемлемой частью любого программного обеспечения! Ржавчина имеет первоклассную поддержку разделного и встроенных проверок (см.[главу о проверке в TRPL](https://doc.rust-lang.org/book/ch11-00-testing.html)).

Из разделов проверок, приведённых выше, мы знаем, как писать разделные и встроенные проверки. Организационно, мы можем расположить разделные проверки в разделах, которые они проверяют, а встроенные - в собственной папке `tests/`:

```txt
foo
├── Cargo.toml
├── src
│   └── main.rs
└── tests
    ├── my_test.rs
    └── my_other_test.rs
```

Каждый файл в папке `tests` - это отдельная встроенная проверка.

`cargo`  естественно, обеспечивает простой способ запуска всех ваших проверок!

```shell
$ cargo test
```

Вы должны увидеть примерно такой итог:

```shell
$ cargo test
   Compiling blah v0.1.0 (file:///nobackup/blah)
    Finished dev [unoptimized + debuginfo] target(s) in 0.89 secs
     Running target/debug/deps/blah-d3b32b97275ec472

running 3 tests
test test_bar ... ok
test test_baz ... ok
test test_foo_bar ... ok
test test_foo ... ok

test result: ok. 3 passed; 0 failed; 0 ignored; 0 measured; 0 filtered out
```

Вы также можете запустить проверки, чьё имя соответствует образцу:

```shell
$ cargo test test_foo
```

```shell
$ cargo test test_foo
   Compiling blah v0.1.0 (file:///nobackup/blah)
    Finished dev [unoptimized + debuginfo] target(s) in 0.35 secs
     Running target/debug/deps/blah-d3b32b97275ec472

running 2 tests
test test_foo ... ok
test test_foo_bar ... ok

test result: ok. 2 passed; 0 failed; 0 ignored; 0 measured; 2 filtered out
```

Одно слово предостережения: Cargo может выполнять несколько проверок одновременно, поэтому убедитесь, что они не участвуют в гонках друг с другом. Например, если они все выводят в файл, вы должны заставить их записывать в разные файлы.
