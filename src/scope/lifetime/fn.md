# Функции

Описания функции с указанием времени жизни имеют некоторые ограничения:

- любая ссылка *должна* иметь изложенное время жизни
- любая возвращаемая ссылка *должна* иметь то же время жизни, что входящая ссылкаили `static`.

Кроме того, обратите внимание, что возврат ссылок из функции, которая не имеет ссылок во входных переменнойх, запрещен, если он
приведет к возвращению ссылок на недопустимые данные. В следующем примере показаны
некоторые действительные формы функции со временем жизни:

```rust,editable
// Одна входная ссылка со временем жизни `'a`, которая
// будет жить как минимум до конца функции.
fn print_one<'a>(x: &'a i32) {
    println!("`print_one`: x is {}", x);
}

// Использование времени жизни также возможно с изменяемыми ссылками.
fn add_one<'a>(x: &'a mut i32) {
    *x += 1;
}

// Несколько элементов с различными временами жизни. В этом случае
// было бы хорошо, чтобы у обоих ссылок было одно время жизни `'a`,
// в более сложных случаях может потребоваться различное время жизни.
fn print_multi<'a, 'b>(x: &'a i32, y: &'b i32) {
    println!("`print_multi`: x is {}, y is {}", x, y);
}

// Возврат переданных на вход ссылок допустим.
// Однако должен быть указано правильное время жизни.
fn pass_x<'a, 'b>(x: &'a i32, _: &'b i32) -> &'a i32 { x }

//fn invalid_output<'a>() -> &'a String { &String::from("foo") }
// Рукопись написанная выше является недопустимым: время жизни `'a`
// должно жить после выхода из функции.
// Здесь, `&String::from("foo")` создает ссылку на `String`
// Данные будут удалены после выхода из области видимости, и
// будет возвращена ссылка на недопустимые данные.

fn main() {
    let x = 7;
    let y = 9;
    
    print_one(&x);
    print_multi(&x, &y);
    
    let z = pass_x(&x, &y);
    print_one(z);

    let mut t = 3;
    add_one(&mut t);
    print_one(&t);
}
```

### Смотрите также:

[функции](fn.md)
