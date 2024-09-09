# `Box`, обойма  и куча

Все значения в Ржачине по умолчанию распределяются в обойме. Значения могут быть *упакованы*
(распределены в куче) при помощи создания `Box<T>`. `Box` - это умный указатель на находящееся в куче значение вида `T`. Когда `Box` покидает область видимости, вызывается его уничтожитель, который уничтожает внутренний предмет, и занятая им память в куче освобождается.

Упакованные значения могут быть разыменованы с помощью действия `*`.
Эта действие убирает один уровень косвенности.

```rust,editable
use std::mem;

#[allow(dead_code)]
#[derive(Debug, Clone, Copy)]
struct Point {
    x: f64,
    y: f64,
}

#[allow(dead_code)]
struct Rectangle {
    p1: Point,
    p2: Point,
}

fn origin() -> Point {
    Point { x: 0.0, y: 0.0 }
}

fn boxed_origin() -> Box<Point> {
    // Расположить эту точку в куче и вернуть указатель на неё
    Box::new(Point { x: 0.0, y: 0.0 })
}

fn main() {
    // (все изложения видов избыточны)
    // Переменные, находящиеся в обойме
    let point: Point = origin();
    let rectangle: Rectangle = Rectangle {
        p1: origin(),
        p2: Point { x: 3.0, y: 4.0 }
    };

    // Прямоугольник, находящийся в куче
    let boxed_rectangle: Box<Rectangle> = Box::new(Rectangle {
        p1: origin(),
        p2: origin()
    });

    // Итог функции может быть упакован
    let boxed_point: Box<Point> = Box::new(origin());

    // Двойная косвенность
    let box_in_a_box: Box<Box<Point>> = Box::new(boxed_origin());

    println!("Точка занимает {} байт в обойме",
             mem::size_of_val(&point));
    println!("Прямоугольник занимает {} байт в обойме",
             mem::size_of_val(&rectangle));

    // box size == pointer size
    println!("Упакованная точка занимает {} байт в обойме",
             mem::size_of_val(&boxed_point));
    println!("Упакованный прямоугольник занимает {} байт в обойме",
             mem::size_of_val(&boxed_rectangle));
    println!("Упакованная 'упаковка' занимает {} байт в обойме",
             mem::size_of_val(&box_in_a_box));

    // Копируем данные из `boxed_point` в `unboxed_point`
    let unboxed_point: Point = *boxed_point;
    println!("Распакованная точка занимает {} байт в обойме",
             mem::size_of_val(&unboxed_point));
}
```
