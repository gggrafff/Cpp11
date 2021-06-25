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
Ключевое слово auto указывает, что тип объявляемой переменной будет автоматически выводиться из ее инициализатора.  
Переменная, объявленная с помощью ключевого слова auto, должна быть инициализирована во время ее объявления, иначе возникнет ошибка компиляции (компилятор не сможет вывести тип).  
Пример:  
```C++
auto x = 4;
auto y = 3.37;
auto ptr = &x;
```

Более жизненный пример:  
```
std::map<std::string, std::shared_ptr<Widget>> table;

// C++98
std::map<std::string, std::shared_ptr<Widget>>::iterator i = table.find("42");

// C++11/14/17
auto j = table.find("42");
```
В данном примере `auto` помогает нам не писать сложное название типа.  

Если `auto` используется для объявления нескольких переменных, выведенные типы должны совпадать. Например, объявление `auto i = 0, d = 0.0;` некорректно, а объявление `auto i = 0, *p = &i;` верное и `auto` выводится как `int`. То есть действует простое правило: одно упоминание ключевого слова `auto` заменяется при компиляции только одним названием типа, если его можно вывести.  

**Как выводится тип?**  
Для вывода типов с `auto` используются те же правила, которые действуют при [выводе типа шаблонов](https://en.cppreference.com/w/cpp/language/template_argument_deduction#Other_contexts) (с одним исключением, которое рассмотрим ниже).  

То есть, при выводе типов в примере:  
```C++
auto i = 42;
```
работают те же правила, что и при выводе типа для параметра шаблона:  
```C++
template<typename T>
void foo(T i) {}

foo(42);
```

Это значит, что  
 * При использовании `auto` теряется ссылочность `&`, а так же теряются квалификаторы `const` и `volatile`.  
 * Необходимо явно указывать, что мы хотим создать ссылку, const или volatile переменную.  
 * С указателями всё в порядке: если тип инициализатора T* или const T*, то тип var также будет T* или const T* соответственно.  
 * Универсальные ссылки работают так же, как у параметров шаблонов. Тип переменной будет зависеть от того какая value category у инициализирующего значения. Если rvalue, то тип var будет T&&, если же lvalue, то T&. Cv квалификаторы при этом сохраняются.  

Объяснить это можно тем, что неявное создание ссылок на объекты могло бы приводить к труднодиагностируемым ошибкам, а если вам всё таки требуется такое поведение, то можно использовать decltype.  

Пример:  
```C++
// function that returns a 'reference to int' type
int& fun() {   }

// m will default to int type instead of
// int& type
auto m = fun();

// n will be of int& type because of use of
// extra & with auto keyword
auto& n = fun();
```

Ещё пример:  
```C++
const double pi = 3.14;

auto copy_pi = pi;  // изменяемая переменная
const auto const_copy_pi = pi;  // неизменяемая константа
const auto& const_ref_pi = pi;  // константная ссылка на pi
```

Пример с универсальной ссылкой:  
```C++
auto&& var = some_expression;  // тип var будет зависеть от того какая value category у some_expression. Если rvalue, то тип var будет T&&, если же lvalue, то T&. Cv квалификаторы при этом сохраняются.
```

Но есть одна ситуация, в которой auto и параметры шаблона выводятся по-разному - это std::initializer_list:  
```C++
auto var = {1, 2, 3};  //  Ok, var будет иметь тип std::initializer_list<int>
template<typename T> void foo(T t);
foo({1, 2, 3}); // Не компилируется
```
Объяснений такому поведению немного и все они не отличаются внятностью. Скотт Майерс, например, по этому поводу пишет так: “I have no idea why type deduction for auto and for templates is not identical. If you know, please tell me!”.  

### Функции
#### Возвращаемое значение
Появилось в C++14.  

Если возвращаемый функцией тип обозначен как `auto`, то тип будет выведен компилятором из типа выражения, используемого в операторе `return`. Вывод следует правилам вывода аргументов шаблона.  
```C++
int x = 1;
auto f() { return x; }        // return type is int
const auto& f() { return x; } // return type is const int&
```

Если есть несколько операторов `return`, все они должны возвращать одинаковый тип.  
```C++
auto f(bool val)
{
    if (val) return 123; // deduces return type int
    else return 3.14f;   // error: deduces return type float
}
```

С std::initializer_list тип вывести не получится:  
```C++
auto func () { return {1, 2, 3}; } // error
```

Виртуальные функции и корутины не могут использовать `auto` в качестве типа возвращаемого значения:  
```C++
struct F
{
    virtual auto f() { return 2; } // error
};
```

Выводимый тип можно подсказать:  
```C++
auto main() -> int {}
// is equivalent to
int main() {}
```
Это может быть удобно в сочетании с decltype:  
```C++
template <typename T1, typename T2>
auto Add(const T1& lhs, const T2& rhs) -> decltype(foo(lhs)) { return lhs + rhs; }
```

[Подробнее](https://en.cppreference.com/w/cpp/language/function#Return_type_deduction)  

#### Аргумент функции
Появилось в C++20.  
В C++20 с помощью `auto` можно записывать сокращённое объявление шаблона:  
```C++
void f1(auto);    // same as template<class T> void f(T)
void f2(C1 auto); // same as template<C1 T> void f7(T), if C1 is a concept
```
[Подробнее](https://en.cppreference.com/w/cpp/language/function#Parameter_list)  

### Range-based for
Автоматический вывод типов удобно использовать в контексте range-based циклов:  
```C++
std::vector<std::string> strings = { "stuff", "things", "misc" };
for(auto s : strings) {
    std::cout << s << std::endl;
}
```
А также для обращения к элементам по ссылке:  
```C++
for(auto& s : strings) {
    std::cout << s << std::endl;
}
```
И аналогично с константными ссылками:  
```C++
for(const auto& s : strings) {
    std::cout << s << std::endl;
}
```

### Тип замыкания
При объявлении замыкания с использованием лямбда-выражений тип нового замыкания нам не известен. Поэтому здесь просто невозможно было бы обойтись без `auto`:  
```C++
auto f = [](){ std::cout << "lambda\n"; };
f();
```
При создании замыкания компилятор создаёт функтор (структуру с оператором `operator()`) с уникальным типом (чтобы избежать конфликта имён). Этот тип не известнен во время написания кода, он будет сгенерирован при компиляции, поэтому обойтись без `auto` мы бы в этой ситуации не смогли, даже если бы захотели.  

### Тип аргументов лямбда-выражения
Появилось в C++14.  
Начиная с C++14 можно использовать `auto` для типов аргументов лямбда-выражения. Это работает аналогично тому, как работает `auto` для аргументов функции в C++20: в создаваемом замыкании оператор `operator()` становится шаблоном и может быть инстанцирован для разных типов данных.  
```C++
// generic lambda, operator() is a template with two parameters
auto glambda = [](auto a, auto&& b) { return a < b; };
bool b = glambda(3, 3.14); // ok
 
// generic lambda, operator() is a template with one parameter
auto vglambda = [](auto printer) {
    return [=](auto&&... ts) // generic lambda, ts is a parameter pack
    { 
        printer(std::forward<decltype(ts)>(ts)...);
        return [=] { printer(ts...); }; // nullary lambda (takes no parameters)
    };
};
auto p = vglambda([](auto v1, auto v2, auto v3) { std::cout << v1 << v2 << v3; });
auto q = p(1, 'a', 3.14); // outputs 1a3.14
q();                      // outputs 1a3.14
```
Это позволяет вызывать замыкание для разных типов параметров, а так же позволяет не писать длинные и сложные названия типов.  
[Подробнее](https://en.cppreference.com/w/cpp/language/lambda)  

### Non-type параметры шаблонов
Новая возможность С++17. Подробнее в [Declaring non-type template arguments with auto](https://github.com/gggrafff/Cpp17-Constexpr-lambda-Fold-expression-Attributes-Type-deduction-Auto-template-parameter/blob/main/AutoTemplateParameters.md).  

### Structured bindings
Новая возможность С++17. Подробнее в [Structured bindings](https://github.com/gggrafff/Cpp17-If-constexpr-Structured-bindings-Statements-with-initializer-std-filesystem/blob/main/StructuredBindings.md).  

## Когда этим пользоваться?
Этот вопрос почти религиозный и однозначного ответа на него нет. Некоторые стайлгайды рекомендуют использовать `auto` везде, где это возможно, некоторые другие - никогда не использовать.  

Использование `auto` даёт следующие преимущества:  
 * Надежность: если тип выражения изменился — то автоматически изменится тип переменной или возвращаемого значения функции, изменения не придётся вносить в несколько мест.  
 * Производительность: вы избегаете ошибочных случайных преобразований типа.  
 * Удобство использования: не нужно беспокоиться о неправильном написании имени типа.  
 * Эффективность: писать код проще.  

Конечно, чтение кода при использовании `auto` может быть затруднено. Например, вам может потребоваться посмотреть весь код функции, чтобы понять, какой тип она возвращает.  

Я рекомендую использовать `auto` в тех ситуациях, когда оно не затрудняет чтение кода.  

Например, `auto` может быть удобным в следующих ситуациях:  
 * работа с длинными именами типов  
 * range-based циклы  
 * чтобы не повторять тип дважды: `auto w = std::make_shared<Widget>();`  
 * и т.д.  

## Тонкие места и интересные примеры

1. Правила выведения типов для `auto` почти такие же, как для параметров шаблонов, но есть исключение: std::initializer_list выводится по-разному, см. выше.  
2. 'auto' может помочь избежать ошибок в написании типов, а соотвтетственно избежать ненужных приведений типов и копирований объектов.  
Например:  
```C++
#include <iostream>
#include <map>

struct Test {
        Test() = default;
        Test(const Test &other): a(other.a), b(other.b) {
            std::cout << "Copy constructor" << std::endl;
        }
        int a {0};
        int b {1};
    };

int main() {
    auto myMap = std::map<int,Test>();
    myMap.emplace(1, Test{});
    
    std::cout << "first example: ";
    std::pair<int, Test> const& firstPair2 = *myMap.begin();  // copy!
    
    std::cout << std::endl << std::endl << "second example: ";
    auto const& firstPair = *myMap.begin();  // no copy!
}
```
Причина лишнего копирования в этом примере в том, что настоящий тип - это `std::pair<const int, Test>`.  

3. Легко допустить ошибки при использовании `auto` с прокси-объектами. Явное указание типа позволило бы выявить ошибку на этапе компиляции или привести тип к нужному. Использование `auto` может повлечь создание объектов, ссылающихся на удалённые данные.  
Пример:  
```C++
std::vector<bool> flags{true, true, false};
auto flag = flags[0];
flags.push_back(true);
```
Здесь `flag` имеет тип не `bool`, а `std::vector<bool>::reference`, поскольку для `std::vector<bool>` оператор `operator[]` возвращает прокси-объект. Явное указание типа `bool` привело бы к преобразаванию типа и ошибок бы не было. Когда `flags.push_back(true)` изменяет контейнер, может произойти реаллокация, и прокси-объект может начать ссылаться на элемент, который больше не существует.  
Ещё пример:  
```C++
void foo(bool b);

std::vector<bool> getFlags();

auto flag = getFlags()[5];
foo(flag);
```
В этом примере vector является временным объектом и сразу удаляется после присваивания, поэтому вызов `foo` вызывает неопределенное поведение.  


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
