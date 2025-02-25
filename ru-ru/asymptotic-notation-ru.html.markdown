---
category: Algorithms & Data Structures
name: Asymptotic Notation
contributors:
    - ["Jake Prather", "http://github.com/JakeHP"]
    - ["Divay Prakash", "http://github.com/divayprakash"]
translators:
    - ["pru-mike", "http://github.com/pru-mike"]
lang: ru-ru
---

# О-символика

## Что это такое?

О-символика, или асимптотическая запись, — это система символов, позволяющая
оценить время выполнения алгоритма, устанавливая зависимость времени выполнения
от увеличения объёма входных данных. Она также известна как оценка
сложности алгоритмов. Станет ли алгоритм невероятно медленным, когда
объём входных данных увеличится? Будет ли алгоритм выполняться достаточно быстро,
если объём входных данных возрастёт? О-символика позволяет ответить на эти
вопросы.

## Можно ли по-другому найти ответы на эти вопросы?

Один способ — это подсчитать число элементарных операций в зависимости от
различных объёмов входных данных. Хотя это и приемлемое решение, тот объём
работы, которого оно потребует, даже для простых алгоритмов делает его 
использование неоправданным.

Другой способ — это измерить, какое время алгоритм потребует для завершения на
различных объёмах входных данных. В то же время, точность и относительность
этого метода (полученное время будет относиться только к той машине, на которой 
оно вычислено) зависит от среды выполнения: компьютерного аппаратного
обеспечения, мощности процессора и т.д.

## Виды О-символики

В первом разделе этого документа мы определили, что О-символика
позволяет оценивать алгоритмы в зависимости от изменения размера входных
данных. Представим, что алгоритм — это функция f, n — размер входных данных и 
f(n) — время выполнения. Тогда для данного алгоритма f с размером входных
данных n получим какое-то результирующее время выполнения f(n).
Из этого можно построить график, где ось y — время выполнения, ось x — размер входных
данных, а точки на графике — это время выполнения для заданного размера входных
данных.

С помощью О-символики можно оценить функцию или алгоритм
несколькими различными способами. Например, можно оценить алгоритм исходя
из нижней оценки, верхней оценки, тождественной оценки. Чаще всего встречается
анализ на основе верхней оценки. Как правило, не используется нижняя оценка,
потому что она не подходит под планируемые условия. Отличный пример — алгоритмы
сортировки, особенно добавление элементов в древовидную структуру. Нижняя оценка
большинства таких алгоритмов может быть дана как одна операция. В то время как в
большинстве случаев добавляемые элементы должны быть отсортированы
соответствующим образом при помощи дерева, что может потребовать обхода целой
ветви. Это и есть худший случай, для которого планируется верхняя оценка.

### Виды функций, пределы и упрощения

```
Логарифмическая функция — log n
Линейная функция — an + b
Квадратичная функция — an^2 + bn +c
Степенная функция — an^z + . . . + an^2 + a*n^1 + a*n^0, где z — константа
Показательная функция — a^n, где a — константа
```

Приведены несколько базовых функций, используемых при определении сложности в
различных оценках. Список начинается с самой медленно возрастающей функции
(логарифм, наиболее быстрое время выполнения) и следует до самой быстро
возрастающей функции (экспонента, самое медленное время выполнения). Отметим,
что в то время, как «n», или размер входных данных, возрастает в каждой из этих функций,
результат намного быстрее возрастает в квадратичной, степенной
и показательной по сравнению с логарифмической и линейной.

Крайне важно понимать, что при использовании описанной далее нотации необходимо
использовать упрощённые выражения.
Это означает, что необходимо отбрасывать константы и слагаемые младших порядков,
потому что если размер входных данных (n в функции f(n) нашего примера)
увеличивается до бесконечности (в пределе), тогда слагаемые младших порядков
и константы становятся пренебрежительно малыми. Таким образом, если есть
константа, например, размера 2^9001 или любого другого невообразимого размера,
надо понимать, что её упрощение внесёт значительные искажения в точность
оценки.

Т.к. нам нужны упрощённые выражения, немного скорректируем нашу таблицу...

```
Логарифм — log n
Линейная функция — n
Квадратичная функция — n^2
Степенная функция — n^z, где z — константа
Показательная функция — a^n, где a — константа
```

### О Большое
О Большое, записывается как **О**, — это асимптотическая запись для оценки худшего
случая, или для ограничения заданной функции сверху. Это позволяет сделать 
_**асимптотическую оценку верхней границы**_ скорости роста времени выполнения 
алгоритма. Пусть `f(n)` — время выполнения алгоритма, а `g(n)` — заданная временная
сложность, которая проверяется для алгоритма. Тогда `f(n)` — это O(g(n)), если
существуют действительные константы c (c > 0) и n<sub>0</sub>, такие,
что `f(n)` <= `c g(n)` выполняется для всех n, начиная с некоторого n<sub>0</sub> (n > n<sub>0</sub>).

*Пример 1*

```
f(n) = 3log n + 100
g(n) = log n
```

Является ли `f(n)` O(g(n))?
Является ли `3 log n + 100` O(log n)?
Посмотрим на определение О Большого:

```
3log n + 100 <= c * log n
```

Существуют ли константы c и n<sub>0</sub>, такие, что выражение верно для всех n > n<sub>0</sub>?

```
3log n + 100 <= 150 * log n, n > 2 (не определенно для n = 1)
```

Да! По определению О Большого `f(n)` является O(g(n)).

*Пример 2*

```
f(n) = 3 * n^2
g(n) = n
```

Является ли `f(n)` O(g(n))?
Является ли `3 * n^2` O(n)?
Посмотрим на определение О Большого:

```
3 * n^2 <= c * n
```

Существуют ли константы c и n<sub>0</sub>, такие, что выражение верно для всех n > n<sub>0</sub>?
Нет, не существуют. `f(n)` НЕ ЯВЛЯЕТСЯ O(g(n)).

### Омега Большое
Омега Большое, записывается как **Ω**, — это асимптотическая запись для оценки
лучшего случая, или для ограничения заданной функции снизу. Это позволяет сделать
_**асимптотическую оценку нижней границы**_ скорости роста времени выполнения 
алгоритма. 

`f(n)` является Ω(g(n)), если существуют действительные константы
c (c > 0) и n<sub>0</sub> (n<sub>0</sub> > 0), такие, что `f(n)` >= `c g(n)` для всех n > n<sub>0</sub>.

### Примечание

Асимптотические оценки, сделанные при помощи О Большого и Омега Большого, могут
как являться, так и не являться точными. Для того чтобы обозначить, что границы не
являются асимптотически точными, используются записи О Малое и Омега Малое.

### О Малое
O Малое, записывается как **о**, — это асимптотическая запись для оценки верхней
границы времени выполнения алгоритма при условии, что граница не является
асимптотически точной.

`f(n)` является o(g(n)), если можно подобрать такие действительные константы,
что для всех c (c > 0) найдётся n<sub>0</sub> (n<sub>0</sub> > 0), так
что `f(n)` < `c g(n)` выполняется для всех n (n > n<sub>0</sub>).

Определения О-символики для О Большого и О Малого похожи. Главное отличие в том,
что если f(n) = O(g(n)), тогда условие f(n) <= c g(n) выполняется, если _**существует**_
константа c > 0, но если f(n) = o(g(n)), тогда условие f(n) < c g(n) выполняется
для _**всех**_ констант c > 0.

### Омега Малое
Омега Малое, записывается как **ω**, — это асимптотическая запись для оценки
верхней границы времени выполнения алгоритма при условии, что граница не является
асимптотически точной.

`f(n)` является ω(g(n)), если можно подобрать такие действительные константы,
что для всех c (c > 0) найдётся n<sub>0</sub> (n<sub>0</sub> > 0), так
что `f(n)` > `c g(n)` выполняется для всех n (n > n<sub>0</sub>).

Определения Ω-символики и ω-символики похожи. Главное отличие в том, что
если f(n) = Ω(g(n)), тогда условие f(n) >= c g(n) выполняется, если _**существует**_
константа c > 0, но если f(n) = ω(g(n)), тогда условие f(n) > c g(n)
выполняется для _**всех**_ констант c > 0.

### Тета
Тета, записывается как **Θ**, — это асимптотическая запись для оценки
_***асимптотически точной границы***_ времени выполнения алгоритма.

`f(n)` является Θ(g(n)), если для некоторых действительных
констант c1, c2 и n<sub>0</sub> (c1 > 0, c2 > 0, n<sub>0</sub> > 0)
`c1 g(n)` < `f(n)` < `c2 g(n)` для всех n (n > n<sub>0</sub>).

∴ `f(n)` является Θ(g(n)) означает, что `f(n)` является O(g(n))
и `f(n)` является Ω(g(n)).

О Большое — основной инструмент для анализа сложности алгоритмов.
Также см. примеры по ссылкам.

### Заключение
Такую тему сложно изложить кратко, поэтому обязательно стоит пройти по ссылкам и
посмотреть дополнительную литературу. В ней даётся более глубокое описание с
определениями и примерами.


## Дополнительная литература

* [Алгоритмы на Java](https://www.ozon.ru/context/detail/id/18319699/)
* [Алгоритмы. Построение и анализ](https://www.ozon.ru/context/detail/id/33769775/)

## Ссылки

* [Оценки времени исполнения. Символ O()](http://algolist.manual.ru/misc/o_n.php)
* [Асимптотический анализ и теория вероятностей](https://www.lektorium.tv/course/22903)

## Ссылки (англ.)

* [Algorithms, Part I](https://www.coursera.org/learn/algorithms-part1)
* [Cheatsheet 1](http://web.mit.edu/broder/Public/asymptotics-cheatsheet.pdf)
* [Cheatsheet 2](http://bigocheatsheet.com/)

