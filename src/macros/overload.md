# Перегрузка

Макросы могут быть перегружены, принимая различные комбинации переменных.
В этом плане, `macro_rules!` может работать подобно разделу сопоставления (match):

```rust,editable
// `test!` будет сравнивать `$left` и `$right`
// по разному, в зависимости от того, как вы объявите их:
macro_rules! test {
    // Не нужно разделять переменные запятой.
    // Можно использовать любой образец!
    ($left:expr; and $right:expr) => (
        println!("{:?} и {:?} это {:?}",
                 stringify!($left),
                 stringify!($right),
                 $left && $right)
    );
    // ^ каждый раздел должен заканчиваться точкой с запятой.
    ($left:expr; or $right:expr) => (
        println!("{:?} или {:?} это {:?}",
                 stringify!($left),
                 stringify!($right),
                 $left || $right)
    );
}

fn main() {
    test!(1i32 + 1 == 2i32; and 2i32 * 2 == 4i32);
    test!(true; or false);
}
```