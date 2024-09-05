# Объявление `use`

Объявление `use` используется, чтобы связать полный путь с новым именем,
что упрощает доступ.

```rust,editable,ignore
// extern crate deeply; // обычно эта строка есть и она не накрыта примечанием!

use crate::deeply::nested::{
    my_first_function,
    my_second_function,
    AndATraitType
};

fn main() {
    my_first_function();
}
```

Вы можете использовать ключевое слово `as`, что импортировать сущности и функции под другим именем:

```rust,editable
// Привязать путь `deeply::nested::function` к `other_function`.
use deeply::nested::function as other_function;

fn function() {
    println!("вызвана `function()`");
}

mod deeply {
    pub mod nested {
        pub fn function() {
            println!("вызвана `deeply::nested::function()`")
        }
    }
}

fn main() {
    // Упрощённый доступ к `deeply::nested::function`
    other_function();

    println!("Entering block");
    {
        // Эквивалентно `use deeply::nested::function as function`.
        // `function()` затенение собой внешнюю функцию.
        use deeply::nested::function;
        function();

        // у привязок `use` местная область видимости. В данном случае
        // внешняя `function()` затенена только в этом разделе.
        println!("Leaving block");
    }

    function();
}
```
