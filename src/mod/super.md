# `super` и `self`

Ключевые слова `super` и `self` в пути используются, 
чтобы устранить неоднозначность между используемыми элементами раздела.

```rust,editable
fn function() {
    println!("вызвана `function()`");
}

mod cool {
    pub fn function() {
        println!("called `cool::function()`");
    }
}

mod my {
    fn function() {
        println!("вызвана `my::function()`");
    }
    
    mod cool {
        pub fn function() {
            println!("вызвана `my::cool::function()`");
        }
    }
    
    pub fn indirect_call() {
        // Давайте вызовем  все функции под названием `function` в этой области видимости!
        print!("вызвана `my::indirect_call()`, с помощью которой\n> ");
        
        // Ключевое слово `self` ссылается на область видимости текущего раздела. 
        // В нашем случае - раздел `my`.
        // Вызов `self::function()`, так же как и вызов `function()` дают одинаковый итог,
        // т.к они ссылаются на одну и ту же функцию.
        self::function();
        function();
        
        // Мы так же можем использовать ключевое слово `self`,
        // чтобы получить доступ к другим разделам внутри раздела `my`:
        self::cool::function();
        
        // Ключевое слово `super` ссылается на родительскую область видимости (вне раздела `my`).
        super::function();
        
        // Этим действием мы свяжем `cool::function` в области видимости *дополнения*.
        // В данном случае область видимости дополнения является самой дальней областью видимости.
        {
            use cool::function as root_function;
            root_function();
        }
    }
}

fn main() {
    my::indirect_call();
}
```