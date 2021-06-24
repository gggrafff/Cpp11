# auto
## Что это такое?
Ключевое слово `auto` - это ключевое слово, представляющее автоматически выводимый тип.  

`auto` уже было зарезервированным ключевым словом в C++98, унаследованным от C.  
В старых версиях C ++ его можно было использовать для указания времени жизни переменной. Этот смысл был убран из языка.  

Ключевое слово auto обеспечивает автоматический вывод типа переменной.  

Вывод типа подразумевает автоматическое определение типа данных выражения.  
До C++11 каждый тип данных должен быть явно объявлен во время компиляции, но в C++11 `auto` позволяет программисту возложить определение типа на компилятор.  
Благодаря этому мы можем тратить меньше времени на то, чтобы записывать то, что компилятор уже знает.  
Поскольку все типы выводятся только на этапе компиляции, время компиляции немного увеличивается, но это не влияет на время выполнения программы.  

## Как этим пользоваться?
Ключевое слово `auto` может применяться в различных контекстах. Рассмотрим подробнее...  

### Тип переменных


### Функции


### Тип замыкания


### Тип аргументов лямбда-выражения


### Non-type параметры шаблонов
Новая возможность С++17. Подробнее в [Declaring non-type template arguments with auto](https://github.com/gggrafff/Cpp17-Constexpr-lambda-Fold-expression-Attributes-Type-deduction-Auto-template-parameter/blob/main/AutoTemplateParameters.md).  

### Structured bindings
Новая возможность С++17. Подробнее в [Structured bindings](https://github.com/gggrafff/Cpp17-If-constexpr-Structured-bindings-Statements-with-initializer-std-filesystem/blob/main/StructuredBindings.md).  

## Когда этим пользоваться?


## Тонкие места


## Упражнения
Взято из [Секреты auto и decltype](https://habr.com/ru/post/206458/).  

1. Какой тип будет у переменных ri1..riN после выполнения следующего кода?  
```C++
int foo();
int& foo1();
const int foo2();
const int& foo3();

int main()
{
  auto ri = foo();
  auto ri1 = foo1();
  auto ri2 = foo2();
  auto ri3 = foo3();

  auto& ri4 = foo();
  auto& ri5 = foo1();
  auto& ri6 = foo2();
  auto& ri7 = foo3();

  auto&& ri8 = foo();
  auto&& ri9 = foo1();
  auto&& ri10 = foo2();
  auto&& ri11 = foo3();

  int k = 5;
  decltype(k)&& rk = k;

  decltype(foo())&& ri12 = foo();
  decltype(foo1())&& ri13 = foo1();
  
  int i = 3;
  decltype(i) ri14;
  decltype((i)) ri15;
}
```

2. Скомпилируются ли следующие фрагменты?

`auto lmbd = [](auto i){...};`

`void foo(auto i);`

`decltype(auto) var = some_expression; //WTF?!`

`auto var = {1, 2, 3}; //Если да, какой тип будет у var?`

```
template<typename T> void foo(T t){}
foo({1, 2, 3});
```
