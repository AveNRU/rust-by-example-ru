# `Result`

Раньше мы видели, что в качестве возвращаемого значения из 
функции, которая может завершиться с ошибкой, можно использовать 
перечисление `Option`, в котором `None` 
будет обозначать неудачу. Однако иногда важно понять 
*почему* действие потерпела неудачу. Для этого у нас есть 
перечисление `Result`.

Перечисление `Result<T, E>` имеет два исхода:

- `Ok(value)`, который обозначает, что действие успешно завершилась, и оборачивает значение (`value`), возвращаемое действием (`value` имеет вид `T`).
- `Err(why)`, который показывает, что действие потерпела неудачу, оборачивает значение ошибки (причину, `why`), которое (надеемся) описывает причину неудачи. `why` имеет вид `E`.

```rust,editable,ignore,mdbook-runnable
mod checked {
    // Математические "ошибки", которые мы хотим отлавливать
    #[derive(Debug)]
    pub enum MathError {
        DivisionByZero,
        NonPositiveLogarithm,
        NegativeSquareRoot,
    }

    pub type MathResult = Result<f64, MathError>;

    pub fn div(x: f64, y: f64) -> MathResult {
        if y == 0.0 {
            // При таком значение действие потерпит неудачу.
            // Вместо этого давайте вернём ошибку, обёрнутую в `Err`
            Err(MathError::DivisionByZero)
        } else {
            // Эта действие возможна, так что вернём итог, обёрнутый в `Ok`
            Ok(x / y)
        }
    }

    pub fn sqrt(x: f64) -> MathResult {
        if x < 0.0 {
            Err(MathError::NegativeSquareRoot)
        } else {
            Ok(x.sqrt())
        }
    }

    pub fn ln(x: f64) -> MathResult {
        if x <= 0.0 {
            Err(MathError::NonPositiveLogarithm)
        } else {
            Ok(x.ln())
        }
    }
}

// `op(x, y)` === `sqrt(ln(x / y))`
fn op(x: f64, y: f64) -> f64 {
    // Это трёхуровневая пирамида из `match`!
    match checked::div(x, y) {
        Err(why) => panic!("{:?}", why),
        Ok(ratio) => match checked::ln(ratio) {
            Err(why) => panic!("{:?}", why),
            Ok(ln) => match checked::sqrt(ln) {
                Err(why) => panic!("{:?}", why),
                Ok(sqrt) => sqrt,
            },
        },
    }
}

fn main() {
    // Потерпит ли это неудачу?
    println!("{}", op(1.0, 10.0));
}
```
