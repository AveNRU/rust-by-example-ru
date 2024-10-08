# Видимость

По умолчанию, элементы раздела являются закрытыми,
но это можно изменить добавив слово `pub`.
Только общедоступные элементы раздела могут быть доступны за пределами его области видимости.

```rust,editable
// Раздел по имени `my_mod`
mod my_mod {
    // Все элементы раздела по умолчанию являются закрытыми.
    fn private_function() {
        println!("вызвана `my_mod::private_function()`");
    }

    // Используем переключатель `pub`, чтобы сделать элемент открытым.
    pub fn function() {
        println!("вызвана `my_mod::function()`");
    }

    // Закрытые элементы раздела доступны другим элементам
    // данного раздела.
    pub fn indirect_access() {
        print!("вызвана `my_mod::indirect_access()`, которая\n> ");
        private_function();
    }

    // Разделы так же могут быть вложенными
    pub mod nested {
        pub fn function() {
            println!("вызвана `my_mod::nested::function()`");
        }

        #[allow(dead_code)]
        fn private_function() {
            println!("вызвана `my_mod::nested::private_function()`");
        }

        // Функции объявленные с использованием правил написания `pub(in path)` будет видна
        // только в пределах заданного пути.
        // `path` должен быть родительским или наследуемым разделем
        pub(in my_mod) fn public_function_in_my_mod() {
            print!("вызвана `my_mod::nested::public_function_in_my_mod()`, которая\n > ");
            public_function_in_nested()
        }

        // Функции объявленные с использованием правил написания `pub(self)` будет видна
        // только в текущем разделе
        pub(self) fn public_function_in_nested() {
            println!("вызвана `my_mod::nested::public_function_in_nested");
        }

        // Функции объявленные с использованием правил написания `pub(super)` будет видна
        // только в родительском разделе
        pub(super) fn public_function_in_super_mod() {
            println!("вызвана my_mod::nested::public_function_in_super_mod");
        }
    }

    pub fn call_public_function_in_my_mod() {
        print!("вызвана `my_mod::call_public_funcion_in_my_mod()`, которая\n> ");
        nested::public_function_in_my_mod();
        print!("> ");
        nested::public_function_in_super_mod();
    }

    // pub(crate) сделает функцию видимой для всего текущего дополнения
    pub(crate) fn public_function_in_crate() {
        println!("called `my_mod::public_function_in_crate()");
    }

    // Вложенные разделы подчиняются тем же правилам видимости
    mod private_nested {
        #[allow(dead_code)]
        pub fn function() {
            println!("вызвана `my_mod::private_nested::function()`");
        }
    }
}

fn function() {
    println!("вызвана `function()`");
}

fn main() {
    // Разделы позволяют устранить противоречия между элементами,
    // которые имеют одинаковые названия.
    function();
    my_mod::function();

    // Общедоступные элементы, включая те, что находятся во вложенном разделе,
    // доступны извне родительского раздела
    my_mod::indirect_access();
    my_mod::nested::function();
    my_mod::call_public_function_in_my_mod();

    // pub(crate) элементы можно вызвать от везде в этом же пакете
    my_mod::public_function_in_crate();
    
    // pub(in path) элементы могут вызываться только для указанного пути
    // Ошибка! функция `public_function_in_my_mod` закрытая
    //my_mod::nested::public_function_in_my_mod();
    // TODO ^ Попробуйте убрать примечания эту строку

    // Закрытые элементы раздела не доступны напрямую,
    // даже если вложенный раздел является открытым:

    // Ошибка! функция `private_function` закрытая
    //my_mod::private_function();
    // ЗАДАНИЕ ^ Попробуйте убрать примечания эту строку

    // Ошибка! функция `private_function` закрытая
    //my_modmy::nested::private_function();
    // ЗАДАНИЕ ^ Попробуйте убрать примечания эту строку

    // Ошибка! Раздел `private_nested` является закрытым
    //my_mod::private_nested::function();
    // ЗАДАНИЕ ^ Попробуйте убрать примечания эту строку
}
```