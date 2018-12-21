---
title: поддержка JSON
description: решения на Dart для работы с JSON.
---

Большинство мобильных и веб приложений используют JSON для таких задач, как обмен
данными с web сервером.
На этой странице обсуждается поддержка JSON _серилизации_ и _дисериализации_:
конвертирования Dart объектов в/из JSON.

## Библиотеки

Следующие библиотеки и пакеты полезны на всех Dart платформах:

* [dart:convert](/guides/libraries/library-tour#dartconvert---decoding-and-encoding-json-utf-8-and-more)<br>
  Конверторы для JSON и UTF-8
  (кодировка, которую требует JSON).

* [package:json_serializable](https://pub.dartlang.org/packages/json_serializable)<br>
  Лёгкий в использовании пакет для генерации кода.
  Когда вы добавляете анотации с мета данными
  и используете сборщик предоставленный этим пакетом,
  система сборки Dart генерирует сериализованный и десиреализованный код для вас.

* [package:built_value](https://pub.dartlang.org/packages/built_value)<br>
  Мощная, самодостаточная альтернатива для json_serializable.

## Flutter ресурсы

[JSON и сериализация](https://flutter.io/json/)
: Показано, как приложения на Flutter могут сериализовать и десериализовать с dart:convert и c json_serializable.


## Web приложения ресурсы

[AngularDart руководство, часть 6: HTTP]({{site.webdev}}/angular/tutorial/toh-pt6)
: Иллюсрации как web приложения на Dart могут взаимодействовать с RESTful бекэндом, используя JSON данные.

[Использование HTTP ресурсов с HttpRequests]({{site.webdev}}/guides/html-library-tour#using-http-resources-with-httprequest)
: Демонстрации как использовать HttpRequest для обмена данными с web сервером.
Часть кода [Тур в HTML библиотеку.]({{site.webdev}}/guides/html-library-tour)

## VM ресурсы

[Написание HTTP клиентов и серверов](/tutorials/dart-vm/httpserver)
: Посмотрите, как легко реализовать клиента командной строки и сервер, обменивающийся JSON данными.


{% comment %}
## Другие утилиты и ресурсы
{% endcomment %}
