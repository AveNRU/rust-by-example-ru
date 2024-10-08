# Файловый ввод-вывод

Стопка `File` представляет открытый файл (она является обёрткой над файловым дескриптором) и даёт возможность чтения/записи этого файла.

Из-за того, что многие вещи могут пойти не так в процессе файлового 
ввода-вывода, все способы `File` возвращают вид 
`io::Result<T>`, который является псевдонимом для 
`Result<T, io::Error>`.

Это делает *явными* ошибки всех действий ввода-вывода. 
Благодаря этому, программист может увидеть все пути отказов и 
обрабатывать их упреждающей форме.
