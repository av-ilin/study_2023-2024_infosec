---
## Front matter
title: "Лабораторная работа №3"
subtitle: "Дискреционное разграничение прав в Linux. Два пользователя"
author: "Ильин Андрей Владимирович"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: false # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
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
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы

Получение практических навыков работы в консоли с атрибутами файлов для групп пользователей. Закрепление теоретических основ дискреционного разграничения доступа в современных системах с открытым кодом на базе ОС Linux.

# Задачи

1. Создать пользователей guest и guest2. Добавить пользователя guest2 в группу guest.

2. Уточнить информацию о пользователях.

3. Заполнить таблицы "Установленные права и разрешённые действия для групп" и "Минимальные права для совершения операций от имени пользователей входящих в группу", меняя атрибуты у директории `dir1` и файла `file1` от имени пользователя `guest` и делая проверку от пользователя `guest2`.

# Теоретическое введение

## Термины

- Терминал (или «Bash», сокращение от «Bourne-Again shell») — это программа, которая используется для взаимодействия с командной оболочкой. Терминал применяется для выполнения административных задач, например: установку пакетов, действия с файлами и управление пользователями. [@terminal]

- Права доступа определяют, какие действия конкретный пользователь может или не может совершать с определенным файлами и каталогами. [@mode]

## Окружение

- Rocky Linux - это корпоративная операционная система с открытым исходным кодом, разработанная таким образом, чтобы быть на 100% совместимой с Red Hat Enterprise Linux. Он находится в стадии интенсивной разработки сообществом. [@rocky-docs]

- Git - это распределенное программное обеспечение для контроля версиями. [@git-guides]

- VirtualBox - это кросс-платформенное ПО для виртуализации x86 и AMD64/Intel64 с открытым кодом для корпоративного и домашнего использования. [@vbox]

# Выполнение лабораторной работы

1. Создадим учётную запись пользователя `guest2` (`guest1` был создан в предыдущей лабораторной работе). Зададим пароль для пользователя `guest2`. Добавим пользователя `guest2` в группу пользователя `guest`. (рис. @fig:001)

```bash
useradd guest
passwd guest
gpasswd -a guest2 guest
```

![Пользователь guest2](images/01.png){#fig:001 width=86%}

2. Осуществим вход в систему от двух пользователей на двух разных консолях: `guest` (`tty2`) и `guest2` (`tty3`). Определим директорию, в которой находимся. Уточним имя пользователя, используя `whoami`. Уточним имя, группу, а также группы, куда входит пользователь, командой `id`. Вспользуемся командой `groups`. Просмотрим файл `/etc/passwd`. (рис. @fig:002, @fig:003, @fig:004)

```bash
pwd
whoami
groups guest # guest2
id
id -G
id -Gn
```

![Изучение пользователя `guest1`](images/02.png){#fig:002 width=86%}

![Изучение пользователя `guest2`](images/03.png){#fig:003 width=86%}

```bash
cat /etc/passwd | grep guest
```

![Просмотр `cat /etc/passwd`](images/04.png){#fig:004 width=86%}

3. От имени пользователя `guest2` выполним регистрацию пользователя `guest2` в группе `guest` командой `newgrp guest`. От имени пользователя `guest` изменим права директории `/home/guest`, разрешив все действия для пользователей группы. От имени пользователя `guest` снимем с директории `/home/guest/dir1` все атрибуты. (рис. @fig:005, @fig:006)

```bash
newgrp guest
```

![Регистрация пользователя `guest2` в группе `guest`](images/05.png){#fig:005 width=86%}

```bash
chmod g+rwx /home/guest
ls -l /home
chmod 000 dirl
ls -l
```

![Изменения права директории `/home/guest` и `/home/guest/dir1`](images/06.png){#fig:006 width=86%}

5. Заполним таблицу «Установленные права и разрешённые действия для групп» [@mygit]. (рис. @fig:007, @fig:008, @fig:009)

![Эксперименты с правами доступа (1)](images/07.png){#fig:007 width=86%}

![Эксперименты с правами доступа (2)](images/08.png){#fig:008 width=86%}

![Фрагмент таблицы «Установленные права и разрешённые действия для групп»](images/09.png){#fig:009 width=86%}

6. Заполним таблицу «Минимальные права для совершения операций» [@mygit]. (рис. @fig:010)

![Таблица «Минимальные права для совершения операций от имени пользователей входящих в группу»](images/10.png){#fig:010 width=86%}

# Анализ результатов

Работа выполненна без непредвиденных проблем в соответствии с руководством. Ошибок и сбоев не произошло.

# Выводы

В рамках лабораторной работы был создан новый пользовтель `guest2`, который был добавлен в группу пользователя `guest`. На примере данных пользователей мы разобрали базовые команды и права доступа для групп.

# Список литературы{.unnumbered}

::: {#refs}
:::
