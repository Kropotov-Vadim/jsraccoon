#ES6: операторы spread и rest

Новый оператор `...` называется spread (распростанение, расширение) или rest (остаток) в зависимости от того, где и как он используется. 

## Оператор spread
Начнём сразу с примера:

```javascript
var log = function(a, b, c) {
  console.log(a, b, c);
};

log(...['Spread', 'Rest', 'Operator']); // Spread Rest Operator
```
В данном примере переданный в функцию массив разделяется на три значения: `Spread`, `Rest` и `Operator`, после чего передаются функции `log`. Таким образом, оператор `...`, используемый перед массивом, или любой другой коллекцией значений (строки, объекты), называет *spread*.

Подобным образом использовать массив можно было и до появления оператора `...` с помощью метода функций `apply`:
```javascript
log.apply(null, [1, 2, 3]); // 1 2 3
// Равнозначно
log(...[1, 2, 3]); // 1 2 3
```

### Использование оператора spread
Использование оператора *spread* не ограничивается передачей параметров функции. Несколько примеров его *полезного* использования:

#### Клонирование свойств массивов
```javascript
var arr = ['will', 'love'];
var data = ['You', ...arr, 'spread', 'operator'];
console.log(data); // ['You', 'will', 'love', 'spread', 'operator']
```

Подобным образом можно скопировать и весь массив целиком
```javascript
var arr = [1, 2, 3, 4, 5];
var data = [...arr]; 
console.log(data); // [1, 2, 3, 4, 5]
```

Важно понимать, что при подобном использовании оператора `...` происходит именно копирование всех свойств, а не ссылки на массив. 
```javascript
var arr = [1, 2, 3, 4, 5];
var data = [...arr]; 
var copy = arr;

arr === data; // false − ссылки отличаются − два разных массива
arr === copy; // true − две переменные ссылаются на один массив
```

#### Преобразование коллекции DOM элементов
Раньше для преобразования коллекций в массивы приходилось использовать подобные конструкции:
```javascript
var links = document.querySelectorAll('a');
var linksArr = Array.slice.call(links);

// Или более короткий вариант
var linksArr = [].slice.call(document.links);

Array.isArray(links); // false
Array.isArray(linksArr); // true
```

Подобные преобразования очень распространены, но не могут похвастаться элегантностью и, тем более, очевидностью − если подобную конструкцию увидит разработчик, никогда не использовавший её самостоятельно, то его шансы понять происходящее стремятся к нулю.

Преобразование DOM коллекции в массив с помощью оператора *spread* выглядит следующим образом:
```javascript
var links = [...document.querySelectorAll('a')];
// Или просто
var links = [...document.links];

Array.isArray(links); // true
```

#### Замена apply для функций конструкторов
Проще всего продемонстрировать использование `...`, как замену `apply` для функций конструкторов, на примере `Date`. Обычно конструктор `Date` работает подобным образом:
```javascript
var birthday = new Date(1993, 3, 24); // 24 Апреля 1993 года
console.log(birthday); // "1993-04-23T21:00:00.000Z"
```

Всё работает хорошо до тех пор, пока нет необходимости передать массив `[1993, 3, 24]` в конструктор `Date`. В данной ситуации вам пришлось бы написать "костыль":
```javascript
// Никогда не используйте подобный код − он абсолютно безумен
new (Date.bind.apply(Date, [null].concat([1993, 3, 24]))); // 24 Апреля 1993 года
```

ES6 даёт возможность избежать подобных ситуаций:
```javascript
var day = [1993, 3, 24];
var birthday = new Date(...day);
```

## Оператор rest
В самом начале статьи я уже упоминал, что оператор `...` интерпретируется по-разному, в зависимости от контекста применения. *Spread* используется для разделения коллекций на отдельные элементы, а *rest*, наоборот, для соединения отдельных значений в массив.

```javascript
var log = function(a, b, ...rest) {
  console.log(a, b, rest);
};

log('Basic', 'rest', 'operator', 'usage'); // Basic rest ['operator', usage]
```

Используя параметр `...rest` в функции `log` вы говорите интерпретатору: "собери все *оставшиеся* элементы в массив с именем `rest`". Разумеется, если вы не передадите в функцию других именновах параметров, то `...` соберёт все аргументы:
```javascript
var log = function(...args) {
  conole.log(args);
};

log(1, 2, 3, 4, 5); // [1, 2, 3, 4, 5]
```

Таким образом, больше нет необходимости переводить псевдо-массив `arguments` функций в настоящий массив − оператор *rest* сделает это сам:
```javascript
// Раньше
var sum = function() {
  var args = [].slice.call(arguments);
  return args.reduce(function(s, num) {
    return s += num;
  }, 0);
};

// Теперь
var sum = function(..args) {
  return args.reduce(function(s, num) {
    return s + num;
  }, 0);
};

// Ещё короче с использованием стрелочных функций
var sum = (...args) => args.reduce((s, num) => s + num, 0);
```