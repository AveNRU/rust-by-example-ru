# Дочерние процессы

Структура `process::Output` представляет итог завершённого дочернего процесса,
и структура `process::Command` - это строитель процесса.

```rust,editable,ignore
use std::process::Command;

fn main() {
    let output = Command::new("rustc")
        .arg("--version")
        .output().unwrap_or_else(|e| {
            panic!("Ошибка выполнения процесса {}", e)
    });

    if output.status.success() {
        let s = String::from_utf8_lossy(&output.stdout);

        print!("rustc завершился успешно и вывел в stdout:\n{}", s);
    } else {
        let s = String::from_utf8_lossy(&output.stderr);

        print!("rustc завершился с ошибкой и вывел в stderr:\n{}", s);
    }
}
```

(Советуется попробовать предыдущий пример с неправильным флагом обращения к `rustc`)
