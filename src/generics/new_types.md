# New Type идиома

Идиома `newtype` обеспечивает во время сборки, 
что программе передаётся значение правильного вида.

Например, функция верификации возраста, которая проверяет 
возраст в годах *должна* получать значение вида 
`Years`.

```rust,
struct Years(i64);

struct Days(i64);

impl Years {
    pub fn to_days(&self) -> Days {
        Days(self.0 * 365)
    }
}


impl Days {
    /// truncates partial years
    pub fn to_years(&self) -> Years {
        Years(self.0 / 365)
    }
}

fn old_enough(age: &Years) -> bool {
    age.0 >= 18
}

fn main() {
    let age = Years(5);
    let age_days = age.to_days();
    println!("Old enough {}", old_enough(&age));
    println!("Old enough {}", old_enough(&age_days.to_years()));
    // println!("Old enough {}", old_enough(&age_days));
}
```

Удалите примечание с последнего `println`, чтобы увидеть, что вид 
должен быть `Years`.

Чтобы получить из `newtype`-переменной значение 
базового вида, вы можете использовать кортежный правила написания, 
как в примере:

```rust,
struct Years(i64);

fn main() {
    let years = Years(42);
    let years_as_primitive: i64 = years.0;
}
```

### Смотрите также:

[`struct`](../custom_types/structs.md)
