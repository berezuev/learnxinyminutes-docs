---
language: forth
contributors:
    - ["Horse M.D.", "http://github.com/HorseMD/"]
translators:
    - ["Dmitrii Kuznetsov", "https://github.com/torgeek"]
filename: learnforth-ru.fs
lang: ru-ru
---

Форт создан Чарлзом Муром в 70-е годы. Это императивный, стековый язык программирования и среда исполнения программ. Использовался в таких проектах как Open Firmware. Продолжает применятся в проектах. Применяется в НАСА.

Внимание: этот материал использует реализацию Форта - Gforth, но большая часть написанного будет работать в других средах.


```
\ Это комментарий
( Это тоже комментарий, но используется для предоределённых слов )

\ --------------------------------- Прекурсор --------------------------------

\ Всё программирование на Форте заключается в манипулировании 
\ параметрами на стеке.
5 2 3 56 76 23 65    \ ok

\ Эти числа добавляются в стек слева направо
.s    \ <7> 5 2 3 56 76 23 65 ok

\ В Форте всё - это слова-команды или числа. Слова разделяются любым числом
\ пробелов и переходов на новую строку. Длина слова не больше 31 литеры.

\ ---------------------------- Базовая арифметика ----------------------------

\ Арифметика (фактически все ключевые слова требуют данных) - это манипуляция 
\ данными на стеке.
5 4 +    \ ok

\ `.` показывает верхнее значение в стеке:
.    \ 9 ok

\ Ещё примеры арифметических выражений:
6 7 * .        \ 42 ok
1360 23 - .    \ 1337 ok
12 12 / .      \ 1 ok
13 2 mod .     \ 1 ok

99 negate .    \ -99 ok
-99 abs .      \ 99 ok
52 23 max .    \ 52 ok
52 23 min .    \ 23 ok

\ --------------------------- Манипуляции со стеком ---------------------------

\ Естественно, когда мы работаем со стеком, то используем 
\ больше полезных методов:

3 dup -          \ дублировать верхний элемент в стеке 
                 \ (1-й становится эквивалентным 2-му): 3 - 3
2 5 swap /       \ поменять местами верхний элемент со 2-м элементом: 5 / 2
6 4 5 rot .s     \ сменять по очереди 3-и верхних элемента: 4 5 6
4 0 drop 2 /     \ снять верхний элемент (не печатается на экране): 4 / 2
1 2 3 nip .s     \ снять второй элемент (подобно исключению элемента):   1 3

\ ------------------ Более продвинутые манипуляции со стеком ------------------

1 2 3 4 tuck   \ дублировать верхний елемент стека во вторую позицию: 
               \ 1 2 4 3 4 ok
1 2 3 4 over   \ диблировать второй елемент наверх стека: 
               \ 1 2 3 4 3 ok
1 2 3 4 2 roll \ *переместить* элемент в заданной позиции наверх стека:
               \ 1 3 4 2 ok
1 2 3 4 2 pick \ *дублировать* элемент в заданной позиции наверх: 
               \ 1 2 3 4 2 ok

\ Внимание! Обращения к стеку индексируются с нуля.

\ --------------------------- Создание новых слов -----------------------------

\ Определение новых слов через уже известные. Двоеточие `:` переводит Форт 
\ в режим компиляции выражения, которое заканчивается точкой с запятой `;`.
: square ( n -- n ) dup * ;    \ ok
5 square .                     \ 25 ok

\ Мы всегда можем посмотреть, что содержится в слове:
see square     \ : square dup * ; ok

\ -------------------------------- Зависимости --------------------------------

\ -1 == true, 0 == false. Однако, некоторые ненулевые значения 
\ обрабатываются как true:
42 42 =    \ -1 ok
12 53 =    \ 0 ok

\ `if` это компилируемое слово. `if` <stuff to do> `then` <rest of program>.
: ?>64 ( n -- n ) dup 64 > if ." Больше чем 64!" then ;   
\ ok
100 ?>64                                                  
\ Больше чем 64! ok

\ Else:
: ?>64 ( n -- n ) dup 64 > if ." Больше чем 64!" else ." меньше чем 64!" then ;
100 ?>64    \ Больше чем 64! ok
20 ?>64     \ меньше чем 64! ok

\ ------------------------------------ Циклы -----------------------------------

\ `do` это тоже компилируемое слово.
: myloop ( -- ) 5 0 do cr ." Hello!" loop ; \ ok
myloop
\ Hello!
\ Hello!
\ Hello!
\ Hello!
\ Hello! ok

\ `do` предполагает наличие двух чисел на стеке: конечное и начальное число.

\ Мы можем назначить в цикле переменную `i` для значения индекса:
: one-to-12 ( -- ) 12 0 do i . loop ;     \ ok
one-to-12                                 \ 0 1 2 3 4 5 6 7 8 9 10 11 12 ok

\ `?do` работает подобным образом, за исключением пропуска начального 
\ и конечного значения индекса цикла.
: squares ( n -- ) 0 ?do i square . loop ;   \ ok
10 squares                                   \ 0 1 4 9 16 25 36 49 64 81 ok

\ Изменение "шага" цикла проиводится командой `+loop`:
: threes ( n n -- ) ?do i . 3 +loop ;    \ ok
15 0 threes                             \ 0 3 6 9 12 ok

\ Запуск бесконечного цикла - `begin` <stuff to do> <flag> `until`:
: death ( -- ) begin ." Вы всё ещё здесь?" 0 until ;    \ ok

\ ---------------------------- Переменные и память ----------------------------

\ Используйте `variable`, что бы объявить `age` в качестве переменной.
variable age    \ ok

\ Затем мы запишем число 21 в переменную 'age' (возраст) словом `!`.
21 age !    \ ok

\ В заключении мы можем напечатать значение переменной прочитав его словом `@`, 
\ которое добавит значение на стек или использовать слово `?`, 
\ что бы прочитать и распечатать в одно действие.
age @ .    \ 21 ok
age ?      \ 21 ok

\ Константы объявляются аналогично, за исключем того, что мы не должны 
\ беспокоиться о выделении адреса в памяти:
100 constant WATER-BOILING-POINT    \ ok
WATER-BOILING-POINT .               \ 100 ok

\ ---------------------------------- Массивы ----------------------------------

\ Создание массива похоже на объявление переменной, но нам нужно выделить
\ больше памяти.

\ Вы можете использовать слова `2 cells allot` для создания массива 
\ размером 3 элемента:
variable mynumbers 2 cells allot    \ ok

\ Инициализировать все значения в 0
mynumbers 3 cells erase    \ ok

\ В качестве альтернативы мы можем использовать `fill`:
mynumbers 3 cells 0 fill

\ или мы можем пропустить все слова выше и инициализировать массив 
\ нужными значениями:
create mynumbers 64 , 9001 , 1337 , \ ok (the last `,` is important!)

\ ... что эквивалентно:

\ Ручная запись значений по индексам ячеек:
64 mynumbers 0 cells + !      \ ok
9001 mynumbers 1 cells + !    \ ok
1337 mynumbers 2 cells + !    \ ok

\ Чтение значений по индексу:
0 cells mynumbers + ?    \ 64 ok
1 cells mynumbers + ?    \ 9001 ok

\ Мы можем просто сделать собственное слово для манипуляции массивом:
: of-arr ( n n -- n ) cells + ;    \ ok
mynumbers 2 of-arr ?               \ 1337 ok

\ Которую тоже можно использовать для записи значений:
20 mynumbers 1 of-arr !    \ ok
mynumbers 1 of-arr ?       \ 20 ok

\ ------------------------------ Стек возвратов ------------------------------

\ Стек возвратов используется для удержания ссылки,
\ когда одно слово запускает другое, например, в цикле.

\ Мы всегда видим это, когда используем `i`, которая возвращает дубль верхнего
\ значения стека. `i` это эквивалент `r@`.
: myloop ( -- ) 5 0 do r@ . loop ;    \ ok

\ Так же как при чтении мы можем добавить ссылку в стек возвратов и удалить её:
5 6 4 >r swap r> .s    \ 6 5 4 ok

\ Внимание: так как Форт использует стек возвратов для указателей на слово `>r`
\ следует всегда пользоваться `r>`.

\ ---------------- Операции над числами с плавающей точкой --------------------

\ Многие фортовцы стараются избегать использование слов с вещественными числами.
8.3e 0.8e f+ f.    \ 9.1 ok

\ Обычно мы просто используем слово 'f', когда обращаемся к вещественным числам:
variable myfloatingvar    \ ok
4.4e myfloatingvar f!     \ ok
myfloatingvar f@ f.       \ 4.4 ok

\ ---------- В завершение несколько полезных замечаний и слов -----------------

\ Указание несуществующего слова очистит стек. Тем не менее, есть специальное 
\ слово для этого:
clearstack

\ Очистка экрана:
page

\ Загрузка форт-файла:
\ s" forthfile.fs" included

\ Вы можете вывести список всех слов словаря Форта (это большой список!):
words

\ Выход из Gforth:
bye

```

##Готовы к большему?

* [Начала Форта (англ.)](http://www.forth.com/starting-forth/)
* [Простой Форт (англ.)](http://www.murphywong.net/hello/simple.htm)
* [Мышление Форта (англ.)](http://thinking-forth.sourceforge.net/)
* [Учебники Форта (рус.)](http://wiki.forth.org.ru/УчебникиФорта)
