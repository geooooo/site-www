---
layout: default
title: "Тестирование на Dart"
description: "Как тестировать Flutter, Web, и VM приложения."
---

Тестирование программного обеспечения - важная часть разработки приложений, помогающая проверить,
что ваше приложение работает корректно перед тем, как вы его отправите в релиз.
Это руководство по тестированию на Dart охватывает несколько типов тестирования и
точек, где вы можете изучить как тестировать ваше
[мобильное]({{site.flutter}}), [web]({{site.webdev}}),
и [серверное приложение и скрипт](/dart-vm).

<aside class="alert alert-info" markdown="1">
**Терминология: виджеты против компонентов**<br>
Flutter это SDK для сборки приложений под iOS и Android, определяет
такие элементы GUI как _виджеты_. AngularDart - фреймворк для web приложений, определяет
такие элементы GUI как  _компоненты_.
Этот документ использует понятие **компонент** (За исключением, когда ведётся разговор конкретно про Flutter), но оба термина подразумевают одно и то же.
</aside>

## Виды тестирования

Документация по тестированию на Dart фокусируется на трёх видах тестирования,
вне [многих видов тестирования](https://en.wikipedia.org/wiki/Software_testing)
так что вы может быть знакомы с: модульным, компонентным и e2e тестированием (форма интеграционного тестирования).
Терминология тестирования варьируется, но вот термины и концепции, с которыми вы скорее всего столкнётесь,
когда будете использовать технологии на Dart:

* _Модульные_ тесты фокусируются на проверке мелких кусоков тестируемой программы, таких как функции,
  методы или классы. Ваше наборы тестов должны иметь больше модульных тестов чем других видов тестов.

* _Компонентные_ тесты проверяют, чтобы компонент (который обычно состоит из множества классов)
  вёл себя ожидаемым образом. Компонентные тесты часто требуют использования mock объектов, чтобы
  мочь имитировать действия пользователя, события, представлять расположение и создавать
  дочерние компоненты.

* _Интеграционные_ и _e2e_ тесты проверяют поведение целого приложения или большой части приложения.
  Интеграционные тесты в общем запускаются на реальных устройствах или эмуляторах ОС (для мобильных) или
  в браузере (для web) и состоят из двух частей:
  самого приложения и тестового приложения, чтобы пропустить приложение через несколько шагов.
  Интеграционные тесты часто измеряют производительность, таким образом тестируемое
  приложение в обычно запускается на отдельном устройстве или ОС, чтобы приложение могло быть протестировано.

## Наиболее полезные библиотеки

Although your tests partly depend on the platform your code is intended
for&mdash;Flutter, the web, or server-side, for example&mdash;the
following packages are useful across Dart platforms:

* [package:test](https://pub.dartlang.org/packages/test)<br>
  Provides a standard way of writing tests in Dart. You can use the test
  package to:
    * Write single tests, or groups of tests.
    * Use the `@TestOn` annotation to restrict tests to run on
      specific environments.
    * Write asynchronous tests just as you would write synchronous
      tests.
    * Tag tests using the `@Tag` annotation. For example, define a tag to
      create a custom configuration for some tests, or to identify some tests
      as needing more time to complete.
    * Create a `dart_test.yaml` file to configure tagged tests across
      multiple files or an entire package.


* [package:mockito](https://pub.dartlang.org/packages/mockito)<br>
  Provides a way to create
  [mock objects,](https://en.wikipedia.org/wiki/Mock_object)
  easily configured for use in fixed scenarios, and to verify
  that the system under test interacts with the mock object in
  expected ways.
  For an example that uses both package:test and package:mockito,
  see the [International Space Station API library and its unit
  tests](https://github.com/dart-lang/mockito/tree/master/test/example/iss)
  in the [mockito package](https://github.com/dart-lang/mockito).

## Flutter testing

Use the following resources to learn more about testing Flutter apps:

* [Testing Flutter Apps](https://flutter.io/testing/)<br>
  How to perform unit, widget, or integration tests on a Flutter app.
* [flutter_test](https://docs.flutter.io/flutter/flutter_test/flutter_test-library.html)<br>
  A testing library for Flutter built on top of package:test.
* [flutter_driver](https://docs.flutter.io/flutter/flutter_driver/flutter_driver-library.html)<br>
  A testing library for testing Flutter applications on real devices and
  emulators (in a separate process).
* [flutter/examples/flutter_gallery](https://github.com/flutter/flutter/tree/master/examples/flutter_gallery)<br>
  Tests for the Flutter gallery example.
* [flutter/dev/manual_tests](https://github.com/flutter/flutter/tree/master/dev/manual_tests)<br>
  Many examples of tests in the Flutter SDK.

## Web testing

Use the following resources to learn more about testing Dart web
applications:

* [Testing]({{site.webdev}}/angular/guide/testing)(a page
  in the AngularDart guide)<br>
  How to use the [angular_test](https://pub.dartlang.org/packages/angular_test)
  package to test AngularDart components and subsystems.
  <!-- More pages are coming! -->
* [package:webdriver](https://pub.dartlang.org/packages/webdriver)<br>
  A Dart package for interfacing with
  [WebDriver](https://www.w3.org/TR/webdriver/) servers.

## Other tools and resources

You may also find the following resources useful for developing and
debugging Dart applications.

### IDE

When it comes to debugging, your first line of defense is your IDE.
Dart plugins exist for many commonly used IDEs.

If you don't have a preferred IDE, try
[WebStorm]({{site.webdev}}/tools/webstorm) for web apps, or
[IntelliJ](https://www.dartlang.org/tools/jetbrains-plugin) for Flutter.
The JetBrains products have a full-featured Dart debugger, and WebStorm and
IntelliJ Ultimate include additional built-in support for running test suites.

### Observatory

Observatory is a browser-based tool for profiling and debugging your
Dart applications. You can learn more using the following resources:

* [Observatory: A Profiler for Dart
  Apps](https://dart-lang.github.io/observatory/)
* [Dart
  Observatory](https://flutter.io/debugging/#dart-observatory-statement-level-single-stepping-debugger-and-profiler),
  a section in [Debugging Flutter Apps](https://flutter.io/debugging/)
* [Dart VM
  Observatory](https://groups.google.com/a/dartlang.org/forum/#!forum/observatory-discuss)
  discussion group

### Continuous integration

Consider using continuous integration (CI) to build your project
and run its tests after every commit. Two CI services for GitHub are
[Travis CI](https://travis-ci.org/) (for OS X and Unix) and
[AppVeyor](https://www.appveyor.com/) (for Windows).

Travis has built-in support for Dart projects.
Learn more at the following links:

* [Building a Dart Project](https://docs.travis-ci.com/user/languages/dart)
  covers how to configure Travis for Dart projects
* The [shelf](https://github.com/dart-lang/shelf/blob/master/.travis.yml)
  example uses the `dart_task` tag (in `.travis.yml`) to configure
  the build.
