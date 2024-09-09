# Способы

Способы - это функции, прикреплённые к предметам. Эти способы 
имеют допуск к данным предмета и другим его способам через 
ключевое слово `self`. Способы определяются под 
разделом `impl`.

```rust,editable
struct Point {
    x: f64,
    y: f64,
}

// Раздел реализаций, все способы `Point` расположены здесь
impl Point {
    // Это статический способ
    // Статические способы не нуждаются в вызове от экземпляра
    // Эти способы, как правило, используются как конструкторы
    fn origin() -> Point {
        Point { x: 0.0, y: 0.0 }
    }

    // Другой статический способ, берёт два переменной
    fn new(x: f64, y: f64) -> Point {
        Point { x: x, y: y }
    }
}

struct Rectangle {
    p1: Point,
    p2: Point,
}

impl Rectangle {
    // Это способ экземпляра
    // `&self` - это сахар для `self: &Self`, где `Self` - это вид
    // вызываемого предмета. В этом месте `Self` = `Rectangle`
    fn area(&self) -> f64 {
        // `self` даёт допуск к полям стопки через приказчик точка
        let Point { x: x1, y: y1 } = self.p1;
        let Point { x: x2, y: y2 } = self.p2;

        // `abs` - это способ `f64`, который возвращает абсолютную величину
        // вызываемого
        ((x1 - x2) * (y1 - y2)).abs()
    }

    fn perimeter(&self) -> f64 {
        let Point { x: x1, y: y1 } = self.p1;
        let Point { x: x2, y: y2 } = self.p2;

        2.0 * ((x1 - x2).abs() + (y1 - y2).abs())
    }

    // Этот способ требует чтобы вызываемый предмет был изменяемым
    // `&mut self` - сахар для `self: &mut Self`
    fn translate(&mut self, x: f64, y: f64) {
        self.p1.x += x;
        self.p2.x += x;

        self.p1.y += y;
        self.p2.y += y;
    }
}

// `Pair` владеет ресурсами: два целых числа в куче
struct Pair(Box<i32>, Box<i32>);

impl Pair {
    // Этот способ "съедает" ресурсы вызываемого предмета
    // `self` - сахар для `self: Self`
    fn destroy(self) {
        // разбор `self`
        let Pair(first, second) = self;

        println!("Destroying Pair({}, {})", first, second);

        // `first` и `second` выходят из области видимости и освобождаются
    }
}

fn main() {
    let rectangle = Rectangle {
        // Статические способы вызываются двойными двоеточиями
        p1: Point::origin(),
        p2: Point::new(3.0, 4.0),
    };

    // Способ экземпляра вызывается с помощью приказчика точка
    // Обратите внимание, что первый переменная `&self` неявно пропускается т.е.
    // `rectangle.perimeter()` === `perimeter(&rectangle)`
    println!("Rectangle perimeter: {}", rectangle.perimeter());
    println!("Rectangle area: {}", rectangle.area());

    let mut square = Rectangle {
        p1: Point::origin(),
        p2: Point::new(1.0, 1.0),
    };

    // Ошибка! `rectangle` неизменяемый, но этот способ нуждается в изменяемом
    // предмете
    //rectangle.translate(1.0, 0.0);
    // ЗАДАНИЕ ^ Попробуйте удалить примечание

    // Хорошо, изменяемый предмет может вызывать изменяемые способы
    square.translate(1.0, 1.0);

    let pair = Pair(Box::new(1), Box::new(2));

    pair.destroy();

    // Ошибка! `destroy` вызывает "съеденный" `pair`
    //pair.destroy();
    // ЗАДАНИЕ ^ Попробуйте удалить примечание
}
```
