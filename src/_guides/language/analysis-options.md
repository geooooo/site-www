---
title: Настройка статического анализаCustomize Static Analysis
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
Вы можете игнорировать их.
</aside>

Если анализатор не может найти файл с опциями в корне пакета,
он идёт вверх по дереву директорий в поисках его.
Если нет доступного файла, анализатор использует стандартные проверки.

Рассмортим следующую структуру директории для большого проекта:

{% asset guides/analysis-options-directory-structure.png alt="project root contains analysis_options.yaml (#1) and 3 packages, one of which (my_package) contains an analysis_options.yaml file (#2)." %}

Анализатор будет использовать файл #1, чтобы анализировать код в `my_other_package`
и `my_other_other_package`, а файл #2, чтобы анализировать код в `my_package`.


## Enabling additional type checks

If you want stricter static checks than
the [Dart type system][sound-dart] requires,
consider using the `implicit-casts` and `implicit-dynamic` flags:

{% prettify yaml %}
analyzer:
  strong-mode:
    implicit-casts: false
    implicit-dynamic: false
{% endprettify %}

You can use the flags together or separately;
both default to `true`.
The presence of either flag, regardless of value, enables
the Dart 2 type system.

{% comment %}
**PENDING:
Will these flags still appear under strong-mode in Dart 2.0?
Should we mention related command-line flags
(--no-implicit-casts, --no-implicit-dynamic)?**
{% endcomment %}

`implicit-casts: <bool>`
: A value of `false` ensures that the type inference engine never
  implicitly casts to a more specific type.
  The following valid Dart code
  includes an implicit downcast that would be caught by this flag:

{% prettify dart %}
Object o = ...;
String s = o;  // Implicit downcast
String s2 = s.substring(1);
{% endprettify %}

`implicit-dynamic: <bool>`
: A value of `false` ensures that the type inference engine never chooses
  the `dynamic` type when it can't determine a static type.

{% comment %}
TODO: Clarify that description, and insert an example here.
{% endcomment %}


## Enabling linter rules

The analyzer package also provides a code linter. A wide variety of
[linter rules](http://dart-lang.github.io/linter/lints/)
are available. Linters tend to be
nondenominational&mdash;rules don't have to agree with each other.
For example, some rules are more appropriate for library packages
and others are designed for Flutter apps.
Note that linter rules can have false positives, unlike static analysis.

To enable a linter rule, add `linter:` to the analysis options file,
followed by `rules:`.
On subsequent lines, specify the rules that you want to apply,
prefixed with dashes. For example:

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

## Excluding files

Perhaps you rely on code generated from a package that
you don't own&mdash;the generated code works,
but produces errors during static analysis.
You can exclude files from static analysis using the `exclude:` field.

{% prettify yaml %}
analyzer:
  exclude:
    - lib/client/piratesapi.dart
{% endprettify %}

You can specify a group of files using
[glob](https://pub.dartlang.org/packages/glob) syntax:

{% prettify yaml %}
analyzer:
  exclude:
    - src/test/_data/**
    - test/*_example.dart
{% endprettify %}

## Excluding lines within a file

Perhaps one of the linter rules causes a false positive and you
want to suppress that warning.
To suppress a specific rule on a specific line of code,
preceed that line with a comment using the following format:

{% prettify dart %}
// ignore: <linter rule>
{% endprettify %}

For example:

{% prettify dart %}
// ignore: invalid_assignment
int x = '';
{% endprettify %}

If you want to suppress more than one rule, supply a comma-separated list.
For example:

{% prettify dart %}
// ignore: invalid_assignment, const_initialized_with_non_constant_value
const x = y;
{% endprettify %}

Alternatively, you can append the ignore rule to the line that it applies to.
For example:

{% prettify dart %}
int x = ''; // ignore: invalid_assignment
{% endprettify %}

## Ignoring specific analysis rules

Sometimes your code doesn't fit perfectly within the standard
analysis guidelines, or violates a rule here or there, for
reasons you'd rather not get into. You can ignore specific
rules during analysis using the `errors:` field. List the
rule, followed by `: ignore`. For example:

{% prettify yaml %}
analyzer:
  errors:
    todo: ignore
{% endprettify %}

This analysis options file instructs the analysis tools to ignore
the TODO rule.

Alternatively, as of Dart 1.24 you can ignore a specific rule for a
specific file using an `ignore_for_file` comment:

{% prettify dart %}
// ignore_for_file: unused_import
{% endprettify %}

This acts for the whole file, before or after the comment, and is
particularly useful for generated code. A comma-separated list may be
used to suppress more than one rule:

{% prettify dart %}
// ignore_for_file: unused_import, invalid_assignment
{% endprettify %}

## Changing the severity of analysis rules

Using the same mechanism, you can also globally change the severity
of a particular rule using one of the following values: `warning`,
`error`, or `info`. This works for regular analysis issues as well as
for lints. For example:

{% prettify yaml %}
analyzer:
  errors:
    invalid_assignment: warning
    missing_return: error
    dead_code: info
{% endprettify %}

This analysis options file instructs the analysis tools to
ignore unused local variables, treat invalid assignments as warnings and
missing returns as errors, and only provide information about dead code.

## Resources

{% comment %}
Join the discussion list for linter enthusiasts:

* [linter-discuss (TBD)](xxx)  Doesn't exist yet...
{% endcomment %}

Use the following resources to learn more about static analysis in Dart:

* [Dart's Type System][sound-dart]
* [Dart linter](https://github.com/dart-lang/linter#linter-for-dart)
* [Dart linter rules](http://dart-lang.github.io/linter/lints/)
* [dartanalyzer](https://github.com/dart-lang/sdk/tree/master/pkg/analyzer_cli#dartanalyzer)
* [dartdevc]({{site.webdev}}/tools/dartdevc)
* [analyzer package](https://pub.dartlang.org/packages/analyzer)

[sound-dart]: /guides/language/sound-dart
