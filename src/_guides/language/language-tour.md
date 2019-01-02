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


## Операторы

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


### Побитовые операторы и операторы сдвига

Вы можете манипулировать отдельными битами чисел в Dart.
Обычны, вы используете эти побитовые и операторы сдвига с целыми числами.

|-----------------------------+-------------------------------------------|
| Оператор                    | Смысл                                   |
|-----------------------------+-------------------------------------------|
| `&`                         | И
| `|`                         | ИЛИ
| `^`                         | Исключающее ИЛИ
| <code>~<em>выражение</em></code> | Унарная побитовая инверсия (из 0 получаем 1; из 1 получим 0)
| `<<`                        | Сдвиг влево
| `>>`                        | Сдвиг вправо
{:.table .table-striped}
Далее примеры использования операторов сдвига и побитовых операторов:

<?code-excerpt "misc/test/language_tour/operators_test.dart (op-bitwise)"?>
{% prettify dart %}
final value = 0x22;
final bitmask = 0x0f;

assert((value & bitmask) == 0x02); // AND
assert((value & ~bitmask) == 0x20); // AND NOT
assert((value | bitmask) == 0x2f); // OR
assert((value ^ bitmask) == 0x2d); // XOR
assert((value << 4) == 0x220); // Сдвиг влево
assert((value >> 4) == 0x02); // Сдвиг вправо
{% endprettify %}


### Условные выражения

У Dart есть два оператора, позволяющих вычислять выражения условно,
вместо использования инструкции [if-else](#if-and-else) в отдельных случаях:

<code><em>условие</em> ? <em>выражение1</em> : <em>выражение2</em>
: Если _условие_ истино, вычисляется _выражение1_ (и возвращается это значение);
  во всех остальных случаях вычисляется и возвращается значение _выражение2_.

<code><em>выражение1</em> ?? <em>выражение2</em></code>
: Если _выражение1_ не null, возвращается это значение;
  в ином случае вычисляет и возвращает значение _выражение2_.

Когда вам необходимо присвоить значение на основе логического
выражения, расмотрите использование `?:`.

<?code-excerpt "misc/lib/language_tour/operators.dart (if-then-else-operator)"?>
{% prettify dart %}
var visibility = isPublic ? 'public' : 'private';
{% endprettify %}

Если логическое значение проверяет на null,
попробуйте воспользоваться `??`.

<?code-excerpt "misc/test/language_tour/operators_test.dart (if-null)"?>
{% prettify dart %}
String playerName(String name) => name ?? 'Гость';
{% endprettify %}

Следующий пример мог быть написан по меньшей мере двумя другими способами,
но не так кратко:

<?code-excerpt "misc/test/language_tour/operators_test.dart (if-null-alt)"?>
{% prettify dart %}
// Чуть более длинная версия использует оператор ?:
String playerName(String name) => name != null ? name : 'Гость';

// Очень длинна версия использует инструкцию if-else
String playerName(String name) {
  if (name != null) {
    return name;
  } else {
    return 'Гость';
  }
}
{% endprettify %}

<a id="cascade"></a>
### Каскадная запись (..)

Каскад (`..`) позволяет вам делать последовательность операций над одним
и тем же объектом. В дополнение к вызовам функций,
вы также можете обращаться к полям одного и того же объекта.
Это часто спасает вас от создания временных переменных и
позволяет вам писать более гибкий код.

Расмотрите следующий код:

<?code-excerpt "misc/test/language_tour/browser_test.dart (cascade-operator)"?>
{% prettify dart %}
querySelector('#confirm') // Получение объекта
  ..text = 'Confirm' // Использование его поля
  ..classes.add('important')
  ..onClick.listen((e) => window.alert('Confirmed!'));
{% endprettify %}

Первый вызов метода `querySelector()`, возвращает выбранный объект.
Код, который следует за каскадной нотацией, работает с этим объектом,
игнорируя любые последующие значения, которые могут быть возвращены.

Предыдущий пример равносилен этому:

<?code-excerpt "misc/test/language_tour/browser_test.dart (cascade-operator-example-expanded)"?>
{% prettify dart %}
var button = querySelector('#confirm');
button.text = 'Confirm';
button.classes.add('important');
button.onClick.listen((e) => window.alert('Confirmed!'));
{% endprettify %}

Вы также можете делать вложенный каскад. Например:

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

Будьте осторожны при создании каскада для функции,
которая не возвращает реальный объект.
Например, следующий код не работает:

<?code-excerpt "misc/lib/language_tour/operators.dart (cannot-cascade-on-void)" plaster="none"?>
{% prettify dart %}
var sb = StringBuffer();
sb.write('foo')
  ..write('bar'); // Ошибка: метод 'write' не определён для 'void'.
{% endprettify %}

Вызов `sb.write()` возвращает void,
и вы не можете создать каскад с `void`.

<div class="alert alert-info" markdown="1">
**Замечание:**
Строго говоря, "две точки" для каскада - не оператор.
Это просто часть синтаксиса Dart.
</div>

### Другие операторы

Вы видели большинство оставшихся операторов в других примерах:

|----------+-------------------------------------------|
| Оператор | Имя                 |          Смысл   |
|-----------+------------------------------------------|
| `()`     | Применение функции | Вызов функции
| `[]`     | Доступ к списку    | Сослаться к значению по указаному индексу в списке
| `.`      | Доступ к члену     | Сослаться к свойству выражения, например: `foo.bar` выбирает свойство `bar` из выражения `foo`
| `?.`     | Условный доступ к полю | Как `.`, но крайний слева операнд может быть null; например: `foo?.bar`   выбирает поле `bar` из выражения `foo` если только `foo` не null (в этом случае значение `foo?.bar` - null)
{:.table .table-striped}

Для большей информации об операторах: `.`, `?.`, и `..`, смотрите
[Классы](#classes).


## Иструкции управления потоком

Вы можете использовать в своём коде на Dart любые из следующих конструкций
управления потоком:

-   `if` и `else`
-   цикл `for`
-   циклы `while` и `do`-`while`
-   `break` и `continue`
-   `switch` и `case`
-   `assert`

Вы можете влиять на поток управления, используя `try-catch` и `throw`, как
объясняется в [Исключениях](#exceptions).


### If и else

Dart поддерживает инструкцию `if` с необязательной частью `else`, как будет
показано в следующем примере. Также смотрите [условные выражения](#conditional-expressions).

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

В отличии от JavaScript, условия должны использовать логические значения, ничего друго кроме них.
Смотрите [Логические значения](#booleans) для большей информации.


### Цикл For

Вы можете итерироваться с помощью стандартного цикла `for`. Например:

<?code-excerpt "misc/test/language_tour/control_flow_test.dart (for)"?>
{% prettify dart %}
var message = StringBuffer('Dart - это весело');
for (var i = 0; i < 5; i++) {
  message.write('!');
}
{% endprettify %}

Замыкание внутри цикла `for` в Dart захватывают _значение_ индекса,
избегая распространённой ловушки, найденной в JavaScript. Например, рассмотрим:

<?code-excerpt "misc/test/language_tour/control_flow_test.dart (for-and-closures)"?>
{% prettify dart %}
var callbacks = [];
for (var i = 0; i < 2; i++) {
  callbacks.add(() => print(i));
}
callbacks.forEach((c) => c());
{% endprettify %}

Выведет `0`, а потом `1` как и ожидалось. В противовес этому, в JavaScript будет
напечатано `2`, а потом  `2`.

Если объект, который вы итерируете - Iterable, вы можете использовать
метод [forEach()][]. Использование `forEach()` хороший вариант, если вам не нужно
знать текущий счётчик итераций:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (forEach)"?>
{% prettify dart %}
candidates.forEach((candidate) => candidate.interview());
{% endprettify %}

Итерируемые классы, такие как List и Set также поддерживают форму
[итерации](/guides/libraries/library-tour#iteration) `for-in` :

<?code-excerpt "misc/test/language_tour/control_flow_test.dart (collection)"?>
{% prettify dart %}
var collection = [0, 1, 2];
for (var x in collection) {
  print(x); // 0 1 2
}
{% endprettify %}


### While и do-while

Цикл `while` вычисляет условие перед телом цикла:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (while)"?>
{% prettify dart %}
while (!isDone()) {
  doSomething();
}
{% endprettify %}

Цикл `do`-`while` вычисляет условие *после* тела цикла:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (do-while)"?>
{% prettify dart %}
do {
  printLine();
} while (!atEndOfPage());
{% endprettify %}


### Break и continue

Используйте `break` для остановки цикла:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (while-break)"?>
{% prettify dart %}
while (true) {
  if (shutDownRequested()) break;
  processIncomingRequests();
}
{% endprettify %}

Используйте `continue` чтобы пропустить следующую итерацию цикла:

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

Вы можете написать этот пример по-другому,
если вы используете [Iterable][], такой как список или множество:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (where)"?>
{% prettify dart %}
candidates
    .where((c) => c.yearsExperience >= 5)
    .forEach((c) => c.interview());
{% endprettify %}


### Switch и case

Инструкция switch в Dart сравнивает целые числа, строки или константы
времени компиляции, используя `==`. Все сравниваемые объекты должны быть
экземплярами одного и того же класса (и не какими-либо его подтипами),
и этот класс не должен переопределять `==`.
[Перечислимые типы](#enumerated-types) отлично работают в инструкциях `switch`.

<div class="alert alert-info" markdown="1">
**Замечание:**
Инструкция switch в Dart служит для ограниченных случаев, таких как
переводчики или анализаторы.
</div>

Каждый не пустой пункт `case` заканчивается спомощью инструкции `break`, как правило.
Другие допустимые способы для завершения не пустых пунктов `case` инструкции `continue`,
`throw` или `return`.

Используйте пункт `default` для исполнения кода, когда никакой из пунктов
`case` не подошёл:

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

В следующем примере в `case` пропускается инструкция `break`, что приводит к ошибке:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (switch-break-omitted)" plaster="none"?>
{% prettify dart %}
var command = 'OPEN';
switch (command) {
  case 'OPEN':
    executeOpen();
    // Ошибка: пропущен break

  case 'CLOSED':
    executeClosed();
    break;
}
{% endprettify %}

Тем не менее, Dart поддерживает пустой `case`, разрешая
делать "проваливание":

<?code-excerpt "misc/lib/language_tour/control_flow.dart (switch-empty-case)"?>
{% prettify dart %}
var command = 'CLOSED';
switch (command) {
  case 'CLOSED': // Пустой case проваливается
  case 'NOW_CLOSED':
    //  Запускает для CLOSED и NOW_CLOSED.
    executeNowClosed();
    break;
}
{% endprettify %}

Если вы хотите сделать "проваливание" в непустом `case`, вы можете
использовать инструкцию `continue` и метку:

<?code-excerpt "misc/lib/language_tour/control_flow.dart (switch-continue)"?>
{% prettify dart %}
var command = 'CLOSED';
switch (command) {
  case 'CLOSED':
    executeClosed();
    continue nowClosed;
  // Продолжает исполнение с метки nowClosed.
  nowClosed:
  case 'NOW_CLOSED':
    //  Запускает для CLOSED и NOW_CLOSED.
    executeNowClosed();
    break;
}
{% endprettify %}

Пункт `case` может иметь локальные переменные, которые видны только внутри области
видимости этого пункта.


### Assert

Используйте инструкцию `assert` чтобы прервать нормальное исполнение, если
логическое условие ложно. Вы можете найти примеры инструкции `assert` на протяжении этого тура.
Вот ещё немного:

<?code-excerpt "misc/test/language_tour/control_flow_test.dart (assert)"?>
{% prettify dart %}
// Удоствериться, что значение не равно null
assert(text != null);

// Удоствериться, что значение меньше 100
assert(number < 100);

// Удоствериться, что это URL с https
assert(urlString.startsWith('https'));
{% endprettify %}

<div class="alert alert-info" markdown="1">
**Замечание:**
Инструкция assert не оказывает влияние на продакшен код;
она только для разработки.
Во Flutter assert доступен в [отладочном режиме][Flutter debug mode].
Утилиты предназначенные только для разработки, такие как [dartdevc][]
поддерживают assert поумолчанию.
Некоторые инструменты, такие как [dart][] и [dart2js][dart2js],
поддерживают assert через флаг коммандной строки: `--enable-asserts`.
</div>

Чтобы добавить сообщение к assert,
добавте строку вторым аргументом.

<?code-excerpt "misc/test/language_tour/control_flow_test.dart (assert-with-message)"?>
{% prettify dart %}
assert(urlString.startsWith('https'),
    'URL ($urlString) должен начинаться с "https".');
{% endprettify %}

Первый аргумент `assert` может быть выражением, которое вычисляется в логическое значение.
Если значение выражения - true, утверждение успешно и исполнение кода продолжается.
Если оно - false, утверждение неуспешно и бросается исключение [AssertionError][].


## Исключения

Ваш код на Dart может бросать и ловить исключения. Исключения - ошибки,
сообщающие, что произошло что-то неожиданное. Если исключение не поймано,
изолят, который возбудил исключение - приостанавливается и обычно, изолят
и его программа прерывается.

В отличии от Java, все исключения в Dart - непроверяемые.
Методы не объявляют какие исключения они могут бросить, и от вас
не требуется отлавливать любые исключения.

Dart предоставляет типы [Exception][] и [Error][],
а также многочисленные предопределенные подтипы.
Вы можете, конечно, определить ваше собственное исключение.
Тем неменее, Dart программы могут бросать любые не null объекты как исключения,
не только Exception и Error.

### Throw

Здесь пример бросания или *возбуждение* исключения:

<?code-excerpt "misc/lib/language_tour/exceptions.dart (throw-FormatException)"?>
{% prettify dart %}
throw FormatException('Ожидается хотя бы 1 раздел');
{% endprettify %}

Вы также можете бросать произвольные объекты:

<?code-excerpt "misc/lib/language_tour/exceptions.dart (out-of-llamas)"?>
{% prettify dart %}
throw 'Out of llamas!';
{% endprettify %}

<div class="alert alert-info" markdown="1">
  **Замечание:**
  Грамотный и качественный код обычно бросает типы,
  реализующие [Error][] или [Exception][].
</div>

Поскольку генерирование исключения является выражением,
вы можете бросать исключения в операторах =\>,
а также в любом другом месте, где разрешены выражения:

<?code-excerpt "misc/lib/language_tour/exceptions.dart (throw-is-an-expression)"?>
{% prettify dart %}
void distanceTo(Point other) => throw UnimplementedError();
{% endprettify %}


### Catch

Ловля или захват исключения останавливает распространение исключения
(только если вы не пробросите его дальше).
Захват исключения даёт вам возможность обработать его:

<?code-excerpt "misc/lib/language_tour/exceptions.dart (try)"?>
{% prettify dart %}
try {
  breedMoreLlamas();
} on OutOfLlamasException {
  buyMoreLlamas();
}
{% endprettify %}

Для обработки кода, который может генерировать более одного типа исключения
вы можете указать много пунктов захвата.
Первый пункт захвата, соответствующий типу брошенного объекта, обрабатывает исключение.
Если у пункта захвата не указан тип, то этот пункт может обработать бросаемый объект с любыми типом.

<?code-excerpt "misc/lib/language_tour/exceptions.dart (try-catch)"?>
{% prettify dart %}
try {
  breedMoreLlamas();
} on OutOfLlamasException {
  // Тут указан тип исключения
  buyMoreLlamas();
} on Exception catch (e) {
  // Все остальное, что является исключением
  print('Неизвестное исключение: $e');
} catch (e) {
  // Не указан тип, обрабатывает всё
  print('Что-то действительно неизвестное: $e');
}
{% endprettify %}

Как показано в предыдущем коде, вы можете использовать `on` или `catch` или всё вместе.
Используйте `on`, когда вам необходимо указать тип исключения.
Используйте `catch`, когда ваш обработчик исключения нуждается в объекте исключения.

Вы можете указать один или два параметра для `catch()`.
Первый - брошенное исключение,
второй - трассировки стека (объект [StackTrace][]).

<?code-excerpt "misc/lib/language_tour/exceptions.dart (try-catch-2)" replace="/\(e.*?\)/[!$&!]/g"?>
{% prettify dart %}
try {
  // ···
} on Exception catch [!(e)!] {
  print('Подробности об исключении:\n $e');
} catch [!(e, s)!] {
  print('Подробности об исключении:\n $e');
  print('Трассировка стека:\n $s');
}
{% endprettify %}

Чтобы частично обработать исключение и разрешить его распространение,
используйте ключевое слово `rethrow`.

<?code-excerpt "misc/test/language_tour/exceptions_test.dart (rethrow)" replace="/rethrow;/[!$&!]/g"?>
{% prettify dart %}
void misbehave() {
  try {
    dynamic foo = true;
    print(foo++); // Ошибка времени исполнения
  } catch (e) {
    print('misbehave() частично обработана ${e.runtimeType}.');
    [!rethrow;!] // Позволяет вызывающим увидеть исключение.
  }
}

void main() {
  try {
    misbehave();
  } catch (e) {
    print('main() завершение обработки ${e.runtimeType}.');
  }
}
{% endprettify %}


### Finally

Чтобы убедиться, что какой-то код выполняется независимо от того, выдано исключение или нет,
используйте пункт `finally`. Если исключение не подошло ни одному пункту `catch`,
исключение будет рапространено дальше после выполнения пункта `finally`:

<?code-excerpt "misc/lib/language_tour/exceptions.dart (finally)"?>
{% prettify dart %}
try {
  breedMoreLlamas();
} finally {
  // Всегда убираться, даже если выдается исключение.
  cleanLlamaStalls();
}
{% endprettify %}

Пункт `finally` запускается после любого подошедшего пункта `catch`:

<?code-excerpt "misc/lib/language_tour/exceptions.dart (try-catch-finally)"?>
{% prettify dart %}
try {
  breedMoreLlamas();
} catch (e) {
  print('Error: $e'); // Сначала обработать исключение.
} finally {
  cleanLlamaStalls(); // Потом убраться.
}
{% endprettify %}

Узнайте больше, прочитав раздел
[Исключения](/guides/libraries/library-tour#exceptions)
тура по библиотеке.

## Классы

Dart - объектно-ориентированный язык с классами и наследованием, основанным на миксинах.
Каждый объект - экземпляр класса и все классы происходят от [Object][Object].
*Наследование, основанное на миксинах* означает, что каждый класс (за исключением Object)
имеет ровно один суперкласс, а тело класса может быть переиспользовано во множестве
иерархией классов.

### Использование членов класса

У объектов есть *члены*, состоящие из функций и данных (*методы* и *переменные экземпляра*, соответственно).
Когда вы вызывает метод, вы вызываете его на объекте: метод имеет доступ к функциям
и данным объекта.

Используйте точку (`.`) чтобы обратиться к переменной экземпляра или методу:

<?code-excerpt "misc/test/language_tour/classes_test.dart (object-members)"?>
{% prettify dart %}
var p = Point(2, 2);

// Установка значения переменной экземпляра y.
p.y = 3;

// Получение значения y.
assert(p.y == 3);

// Вызов метода distanceTo() объекта p.
num distance = p.distanceTo(Point(4, 4));
{% endprettify %}

Используйте `?.` вместо `.` чтобы избежать исключения,
когда крайний слева операнд null:

{% comment %}
https://dartpad.dartlang.org/0cb25997742ed5382e4a
https://gist.github.com/0cb25997742ed5382e4a
{% endcomment %}

<?code-excerpt "misc/test/language_tour/classes_test.dart (safe-member-access)"?>
{% prettify dart %}
// Если p не null, установить его полю y значение 4.
p?.y = 4;
{% endprettify %}


### Использование конструкторов

Вы можете создать объект, используя *конструктор*.
Имя конструктора может быть либо <code><em>ИмяКласса</em></code> или
<code><em>ИмяКласса</em>.<em>идентификатор</em></code>. Например,
следующий код создаёт объект `Point`, используя конструкторы
`Point()` и `Point.fromJson()`:

<?code-excerpt "misc/test/language_tour/classes_test.dart (object-creation)" replace="/ as .*?;/;/g"?>
{% prettify dart %}
var p1 = Point(2, 2);
var p2 = Point.fromJson({'x': 1, 'y': 2});
{% endprettify %}

Следующий код работатет точно также, но
использует необязательное ключевое слово `new` перед именем конструктора:

<?code-excerpt "misc/test/language_tour/classes_test.dart (object-creation-new)" replace="/ as .*?;/;/g"?>
{% prettify dart %}
var p1 = new Point(2, 2);
var p2 = new Point.fromJson({'x': 1, 'y': 2});
{% endprettify %}

<aside class="alert alert-info" markdown="1">
  **Замечание по версии:** Ключевое слово `new` стало опциональным в Dart 2.
</aside>

Некоторые классы предоставляют [конструкторы констант](#constant-constructors).
Для создания констант времени компиляции используется константный конструктор,
поместите ключевое слово `const` перед именем конструктора:

<?code-excerpt "misc/test/language_tour/classes_test.dart (const)"?>
{% prettify dart %}
var p = const ImmutablePoint(2, 2);
{% endprettify %}

Конструирование двух идентичных констант времени компиляции
даёт в результате один и тот же экземпляр:

<?code-excerpt "misc/test/language_tour/classes_test.dart (identical)"?>
{% prettify dart %}
var a = const ImmutablePoint(1, 1);
var b = const ImmutablePoint(1, 1);

assert(identical(a, b)); // Один и тот же экземпляр!
{% endprettify %}

Внутри _константного контекста_, вы можете опустить `const` перед конструктором или литералом.
Например, посмотрите на этот код, который создаёт константную мапу:

<?code-excerpt "misc/test/language_tour/classes_test.dart (const-context-withconst)" replace="/pointAndLine1/pointAndLine/g"?>
{% prettify dart %}
// Здесь много ключевых слов const.
const pointAndLine = const {
  'point': const [const ImmutablePoint(0, 0)],
  'line': const [const ImmutablePoint(1, 10), const ImmutablePoint(-2, 11)],
};
{% endprettify %}

Вы можете опустить все, кроме первого ключевого слова `const`:

<?code-excerpt "misc/test/language_tour/classes_test.dart (const-context-noconst)" replace="/pointAndLine2/pointAndLine/g"?>
{% prettify dart %}
// Только один const, который устанавливает константный контекст.
const pointAndLine = {
  'point': [ImmutablePoint(0, 0)],
  'line': [ImmutablePoint(1, 10), ImmutablePoint(-2, 11)],
};
{% endprettify %}

Если константный конструктор вне константного контекста и вызывается без `const`,
создаётся **не константный объект**:

<?code-excerpt "misc/test/language_tour/classes_test.dart (nonconst-const-constructor)"?>
{% prettify dart %}
var a = const ImmutablePoint(1, 1); // Создаётся константа
var b = ImmutablePoint(1, 1); // Здесь не будет создана константа

assert(!identical(a, b)); // Не один и тот же экземпляр
{% endprettify %}

<aside class="alert alert-info" markdown="1">
  **Замечание по версии:**
  Ключевое слово `const` стало опциональным внутри константного
  контекста в Dart 2.
</aside>


### Получение типа объектов

Чтобы получить тип объектов во время исполнения,
вы можете использовать свойство `runtimeType`, которое
возвращает объект [Type][].

<?code-excerpt "misc/test/language_tour/classes_test.dart (runtimeType)"?>
{% prettify dart %}
print('Тип a: ${a.runtimeType}');
{% endprettify %}

До этого, вы видели как _использовать_ классы.
Остальное из этого раздела показывает как классы _реализуются_.


### Переменные экземпляра

Вот как объявляются переменные экземпляра:

<?code-excerpt "misc/lib/language_tour/classes/point_with_main.dart (class)"?>
{% prettify dart %}
class Point {
  num x; // Объявление переменной экземпляра x, исходное значение null.
  num y; // Объявление y, исходное значение null.
  num z = 0; // Объявление z, исходное значение 0.
}
{% endprettify %}

Все неинициализированные переменные экземпляра имеют значение `null`.

Все переменные экземпляра неявно генерируют метод *getter*.
Не финальные переменные экземпляра также генерируют неявно метод *setter*.
За деталями смотрите [Getter'ы and setter'ы](#getters-and-setters).

<?code-excerpt "misc/lib/language_tour/classes/point_with_main.dart (class+main)" replace="/(num .*?;).*/$1/g" plaster="none"?>
{% prettify dart %}
class Point {
  num x;
  num y;
}

void main() {
  var point = Point();
  point.x = 4; // Использование метода setter для x.
  assert(point.x == 4); // Использование метода getter для x.
  assert(point.y == null); // Значения поумолчанию null.
}
{% endprettify %}

Если вы инициализируете переменные экземпляра, где они объявлены
(а не в конструкторе или в методе), значение устанавливается при создании экземпляра,
то есть перед выполнением конструктора и его списка инициализаторов.


### Конструкторы

Объявите конструктор спомощью создания функции с таким же именем как у класса
(плюс, необязательно, дополнительный идентификатор как описано в
[Именованных конструкторах](#named-constructors)).
Наиболее распространенная форма конструктора - порождающий конструктор,
который создает новый экземпляр класса:

<?code-excerpt "misc/lib/language_tour/classes/point_alt.dart (constructor-long-way)" plaster="none"?>
{% prettify dart %}
class Point {
  num x, y;

  Point(num x, num y) {
    // Есть более лучший способ сделать это, будте в курсе.
    this.x = x;
    this.y = y;
  }
}
{% endprettify %}

Ключевое слово `this` ссылается на текущий экземпляр.

<div class="alert alert-info" markdown="1">
**Замечание:**
Используйте `this` только когда есть конфликт имён.
Во всех остальных случаях в Dart принято опускать `this`.
</div>

Шаблон присваивания аргументов конструктора переменные экземпляра настолько распространён,
что Dart имеет синтаксический сахар, чтобы сделать это проще:

<?code-excerpt "misc/lib/language_tour/classes/point.dart (constructor-initializer)" plaster="none"?>
{% prettify dart %}
class Point {
  num x, y;

  // Синтаксический сахар для установки x и y
  // перед исполнением тела конструктора.
  Point(this.x, this.y);
}
{% endprettify %}

#### Конструкторы поумолчанию

Если вы не объявите конструктор, вам будет предоставлен конструктор поумолчанию.
Конструктор поумолчанию не имеет аргументов и вызывает конструктор без аргументов в суперклассе.

#### Конструкторы не наследуются

Подклассы не наследуют конструкторы из своих суперклассов.
Подкласс, который не объявляет конструктора имеет только конструктор поумолчанию.

#### Именованные конструкторы

Используйте именованный конструктор для реализации нескольких конструкторов класса
или чтобы обеспечить дополнительную ясность:

<?code-excerpt "misc/lib/language_tour/classes/point.dart (named-constructor)" replace="/Point\.\S*/[!$&!]/g" plaster="none"?>
{% prettify dart %}
class Point {
  num x, y;

  Point(this.x, this.y);

  // Именованный конструктор
  [!Point.origin()!] {
    x = 0;
    y = 0;
  }
}
{% endprettify %}

Запомните, что конструкторы не наследуются, это значит, что
подклассы не наследуют именованные конструкторы суперклассов.
Если вы хотите создать подкласс с именнованным конструктором, определённым
в суперклассе, вы должны реализовать этот конструктор в подклассе.


#### Вызов конструктора (не поумолчанию) суперкласса

Поумолчанию, конструктор в подклассе вызывает у суперкласса безымянный
конструктор, у которого нет аргументов.
Конструктор суперкласса вызывается в начале тела конструктора.
Если также используется [список инициализации](#initializer-list), он выполняется до вызова суперкласса.
В итоге, порядок исполнения таков:

1. список инициализации
2. конструктор без аргументов суперкласса
3. конструктор без аргументов текущего класса

Если у суперкласса нет безымяного конструктора без аргументов,
тогда вы должны вручную вызвать один из конструкторов суперкласса.
Укажите конструктор суперкласса после двоеточия (`:`),
просто перед телом конструктора (если имеется).

В следующем пример, конструктор для класса Employee вызывает именованный конструктор
своего суперкласса Person.
Нажмите на кнопку запуска {% asset red-run.png alt="" %} для исполнения кода.

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
  // У Person нет конструктора поумолчанию
  // вы должны вызвать super.fromJson(data).
  Employee.fromJson(Map data) : super.fromJson(data) {
    print('in Employee');
  }
}

void main() {
  var emp = Employee.fromJson({});
  // Напечатает:
  // in Person
  // in Employee

  if (emp is Person) {
    // Проверка типа
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

Так как аргументы для конструктора суперкласса вычисляются перед вызовом конструктора,
аргументы могут быть выражениями, такими как вызов функции:

<?code-excerpt "misc/lib/language_tour/classes/employee.dart (method-then-constructor)"?>
{% prettify dart %}
class Employee extends Person {
  Employee() : super.fromJson(getDefaultData());
  // ···
}
{% endprettify %}

<div class="alert alert-warning" markdown="1">
**Предупреждение:**
Аргументы для конструктора суперкласса не имеют доступа к `this`.
Например, аргументы могут вызывать статические методы, но не методы экземпляра.
</div>

#### Список инициализации

Кроме вызыва конструктора суперкласса, вы можете также инициализировать
переменные экземпляра перед запуском тела конструктора.
Разделяются инициализаторы запятой.

<?code-excerpt "misc/lib/language_tour/classes/point_alt.dart (initializer-list)"?>
{% prettify dart %}
// Список инициализации задаёт
// переменные экземпляра перед запуском тела конструктора
Point.fromJson(Map<String, num> json)
    : x = json['x'],
      y = json['y'] {
  print('In Point.fromJson(): ($x, $y)');
}
{% endprettify %}

<div class="alert alert-warning" markdown="1">
**Предупреждение:**
Правая сторона инициализатора не имеет доступа к `this`.
</div>

В течении разработки, вы можете проверять входные значения, используя `assert`
в списке инициализации.

<?code-excerpt "misc/lib/language_tour/classes/point_alt.dart (initializer-list-with-assert)" replace="/assert\(.*?\)/[!$&!]/g"?>
{% prettify dart %}
Point.withAssert(this.x, this.y) : [!assert(x >= 0)!] {
  print('In Point.withAssert(): ($x, $y)');
}
{% endprettify %}

Список инициализации удобен, когда устанавливаются финальные поля.
Следующий пимер инициализирует три финальных поля в списке инициализации.
Нажмите на кнопку запуска {% asset red-run.png alt="" %} чтобы исполнить код.

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


#### Перенаправление конструкторов

Иногда конструкторы предназначены только для перенаправления
к другим конструкторам в том же классе. Тело перенаправляющего конструктора
пустое, вызов конструктора появляется после двоеточия (:).

<?code-excerpt "misc/lib/language_tour/classes/point_redirecting.dart"?>
{% prettify dart %}
class Point {
  num x, y;

  // Основной конструктор этого класса.
  Point(this.x, this.y);

  // Делегирует основному конструктору.
  Point.alongXAxis(num x) : this(x, 0);
}
{% endprettify %}

#### Константные конструкторы

Если ваш класс производит объекты, которые никогда не изменяются,
вы можете сделать эти объекты константами времени компиляции. Для этого
определите конструктор с `const` и сделайте все переменные экземпляра `final`.

<?code-excerpt "misc/lib/language_tour/classes/immutable_point.dart"?>
{% prettify dart %}
class ImmutablePoint {
  static final ImmutablePoint origin =
      const ImmutablePoint(0, 0);

  final num x, y;

  const ImmutablePoint(this.x, this.y);
}
{% endprettify %}

Константные конструкторы не всегда создаёт константы.
За подробностями, смотрите разедл по
[использованию конструкторов](#using-constructors).


#### Фабричные конструкторы

Используйте ключевое слово `factory`, когда реализуете конструктор, который
не всегда создаёт новый экземпляр своего класса. Например, фабричный конструктор может возвращать
экземпляр из кэша или может возвращать экземпляр подкласса.

Следующий пример демонстрирует фабричный конструктор, возвращающий объекты из кэша:

<?code-excerpt "misc/lib/language_tour/classes/logger.dart"?>
{% prettify dart %}
class Logger {
  final String name;
  bool mute = false;

  // _cache приватный, благодаря символу _ спереди
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
**Замечание:**
Фабричные конструкторы не имеют доступа к `this`.
</div>

Вызов фабричного конструктора также прост, как и любого друго конструктора:

<?code-excerpt "misc/lib/language_tour/classes/logger.dart (logger)"?>
{% prettify dart %}
var logger = Logger('UI');
logger.log('Button clicked');
{% endprettify %}


### Методы

Методы - это функции, которые предоставляют поведение для объекта.

#### Методы экземпляра

Методы экземпляра в объектах могут обращаться к переменным экземпляра и `this`.
Метод `distanceTo()` в следующем образце - пример метода экземпляра:

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

#### Getter'ы и setter'ы

Getter'ы and setter'ы - специальные методы, которые обеспечивают доступ к чтению
и записи свойст объекта. Напомним, что каждая переменная экземпляра имеет неявный метод
getter, а также, если необходимо, метод setter.
Вы можете создать дополнительные свойства реализовав getter'ы и
setter'ы, используя ключевые слова `get` и `set`:

<?code-excerpt "misc/lib/language_tour/classes/rectangle.dart"?>
{% prettify dart %}
class Rectangle {
  num left, top, width, height;

  Rectangle(this.left, this.top, this.width, this.height);

  // Определение двух вычисляемых свойств: right и bottom.
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

Благодаря getter'ам and setter'ам, вы можете начать с переменных экземпляра,
позже обернуть их спомощью методов, и всё это без изменения клиентского кода.

<div class="alert alert-info" markdown="1">
**Замечание:**
Операторы, такие как инкремент (++) работают ожидаемым образом, вне зависимости от
того, определён ли getter явно.
Чтобы избежать неожиданных побочных эффектов,
оператор вызывает getter ровно один раз,
сохраняя его значение во временной переменной.
</div>

#### Абстрактные методы

Методы getter'ы, и setter'ы могут быть абстрактными,
определяя интрефейс, но оставляя его реализацию другим классам.
Абстрактные методы могут быть только в [абстрактных классах](#abstract-classes).

Чтобы сделать метод абстрактным, используйте точку с запятов (;) вместо тела метода:

<?code-excerpt "misc/lib/language_tour/classes/doer.dart"?>
{% prettify dart %}
abstract class Doer {
  // Определение методов и переменных экземпляра.

  void doSomething(); // Определение абстрактного метода.
}

class EffectiveDoer extends Doer {
  void doSomething() {
    // Реализация, так что этот метод здесь не абстрактный...
  }
}
{% endprettify %}


### Абстрактные классы

Используйте модификатор `abstract`, чтобы определить *абстрактный класс* -
класс, от которого нельзя создать экземпляр. Абстрактные классы полезны для
определения интерфейсов, часто с некоторой реализацией.
Если вы хотите, чтобы ваш абстрактный класс казался инстанцируемым,
определите [фабричный конструктор](#factory-constructors).

Абстрактные классы часто имеют [абстрактные методы](#abstract-methods).
Здесь пример объявления абстрактного класса, у которого
есть абстрактный метод:

<?code-excerpt "misc/lib/language_tour/classes/misc.dart (abstract)"?>
{% prettify dart %}
// This class is declared abstract and thus
// can't be instantiated.
abstract class AbstractContainer {
  // Определение полей, методов...

  void updateChildren(); // Абстрактный метод.
}
{% endprettify %}


### Неявные интерфейсы

Каждый класс неявно определяет интерфейс, содержащий все члены экземпляра
класса и любые интерфейсы, которые он реализует.
Если вы хотите создать класс A, который поддерживает API класса B без
наследования реализации B, класс A должен реализовать интерфейс B.

Класс, реализует один или более интерфейсов, объявляя их в
предложении `implements` и предоставляя API требуемый интерфейсами.
Например:

<?code-excerpt "misc/lib/language_tour/classes/impostor.dart"?>
{% prettify dart %}
// Неявный интерфейс, содержащий greet().
class Person {
  // В интерфейсе, но виден только в этом классе.
  final _name;

  // Не в интерфейсе, поскольку это конструктор.
  Person(this._name);

  // В интерфейсе.
  String greet(String who) => 'Hello, $who. I am $_name.';
}

// Реализация интерфейса Person.
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

Здесь пример указания множественной реализации интерфейсов:

<?code-excerpt "misc/lib/language_tour/classes/misc.dart (point_interfaces)"?>
{% prettify dart %}
class Point implements Comparable, Location {...}
{% endprettify %}


### Наследование классов

Используйте `extends` чтобы создать подкласс и `super` чтобы сослаться к суперклассу:

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


#### Переопределение членов класса

Подклассы могут переопределять методы экземпляра, getter'ы, setter'ы.
Вы можете использовать аннтоацию `@override`, чтобы указать, что вы
намеренно переопределяете член класса:

<?code-excerpt "misc/lib/language_tour/metadata/television.dart (override)" replace="/@override/[!$&!]/g"?>
{% prettify dart %}
class SmartTelevision extends Television {
  [!@override!]
  void turnOn() {...}
  // ···
}
{% endprettify %}

Чтобы сузить тип параметра метода или переменной экземпляра в коде, который является
[типо - безопасным](/guides/language/sound-dart), вы можете использовать
[ключевое слово `covariant`](/guides/language/sound-problems#the-covariant-keyword).


#### Переопределяемые операторы

Вы можете переопределить операторы, показанные в следующей таблице.
Например, если вы определите класс Vector,
вы возможно определите метод `+`, чтобы складывать два вектора.

`<`  | `+`  | `|`  | `[]`
`>`  | `/`  | `^`  | `[]=`
`<=` | `~/` | `&`  | `~`
`>=` | `*`  | `<<` | `==`
`–`  | `%`  | `>>`
{:.table}

<div class="alert alert-info" markdown="1">
  **Замечание:** Вы можете заметить, что `!=` не переопределяемый оператор.
  Выражение `e1 != e2` просто синтаксический сахар для `!(e1 == e2)`.
</div>

Приведём пример класса, который переопределяет операторы `+` и `-`:

<?code-excerpt "misc/lib/language_tour/classes/vector.dart"?>
{% prettify dart %}
class Vector {
  final int x, y;

  Vector(this.x, this.y);

  Vector operator +(Vector v) => Vector(x + v.x, y + v.y);
  Vector operator -(Vector v) => Vector(x - v.x, y - v.y);

  // Operator == и hashCode не показаны. За подробностями смотрите замечание ниже.
  // ···
}

void main() {
  final v = Vector(2, 3);
  final w = Vector(2, 2);

  assert(v + w == Vector(4, 5));
  assert(v - w == Vector(0, 1));
}
{% endprettify %}

Если вы переопределяете `==`, вы должны также переопределить getter класса Object - `hashCode`.
Пример переопределения `==` и `hashCode` смотрите в
[Реализация ключей мапы](/guides/libraries/library-tour#implementing-map-keys).

За подробностями по переопределению в целом, смотрите
[Наследование классов](#extending-a-class).


#### noSuchMethod()

Чтобы обнаружить или реагировать всякий раз,
когда код пытается использовать несуществующий метод или переменную экземпляра,
вы можете переопределить `noSuchMethod()`:

<?code-excerpt "misc/lib/language_tour/classes/no_such_method.dart" replace="/noSuchMethod(?!,)/[!$&!]/g"?>
{% prettify dart %}
class A {
  // Если вы не переопределите noSuchMethod,
  // использование не существующего члена класса, приведёт к ислючению NoSuchMethodError.
  @override
  void [!noSuchMethod!](Invocation invocation) {
    print('You tried to use a non-existent member: ' +
        '${invocation.memberName}');
  }
}
{% endprettify %}

Вы **не можете вызвать** не реализованный метод,
если не выполнено **одно** из следующих условий:

* Получатель имеет статический тип `dynamic`.

* Получатель имеет статический тип, который определяет не реализованный метод
(абстрактный), и динамический тип получателя имеет
реализацию `noSuchMethod()`, который отличается от реализации в классе `Object`.

За большими подробностями, смотрите неофициальную
[ссылку на спецификацию noSuchMethod.](https://github.com/dart-lang/sdk/blob/master/docs/language/informal/nosuchmethod-forwarding.md)


<a id="enums"></a>
### Перечислимые типы
Перечислимые типы часты называют _перечислениями_ или _enums_,
это специальный вид класса, используемый для представления фиксированного числа
константных значений.


#### Использование перечислений

Объявите перечислимый тип, используя ключевое слово `enum`:

<?code-excerpt "misc/lib/language_tour/classes/enum.dart (enum)"?>
{% prettify dart %}
enum Color { red, green, blue }
{% endprettify %}

Каждое значение в перечислении имеет getter `index`, который
возвращает начинающуюся с нуля позицию в объявлении перечисления.
Например, первое значение имеет индекс 0, а второе значение индекс 1.

<?code-excerpt "misc/lib/language_tour/classes/enum.dart (index)"?>
{% prettify dart %}
assert(Color.red.index == 0);
assert(Color.green.index == 1);
assert(Color.blue.index == 2);
{% endprettify %}

Чтобы получить список всех значений в перечислении, используйте
константу `values`.

<?code-excerpt "misc/lib/language_tour/classes/enum.dart (values)"?>
{% prettify dart %}
List<Color> colors = Color.values;
assert(colors[2] == Color.blue);
{% endprettify %}

Вы можете использовать перечислимые типы в [инструкциях switch](#switch-and-case),
и вы получите предупреждение, если вы не обработаете все значения перечислимого типа:

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
  default: // Без этого, вы увидите ПРЕДУПРЕЖДЕНИЕ.
    print(aColor); // 'Color.blue'
}
{% endprettify %}

Перечислимые типы имеют следующие ограничения:

* Вы не можете создавать подклассы, делать миксины или реалзиовывать от перечислений.
* Вы не можете явно создать экземпляр перечислимого типа.

За большей информацией, смотрите
[Спецификацию языка Dart](/guides/language/spec).


### Дополнительные возможности классов: миксины

Миксины - способ переиспользования кода классов во множествах
иерархий классов.

Чтобы _использовать_ миксин, используйте ключевое слово `with`, с следующими
одним или более за ним именами миксинов:

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

Чтобы _реализовать_ миксин, создатйте класс, который наследует от Object и
не объявляет конструктор.
Если вы не хотите, чтобы ваш миксин был обычным классом, используйте
ключевое слово `mixin` вместо `class`.
Например:

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

Чтобы указать, что только некоторые типы могут
использовать миксин - например, чтобы ваш миксин мог вызывать метод,
который он не определяет - используйте `on` для указания требуемого суперкласса:

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
  **Замечание по версии:** Поддержка ключевого слова `mixin` введена в Dart 2.1.
  Код ранних релизов обычно использовал `abstract class` вместо этого.
  За большей информацией об изменениях миксинов в версии 2.1, смотрите
  [Dart SDK лог изменений][] и [2.1 спецификация миксинов.][]
</aside>

[Dart SDK changelog]: https://github.com/dart-lang/sdk/blob/master/CHANGELOG.md
[2.1 mixin specification.]: https://github.com/dart-lang/language/blob/master/accepted/2.1/super-mixins/feature-specification.md#dart-2-mixin-declarations


### Переменные и методы класса

Используйте ключевое слово `static`, чтобы реализовать переменные и методы класса.

#### Статические переменные

Статические переменные (переменные класса) обычно нужны для хранения состояния класса и констант:

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

Статические переменные не инициализируются до тех пор, пока не будут использваны.

<div class="alert alert-info" markdown="1">
**Замечание:**
Эта страница следует
[Руководству с рекомендациями по стилю](/guides/language/effective-dart/style#identifiers),
предпочитающй `lowerCamelCase` для имён констант.
</div>

#### Статические методы

Статические методы (методы класса) не работают с экземплярами, и поэтому не имеют доступа к `this`.
Например:

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
**Замечание:**
Рассмотрите использование глобальных функций вместо статических методов,
для распространнёных или широко используемых утилит и функциональности.
</div>

Вы можете использовать статические методы как константы времени компиляции. Например, вы можете
вставить статический метод как параметр в константный конструктор.


## Обобщения

Если вы посмотрите документацию по API базового типа массива -
If you look at the API documentation for the basic array type,
[Список,][List] вы увидите, что тип на самом деле `List<E>`.
Запись \<...\> отмечает List как
*обобщённый* (или *параметризованный*) тип - тип, который имеет
формальные параметры типов. [По соглашению][], большинство переменных типа имеют
однобуквенные имена, такие как E, T, S, K и V.

[По соглашению]: /guides/language/effective-dart/design#do-follow-existing-mnemonic-conventions-when-naming-type-parameters


### Зачем использовать обобщения?

Обобщения часто требуются для безопасности типов, но они имеют большую выгоду,
чем просто позволить запустить ваш код:

* Правильно указанные обобщённые типы приводят к лучшей генерации кода.
* Вы можете использовать обобщения, чтобы уменьшить дублирование кода.

Если вы намерены хранить в списках только строки, вы можете
объявить их `List<String>` (читается как "список строк").
Таким образом вы, ваши коллеги программисты и ваши иструменты могут обнаружить, что
присваивание не строки списку скорее всего является ошибкой. Пример:

{:.fails-sa}
<?code-excerpt "misc/lib/language_tour/generics/misc.dart (why-generics)"?>
{% prettify dart %}
var names = List<String>();
names.addAll(['Seth', 'Kathy', 'Lars']);
names.add(42); // Ошибка
{% endprettify %}

Другая причина использования обобщений - сокращение дублирования кода.
Обобщения позволяют вам совместно использовать единый интерфейс и
реализацию для многих типов, но при этом использовать статический анализ.
Например, допустим, вы создали интерфейс для кэширования объекта:

<?code-excerpt "misc/lib/language_tour/generics/cache.dart (ObjectCache)"?>
{% prettify dart %}
abstract class ObjectCache {
  Object getByKey(String key);
  void setByKey(String key, Object value);
}
{% endprettify %}

Вы обнаруживаете, что хотите версию этого интерфейса для строк,
поэтому вы создаёте другой интерфейс:

<?code-excerpt "misc/lib/language_tour/generics/cache.dart (StringCache)"?>
{% prettify dart %}
abstract class StringCache {
  String getByKey(String key);
  void setByKey(String key, String value);
}
{% endprettify %}

Позже, вы решите, что хотите версию этого интерфейса для чисел...
У вас появляется идея.

Обобщённые типы могут спасти вас от проблем создания всех этих интерфейсов.
Вместо этого вы можете создать один интерфейс, который получит параметр типа:

<?code-excerpt "misc/lib/language_tour/generics/cache.dart (Cache)"?>
{% prettify dart %}
abstract class Cache<T> {
  T getByKey(String key);
  void setByKey(String key, T value);
}
{% endprettify %}

В этом коде T является типом дублёром.
Это заполнитель, который вы можете рассматривать как тип, который разработчик определит позже.


### Использование литералов коллекций:

Литералы списков и мап могут быть параметризованы.
Параметризованные литералы просто как литералы, которые вы уже видили,
за исключением того, что вы добавляете
<code>&lt;<em>тип</em>></code> (для списков) или
<code>&lt;<em>типКлюча</em>, <em>типЗначения</em>></code> (для мап)
перед отрывавающей скобкой.
Здесь пример использования типизированных литералов:

<?code-excerpt "misc/lib/language_tour/generics/misc.dart (collection-literals)"?>
{% prettify dart %}
var names = <String>['Seth', 'Kathy', 'Lars'];
var pages = <String, String>{
  'index.html': 'Homepage',
  'robots.txt': 'Hints for web robots',
  'humans.txt': 'We are people, not machines'
};
{% endprettify %}


### Использование параметризованных типов с конструкторами

Чтобы указать один или более типов, когда используете конструктор, поместите
типы в угловые скобки (`<...>`) просто после имени класса.
Например:

<?code-excerpt "misc/test/language_tour/generics_test.dart (constructor-1)"?>
{% prettify dart %}
var names = List<String>();
names.addAll(['Seth', 'Kathy', 'Lars']);
var nameSet = Set<String>.from(names);
{% endprettify %}

Следующий код создаёт мапу, которая имеет целочисленные ключи и значения типа View:

<?code-excerpt "misc/test/language_tour/generics_test.dart (constructor-2)"?>
{% prettify dart %}
var views = Map<int, View>();
{% endprettify %}


### Обобщённые коллекции и типы, которые они содержат

Обобщённые типы Dart являются _материализованными_, что означает,
что они переносят информацию о своих типах во время выполнения.
Например, вы можете проверить тип коллекции:

<?code-excerpt "misc/test/language_tour/generics_test.dart (generic-collections)"?>
{% prettify dart %}
var names = List<String>();
names.addAll(['Seth', 'Kathy', 'Lars']);
print(names is List<String>); // true
{% endprettify %}

<div class="alert alert-info" markdown="1">
**Замечание:**
В отличие от обобщённых типов в Java используется *стирание*,
что означает, что параметры обобщённого типа удаляются во время выполнения.
В Java вы можете проверить, является ли объект List,
но вы не можете проверить, является ли он `List<String>`.
</div>


### Ограничение параметризованных типов

Когда вы реализуете обобщённый тип, вы можете захотеть
ограничить типы его параметров.
Вы можете использовать для этого `extends`.

<?code-excerpt "misc/lib/language_tour/generics/base_class.dart" replace="/extends SomeBaseClass(?=. \{)/[!$&!]/g"?>
{% prettify dart %}
class Foo<T [!extends SomeBaseClass!]> {
  // Реализация идёт здесь...
  String toString() => "Экземпляр 'Foo<$T>'";
}

class Extender extends SomeBaseClass {...}
{% endprettify %}

Допустимо использовать `SomeBaseClass` или любой его поддит как аргумент обобщения:

<?code-excerpt "misc/test/language_tour/generics_test.dart (SomeBaseClass-ok)" replace="/Foo.\w+./[!$&!]/g"?>
{% prettify dart %}
var someBaseClassFoo = [!Foo<SomeBaseClass>!]();
var extenderFoo = [!Foo<Extender>!]();
{% endprettify %}

Также допустимо не указывать аргумент обобщения:

<?code-excerpt "misc/test/language_tour/generics_test.dart (no-generic-arg-ok)" replace="/expect\((.*?).toString\(\), .(.*?).\);/print($1); \/\/ $2/g"?>
{% prettify dart %}
var foo = Foo();
print(foo); // Instance of 'Foo<SomeBaseClass>'
{% endprettify %}

Указание любого не `SomeBaseClass` типа приведёт к ошибке:

{:.fails-sa}
<?code-excerpt "misc/lib/language_tour/generics/misc.dart (Foo-Object-error)" replace="/Foo.\w+./[!$&!]/g"?>
{% prettify dart %}
var foo = [!Foo<Object>!]();
{% endprettify %}


### Использование обобщённых методов

Изначально, в Dart поддержка обобщений была ограничена только классами.
Новый синтаксис, называемый _обобщённые методы_, разрешает использовать
аргументы типы в методах и функциях:

<!-- https://dartpad.dartlang.org/a02c53b001977efa4d803109900f21bb -->
<!-- https://gist.github.com/a02c53b001977efa4d803109900f21bb -->
<?code-excerpt "misc/test/language_tour/generics_test.dart (method)" replace="/<T.(?=\()|T/[!$&!]/g"?>
{% prettify dart %}
[!T!] first[!<T>!](List<[!T!]> ts) {
  // Сделать некоторую начальную работу или проверку ошибок, затем ...
  [!T!] tmp = ts[0];
  // Сделать дополнительную проверку или обработку...
  return tmp;
}
{% endprettify %}

Здесь параметр обобщённого типа в `first` (`<T>`)
позволяет вам использовать аргумент тип `T` в нескольких местах:

* В возвращаемом функцией типе (`T`).
* В типах аргументов (`List<T>`).
* В типах локальных переменных (`T tmp`).

За большей информацией о обобщённых типах, смотрите
[Использование обобщённых методов](https://github.com/dart-lang/sdk/blob/master/pkg/dev_compiler/doc/GENERIC_METHODS.md).


## Библиотеки и видимость

Директивы `import` и `library` могут помочь вам создать
модульную и переиспользуемую кодовую базу.
Библиотеки не только предоставляют API, но и единицы приватности:
идентификаторы, которые начинаются с нижнего подчёркивания (\_), видимые
только внутри библиотеки. *Каждое Dart приложение - библиотека*, даже, если
не использует директиву `library`.

Библиотеки могут распространяться использованием пакетов.
Смотрите [пакетный менеджер Pub](/tools/pub)
с информацией о pub - пакетном менеджере, включенном в SDK.


### Использование библиотек

Используйте `import` чтобы указать, как пространство имён одной библиотеки используется
в области видимости другой библиотеки.

Например, web приложения на Dart в основном используют
библиотеку [dart:html][], которая может импортироваться, как здесь:

<?code-excerpt "misc/test/language_tour/browser_test.dart (dart-html-import)"?>
{% prettify dart %}
import 'dart:html';
{% endprettify %}

Единственный требуемый аргумент для `import` - URI, указывающий библиотеку.
Для встроенных библиотек у URI специальная схема `dart:`.
Для других библиотек, вы можете использовать путь файловой системы или схему `package:`.
Схема `package:` указывает библиотеки, предоставляемые пакетным
менеджером pub. Например:


<?code-excerpt "misc/test/language_tour/browser_test.dart (package-import)"?>
{% prettify dart %}
import 'package:test/test.dart';
{% endprettify %}

<div class="alert alert-info" markdown="1">
**Замечание:**
*URI* обозначает универсальный идентификатор ресурса.
*URLs* (единый указатель ресурса) - самый распространнёный вид URI.
</div>


#### Указание префикса библиотеки

Если вы импортируете две библиотеки, которые имеют конфликтующие идентификаторы, тогда вы
должны указать префик для одной или обеих библиотек.
Например, если библиотека1 и библиотека2 имеют класс Element,
вы можете написать код как здесь:

<?code-excerpt "misc/lib/language_tour/libraries/import_as.dart" replace="/(lib\d)\.dart/package:$1\/$&/g"?>
{% prettify dart %}
import 'package:lib1/lib1.dart';
import 'package:lib2/lib2.dart' as lib2;

// Использование Element из lib1.
Element element1 = Element();

// Использование Element из lib2.
lib2.Element element2 = lib2.Element();
{% endprettify %}

#### Импортирование части библиотеки

Если вы хотите использовать только часть библиотеки, вы можете
выборочно импортировать библиотеку. Например:

<?code-excerpt "misc/lib/language_tour/libraries/show_hide.dart" replace="/(lib\d)\.dart/package:$1\/$&/g"?>
{% prettify dart %}
// Импортировать только foo.
import 'package:lib1/lib1.dart' show foo;

// Импортировать все имена, исключая foo.
import 'package:lib2/lib2.dart' hide foo;
{% endprettify %}

<a id="deferred-loading"></a>
#### Ленивая загрузка библиотеки

_Отложенная загрузка_ (также называемая _ленивой загрузкой_)
позволяет приложению загружать библиотеку по требованию, если и когда это необходимо.
Здесь некоторые случаи, когда вы можете использовать отложенную загрузку:

* Чтобы сократить время начальной загрузки приложения.
* Выполнить A/B тестирование, например,
  попробовать альтернативные реализации алгоритма.
* Для загрузки редко используемого функционала,
  такого как необязательные экраны и диалоги.

Для ленивой загрузки библиотеки, вы должны сперва
импортировать её, используя `deferred as`.

<?code-excerpt "misc/lib/language_tour/libraries/greeter.dart (import)" replace="/hello\.dart/package:greetings\/$&/g"?>
{% prettify dart %}
import 'package:greetings/hello.dart' deferred as hello;
{% endprettify %}

Когда вам необходима библиотека, вызовите
`loadLibrary()` исользуя идентификатор библиотеки.

<?code-excerpt "misc/lib/language_tour/libraries/greeter.dart (loadLibrary)"?>
{% prettify dart %}
Future greet() async {
  await hello.loadLibrary();
  hello.printGreeting();
}
{% endprettify %}

В предыдущем коде, ключевое слово `await` останавливает исполнение до тех пор
пока библиотека не загрузится.
За большей информацией о `async` и `await`, смотрите
[поддрежка асинхронности](#asynchrony-support).

Вы можете вызвать `loadLibrary()` множество раз без каких-то проблем.
Библиотека загружается только единожды.

Учтите следующее, когда вы используете отложенную загрузку:

* Константы отложенной библиотеки не константы в импортирующем файле.
  Запомните, эти константы не существуют до тех пор, пока отложенная библиотека
  не загрузится.
* Вы не можете использовать типы из отложенной библиотеки в импортирующем файле.
  Вместо этого, рассмотрите перемещение интерфейсных типов в библиотеку, импортируемую
  отложенной библиотекой и импортирующим файлом.
* Dart неявно вставляет `loadLibrary()` в пространство имён, которое вы определяете,
  используя <code>deferred as <em>пространство_имён</em></code>.
  Функция `loadLibrary()` возвращает [Future](/guides/libraries/library-tour#future).

<aside class="alert alert-warning" markdown="1">
**Различия Dart VM:**
Dart VM разрешает доступ к членам отложенных библиотек
даже перед вызовом `loadLibrary()`.
Это поведение может измениться, такчто
**не полагайтесь на это поведение**.
За подробностями смотрите [issue #33118.](https://github.com/dart-lang/sdk/issues/33118)
</aside>

### Реализация библиотек

Смотрите
[Создание библиотечного пакета](/guides/libraries/create-library-packages)
чтобы проконсультироваться как реализовать пакет библиотеки, включая:

* Как организовать исходный код библиотеки.
* Как использовать директиву `export`.
* Когда использовать директиву `part`.
* Когда использовать директиву `library`.


<a id="asynchrony"></a>
## Поддержка асинхронности

В библиотеках Dart полно функций, возвращающих объекты
[Future][] или [Stream][].
Эти функции _асинхронные_:
они возвращают результат после длительной по времени операции (такой как ввод/вывод),
без ожидания завершения операции.

Ключевые слова `async` и `await` поддерживают асинхронное программирование,
позволяют вам писать асинхронный код, который выглядит как синхронный.


<a id="await"></a>
### Обработка Future

Когда вам необходим результат завершённоё Future,
у вас есть два варианта:

* Использовать `async` и `await`.
* Использовать Future API, описанный
  [в туре по библиотекам](/guides/libraries/library-tour#future).

Код, который использует `async` и `await` - асинхронный,
но он выглядит похоже на синхронный код.
Например, здесь некоторый код, который использует `await`,
чтобы дождаться результата асинхронной функции:

<?code-excerpt "misc/lib/language_tour/async.dart (await-lookUpVersion)"?>
{% prettify dart %}
await lookUpVersion();
{% endprettify %}

Чтобы использовать `await`, код должен быть в _асинхронной функции_ -
функции, помеченной `async`:

<?code-excerpt "misc/lib/language_tour/async.dart (checkVersion)" replace="/async|await/[!$&!]/g"?>
{% prettify dart %}
Future checkVersion() [!async!] {
  var version = [!await!] lookUpVersion();
  // Сделать что-нибудь с версией
}
{% endprettify %}

<aside class="alert alert-info" markdown="1">
**Замечание:**
Хотя в асинхронной функция может выполняться длительная по времени операция,
она не ждёт этих операций.
Вместо этого, асинхронная функция исполняется только до тех пора, пока она
не встретит первое выражение с `await` ([детали][synchronous-async-start]).
Потом она возвращает объект Future, возобнавляя исполнение только
после завершения выражения с `await`.
</aside>

Используйте `try`, `catch`, и `finally` чтобы
обработать ошибки и навести порядок в коде, использующий `await`:

<?code-excerpt "misc/lib/language_tour/async.dart (try-catch)"?>
{% prettify dart %}
try {
  version = await lookUpVersion();
} catch (e) {
  // Реакция на невозможность просмотреть версию
}
{% endprettify %}

Вы можете использовать `await` множество раз в асинхронной функции.
Например, следующий код ждёт три раза результат от функций:

<?code-excerpt "misc/lib/language_tour/async.dart (repeated-await)"?>
{% prettify dart %}
var entrypoint = await findEntrypoint();
var exitCode = await runExecutable(entrypoint, args);
await flushThenExit(exitCode);
{% endprettify %}

В <code>await <em>выражения</em></code>,
значение <code><em>выражения</em></code> обычно - Future;
Если нет, тогда значение автоматически оборачивается в Future.
Этот объект Future сообщает об обещании вернуть объект.
Значение <code>await <em>выражения</em></code> - возвращаемый объект.
Выражение с await приостанавливает исполнение до тех пор, пока объект не станет доступным.

**Если вы получили ошибку времени компиляции, когда используете `await`,
убедитесь, что используете `await` в асинхронной функции.**
Например, чтобы использовать `await` в функции `main()` вашего приложения,
тело `main()` должно быть помечено `async`:

<?code-excerpt "misc/lib/language_tour/async.dart (main)" replace="/async|await/[!$&!]/g"?>
{% prettify dart %}
Future main() [!async!] {
  checkVersion();
  print('In main: version is ${[!await!] lookUpVersion()}');
}
{% endprettify %}


<a id="async"></a>
### Объявление асинхронных функций

_Асинхронная функция_ - функция, чьё тело помечено модификатором `async`.

Добавляя ключевое слово `async` к функции, сделайте её возвращаемое значение Future.
Например, рассмотрите эту синхронную функцию, которая возвращает String:

<?code-excerpt "misc/lib/language_tour/async.dart (sync-lookUpVersion)"?>
{% prettify dart %}
String lookUpVersion() => '1.0.0';
{% endprettify %}

Если вы измените её на асинхронную функцию - например,
потому что будущая реализация может занять много времени -
возвращаемое значение будет Future:

<?code-excerpt "misc/lib/language_tour/async.dart (async-lookUpVersion)"?>
{% prettify dart %}
Future<String> lookUpVersion() async => '1.0.0';
{% endprettify %}

Заметьте, что тело функции не нуждается в использовании Future API.
Dart создаст объект Future, если необходимо.

Если ваша функция не возвращает используемое значение, сделайте
её возвращаемый тип `Future<void>`.


<a id="await-for"></a>
### Обработка Stream

Когда вам необходимо получить значения из Stream,
у вас есть два варианта:

* Использовать `async` и _асинхронный цикл for_ (`await for`).
* Использовать Stream API, описанный
  [в туре по библиотекам](/guides/libraries/library-tour#stream).

<aside class="alert alert-warning" markdown="1">
**Замечание:**
Перед использованием `await for`, убедитесь, что это делает код более понятным,
и что вы действительно хотите дождаться всех результатов Stream.
Например, вы обычно **не** должны использовать `await for`
для обработчиков событий UI, поскольку
UI фреймворки отправляют бесконечные потоки событий.
</aside>

Асинхронный цикл for имеет следующую форму:

<?code-excerpt "misc/lib/language_tour/async.dart (await-for)"?>
{% prettify dart %}
await for (varOrType identifier in expression) {
  // Исполняется каждый раз, когда Stream даёт значение.
}
{% endprettify %}

Значение <code><em>expression</em></code> должно иметь тип Stream.
Процесс исполнения следующий:

1. Дождаться, пока Stream даст значение.
2. Исполнение тела цикла for, с переменной, которая получает значение.
3. Повторение 1 и 2 до тех пор, пока Stream не будет закрыт.

Чтобы остановить прослушивание Stream,
вы можете использовать инструкцию
`break` или `return`,
которые выпригивают из цикла for и
отписываются от Stream.

**Если вы получаете ошибку времени компиляции когда
реализуете асинхронный цикл for, убедитесь, что
`await for` находится в асинхронной функции.**
Например, чтобы использовать асинхронный цикл for в вашей функции `main()`,
тело `main()` должно быть помечено `async`:

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

За большей информацией об асинхронном программирование в целом, смотрите
раздел [dart:async](/guides/libraries/library-tour#dartasync---asynchronous-programming)
тура по библиотекам.

Также смотрите статьи
[Язык Dart поддержка асинхронности: Фаза 1](/articles/language/await-async)
и
[Язык Dart поддержка асинхронности: Фаза 2](/articles/language/beyond-async),
и [спецификацию языка Dart](/guides/language/spec).


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
