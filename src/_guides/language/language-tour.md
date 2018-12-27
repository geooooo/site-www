---
title: Тур по языку Dart
description: Тур по всем основным возможностям языка Dart.
short-title: Тур по языку
---
<?code-excerpt replace="/([A-Z]\w*)\d\b/$1/g"?>

На этой странице показывается, как использовать каждую основную возможность Dart,
от переменных и операторов до классов и библиотек, предполагается, что вы уже знаете какой-нибудь другой язык программирования.

Чтобы узнать больше о стандартной библиотеке Dart, смотрите
[Тур по библиотекам Dart](/guides/libraries/library-tour).
Всякий раз, когда вы желаете больше подробностей о возможностях языка,
проконсультируйтесь со [Спецификацией языка Dart](/guides/language/spec).

<div class="alert alert-info" markdown="1">
**Совет:**
Вы можете поиграть с большинстов возможностей языка Dart, используя DartPad
([узнать больше](/tools/dartpad)).

**<a href="{{ site.custom.dartpad.direct-link }}" target="_blank">открыть DartPad</a>**
</div>


## Базовая программа на Dart

Следующий код использует большинство наиболее базовых возможностей Dart:

<?code-excerpt "misc/test/language_tour/basic_test.dart"?>
{% prettify dart %}
// Определение функции.
printInteger(int aNumber) {
  print('Число $aNumber.'); // Вывод в консоль.
}

// Это точка начала исполнения приложения
main() {
  var number = 42; // Объявление и инициализация переменной.
  printInteger(number); // Вызов функции.
}
{% endprettify %}

Здесь то, что используется в этой программе и примяется во всех (или почти во всех) приложениях на Dart:


<code>// <em>Это комментарий.</em> </code>

:   Однострочных комментарий.
    Dart также поддерживает много-строчные и докуметирующие комментарии.
    За деталями, смотрите [Комментарии](#comments).

`int`

:   Тип. Один из других [встроенных типов](#built-in-types)
    , таких как `String`, `List`, и `bool`.

`42`

:   Числовой литерал. Числовой литерал - это один из видов констант времени компиляции.

`print()`

:   Простой способ вывести данные.

`'...'` (или `"..."`)

:   Литерал строки.

<code>$<em>имяПеременной</em></code> (или <code>${<em>выражение</em>}</code>)

:   Строковая интерполяция: включает строковое представление переменной или выражения
    как строковый литерал. За большими подробностями, смотрите
    [Строки](#strings).

`main()`

:   Специальная *требуемая*, глобальная функция, которая начинает исполнение приложения.
    За большей информацией, смотрите
    [функция main()](#the-main-function).

`var`

:   Способ объявить переменную, без уточнения её типа.

<div class="alert alert-info" markdown="1">
**Замечание:**
Код на этом сайте следует конвенции
[руководства стиля кода на Dart](/guides/language/effective-dart/style).
</div>


## Важные концепции

Раз вы уже изучаете язык Dart, примите во внимание эти факты и концепции:

-   Всё, что вы помещаете в переменные есть *объект* и каждый объект
    являетсе экземпляром некоторого *класса*. Каждое число, фукция и `null`
    объекты. Все объекты наследуются от класса [Object][].

-   Хоть Dart строго типизированный язык, аннотация типов опциональна,
    так как Dart может выводить типы. В коде выше, `number` выводится
    в тип `int`. Когда вы хотите сказать, что явного типа не ожидается,
    [исользуйте специальный тип `dynamic`][ObjectVsDynamic].

-   Dart поддерживает обобщённые типы, как `List<int>` (список целых чисел)
    или `List<dynamic>` (спиок объектов любого типа).

-   Dart поддерживает глобальные функции (такие как `main()`),
    а также функции, связанные с классом или объектом
    (*статические* и *методы экземпляра* соответственно).
    Вы можете также создать функцию с функцией (*вложенные* или *локальные функции*).

-   Так же, Dart поддерживает глобальные *переменные*, а также переменные,
    связанные с классом или объектом (статические или переменные экземпляра).
    Переменные экземпляра известны как поля или свойства.

-   В отличии от Java, Dart не имеет ключевых слов `public`, `protected` и `private`.
    Если идентификатор начинается с нижнего подчёркивания (\_), то он приватен
    для своей библиотеки. За деталями смотрите [Библиотека и видимость](#libraries-and-visibility).

-   *Идентификаторы* могут начинаться с буквы или нижнего подчёркивания (\_),
    следуя в любом порядке из тех же символов плюс цифр.

-   У Dart есть понятие *выражений* (которые имеют значение во время исполнения) и
    *инструкций* (которые его не имеют).
    Например, [условные выражения](#conditional-expressions)
    `условие ? выражение1 : выражение2` имеет значение `выражение1` или `выражение2`.
    В сравнение есть [инструкция if-else](#if-and-else), у которого нет значения.
    Инструкции часто содержат одно или множество выражений, но
    выражение не может содержать инструкцию.

-   Утилиты Dart могут сообщать о двух видах проблем: _предупреждения_ и _ошибки_.
    Предупреждения просто сообщают, что ваш код может не работать, но они не мешают
    исполнению вашей программы. Ошибки могут быть либо времени компиляции, либо времени исполнения.
    Ошибки времени компиляции не дадут программе запуститься в любом случае; ошибки времени исолнения
    приводят к [исключению](#exceptions), поднимающемуся во время исполнения кода.

## Ключевые слова

Следующая таблица показывает слова, которые в языке Dart относятся к специальным.

{% assign ckw = '&nbsp;<sup title="Контекстуальное ключевое слово" alt="Контекстуальное ключевое слово">1</sup>' %}
{% assign bii = '&nbsp;<sup title="Встроенный идентификатор" alt="Встроенный идентификатор">2</sup>' %}
{% assign lrw = '&nbsp;<sup title="Ограниченно зарезервированное слово" alt="Ограниченно зарезервированное слово">3</sup>' %}
| [abstract][]{{bii}}   | [dynamic][]{{bii}}    | [implements][]{{bii}} | [show][]{{ckw}}   |
| [as][]{{bii}}         | [else][]              | [import][]{{bii}}     | [static][]{{bii}} |
| [assert][]            | [enum][]              | [in][]                | [super][]         |
| [async][]{{ckw}}      | [export][]{{bii}}     | [interface][]{{bii}}  | [switch][]        |
| [await][]{{lrw}}      | [extends][]           | [is][]                | [sync][]{{ckw}}   |
| [break][]             | [external][]{{bii}}   | [library][]{{bii}}    | [this][]          |
| [case][]              | [factory][]{{bii}}    | [mixin][]{{bii}}      | [throw][]         |
| [catch][]             | [false][]             | [new][]               | [true][]          |
| [class][]             | [final][]             | [null][]              | [try][]           |
| [const][]             | [finally][]           | [on][]{{ckw}}         | [typedef][]{{bii}}|
| [continue][]          | [for][]               | [operator][]{{bii}}   | [var][]           |
| [covariant][]{{bii}}  | [Function][]{{bii}}   | [part][]{{bii}}       | [void][]          |
| [default][]           | [get][]{{bii}}        | [rethrow][]           | [while][]         |
| [deferred][]{{bii}}   | [hide][]{{ckw}}       | [return][]            | [with][]          |
| [do][]                | [if][]                | [set][]{{bii}}        | [yield][]{{lrw}}  |
{:.table .table-striped .nowrap}

[abstract]: #abstract-classes
[as]: #type-test-operators
[assert]: #assert
[async]: #asynchrony-support
[await]: #asynchrony-support
[break]: #break-and-continue
[case]: #switch-and-case
[catch]: #catch
[class]: #instance-variables
[const]: #final-and-const
{% comment %}
  [TODO: Make sure that points to a place that talks about const constructors,
  as well as const literals and variables.]
{% endcomment %}
[continue]: #break-and-continue
[covariant]: /guides/language/sound-problems#the-covariant-keyword
[default]: #switch-and-case
[deferred]: #lazily-loading-a-library
[do]: #while-and-do-while
[dynamic]: #important-concepts
[else]: #if-and-else
[enum]: #enumerated-types
[export]: /guides/libraries/create-library-packages
[extends]: #extending-a-class
[external]: https://stackoverflow.com/questions/24929659/what-does-external-mean-in-dart
[factory]: #factory-constructors
[false]: #booleans
[final]: #final-and-const
[finally]: #finally
[for]: #for-loops
[Function]: #functions
[get]: #getters-and-setters
[hide]: #importing-only-part-of-a-library
[if]: #if-and-else
[implements]: #implicit-interfaces
[import]: #using-libraries
[in]: #for-loops
[interface]: https://stackoverflow.com/questions/28595501/was-the-interface-keyword-removed-from-dart
[is]: #type-test-operators
[library]: #libraries-and-visibility
[mixin]: #adding-features-to-a-class-mixins
[new]: #using-constructors
[null]: #default-value
[on]: #catch
[operator]: #overridable-operators
[part]: /guides/libraries/create-library-packages#organizing-a-library-package
[rethrow]: #catch
[return]: #functions
[set]: #getters-and-setters
[show]: #importing-only-part-of-a-library
[static]: #class-variables-and-methods
[super]: #extending-a-class
[switch]: #switch-and-case
[sync]: #generators
[this]: #constructors
[throw]: #throw
[true]: #booleans
[try]: #catch
[typedef]: #typedefs
[var]: #variables
[void]: https://medium.com/dartlang/dart-2-legacy-of-the-void-e7afb5f44df0
{% comment %}
  TODO: Add coverage of void to the language tour.
{% endcomment %}
[with]: #adding-features-to-a-class-mixins
[while]: #while-and-do-while
[yield]: #generators

Избегайте использования этих слов как идентификаторв.
Тем неменее, если необходимо, ключевые слова, помеченные надстрочным индексом, могут
быть идентификаторами:

* Слова с надстрочным индексом **1** это **контекстуальные ключевые слова**,
  которые имеют смысл только в определённом месте.
  Они коректные идентификаторы везде.

* Слова с надстрочным индексом **2** это **встроенные идентификаторы**.
  Для упрощения задач портирования JavaScript кода в Dart,
  эти ключевые слова правильные идентификаторы в большинстве мест,
  но они не могут быть использованы как классы или имена типов или префиксы импорта.

* Слова с надстрочным индексом **3** новейшие, ограниченно зарезервированные слова, относящиеся
  к [поддержке асинхронности](#asynchrony-support), что были добавлены после релиза Dart 1.0.
  Вы не можете использовать `await` или `yield` как идентификтор в любых функциях, помеченных
  с помощью `async`, `async*` или `sync*`.

Все другие слова в таблице **зарезервированные слова**, которые не могут быть идентификаторами.

## Переменные

Здесь примеры по созданию переменных и их инициализации:


<?code-excerpt "misc/lib/language_tour/variables.dart (var-decl)"?>
{% prettify dart %}
var name = 'Bob';
{% endprettify %}

Переменные хранят ссылки. Переменная с именем `name` содеражит ссылку на объект
`String` со значением "Bob".
Тип переменной `name` при выводе типа будет `String`,
но вы можете изменить этот тип, указав его.
Если объект не ограничен одним типом, укажите тип `Object` или `dynamic`,
следуя [методическим указаниям по дизайну и проектированию][ObjectVsDynamic].

<?code-excerpt "misc/lib/language_tour/variables.dart (type-decl)"?>
{% prettify dart %}
dynamic name = 'Bob';
{% endprettify %}

Другой способ явно указать, какой тип должен быть выведен:

<?code-excerpt "misc/lib/language_tour/variables.dart (static-types)"?>
{% prettify dart %}
String name = 'Bob';
{% endprettify %}

<div class="alert alert-info" markdown="1">
**Замечание:**
Код на этом сайте следует конвенции
[руководства стиля кода на Dart](/guides/language/effective-dart/design#types)
по использованию `var`, вместо анотации типов для локальных переменных.
</div>


### Значения поумолчанию

Не инициализированные переменные имеют исходное значение `null`. Каждая переменная
числового типа изначально равна `null`, потому что числа, как и всё остальное в Dart - объекты.

<?code-excerpt "misc/test/language_tour/variables_test.dart (var-null-init)"?>
{% prettify dart %}
int lineCount;
assert(lineCount == null);
{% endprettify %}

<div class="alert alert-info" markdown="1">
**Замечание:**
Вызов `assert()` игнорируется в продакшн коде.
Во время разработки, <code>assert(<em>условие</em>)</code>
бросает исключение, если *условие* ложно. За подробностями, смотрите
[Assert](#assert).
</div>


### Final и const

Если вы никогда не хотите изменять переменную, используйте `final` или `const`,
либо вместо `var` ли в дополнение к типу. Финальные переменные могут быть заданы только один раз;
константные переменные - это константы времени компиляции. (Константные переменные неявно финальные).
Финальные глобальные переменные или переменные класса инициализируются при первом использовании.

<div class="alert alert-info" markdown="1">
**Замечание:**
Переменные экземпляра могут быть `final`, но не `const`.
Финальные переменные экземпляра должны быть инициализированы перед началом тела конструктора -
при объявлении переменной, параметром конструктора или в [списке инициализатора](#initializer-list) конструктора.
</div>

Здесь пример создания и установки финальных переменных:

<?code-excerpt "misc/lib/language_tour/variables.dart (final)"?>
{% prettify dart %}
final name = 'Bob'; // Без аннотации типа
final String nickname = 'Bobby';
{% endprettify %}

Вы не можете изменить значение финальной переменной:

{:.fails-sa}
<?code-excerpt "misc/lib/language_tour/variables.dart (cant-assign-to-final)"?>
{% prettify dart %}
name = 'Alice'; // Ошибка: финальная переменная может быть задана только один раз
{% endprettify %}

Используйте `const` для переменных, которые вы хотите сделать **константами времени компиляции**.
Если константная переменная задаётся в классе, пометьте её `static const`.
Там, где вы объявляете переменную, задайте значение константы времени компиляции, такое как
числовой или строковый литерал, константная переменная или результат арифметической операции над
константными числами:

<?code-excerpt "misc/lib/language_tour/variables.dart (const)"?>
{% prettify dart %}
const bar = 1000000; // Единица измерения давления (dynes/cm2)
const double atm = 1.01325 * bar; // Стандартное атмосферное
{% endprettify %}

Ключевое слово `const` предназначено не только для объявления константных переменных.
Вы также можете использовать её для создания константных _значений_,
а также объявить конструктор, который _созданёт_ константные значения.
Любая переменная может иметь константное значение.

<?code-excerpt "misc/lib/language_tour/variables.dart (const-vs-final)"?>
{% prettify dart %}
var foo = const [];
final bar = const [];
const baz = []; // Эквивалентно `const []`
{% endprettify %}

Вы можете опустить `const` в выражении инициализации при объявлении `const`,
как выше для `baz`, за подробностями, смотрите [DON’T use const redundantly][].

Вы можете изменить значение не финальной, не константной переменной,
даже если раннее оно имело константное значение:

<?code-excerpt "misc/lib/language_tour/variables.dart (reassign-to-non-final)"?>
{% prettify dart %}
foo = [1, 2, 3]; // Было const []
{% endprettify %}

Вы не можете изменить значение константной переменной:

{:.fails-sa}
<?code-excerpt "misc/lib/language_tour/variables.dart (cant-assign-to-const)"?>
{% prettify dart %}
baz = [42]; // Ошибка: Константным переменным нельзя присвоить значение.
{% endprettify %}

За большей информацией об использовании `const` для создания константных значений, смотрите
[Списки](#lists), [Мапы](#maps), и [Классы](#classes).


## Встроенные типы

Язык Dart имеет специальную поддержку для следующих типов:

- числа
- строки
- логические значения
- списки (также известные как *массивы*)
- мапы (также известные как *хеши* и *словари*)
- руны (для выражения символов Unicode в строки)
- символы

Вы можете инициализировать объект любым из этих специальных типов, используя литерал.
Например, `'this is a string'` - строковый литерал и `true` - логический литерал.

Так как каждая переменная в Dart ссылается на объект-экземпляр *класса*, вы можете
обычно использовать *конструкторы* для инициализации переменных. Некоторые из встроенных
типов имеют свои собственные конструкторы. Например, вы можете использовать
`Map()` конструктор для создания мапы.


### Числа

Числа в Dart бывают двух видов:

[int][]

:   Целоечисленные значения не больше 64 бит, зависящие от платформы.
    На Dart VM, значения могут быть от
    -2<sup>63</sup> до 2<sup>63</sup> - 1.
    Dart, который компилируется в JavaScript использует
    [JavaScript числа, ][js numbers]
    допустимые значения от -2<sup>53</sup> до 2<sup>53</sup> - 1.

[double][]

:   64 битные (двойной точности) числа с плавающей точкой,
    специфицированные стандартом IEEE 754.

`int` и `double` - подтипы [`num`.][num]
Тип num включает базовые операторы, такие как +, -, / и \*,
а также, где вы найдёте `abs()`, `ceil()` и `floor()` среди других методов.
(Побитовые операторы, такие как \>\>, определены в классе `int`)
Если num и его типы не имеют того, что вы ищите, вам может помочь библиотека [dart:math][].

Целые - это числа без десятичной точки, далее несколько примеров определения целых литералов:

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (integer-literals)"?>
{% prettify dart %}
var x = 1;
var hex = 0xDEADBEEF;
{% endprettify %}

Если число включает десятичную точку - это вещественное число (с плавающей точкой) типа `double`.
Далее несколько примеров определения вещественных литералов:

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (double-literals)"?>
{% prettify dart %}
var y = 1.1;
var exponents = 1.42e5;
{% endprettify %}

Начиная с Dart 2.1, целые литералы автоматически конвертируются в вещественные когда это необходимо:

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (int-to-double)"?>
{% prettify dart %}
double z = 1; // Эквивалентно double z = 1.0.
{% endprettify %}

<aside class="alert alert-info" markdown="1">
  **Замечание по версии:**
  До Dart 2.1, было ошибкой использовать
  целый литерал в вещественном контексте.
</aside>

Здесь показано, как преобразовать строку в число или наоборот:

<?code-excerpt "misc/test/language_tour/built_in_types_test.dart (number-conversion)"?>
{% prettify dart %}
// String -> int
var one = int.parse('1');
assert(one == 1);

// String -> double
var onePointOne = double.parse('1.1');
assert(onePointOne == 1.1);

// int -> String
String oneAsString = 1.toString();
assert(oneAsString == '1');

// double -> String
String piAsString = 3.14159.toStringAsFixed(2);
assert(piAsString == '3.14');
{% endprettify %}

Тип int поддерживает традиционные побитовые операторы сдвига (\<\<, \>\>),
побитовые логические операторы: И (&), ИЛИ (|).
Например:

<?code-excerpt "misc/test/language_tour/built_in_types_test.dart (bit-shifting)"?>
{% prettify dart %}
assert((3 << 1) == 6); // 0011 << 1 == 0110
assert((3 >> 1) == 1); // 0011 >> 1 == 0001
assert((3 | 4) == 7); // 0011 | 0100 == 0111
{% endprettify %}

Литералы чисел - константы времени компиляции.
Многие арифметические выражения также константы времени компиляции,
до тех пор, пока их операнды константы времени компиляции, которые вычисляются в числа.

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (const-num)"?>
{% prettify dart %}
const msPerSecond = 1000;
const secondsUntilRetry = 5;
const msUntilRetry = secondsUntilRetry * msPerSecond;
{% endprettify %}


### Строки

В Dart строки - это последовательность кодовых единиц UTF-16.
Вы можете использовать либо одиночные, либо двойные кавычки для создания строки:

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (quoting)"?>
{% prettify dart %}
var s1 = 'Одиночные ковычки отлично работают для строковых литералов.';
var s2 = "Двойные кавычки работают просто отлично.";
var s3 = 'Легко экранировать \'разделитель\' строки.';
var s4 = "Ещё проще использовать другой 'разделитель'.";
{% endprettify %}

Вы можете поместить значение выражения внутрь строки, используя
`${`*`выражение`*`}`. Если выражение - идентификатор, вы можете опустить {}.
Чтобы получить строковое представление объекта, Dart вызывает метод `toString()` у этого объекта.

<?code-excerpt "misc/test/language_tour/built_in_types_test.dart (string-interpolation)"?>
{% prettify dart %}
var s = 'интерполяция строки';

assert('У Dart есть $s, которая очень проста.' ==
    'У Dart есть интерполяция строки, которая очень проста.');
assert('Это заслуживает больших букв. ' +
        '${s.toUpperCase()} очень проста!' ==
    'Это заслуживает больших букв. ИНТЕРПОЛЯЦИЯ СТРОКИ очень проста!');
{% endprettify %}

<div class="alert alert-info" markdown="1">
**Замечание:**
Оператор `==` проверяет два объекта на эквивалентность. Две строки
эквивалентны, если они содержат одинаковую последовательность кодовых единиц.
</div>

Вы можете конкатенировать строки, используя смежный литерал строки или
оператор `+`:

<?code-excerpt "misc/test/language_tour/built_in_types_test.dart (adjacent-string-literals)"?>
{% prettify dart %}
var s1 = 'Конкатенация '
    'строк'
    " работает даже с разрывами строк.";
assert(s1 ==
    'Конкатенация строк'
    ' работает даже с разрывами строк.');

var s2 = 'Оператор + ' + 'работает отлично.';
assert(s2 == 'Оператор + работает отлично.');
{% endprettify %}

Другой путь создания многострочных строк:
использовать тройные кавычки (три одиночные или из три двойные):

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (triple-quotes)"?>
{% prettify dart %}
var s1 = '''
Вы можете создать
многострочную строку как эта.
''';

var s2 = """Это также
многострочная строка.""";
{% endprettify %}

Вы можете создать "сырую" строку с помощью префикса `r`:

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (raw-strings)"?>
{% prettify dart %}
var s = r'В сырой строки, даже \n не обрабатывается.';
{% endprettify %}

Смотрите [Руны](#runes) чтобы узнать как представить символы Unicode в строку.

Литеральные строки являются константами времени компиляции,
если любое интерполированное выражение является константой времени компиляции,
и относится к null, числам, строке или логическому значению.

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (string-literals)"?>
{% prettify dart %}
// Эти работают в константных строках
const aConstNum = 0;
const aConstBool = true;
const aConstString = 'константная строка';

// Эти не работают в константных строках
var aNum = 0;
var aBool = true;
var aString = 'строка';
const aConstList = [1, 2, 3];

const validConstString = '$aConstNum $aConstBool $aConstString';
// const invalidConstString = '$aNum $aBool $aString $aConstList';
{% endprettify %}

Более подробную информацию об использовании строк, смотрите
[Строки и регулярные выражения](/guides/libraries/library-tour#strings-and-regular-expressions).


### Логические значения

Для представления логических значений, у Dart сть тип, называемый `bool`.
Только два объекта имеют тип bool: логические литералы `true` и `false`,
которые оба константы времени компиляции.

Безопасность типов в Dart не даёт использовать такой код, как
<code>if (<em>неЛогическоеЗначение</em>)</code> или
<code>assert (<em>неЛогическоеЗначение</em>)</code>.

Вместо этого, следует использовать явную проверку значений, как тут:

<?code-excerpt "misc/test/language_tour/built_in_types_test.dart (no-truthy)"?>
{% prettify dart %}
// Проверка строки на пустоту.
var fullName = '';
assert(fullName.isEmpty);

// Проверка на ноль.
var hitPoints = 0;
assert(hitPoints <= 0);

// Проверка на null.
var unicorn;
assert(unicorn == null);

// Проверка на NaN.
var iMeantToDoThis = 0 / 0;
assert(iMeantToDoThis.isNaN);
{% endprettify %}


### Списки

Возможно наиболее распространёная коллекция почти в каждом языке программирования - *массив* или
упорядоченная группа объектов. В Dart массив - это [Список][] объектов, так что большинство людей
называют их просто *списки*.

В Dart литерал списка выглядит также, как в JavaScript литерал массива.
Здесь пример списка в Dart:

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (list-literal)"?>
{% prettify dart %}
var list = [1, 2, 3];
{% endprettify %}

<aside class="alert alert-info" markdown="1">
  **Замечание:**
  Анализатор выведет, что `list` имеет тип `List<ing>`.
  Если вы попробуете добавить не целочисленный объект в этот список,
  анализатор сообщит об ошибке или ошибка будет выдана во время исполнения.
  Для большей информацией, читайте о [выводе типов](/guides/language/sound-dart#type-inference).
</aside>

Списки используют индексацию с нуля, когда 0 - индекс первого элемента и
`list.length - 1` - индекс последнего. Вы можете получить длинну списка и сослаться
на элемент списка также просто, как в JavaScript:

<?code-excerpt "misc/test/language_tour/built_in_types_test.dart (list-indexing)"?>
{% prettify dart %}
var list = [1, 2, 3];
assert(list.length == 3);
assert(list[1] == 2);

list[1] = 1;
assert(list[1] == 1);
{% endprettify %}

Для создания списка, который будет константой времени компиляции,
добавте `const` перед литералом списка:

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (const-list)"?>
{% prettify dart %}
var constantList = const [1, 2, 3];
// constantList[1] = 1; // Раскомментировав это, получите ошибку.
{% endprettify %}

Тип List имеет много простых методов для работы со списками.
Для получения большей информации о списках, смотрите [Обобщения](#generics) и
[Коллекции](/guides/libraries/library-tour#collections).


### Мапы

В общем, мапа - это объект, в котором ключи, ассоциированы со значением.
Ключи и значения могут быть объектами любого типа. Каждый *ключ* встречается только один раз,
но вы можете использовать некоторые *значения* множество раз. Dart поддерживает
литерал мап и тип [Map][].

Здесь пара простых мап на Dart, созданных, используя литерал мап:

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (map-literal)"?>
{% prettify dart %}
var gifts = {
  // Key:    Value
  'first': 'partridge',
  'second': 'turtledoves',
  'fifth': 'golden rings'
};

var nobleGases = {
  2: 'helium',
  10: 'neon',
  18: 'argon',
};
{% endprettify %}

<aside class="alert alert-info" markdown="1">
  **Замечание:**
  Анализатор выведет, что `gifts` имеет тип `Map<String, String>` и
  `nobleGases` имеет тип `Map<int, String>`. Если вы попробуете добавить
  значение неправильного типа в эту мапу,
  анализатор сообщит об ошибке или ошибка будет выдана во время исполнения.
  За большей информацией, читайте о [выводе типов](/guides/language/sound-dart#type-inference).
</aside>

Вы можете создать некоторые объекты, используя конструктор мап:

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (map-constructor)"?>
{% prettify dart %}
var gifts = Map();
gifts['first'] = 'partridge';
gifts['second'] = 'turtledoves';
gifts['fifth'] = 'golden rings';

var nobleGases = Map();
nobleGases[2] = 'helium';
nobleGases[10] = 'neon';
nobleGases[18] = 'argon';
{% endprettify %}

<aside class="alert alert-info" markdown="1">
**Замечание:**
Возможно вы заметили вместо `new Map()` просто `Map()`.
Начиная с Dart 2, ключевое слово `new` опционально.
За деталями, смотрите [Использование конструкторов](#using-constructors).
</aside>

Добавить новую пару ключ-значение в существующую мапу просто как и в JavaScript:

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (map-add-item)"?>
{% prettify dart %}
var gifts = {'first': 'partridge'};
gifts['fourth'] = 'calling birds'; // Добавление новой пары ключ-значение
{% endprettify %}

Получение значения из мапы выглядит похоже, как в JavaScript:

<?code-excerpt "misc/test/language_tour/built_in_types_test.dart (map-retrieve-item)"?>
{% prettify dart %}
var gifts = {'first': 'partridge'};
assert(gifts['first'] == 'partridge');
{% endprettify %}

Если ключа нет в мапе, вы получите null:

<?code-excerpt "misc/test/language_tour/built_in_types_test.dart (map-missing-key)"?>
{% prettify dart %}
var gifts = {'first': 'partridge'};
assert(gifts['fifth'] == null);
{% endprettify %}

Используйте `.length` для получения количества пар ключ-значение в мапе:

<?code-excerpt "misc/test/language_tour/built_in_types_test.dart (map-length)"?>
{% prettify dart %}
var gifts = {'first': 'partridge'};
gifts['fourth'] = 'calling birds';
assert(gifts.length == 2);
{% endprettify %}

Чтобы создать мапу, которая будет константой времени компиляции,
добавте перед литералом мапы `const` :

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (const-map)"?>
{% prettify dart %}
final constantMap = const {
  2: 'helium',
  10: 'neon',
  18: 'argon',
};

// constantMap[2] = 'Helium'; // Раскомментировав это, получите ошибку
{% endprettify %}

За большей информацией, смотрите
[Обобщения](#generics) и
[Мапы](/guides/libraries/library-tour#maps).


### Руны

В Dart руны - это строки из кодовых точек UTF-32.

Unicode определяет уникальное числовое значение для каждой буквы,
цифры и символа, используемых во всех мировых системах письма.
Так как в Dart строки - последовательность кодовых единиц UTF-16,
вычисление 32 битных Unicode значений в строках требует специального синтаксиса.

Обычный способ для вычисления кодовых точек Unicode - `\uXXXX`,
где XXXX - шестнадцатеричное значение из 4 цифр.
Например, символ сердца (♥) - `\u2665`.
Чтобы указать больше или меньше, чем 4 шестнадцатеричные цифры,
поместите значение в фигурные скобки.
Например, смеющийся смайлик (😆) - `\u{1f600}`.

У Класса [String][]
несколько свойств могут извлекать информацию о рунах.
Свойства `codeUnitAt` и `codeUnit` возвращают 16 битные кодовые единицы.
Используйте свойство `runes`, чтобы получить руну как строку.

Следующий пример иллюстрирует взаимосвязь между рунами 16 битных кодовых единиц и
32 битных кодовых единиц.
Нажмите на кнопку запуска {% asset red-run.png alt="" %}, чтобы увидеть руны в действии.

{% comment %}
https://gist.github.com/589bc5c95318696cefe5
https://dartpad.dartlang.org/589bc5c95318696cefe5
Unicode emoji: http://unicode.org/emoji/charts/full-emoji-list.html

<?code-excerpt "misc/lib/language_tour/built_in_types.dart (runes)"?>
{% prettify dart %}
void main() {
  var clapping = '\u{1f44f}';
  print(clapping);
  print(clapping.codeUnits);
  print(clapping.runes.toList());

  Runes input =
      Runes('\u2665  \u{1f605}  \u{1f60e}  \u{1f47b}  \u{1f596}  \u{1f44d}');
  print(String.fromCharCodes(input));
}
{% endprettify %}
{% endcomment %}

<iframe
src="{{site.custom.dartpad.embed-inline-prefix}}?id=589bc5c95318696cefe5&verticalRatio=65"
    width="100%"
    height="333px"
    style="border: 1px solid #ccc;">
</iframe>

<div class="alert alert-warning" markdown="1">
**Замечание:**
Будьте осторожны, когда обрабатываете руны, используя операции списков.
Этот подход может легко сломаться, в зависимости от конкретного языка, набора символов и операций.
Больше информации смотрите в [Как мне перевернуть строку в Dart?](http://stackoverflow.com/questions/21521729/how-do-i-reverse-a-string-in-dart) на Stack Overflow.
</div>

### Символы

Объекты [Symbol][]
представляют оператор или идентификатор в программе на Dart.
Возможно вам никогда не нужно будет использовать символы, но они бесцены
для API, чтобы ссылаться на идентификаторы по именам, так как минификация
изменяет имена идентификаторов, но не изменяет имена идентификаторов символов.

Чтобы получить символ для идентификатора,
используйте символьный литерал, который является просто #,
за которым следует идентификатор:

```nocode
#radix
#bar
```

Литералы символов - константы времени компиляции.


## Функции

Dart - настоящий объектно-ориентированный язык, так что каждая функция - объект
и имеет тип [Function.][Function API reference]
Функцию можно присвоить переменной или передать параметром в другую функцию.
Вы можете также вызвать экземпляр некоторого класса как функцию.
За подробностями, смотрите [Вызываемые классы](#callable-classes).

Здесь пример реализации функции:

<?code-excerpt "misc/lib/language_tour/functions.dart (function)"?>
{% prettify dart %}
bool isNoble(int atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}
{% endprettify %}

Хотя в Dart и рекомендуется использовать [аннотацию типов для публичных API](/guides/language/effective-dart/design#prefer-type-annotating-public-fields-and-top-level-variables-if-the-type-isnt-obvious),
функция всё ещё будет работать, если вы опустите типы:

<?code-excerpt "misc/lib/language_tour/functions.dart (function-omitting-types)"?>
{% prettify dart %}
isNoble(atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}
{% endprettify %}

Для функций, которые содеражит одно простое выражение, вы можете использовать
сокращённый синтаксис:

<?code-excerpt "misc/lib/language_tour/functions.dart (function-shorthand)"?>
{% prettify dart %}
bool isNoble(int atomicNumber) => _nobleGases[atomicNumber] != null;
{% endprettify %}

Такой синтаксис <code>=> <em>выражение</em></code> - сокращённая форма
<code>{ return <em>выражение</em>; }</code>. Синтаксис с аннотацией `=>` иногда называют
_стрелочным_.

<div class="alert alert-info" markdown="1">
**Замечание:**
Только *выражение*, но не *инструкция*,
может быть помещено между стрелкой (=\>) и точкой с запятой (;)
Например, вы не можете поместить [инструкцию if](#if-and-else) туда, но
вы можете воспользоваться [условным выражением](#conditional-expressions).
</div>

У функции может быть два типа параметров: обязательные и опциональные.
Обязательные параметры следуют первыми, далее любые опциональные параметры.
Именованные опциональные параметры могут также быть помечены как `@required`.
Смотрите следующий раздел для подробностей.


### Опциональные параметры

Опциональные параметры могут быть либо позиционными, либо именованными, но не обоими сразу.

#### Опциональные именованные параметры

Там, где вызывается функция, вы можете указать имя параметра, используя
<code><em>имяПараметра</em>: <em>значение</em></code>. Например:

<?code-excerpt "misc/lib/language_tour/functions.dart (use-named-parameters)"?>
{% prettify dart %}
enableFlags(bold: true, hidden: false);
{% endprettify %}

Когда определяете функцию, используйте
<code>{<em>параметр1</em>, <em>параметр2</em>, …}</code>
для указания именованных параметров:

<?code-excerpt "misc/lib/language_tour/functions.dart (specify-named-parameters)"?>
{% prettify dart %}
/// Устанавливает флаги [bold] и [hidden] ...
void enableFlags({bool bold, bool hidden}) {...}
{% endprettify %}

У [Flutter][] выражение создания экземпляра может быть сложным,
поэтому конструкторы виджетов используют в основном именованные параметры.
Это делает выражение создание экземпляра простым для чтения.

Вы можете аннотировать именованный параметр в любом Dart коде
(не только в Flutter) с помощью [@required][],
чтобы сообщить, что это _обязательный_ параметр. Например:

<?code-excerpt "misc/lib/language_tour/functions.dart (required-named-parameters)" replace="/@required/[!$&!]/g"?>
{% prettify dart %}
const Scrollbar({Key key, [!@required!] Widget child})
{% endprettify %}

Когда `Scrollbar` создан, анализатор сообщит о проблеме, где
`дочерний` аргумент пропущен.

[Required][@required] определён в пакете [meta][]. Либо импортируется целенаправленно
`package:meta/meta.dart`, либо импортиртируется другой пакет, который экспортирует
`meta`, такой как у Flutter `package:flutter/material.dart`.

#### Опциональные позиционные параметры

Оберните множество параметров функции в `[]`,
пометив их тем самым как опциональные позиционные параметры:

<?code-excerpt "misc/test/language_tour/functions_test.dart (optional-positional-parameters)"?>
{% prettify dart %}
String say(String from, String msg, [String device]) {
  var result = '$from сказал $msg';
  if (device != null) {
    result = '$result с помощью $device';
  }
  return result;
}
{% endprettify %}

Здесь пример вызова этой фукнции без опциональных параметров:

<?code-excerpt "misc/test/language_tour/functions_test.dart (call-without-optional-param)"?>
{% prettify dart %}
assert(say('Bob', 'Howdy') == 'Bob сказал Howdy');
{% endprettify %}

А здесь пример вызова этой функции с этим параметром:

<?code-excerpt "misc/test/language_tour/functions_test.dart (call-with-optional-param)"?>
{% prettify dart %}
assert(say('Bob', 'Howdy', 'дымового сигнала') ==
    'Bob сказал Howdy с помощью дымового сигнала');
{% endprettify %}

<a id="default-parameters"></a>
#### Значение параметров поумолчанию

Ваши функции могут использовать `=` для определения значений поумолчанию для
именованных и позиционных параметров. Значения поумолчанию должны быть константами
времени компиляции. Если не предоставлено значение поумолчанию, исходное значение `null`.

Здесь пример установки значений поумолчанию для именованых параметров:

<?code-excerpt "misc/lib/language_tour/functions.dart (named-parameter-default-values)"?>
{% prettify dart %}
/// Установить флаги [bold] и [hidden] ...
void enableFlags({bool bold = false, bool hidden = false}) {...}

// bold будет true; hidden будет false.
enableFlags(bold: true);
{% endprettify %}

<div class="alert alert-info" markdown="1">
**Неодобрение:**
Старый код может использовать (`:`) вместо `=`
для установки значений поумолчанию именованых параметров.
Первоначально предполагалось, что для именованных параметров
будет поддерживаться только `:`.
Сейчас его использование осуждается, так что мы рекомендуем вам
**[использовать `=` для указания значений поумолчанию.](/tools/pub/pubspec#sdk-constraints)**
</div>

Следующий пример показывает, как задать значения поумолчанию для позиционных параметров:

<?code-excerpt "misc/test/language_tour/functions_test.dart (optional-positional-param-default)"?>
{% prettify dart %}
String say(String from, String msg,
    [String device = 'carrier pigeon', String mood]) {
  var result = '$from says $msg';
  if (device != null) {
    result = '$result with a $device';
  }
  if (mood != null) {
    result = '$result (in a $mood mood)';
  }
  return result;
}

assert(say('Bob', 'Howdy') ==
    'Bob says Howdy with a carrier pigeon');
{% endprettify %}

Вы также можете вставить списки или мапы как значения поумолчанию
В следующем примере определяется функция `doStuff()`,
которая указывает список поумолчанию для параметра `list`
и мапу поумолчанию для параметра `gifts`.
{% comment %}
Нажмите кнопку запустить
{% asset red-run.png alt="" %}
чтобы увидеть список и мапу как значения поумолчанию в действии.
{% endcomment %}

<?code-excerpt "misc/lib/language_tour/functions.dart (list-map-default-function-param)"?>
{% prettify dart %}
void doStuff(
    {List<int> list = const [1, 2, 3],
    Map<String, String> gifts = const {
      'first': 'paper',
      'second': 'cotton',
      'third': 'leather'
    }}) {
  print('list:  $list');
  print('gifts: $gifts');
}
{% endprettify %}

{% comment %}
https://gist.github.com/d988cfce0a54c6853799
https://dartpad.dartlang.org/d988cfce0a54c6853799
(The gist needs updating: see https://github.com/dart-lang/site-www/issues/189)
<iframe
src="{{site.custom.dartpad.embed-inline-prefix}}?id=d988cfce0a54c6853799&verticalRatio=70"
    width="100%"
    height="450px"
    style="border: 1px solid #ccc;">
</iframe>
{% endcomment %}


### Функция main()

Каждое приложение должно определять глобальную функцию `main()`,
которая будет входной точкой для приложения. Функция `main()` возвращает `void`
и имеет опциональный параметр `List<String>` для аргументов командной строки.

Здесь пример функции `main()` для web приложения:

<?code-excerpt "misc/test/language_tour/browser_test.dart (simple-web-main-function)"?>
{% prettify dart %}
void main() {
  querySelector('#sample_text_id')
    ..text = 'Нажми на меня!'
    ..onClick.listen(reverseText);
}
{% endprettify %}

<div class="alert alert-info" markdown="1">
**Замечание:**
Синтаксис `..` в предшествующем коде называется [каскадом](#cascade-notation-).
С каскадом, вы можете выполнять множество операций над членами одного объекта.
</div>

Здесь пример функции `main()` для приложения командной строки, которая получает аргументы:

<?code-excerpt "misc/test/language_tour/functions_test.dart (main-args)"?>
{% prettify dart %}
// Запустите программу так: dart args.dart 1 test
void main(List<String> arguments) {
  print(arguments);

  assert(arguments.length == 2);
  assert(int.parse(arguments[0]) == 1);
  assert(arguments[1] == 'test');
}
{% endprettify %}

Вы можете использовать [библиотеку args](https://pub.dartlang.org/packages/args)
для определения и разбора аргументов командной строки
define and parse command-line arguments.

### Функции как объекты первого порядка

Вы можете вставить функцию как параметр другой функции. Например:

<?code-excerpt "misc/lib/language_tour/functions.dart (function-as-param)"?>
{% prettify dart %}
void printElement(int element) {
  print(element);
}

var list = [1, 2, 3];

// Вставка printElement как параметра.
list.forEach(printElement);
{% endprettify %}

Вы также можете присвоить функцию переменной, как тут:

<?code-excerpt "misc/test/language_tour/functions_test.dart (function-as-var)"?>
{% prettify dart %}
var loudify = (msg) => '!!! ${msg.toUpperCase()} !!!';
assert(loudify('hello') == '!!! HELLO !!!');
{% endprettify %}

Это пример использования анонимной фукнции.
Больше о об этом в следующем разделе.

### Анонимные функции

Большинство функций - именованые. такие как `main()` или `printElement()`.
Вы можете также создать безымянную функцию, называемую _анонимной функцией_ или
по другому _лямбда_ или _замыкание_.
Вы можете присвоить анонимную функцию переменной, чтобы,
например, вы могли добавить или удалить ее из коллекции.

Анонимная функция выглядит похоже на именованную функцию&mdash;
с нулём или большим числом параметров, разделённх запятыми и
опциональной анотацией типов между круглыми скобками.

Следующий за ней блок кода содержит тело функции:

<code>
([[<em>Type</em>] <em>параметр1</em>[, …]]) { <br>
&nbsp;&nbsp;<em>блокКода</em>; <br>
}; <br>
</code>

Следующий пример определяет анонимную функцию с нетипизированным параметром `item`.
Функция вызывается для каждого элемента списка,
печатает строку, которая включает значение по и соответсвтвующий индекс.

<?code-excerpt "misc/test/language_tour/functions_test.dart (anonymous-function)"?>
{% prettify dart %}
var list = ['apples', 'bananas', 'oranges'];
list.forEach((item) {
  print('${list.indexOf(item)}: $item');
});
{% endprettify %}

Нажмите кнопку запуска {% asset red-run.png alt="" %} чтобы выполнить этот код.

{% comment %}
https://gist.github.com/chalin/5d70bc1889d055c7a18d35d77874af88
https://dartpad.dartlang.org/5d70bc1889d055c7a18d35d77874af88
{% endcomment %}

<iframe
src="{{site.custom.dartpad.embed-inline-prefix}}?id=5d70bc1889d055c7a18d35d77874af88&verticalRatio=60"
    width="100%"
    height="250px"
    style="border: 1px solid #ccc;">
</iframe>

Если функция содержит только одно выражение, вы можете использовать
короткую стрелочную запись. Вставте следующую строку в DartPad и нажмите
запустить, чтобы проверить, что эта функциональность эквивалентна.

<?code-excerpt "misc/test/language_tour/functions_test.dart (anon-func)"?>
{% prettify dart %}
list.forEach(
    (item) => print('${list.indexOf(item)}: $item'));
{% endprettify %}


### Лексическая область видимости

Dart - язык с лексической области видимости, которая означает, что
область видимости переменой задаётся статично, просто расположением в коде.
Вы можете "проследовать за фигурными скобками наружу", чтобы увидеть, есть ли переменная в
области видимости.

Здесь пример вложенных функций с переменными на каждом уровне области видимости:

<?code-excerpt "misc/test/language_tour/functions_test.dart (nested-functions)"?>
{% prettify dart %}
bool topLevel = true;

void main() {
  var insideMain = true;

  void myFunction() {
    var insideFunction = true;

    void nestedFunction() {
      var insideNestedFunction = true;

      assert(topLevel);
      assert(insideMain);
      assert(insideFunction);
      assert(insideNestedFunction);
    }
  }
}
{% endprettify %}

Обратите внимание, как функция `nestedFunction()` может использовать переменные из каждого уровня
области видимости, вплоть до самого верхнего - глобального.


### Лексические замыкания

*Замыкание* - это функциональный объект, который имеет доступ к переменным в своей
лексической области видимости, даже когда функция используется вне своей исходной
области видимости.

Функции могут перекрывать переменные, определённые в окружающих областях видимости.
В следующем примере `makeAdder()` захватывает переменную `addBy`. Куда бы
не возвращалась функция, она запоминает `addBy`.

<?code-excerpt "misc/test/language_tour/functions_test.dart (function-closure)"?>
{% prettify dart %}
/// Возвращает функцию, которая добавляет [addBy] к аргументу функции
Function makeAdder(num addBy) {
  return (num i) => addBy + i;
}

void main() {
  // Создание функции, которая прибавляет 2.
  var add2 = makeAdder(2);
  // Создание функции, которая прибавляет 4.
  var add4 = makeAdder(4);
  assert(add2(3) == 5);
  assert(add4(3) == 7);
}
{% endprettify %}


### Тестирование функций на равенство

Здесь пример тестирования глобальных функций, статических методов и
методов экземпляра на равенство

<?code-excerpt "misc/lib/language_tour/function_equality.dart"?>
{% prettify dart %}
void foo() {} // Глобальная функция

class A {
  static void bar() {} // Статический метод
  void baz() {} // Метод экземпляра
}

void main() {
  var x;

  // Сравнение глобальных функций
  x = foo;
  assert(foo == x);

  // Сравнение статических методов
  x = A.bar;
  assert(A.bar == x);

  // Сравнение методов экземпляра
  var v = A(); // Экземпляр A #1
  var w = A(); // Экземпляр A #2
  var y = w;
  x = w.baz;

  // Эти замыкания ссылаются на один и тот же экземпляр (#2),
  // поэтому они равны.
  assert(y.baz == x);

  // Эти замыкания ссылаются на разные экземпляры, поэтому они не равны.
  assert(v.baz != w.baz);
}
{% endprettify %}


### Возврат значений

Все функции возвращают занчение. Если возвращаемое значение не указано,
инструкция `return null` неявно добавляется в тело функции.

<?code-excerpt "misc/test/language_tour/functions_test.dart (implicit-return-null)"?>
{% prettify dart %}
foo() {}

assert(foo() == null);
{% endprettify %}


## Operators

В Dart определены операторы, показаные в следующей таблице.
Вы можете переопределить некоторые из этих операторов, описаные в
[Переопределяемые операторы](#overridable-operators).

|--------------------------+------------------------------------------------|
|Описание                  | Оператор                                       |
|--------------------------|------------------------------------------------|
| унарный постфиксный      | <code><em>expr</em>++</code>    <code><em>expr</em>--</code>    `()`    `[]`    `.`    `?.` |
| унарный префиксный             | <code>-<em>expr</em></code>    <code>!<em>expr</em></code>    <code>~<em>expr</em></code>    <code>++<em>expr</em></code>    <code>--<em>expr</em></code>   |
| мультипликативный           | `*`    `/`    `%`    `~/`                      |
| аддитивный                 | `+`    `-`                                     |
| свдиг                    | `<<`    `>>`                                   |
| побитовый И            | `&`                                            |
| побитовый Исключающий ИЛИ              | `^`                                            |
| побитовый ИЛИ               | `|`                                            |
| отношения&nbsp;и&nbsp;проверка&nbsp;типов | `>=`    `>`    `<=`    `<`    `as`    `is`    `is!` |
| равенство                 | `==`    `!=`                                   |
| логический И              | `&&`                                           |
| логический ИЛИ               | `||`                                           |
| если null                 | `??`                                           |
| сравнение              | <code><em>expr1</em> ? <em>expr2</em> : <em>expr3</em></code> |
| каскад                  | `..`                                           |
| присваивание               | `=`    `*=`    `/=`    `~/=`    `%=`    `+=`    `-=`    `<<=`    `>>=`    `&=`    `^=`    `|=`    `??=` |
{:.table .table-striped}

Когда вы используете операторы, вы создаёте выражения. Здесь несколько
примеров выражений с операторами:

<?code-excerpt "misc/test/language_tour/operators_test.dart (expressions)" replace="/,//g"?>
{% prettify dart %}
a++
a + b
a = b
a == b
c ? a : b
a is T
{% endprettify %}

В [таблице операторов](#operators),
строки с операторами располагаются в порядке убывания приоритетов.
Например, мультипликативный оператор `%` имеет более высокий приоритет (исполнится раньше) чем оператор
равенства `==`, который в свою очередь имеет более высокий приоритет, чем логический оператор И `&&`.
Это означает, что следующие строки кода исполняются одинаково:

<?code-excerpt "misc/test/language_tour/operators_test.dart (precedence)"?>
{% prettify dart %}
// Круглые скобки улучшают читабельность.
if ((n % i == 0) && (d % i == 0)) ...

// Тяжелее для чтения, но аналогично предыдущему
if (n % i == 0 && d % i == 0) ...
{% endprettify %}

<div class="alert alert-warning" markdown="1">
**Предупреждение:**
Для операторов, которые работают с двумя операндами, крайний слева
операнд определяет какая версия оператора используется Например, если у вас
есть объект Vector (вектор) и объект Point (точка), `aVector + aPoint` использует
версию оператора + из Vector.
</div>


### Арифметические операторы

Dart поддерживает обычные арифметические операторы, которые показаны
в следующей таблице.

|-----------------------------+-------------------------------------------|
| Оператор                    | Смысл                                   |
|-----------------------------+-------------------------------------------|
| `+`                         | Сложение
| `–`                         | Вычитание
| <code>-<em>expr</em></code> | Унарный минус (меняет знак числа)
| `*`                         | Умножение
| `/`                         | Деление
| `~/`                        | Деление, которое возвращает целую часть от деления
| `%`                         | Остаток от деления
{:.table .table-striped}

Примеры:

<?code-excerpt "misc/test/language_tour/operators_test.dart (arithmetic)"?>
{% prettify dart %}
assert(2 + 3 == 5);
assert(2 - 3 == -1);
assert(2 * 3 == 6);
assert(5 / 2 == 2.5); // Результат типа double
assert(5 ~/ 2 == 2); // Результат типа int
assert(5 % 2 == 1); // Остаток

assert('5/2 = ${5 ~/ 2} r ${5 % 2}' == '5/2 = 2 r 1');
{% endprettify %}

Dart также поддерживает операторы префиксного и постфиксного инкремента.

|-----------------------------+-------------------------------------------|
| Оператор                    | Смысл                                   |
|-----------------------------+-------------------------------------------|
| <code>++<em>var</em></code> | <code><em>var</em> = <em>var</em> + 1</code> (вычисленное значение - <code><em>var</em> + 1</code>)
| <code><em>var</em>++</code> | <code><em>var</em> = <em>var</em> + 1</code> (вычисленное значение -  <code><em>var</em></code>)
| <code>--<em>var</em></code> | <code><em>var</em> = <em>var</em> – 1</code> (вычисленное значение - <code><em>var</em> – 1</code>)
| <code><em>var</em>--</code> | <code><em>var</em> = <em>var</em> – 1</code> (вычисленное значение - <code><em>var</em></code>)
{:.table .table-striped}

Примеры:

<?code-excerpt "misc/test/language_tour/operators_test.dart (increment-decrement)"?>
{% prettify dart %}
var a, b;

a = 0;
b = ++a; // Инкремент a перед тем как b получит значение.
assert(a == b); // 1 == 1

a = 0;
b = a++; // Сначала b получает значение a, зачем происходит инкремент
assert(a != b); // 1 != 0

a = 0;
b = --a; // Декремент a перед тем как b получит значение.
assert(a == b); // -1 == -1

a = 0;
b = a--; // Сначала b получает значение a, зачем происходит декремент
assert(a != b); // -1 != 0
{% endprettify %}


### Операторы равенства и отношений

The following table lists the meanings of equality and relational operators.

|-----------+-------------------------------------------|
| Оператор  | Смысл                                   |
|-----------+-------------------------------------------|
| `==`      |       Равно, смотрите обсуждение ниже
| `!=`      |       Не равно
| `>`       |       Больше
| `<`       |       Меньше
| `>=`      |       Больше или равно
| `<=`      |       Меньше или равно
{:.table .table-striped}

Чтобы проверить, представляют ли два объекта x и y одно и то же,
используйте оператор `==`. (В редких случаях, когда нужно узнать, представляют ли два объекта
один и тот же объект, используйте функцию [identical()][]).
Далее расказано, как работает оператор `==`:


1.  Если *x* и  *y* равны null, вернёт true, иначе если только один из них равен null, вернёт false.

2.  Возвращает результат вызова метода.
    <code><em>x</em>.==(<em>y</em>)</code>.
    Такие операторы, как `==` - методы, вызываемые из первого операнда.
    Вы даже можете переопределить некоторые операторы, включая `==`, как вы могли увидеть
    [Переопределяемые операторы](#overridable-operators).

Здесь примеры использования каждого из операторов равенства и отношения:

<?code-excerpt "misc/test/language_tour/operators_test.dart (relational)"?>
{% prettify dart %}
assert(2 == 2);
assert(2 != 3);
assert(3 > 2);
assert(2 < 3);
assert(3 >= 3);
assert(2 <= 3);
{% endprettify %}


### Операторы проверки типов


Операторы `as`, `is`, и `is!` просто проверяют типы во время исполнения.

|-----------+-------------------------------------------|
| Оператор  | Смысл                                   |
|-----------+-------------------------------------------|
| `as`      | Приведение типа (также используется для указания [префиксов библиотек](#specifying-a-library-prefix))
| `is`      | true, если объект имеет указанный тип
| `is!`     | false, если объект не имеет указанный тип
{:.table .table-striped}

Результат `obj is T` true, если `obj` реализует интерфейс, указанный в `T`.
Например, `obj is Object` всегда true.

Используйте оператор `as` для прведения объекта к конкретному типу.
В общем, вы должны использовать его как сокращение проверки `is`
для объекта, за которым следует выражение, использующее этот объект.
Например, рассмотрим следующий код:

<?code-excerpt "misc/lib/language_tour/classes/employee.dart (emp is Person)"?>
{% prettify dart %}
if (emp is Person) {
  // Проверка типа
  emp.firstName = 'Bob';
}
{% endprettify %}

Вы можете сделать код короче, используя оператор `as`:

<?code-excerpt "misc/lib/language_tour/classes/employee.dart (emp as Person)"?>
{% prettify dart %}
(emp as Person).firstName = 'Bob';
{% endprettify %}

<div class="alert alert-info" markdown="1">
**Замечание:**
Приведённые выше два кода не эквивалентны.
Если `emp` - null или не Person, то первый пример (с `is`) ничего не делает,
второй (с `as`) бросит исключение.
</div>


### Операторы присваивания

Как вы уже видели, вы можете присваивать значения, используя оператор `=`.
Используйте оператор `??=`, если вы хотите присвоить значение переменной только
когда она равна null.

<?code-excerpt "misc/test/language_tour/operators_test.dart (assignment)"?>
{% prettify dart %}
// Присваивание значения переменной a
a = value;
// Присвоить значение b, если b равна null; в остальных случаях, b остаётся прежней
b ??= value;
{% endprettify %}

{% comment %}
<!-- embed a dartpad when we can hide code -->
https://gist.github.com/9de887c4daf76d39e524
https://dartpad.dartlang.org/9de887c4daf76d39e524

<?code-excerpt "misc/test/language_tour/operators_test.dart (assignment-gist-main-body)" plaster="none"?>
{% prettify dart %}
void assignValues(int a, int b, int value) {
  print('Initially: a == $a, b == $b');
  // Assign value to a
  a = value;
  // Assign value to b if b is null; otherwise, b stays the same
  b ??= value;
  print('After: a == $a, b == $b');
}

main() {
  assignValues(0, 0, 1);
  assignValues(null, null, 1);
}
{% endprettify %}
{% endcomment %}

Составные операторы присваивания, такие как `+=`,
объединяют операцию с присваиванием.

| `=`  | `–=` | `/=`  | `%=`  | `>>=` | `^=`
| `+=` | `*=` | `~/=` | `<<=` | `&=`  | `|=`
{:.table}

Здесь показано, как работают операторы присваивания:

|-----------+----------------------+-----------------------|
|           | Составное присваивание  | Эквивалентное выражение |
|-----------+----------------------+-----------------------|
|**Для оператора <em>оп</em>:** | <code>a <em>оп</em>= b</code> | <code>a = a <em>оп</em> b</code>
|**Пример:**                     |`a += b`                       | `a = a + b`
{:.table}

Следующий пример использует  обычное присваивание и составные операторы присваивания:

<?code-excerpt "misc/test/language_tour/operators_test.dart (op-assign)"?>
{% prettify dart %}
var a = 2; // Присваивание, используя =
a *= 3; // Присвоить и умножить: a = a * 3
assert(a == 6);
{% endprettify %}


### Логические операторы

Вы можете инвертировать или комбинировать логические выражения, используя
логические операторы:

|-----------------------------+-------------------------------------------|
| Оператор                    | Смысл                                   |
|-----------------------------+-------------------------------------------|
| <code>!<em>выражение</em></code> | инвертирует следующее выражение (изменяет false на true и наоборот)
| `||`                        | логическое ИЛИ
| `&&`                        | логическое И
{:.table .table-striped}

Далее пример использования логических операторов:

<?code-excerpt "misc/lib/language_tour/operators.dart (op-logical)"?>
{% prettify dart %}
if (!done && (col == 0 || col == 3)) {
  // ... тут что-то происходит ...
}
{% endprettify %}


### Bitwise and shift operators

You can manipulate the individual bits of numbers in Dart. Usually,
you’d use these bitwise and shift operators with integers.

|-----------------------------+-------------------------------------------|
| Operator                    | Meaning                                   |
|-----------------------------+-------------------------------------------|
| `&`                         | AND
| `|`                         | OR
| `^`                         | XOR
| <code>~<em>expr</em></code> | Unary bitwise complement (0s become 1s; 1s become 0s)
| `<<`                        | Shift left
| `>>`                        | Shift right
{:.table .table-striped}

Here’s an example of using bitwise and shift operators:

<?code-excerpt "misc/test/language_tour/operators_test.dart (op-bitwise)"?>
{% prettify dart %}
final value = 0x22;
final bitmask = 0x0f;

assert((value & bitmask) == 0x02); // AND
assert((value & ~bitmask) == 0x20); // AND NOT
assert((value | bitmask) == 0x2f); // OR
assert((value ^ bitmask) == 0x2d); // XOR
assert((value << 4) == 0x220); // Shift left
assert((value >> 4) == 0x02); // Shift right
{% endprettify %}


### Conditional expressions

Dart has two operators that let you concisely evaluate expressions
that might otherwise require [if-else](#if-and-else) statements:

<code><em>condition</em> ? <em>expr1</em> : <em>expr2</em>
: If _condition_ is true, evaluates _expr1_ (and returns its value);
  otherwise, evaluates and returns the value of _expr2_.

<code><em>expr1</em> ?? <em>expr2</em></code>
: If _expr1_ is non-null, returns its value;
  otherwise, evaluates and returns the value of _expr2_.

When you need to assign a value
based on a boolean expression,
consider using `?:`.

<?code-excerpt "misc/lib/language_tour/operators.dart (if-then-else-operator)"?>
{% prettify dart %}
var visibility = isPublic ? 'public' : 'private';
{% endprettify %}

If the boolean expression tests for null,
consider using `??`.

<?code-excerpt "misc/test/language_tour/operators_test.dart (if-null)"?>
{% prettify dart %}
String playerName(String name) => name ?? 'Guest';
{% endprettify %}

The previous example could have been written at least two other ways,
but not as succinctly:

<?code-excerpt "misc/test/language_tour/operators_test.dart (if-null-alt)"?>
{% prettify dart %}
// Slightly longer version uses ?: operator.
String playerName(String name) => name != null ? name : 'Guest';

// Very long version uses if-else statement.
String playerName(String name) {
  if (name != null) {
    return name;
  } else {
    return 'Guest';
  }
}
{% endprettify %}

<a id="cascade"></a>
### Cascade notation (..)

Cascades (`..`) allow you to make a sequence of operations
on the same object. In addition to function calls,
you can also access fields on that same object.
This often saves you the step of creating a temporary variable and
allows you to write more fluid code.

Consider the following code:

<?code-excerpt "misc/test/language_tour/browser_test.dart (cascade-operator)"?>
{% prettify dart %}
querySelector('#confirm') // Get an object.
  ..text = 'Confirm' // Use its members.
  ..classes.add('important')
  ..onClick.listen((e) => window.alert('Confirmed!'));
{% endprettify %}

The first method call, `querySelector()`, returns a selector object.
The code that follows the cascade notation operates
on this selector object, ignoring any subsequent values that
might be returned.

The previous example is equivalent to:

<?code-excerpt "misc/test/language_tour/browser_test.dart (cascade-operator-example-expanded)"?>
{% prettify dart %}
var button = querySelector('#confirm');
button.text = 'Confirm';
button.classes.add('important');
button.onClick.listen((e) => window.alert('Confirmed!'));
{% endprettify %}

You can also nest your cascades. For example:

<?code-excerpt "misc/lib/language_tour/operators.dart (nested-cascades)"?>
{% prettify dart %}
final addressBook = (AddressBookBuilder()
      ..name = 'jenny'
      ..email = 'jenny@example.com'
      ..phone = (PhoneNumberBuilder()
            ..number = '415-555-0100'
            ..label = 'home')
          .build())
    .build();
{% endprettify %}

Be careful to construct your cascade on a function that returns
an actual object. For example, the following code fails:

<?code-excerpt "misc/lib/language_tour/operators.dart (cannot-cascade-on-void)" plaster="none"?>
{% prettify dart %}
var sb = StringBuffer();
sb.write('foo')
  ..write('bar'); // Error: method 'write' isn't defined for 'void'.
{% endprettify %}

The `sb.write()` call returns void,
and you can't construct a cascade on `void`.

<div class="alert alert-info" markdown="1">
**Note:**
Strictly speaking,
the "double dot" notation for cascades is not an operator.
It's just part of the Dart syntax.
</div>

### Other operators

You've seen most of the remaining operators in other examples:

|----------+-------------------------------------------|
| Operator | Name                 |          Meaning   |
|-----------+------------------------------------------|
| `()`     | Function application | Represents a function call
| `[]`     | List access          | Refers to the value at the specified index in the list
| `.`      | Member access        | Refers to a property of an expression; example: `foo.bar` selects property `bar` from expression `foo`
| `?.`     | Conditional member access | Like `.`, but the leftmost operand can be null; example: `foo?.bar` selects property `bar` from expression `foo` unless `foo` is null (in which case the value of `foo?.bar` is null)
{:.table .table-striped}

For more information about the `.`, `?.`, and `..` operators, see
[Classes](#classes).


## Control flow statements

You can control the flow of your Dart code using any of the following:

-   `if` and `else`
-   `for` loops
-   `while` and `do`-`while` loops
-   `break` and `continue`
-   `switch` and `case`
-   `assert`

You can also affect the control flow using `try-catch` and `throw`, as
explained in [Exceptions](#exceptions).


### If and else

Dart supports `if` statements with optional `else` statements, as the
next sample shows. Also see [conditional expressions](#conditional-expressions).

<?code-excerpt "misc/lib/language_tour/control_flow.dart (if-else)"?>
{% prettify dart %}
if (isRaining()) {
  you.bringRainCoat();
} else if (isSnowing()) {
  you.wearJacket();
} else {
  car.putTopDown();
}
{% endprettify %}

Unlike JavaScript, conditions must use boolean values, nothing else. See
[Booleans](#booleans) for more information.


### For loops

You can iterate with the standard `for` loop. For example:

<?code-excerpt "misc/test/language_tour/control_flow_test.dart (for)"?>
{% prettify dart %}
var message = StringBuffer('Dart is fun');
for (var i = 0; i < 5; i++) {
  message.write('!');
}
{% endprettify %}

Closures inside of Dart’s `for` loops capture the _value_ of the index,
avoiding a common pitfall found in JavaScript. For example, consider:

<?code-excerpt "misc/test/language_tour/control_flow_test.dart (for-and-closures)"?>
{% prettify dart %}
var callbacks = [];
for (var i = 0; i < 2; i++) {
  callbacks.add(() => print(i));
}
callbacks.forEach((c) => c());
{% endprettify %}

The output is `0` and then `1`, as expected. In contrast, the example
would print `2` and then `2` in JavaScript.

If the object that you are iterating over is an Iterable, you can use the
[forEach()][] method. Using `forEach()` is a good option if you don’t need to
know the current iteration counter:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (forEach)"?>
{% prettify dart %}
candidates.forEach((candidate) => candidate.interview());
{% endprettify %}

Iterable classes such as List and Set also support the `for-in` form of
[iteration](/guides/libraries/library-tour#iteration):

<?code-excerpt "misc/test/language_tour/control_flow_test.dart (collection)"?>
{% prettify dart %}
var collection = [0, 1, 2];
for (var x in collection) {
  print(x); // 0 1 2
}
{% endprettify %}


### While and do-while

A `while` loop evaluates the condition before the loop:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (while)"?>
{% prettify dart %}
while (!isDone()) {
  doSomething();
}
{% endprettify %}

A `do`-`while` loop evaluates the condition *after* the loop:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (do-while)"?>
{% prettify dart %}
do {
  printLine();
} while (!atEndOfPage());
{% endprettify %}


### Break and continue

Use `break` to stop looping:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (while-break)"?>
{% prettify dart %}
while (true) {
  if (shutDownRequested()) break;
  processIncomingRequests();
}
{% endprettify %}

Use `continue` to skip to the next loop iteration:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (for-continue)"?>
{% prettify dart %}
for (int i = 0; i < candidates.length; i++) {
  var candidate = candidates[i];
  if (candidate.yearsExperience < 5) {
    continue;
  }
  candidate.interview();
}
{% endprettify %}

You might write that example differently if you’re using an
[Iterable][] such as a list or set:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (where)"?>
{% prettify dart %}
candidates
    .where((c) => c.yearsExperience >= 5)
    .forEach((c) => c.interview());
{% endprettify %}


### Switch and case

Switch statements in Dart compare integer, string, or compile-time
constants using `==`. The compared objects must all be instances of the
same class (and not of any of its subtypes), and the class must not
override `==`.
[Enumerated types](#enumerated-types) work well in `switch` statements.

<div class="alert alert-info" markdown="1">
**Note:**
Switch statements in Dart are intended for limited circumstances,
such as in interpreters or scanners.
</div>

Each non-empty `case` clause ends with a `break` statement, as a rule.
Other valid ways to end a non-empty `case` clause are a `continue`,
`throw`, or `return` statement.

Use a `default` clause to execute code when no `case` clause matches:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (switch)"?>
{% prettify dart %}
var command = 'OPEN';
switch (command) {
  case 'CLOSED':
    executeClosed();
    break;
  case 'PENDING':
    executePending();
    break;
  case 'APPROVED':
    executeApproved();
    break;
  case 'DENIED':
    executeDenied();
    break;
  case 'OPEN':
    executeOpen();
    break;
  default:
    executeUnknown();
}
{% endprettify %}

The following example omits the `break` statement in a `case` clause,
thus generating an error:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (switch-break-omitted)" plaster="none"?>
{% prettify dart %}
var command = 'OPEN';
switch (command) {
  case 'OPEN':
    executeOpen();
    // ERROR: Missing break

  case 'CLOSED':
    executeClosed();
    break;
}
{% endprettify %}

However, Dart does support empty `case` clauses, allowing a form of
fall-through:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (switch-empty-case)"?>
{% prettify dart %}
var command = 'CLOSED';
switch (command) {
  case 'CLOSED': // Empty case falls through.
  case 'NOW_CLOSED':
    // Runs for both CLOSED and NOW_CLOSED.
    executeNowClosed();
    break;
}
{% endprettify %}

If you really want fall-through, you can use a `continue` statement and
a label:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (switch-continue)"?>
{% prettify dart %}
var command = 'CLOSED';
switch (command) {
  case 'CLOSED':
    executeClosed();
    continue nowClosed;
  // Continues executing at the nowClosed label.

  nowClosed:
  case 'NOW_CLOSED':
    // Runs for both CLOSED and NOW_CLOSED.
    executeNowClosed();
    break;
}
{% endprettify %}

A `case` clause can have local variables, which are visible only inside
the scope of that clause.


### Assert

Use an `assert` statement to disrupt normal execution if a boolean
condition is false. You can find examples of assert statements
throughout this tour. Here are some more:

<?code-excerpt "misc/test/language_tour/control_flow_test.dart (assert)"?>
{% prettify dart %}
// Make sure the variable has a non-null value.
assert(text != null);

// Make sure the value is less than 100.
assert(number < 100);

// Make sure this is an https URL.
assert(urlString.startsWith('https'));
{% endprettify %}

<div class="alert alert-info" markdown="1">
**Note:**
Assert statements have no effect in production code;
they're for development only.
Flutter enables asserts in [debug mode.][Flutter debug mode]
Development-only tools such as [dartdevc][]
typically support asserts by default.
Some tools, such as [dart][] and [dart2js,][dart2js]
support asserts through a command-line flag: `--enable-asserts`.
</div>

To attach a message to an assert,
add a string as the second argument.

<?code-excerpt "misc/test/language_tour/control_flow_test.dart (assert-with-message)"?>
{% prettify dart %}
assert(urlString.startsWith('https'),
    'URL ($urlString) should start with "https".');
{% endprettify %}

The first argument to `assert` can be any expression that
resolves to a boolean value. If the expression’s value
is true, the assertion succeeds and execution
continues. If it's false, the assertion fails and an exception (an
[AssertionError][]) is thrown.


## Exceptions

Your Dart code can throw and catch exceptions. Exceptions are errors
indicating that something unexpected happened. If the exception isn’t
caught, the isolate that raised the exception is suspended, and
typically the isolate and its program are terminated.

In contrast to Java, all of Dart’s exceptions are unchecked exceptions.
Methods do not declare which exceptions they might throw, and you are
not required to catch any exceptions.

Dart provides [Exception][] and [Error][]
types, as well as numerous predefined subtypes. You can, of course,
define your own exceptions. However, Dart programs can throw any
non-null object—not just Exception and Error objects—as an exception.

### Throw

Here’s an example of throwing, or *raising*, an exception:

<?code-excerpt "misc/lib/language_tour/exceptions.dart (throw-FormatException)"?>
{% prettify dart %}
throw FormatException('Expected at least 1 section');
{% endprettify %}

You can also throw arbitrary objects:

<?code-excerpt "misc/lib/language_tour/exceptions.dart (out-of-llamas)"?>
{% prettify dart %}
throw 'Out of llamas!';
{% endprettify %}

<div class="alert alert-info" markdown="1">
  **Note:** Production-quality code usually throws types that implement
  [Error][] or [Exception][].
</div>

Because throwing an exception is an expression, you can throw exceptions
in =\> statements, as well as anywhere else that allows expressions:

<?code-excerpt "misc/lib/language_tour/exceptions.dart (throw-is-an-expression)"?>
{% prettify dart %}
void distanceTo(Point other) => throw UnimplementedError();
{% endprettify %}


### Catch

Catching, or capturing, an exception stops the exception from
propagating (unless you rethrow the exception).
Catching an exception gives you a chance to handle it:

<?code-excerpt "misc/lib/language_tour/exceptions.dart (try)"?>
{% prettify dart %}
try {
  breedMoreLlamas();
} on OutOfLlamasException {
  buyMoreLlamas();
}
{% endprettify %}

To handle code that can throw more than one type of exception, you can
specify multiple catch clauses. The first catch clause that matches the
thrown object’s type handles the exception. If the catch clause does not
specify a type, that clause can handle any type of thrown object:

<?code-excerpt "misc/lib/language_tour/exceptions.dart (try-catch)"?>
{% prettify dart %}
try {
  breedMoreLlamas();
} on OutOfLlamasException {
  // A specific exception
  buyMoreLlamas();
} on Exception catch (e) {
  // Anything else that is an exception
  print('Unknown exception: $e');
} catch (e) {
  // No specified type, handles all
  print('Something really unknown: $e');
}
{% endprettify %}

As the preceding code shows, you can use either `on` or `catch` or both.
Use `on` when you need to specify the exception type. Use `catch` when
your exception handler needs the exception object.

You can specify one or two parameters to `catch()`.
The first is the exception that was thrown,
and the second is the stack trace (a [StackTrace][] object).

<?code-excerpt "misc/lib/language_tour/exceptions.dart (try-catch-2)" replace="/\(e.*?\)/[!$&!]/g"?>
{% prettify dart %}
try {
  // ···
} on Exception catch [!(e)!] {
  print('Exception details:\n $e');
} catch [!(e, s)!] {
  print('Exception details:\n $e');
  print('Stack trace:\n $s');
}
{% endprettify %}

To partially handle an exception,
while allowing it to propagate,
use the `rethrow` keyword.

<?code-excerpt "misc/test/language_tour/exceptions_test.dart (rethrow)" replace="/rethrow;/[!$&!]/g"?>
{% prettify dart %}
void misbehave() {
  try {
    dynamic foo = true;
    print(foo++); // Runtime error
  } catch (e) {
    print('misbehave() partially handled ${e.runtimeType}.');
    [!rethrow;!] // Allow callers to see the exception.
  }
}

void main() {
  try {
    misbehave();
  } catch (e) {
    print('main() finished handling ${e.runtimeType}.');
  }
}
{% endprettify %}


### Finally

To ensure that some code runs whether or not an exception is thrown, use
a `finally` clause. If no `catch` clause matches the exception, the
exception is propagated after the `finally` clause runs:

<?code-excerpt "misc/lib/language_tour/exceptions.dart (finally)"?>
{% prettify dart %}
try {
  breedMoreLlamas();
} finally {
  // Always clean up, even if an exception is thrown.
  cleanLlamaStalls();
}
{% endprettify %}

The `finally` clause runs after any matching `catch` clauses:

<?code-excerpt "misc/lib/language_tour/exceptions.dart (try-catch-finally)"?>
{% prettify dart %}
try {
  breedMoreLlamas();
} catch (e) {
  print('Error: $e'); // Handle the exception first.
} finally {
  cleanLlamaStalls(); // Then clean up.
}
{% endprettify %}

Learn more by reading the
[Exceptions](/guides/libraries/library-tour#exceptions)
section of the library tour.

## Classes

Dart is an object-oriented language with classes and mixin-based
inheritance. Every object is an instance of a class, and all classes
descend from [Object.][Object]
*Mixin-based inheritance* means that although every class (except for
Object) has exactly one superclass, a class body can be reused in
multiple class hierarchies.


### Using class members

Objects have *members* consisting of functions and data (*methods* and
*instance variables*, respectively). When you call a method, you *invoke*
it on an object: the method has access to that object’s functions and
data.

Use a dot (`.`) to refer to an instance variable or method:

<?code-excerpt "misc/test/language_tour/classes_test.dart (object-members)"?>
{% prettify dart %}
var p = Point(2, 2);

// Set the value of the instance variable y.
p.y = 3;

// Get the value of y.
assert(p.y == 3);

// Invoke distanceTo() on p.
num distance = p.distanceTo(Point(4, 4));
{% endprettify %}

Use `?.` instead of `.` to avoid an exception
when the leftmost operand is null:

{% comment %}
https://dartpad.dartlang.org/0cb25997742ed5382e4a
https://gist.github.com/0cb25997742ed5382e4a
{% endcomment %}

<?code-excerpt "misc/test/language_tour/classes_test.dart (safe-member-access)"?>
{% prettify dart %}
// If p is non-null, set its y value to 4.
p?.y = 4;
{% endprettify %}


### Using constructors

You can create an object using a *constructor*.
Constructor names can be either <code><em>ClassName</em></code> or
<code><em>ClassName</em>.<em>identifier</em></code>. For example,
the following code creates `Point` objects using the
`Point()` and `Point.fromJson()` constructors:

<?code-excerpt "misc/test/language_tour/classes_test.dart (object-creation)" replace="/ as .*?;/;/g"?>
{% prettify dart %}
var p1 = Point(2, 2);
var p2 = Point.fromJson({'x': 1, 'y': 2});
{% endprettify %}

The following code has the same effect, but
uses the optional `new` keyword before the constructor name:

<?code-excerpt "misc/test/language_tour/classes_test.dart (object-creation-new)" replace="/ as .*?;/;/g"?>
{% prettify dart %}
var p1 = new Point(2, 2);
var p2 = new Point.fromJson({'x': 1, 'y': 2});
{% endprettify %}

<aside class="alert alert-info" markdown="1">
  **Version note:** The `new` keyword became optional in Dart 2.
</aside>

Some classes provide [constant constructors](#constant-constructors).
To create a compile-time constant using a constant constructor,
put the `const` keyword before the constructor name:

<?code-excerpt "misc/test/language_tour/classes_test.dart (const)"?>
{% prettify dart %}
var p = const ImmutablePoint(2, 2);
{% endprettify %}

Constructing two identical compile-time constants results in a single,
canonical instance:

<?code-excerpt "misc/test/language_tour/classes_test.dart (identical)"?>
{% prettify dart %}
var a = const ImmutablePoint(1, 1);
var b = const ImmutablePoint(1, 1);

assert(identical(a, b)); // They are the same instance!
{% endprettify %}

Within a _constant context_, you can omit the `const` before a constructor
or literal. For example, look at this code, which creates a const map:

<?code-excerpt "misc/test/language_tour/classes_test.dart (const-context-withconst)" replace="/pointAndLine1/pointAndLine/g"?>
{% prettify dart %}
// Lots of const keywords here.
const pointAndLine = const {
  'point': const [const ImmutablePoint(0, 0)],
  'line': const [const ImmutablePoint(1, 10), const ImmutablePoint(-2, 11)],
};
{% endprettify %}

You can omit all but the first use of the `const` keyword:

<?code-excerpt "misc/test/language_tour/classes_test.dart (const-context-noconst)" replace="/pointAndLine2/pointAndLine/g"?>
{% prettify dart %}
// Only one const, which establishes the constant context.
const pointAndLine = {
  'point': [ImmutablePoint(0, 0)],
  'line': [ImmutablePoint(1, 10), ImmutablePoint(-2, 11)],
};
{% endprettify %}

If a constant constructor is outside of a constant context
and is invoked without `const`,
it creates a **non-constant object**:

<?code-excerpt "misc/test/language_tour/classes_test.dart (nonconst-const-constructor)"?>
{% prettify dart %}
var a = const ImmutablePoint(1, 1); // Creates a constant
var b = ImmutablePoint(1, 1); // Does NOT create a constant

assert(!identical(a, b)); // NOT the same instance!
{% endprettify %}

<aside class="alert alert-info" markdown="1">
  **Version note:** The `const` keyword became optional
  within a constant context in Dart 2.
</aside>


### Getting an object's type

To get an object's type at runtime,
you can use Object's `runtimeType` property,
which returns a [Type][] object.

<?code-excerpt "misc/test/language_tour/classes_test.dart (runtimeType)"?>
{% prettify dart %}
print('The type of a is ${a.runtimeType}');
{% endprettify %}

Up to here, you've seen how to _use_ classes.
The rest of this section shows how to _implement_ classes.


### Instance variables

Here’s how you declare instance variables:

<?code-excerpt "misc/lib/language_tour/classes/point_with_main.dart (class)"?>
{% prettify dart %}
class Point {
  num x; // Declare instance variable x, initially null.
  num y; // Declare y, initially null.
  num z = 0; // Declare z, initially 0.
}
{% endprettify %}

All uninitialized instance variables have the value `null`.

All instance variables generate an implicit *getter* method. Non-final
instance variables also generate an implicit *setter* method. For details,
see [Getters and setters](#getters-and-setters).

<?code-excerpt "misc/lib/language_tour/classes/point_with_main.dart (class+main)" replace="/(num .*?;).*/$1/g" plaster="none"?>
{% prettify dart %}
class Point {
  num x;
  num y;
}

void main() {
  var point = Point();
  point.x = 4; // Use the setter method for x.
  assert(point.x == 4); // Use the getter method for x.
  assert(point.y == null); // Values default to null.
}
{% endprettify %}

If you initialize an instance variable where it is declared (instead of
in a constructor or method), the value is set when the instance is
created, which is before the constructor and its initializer list
execute.


### Constructors

Declare a constructor by creating a function with the same name as its
class (plus, optionally, an additional identifier as described in
[Named constructors](#named-constructors)).
The most common form of constructor, the generative constructor, creates
a new instance of a class:

<?code-excerpt "misc/lib/language_tour/classes/point_alt.dart (constructor-long-way)" plaster="none"?>
{% prettify dart %}
class Point {
  num x, y;

  Point(num x, num y) {
    // There's a better way to do this, stay tuned.
    this.x = x;
    this.y = y;
  }
}
{% endprettify %}

The `this` keyword refers to the current instance.

<div class="alert alert-info" markdown="1">
**Note:**
Use `this` only when there is a name conflict. Otherwise, Dart style
omits the `this`.
</div>

The pattern of assigning a constructor argument to an instance variable
is so common, Dart has syntactic sugar to make it easy:

<?code-excerpt "misc/lib/language_tour/classes/point.dart (constructor-initializer)" plaster="none"?>
{% prettify dart %}
class Point {
  num x, y;

  // Syntactic sugar for setting x and y
  // before the constructor body runs.
  Point(this.x, this.y);
}
{% endprettify %}

#### Default constructors

If you don’t declare a constructor, a default constructor is provided
for you. The default constructor has no arguments and invokes the
no-argument constructor in the superclass.

#### Constructors aren’t inherited

Subclasses don’t inherit constructors from their superclass. A subclass
that declares no constructors has only the default (no argument, no
name) constructor.

#### Named constructors

Use a named constructor to implement multiple constructors for a class
or to provide extra clarity:

<?code-excerpt "misc/lib/language_tour/classes/point.dart (named-constructor)" replace="/Point\.\S*/[!$&!]/g" plaster="none"?>
{% prettify dart %}
class Point {
  num x, y;

  Point(this.x, this.y);

  // Named constructor
  [!Point.origin()!] {
    x = 0;
    y = 0;
  }
}
{% endprettify %}

Remember that constructors are not inherited, which means that a
superclass’s named constructor is not inherited by a subclass. If you
want a subclass to be created with a named constructor defined in the
superclass, you must implement that constructor in the subclass.

#### Invoking a non-default superclass constructor

By default, a constructor in a subclass calls the superclass’s unnamed,
no-argument constructor.
The superclass's constructor is called at the beginning of the
constructor body. If an [initializer list](#initializer-list)
is also being used, it executes before the superclass is called.
In summary, the order of execution is as follows:

1. initializer list
1. superclass's no-arg constructor
1. main class's no-arg constructor

If the superclass doesn’t have an unnamed, no-argument constructor,
then you must manually call one of the constructors in the
superclass. Specify the superclass constructor after a colon (`:`), just
before the constructor body (if any).

In the following example, the constructor for the Employee class
calls the named constructor for its superclass, Person.
Click the run button {% asset red-run.png alt="" %} to execute the code.

{% comment %}
https://gist.github.com/Sfshaza/e57aa06401e6618d4eb8
https://dartpad.dartlang.org/e57aa06401e6618d4eb8

<?code-excerpt "misc/lib/language_tour/classes/employee.dart" plaster="none"?>
{% prettify dart %}
class Person {
  String firstName;

  Person.fromJson(Map data) {
    print('in Person');
  }
}

class Employee extends Person {
  // Person does not have a default constructor;
  // you must call super.fromJson(data).
  Employee.fromJson(Map data) : super.fromJson(data) {
    print('in Employee');
  }
}

void main() {
  var emp = Employee.fromJson({});
  // Prints:
  // in Person
  // in Employee

  if (emp is Person) {
    // Type check
    emp.firstName = 'Bob';
  }
  (emp as Person).firstName = 'Bob';
}
{% endprettify %}
{% endcomment %}

<iframe
src="{{site.custom.dartpad.embed-inline-prefix}}?id=e57aa06401e6618d4eb8&verticalRatio=80"
    width="100%"
    height="500px"
    style="border: 1px solid #ccc;">
</iframe>

Because the arguments to the superclass constructor are evaluated before
invoking the constructor, an argument can be an expression such as a
function call:

<?code-excerpt "misc/lib/language_tour/classes/employee.dart (method-then-constructor)"?>
{% prettify dart %}
class Employee extends Person {
  Employee() : super.fromJson(getDefaultData());
  // ···
}
{% endprettify %}

<div class="alert alert-warning" markdown="1">
**Warning:**
Arguments to the superclass constructor do not have access to `this`.
For example, arguments can call static methods but not instance methods.
</div>

#### Initializer list

Besides invoking a superclass constructor, you can also initialize
instance variables before the constructor body runs. Separate
initializers with commas.

<?code-excerpt "misc/lib/language_tour/classes/point_alt.dart (initializer-list)"?>
{% prettify dart %}
// Initializer list sets instance variables before
// the constructor body runs.
Point.fromJson(Map<String, num> json)
    : x = json['x'],
      y = json['y'] {
  print('In Point.fromJson(): ($x, $y)');
}
{% endprettify %}

<div class="alert alert-warning" markdown="1">
**Warning:**
The right-hand side of an initializer does not have access to `this`.
</div>

During development, you can validate inputs by using `assert` in the
initializer list.

<?code-excerpt "misc/lib/language_tour/classes/point_alt.dart (initializer-list-with-assert)" replace="/assert\(.*?\)/[!$&!]/g"?>
{% prettify dart %}
Point.withAssert(this.x, this.y) : [!assert(x >= 0)!] {
  print('In Point.withAssert(): ($x, $y)');
}
{% endprettify %}

{% comment %}
[PENDING: the example could be better.
Note that DartPad doesn't support this yet?

https://github.com/dart-lang/sdk/issues/30968
https://github.com/dart-lang/sdk/blob/master/docs/language/informal/assert-in-initializer-list.md]
{% endcomment %}

Initializer lists are handy when setting up final fields.
The following example initializes three final fields in an initializer list.
Click the run button {% asset red-run.png alt="" %} to execute the code.

{% comment %}
https://gist.github.com/Sfshaza/7a9764702c0608711e08
https://dartpad.dartlang.org/7a9764702c0608711e08

<?code-excerpt "misc/lib/language_tour/classes/point_with_distance_field.dart"?>
{% prettify dart %}
import 'dart:math';

class Point {
  final num x;
  final num y;
  final num distanceFromOrigin;

  Point(num x, num y)
      : x = x,
        y = y,
        distanceFromOrigin = sqrt(x * x + y * y);
}

void main() {
  var p = Point(2, 3);
  print(p.distanceFromOrigin);
}
{% endprettify %}
{% endcomment %}

<iframe
src="{{site.custom.dartpad.embed-inline-prefix}}?id=7a9764702c0608711e08&verticalRatio=85"
    width="100%"
    height="420px"
    style="border: 1px solid #ccc;">
</iframe>


#### Redirecting constructors

Sometimes a constructor’s only purpose is to redirect to another
constructor in the same class. A redirecting constructor’s body is
empty, with the constructor call appearing after a colon (:).

<?code-excerpt "misc/lib/language_tour/classes/point_redirecting.dart"?>
{% prettify dart %}
class Point {
  num x, y;

  // The main constructor for this class.
  Point(this.x, this.y);

  // Delegates to the main constructor.
  Point.alongXAxis(num x) : this(x, 0);
}
{% endprettify %}

#### Constant constructors

If your class produces objects that never change, you can make these
objects compile-time constants. To do this, define a `const` constructor
and make sure that all instance variables are `final`.

<?code-excerpt "misc/lib/language_tour/classes/immutable_point.dart"?>
{% prettify dart %}
class ImmutablePoint {
  static final ImmutablePoint origin =
      const ImmutablePoint(0, 0);

  final num x, y;

  const ImmutablePoint(this.x, this.y);
}
{% endprettify %}

Constant constructors don't always create constants.
For details, see the section on
[using constructors](#using-constructors).


#### Factory constructors

Use the `factory` keyword when implementing a constructor that doesn’t
always create a new instance of its class. For example, a factory
constructor might return an instance from a cache, or it might return an
instance of a subtype.

The following example demonstrates a factory constructor returning
objects from a cache:

<?code-excerpt "misc/lib/language_tour/classes/logger.dart"?>
{% prettify dart %}
class Logger {
  final String name;
  bool mute = false;

  // _cache is library-private, thanks to
  // the _ in front of its name.
  static final Map<String, Logger> _cache =
      <String, Logger>{};

  factory Logger(String name) {
    if (_cache.containsKey(name)) {
      return _cache[name];
    } else {
      final logger = Logger._internal(name);
      _cache[name] = logger;
      return logger;
    }
  }

  Logger._internal(this.name);

  void log(String msg) {
    if (!mute) print(msg);
  }
}
{% endprettify %}

<div class="alert alert-info" markdown="1">
**Note:**
Factory constructors have no access to `this`.
</div>

Invoke a factory constructor just like you would any other constructor:

<?code-excerpt "misc/lib/language_tour/classes/logger.dart (logger)"?>
{% prettify dart %}
var logger = Logger('UI');
logger.log('Button clicked');
{% endprettify %}


### Methods

Methods are functions that provide behavior for an object.

#### Instance methods

Instance methods on objects can access instance variables and `this`.
The `distanceTo()` method in the following sample is an example of an
instance method:

<?code-excerpt "misc/lib/language_tour/classes/point.dart (class-with-distanceTo)" plaster="none"?>
{% prettify dart %}
import 'dart:math';

class Point {
  num x, y;

  Point(this.x, this.y);

  num distanceTo(Point other) {
    var dx = x - other.x;
    var dy = y - other.y;
    return sqrt(dx * dx + dy * dy);
  }
}
{% endprettify %}

#### Getters and setters

Getters and setters are special methods that provide read and write
access to an object’s properties. Recall that each instance variable has
an implicit getter, plus a setter if appropriate. You can create
additional properties by implementing getters and setters, using the
`get` and `set` keywords:

<?code-excerpt "misc/lib/language_tour/classes/rectangle.dart"?>
{% prettify dart %}
class Rectangle {
  num left, top, width, height;

  Rectangle(this.left, this.top, this.width, this.height);

  // Define two calculated properties: right and bottom.
  num get right => left + width;
  set right(num value) => left = value - width;
  num get bottom => top + height;
  set bottom(num value) => top = value - height;
}

void main() {
  var rect = Rectangle(3, 4, 20, 15);
  assert(rect.left == 3);
  rect.right = 12;
  assert(rect.left == -8);
}
{% endprettify %}

With getters and setters, you can start with instance variables, later
wrapping them with methods, all without changing client code.

<div class="alert alert-info" markdown="1">
**Note:**
Operators such as increment (++) work in the expected way, whether or
not a getter is explicitly defined. To avoid any unexpected side
effects, the operator calls the getter exactly once, saving its value
in a temporary variable.
</div>

#### Abstract methods

Instance, getter, and setter methods can be abstract, defining an
interface but leaving its implementation up to other classes.
Abstract methods can only exist in [abstract classes](#abstract-classes).

To make a method abstract, use a semicolon (;) instead of a method body:

<?code-excerpt "misc/lib/language_tour/classes/doer.dart"?>
{% prettify dart %}
abstract class Doer {
  // Define instance variables and methods...

  void doSomething(); // Define an abstract method.
}

class EffectiveDoer extends Doer {
  void doSomething() {
    // Provide an implementation, so the method is not abstract here...
  }
}
{% endprettify %}


### Abstract classes

Use the `abstract` modifier to define an *abstract class*—a class that
can’t be instantiated. Abstract classes are useful for defining
interfaces, often with some implementation. If you want your abstract
class to appear to be instantiable, define a [factory
constructor](#factory-constructors).

Abstract classes often have [abstract methods](#abstract-methods).
Here’s an example of declaring an abstract class that has an abstract
method:

<?code-excerpt "misc/lib/language_tour/classes/misc.dart (abstract)"?>
{% prettify dart %}
// This class is declared abstract and thus
// can't be instantiated.
abstract class AbstractContainer {
  // Define constructors, fields, methods...

  void updateChildren(); // Abstract method.
}
{% endprettify %}


### Implicit interfaces

Every class implicitly defines an interface containing all the instance
members of the class and of any interfaces it implements. If you want to
create a class A that supports class B’s API without inheriting B’s
implementation, class A should implement the B interface.

A class implements one or more interfaces by declaring them in an
`implements` clause and then providing the APIs required by the
interfaces. For example:

<?code-excerpt "misc/lib/language_tour/classes/impostor.dart"?>
{% prettify dart %}
// A person. The implicit interface contains greet().
class Person {
  // In the interface, but visible only in this library.
  final _name;

  // Not in the interface, since this is a constructor.
  Person(this._name);

  // In the interface.
  String greet(String who) => 'Hello, $who. I am $_name.';
}

// An implementation of the Person interface.
class Impostor implements Person {
  get _name => '';

  String greet(String who) => 'Hi $who. Do you know who I am?';
}

String greetBob(Person person) => person.greet('Bob');

void main() {
  print(greetBob(Person('Kathy')));
  print(greetBob(Impostor()));
}
{% endprettify %}

Here’s an example of specifying that a class implements multiple
interfaces:

<?code-excerpt "misc/lib/language_tour/classes/misc.dart (point_interfaces)"?>
{% prettify dart %}
class Point implements Comparable, Location {...}
{% endprettify %}


### Extending a class

Use `extends` to create a subclass, and `super` to refer to the
superclass:

<?code-excerpt "misc/lib/language_tour/classes/extends.dart" replace="/extends|super/[!$&!]/g"?>
{% prettify dart %}
class Television {
  void turnOn() {
    _illuminateDisplay();
    _activateIrSensor();
  }
  // ···
}

class SmartTelevision [!extends!] Television {
  void turnOn() {
    [!super!].turnOn();
    _bootNetworkInterface();
    _initializeMemory();
    _upgradeApps();
  }
  // ···
}
{% endprettify %}


#### Overriding members

Subclasses can override instance methods, getters, and setters.
You can use the `@override` annotation to indicate that you are
intentionally overriding a member:

<?code-excerpt "misc/lib/language_tour/metadata/television.dart (override)" replace="/@override/[!$&!]/g"?>
{% prettify dart %}
class SmartTelevision extends Television {
  [!@override!]
  void turnOn() {...}
  // ···
}
{% endprettify %}

To narrow the type of a method parameter or instance variable in code that is
[type safe](/guides/language/sound-dart),
you can use the [`covariant` keyword](/guides/language/sound-problems#the-covariant-keyword).


#### Overridable operators

You can override the operators shown in the following table.
For example, if you define a
Vector class, you might define a `+` method to add two vectors.

`<`  | `+`  | `|`  | `[]`
`>`  | `/`  | `^`  | `[]=`
`<=` | `~/` | `&`  | `~`
`>=` | `*`  | `<<` | `==`
`–`  | `%`  | `>>`
{:.table}

<div class="alert alert-info" markdown="1">
  **Note:** You may have noticed that `!=` is not an overridable operator.
  The expression `e1 != e2` is just syntactic sugar for `!(e1 == e2)`.
</div>

Here’s an example of a class that overrides the `+` and `-` operators:

<?code-excerpt "misc/lib/language_tour/classes/vector.dart"?>
{% prettify dart %}
class Vector {
  final int x, y;

  Vector(this.x, this.y);

  Vector operator +(Vector v) => Vector(x + v.x, y + v.y);
  Vector operator -(Vector v) => Vector(x - v.x, y - v.y);

  // Operator == and hashCode not shown. For details, see note below.
  // ···
}

void main() {
  final v = Vector(2, 3);
  final w = Vector(2, 2);

  assert(v + w == Vector(4, 5));
  assert(v - w == Vector(0, 1));
}
{% endprettify %}

If you override `==`, you should also override Object's `hashCode` getter.
For an example of overriding `==` and `hashCode`, see
[Implementing map keys](/guides/libraries/library-tour#implementing-map-keys).

For more information on overriding, in general, see
[Extending a class](#extending-a-class).


#### noSuchMethod()

To detect or react whenever code attempts to use a non-existent method or
instance variable, you can override `noSuchMethod()`:

<?code-excerpt "misc/lib/language_tour/classes/no_such_method.dart" replace="/noSuchMethod(?!,)/[!$&!]/g"?>
{% prettify dart %}
class A {
  // Unless you override noSuchMethod, using a
  // non-existent member results in a NoSuchMethodError.
  @override
  void [!noSuchMethod!](Invocation invocation) {
    print('You tried to use a non-existent member: ' +
        '${invocation.memberName}');
  }
}
{% endprettify %}

You **can't invoke** an unimplemented method unless
**one** of the following is true:

* The receiver has the static type `dynamic`.

* The receiver has a static type that
defines the unimplemented method (abstract is OK),
and the dynamic type of the receiver has an implemention of `noSuchMethod()`
that's different from the one in class `Object`.

For more information, see the informal
[noSuchMethod forwarding specification.](https://github.com/dart-lang/sdk/blob/master/docs/language/informal/nosuchmethod-forwarding.md)


<a id="enums"></a>
### Enumerated types

Enumerated types, often called _enumerations_ or _enums_,
are a special kind of class used to represent
a fixed number of constant values.


#### Using enums

Declare an enumerated type using the `enum` keyword:

<?code-excerpt "misc/lib/language_tour/classes/enum.dart (enum)"?>
{% prettify dart %}
enum Color { red, green, blue }
{% endprettify %}

Each value in an enum has an `index` getter,
which returns the zero-based position of the value in the enum declaration.
For example, the first value has index 0,
and the second value has index 1.

<?code-excerpt "misc/lib/language_tour/classes/enum.dart (index)"?>
{% prettify dart %}
assert(Color.red.index == 0);
assert(Color.green.index == 1);
assert(Color.blue.index == 2);
{% endprettify %}

To get a list of all of the values in the enum,
use the enum's `values` constant.

<?code-excerpt "misc/lib/language_tour/classes/enum.dart (values)"?>
{% prettify dart %}
List<Color> colors = Color.values;
assert(colors[2] == Color.blue);
{% endprettify %}

You can use enums in [switch statements](#switch-and-case), and
you'll get a warning if you don't handle all of the enum's values:

<?code-excerpt "misc/lib/language_tour/classes/enum.dart (switch)"?>
{% prettify dart %}
var aColor = Color.blue;

switch (aColor) {
  case Color.red:
    print('Red as roses!');
    break;
  case Color.green:
    print('Green as grass!');
    break;
  default: // Without this, you see a WARNING.
    print(aColor); // 'Color.blue'
}
{% endprettify %}

Enumerated types have the following limits:

* You can't subclass, mix in, or implement an enum.
* You can't explicitly instantiate an enum.

For more information, see the
[Dart Language Specification](/guides/language/spec).


### Adding features to a class: mixins

Mixins are a way of reusing a class's code in multiple class
hierarchies.

To _use_ a mixin, use the `with` keyword followed by one or more mixin
names. The following example shows two classes that use mixins:

<?code-excerpt "misc/lib/language_tour/classes/orchestra.dart (Musician and Maestro)" replace="/(with.*) \{/[!$1!] {/g"?>
{% prettify dart %}
class Musician extends Performer [!with Musical!] {
  // ···
}

class Maestro extends Person
    [!with Musical, Aggressive, Demented!] {
  Maestro(String maestroName) {
    name = maestroName;
    canConduct = true;
  }
}
{% endprettify %}

To _implement_ a mixin, create a class that extends Object and
declares no constructors.
Unless you want your mixin to be usable as a regular class,
use the `mixin` keyword instead of `class`.
For example:

<?code-excerpt "misc/lib/language_tour/classes/orchestra.dart (Musical)"?>
{% prettify dart %}
mixin Musical {
  bool canPlayPiano = false;
  bool canCompose = false;
  bool canConduct = false;

  void entertainMe() {
    if (canPlayPiano) {
      print('Playing piano');
    } else if (canConduct) {
      print('Waving hands');
    } else {
      print('Humming to self');
    }
  }
}
{% endprettify %}

To specify that only certain types can use the mixin — for example,
so your mixin can invoke a method that it doesn't define —
use `on` to specify the required superclass:

<?code-excerpt "misc/lib/language_tour/classes/orchestra.dart (mixin-on)"?>
{% prettify dart %}
mixin MusicalPerformer on Musician {
  // ···
}
{% endprettify %}

<!-- Previous code snippet reveals an issue we expect to be fixed in 2.1.1:
  https://github.com/dart-lang/sdk/issues/35011
-->

<aside class="alert alert-info" markdown="1">
  **Version note:** Support for the `mixin` keyword was introduced in Dart 2.1.
  Code in earlier releases usually used `abstract class` instead.
  For more information on 2.1 mixin changes, see the
  [Dart SDK changelog][] and [2.1 mixin specification.][]
</aside>

[Dart SDK changelog]: https://github.com/dart-lang/sdk/blob/master/CHANGELOG.md
[2.1 mixin specification.]: https://github.com/dart-lang/language/blob/master/accepted/2.1/super-mixins/feature-specification.md#dart-2-mixin-declarations


### Class variables and methods

Use the `static` keyword to implement class-wide variables and methods.

#### Static variables

Static variables (class variables) are useful for class-wide state and
constants:

<?code-excerpt "misc/lib/language_tour/classes/misc.dart (static-field)"?>
{% prettify dart %}
class Queue {
  static const initialCapacity = 16;
  // ···
}

void main() {
  assert(Queue.initialCapacity == 16);
}
{% endprettify %}

Static variables aren’t initialized until they’re used.

<div class="alert alert-info" markdown="1">
**Note:**
This page follows the [style guide
recommendation](/guides/language/effective-dart/style#identifiers)
of preferring `lowerCamelCase` for constant names.
</div>

#### Static methods

Static methods (class methods) do not operate on an instance, and thus
do not have access to `this`. For example:

<?code-excerpt "misc/lib/language_tour/classes/point_with_distance_method.dart"?>
{% prettify dart %}
import 'dart:math';

class Point {
  num x, y;
  Point(this.x, this.y);

  static num distanceBetween(Point a, Point b) {
    var dx = a.x - b.x;
    var dy = a.y - b.y;
    return sqrt(dx * dx + dy * dy);
  }
}

void main() {
  var a = Point(2, 2);
  var b = Point(4, 4);
  var distance = Point.distanceBetween(a, b);
  assert(2.8 < distance && distance < 2.9);
  print(distance);
}
{% endprettify %}

<div class="alert alert-info" markdown="1">
**Note:**
Consider using top-level functions, instead of static methods, for
common or widely used utilities and functionality.
</div>

You can use static methods as compile-time constants. For example, you
can pass a static method as a parameter to a constant constructor.


## Generics

If you look at the API documentation for the basic array type,
[List,][List] you’ll see that the
type is actually `List<E>`. The \<...\> notation marks List as a
*generic* (or *parameterized*) type—a type that has formal type
parameters. [By convention][], most type variables have single-letter names,
such as E, T, S, K, and V.

[By convention]: /guides/language/effective-dart/design#do-follow-existing-mnemonic-conventions-when-naming-type-parameters


### Why use generics?

Generics are often required for type safety, but they have more benefits
than just allowing your code to run:

* Properly specifying generic types results in better generated code.
* You can use generics to reduce code duplication.

If you intend for a list to contain only strings, you can
declare it as `List<String>` (read that as “list of string”). That way
you, your fellow programmers, and your tools can detect that assigning a non-string to
the list is probably a mistake. Here’s an example:

{:.fails-sa}
<?code-excerpt "misc/lib/language_tour/generics/misc.dart (why-generics)"?>
{% prettify dart %}
var names = List<String>();
names.addAll(['Seth', 'Kathy', 'Lars']);
names.add(42); // Error
{% endprettify %}

Another reason for using generics is to reduce code duplication.
Generics let you share a single interface and implementation between
many types, while still taking advantage of static
analysis. For example, say you create an interface for
caching an object:

<?code-excerpt "misc/lib/language_tour/generics/cache.dart (ObjectCache)"?>
{% prettify dart %}
abstract class ObjectCache {
  Object getByKey(String key);
  void setByKey(String key, Object value);
}
{% endprettify %}

You discover that you want a string-specific version of this interface,
so you create another interface:

<?code-excerpt "misc/lib/language_tour/generics/cache.dart (StringCache)"?>
{% prettify dart %}
abstract class StringCache {
  String getByKey(String key);
  void setByKey(String key, String value);
}
{% endprettify %}

Later, you decide you want a number-specific version of this
interface... You get the idea.

Generic types can save you the trouble of creating all these interfaces.
Instead, you can create a single interface that takes a type parameter:

<?code-excerpt "misc/lib/language_tour/generics/cache.dart (Cache)"?>
{% prettify dart %}
abstract class Cache<T> {
  T getByKey(String key);
  void setByKey(String key, T value);
}
{% endprettify %}

In this code, T is the stand-in type. It’s a placeholder that you can
think of as a type that a developer will define later.


### Using collection literals

List and map literals can be parameterized. Parameterized literals are
just like the literals you’ve already seen, except that you add
<code>&lt;<em>type</em>></code> (for lists) or
<code>&lt;<em>keyType</em>, <em>valueType</em>></code> (for maps)
before the opening bracket. Here
is example of using typed literals:

<?code-excerpt "misc/lib/language_tour/generics/misc.dart (collection-literals)"?>
{% prettify dart %}
var names = <String>['Seth', 'Kathy', 'Lars'];
var pages = <String, String>{
  'index.html': 'Homepage',
  'robots.txt': 'Hints for web robots',
  'humans.txt': 'We are people, not machines'
};
{% endprettify %}


### Using parameterized types with constructors

To specify one or more types when using a constructor, put the types in
angle brackets (`<...>`) just after the class name. For example:

<?code-excerpt "misc/test/language_tour/generics_test.dart (constructor-1)"?>
{% prettify dart %}
var names = List<String>();
names.addAll(['Seth', 'Kathy', 'Lars']);
var nameSet = Set<String>.from(names);
{% endprettify %}

The following code creates a map that has integer keys and values of
type View:

<?code-excerpt "misc/test/language_tour/generics_test.dart (constructor-2)"?>
{% prettify dart %}
var views = Map<int, View>();
{% endprettify %}


### Generic collections and the types they contain

Dart generic types are *reified*, which means that they carry their type
information around at runtime. For example, you can test the type of a
collection:

<?code-excerpt "misc/test/language_tour/generics_test.dart (generic-collections)"?>
{% prettify dart %}
var names = List<String>();
names.addAll(['Seth', 'Kathy', 'Lars']);
print(names is List<String>); // true
{% endprettify %}

<div class="alert alert-info" markdown="1">
**Note:**
In contrast, generics in Java use *erasure*, which means that generic
type parameters are removed at runtime. In Java, you can test whether
an object is a List, but you can’t test whether it’s a `List<String>`.
</div>


### Restricting the parameterized type

When implementing a generic type,
you might want to limit the types of its parameters.
You can do this using `extends`.

<?code-excerpt "misc/lib/language_tour/generics/base_class.dart" replace="/extends SomeBaseClass(?=. \{)/[!$&!]/g"?>
{% prettify dart %}
class Foo<T [!extends SomeBaseClass!]> {
  // Implementation goes here...
  String toString() => "Instance of 'Foo<$T>'";
}

class Extender extends SomeBaseClass {...}
{% endprettify %}

It's OK to use `SomeBaseClass` or any of its subclasses as generic argument:

<?code-excerpt "misc/test/language_tour/generics_test.dart (SomeBaseClass-ok)" replace="/Foo.\w+./[!$&!]/g"?>
{% prettify dart %}
var someBaseClassFoo = [!Foo<SomeBaseClass>!]();
var extenderFoo = [!Foo<Extender>!]();
{% endprettify %}

It's also OK to specify no generic argument:

<?code-excerpt "misc/test/language_tour/generics_test.dart (no-generic-arg-ok)" replace="/expect\((.*?).toString\(\), .(.*?).\);/print($1); \/\/ $2/g"?>
{% prettify dart %}
var foo = Foo();
print(foo); // Instance of 'Foo<SomeBaseClass>'
{% endprettify %}

Specifying any non-`SomeBaseClass` type results in an error:

{:.fails-sa}
<?code-excerpt "misc/lib/language_tour/generics/misc.dart (Foo-Object-error)" replace="/Foo.\w+./[!$&!]/g"?>
{% prettify dart %}
var foo = [!Foo<Object>!]();
{% endprettify %}


### Using generic methods

Initially, Dart's generic support was limited to classes.
A newer syntax, called _generic methods_, allows type arguments on methods and functions:

<!-- https://dartpad.dartlang.org/a02c53b001977efa4d803109900f21bb -->
<!-- https://gist.github.com/a02c53b001977efa4d803109900f21bb -->
<?code-excerpt "misc/test/language_tour/generics_test.dart (method)" replace="/<T.(?=\()|T/[!$&!]/g"?>
{% prettify dart %}
[!T!] first[!<T>!](List<[!T!]> ts) {
  // Do some initial work or error checking, then...
  [!T!] tmp = ts[0];
  // Do some additional checking or processing...
  return tmp;
}
{% endprettify %}

Here the generic type parameter on `first` (`<T>`)
allows you to use the type argument `T` in several places:

* In the function's return type (`T`).
* In the type of an argument (`List<T>`).
* In the type of a local variable (`T tmp`).

For more information about generics, see
[Using Generic Methods.](https://github.com/dart-lang/sdk/blob/master/pkg/dev_compiler/doc/GENERIC_METHODS.md)


## Libraries and visibility

The `import` and `library` directives can help you create a
modular and shareable code base. Libraries not only provide APIs, but
are a unit of privacy: identifiers that start with an underscore (\_)
are visible only inside the library. *Every Dart app is a library*, even
if it doesn’t use a `library` directive.

Libraries can be distributed using packages. See
[Pub Package and Asset Manager](/tools/pub)
for information about
pub, a package manager included in the SDK.


### Using libraries

Use `import` to specify how a namespace from one library is used in the
scope of another library.

For example, Dart web apps generally use the [dart:html][]
library, which they can import like this:

<?code-excerpt "misc/test/language_tour/browser_test.dart (dart-html-import)"?>
{% prettify dart %}
import 'dart:html';
{% endprettify %}

The only required argument to `import` is a URI specifying the
library.
For built-in libraries, the URI has the special `dart:` scheme.
For other libraries, you can use a file system path or the `package:`
scheme. The `package:` scheme specifies libraries provided by a package
manager such as the pub tool. For example:

<?code-excerpt "misc/test/language_tour/browser_test.dart (package-import)"?>
{% prettify dart %}
import 'package:test/test.dart';
{% endprettify %}

<div class="alert alert-info" markdown="1">
**Note:**
*URI* stands for uniform resource identifier.
*URLs* (uniform resource locators) are a common kind of URI.
</div>


#### Specifying a library prefix

If you import two libraries that have conflicting identifiers, then you
can specify a prefix for one or both libraries. For example, if library1
and library2 both have an Element class, then you might have code like
this:

<?code-excerpt "misc/lib/language_tour/libraries/import_as.dart" replace="/(lib\d)\.dart/package:$1\/$&/g"?>
{% prettify dart %}
import 'package:lib1/lib1.dart';
import 'package:lib2/lib2.dart' as lib2;

// Uses Element from lib1.
Element element1 = Element();

// Uses Element from lib2.
lib2.Element element2 = lib2.Element();
{% endprettify %}

#### Importing only part of a library

If you want to use only part of a library, you can selectively import
the library. For example:

<?code-excerpt "misc/lib/language_tour/libraries/show_hide.dart" replace="/(lib\d)\.dart/package:$1\/$&/g"?>
{% prettify dart %}
// Import only foo.
import 'package:lib1/lib1.dart' show foo;

// Import all names EXCEPT foo.
import 'package:lib2/lib2.dart' hide foo;
{% endprettify %}

<a id="deferred-loading"></a>
#### Lazily loading a library

_Deferred loading_ (also called _lazy loading_)
allows an application to load a library on demand,
if and when it's needed.
Here are some cases when you might use deferred loading:

* To reduce an app's initial startup time.
* To perform A/B testing—trying out
  alternative implementations of an algorithm, for example.
* To load rarely used functionality, such as optional screens and dialogs.

To lazily load a library, you must first
import it using `deferred as`.

<?code-excerpt "misc/lib/language_tour/libraries/greeter.dart (import)" replace="/hello\.dart/package:greetings\/$&/g"?>
{% prettify dart %}
import 'package:greetings/hello.dart' deferred as hello;
{% endprettify %}

When you need the library, invoke
`loadLibrary()` using the library's identifier.

<?code-excerpt "misc/lib/language_tour/libraries/greeter.dart (loadLibrary)"?>
{% prettify dart %}
Future greet() async {
  await hello.loadLibrary();
  hello.printGreeting();
}
{% endprettify %}

In the preceding code,
the `await` keyword pauses execution until the library is loaded.
For more information about `async` and `await`,
see [asynchrony support](#asynchrony-support).

You can invoke `loadLibrary()` multiple times on a library without problems.
The library is loaded only once.

Keep in mind the following when you use deferred loading:

* A deferred library's constants aren't constants in the importing file.
  Remember, these constants don't exist until the deferred library is loaded.
* You can't use types from a deferred library in the importing file.
  Instead, consider moving interface types to a library imported by
  both the deferred library and the importing file.
* Dart implicitly inserts `loadLibrary()` into the namespace that you define
  using <code>deferred as <em>namespace</em></code>.
  The `loadLibrary()` function returns a [Future](/guides/libraries/library-tour#future).

<aside class="alert alert-warning" markdown="1">
**Dart VM difference:**
The Dart VM allows access to members of deferred libraries
even before the call to `loadLibrary()`.
This behavior might change, so
**don't depend on the current VM behavior.**
For details, see [issue #33118.](https://github.com/dart-lang/sdk/issues/33118)
</aside>

### Implementing libraries

See
[Create Library Packages](/guides/libraries/create-library-packages)
for advice on how to implement a library package, including:

* How to organize library source code.
* How to use the `export` directive.
* When to use the `part` directive.
* When to use the `library` directive.


<a id="asynchrony"></a>
## Asynchrony support

Dart libraries are full of functions that
return [Future][] or [Stream][] objects.
These functions are _asynchronous_:
they return after setting up
a possibly time-consuming operation
(such as I/O),
without waiting for that operation to complete.

The `async` and `await` keywords support asynchronous programming,
letting you write asynchronous code that
looks similar to synchronous code.


<a id="await"></a>
### Handling Futures

When you need the result of a completed Future,
you have two options:

* Use `async` and `await`.
* Use the Future API, as described
  [in the library tour](/guides/libraries/library-tour#future).

Code that uses `async` and `await` is asynchronous,
but it looks a lot like synchronous code.
For example, here's some code that uses `await`
to wait for the result of an asynchronous function:

<?code-excerpt "misc/lib/language_tour/async.dart (await-lookUpVersion)"?>
{% prettify dart %}
await lookUpVersion();
{% endprettify %}

To use `await`, code must be in an _async function_—a
function marked as `async`:

<?code-excerpt "misc/lib/language_tour/async.dart (checkVersion)" replace="/async|await/[!$&!]/g"?>
{% prettify dart %}
Future checkVersion() [!async!] {
  var version = [!await!] lookUpVersion();
  // Do something with version
}
{% endprettify %}

<aside class="alert alert-info" markdown="1">
**Note:**
Although an async function might perform time-consuming operations,
it doesn't wait for those operations.
Instead, the async function executes only until it encounters
its first `await` expression
([details][synchronous-async-start]).
Then it returns a Future object,
resuming execution only after the `await` expression completes.
</aside>

Use `try`, `catch`, and `finally`
to handle errors and cleanup in code that uses `await`:

<?code-excerpt "misc/lib/language_tour/async.dart (try-catch)"?>
{% prettify dart %}
try {
  version = await lookUpVersion();
} catch (e) {
  // React to inability to look up the version
}
{% endprettify %}

You can use `await` multiple times in an async function.
For example, the following code waits three times
for the results of functions:

<?code-excerpt "misc/lib/language_tour/async.dart (repeated-await)"?>
{% prettify dart %}
var entrypoint = await findEntrypoint();
var exitCode = await runExecutable(entrypoint, args);
await flushThenExit(exitCode);
{% endprettify %}

In <code>await <em>expression</em></code>,
the value of <code><em>expression</em></code> is usually a Future;
if it isn't, then the value is automatically wrapped in a Future.
This Future object indicates a promise to return an object.
The value of <code>await <em>expression</em></code> is that returned object.
The await expression makes execution pause until that object is available.

**If you get a compile-time error when using `await`,
make sure `await` is in an async function.**
For example, to use `await` in your app's `main()` function,
the body of `main()` must be marked as `async`:

<?code-excerpt "misc/lib/language_tour/async.dart (main)" replace="/async|await/[!$&!]/g"?>
{% prettify dart %}
Future main() [!async!] {
  checkVersion();
  print('In main: version is ${[!await!] lookUpVersion()}');
}
{% endprettify %}


<a id="async"></a>
### Declaring async functions

An _async function_ is a function whose body is marked with
the `async` modifier.

Adding the `async` keyword to a function makes it return a Future.
For example, consider this synchronous function,
which returns a String:

<?code-excerpt "misc/lib/language_tour/async.dart (sync-lookUpVersion)"?>
{% prettify dart %}
String lookUpVersion() => '1.0.0';
{% endprettify %}

If you change it to be an async function—for example,
because a future implementation will be time consuming—the
returned value is a Future:

<?code-excerpt "misc/lib/language_tour/async.dart (async-lookUpVersion)"?>
{% prettify dart %}
Future<String> lookUpVersion() async => '1.0.0';
{% endprettify %}

Note that the function's body doesn't need to use the Future API.
Dart creates the Future object if necessary.

If your function doesn't return a useful value,
make its return type `Future<void>`.

{% comment %}
PENDING: add example here

Where else should we cover generalized void?
{% endcomment %}


<a id="await-for"></a>
### Handling Streams

When you need to get values from a Stream,
you have two options:

* Use `async` and an _asynchronous for loop_ (`await for`).
* Use the Stream API, as described
  [in the library tour](/guides/libraries/library-tour#stream).

<aside class="alert alert-warning" markdown="1">
**Note:**
Before using `await for`, be sure that it makes the code clearer
and that you really do want to wait for all of the stream's results.
For example, you usually should **not** use `await for` for UI event listeners,
because UI frameworks send endless streams of events.
</aside>

An asynchronous for loop has the following form:

<?code-excerpt "misc/lib/language_tour/async.dart (await-for)"?>
{% prettify dart %}
await for (varOrType identifier in expression) {
  // Executes each time the stream emits a value.
}
{% endprettify %}

The value of <code><em>expression</em></code> must have type Stream.
Execution proceeds as follows:

1. Wait until the stream emits a value.
2. Execute the body of the for loop,
   with the variable set to that emitted value.
3. Repeat 1 and 2 until the stream is closed.

To stop listening to the stream,
you can use a `break` or `return` statement,
which breaks out of the for loop
and unsubscribes from the stream.

**If you get a compile-time error when implementing an asynchronous for loop,
make sure the `await for` is in an async function.**
For example, to use an asynchronous for loop in your app's `main()` function,
the body of `main()` must be marked as `async`:

<?code-excerpt "misc/lib/language_tour/async.dart (number_thinker)" replace="/async|await for/[!$&!]/g"?>
{% prettify dart %}
Future main() [!async!] {
  // ...
  [!await for!] (var request in requestServer) {
    handleRequest(request);
  }
  // ...
}
{% endprettify %}

For more information about asynchronous programming, in general, see the
[dart:async](/guides/libraries/library-tour#dartasync---asynchronous-programming)
section of the library tour.
Also see the articles
[Dart Language Asynchrony Support: Phase 1](/articles/language/await-async)
and
[Dart Language Asynchrony Support: Phase 2](/articles/language/beyond-async),
and the [Dart language specification](/guides/language/spec).


<a id="generator"></a>
## Generators

When you need to lazily produce a sequence of values,
consider using a _generator function_.
Dart has built-in support for two kinds of generator functions:

* **Synchronous** generator: Returns an **[Iterable]** object.
* **Asynchronous** generator: Returns a **[Stream]** object.

To implement a **synchronous** generator function,
mark the function body as `sync*`,
and use `yield` statements to deliver values:

<?code-excerpt "misc/test/language_tour/async_test.dart (sync-generator)"?>
{% prettify dart %}
Iterable<int> naturalsTo(int n) sync* {
  int k = 0;
  while (k < n) yield k++;
}
{% endprettify %}

To implement an **asynchronous** generator function,
mark the function body as `async*`,
and use `yield` statements to deliver values:

<?code-excerpt "misc/test/language_tour/async_test.dart (async-generator)"?>
{% prettify dart %}
Stream<int> asynchronousNaturalsTo(int n) async* {
  int k = 0;
  while (k < n) yield k++;
}
{% endprettify %}

If your generator is recursive,
you can improve its performance by using `yield*`:

<?code-excerpt "misc/test/language_tour/async_test.dart (recursive-generator)"?>
{% prettify dart %}
Iterable<int> naturalsDownFrom(int n) sync* {
  if (n > 0) {
    yield n;
    yield* naturalsDownFrom(n - 1);
  }
}
{% endprettify %}

For more information about generators, see the article
[Dart Language Asynchrony Support: Phase 2](/articles/language/beyond-async).


## Callable classes

To allow your Dart class to be called like a function,
implement the `call()` method.

In the following example, the `WannabeFunction` class defines
a call() function that takes three strings and concatenates them,
separating each with a space, and appending an exclamation.
Click the run button {% asset red-run.png alt="" %} to execute the code.

{% comment %}
https://gist.github.com/405379bacf30335f3aed
https://dartpad.dartlang.org/405379bacf30335f3aed

<?code-excerpt "misc/lib/language_tour/callable_classes.dart"?>
{% prettify dart %}
class WannabeFunction {
  String call(String a, String b, String c) => '$a $b $c!';
}

var wf = WannabeFunction();
var out = wf('Hi', 'there,', 'gang');

main() => print(out);
{% endprettify %}
{% endcomment %}

<iframe
src="{{site.custom.dartpad.embed-inline-prefix}}?id=405379bacf30335f3aed&verticalRatio=73"
    width="100%"
    height="240px"
    style="border: 1px solid #ccc;">
</iframe>

For more information on treating classes like functions, see
[Emulating Functions in Dart](/articles/language/emulating-functions).

## Isolates

Most computers, even on mobile platforms, have multi-core CPUs.
To take advantage of all those cores, developers traditionally use
shared-memory threads running concurrently. However, shared-state
concurrency is error prone and can lead to complicated code.

Instead of threads, all Dart code runs inside of *isolates*. Each
isolate has its own memory heap, ensuring that no isolate’s state is
accessible from any other isolate.

For more information, see the
[dart:isolate library documentation.][dart:isolate]


## Typedefs

In Dart, functions are objects, just like strings and numbers are
objects. A *typedef*, or *function-type alias*, gives a function type a
name that you can use when declaring fields and return types. A typedef
retains type information when a function type is assigned to a variable.

Consider the following code, which doesn't use a typedef:

<?code-excerpt "misc/lib/language_tour/typedefs/sorted_collection_1.dart"?>
{% prettify dart %}
class SortedCollection {
  Function compare;

  SortedCollection(int f(Object a, Object b)) {
    compare = f;
  }
}

// Initial, broken implementation.
int sort(Object a, Object b) => 0;

void main() {
  SortedCollection coll = SortedCollection(sort);

  // All we know is that compare is a function,
  // but what type of function?
  assert(coll.compare is Function);
}
{% endprettify %}

Type information is lost when assigning `f` to `compare`. The type of
`f` is `(Object, ``Object)` → `int` (where → means returns), yet the
type of `compare` is Function. If we change the code to use explicit
names and retain type information, both developers and tools can use
that information.

<?code-excerpt "misc/lib/language_tour/typedefs/sorted_collection_2.dart"?>
{% prettify dart %}
typedef Compare = int Function(Object a, Object b);

class SortedCollection {
  Compare compare;

  SortedCollection(this.compare);
}

// Initial, broken implementation.
int sort(Object a, Object b) => 0;

void main() {
  SortedCollection coll = SortedCollection(sort);
  assert(coll.compare is Function);
  assert(coll.compare is Compare);
}
{% endprettify %}

<aside class="alert alert-info" markdown="1">
**Note:**
Currently, typedefs are restricted to function types. We expect this
to change.
</aside>

Because typedefs are simply aliases, they offer a way to check the type
of any function. For example:

<?code-excerpt "misc/lib/language_tour/typedefs/misc.dart (compare)"?>
{% prettify dart %}
typedef Compare<T> = int Function(T a, T b);

int sort(int a, int b) => a - b;

void main() {
  assert(sort is Compare<int>); // True!
}
{% endprettify %}

## Metadata

Use metadata to give additional information about your code. A metadata
annotation begins with the character `@`, followed by either a reference
to a compile-time constant (such as `deprecated`) or a call to a
constant constructor.

Two annotations are available to all Dart code: `@deprecated` and
`@override`. For examples of using `@override`,
see [Extending a class](#extending-a-class).
Here’s an example of using the `@deprecated`
annotation:

<?code-excerpt "misc/lib/language_tour/metadata/television.dart (deprecated)" replace="/@deprecated/[!$&!]/g"?>
{% prettify dart %}
class Television {
  /// _Deprecated: Use [turnOn] instead._
  [!@deprecated!]
  void activate() {
    turnOn();
  }

  /// Turns the TV's power on.
  void turnOn() {...}
}
{% endprettify %}

You can define your own metadata annotations. Here’s an example of
defining a @todo annotation that takes two arguments:

<?code-excerpt "misc/lib/language_tour/metadata/todo.dart"?>
{% prettify dart %}
library todo;

class Todo {
  final String who;
  final String what;

  const Todo(this.who, this.what);
}
{% endprettify %}

And here’s an example of using that @todo annotation:

<?code-excerpt "misc/lib/language_tour/metadata/misc.dart"?>
{% prettify dart %}
import 'todo.dart';

@Todo('seth', 'make this do something')
void doSomething() {
  print('do something');
}
{% endprettify %}

Metadata can appear before a library, class, typedef, type parameter,
constructor, factory, function, field, parameter, or variable
declaration and before an import or export directive. You can
retrieve metadata at runtime using reflection.


## Comments

Dart supports single-line comments, multi-line comments, and
documentation comments.


### Single-line comments

A single-line comment begins with `//`. Everything between `//` and the
end of line is ignored by the Dart compiler.

<?code-excerpt "misc/lib/language_tour/comments.dart (single-line-comments)"?>
{% prettify dart %}
void main() {
  // TODO: refactor into an AbstractLlamaGreetingFactory?
  print('Welcome to my Llama farm!');
}
{% endprettify %}


### Multi-line comments

A multi-line comment begins with `/*` and ends with `*/`. Everything
between `/*` and `*/` is ignored by the Dart compiler (unless the
comment is a documentation comment; see the next section). Multi-line
comments can nest.

<?code-excerpt "misc/lib/language_tour/comments.dart (multi-line-comments)"?>
{% prettify dart %}
void main() {
  /*
   * This is a lot of work. Consider raising chickens.

  Llama larry = Llama();
  larry.feed();
  larry.exercise();
  larry.clean();
   */
}
{% endprettify %}


### Documentation comments

Documentation comments are multi-line or single-line comments that begin
with `///` or `/**`. Using `///` on consecutive lines has the same
effect as a multi-line doc comment.

Inside a documentation comment, the Dart compiler ignores all text
unless it is enclosed in brackets. Using brackets, you can refer to
classes, methods, fields, top-level variables, functions, and
parameters. The names in brackets are resolved in the lexical scope of
the documented program element.

Here is an example of documentation comments with references to other
classes and arguments:

<?code-excerpt "misc/lib/language_tour/comments.dart (doc-comments)"?>
{% prettify dart %}
/// A domesticated South American camelid (Lama glama).
///
/// Andean cultures have used llamas as meat and pack
/// animals since pre-Hispanic times.
class Llama {
  String name;

  /// Feeds your llama [Food].
  ///
  /// The typical llama eats one bale of hay per week.
  void feed(Food food) {
    // ...
  }

  /// Exercises your llama with an [activity] for
  /// [timeLimit] minutes.
  void exercise(Activity activity, int timeLimit) {
    // ...
  }
}
{% endprettify %}

In the generated documentation, `[Food]` becomes a link to the API docs
for the Food class.

To parse Dart code and generate HTML documentation, you can use the SDK’s
[documentation generation tool.](https://github.com/dart-lang/dartdoc#dartdoc)
For an example of generated documentation, see the [Dart API
documentation.]({{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}) For advice on how to structure
your comments, see
[Guidelines for Dart Doc Comments.](/guides/language/effective-dart/documentation)


## Summary

This page summarized the commonly used features in the Dart language.
More features are being implemented, but we expect that they won’t break
existing code. For more information, see the [Dart Language
Specification](/guides/language/spec) and
[Effective Dart](/guides/language/effective-dart).

To learn more about Dart's core libraries, see
[A Tour of the Dart Libraries](/guides/libraries/library-tour).

[AssertionError]: {{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/AssertionError-class.html
[dart2js]: {{site.webdev}}/tools/dart2js
[dart:html]: {{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-html
[dart:isolate]: {{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-isolate
[dart:math]: {{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-math
[dart]: /dart-vm/tools/dart-vm
[dartdevc]: {{site.webdev}}/tools/dartdevc
[DON’T use const redundantly]: /guides/language/effective-dart/usage#dont-use-const-redundantly
[double]: {{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/double-class.html
[Error]: {{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Error-class.html
[Exception]: {{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Exception-class.html
[Flutter]: https://flutter.io
[Flutter debug mode]: https://flutter.io/debugging/#debug-mode-assertions
[forEach()]: {{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Iterable/forEach.html
[Function API reference]: {{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Function-class.html
[Future]: {{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-async/Future-class.html
[identical()]: {{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/identical.html
[int]: {{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/int-class.html
[Iterable]: {{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Iterable-class.html
[js numbers]: https://stackoverflow.com/questions/2802957/number-of-bits-in-javascript-numbers/2803010#2803010
[List]: {{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/List-class.html
[Map]: {{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Map-class.html
[meta]: {{site.pub-pkg}}/meta
[num]: {{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/num-class.html
[@required]: {{site.pub-api}}/meta/latest/meta/required-constant.html
[Object]: {{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Object-class.html
[ObjectVsDynamic]: /guides/language/effective-dart/design#do-annotate-with-object-instead-of-dynamic-to-indicate-any-object-is-allowed
[StackTrace]: {{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/StackTrace-class.html
[Stream]: {{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-async/Stream-class.html
[String]: {{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/String-class.html
[Symbol]: {{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Symbol-class.html
[synchronous-async-start]: https://github.com/dart-lang/sdk/blob/master/docs/newsletter/20170915.md#synchronous-async-start
[Type]: {{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Type-class.html
