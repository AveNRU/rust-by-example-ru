# Область видимости и затенение

Связывание переменных имеет местную область видимости, и живут эти переменные в *разделе*.
Раздел — набор инструкций, заключённый между узорчатыми скобками `{}`.
Кроме того, допускается [затенение переменных](https://en.wikipedia.org/wiki/Variable_shadowing).

```rust,editable,ignore,mdbook-runnable
fn main() {
    // Эта переменная живёт в функции main
    let long_lived_binding = 1;

    // Это раздел, он имеет меньшую область видимости, чем функция main
    {
        // Эта переменная существует только в этом разделе
        let short_lived_binding = 2;

        println!("inner short: {}", short_lived_binding);

        // Эта переменная *затеняет* собой внешнюю
        let long_lived_binding = 5_f32;

        println!("inner long: {}", long_lived_binding);
    }
    // Конец раздела

    // Ошибка! `short_lived_binding` нет в этой области видимости
    println!("outer short: {}", short_lived_binding);
    // ИСПРАВЬТЕ ^ Накройте примечанием строку

    println!("outer long: {}", long_lived_binding);

    // Это связывание так же *скрывает* собой предыдущие
    let long_lived_binding = 'a';

    println!("outer long: {}", long_lived_binding);
}
```
