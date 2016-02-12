---
title: "Молекулы"
categories: JavaScript
date: 2016-02-07 13:00:00 +0300
type: exercise
identifier: molecules

description: "Напишите функцию, которая по переданной ей химической формуле будет определять количесто содержание каждого атома в молекуле."
---

Напишите функцию `explode`, которая по переданной ей химической формуле будет определять количесто содержание каждого атома в молекуле. Функция возвращает объект, в котором каждый ключ соответствует атому, а значение — общему количеству данных атомов в молекуле.
{% highlight javascript %}
explode('H2O'); // {"H": 2, "O": 1}
explode('KMnO4'); // {"K": 1, "Mn": 1, "O": 4}
explode('Ca(OH)2'); // {"Ca": 1, "O": 2, "H": 2}
explode('K4[Fe(CN)6]'); // {"K": 4, "Fe": 1, "C": 6, "N": 6}
// Элементы могут повторяться
explode('(NH4)2HPO4'); // {"N": 2, "H": 9, "P": 1, "O": 4}
explode('FeOOH'); // {"Fe": 1, "O": 2, "H": 1}
{% endhighlight %}

## Решение
Будет скоро доступно