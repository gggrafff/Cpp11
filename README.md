# Особенности C++11: decltype, rvalue и lvalue ссылки, move semantic, variadic Templates, POD.
## Введение
12 августа 2011 года Международная организация по стандартизации (англ. «ISO») утвердила новую версию языка C++ под названием C++11. Было добавлено много нового функционала.  

Бьёрн Страуструп охарактеризовал цели C++11 следующим образом:  
 * Укрепить сильные стороны языка C++, нежели пытаться захватить те области, в которых язык С++ слаб (например, приложения Windows с «тяжелым» графическим интерфейсом). Сосредоточиться на том, чтобы заставить язык С++ делать еще лучше то, что у него и так хорошо получается.  
 * Сделать язык C++ легче в изучении, использовании и обучении, предоставить функционал, который сделает язык более простым в использовании.  

Руководствуясь этими целями, комитет, который согласовывал версию С++11, постарался выполнить следующее:  
 * Поддерживать стабильность и совместимость со старыми версиями языков C++ и Cи (насколько это возможно). Программы, которые работали под C++03 должны работать и под C++11.  
 * Свести к минимуму количество основных расширений языка С++. Внести большую часть изменений в Стандартную библиотеку С++ (эта цель не была достигнута в полной мере).  
 * Сосредоточиться на улучшении механизмов абстракции (классы, шаблоны), нежели на добавлении механизмов обработки конкретных случаев (которые случаются нечасто).  
 * Повысить безопасность типов данных.  
 * Повысить производительность и позволить языку C++ напрямую работать с оборудованием.  
 * Рассмотреть вопросы юзабилити и экосистемы. Язык C++ должен хорошо работать с другими инструментами, быть простым в использовании, обучении и т.д.  

Давайте рассмотрим некоторые возможности С++11.

## auto


## decltype


## rvalue и lvalue ссылки


## move semantic


## variadic templates


## POD


## Что почитать?
О C++11:  
[Обзор стандарта С++ 11 и 14 версии](https://glebradchenko.susu.ru/courses/bachelor/oop/2014/SUSU_OOP_Rep_3_Cpp11.pdf)  
[C++ 11 FAQ](http://sergeyteplyakov.blogspot.com/2012/05/c-11-faq.html)  
[Десять возможностей C++11, которые должен использовать каждый C++ разработчик](https://habr.com/ru/post/182920/)  
[C++11. Нововведения](https://ravesli.com/c-11-novovvedeniya/)  

Об auto и decltype:  
[Placeholder type specifiers (since C++11)](https://en.cppreference.com/w/cpp/language/auto)  
[decltype specifier](https://en.cppreference.com/w/cpp/language/decltype)  
[Секреты auto и decltype](https://habr.com/ru/post/206458/)  
[decltype (C++)](https://docs.microsoft.com/en-us/cpp/cpp/decltype-cpp)  
[auto (C++)](https://docs.microsoft.com/en-us/cpp/cpp/auto-cpp)  
[auto](https://sodocumentation.net/cplusplus/topic/2421/auto)  
[decltype](https://ru.wikipedia.org/wiki/Decltype)  
[Type Inference in C++ (auto and decltype)](https://www.geeksforgeeks.org/type-inference-in-c-auto-and-decltype/)  

Move-семантика и rvalue:  
[What is move semantics?](https://stackoverflow.com/questions/3106110/what-is-move-semantics)  
[Семантика перемещения](https://ru.wikipedia.org/wiki/Семантика_перемещения)  
[Понимание lvalue и rvalue в C и С++](https://habr.com/ru/post/348198/)  
[Ссылки r-value](https://ravesli.com/urok-190-ssylki-r-value/)  
[Идеальная передача и универсальные ссылки в C++](https://habr.com/ru/post/242639/)  
[C++ std::move and std::forward](https://bajamircea.github.io/coding/cpp/2016/04/07/move-forward.html)  

О variadic templates:  
[Parameter pack](https://en.cppreference.com/w/cpp/language/parameter_pack)  
[Variadic templates. Tuples, unpacking and more](https://habr.com/ru/post/228031/)  
[https://teccxx.neocities.org/mx1/variadic_templates.html](https://teccxx.neocities.org/mx1/variadic_templates.html)  

О POD:  
[PODType](https://en.cppreference.com/w/cpp/named_req/PODType)  
[What are POD types in C++?](https://stackoverflow.com/questions/146452/what-are-pod-types-in-c)  
[Простая структура данных](https://ru.wikipedia.org/wiki/Простая_структура_данных)  
[POD типы в C++ ](http://itw66.ru/blog/c_plus_plus/470.html)  
[Разбор понятий: trivial type, standard layout, POD](https://habr.com/ru/post/532972/)  

Дополнительно:  
[Copy elision](https://en.cppreference.com/w/cpp/language/copy_elision)  
[RVO и NRVO](http://alenacpp.blogspot.com/2008/02/rvo-nrvo.html)  
[Short/Small String Optimization](https://stackoverflow.com/questions/10315041/meaning-of-acronym-sso-in-the-context-of-stdstring/10319672#10319672)  
