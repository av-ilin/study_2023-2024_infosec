---
## Front matter
title: "Лабораторная работа №1"
subtitle: "Настройка рабочего окружения"
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

Настроить окружение для выполнения лабоораторных работ по дисциплине Информационная безопасность.

# Задачи

1. Установить дистрибутив Linux на базе RedHat: установить операционную систему Rocky на виртуальную машину, используя средства VirtualBox.

2. Настроить систему контроля версий - Git: создать репозиторий дисциплины, связать с локальной машиной, проинициализировать начальными значениями (добавить changelog, readme, gitignore, license).

# Теоретическое введение

## Термины

- Репозиторий (от англ. repository — хранилище) — место, где хранятся и поддерживаются какие-либо данные. Чаще всего данные в репозитории хранятся в виде файлов, доступных для дальнейшего распространения по сети. [@wiki-repo]

- Контроль версий - это способ сохранять изменения с течением времени, не перезаписывая предыдущие версии. [@git-guides]

- Распределенное ПО для контроля версий - каждый разработчик, работающий с репозиторием, имеет копию всего этого репозитория. [@git-guides]

## Окружение

- Rocky Linux - это корпоративная операционная система с открытым исходным кодом, разработанная таким образом, чтобы быть на 100% совместимой с Red Hat Enterprise Linux. Он находится в стадии интенсивной разработки сообществом. [@rocky-docs]

- Git - это распределенное программное обеспечение для контроля версиями. [@git-guides]

- VirtualBox - это кросс-платформенное ПО для виртуализации x86 и AMD64/Intel64 с открытым кодом для корпоративного и домашнего использования. [@vbox]


# Выполнение лабораторной работы

1. Начнем выполнения поставленных задач в Julia. Для этого запустим Pluto [@pluto-jl]. (рис. @fig:001, @fig:002)

![bbbb](images/01.png){#fig:001 width=86%}


# Анализ результатов

Работа выполненна без непредвиденных проблем в соответствии с руководством. Ошибок и сбоев не произошло. Rocky Linux (minimal) - отлично себя показал, крайне быстро запускается (перезапускается) и выполняет команды, блягодаря отсутствию GUI. Терминала было достаточно для выполнения заданий текущей лабораторной работы (возможно, будет достаточно и для последующих). В случае необходимости можно установить Rocky Server with GUI при помощи ввода минимального кол-ва команд.

```bash
dnf group list
dnf groupinstall "Server with GUI" -y
systemctl set-default graphical
reboot
```

# Выводы

Создана виртуальная машина с Rocky Linux (minimal), создан хост в соответствии с соглашением об именованиии. На локальную машину установлен VS Code, в котором будет происходить написание отчетов. Также был создан репозиторий git, который был приведен к необходимому начальному состоянию.

# Список литературы{.unnumbered}

::: {#refs}
:::

