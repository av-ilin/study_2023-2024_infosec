---
## Front matter
title: "Лабораторная работа №4"
subtitle: "Дискреционное разграничение прав в Linux. Расширенные атрибуты"
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

Получение практических навыков работы в консоли с расширенными атрибутами файлов. Закрепление теоретических основ дискреционного разграничения доступа в современных системах с открытым кодом на базе ОС Linux.

# Задачи

1. Провести ряд операций над файлом с правами `600` и аттрибутом `a`.

2. Провести ряд операций над файлом с правами `600` и аттрибутом `i`.

# Теоретическое введение

## Термины

- Терминал (или «Bash», сокращение от «Bourne-Again shell») — это программа, которая используется для взаимодействия с командной оболочкой. Терминал применяется для выполнения административных задач, например: установку пакетов, действия с файлами и управление пользователями. [@terminal]

- Права доступа определяют, какие действия конкретный пользователь может или не может совершать с определенным файлами и каталогами. [@mode]

- Расширенные атрибуты файловых объектов (далее - расширенные атрибуты) - поддерживаемая некоторыми файловыми системами возможность ассоциировать с файловыми объектами произвольные метаданные. [@attr]

## Окружение

- Rocky Linux - это корпоративная операционная система с открытым исходным кодом, разработанная таким образом, чтобы быть на 100% совместимой с Red Hat Enterprise Linux. Он находится в стадии интенсивной разработки сообществом. [@rocky-docs]

- Git - это распределенное программное обеспечение для контроля версиями. [@git-guides]

- VirtualBox - это кросс-платформенное ПО для виртуализации x86 и AMD64/Intel64 с открытым кодом для корпоративного и домашнего использования. [@vbox]

# Выполнение лабораторной работы

1. От имени пользователя `guest` определим расширенные атрибуты файла `file1` и установим на файл `file1` права, разрешающие чтение и запись для владельца файла. Попробуем установить расширенный атрибут `a`. (рис. @fig:001)

```bash
# guest
lsattr /home/guest/dir1/file1
chmod 600 /home/guest/dir1/file1
chattr +a /home/guest/dir1/file1
```

2. Установим расширенный атрибут `a` на файл `file1` от имени `суперпользователя`. От пользователя `guest`: проверим правильность установления атрибута; выполним дозапись слова «test»; убедимся, что слово "test" было успешно записано; попробуем стереть в нём информацию; попробуем установить права `000`. После этого снимем расширенный атрибут `a` с файла от имени `суперпользователя`. (рис. @fig:001)

```bash
# root
chattr +a /home/guest/dir1/file1
```

```bash
# guest
lsattr /home/guest/dir1/file1
echo "test" >> /home/guest/dir1/file1
cat /home/guest/dir1/file1
echo "abcd" > /home/guest/dirl/file1
chmod 000 file1
```

```bash
# root
chattr -a /home/guest/dir1/file1
```

![Исследование аттрибута `a`](images/01.png){#fig:001 width=86%}

3. Установим расширенный атрибут `i` на файл `file1` от имени `суперпользователя`. От пользователя `guest`: проверим правильность установления атрибута; выполним дозапись слова «test»; посмотрим записано ли слово "test" второй раз; попробуем стереть в нём информацию; попробуем установить права `000`. После этого снимем расширенный атрибут `i` с файла от имени `суперпользователя`. (рис. @fig:002)

```bash
# root
chattr +i /home/guest/dir1/file1
```

```bash
# guest
lsattr /home/guest/dir1/file1
echo "test" >> /home/guest/dir1/file1
cat /home/guest/dir1/file1
echo "abcd" > /home/guest/dirl/file1
chmod 000 file1
```

```bash
# root
chattr -i /home/guest/dir1/file1
```

![Исследование аттрибута `i`](images/02.png){#fig:002 width=86%}

# Анализ результатов

Работа выполненна без непредвиденных проблем в соответствии с руководством. Ошибок и сбоев не произошло.

# Выводы

В результате выполнения работы мы повысили свои навыки использования интерфейса командой строки (CLI), познакомились на примерах с тем, как используются основные и расширенные атрибуты при разграничении доступа. Опробовали действие на практике расширенных атрибутов «а» и «i».

# Список литературы{.unnumbered}

::: {#refs}
:::
