---
title: "Эффективный Dart: Стиль кодирования"
description: Правила форматирования и именования для консистентности и читаемоести кода.
nextpage:
  url: /guides/language/effective-dart/documentation
  title: Документация
prevpage:
  url: /guides/language/effective-dart
  title: Обзор
---
<?code-excerpt plaster="none"?>

Удивительно важной частью хорошего кода - хороший стиль кодирования.
Последовательное именование, упорядочивание и форматирование помогают одинаковому коду
*выглядеть* одинаково. Это даёт преимущество от мощной аппартной системы сопоставления
шаблонов, которую мы имеем в своей зрительной системе.
Если мы используем последовательный стиль кодирования на всей экосистеме Dart,
всем нам становиться легче учиться и вносить вклад в код друг друга.

* TOC
{:toc}

## Идентификаторы

Идентификаторы в Dart представлены в трёх видах.

*   `UpperCamelCase` имена с заглавной буквы в каждом слове, включая первое.

*   `lowerCamelCase` имена с заглавной буквы в каждом слове, за *исключением* первого,
    которое всегда должно начинаться с маленькой, если это не акроним.

*   `lowercase_with_underscores` использует только маленькие буквы, даже для акронимов,
    для разделения слов используется `_`.


### ДЕЛАТЬ имена типов, используя `UpperCamelCase`.

Классы, перечислимые типы, объеявления типов и параметры типы должны
соответствовать виду: `UpperCamelCase`.

{:.good-style}
<?code-excerpt "misc/lib/effective_dart/style_good.dart (type-names)"?>
{% prettify dart %}
class SliderMenu { ... }

class HttpRequest { ... }

typedef Predicate<T> = bool Function(T value);
{% endprettify %}

Это даже включает классы, предназначенные для использования в аннотациях метаданных.

{:.good-style}
<?code-excerpt "misc/lib/effective_dart/style_good.dart (annotation-type-names)"?>
{% prettify dart %}
class Foo {
  const Foo([arg]);
}

@Foo(anArg)
class A { ... }

@Foo()
class B { ... }
{% endprettify %}

Если конструктор класса аннотации не получает параметры,
вы можете создать отдельную константу `lowerCamelCase` для него.

{:.good-style}
<?code-excerpt "misc/lib/effective_dart/style_good.dart (annotation-const)"?>
{% prettify dart %}
const foo = Foo();

@foo
class C { ... }
{% endprettify %}


### ДЕЛАТЬ имена библиотек и исходных файлов, используя формат `lowercase_with_underscores`.

Некоторые файловые системы не регистро-зависимы, так многие проекты требуют, чтобы
все имена файлов были в нижнем регистре. Использование разделительного символа
позволяет именам оставаться в читаемом виде. Использование нижних подчёркиваний, как
разеделителя гарантирует, что имена остаются валидными идентификаторами Dart,
что может быть полезно, если позже язык будет поддерживать символьный импорт.

{:.good-style}
<?code-excerpt "misc/lib/effective_dart/style_lib_good.dart" replace="/foo\///g"?>
{% prettify dart %}
library peg_parser.source_scanner;

import 'file_system.dart';
import 'slider_menu.dart';
{% endprettify %}

{:.bad-style}
<?code-excerpt "misc/lib/effective_dart/style_lib_good.dart" replace="/foo\///g;/file./file-/g;/slider_menu/SliderMenu/g;/source_scanner/SourceScanner/g;/peg_parser/pegparser/g"?>
{% prettify dart %}
library pegparser.SourceScanner;

import 'file-system.dart';
import 'SliderMenu.dart';
{% endprettify %}

<aside class="alert alert-info" markdown="1">
  **Замечание:**
  Это руководство указывает *как* назвать библиотеку, *если вы выбираете для неё имя*.
  Вы можете *опустить* директиву библиотеки в файле, если хотите.
</aside>


### ДЕЛАТЬ имя префикса импорта, ипользуя форму `lowercase_with_underscores`.

{:.good-style}
<?code-excerpt "misc/lib/effective_dart/style_lib_good.dart (import-as)" replace="/(package):examples[^']*/$1:angular_components\/angular_components/g"?>
{% prettify dart %}
import 'dart:math' as math;
import 'package:angular_components/angular_components'
    as angular_components;
import 'package:js/js.dart' as js;
{% endprettify %}

{:.bad-style}
<?code-excerpt "misc/lib/effective_dart/style_lib_good.dart (import-as)" replace="/(package):examples[^']*/$1:angular_components\/angular_components/g;/as angular_components/as angularComponents/g;/ math/ Math/g;/as js/as JS/g"?>
{% prettify dart %}
import 'dart:math' as Math;
import 'package:angular_components/angular_components'
    as angularComponents;
import 'package:js/js.dart' as JS;
{% endprettify %}


### ДЕЛАТЬ имя других идентификаторов, используя `lowerCamelCase`.

Члены класса, глобальные идентификаторы, переменные, параметры и именованные параметры
должны следовать форме `lowerCamelCase`.

{:.good-style}
<?code-excerpt "misc/lib/effective_dart/style_good.dart (misc-names)"?>
{% prettify dart %}
var item;

HttpRequest httpRequest;

void align(bool clearItems) {
  // ...
}
{% endprettify %}


### ПРЕДПОЧИТАТЬ использовать `lowerCamelCase` для имён констант.

В новом годе, используйте `lowerCamelCase` для переменных констант, включая перечислимые значения.
В существующем коде, который использует `SCREAMING_CAPS`, вы можете продолжить использовать такой формат, чтобы
код оставлся однообразным.

{:.good-style}
<?code-excerpt "misc/lib/effective_dart/style_good.dart (const-names)"?>
{% prettify dart %}
const pi = 3.14;
const defaultTimeout = 1000;
final urlScheme = RegExp('^([a-z]+):');

class Dice {
  static final numberGenerator = Random();
}
{% endprettify %}

{:.bad-style}
<?code-excerpt "misc/lib/effective_dart/style_bad.dart (const-names)"?>
{% prettify dart %}
const PI = 3.14;
const DefaultTimeout = 1000;
final URL_SCHEME = RegExp('^([a-z]+):');

class Dice {
  static final NUMBER_GENERATOR = Random();
}
{% endprettify %}

<aside class="alert alert-info" markdown="1">
  **Замечание:**
  Изначально мы использовали запись `SCREAMING_CAPS` для констант.
  Мы изменили этот стиль, потому что:

  *   `SCREAMING_CAPS` плохо выглядит в большинстве случаев, в частности
      значений перечислимого типа, для таких вещей как CSS цвета.
  *   Константы часто меняются на финальные не константные значения, которые
      потребуют изменения имени.
  *   Свойсво `values` автоматически определено в перечислимом типе,
      оно является константой и в нижнем регистре.
</aside>

{% comment %}
NOTE: Какой-то не совсем понятный раздел ниже
{% endcomment %}

### ДЕЛАТЬ привильно записывать регистр букв акронимов и аббревиатур.

Акронимы, записанные большими буквами могут быть сложны для чтения
и множество смежных акронимов может привести к двусмысленным именам.
Например, дано имя, начинающееся с `HTTPSFTP`, нельзя точно сказать,
значит ли оно HTTPS FTP  или HTTP SFTP.

Чтобы этого избежать, акронимы и аббревиатуры записываются прописными буквами,
как обычные слова, за исключенимем  дву-буквенных акронимов. (дву-буквенные *аббревиатуры*,
как ID и Mr. остаются записанными с большой буквы как слова.)

{:.good-style}
<?code-excerpt "misc/lib/effective_dart/style_good.dart (acronyms and abbreviations)" replace="/,//g"?>
{% prettify dart %}
HttpConnectionInfo
uiHandler
IOStream
HttpRequest
Id
DB
{% endprettify %}

{:.bad-style}
{% prettify dart %}
HTTPConnection
UiHandler
IoStream
HTTPRequest
ID
Db
{% endprettify %}


### НЕ НАДО использовать префиксные буквы

[Венгерская нотация](https://en.wikipedia.org/wiki/Hungarian_notation) и
другие схемы возникли во времена BCPL, когда компиляторы не могли помочь
понять ваш код в значительной мере. Так как Dart может сказать вам тип,
область видимости, изменяемость и другие свойства ваших объявлений, не
имеет неободимости, кодировать эти свойства в имена идентификаторов.

{:.good-style}
{% prettify dart %}
defaultTimeout
{% endprettify %}

{:.bad-style}
{% prettify dart %}
kDefaultTimeout
{% endprettify %}


## Упорядочивание

Чтобы держать начало вашего файла в порядке,
у нас есть предписанный порядок, в котором должны появляться директивы.
Каждый "раздел" должен отделяться пустой строкой.


### ДЕЛАТЬ поместите импорты "dart:" перед другими импортами.

{:.good-style}
<?code-excerpt "misc/lib/effective_dart/style_lib_good.dart (dart-import-first)" replace="/\w+\/effective_dart\///g"?>
{% prettify dart %}
import 'dart:async';
import 'dart:html';

import 'package:bar/bar.dart';
import 'package:foo/foo.dart';
{% endprettify %}


### ДЕЛАТЬ поместите импорты "package:" перед относительными импортами.

{:.good-style}
<?code-excerpt "misc/lib/effective_dart/style_lib_good.dart (pkg-import-before-local)" replace="/\w+\/effective_dart\///g;/'foo/'util/g"?>
{% prettify dart %}
import 'package:bar/bar.dart';
import 'package:foo/foo.dart';

import 'util.dart';
{% endprettify %}


### ПРЕДПОЧИТАТЬ размещение импорты "третьих сторон" "package:" перед другими импортами.

Если у вас есть несколько импортов "package:" для вашего собственного пакета вместе с другими сторонними пакетами,
поместите ваши в отдельном разделе после внешних.

{:.good-style}
<?code-excerpt "misc/lib/effective_dart/style_lib_good.dart (third-party)" replace="/\w+\/effective_dart\///g;/(package):foo(.dart)/$1:my_package\/util$2/g"?>
{% prettify dart %}
import 'package:bar/bar.dart';
import 'package:foo/foo.dart';

import 'package:my_package/util.dart';
{% endprettify %}


### ДЕЛАТЬ укажите экспорты в отдельном разделе после всех импортов.

{:.good-style}
<?code-excerpt "misc/lib/effective_dart/style_lib_good.dart (export)"?>
{% prettify dart %}
import 'src/error.dart';
import 'src/foo_bar.dart';

export 'src/error.dart';
{% endprettify %}

{:.bad-style}
<?code-excerpt "misc/lib/effective_dart/style_lib_bad.dart (export)"?>
{% prettify dart %}
import 'src/error.dart';
export 'src/error.dart';
import 'src/foo_bar.dart';
{% endprettify %}


### ДЕЛАТЬ сортируйте разделы в алфавитном порядке.

{:.good-style}
<?code-excerpt "misc/lib/effective_dart/style_lib_good.dart (sorted)" replace="/\w+\/effective_dart\///g"?>
{% prettify dart %}
import 'package:bar/bar.dart';
import 'package:foo/foo.dart';

import 'foo.dart';
import 'foo/foo.dart';
{% endprettify %}

{:.bad-style}
<?code-excerpt "misc/lib/effective_dart/style_lib_bad.dart (sorted)" replace="/\w+\/effective_dart\///g"?>
{% prettify dart %}
import 'package:foo/foo.dart';
import 'package:bar/bar.dart';

import 'foo/foo.dart';
import 'foo.dart';
{% endprettify %}


## Форматирование

Как и многие языки, Dart игнорирует пробелы, Однако, *люди* не игнорируют их.
Наличие последовательного стиля расстановки пробелов помогает гарантировать, чтобы
люди, читающие код, видели его также как и компилятор.


### ДЕЛАТЬ форматируйте ваш код, используя `dartfmt`.

Форматирование - утомительная работа и в частности во время рефакторинга отимает много времени.
К счастью, вы можете не беспокоиться об этом.
Мы предоставили утончённый автоматизированный форматировщик кода, называемый [dartfmt][],
который делает это для вас. У нас есть  п
У нас есть некоторая [документация][dartfmt docs] по правилам, которые он применяет,
но официальные правила обработки пробелов для Dart - *это то, что выдаёт dartfmt*.

Остальные руководства по форматированию предназначены для тех немногих вещей,
которые dartfmt не может иправить за вас.

[dartfmt]: https://github.com/dart-lang/dart_style
[dartfmt docs]: https://github.com/dart-lang/dart_style/wiki/Formatting-Rules

### РАССМОТРЕТЬ изменить ваш код, чтобы сделать его более дружилюбным для форматировщика.

Форматировщик делает все возможное с любым кодом, который вы даёте ему,
он не может творить чудеса. Если в вашем коде есть длинные идентификаторы,
глубоко вложенные выражения, смесь различных видов операторов и т.п.,
форматированный на выходе код может остаться сложным для чтения.

Когда такое случается, реорганизуйте или упростите ваш код.
Рассмотирте возможность сокращения имён ваших переменных или
вынести выражение в новую локальную переменную.
Другими словами, сделайте такие виды модификации, которые вы сделали бы, если
вы форматировали код вручную и пытались сделать его более читабельным
Думайте о dartfmt как о партнёре, когда вы работаете для создания красивого кода вместе или иногда итеративно.


### ИЗБЕГАТЬ строк длинее, чем 80 символов.

Исследования читабельности показывают, что длинные строки текста труднее читать,
потому что ваш глаз должен двигаться дальше при переходе к началу следующей строки.
Вот почему газеты и журналы используют несколько столбцов текста.

Если вам действительно нужны строки длиной более 80 символов,
наш опыт показывает, что ваш код, вероятно, слишком многословен и может быть немного более компактным.
Основным нарушителем обычно является `VeryLongCamelCaseClassNames`.
Спросите себя: «Каждое слово в этом имени типа говорит мне что-то важное или предотвращает конфликт имен?»
Если нет, рассмотрите возможность опустить его.

Заметьте, что dartfmt делает 99% этого за вас, но последний 1% за вами.
Он не разбивает длинные строковые литералы так, чтобы они помещались в 80 столбцах,
поэтому вы должны сделать это вручную.

Мы делаем исключение для URI адресов и имён файлов.
Когда это происходит в комментариях или строках (обычно в импортах и экспортах),
они могут оставаться оставаться на одной строке, даже если она выходит за пределы строки.
Это облегчает поиск исходных файлов по заданному пути.


### ДЕЛАТЬ использовать фигурные скобки для всех структур управления потоком.

Делайте так воизбежание проблем [свисающего else][].

[свисающего else]: http://en.wikipedia.org/wiki/Dangling_else

{:.good-style}
<?code-excerpt "misc/lib/effective_dart/style_good.dart (curly-braces)"?>
{% prettify dart %}
if (isWeekDay) {
  print('Bike to work!');
} else {
  print('Go dancing or read a book!');
}
{% endprettify %}

К этому есть одно исключение: инструкция `if` без предложения `else`,
где вся инструкция `if` и её тело помещается в одну строку.
В этом случае, вы можете опустить скобки, если вы сочтёте это нужным:

{:.good-style}
<?code-excerpt "misc/lib/effective_dart/style_good.dart (one-line-if)"?>
{% prettify dart %}
if (arg == null) return defaultValue;
{% endprettify %}

Если тело переносится на следующую строку, используйте скобки:

{:.good-style}
<?code-excerpt "misc/lib/effective_dart/style_good.dart (one-line-if-wrap)"?>
{% prettify dart %}
if (overflowChars != other.overflowChars) {
  return overflowChars < other.overflowChars;
}
{% endprettify %}

{:.bad-style}
<?code-excerpt "misc/lib/effective_dart/style_bad.dart (one-line-if-wrap)"?>
{% prettify dart %}
if (overflowChars != other.overflowChars)
  return overflowChars < other.overflowChars;
{% endprettify %}
