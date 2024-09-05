# Разбор переменных

Сопоставление может быть использовано для разбора простых переменных:

```rust,editable
use std::env;

fn increase(number: i32) {
    println!("{}", number + 1);
}

fn decrease(number: i32) {
    println!("{}", number - 1);
}

fn help() {
    println!("usage:
match_args <string>
    Проверяет является ли данная строка ответом.
match_args {{increase|decrease}} <integer>
    Увеличивает или уменьшает число на 1.");
}

fn main() {
    let args: Vec<String> = env::args().collect();

    match args.len() {
        // переменные не переданы
        1 => {
            println!("Я - 'match_args'. Попробуйте передать переменные!");
        },
        // передан один переменная
        2 => {
            match args[1].parse() {
                Ok(42) => println!("Это ответ!"),
                _ => println!("Это не ответ."),
            }
        },
        // переданы одна приказ и один переменная
        3 => {
            let cmd = &args[1];
            let num = &args[2];
            // parse the number
            let number: i32 = match num.parse() {
                Ok(n) => {
                    n
                },
                Err(_) => {
                    eprintln!("ошибка: второй переменная не является числом");
                    help();
                    return;
                },
            };
            // Разбираем приказ
            match &cmd[..] {
                "increase" => increase(number),
                "decrease" => decrease(number),
                _ => {
                    eprintln!("ошибка: неизвестная приказ");
                    help();
                },
            }
        },
        // все остальные случаи
        _ => {
            // показываем сообщение с помощью
            help();
        }
    }
}
```

```shell
$ ./match_args Rust
Это не ответ.
$ ./match_args 42
Это ответ!
$ ./match_args do something
ошибка: второй переменная не является числом
usage:
match_args <string>
    Проверяет является ли данная строка ответом.
match_args {increase|decrease} <integer>
    Увеличивает или уменьшает число на 1.
$ ./match_args do 42
ошибка: неизвестная приказ
usage:
match_args <string>
    Проверяет является ли данная строка ответом.
match_args {increase|decrease} <integer>
    Увеличивает или уменьшает число на 1.
$ ./match_args increase 42
43
```
