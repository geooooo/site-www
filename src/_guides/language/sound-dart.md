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

{% comment %}
NOTE: непонятно о чём и к чему это
{% endcomment %}

This code is not type safe because it would then be possible to define a cat and send it after an alligator:

<?code-excerpt "strong/lib/animal_bad.dart (chase-Alligator)" replace="/Alligator/[!$&!]/g"?>
{% prettify dart %}
Animal a = Cat();
a.chase([!Alligator!]()); // Not type safe or feline safe
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
если он встречает цикл (то есть вывод типа для переменной зависит от знания типа этой переменной).


### Вывод локальных переменных

Local variable types are inferred from their initializer, if any.
Subsequent assignments are not taken into account.
This may mean that too precise a type may be inferred.
If so, you can add a type annotation.

{:.fails-sa}
<?code-excerpt "strong/lib/strong_analysis.dart (local-var-type-inference-error)"?>
{% prettify dart %}
var x = 3; // x is inferred as an int
x = 4.0;
{% endprettify %}

{:.passes-sa}
<?code-excerpt "strong/lib/strong_analysis.dart (local-var-type-inference-ok)"?>
{% prettify dart %}
num y = 3; // a num can be double or int
y = 4.0;
{% endprettify %}

### Type argument inference

Type arguments to constructor calls and
[generic method](/guides/language/language-tour#using-generic-methods) invocations are
inferred based on a combination of downward information from the context
of occurrence, and upward information from the arguments to the constructor
or generic method. If inference is not doing what you want or expect,
you can always explicitly specify the type arguments.

{:.passes-sa}
<?code-excerpt "strong/lib/strong_analysis.dart (type-arg-inference)"?>
{% prettify dart %}
// Inferred as if you wrote <int>[].
List<int> listOfInt = [];

// Inferred as if you wrote <double>[3.0].
var listOfDouble = [3.0];

// Inferred as Iterable<int>
var ints = listOfDouble.map((x) => x.toInt());
{% endprettify %}

In the last example, `x` is inferred as `double` using downward information.
The return type of the closure is inferred as `int` using upward information.
Dart uses this return type as upward information when inferring the `map()`
method's type argument: `<int>`.


## Substituting types

When you override a method, you are replacing something of one type (in the
old method) with something that might have a new type (in the new method).
Similarly, when you pass an argument to a function,
you are replacing something that has one type (a parameter
with a declared type) with something that has another type
(the actual argument). When can you replace something that
has one type with something that has a subtype or a supertype?

When substituting types, it helps to think in terms of _consumers_
and _producers_. A consumer absorbs a type and a producer generates a type.

**You can replace a consumer's type with a supertype and a producer's
type with a subtype.**

Let's look at examples of simple type assignment and assignment with
generic types.

### Simple type assignment

When assigning objects to objects, when can you replace a type with a
different type? The answer depends on whether the object is a consumer
or a producer.

Consider the following type hierarchy:

<img src="images/type-hierarchy.png" alt="a hierarchy of animals where the supertype is Animal and the subtypes are Alligator, Cat, and HoneyBadger. Cat has the subtypes of Lion and MaineCoon">

Consider the following simple assignment where `Cat c` is a _consumer_ and `Cat()`
is a _producer_:

<?code-excerpt "strong/lib/strong_analysis.dart (Cat-Cat-ok)"?>
{% prettify dart %}
Cat c = Cat();
{% endprettify %}

In a consuming position, it's safe to replace something that consumes a
specific type (`Cat`) with something that consumes anything (`Animal`),
so replacing `Cat c` with `Animal c` is allowed, because Animal is
a supertype of Cat.

{:.passes-sa}
<?code-excerpt "strong/lib/strong_analysis.dart (Animal-Cat-ok)"?>
{% prettify dart %}
Animal c = Cat();
{% endprettify %}

But replacing `Cat c` with `MaineCoon c` breaks type safety, because the
superclass may provide a type of Cat with different behaviors, such
as Lion:

{:.fails-sa}
<?code-excerpt "strong/lib/strong_analysis.dart (MaineCoon-Cat-err)"?>
{% prettify dart %}
MaineCoon c = Cat();
{% endprettify %}

In a producing position, it's safe to replace something that produces a
type (Cat) with a more specific type (MaineCoon). So, the following
is allowed:

{:.passes-sa}
<?code-excerpt "strong/lib/strong_analysis.dart (Cat-MaineCoon-ok)"?>
{% prettify dart %}
Cat c = MaineCoon();
{% endprettify %}

### Generic type assignment

Are the rules the same for generic types? Yes. Consider the hierarchy
of lists of animals&mdash;a List of Cat is a subtype of a List of
Animal, and a supertype of a List of MaineCoon:

<img src="images/type-hierarchy-generics.png" alt="List<Animal> -> List<Cat> -> List<MaineCoon>">

In the following example, you can assign a `MaineCoon` list to `myCats` because
`List<MaineCoon>` is a subtype of `List<Cat>`:

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

What about going in the other direction? Can you assign an `Animal` list to a `List<Cat>`?

{:.passes-sa}
<?code-excerpt "strong/lib/strong_analysis.dart (generic-type-assignment-Animal)" replace="/Animal/[!$&!]/g"?>
{% prettify dart %}
List<Cat> myCats = List<[!Animal!]>();
{% endprettify %}

This assignment passes static analysis,
but it creates an implicit cast. It is equivalent to:

<?code-excerpt "strong/lib/strong_analysis.dart (generic-type-assignment-implied-cast)" replace="/as.*(?=;)/[!$&!]/g"?>
{% prettify dart %}
List<Cat> myCats = List<Animal>() [!as List<Cat>!];
{% endprettify %}

The code may fail at runtime. You can disallow implicit casts
by specifying `implicit-casts: false` in the [analysis options file.][analysis_options.yaml]


### Methods

When overriding a method, the producer and consumer rules still apply.
For example:

<img src="images/consumer-producer-methods.png" alt="Animal class showing the chase method as the consumer and the parent getter as the producer">

For a consumer (such as the `chase(Animal)` method), you can replace
the parameter type with a supertype. For a producer (such as
the `parent` getter method), you can replace the return type with
a subtype.

For more information, see
[Use sound return types when overriding methods](#use-proper-return-types)
and [Use sound parameter types when overriding methods](#use-proper-param-types).


## Other resources

The following resources have further information on sound Dart:

* [Fixing Common Type Problems](/guides/language/sound-problems) -
  Errors you may encounter when
  writing sound Dart code, and how to fix them.
* [Dart 2](/dart-2) - How to update Dart 1.x code to Dart 2.
* [Customize Static Analysis](/guides/language/analysis-options) - How
  to set up and customize the analyzer and linter using an analysis
  options file.


[analysis_options.yaml]: /guides/language/analysis-options
[analyzer]: /tools/analyzer
[Dart VM]: /dart-vm/tools/dart-vm
[dartdevc]: {{site.webdev}}/tools/dartdevc
[strong mode]: https://www.dartlang.org/guides/language/sound-dart#how-to-enable-strong-mode
