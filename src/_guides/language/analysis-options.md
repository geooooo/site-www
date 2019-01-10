---
title: Настройка статического анализа
description: Настройка статического анализа, используя файл с опциями анализатора.
---

Статический анализ позволяет вам найти проблемы перед
исполнением строк кода. Его мощный инструмент, используется для
предотвращения багов и обеспечения следованию руководствам по стилю.
С помощью анализатора, вы можете найти простые ошибки.
Например, случайно поставленная таким образом точка с запятой в
инструкции `if`:

{% asset guides/avoid-empty-statements.png alt="`if (count < 10);` results in a hint: Avoid empty statements." %}

Анализатор может также помощь вам найти больше тонких проблем.
Например, вы возможно забыли закрыть метод sink:

{% asset guides/close-sinks.png alt="`_controller = new StreamController()` results in a hint: Close instances of `dart.core.Sink`." %}

В экосистеме Dart, Сервер Анализа Dart и другие утилиты используют
[пакет analyzer](https://pub.dartlang.org/packages/analyzer),
чтобы выполнять статический анализ.

Вы можете настроить статический анализ для поиска потенциальных проблем,
включая ошибки и предупреждения, указаные в
[спецификации языка Dart](/guides/language/spec).
Вы также можете конфигурировать линтер - один из плагинов
анализатора, который обеспечивает следование вашего кода
[Руководству по стилю Dart](/guides/language/effective-dart/style)
и другие предложениям в руководстве
[Эффективный Dart](/guides/language/effective-dart).
Утилиты Dart, такие как
[компилятор для разработки на Dart (dartdevc),]({{site.webdev}}/tools/dartdevc)
[`dartanalyzer`,](https://github.com/dart-lang/sdk/tree/master/pkg/analyzer_cli#dartanalyzer)
[`flutter analyze`,](https://flutter.io/debugging/#the-dart-analyzer)
и [JetBrains IDEs](/tools/jetbrains-plugin)
используют анализатор для оценки вашего кода.

Этот документ объясняет как настроить поведение анализатора, используя
файл опций анализа. Если вы хотите добавить статический анализ в вашу утилиту,
смотрите  документацию к [пакету analyzer](https://pub.dartlang.org/packages/analyzer) и
[Спецификацию API Сервера Анализа](https://htmlpreview.github.io/?https://github.com/dart-lang/sdk/blob/master/pkg/analysis_server/doc/api.html).

<aside class="alert alert-info" markdown="1">
**Замечание:**
Коды ошибок анализатора перечислены в [репозитории Dart SDK.](https://github.com/dart-lang/sdk/blob/master/pkg/analyzer/lib/error/error.dart)
</aside>

## Файл опций анализа

Место файла опций анализа `analysis_options.yaml`
в корне пакета, в той же директории, что и файл pubspec.

<aside class="alert alert-warning" markdown="1">
  **Несовместимые изменения:**
  По конвенции имя файла с опциями анализа было `.analysis_options`
  (заметьте идущую в начале точку и пропущенный суффикс `.yaml`).
  Мы ожидаем, что поддержка для имени `.analysis_options` в будущих релизах
  будет прекращена, так что мы рекомендуем **вам переименовать файлы `.analysis_options`
  в `analysis_options.yaml`.**
  {% comment %}
  Tracking issue: https://github.com/dart-lang/sdk/issues/28385
  {% endcomment %}
</aside>

Вот пример файл с опциями анализа:

{% prettify yaml %}
analyzer:
  strong-mode:
    implicit-casts: false
  errors:
    todo: ignore
  exclude:
    - flutter/**
    - lib/api/*.dart

linter:
  rules:
    - avoid_empty_else
    - cancel_subscriptions
    - close_sinks
    - unnecessary_const
    - unnecessary_new
{% endprettify %}

YAML чувствителен к пробелам &mdash; не используйте табуляции в файле YAML,
и используйте 2 пробела, чтобы обозначить каждый уровень вложености.

<aside class="alert alert-info" markdown="1">
**Замечание**:
Вы можете встретить тег `language:` в файле опций анализа.
Этот тег используется для тестирования эксперементальных возможностей.
Вы можете игнорировать его.
</aside>

Если анализатор не может найти файл с опциями в корне пакета,
он идёт вверх по дереву директорий в поисках его.
Если нет доступного файла, анализатор использует стандартные проверки.

Рассмортим следующую структуру директории для большого проекта:

{% asset guides/analysis-options-directory-structure.png alt="project root contains analysis_options.yaml (#1) and 3 packages, one of which (my_package) contains an analysis_options.yaml file (#2)." %}

Анализатор будет использовать файл #1, чтобы анализировать код в `my_other_package`
и `my_other_other_package`, а файл #2, чтобы анализировать код в `my_package`.


## Включение дополнительных проверок типов

Если вы хотите сделать статические проверки строже, чем
требует [система типов Dart][sound-dart],
рассмотрите использование флагов `implicit-casts` и `implicit-dynamic`:

{% prettify yaml %}
analyzer:
  strong-mode:
    implicit-casts: false
    implicit-dynamic: false
{% endprettify %}

Вы можете использовать флаги вместе или раздельно; оба поумолчанию `true`.
Наличие любого флага, независимо от значения, включает систему типов Dart 2.

{% comment %}
**PENDING:
Will these flags still appear under strong-mode in Dart 2.0?
Should we mention related command-line flags
(--no-implicit-casts, --no-implicit-dynamic)?**
{% endcomment %}

`implicit-casts: <bool>`
: Значение `false` гарантирует, что механизм вывода типов
  никогда не приводит к неявному приведению к более конкретному типу.
  Следующий правильный код на Dart включает в себя неявный downcast,
  который будет пойман этим флагом:

{% prettify dart %}
Object o = ...;
String s = o;  // Неявный downcast
String s2 = s.substring(1);
{% endprettify %}

`implicit-dynamic: <bool>`
: Значение `false` гарантирует, что механизм вывода типов никогда
  не выбирает тип `dynamic`, когда он не может определить статический тип.

{% comment %}
TODO: Clarify that description, and insert an example here.
{% endcomment %}


## Включение правил линтера

Пакет анализатора также предоставляет линтер кода.
Доступно большое разнаообразие [правил линтера](http://dart-lang.github.io/linter/lints/)
Линтеры, как правило, не деноминационные - правила не должны согласовываться друг с другом.
Например, некоторые правила больше подходят для пакетов библиотек, а
другие спроектированы для приложений на Flutter.
Заметьте, что правила линтера приводят к ложному результату в отличии от статического анализа
{% comment %}
TODO: Не совсем понял как правильно перевести последнее предложение выше.
{% endcomment %}

Чтобы активировать правило линтера, добавте `linter:` в файл опций анализа,
а затем `rules:`. Далее укажите правила, которые вы хотите применить,
указывая префикс - тире. Например:

{% prettify yaml %}
linter:
  rules:
    - always_declare_return_types
    - camel_case_types
    - empty_constructor_bodies
    - annotate_overrides
    - avoid_init_to_null
    - constant_identifier_names
    - one_member_abstracts
    - slash_for_doc_comments
    - sort_constructors_first
    - unnecessary_brace_in_string_interps
{% endprettify %}

{% comment %}
Brian expressed concern about including this:
In future, related lint rules may be coalesced into meta rules. See
[Issue 99: Meta linter rules](https://github.com/dart-lang/linter/issues/288)
for more information.
{% endcomment %}

## Исключение файлов

Возможно вы полагаетесь на код, сгенерированый пакетом, который
вам не принадлежит&mdash;сгенерированый код работает,
но выдаёт ошибки во время статического анализа.
Вы можете исключить файлы из статического анализа, используя поле `exclude:`.

{% prettify yaml %}
analyzer:
  exclude:
    - lib/client/piratesapi.dart
{% endprettify %}

Вы можете указать группу файлов, используя
синтаксис [glob](https://pub.dartlang.org/packages/glob):

{% prettify yaml %}
analyzer:
  exclude:
    - src/test/_data/**
    - test/*_example.dart
{% endprettify %}

## Исключение строк файла

Возможно одно из правил линтера приводит к ошибке линтинга и вы
хотите подавить это предупрждение.
Чтобы подавить конкретное правило на конкретной строке кода,
Предварите эту строку коментарием, используя следующий формат:

{% prettify dart %}
// ignore: <linter rule>
{% endprettify %}

Например:

{% prettify dart %}
// ignore: invalid_assignment
int x = '';
{% endprettify %}

Если вы хотите подавить больше, чем одно правило, вставте список, через запятую
If you want to suppress more than one rule, supply a comma-separated list.
Например:

{% prettify dart %}
// ignore: invalid_assignment, const_initialized_with_non_constant_value
const x = y;
{% endprettify %}

Другим образом, вы можете добавить игнорируемое правило к строке, к которой его применяете.
Например:

{% prettify dart %}
int x = ''; // ignore: invalid_assignment
{% endprettify %}

## Игнорирование конкретных правил анализа

Иногда ваш код не совсем вписывается в стандартные правила анализа
или нарушает правила тут или там, по причинам, которым вы не хотите.
Вы можете игнорировать указаные правила во время анализа, используя
поле `errors:`. Запишите правило, затем `: ignore`.
Например:

{% prettify yaml %}
analyzer:
  errors:
    todo: ignore
{% endprettify %}

Этот файл с опциями анализа указывает утилитам анализа игнорировать правило TODO.

Другим образом, как в Dart 1.24 вы можете игнорировать указаное правило для
конкретного файла, используя комментарий: `ignore_for_file`:

{% prettify dart %}
// ignore_for_file: unused_import
{% endprettify %}

Это действует для всего файла, перед и после комментария и особо полезно
для сгенерированного кода. Список через запятую может быть использован,
чтобы подавить больше чем одно правило:

{% prettify dart %}
// ignore_for_file: unused_import, invalid_assignment
{% endprettify %}


## Изменение строгости правил анализа

Используя такой механизм, вы можете также глобально изменить строгость
конкретного правила, используя одно из следующих значений:
`warning`, `error` или `info`.
Это работает для регулярных проблем анализа также хорошо, как и для лингинга.
Например:

{% prettify yaml %}
analyzer:
  errors:
    invalid_assignment: warning
    missing_return: error
    dead_code: info
{% endprettify %}

Этот файла опций анализа указывает утилитам анализа, чтобы они
игнорировали неиспользуемые локальные переменные,
рассматривали неправильные присваивания как предупреждения
и пропущенные инструкции return как ошибки и предоставлять информацию только о мёртвом коде.


## Ресурсы

{% comment %}
Join the discussion list for linter enthusiasts:

* [linter-discuss (TBD)](xxx)  Doesn't exist yet...
{% endcomment %}

Используйте следующие ресурсы, чтобы узнать больше о статическом анализе в Dart:

* [Dart's Type System][sound-dart]
* [Dart linter](https://github.com/dart-lang/linter#linter-for-dart)
* [Dart linter rules](http://dart-lang.github.io/linter/lints/)
* [dartanalyzer](https://github.com/dart-lang/sdk/tree/master/pkg/analyzer_cli#dartanalyzer)
* [dartdevc]({{site.webdev}}/tools/dartdevc)
* [analyzer package](https://pub.dartlang.org/packages/analyzer)

[sound-dart]: /guides/language/sound-dart
