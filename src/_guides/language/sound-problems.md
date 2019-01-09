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
### Неожиданный тип элемента коллекции

<?code-excerpt "strong/analyzer-2-results.txt" retain="/'double' can't be assigned to a variable of type 'int'.*common_problems/" replace="/'\S+'/'...'/g"?>
```nocode
error • A value of type '...' can't be assigned to a variable of type '...' • invalid_assignment
```

Это иногда происходит при создании простой динамической коллекции и
анализатор выводит тип, которые вы не ожидали.
Когда вы позже добавите значение другого типа, анализатор сообщит об проблеме.


#### Пример

Следующий код инициализирует мапу с несколькими парами (String, int).
Анализатор выведет, что мапа должна быть типа `<String, int>`,
но кажется, что код предполагает или `<String, dynamic>` или `<String, num>`.
Когда код добавляет пару (String, float), анализатор пожалуется:

{:.fails-sa}
<?code-excerpt "strong/lib/common_problems_analysis.dart (inferred-collection-types)" replace="/1.5/[!$&!]/g"?>
{% prettify dart %}
// Inferred as Map<String, int>
var map = {'a': 1, 'b': 2, 'c': 3};
map['d'] = [!1.5!]; // double не является int
{% endprettify %}

{:.console-output}
<?code-excerpt "strong/analyzer-2-results.txt" retain="/'double' can't be assigned to a variable of type 'int'.*common_problems/"?>
```nocode
error • A value of type 'double' can't be assigned to a variable of type 'int' • invalid_assignment
```

#### Исправление: Явное указание типа

Пример может быть исправлен, путём явного определения типа мапы,
чтобы он был `<String, num>`.

{:.passes-sa}
<?code-excerpt "strong/lib/common_fixes_analysis.dart (inferred-collection-types)" replace="/<.*?\x3E/[!$&!]/g"?>
{% prettify dart %}
var map = [!<String, num>!]{'a': 1, 'b': 2, 'c': 3};
map['d'] = 1.5;
{% endprettify %}

В другом случае, если вы хотите, чтобы эта мапа принимала любые значения,
укажите тип `<String, dynamic>`.

<hr>

<a id="constructor-initialization-list"></a>
### Вызов super() в списке инициализации конструктора

<?code-excerpt "strong/analyzer-2-results.txt" retain="/super call must be last.*common_problems/" replace="/'\S+'/'...'/g"?>
```nocode
error • super call must be last in an initializer list (see https://goo.gl/EY6hDP): '...' • strong_mode_invalid_super_invocation
```

Эта ошибка происходит, когда вызов `super()` - не стоит в конце списка
инициализации конструктора.

#### Пример

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

#### Исправление: поместите вызов `super()` в конец

Компилятор может генерировать более простой код, если вызов `super()` происходит в конце.

Исправте эту ошибку, переместив вызов `super()`:

{:.passes-sa}
<?code-excerpt "strong/lib/common_fixes_analysis.dart (super-goes-last)" replace="/super/[!$&!]/g"?>
{% prettify dart %}
HoneyBadger(Eats food, String name)
    : _name = name,
      [!super!](food) { ... }
{% endprettify %}

<hr>

<a name="uses-dynamic-as-bottom"></a>
### Функция типа ... не может быть присвоена

<?code-excerpt "strong/analyzer-2-results.txt" retain="/The function expression type.*common_problems/" replace="/'\S+ → \S+'/'...'/g"?>
```nocode
error • The function expression type '...' isn't of type '...'. This means its parameter or return type does not match what is expected. Consider changing parameter type(s) or the returned type(s) • strong_mode_invalid_cast_function_expr
```

В Dart 1.x `dynamic` был [верхним типом][] (супертип всех типов)
и [нижним типом][] (подтип всех типов) в зависимости от контекста.
Это означало, что было бы правильно присвоить, например, функцию с параметром типа `String` в место,
где ожидался тип функции с параметром `dynamic`.

Тем не менее, в Dart 2 вставка чего-то отличного от `dynamic`
(или другого _верхнего_ типа, такого как `Object`, или указание _нижнего_ типа, такого как `Null`)
приведёт к ошибке времени компиляции.

#### Пример

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

#### Исправление: добавте параметрам тип _или_ приведите тип из dynamic явно

Когда возможно, избегайте этой ошибки, добавляя параметрам тип:

{:.passes-sa}
<?code-excerpt "strong/lib/common_fixes_analysis.dart (func-T)" replace="/<\w+\x3E/[!$&!]/g"?>
{% prettify dart %}
typedef Filter[!<T>!] = bool Function(T any);
Filter[!<String>!] filter = (String x) => x.contains('Hello');
{% endprettify %}

Во всех остальных случаях используйте приведение типов:

{:.passes-sa}
<?code-excerpt "strong/lib/common_fixes_analysis.dart (func-cast)" replace="/([Ff]ilter)1/$1/g; /as \w+/[!$&!]/g"?>
{% prettify dart %}
typedef Filter = bool Function(dynamic any);
Filter filter = (x) => (x [!as String!]).contains('Hello');
{% endprettify %}

<hr>

<a id="common-errors-and-warnings"></a>
## Ошибки времени исполнения

В отличии от Dart 1.x, Dart 2 обеспечивает надёжную систему типов.
Грубо говоря, это значит, что вы не можете получить ситуацию, когда значение,
хранящееся в переменной отличается от статического типа этой переменной.
Как и большинство современных статически типизированных языков,
Dart осуществляет это засчёт комбинации статической (времени компиляции) и
динамической (во время исполнения) проверкок.

Например, следующая ошибка типов обнаруживается во время компиляции
(когда отключена опция [неявного приведения типов][]):

{:.fails-sa}
<?code-excerpt "strong/lib/common_problems_analysis.dart (int-not-string)" replace="/string[^;]*/[!$&!]/g"?>
{% prettify dart %}
List<int> numbers = [1, 2, 3];
List<String> [!string = numbers!];
{% endprettify %}

Поскольку ни `List<int>`, ни `List<String>` не являются подтипами друг друга,
Dart исключает это статически. В можете посмотреть другие примеры этих
ошибок статического анализа в [Неожиданный тип элемента коллекции](#unexpected-collection-element-type).

Ошибки, обсуждаемые в оставшейся части этого раздела сообщаются во
[время исполнения](sound-dart#runtime-checks).

### Неправильное приведение типов

Для того, чтобы обеспечить безопасность типов, Dart необходимо вставлять проверки
_во время исполнения_ в некоторых случаях. Рассмотрим:

{:.passes-sa}
<?code-excerpt "strong/test/strong_test.dart (downcast-check)" replace="/string = objects/[!$&!]/g"?>
{% prettify dart %}
assumeStrings(List<Object> objects) {
  List<String> strings = objects; // Runtime downcast check
  String string = strings[0]; // Expect a String value
}
{% endprettify %}

В присваивание к переменной `strings` - _downcasting_ `List<Object>` к `List<String>`
неявно (как если бы вы написали `as List<String>`), так если значение, вставленое вами в
`objects` во время исполнения `List<String>`, тогда приведение типов завершиться успехом.

Во всех остальных случаях приведение типов может быть неудачным во время исполнения:

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

#### Исправление: Сузить или поправить типы

Иногда отсутствие типа, особенно с пустыми коллекциями,
означает, что вместо типизированной коллекции, которую вы намеревались создать,
создается коллекция `dynamic`.
Добавление явного типа аргумента может помочь:

{:.runtime-success}
<?code-excerpt "strong/test/strong_test.dart (typed-list-lit)" replace="/<String\x3E/[!$&!]/g"?>
{% prettify dart %}
var list = [!<String>!][];
list.add('a string');
list.add('another');
assumeStrings(list);
{% endprettify %}

Вы также можете укзать более точный тип локальной переменной, а вывод типов поможет:

{:.runtime-success}
<?code-excerpt "strong/test/strong_test.dart (typed-list)" replace="/<String\x3E/[!$&!]/g"?>
{% prettify dart %}
List[!<String>!] list = [];
list.add('a string');
list.add('another');
assumeStrings(list);
{% endprettify %}

В случаях, когда вы работаете с коллекцией, которую вы не создавали, такой как
JSON или внешний источник данных, вы можете использовать метод
[cast()]({{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/List/cast.html),
предоставленный `List` и другими классами коллекций.

Вот пример предпочтительного решения: сужение типа объектов:

{:.runtime-success}
<?code-excerpt "strong/test/strong_test.dart (cast)" replace="/cast/[!$&!]/g"?>
{% prettify dart %}
Map<String, dynamic> json = getFromExternalSource();
var names = json['names'] as List;
assumeStrings(names.[!cast!]<String>());
{% endprettify %}

<aside class="alert alert-info" markdown="1">
  **Замечание по версии:**
  Метод `cast()` был введён в 2.0.0-dev.22.0.
</aside>

Если вы не можете сузить тип или использовать `cast`,
вы можете скопировать объект дргим путём.
Например:

{:.runtime-success}
<?code-excerpt "strong/test/strong_test.dart (create-new-object)" replace="/\.map.*\.toList../[!$&!]/g"?>
{% prettify dart %}
Map<String, dynamic> json = getFromExternalSource();
var names = json['names'] as List;
// До 2.0.0-dev.22.0 используйте `as` и `toList` до, или `cast`, если доступен:
assumeStrings(names[!.map((name) => name as String).toList()!]);
{% endprettify %}

## Приложение

### Ключевое слово covariant

Некоторые (редко используемые) паттерны кодирования полагаются на сужение типа,
путём переопределения типа параметра на его подтип, который недопустим.
В этом случае, вы можете использовать ключевое слово `covariant`, чтобы сказать
анализатору, что вы делаете это намерено.
Это убирает статические ошибки и вместо этого, проверяет
правильность типов аргументов во время исполнения.

<aside class="alert alert-info" markdown="1">
  **Замечание по версии:**
  Ключевое слово `covariant` было введено в 1.22.
  Это замена аннотации `@checked`.
</aside>

Далее показывается, как вы можете использовать `covariant`:

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

Хотя этот пример показывает использование `covariant` в подтипе,
Ключевое слово `covariant` может быть помещено либо в методе суперкласса,
либо в методе подкласса.
Обычно метод супекласса - лучшее место, чтобы его разместить.
Ключевое слово `covariant` применяет единственный параметр
и также поддерживается в setter'ах и полях.

[bottom type]: https://en.wikipedia.org/wiki/Bottom_type
[dartanalyzer README]: https://github.com/dart-lang/sdk/tree/master/pkg/analyzer_cli#dartanalyzer
[Iterable]: {{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/Iterable-class.html
[implicit casts]: /guides/language/analysis-options#enabling-additional-type-checks
[List]: {{site.dart_api}}/{{site.data.pkg-vers.SDK.channel}}/dart-core/List-class.html
[top type]: https://en.wikipedia.org/wiki/Top_type
