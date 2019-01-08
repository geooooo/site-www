---
title: "Исправление распространёных проблем с типами"
description: Распространёные проблемы, с которыми вы можете столкнуться и как их исправить.
---
{% comment %}Don't show exact file names in analyzer error output.{% endcomment %}
<?code-excerpt replace="/ at (lib|test)\/\w+\.dart:\d+:\d+//g"?>
<?code-excerpt plaster="none"?>

Если у вас есть проблемы с проверками типа, эта страница может помочь.
Чтобы узнать больше, читайте
[Система типов Dart](/guides/language/sound-dart)
и [другие ресурсы](/guides/language/sound-dart#other-resources).

<aside class="alert alert-info" markdown="1">
**Помогите нам улучшить эту страницу!**
Если вы встретите предупреждение или ошибку, которая не перечислена здесь,
пожалуйста, сообщите о проблеме, нажав **иконку с жучком** сверху справа.
Приложите **сообщение об предупреждении или ошибке** и,
если возможно, небольшой код, чтобы воспроизвести ситуацию и его правильный эквивалент.
</aside>


## Поиск проблем

<a name="am-i-using-strong-mode"></a>
### Я и в правду использую типо-безопасный Dart?

Если вы не увидели ожидаемую ошибку или предупреждение,
убедитесь, что вы используете последнюю версию Dart.

Для альтернативы, попробуйте добавить следующий код в файл:

{:.fails-sa}
<?code-excerpt "strong/lib/common_problems_analysis.dart (is-strong-mode-enabled)"?>
{% prettify dart %}
bool b = [0][0];
{% endprettify %}

С типо-безопасным Dart, анализатор выдаст следующую ошибку:

{:.console-output}
<?code-excerpt "strong/analyzer-2-results.txt" retain="/'int' can't be .* 'bool'.*common_problems/"?>
```nocode
error • A value of type 'int' can't be assigned to a variable of type 'bool' • invalid_assignment
```


<a name="common-errors"></a>
## Статические ошибки и предупреждения

Этот раздел показывает, как исправить некоторые из ошибок и предупрждений, которые
вы можете увидеть из анализатора или в IDE.

Статический анализатор не может отловить все ошибки.
Для помощи по исправлению ошибок, которые получаются только во
время исполнения, смотрите
[Ошибки времени исполнения](#common-errors-and-warnings).

### Неопределённые члены

<?code-excerpt "strong/analyzer-2-results.txt" retain="/isn't defined for the class.*common_problems/" replace="/getter/<member\x3E/g; /'\w+'/'...'/g"?>
```nocode
error • The <member> '...' isn't defined for the class '...' • undefined_<member>
```

Эти ошибки могут появляться при следующих условиях:

- Статически известно, что переменная является неким супертипом,
  но код предполагает использование подтипа.
- A variable is statically known to be some supertype, but the code assumes a subtype.
- Обобщённый класс имеет ограниченный тип параметра,
  но в выражении создания экземпляра класса отсутствует тип аргумента.

#### Пример 1: Статически известно, что переменная является неким супертипом, но код предполагает использование подтипа.

В следующем коде, анализатор жалуется, что `context2D` не определён:

{:.fails-sa}
<?code-excerpt "strong/lib/common_problems_analysis.dart (canvas-error)" replace="/context2D/[!$&!]/g"?>
{% prettify dart %}
var canvas = querySelector('canvas');
canvas.[!context2D!].lineTo(x, y);
{% endprettify %}

{:.console-output}
<?code-excerpt "strong/analyzer-2-results.txt" retain="/context2D.*isn't defined for the class/"?>
```nocode
error • The getter 'context2D' isn't defined for the class 'Element' • undefined_getter
```

#### Исправление: Заменить определение члена на явное объявление типа или понижение

Метод `querySelector()` статически возвращает Element,
но код предполагает его возвращаемый подтип CanvasElement,
где `context2D` определён.
Поле `canvas` объявлено как как `var`,
которое в версиях Dart 1.x без строгого режима
вводит его как `dynamic` и умолчаивает обо всех ошибках.
Dart выведет `canvas` как Element.

Вы можете исправить эту ошибку путём явного объявления типа:

{:.passes-sa}
<?code-excerpt "strong/lib/common_fixes_analysis.dart (canvas-ok)" replace="/CanvasElement/[!$&!]/g"?>
{% prettify dart %}
[!CanvasElement!] canvas = querySelector('canvas');
canvas.context2D.lineTo(x, y);
{% endprettify %}

Код выше пройдёт статический анализ при разрешённом [неявном приведении][].

Вы также можете использовать явное понижение:

{:.passes-sa}
<?code-excerpt "strong/lib/common_fixes_analysis.dart (canvas-as)" replace="/as \w+/[!$&!]/g"?>
{% prettify dart %}
var canvas = querySelector('canvas') [!as CanvasElement!];
canvas.context2D.lineTo(x, y);
{% endprettify %}

Во всех остальных случаях, используйте `dynamic` в ситуациях,
когда вы не можете использовать один тип:

{:.passes-sa}
<?code-excerpt "strong/lib/common_fixes_analysis.dart (canvas-dynamic)" replace="/dynamic/[!$&!]/g"?>
{% prettify dart %}
[!dynamic!] canvasOrImg = querySelector('canvas, img');
var width = canvasOrImg.width;
{% endprettify %}

#### Пример 2: Параметры пропущенных типов по умолчанию соответствуют границам их типов

Рассмотрим следующий **обобщённый класс** с **ограниченным типом параметра**,
который наследует `Iterable`:

<?code-excerpt "strong/lib/bounded/my_collection.dart"?>
{% prettify dart %}
class C<T extends Iterable> {
  final T collection;
  C(this.collection);
}
{% endprettify %}

Следующий код создаёт новый экземпляр этого класса (опуская тип аргумента)
и обращаясь к его члену `collection`:

{:.fails-sa}
<?code-excerpt "strong/lib/bounded/instantiate_to_bound.dart (undefined_method)" replace="/c\..*;/[!$&!]/g"?>
{% prettify dart %}
var c = C(Iterable.empty()).collection;
[!c.add(2);!]
{% endprettify %}

{:.console-output}
<?code-excerpt "strong/analyzer-2-results.txt" retain="/add.*isn't defined for the class/"?>
```nocode
error • The method 'add' isn't defined for the class 'Iterable' at lib/bounded/instantiate_to_bound.dart:7:5 • undefined_method
```

В то время как тип [List][] имеет метод `add()`, [Iterable][] - нет.

#### Исправление: указание типа аргументов или исправление ошибок downstream

В Dart 1.x, при создании обобщёного класса без явных типов аргументов предполагается `dynamic`.
Поэтому в отрывке кода выше, `c` имеет тип `dynamic` и не будет сообщения об ошибке для `c.add()`.

В Dart 2, при создании обобщённого класса без явных типов аргументов,
каждый тип параметра по умолчанию привязан к своему типу (в этом пример `Iterable`)
если он явно задан или во всех остальных случаях `dynamic`.

Вам необходимо подходить к исправлению таких ошибок индивидуально в каждом случае.
Это помогает хорошо понять оригинальный замысел конструкции.

Явная вставка типа аргументов - эффективный путь помочь идентифицировать тип ошибок.
Например, если вы измените код, чтобы указать `List` как тип аргумента, анализатор
сможет обнаружить несоответствие типа в аргументе конструктора.
Исправьте ошибку, указав аргумент конструктора соответствующего типа:

{:.passes-sa}
<?code-excerpt "strong/test/strong_test.dart (add-type-arg)" replace="/.List.|\[\]/[!$&!]/g"?>
{% prettify dart %}
var c = C[!<List>!]([![]!]).collection;
c.add(2);
{% endprettify %}


<hr>

### Неправильное переопределение метода

<?code-excerpt "strong/analyzer-2-results.txt" retain="/isn't a valid override of.*num.*common_problems/" replace="/'[\w\.]+'/'...'/g; /\('.*?'\)//g"?>
```nocode
error • '...'  isn't a valid override of '...'  • invalid_override
```

Эти ошибки обычно происходят, когда подкласс сужает типы параметров метода,
путём указания подкласса исходного класса.

<aside class="alert alert-info" markdown="1">
**Замечание:**
Эта проблема может также происходить, когда обобщённый подкласс
пренебрегает указанию типа. За большей информацией смотрите
[Потеря типа аргуметнов](#missing-type-arguments).
</aside>

#### Пример

В следующем примере параметры метода `add()` имеют тип `int` - подтип `num`, который
является типом параметра, используемый в родительском классе.

{:.fails-sa}
<?code-excerpt "strong/lib/common_problems_analysis.dart (invalid-method-override)" replace="/int(?= \w\b)/[!$&!]/g"?>
{% prettify dart %}
abstract class NumberAdder {
  num add(num a, num b);
}

class MyAdder extends NumberAdder {
  int add([!int!] a, [!int!] b) => a + b;
}
{% endprettify %}

{:.console-output}
<?code-excerpt "strong/analyzer-2-results.txt" retain="/isn't a valid override of.*num.*common_problems/"?>
```nocode
error • 'MyAdder.add' ('(int, int) → int') isn't a valid override of 'NumberAdder.add' ('(num, num) → num') • invalid_override
```
Рассмотрим следующий сценарий, где значение с плавающей точкой помещается в MyAdder:

{:.runtime-fail}
<?code-excerpt "strong/lib/common_problems_analysis.dart (unsafe-method-call)" replace="/\d[\d\.]+/[!$&!]/g"?>
{% prettify dart %}
NumberAdder adder = MyAdder();
adder.add([!1.2!], [!3.4!]);
{% endprettify %}

Если переопределение было бы разрешено, код выдал ошибку во время исполнения.

#### Исправление: Расширение типов параметров метода

Метод подкласса должен принимать каждый объект, который получает метод суперкласса.

Исправте пример, с помощью расширения типов в подклассе:

{:.passes-sa}
<?code-excerpt "strong/lib/common_fixes_analysis.dart (valid-method-override)" replace="/num(?= \w\b.*=)/[!$&!]/g"?>
{% prettify dart %}
abstract class NumberAdder {
  num add(num a, num b);
}

class MyAdder extends NumberAdder {
  num add([!num!] a, [!num!] b) => a + b;
}
{% endprettify %}

Большую информацию смотрите
[Используйте правильные типы входных параметров при переопределении методов](/guides/language/sound-dart#use-proper-param-types).

<aside class="alert alert-info" markdown="1">
  **Замечание:**
  Если у вас есть хороший повод, чтобы использовать подтип,
  вы можете использовать [ключевое слово covariant](#the-covariant-keyword).
</aside>

<hr>

### Потеря типа аргументов

<?code-excerpt "strong/analyzer-2-results.txt" retain="/isn't a valid override of.*dynamic.*common_problems/" replace="/'\S+'/'...'/g; /\('.*?'\)//g"?>
```nocode
error • '...'  isn't a valid override of '...'  • invalid_override
```

#### Пример

В следующем примере `Subclass` наследует `Superclass<T>` но не указывает
тип аргумента. Анализатор выведет `Subclass<dynamic>`, который приведёт
к ошибке неправильности переопределения у `method(int)`.

{:.fails-sa}
<?code-excerpt "strong/lib/common_problems_analysis.dart (missing-type-arguments)" replace="/int(?= \w\b)/[!$&!]/g"?>
{% prettify dart %}
class Superclass<T> {
  void method(T t) { ... }
}

class Subclass extends Superclass {
  void method([!int!] i) { ... }
}
{% endprettify %}

{:.console-output}
<?code-excerpt "strong/analyzer-2-results.txt" retain="/isn't a valid override of.*dynamic.*common_problems/"?>
```nocode
error • 'Subclass.method' ('(int) → void') isn't a valid override of 'Superclass.method' ('(dynamic) → void') • invalid_override
```

#### Исправление: Указание типа аргументов для обобщёного подкласса

При пропуске указания типа аргумента в обобщёном подклассе,
анализатор выведет тип `dynamic`. Это скорее всего приведёт к ошибкам.

Вы можете исправить пример, указав тип в подклассе:

{:.passes-sa}
<?code-excerpt "strong/lib/common_fixes_analysis.dart (type-arguments)" replace="/<int\x3E/[!$&!]/g"?>
{% prettify dart %}
class Superclass<T> {
  void method(T t) { ... }
}

class Subclass extends Superclass[!<int>!] {
  void method(int i) { ... }
}
{% endprettify %}

<hr>

<a id ="assigning-mismatched-types"></a>
### Unexpected collection element type

<?code-excerpt "strong/analyzer-2-results.txt" retain="/'double' can't be assigned to a variable of type 'int'.*common_problems/" replace="/'\S+'/'...'/g"?>
```nocode
error • A value of type '...' can't be assigned to a variable of type '...' • invalid_assignment
```

This sometimes happens when you create a simple dynamic collection
and the analyzer infers the type in a way you didn't expect.
When you later add values of a different type, the analyzer reports an issue.

#### Example

The following code initializes a map with several
(String, int) pairs. The analyzer infers that map to be of type
`<String, int>` but the code seems to assume either `<String, dynamic>` or `<String, num>`.
When the code adds a (String, float) pair, the analyzer complains:

{:.fails-sa}
<?code-excerpt "strong/lib/common_problems_analysis.dart (inferred-collection-types)" replace="/1.5/[!$&!]/g"?>
{% prettify dart %}
// Inferred as Map<String, int>
var map = {'a': 1, 'b': 2, 'c': 3};
map['d'] = [!1.5!]; // a double is not an int
{% endprettify %}

{:.console-output}
<?code-excerpt "strong/analyzer-2-results.txt" retain="/'double' can't be assigned to a variable of type 'int'.*common_problems/"?>
```nocode
error • A value of type 'double' can't be assigned to a variable of type 'int' • invalid_assignment
```

#### Fix: Specify the type explicitly

The example can be fixed by explicitly defining the map's type to be
`<String, num>`.

{:.passes-sa}
<?code-excerpt "strong/lib/common_fixes_analysis.dart (inferred-collection-types)" replace="/<.*?\x3E/[!$&!]/g"?>
{% prettify dart %}
var map = [!<String, num>!]{'a': 1, 'b': 2, 'c': 3};
map['d'] = 1.5;
{% endprettify %}

Alternatively, if you want this map to accept any value, specify the type as `<String, dynamic>`.

<hr>

<a id="constructor-initialization-list"></a>
### Constructor initialization list super() call

<?code-excerpt "strong/analyzer-2-results.txt" retain="/super call must be last.*common_problems/" replace="/'\S+'/'...'/g"?>
```nocode
error • super call must be last in an initializer list (see https://goo.gl/EY6hDP): '...' • strong_mode_invalid_super_invocation
```

This error occurs when the `super()` call is not last in a constructor's
initialization list.

#### Example

{:.fails-sa}
<?code-excerpt "strong/lib/common_problems_analysis.dart (super-goes-last)" replace="/super/[!$&!]/g"?>
{% prettify dart %}
HoneyBadger(Eats food, String name)
    : [!super!](food),
      _name = name { ... }
{% endprettify %}

{:.console-output}
<?code-excerpt "strong/analyzer-2-results.txt" retain="/super call must be last.*common_problems/"?>
```nocode
error • super call must be last in an initializer list (see https://goo.gl/EY6hDP): 'super(food)' • strong_mode_invalid_super_invocation
```

#### Fix: Put the `super()` call last

The compiler can generate simpler code if it relies on the `super()` call appearing last.

Fix this error by moving the `super()` call:

{:.passes-sa}
<?code-excerpt "strong/lib/common_fixes_analysis.dart (super-goes-last)" replace="/super/[!$&!]/g"?>
{% prettify dart %}
HoneyBadger(Eats food, String name)
    : _name = name,
      [!super!](food) { ... }
{% endprettify %}

<hr>

<a name="uses-dynamic-as-bottom"></a>
### A function of type ... can't be assigned

<?code-excerpt "strong/analyzer-2-results.txt" retain="/The function expression type.*common_problems/" replace="/'\S+ → \S+'/'...'/g"?>
```nocode
error • The function expression type '...' isn't of type '...'. This means its parameter or return type does not match what is expected. Consider changing parameter type(s) or the returned type(s) • strong_mode_invalid_cast_function_expr
```

In Dart 1.x `dynamic` was both a [top type][] (supertype of all types) and a
[bottom type][]  (subtype of all types)
depending on the context. This meant it was valid to assign, for example,
a function with a parameter of type `String` to a place that expected a
function type with a parameter of `dynamic`.

However, in Dart 2 passing something other than `dynamic` (or another _top_
type, such as `Object`, or a specific bottom type, such as `Null`) results
in a compile-time error.

#### Example

{:.fails-sa}
<?code-excerpt "strong/lib/common_problems_analysis.dart (func-dynamic)" replace="/String/[!$&!]/g"?>
{% prettify dart %}
typedef Filter = bool Function(dynamic any);
Filter filter = ([!String!] x) => x.contains('Hello');
{% endprettify %}

{:.console-output}
<?code-excerpt "strong/analyzer-2-results.txt" retain="/type '\(String\) → bool'.*common_problems/"?>
```nocode
error • A value of type '(String) → bool' can't be assigned to a variable of type '(dynamic) → bool' • invalid_assignment
error • The function expression type '(String) → bool' isn't of type '(dynamic) → bool'. This means its parameter or return type does not match what is expected. Consider changing parameter type(s) or the returned type(s) • strong_mode_invalid_cast_function_expr
```

#### Fix: Add type parameters _or_ cast from dynamic explicitly

When possible, avoid this error by adding type parameters:

{:.passes-sa}
<?code-excerpt "strong/lib/common_fixes_analysis.dart (func-T)" replace="/<\w+\x3E/[!$&!]/g"?>
{% prettify dart %}
typedef Filter[!<T>!] = bool Function(T any);
Filter[!<String>!] filter = (String x) => x.contains('Hello');
{% endprettify %}

Otherwise use casting:

{:.passes-sa}
<?code-excerpt "strong/lib/common_fixes_analysis.dart (func-cast)" replace="/([Ff]ilter)1/$1/g; /as \w+/[!$&!]/g"?>
{% prettify dart %}
typedef Filter = bool Function(dynamic any);
Filter filter = (x) => (x [!as String!]).contains('Hello');
{% endprettify %}

<hr>

<a id="common-errors-and-warnings"></a>
## Runtime errors

Unlike Dart 1.x, Dart 2 enforces a sound type system. Roughly, this means
you can't get into a situation where the value stored in a variable is
different than the variable's static type. Like most modern statically
typed languages, Dart accomplishes this with a combination of static
(compile-time) and dynamic (runtime) checking.

For example, the following type error is detected at compile-time
(when the [implicit casts][] option is disabled):

{:.fails-sa}
<?code-excerpt "strong/lib/common_problems_analysis.dart (int-not-string)" replace="/string[^;]*/[!$&!]/g"?>
{% prettify dart %}
List<int> numbers = [1, 2, 3];
List<String> [!string = numbers!];
{% endprettify %}

Since neither `List<int>` nor `List<String>` is a subtype of the other,
Dart rules this out statically. You can see other examples of these
static analysis errors in [Unexpected collection element type](#unexpected-collection-element-type).

The errors discussed in the remainder of this section are reported at
[runtime](sound-dart#runtime-checks).

### Invalid casts

To ensure type safety, Dart needs to insert _runtime_ checks in some cases. Consider:

{:.passes-sa}
<?code-excerpt "strong/test/strong_test.dart (downcast-check)" replace="/string = objects/[!$&!]/g"?>
{% prettify dart %}
assumeStrings(List<Object> objects) {
  List<String> strings = objects; // Runtime downcast check
  String string = strings[0]; // Expect a String value
}
{% endprettify %}

The assignment to `strings` is _downcasting_ the `List<Object>` to `List<String>`
implicitly (as if you wrote `as List<String>`), so if the value you pass in
`objects` at runtime is a `List<String>`, then the cast succeeds.

Otherwise, the cast will fail at runtime:

{:.runtime-fail}
<?code-excerpt "strong/test/strong_test.dart (fail-downcast-check)" replace="/\[.*\]/[!$&!]/g"?>
{% prettify dart %}
assumeStrings(<int>[![1, 2, 3]!]);
{% endprettify %}

{:.console-output}
<?code-excerpt "strong/test/strong_test.dart (downcast-check-msg)" replace="/final msg = ./Exception: /g; /.;//g"?>
```nocode
Exception: type 'List<int>' is not a subtype of type 'List<String>'
```

#### Fix: Tighten or correct types

Sometimes, lack of a type, especially with empty collections, means that a `<dynamic>`
collection is created, instead of the typed one you intended. Adding an explicit
type argument can help:

{:.runtime-success}
<?code-excerpt "strong/test/strong_test.dart (typed-list-lit)" replace="/<String\x3E/[!$&!]/g"?>
{% prettify dart %}
var list = [!<String>!][];
list.add('a string');
list.add('another');
assumeStrings(list);
{% endprettify %}

You can also more precisely type the local variable, and let inference help:

{:.runtime-success}
<?code-excerpt "strong/test/strong_test.dart (typed-list)" replace="/<String\x3E/[!$&!]/g"?>
{% prettify dart %}
List[!<String>!] list = [];
list.add('a string');
list.add('another');
assumeStrings(list);
{% endprettify %}

In cases where you are working with a collection that you don't create, such
as from JSON or an external data source, you can use the
[cast()]({{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/List/cast.html) method
provided by `List` and other collection classes.

Here's an example of the preferred solution: tightening the object's type.

{:.runtime-success}
<?code-excerpt "strong/test/strong_test.dart (cast)" replace="/cast/[!$&!]/g"?>
{% prettify dart %}
Map<String, dynamic> json = getFromExternalSource();
var names = json['names'] as List;
assumeStrings(names.[!cast!]<String>());
{% endprettify %}

<aside class="alert alert-info" markdown="1">
  **Version note:**
  The `cast()` method was introduced in 2.0.0-dev.22.0.
</aside>

If you can't tighten the type or use `cast`, you can copy the object
in a different way. For example:

{:.runtime-success}
<?code-excerpt "strong/test/strong_test.dart (create-new-object)" replace="/\.map.*\.toList../[!$&!]/g"?>
{% prettify dart %}
Map<String, dynamic> json = getFromExternalSource();
var names = json['names'] as List;
// Use `as` and `toList` until 2.0.0-dev.22.0, when `cast` is available:
assumeStrings(names[!.map((name) => name as String).toList()!]);
{% endprettify %}

{% comment %}
## Known issues
Do we have any known issues or bugs to list here?
{% endcomment %}

## Appendix

### The covariant keyword

Some (rarely used) coding patterns rely on tightening a type
by overriding a parameter's type with a subtype, which is invalid.
In this case, you can use the `covariant` keyword to
tell the analyzer that you are doing this intentionally.
This removes the static error and instead checks for an invalid
argument type at runtime.

<aside class="alert alert-info" markdown="1">
  **Version note:**
  The `covariant` keyword was introduced in 1.22.
  It replaces the `@checked` annotation.
</aside>

The following shows how you might use `covariant`:

{:.passes-sa}
<?code-excerpt "strong/lib/covariant.dart" replace="/covariant/[!$&!]/g"?>
{% prettify dart %}
class Animal {
  void chase(Animal x) { ... }
}

class Mouse extends Animal { ... }

class Cat extends Animal {
  void chase([!covariant!] Mouse x) { ... }
}
{% endprettify %}

Although this example shows using `covariant` in the subtype,
the `covariant` keyword can be placed in either the superclass
or the subclass method.
Usually the superclass method is the best place to put it.
The `covariant` keyword applies to a single parameter and is
also supported on setters and fields.

[bottom type]: https://en.wikipedia.org/wiki/Bottom_type
[dartanalyzer README]: https://github.com/dart-lang/sdk/tree/master/pkg/analyzer_cli#dartanalyzer
[Iterable]: {{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Iterable-class.html
[implicit casts]: /guides/language/analysis-options#enabling-additional-type-checks
[List]: {{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/List-class.html
[top type]: https://en.wikipedia.org/wiki/Top_type
