---
title: Система типов Dart
description: Зачем и как писать звучный код на Dart.
---
<?code-excerpt replace="/([A-Z]\w*)\d\b/$1/g; /\b(main)\d\b/$1/g"?>

Dart - типо безопасен: он использует комбинацию статической проверки типов и
[проверки во время исполнения](#runtime-checks), обеспечивая, чтобы значения
переменных всегда соответствовали статическим типам переменных.
Хотя _типы_ обязательные, _аннотация_ типов опциональна, так как Dart выполняет
[вывод типов](#type-inference).

Эта страница концентрирует внимание на безопасности типов - возможности, добавленной в Dart 2.
Для полного введения в язык Dart, включая типы, смотрите
[тур по языку](/guides/language/language-tour).

<aside class="alert alert-info" markdown="1">
  **Замечание по термонологии:**
  Термин **надёжность (sound)** в Dart и **безопасность типов** в Dart
  часто используется взаимозаменяемо.
  Вы также могли видеть термин **строгий режим**.
  Строгий режим был опциональной возможностью Dart 1.x,
  которая предоставляла частичную поддержку безопасности типов.
</aside>

Одна из выгод статической проверки типов - способность находить баги во
время компиляции, используя [статический анализатор][analyzer] Dart.

Вы можете исправить большинство ошибок статического анализа с помощью добавления
аннотаций типов для обобщённых классов. Большинство распространнёных обобщённых классов
- типы коллекций `List<T>` и `Map<K,V>`.

Например, в следующем коде функция `printInts()` печатает список целых чисел,
а `main()` создаёт список и вставляет его в `printInts()`.

{:.fails-sa}
<?code-excerpt "strong/lib/strong_analysis.dart (opening-example)" replace="/list(?=\))/[!$&!]/g"?>
{% prettify dart %}
void printInts(List<int> a) => print(a);

void main() {
  var list = [];
  list.add(1);
  list.add("2");
  printInts([!list!]);
}
{% endprettify %}

Предыдущий код приводит к ошибке типов в `list` (подсвечено выше)
при вызове `printInts(list)`:

{:.console-output}
<?code-excerpt "strong/analyzer-2-results.txt" retain="/List.*strong_analysis.*argument_type_not_assignable/" replace="/ at (lib|test)\/\w+\.dart:\d+:\d+//g"?>
```nocode
error • The argument type 'List' can't be assigned to the parameter type 'List<int>' • argument_type_not_assignable
```

Ошибка выделяет неправильное неявное приведение типов из `List<dynamic>` в `List<int>`.
Переменная `list` имеет статический тип `List<dynamic>`.
Так как инициализируещее объявление `var list = []`
не предоставляет анализатору достаточно информации для вывода типа более конкретного, чем `dynamic`.
Функция `printInts()` ождиает параметр с типом `List<int>`,
вызывая несоответствие типов.

Если добавить аннотацию типа (`<int>`) при создании списка (выделено ниже)
анализатор пожалуется, что строковый аргумент не может быть присвоен параметру `int`.
Удалив кавычки в `list.add("2")`, получим код, проходящий статический анализ и запускающийся без ошибок и предупреждений.

{:.passes-sa}
<?code-excerpt "strong/test/strong_test.dart (opening-example)" replace="/<int.(?=\[)|2/[!$&!]/g"?>
{% prettify dart %}
void printInts(List<int> a) => print(a);

void main() {
  var list = [!<int>!][];
  list.add(1);
  list.add([!2!]);
  printInts(list);
}
{% endprettify %}

{% comment %}
  Note: DartPad does not yet support no-implicit-casts, but it does
  report a runtime type error.
{% endcomment -%}

[Try it in DartPad](https://dartpad.dartlang.org/f64e963cb5f894e2146c2b28d5efa4ed).

## Что такое надёжность (soundness)?

*Надёжность (Soundness)* означает, что ваша программа не может войти в определенные недопустимые состояния.
*Надёжная (sound) система типов* означает, что вы никогда не попадете в состояние,
в котором выражение вычисляется как значение, которое не соответствует статическому типу выражения.
Например, если статический тип выражения - `String`,
во время выполнения вы гарантированно получите только строку при ее вычислении.

Система типов Dart, как система типов в Java и C# - надёжная.
Это приводит к тому, что надёжность использует комбинацию статической проверки
(ошибки времени компиляции) и проверки во время исполнения. Например, присваивание
Приведение `Object` в строку, используя `as String`, потерпит неудачу во время исполнения, если
объект не является строкой.


## Выгоды от надёжности

Надёжная система типов имеет несколько преимуществ:

* Показывает ошибки времени компиляции, связанные с типами.<br>
  Надёжная система типов заставляет код быть однозначным относительно его типов,<br>
  поэтому связанные с типом ошибки, которые сложно найти во время выполнения,

* Более читабельный код.<br>
  Код легче читать, потому что вы можете полагаться на значение, имеющее указанный тип.<br>
  В надёжном Dart, типы не могут лгать.

* Более поддерживаемый код.<br>
  С надёжной системой типов, когда вы изменяете одну часть кода,<br>
  система типов может уведомить вас о других частях кода, которые просто сломались.

* Лучшая ahead of time (AOT) компиляция.<br>
  Хотя AOT компиляция возможна без типов, сгенерированный код гораздо менее эффективен.


## Подсказки для прохождения статического анализа

Большинство правил для статических типов просты для понимания.
Здесь некоторый из менее очевидных правил:

* Используйте надёжные типы возвращаемых значений
* Используйте надёжные типы параметров, когда переопределяете методы.
* Не используйте список dynamic как типизированный список.

Посмотрим эти правила в деталалях с примерами,
которые используют следующую иерархию типов:

<img src="images/type-hierarchy.png" alt="иерархия животных, где супертип - Animal и подтипы: Alligator, Cat, и HoneyBadger. Cat имеет подтипы: Lion и MaineCoon">

<a name="use-proper-return-types"></a>
### Использование надёжных типов возвращаемых значений при переопределении методов

Возвращаемый тип метода в подклассе должен быть такого же типа или
подтипом возвращаемого типа метода в суперклассе. Рассмотрим метод getter в классе Animal:

<?code-excerpt "strong/lib/animal.dart (Animal)" replace="/Animal get.*/[!$&!]/g"?>
{% prettify dart %}
class Animal {
  void chase(Animal a) { ... }
  [!Animal get parent => ...!]
}
{% endprettify %}

Метод getter `parent` возвращает Animal. В подклассе HoneyBadger вы можете заменить
возвращаемый тип getter'а на HoneyBadger (или любой другой подтип Animal),
но не связанный тип не допустим.

{:.passes-sa}
<?code-excerpt "strong/lib/animal.dart (HoneyBadger)" replace="/(\w+)(?= get)/[!$&!]/g"?>
{% prettify dart %}
class HoneyBadger extends Animal {
  void chase(Animal a) { ... }
  [!HoneyBadger!] get parent => ...
}
{% endprettify %}

{:.fails-sa}
<?code-excerpt "strong/lib/animal_bad.dart (HoneyBadger)" replace="/(\w+)(?= get)/[!$&!]/g"?>
{% prettify dart %}
class HoneyBadger extends Animal {
  void chase(Animal a) { ... }
  [!Root!] get parent => ...
}
{% endprettify %}

<a name="use-proper-param-types"></a>
### Использование надёжных типов параметров при переопределении методов:

Параметр переопределяемого метода должен иметь либо такой же тип, либо
супертипа соответствующего параметра в суперклассе.
Не "сужайте" тип параметра путём замены типа подтипом оригинального параметра.

<aside class="alert alert-info" markdown="1">
  **Замечание:**
  Если у вас есть реальная необходимость использовать подтип,
  вы можете использовать [ключевое слово `covariant`](/guides/language/sound-problems#the-covariant-keyword).
</aside>

Рассмотрим метод `chase(Animal)` класса Animal:

<?code-excerpt "strong/lib/animal.dart (Animal)" replace="/void chase.*/[!$&!]/g"?>
{% prettify dart %}
class Animal {
  [!void chase(Animal a) { ... }!]
  Animal get parent => ...
}
{% endprettify %}

Метод `chase()` принимает параметр Animal. Класс HoneyBadger иной (Object).
Допустимо переопределить метод `chase()`, чтобы тот принимал Object.

{:.passes-sa}
<?code-excerpt "strong/lib/animal.dart (chase-Object)" replace="/Object/[!$&!]/g"?>
{% prettify dart %}
class HoneyBadger extends Animal {
  void chase([!Object!] a) { ... }
  Animal get parent => ...
}
{% endprettify %}

Следующий код сужает тип параметра метода `chase()`
с Animal до Mouse - подкласса Animal.

{:.fails-sa}
<?code-excerpt "strong/lib/animal_bad.dart (chase-Mouse)" replace="/(\w+)(?= x)/[!$&!]/g"?>
{% prettify dart %}
class Mouse extends Animal {...}

class Cat extends Animal {
  void chase([!Mouse!] x) { ... }
}
{% endprettify %}

Этот код не типо безопасен, потомучто тогда будет возможно определить кошку и после отправить ей аллигатора:

<?code-excerpt "strong/lib/animal_bad.dart (chase-Alligator)" replace="/Alligator/[!$&!]/g"?>
{% prettify dart %}
Animal a = Cat();
a.chase([!Alligator!]()); // Не типо безопасно или не безопасно для кошки
{% endprettify %}

### Не используйте список dynamic как типизированный список

Список dynamic хорош, когда вы хотите иметь список с различным содержимым.
Тем не менее вы не можете использовать список с dynamic как типизированный список.

Это правило также применимо к экземплярам обобщённых типов.

Следующий код создаёт список с dynamic из данных с типом Dog и присваивает его списку с Cat,
код генерирует ошибку во время статического анализа.

{:.fails-sa}
<?code-excerpt "strong/lib/animal_bad.dart (dynamic-list)" replace="/.dynamic.(?!.*OK)/[!$&!]/g"?>
{% prettify dart %}
class Cat extends Animal { ... }

class Dog extends Animal { ... }

void main() {
  List<Cat> foo = [!<dynamic>!][Dog()]; // Ошибка
  List<dynamic> bar = <dynamic>[Dog(), Cat()]; // OK
}
{% endprettify %}

## Проверки во время исполнения

Проверки во время исполнения в таких утилитах, как [Dart VM][] и [dartdevc][]
имеют дело с проблемами безопасности типов, которые анализатор не может отловить.

Например, следующий код бросает исключение во время исполнения, так как
ошибочно присваивать список собак списку кошек:

{:.runtime-fail}
<?code-excerpt "strong/test/strong_test.dart (runtime-checks)" replace="/cats[^;]*/[!$&!]/g"?>
{% prettify dart %}
void main() {
  List<Animal> animals = [Dog()];
  List<Cat> [!cats = animals!];
}
{% endprettify %}


## Вывод типов

Анализатор может выводить типы для полей, методов, локальных переменных и
большинства аргументов типов обобщений.
Когда анализатор не имеет достаточно информации для вывода конкретного типа,
он использует тип `dynamic`.

Здесь пример того, как работает вывод типов с обобщениями.
В этом пример переменная, называемая `arguments`
содержит мапу строк в качестве ключей и значений различных типов.

Если вы явно вводите переменную, вы можете написать это:

<?code-excerpt "strong/lib/strong_analysis.dart (type-inference-1-orig)" replace="/Map<String, dynamic\x3E/[!$&!]/g"?>
{% prettify dart %}
[!Map<String, dynamic>!] arguments = {'argA': 'hello', 'argB': 42};
{% endprettify %}

Вместо этого, вы можете использовать `var` и предоставить Dart вывод типа:

<?code-excerpt "strong/lib/strong_analysis.dart (type-inference-1)" replace="/var/[!$&!]/g"?>
{% prettify dart %}
[!var!] arguments = {'argA': 'hello', 'argB': 42}; // Map<String, Object>
{% endprettify %}

Литерал мапы выводит свой тип из типа своих элементов,
а затем переменная выводит свой тип из типа литерала мапы.
В этой мапе, ключи - строки, но значения имеют различные типы
(Строки и целые числа, которые имеют верхнюю границу Object).
Так литерал мапы имеет тип `Map<String, Object>` и тот же самый
переменная `arguments`.


### Вывод поля и метода

Поле или метод, который не имеет указанного типа и которое
переопределяет поле или метод суперкласса, наследует тип поля или метода суперкласса.

Поле, которое не имеет объявленного или унаследованного типа, но которое объявлено с начальным значением,
получает выводимый тип на основе начального значения.


### Вывод статических полей

Статические поля и переменные получают вывод их типов из их инициализаторов.
Обратите внимание, что вывод завершается неудачей,
если он встречает цикл (то есть вывод типа переменной зависит от знания типа этой переменной).


### Вывод локальных переменных

Тип локальной переменной выводится из её инициализатора, если он имеется.
Последующие присваивания не беруться во внимание.
Это означает, что может быть выведен слишком точный тип.
Если это так, вы можете добавить аннотацию типа.

{:.fails-sa}
<?code-excerpt "strong/lib/strong_analysis.dart (local-var-type-inference-error)"?>
{% prettify dart %}
var x = 3; // x выводится как int
x = 4.0;
{% endprettify %}

{:.passes-sa}
<?code-excerpt "strong/lib/strong_analysis.dart (local-var-type-inference-ok)"?>
{% prettify dart %}
num y = 3; // num может быть double или int
y = 4.0;
{% endprettify %}

### Вывод аргументов типов

Аргументы типы в вызовах конструктора и в
вызовах [обобщённых методов](/guides/language/language-tour#using-generic-methods)
выводятся на основе комбинации
нисходящей информации из контекста вхождения и
восходящей информации из аргументов в конструкторе или обобщённом методе.
Если вывод не такой, какой вы хотели или ожидали, вы всегда можете явно
указать аргументы типы.

{:.passes-sa}
<?code-excerpt "strong/lib/strong_analysis.dart (type-arg-inference)"?>
{% prettify dart %}
// Выведет, как если бы вы написали <int>[].
List<int> listOfInt = [];

// Выведет, как если бы вы написали <double>[3.0].
var listOfDouble = [3.0];

// Выведет как Iterable<int>
var ints = listOfDouble.map((x) => x.toInt());
{% endprettify %}

В последнем примере `x` выведется как `double`, используя нисходящую информацию.
Возвращаемый тип замыкания выведется как `int`, используя восходящую информацию.
Dart использует этот возвращаемый тип `<int>` как восходящую информацию
при выводе аргумента типа метода `map()`.


## Подстановка типов

Когда вы переопределяете метод, вы заменяете что-то одного типа (в старом методе)
на что-то, что может иметь новый тип (в новом методе).
Точно так же, когда вы передаете аргумент функции,
вы заменяете что-то, имеющее один тип (параметр с объявленным типом),
чем-то, имеющим другой тип (фактический аргумент).
Когда вы можете заменить что-то,
имеющее один тип, чем-то его подтипа или супертипа?

При подстановке типов, удобно рассуждать в терминах _потребителей_
и _поставщиков_. Потребитель поглащает тип, а поставщик генерирует тип.

**Вы можете заменить тип потребителя на супертип, а
тип поставщика на подтип.**

Взгляните на пример простого присваивания типа и присваивания с обобщёнными типами.


### Простое присваивание типа

При присваивание объектов объектам, когда вы можете заменить тип на
другой тип? Ответ зависит от того, является ли объект потребителем или поставщиком.

Рассмотрите следующую иерархию типов:

<img src="images/type-hierarchy.png" alt="иерархия животных, где супертип - Animal, а подтипы: Alligator, Cat, и HoneyBadger. Cat имеет подтипы: Lion и MaineCoon">

Рассмотрим следующее простое присваивание, где `Cat c` - _потребитель_, а `Cat()`
- _поставщик_:

<?code-excerpt "strong/lib/strong_analysis.dart (Cat-Cat-ok)"?>
{% prettify dart %}
Cat c = Cat();
{% endprettify %}

На месте потребителя, безопасно заменить тип (`Cat`)
на другой тип (`Animal`),
заменить `Cat c` на `Animal c` допустимо, так как Animal - супертип Cat.

{:.passes-sa}
<?code-excerpt "strong/lib/strong_analysis.dart (Animal-Cat-ok)"?>
{% prettify dart %}
Animal c = Cat();
{% endprettify %}

Но замена `Cat c` на `MaineCoon c` сломает безопасность типов, так как
суперкласс может предоставить тип от Cat с отличающимся поведением, таким как Lion:

{:.fails-sa}
<?code-excerpt "strong/lib/strong_analysis.dart (MaineCoon-Cat-err)"?>
{% prettify dart %}
MaineCoon c = Cat();
{% endprettify %}

На месте поставщика, безопасно заменить тип (Cat) на более конкретный тип (MaineCoon).
Так что, следующее допустимо:

{:.passes-sa}
<?code-excerpt "strong/lib/strong_analysis.dart (Cat-MaineCoon-ok)"?>
{% prettify dart %}
Cat c = MaineCoon();
{% endprettify %}


### Присваивание обобщённого типа

Являются ли правила такими же для обобщённых типов? Да. Рассмотрим иерархию
списка животных&mdash; список Cat - подтип списка Animal и супертип списка MaineCoon:

<img src="images/type-hierarchy-generics.png" alt="List<Animal> -> List<Cat> -> List<MaineCoon>">

В следующем пример, вы можете присвоить список `MaineCoon` переменной `myCats`, потомучто
`List<MaineCoon>` - подтип `List<Cat>`:

{:.passes-sa}
<?code-excerpt "strong/lib/strong_analysis.dart (generic-type-assignment-MaineCoon)" replace="/MaineCoon/[!$&!]/g"?>
{% prettify dart %}
List<Cat> myCats = List<[!MaineCoon!]>();
{% endprettify %}

{% comment %}
Gist:  https://gist.github.com/4a2a9bc2242042ba5338533d091213c0
DartPad: https://dartpad.dartlang.org/4a2a9bc2242042ba5338533d091213c0

[Try it in DartPad](https://dartpad.dartlang.org/4a2a9bc2242042ba5338533d091213c0).
{% endcomment %}

Как насчет идти в другом направлении? Можете ли вы список `Animal` присвоить `List<Cat>`?

{:.passes-sa}
<?code-excerpt "strong/lib/strong_analysis.dart (generic-type-assignment-Animal)" replace="/Animal/[!$&!]/g"?>
{% prettify dart %}
List<Cat> myCats = List<[!Animal!]>();
{% endprettify %}

Это присваивание пройдёт статический анализ, но оно
создаст неявное приведение типов. Это эквивалентно:
This assignment passes static analysis,
but it creates an implicit cast. It is equivalent to:

<?code-excerpt "strong/lib/strong_analysis.dart (generic-type-assignment-implied-cast)" replace="/as.*(?=;)/[!$&!]/g"?>
{% prettify dart %}
List<Cat> myCats = List<Animal>() [!as List<Cat>!];
{% endprettify %}

Код может упасть во время исполнения. Вы можете запретить неявное приведение типов,
указав `implicit-casts: false` в [файле опций анализа.][analysis_options.yaml]


### Методы

При переопределении метода правила производителя и потребителя все еще в силе.
Например:

<img src="images/consumer-producer-methods.png" alt="Animal class showing the chase method as the consumer and the parent getter as the producer">

Для потребителя (такой метод как `chase(Animal)`), вы можете
заменить тип параметра на супертип. Для производителя (такой метод getter как `parent`),
вы можете заменить возвращаемый тип на подтип.

За большей информацией смотрите
[Использование надёжных возвращаемых типов при переопределении методов](#use-proper-return-types)
и [Использование надёжных типов параметров при переопределении методов](#use-proper-param-types).


## Другие ресурсы

Следующие ресурсы имеют сопутствующую информацию о надёжности Dart:

* [Исправление распространнёных проблем с типами](/guides/language/sound-problems) -
  Ошибки с которыми вы можете столкнуться при написании надёжного кода на Dart, и как их исправить.
* [Dart 2](/dart-2) - Как обновить код на Dart 1.x до Dart 2.
* [Настройка статического анализа](/guides/language/analysis-options) -
  Как установить и настроить анализатор и линтер, используя файл опций анализа.


[analysis_options.yaml]: /guides/language/analysis-options
[analyzer]: /tools/analyzer
[Dart VM]: /dart-vm/tools/dart-vm
[dartdevc]: {{site.webdev}}/tools/dartdevc
[strong mode]: https://www.dartlang.org/guides/language/sound-dart#how-to-enable-strong-mode
