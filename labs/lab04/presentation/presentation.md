---
## Front matter
lang: ru-RU
title: "Лабораторная работа №3"
subtitle: "Дискреционное разграничение прав в Linux. Два пользователя"
author:
  - Ильин А.В.
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 23 сентября 2023

## i18n babel
babel-lang: russian
babel-otherlangs: english

## Fonts
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase,Scale=0.9

## Formatting pdf
toc: false
toc-title: Содержание
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
  - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
  - '\makeatletter'
  - '\beamer@ignorenonframefalse'
  - '\makeatother'
---

# Информация

## Докладчик

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

- Ильин Андрей Владимирович
- НФИбд-01-20
- 1032201656
- Российский Университет Дружбы Народов
- [1032201656@pfur.ru](mailto:1032201656@pfur.ru)
- <https://github.com/av-ilin>

:::
::: {.column width="30%"}

![](./images/avilin.jpg)

:::
::::::::::::::

# Вводная часть

## Актуальность

- Приобрести необхдимые в современном научном сообществе навыки администрирования систем и информационной безопасности.

## Цель

- Получение практических навыков работы в консоли с атрибутами файлов для групп пользователей. Закрепление теоретических основ дискреционного разграничения доступа в современных системах с открытым кодом на базе ОС Linux.

## Задачи

1. Создать пользователей guest и guest2. Добавить пользователя guest2 в группу guest.

2. Уточнить информацию о пользователях.

3. Заполнить таблицы "Установленные права и разрешённые действия для групп" и "Минимальные права для совершения операций от имени пользователей входящих в группу", меняя атрибуты у директории `dir1` и файла `file1` от имени пользователя `guest` и делая проверку от пользователя `guest2`.

## Материалы и методы

- Rocky Linux
- Git
- VirtualBox

# Выполнение работы

## Пользователь guest2

![Пользователь guest2](images/01.png)

## Изучение пользователя

:::::::::::::: {.columns align=center}
::: {.column width="50%"}
![Изучение пользователя `guest2`](images/03.png)
:::
::: {.column width="50%"}
![Просмотр `cat /etc/passwd`](images/04.png)
:::
::::::::::::::

## Права группы

:::::::::::::: {.columns align=center}
::: {.column width="50%"}
![Регистрация пользователя `guest2` в группе `guest`](images/05.png)
:::
::: {.column width="50%"}
![Изменения права директории `/home/guest` и `/home/guest/dir1`](images/06.png)
:::
::::::::::::::

## Эксперименты с правами доступа

:::::::::::::: {.columns align=center}
::: {.column width="50%"}
![Эксперименты с правами доступа (1)](images/07.png)
:::
::: {.column width="50%"}
![Эксперименты с правами доступа (2)](images/08.png)
:::
::::::::::::::

## Установленные права и разрешённые действия для групп

![Фрагмент таблицы «Установленные права и разрешённые действия для групп»](images/09.png)

## Минимальные права для совершения операций от имени пользователей входящих в группу

![Таблица «Минимальные права для совершения операций от имени пользователей входящих в группу»](images/10.png)

# Результаты

## Итог

В рамках лабораторной работы был создан новый пользовтель `guest2`, который был добавлен в группу пользователя `guest`. На примере данных пользователей мы разобрали базовые команды и права доступа для групп.

## {.standout}

Спасибо за внимание!
