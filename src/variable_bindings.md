# Связывание переменных

Ржавчина предоставляет безопасность видов с помощью статической видизации.
Вид переменной может быть указан при объявление связи с переменной.
Тем не менее, в большинстве случаев, сборщик сможет определить вид переменной из окружения.

Значения (как и записи) могут быть привязаны к переменным, используя приказчик `let`.

```rust,editable
fn main() {
    let an_integer = 1u32;
    let a_boolean = true;
    let unit = ();

    // скопировать значение `an_integer` в `copied_integer`
    let copied_integer = an_integer;

    println!("Целое: {:?}", copied_integer);
    println!("Разумное: {:?}", a_boolean);
    println!("Встречайте единичное значение: {:?}", unit);

    // Сборщик предупреждает о неиспользуемых переменных; эти предупреждения можно
    // отключить используя подчёркивание перед именем переменной
    let _unused_variable = 3u32;
    let noisy_unused_variable = 2u32;
    // ИСПРАВЬТЕ ^ Добавьте подчёркивание
}
```