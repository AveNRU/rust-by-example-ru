# Сущности

`Сущность (trait)` - это набор способов, определённых для неизвестного вида:
`Self`. Они могут получать доступ к другим способам,
которые были объявлены в том же сущности.

Сущности могут быть реализованы для любых видов данных. В примере ниже,
мы определили группу способов `Animal`. Сущность `Animal` реализован для вида данных
`Sheep`, что позволяет использовать способы из `Animal` внутри `Sheep`.

```rust,editable
struct Sheep { naked: bool, name: &'static str }

trait Animal {
    // Описание статического способа, `Self` ссылается на реализующий вид.
    fn new(name: &'static str) -> Self;

    // Описание способа экземпляра; они возвращают строки.
    fn name(&self) -> &'static str;
    fn noise(&self) -> &'static str;

    // Сущность может содержать определение способа по умолчанию
    fn talk(&self) {
        println!("{} говорит {}", self.name(), self.noise());
    }
}

impl Sheep {
    fn is_naked(&self) -> bool {
        self.naked
    }

    fn shear(&mut self) {
        if self.is_naked() {
            // Способы вида могут использовать способы сущности, реализованного для этого вида.
            println!("{} уже без волос...", self.name());
        } else {
            println!("{} подстригается!", self.name);

            self.naked = true;
        }
    }
}

// Реализуем сущность `Animal` для `Sheep`.
impl Animal for Sheep {
    // `Self` реализующий вид: `Sheep`.
    fn new(name: &'static str) -> Sheep {
        Sheep { name: name, naked: false }
    }

    fn name(&self) -> &'static str {
        self.name
    }

    fn noise(&self) -> &'static str {
        if self.is_naked() {
            "baaaaah?"
        } else {
            "baaaaah!"
        }
    }

    // Способы по умолчанию могут быть переопределены.
    fn talk(&self) {
        // Например, мы добавили немного спокойного миросозерцания...
        println!("{} делает паузу... {}", self.name, self.noise());
    }
}

fn main() {
    // Изложение вида в данном случае необходима.
    let mut dolly: Sheep = Animal::new("Dolly");
    // ЗАДАНИЕ ^ Попробуйте убрать изложение вида

    dolly.talk();
    dolly.shear();
    dolly.talk();
}
```
