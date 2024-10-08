# Domain Specific Languages (DSLs)

DSL - это мини язык, встроенный в макросы Ржавчины. Это полностью 
допустимая рукопись на Ржавчине, так как система макросов разворачивается 
в нормальные конструкции, но выглядит как маленький язык. Это 
позволяет вам определять краткий или интуитивный правила написания для 
некоторой возможности (в пределах границ).

Предположим, я хочу определить небольшое API для калькулятора. 
Я хотел бы предоставить выражение и вывести итог в окно вывода.

```rust,editable
macro_rules! calculate {
    (eval $e:expr) => {{
        {
            let val: usize = $e; // Заставим быть переменную целым числом.
            println!("{} = {}", stringify!{$e}, val);
        }
    }};
}

fn main() {
    calculate! {
        eval 1 + 2 // хе-хе, `eval` _не_ ключевое слово Rust!
    }

    calculate! {
        eval (1 + 2) * (3 / 4)
    }
}
```

Вывод:

```txt
1 + 2 = 3
(1 + 2) * (3 / 4) = 0
```

Это очень простой пример, но можно разработать и гораздо более 
сложные внешние оболочки, такие как [`lazy_static`](https://crates.io/crates/lazy_static) 
или [`clap`](https://crates.io/crates/clap).

Также обратите внимание на две пары скобок в макросе. Внешняя 
пара является частью правил написания `macro_rules!`, в 
дополнение к `()` или `[]`.
