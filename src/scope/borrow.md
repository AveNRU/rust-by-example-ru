# Заимствование

Большую часть времени мы хотим обращаться к данным без получения владения над
ними. Для этого Ржавчина предоставляет рычаг *заимствования* Вместо передачи
предметов по значению (`T`), предметы могут быть переданы по ссылке (`&T`).

Сборщик статически обеспечивает, что ссылки *всегда* указывают на допустимые
предметы посредством проверки заимствований. К примеру, исходный предмет не может
быть уничтожен, пока существуют ссылки на него.

```rust,editable,ignore,mdbook-runnable
// Эта функция берёт во владение упаковку и уничтожает её
fn eat_box_i32(boxed_i32: Box<i32>) {
    println!("Уничтожаем упаковку в которой хранится {}", boxed_i32);
}

// Эта функция заимствует i32
fn borrow_i32(borrowed_i32: &i32) {
    println!("Это число равно: {}", borrowed_i32);
}

fn main() {
    // Создаём упакованное i32, и i32 в обойме
    let boxed_i32 = Box::new(5_i32);
    let stacked_i32 = 6_i32;

    // Заимствуем содержимое упаковки. При этом мы не владеем ресурсом.
    // Содержимое может быть заимствовано снова.
    borrow_i32(&boxed_i32);
    borrow_i32(&stacked_i32);

    {
        // Получаем ссылку на данные, которые хранятся внутри упаковки
        let _ref_to_i32: &i32 = &boxed_i32;

        // Ошибка!
        // Нельзя уничтожать упаковку `boxed_i32` пока данные внутри заимствованы.
        eat_box_i32(boxed_i32);
        // ИСПРАВЬТЕ ^ Накройте примечанием эту строку

        // `_ref_to_i32` покидает область видимости и больше не является заимствованным ресурсом.
    }

    // `boxed_i32` теперь может получить владение над `eat_box` и быть уничтожено
    eat_box_i32(boxed_i32);
}
```
