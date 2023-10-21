---
## Front matter
title: "Лабораторная работа №7"
subtitle: "Элементы криптографии. Однократное гаммирование"
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

Освоить на практике применение режима однократного гаммирования1

# Задачи

Требуется разработать приложение, позволяющее шифровать и дешифровать данные в режиме однократного гаммирования. Приложение должно:

1. Определить вид шифротекста при известном ключе и известном открытом тексте.
2. Определить ключ, с помощью которого шифротекст может быть преобразован в некоторый фрагмент текста, представляющий собой один из возможных вариантов прочтения открытого текста.

# Теоретическое введение

Гаммирование представляет собой наложение (снятие) на открытые (зашифрованные) данные последовательности элементов других данных, полученной с помощью некоторого криптографического алгоритма, для получения зашифрованных (открытых) данных. Иными словами, наложение гаммы — это сложение её элементов с элементами открытого (закрытого) текста по некоторому фиксированному модулю, значение которого представляет собой известную часть алгоритма шифрования. [@rudn-task]

В соответствии с теорией криптоанализа, если в методе шифрования используется однократная вероятностная гамма (однократное гаммирование) той же длины, что и подлежащий сокрытию текст, то текст нельзя раскрыть. Даже при раскрытии части последовательности гаммы нельзя получить информацию о всём скрываемом тексте.

Наложение гаммы по сути представляет собой выполнение операции сложения по модулю 2 (XOR).

# Выполнение лабораторной работы

1. Для выполнения лабораторной работы восопользуемся открытым ресурсов Google Colab. Создадим новый ноутбук - в нем будем выполнять лабораторную работу. Реализуем класс `Gumming`, в нем будут следующие методы: `xor` и `__xor` - выполняет операцию XOR к двум строкам шестнадцатиричного кода (разделленного пробелами), `to_hex` - метод конвертации обычной строки к строке шестнадцатиричного кода, `from_hex` - метод для конвертации строки шестнадцатиричного кода к обычной строке. (рис. @fig:001)

```python
class Gumming:
    def xor(self, hex_seq1, hex_seq2):
        hex1 = hex_seq1.split()
        hex2 = hex_seq2.split()
        return ' '.join([self.__xor(hex1, hex2) for hex1, hex2 in zip(hex1, hex2)])

    def to_hex(self, msg):
        msg_hex = []
        for char in msg:
            char_cp1251 = char.encode('cp1251')
            char_code = int.from_bytes(char_cp1251, 'little')
            char_hex = hex(char_code)[-2:].upper()
            msg_hex.append(char_hex)
        return ' '.join(msg_hex)

    def from_hex(self, msg_hex):
        msg = ''
        for char_hex in msg_hex.split():
            char_code = int(char_hex, 16)
            char_cp1251 = char_code.to_bytes(1, 'little')
            char = char_cp1251.decode('cp1251')
            msg += char
        return msg

    def __xor(self, sym1, sym2):
        xor = lambda x, y: bytes(a^b for a, b in zip(x, y))
        b_sym1 = bytes.fromhex(sym1)
        b_sym2 = bytes.fromhex(sym2)
        r_result = xor(b_sym1, b_sym2)
        result = r_result.hex().upper()
        return result

gumming = Gumming()
```

![Класс Gumming](images/01.png){#fig:001 width=86%}

2. Проведем эксперимент "Центр и Мюллер", приведенный в укзаниях к лабораторной работе, используя написанный класс. (рис. @fig:002)

```python
src_msg = 'Штирлиц – Вы Герой!!'
hex_msg = gumming.to_hex(src_msg)
src_key = '05 0C 17 7F 0E 4E 37 D2 94 10 09 2E 22 57 FF C8 0B B2 70 54'
enc_msg = gumming.xor(hex_msg, src_key)

mul_key = '05 0C 17 7F 0E 4E 37 D2 94 10 09 2E 22 55 F4 D3 07 BB BC 54'
mul_res = gumming.xor(enc_msg, mul_key)
mul_msg = gumming.from_hex(mul_res)

print(src_msg, '<-- Сообщение Центра')
print(hex_msg, '<-- Сообщение Центра (16)')
print(src_key, '<-- Ключ Центра')
print(enc_msg, '<-- Закодированное cообщение Центра')
print(mul_key, '<-- Ключ Мюллера')
print(mul_res, '<-- Сообщение Мюллера (16)')
print(mul_msg, '<-- Сообщение Мюллера')
```

![Центр и Мюллер](images/02.png){#fig:002 width=86%}

3. Определим ключ, с помощью которого шифротекст может быть преобразован в некоторый фрагмент текста (`С Новым Годом,друзья!`), (рис. @fig:003)

```python
ng_msg = 'С Новым Годом,друзья!'
ng_hex = gumming.to_hex(ng_msg)
ng_key = gumming.xor(enc_msg, ng_hex)
ng_res = gumming.from_hex(gumming.xor(enc_msg, ng_key))

print(ng_msg, '<-- Необходимое сообщение')
print(ng_hex, '<-- Необходимое сообщение (16)')
print(enc_msg, '<-- Закодированное cообщение Центра')
print(ng_key, '<-- Искомый ключ')
print(ng_res, '<-- Необходимое сообщение из cообщения Центра')
```

![Ключ для `С Новым Годом,друзья!`](images/03.png){#fig:003 width=86%}

# Анализ результатов

Работа выполненна без непредвиденных проблем в соответствии с руководством. Ошибок и сбоев не произошло.

# Выводы

Нам удалось освоить на практике применение режима однократного гаммирования, в дополнение закрпеили навки владения языками программирования, в частности языком программирования - `python`.

# Список литературы{.unnumbered}

::: {#refs}
:::
