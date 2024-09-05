# PhantomData-параметры

Параметры фантомного вида - единственное, что не отображается во 
время выполнения, но проверяется статически (и только статически) 
во время сборки.

Виды данных могут использовать дополнительные обобщённые виды 
в качестве параметров-маркеров или для выполнения проверки 
видов во время сборки. Эти дополнительные параметры не 
сохраняют значения и не имеют поведения во время выполнения.

В следующем примере мы совмеисполнения [std::marker::PhantomData](https://doc.rust-lang.org/std/marker/struct.PhantomData.html) и подход параметров фантомных видов для создания упорядоченных рядов разных видов.

```rust,editable
use std::marker::PhantomData;

// Фантомная кортежная структура, которая имеет обобщение `A` со скрытым параметром `B`.
#[derive(PartialEq)] // Разрешаем для данного вида сравнения.
struct PhantomTuple<A, B>(A,PhantomData<B>);

// Фантомная структура, которая имеет обобщение `A` со скрытым параметром `B`.
#[derive(PartialEq)] // Разрешаем для данного вида сравнения.
struct PhantomStruct<A, B> { first: A, phantom: PhantomData<B> }

// Заметьте: память выделена для обобщённого вида `A`, но не для `B`.
//           Следовательно, `B` не может быть использована в вычислениях.

fn main() {
    // Здесь `f32` и `f64` - скрытые параметры.
    // вид PhantomTuple объявлен с `<char, f32>`.
    let _tuple1: PhantomTuple<char, f32> = PhantomTuple('Q', PhantomData);
    // вид PhantomTuple объявлен с `<char, f64>`.
    let _tuple2: PhantomTuple<char, f64> = PhantomTuple('Q', PhantomData);

    // вид определён как `<char, f32>`.
    let _struct1: PhantomStruct<char, f32> = PhantomStruct {
        first: 'Q',
        phantom: PhantomData,
    };
    // вид определён как `<char, f64>`.
    let _struct2: PhantomStruct<char, f64> = PhantomStruct {
        first: 'Q',
        phantom: PhantomData,
    };
    
    // Ошибка времени сборки! виды не совпадают, так что сравнение не может быть произведено:
    //println!("_tuple1 == _tuple2 даёт в итоге: {}",
    //          _tuple1 == _tuple2);
    
    // Ошибка времени сборки! виды не совпадают, так что сравнение не может быть произведено:
    //println!("_struct1 == _struct2 даёт в итоге: {}",
    //          _struct1 == _struct2);
}
```

### Смотрите также:

[`derive`](../trait/derive.md), [`struct`](../custom_types/structs.md) и [кортежные структуры](../custom_types/structs.md)
