# How to iterate Array elements

## Перебор: forEach

Метод [arr.forEach](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) позволяет запускать функцию для каждого элемента массива.

Синтаксис:

```js
arr.forEach(function(item, index, array) {
  // ... делать что-то с item
});
```

Результат функции (если она что-то возвращает) отбрасывается и игнорируется.

## split и join

Метод [str.split(delim)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split) именно это и делает. Он разбивает строку на массив по заданному разделителю `delim`.

У метода `split` есть необязательный второй числовой аргумент -- ограничение на количество элементов в массиве. Если их больше, чем указано, то остаток массива будет отброшен. На практике это редко используется:

```js
let arr = 'Вася, Петя, Маша, Саша'.split(', ', 2);

alert(arr); // Вася, Петя
```

Вызов `split(s)` с пустым аргументом `s` разбил бы строку на массив букв.

Вызов [arr.join(glue)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join) делает в точности противоположное `split`. Он создаёт строку из элементов `arr`, вставляя `glue` между ними.

```js
let arr = ['Вася', 'Петя', 'Маша'];

let str = arr.join(';'); // объединить массив в строку через ;

alert( str ); // Вася;Петя;Маша
```

## reduce/reduceRight

Когда нам нужно перебрать массив -- мы можем использовать `forEach`, `for` или `for..of`.

Когда нам нужно перебрать массив и вернуть данные для каждого элемента -- мы можем использовать `map`.

Методы [arr.reduce](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce) и [arr.reduceRight](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduceRight) похожи на методы выше, но они немного сложнее. Они используются для вычисления единого значения на основе всего массива.

Синтаксис:

```js
let value = arr.reduce(function(accumulator, item, index, array) {
  // ...
}, [initial]);
```

Функция применяется по очереди ко всем элементам массива и «переносит» свой результат на следующий вызов.

Аргументы:

- `accumulator` -- результат предыдущего вызова этой функции, равен `initial` при первом вызове (если передан `initial`),

- `item` -- очередной элемент массива,

- `index` -- его позиция,

- `array` -- сам массив.

При вызове функции результат её предыдущего вызова передаётся на следующий вызов в качестве первого аргумента.

Так, первый аргумент является по сути аккумулятором, который хранит объединённый результат всех предыдущих вызовов функции. По окончании он становится результатом `reduce`.

```js
let arr = [1, 2, 3, 4, 5];

let result = arr.reduce((sum, current) => sum + current, 0);

alert(result); // 15
```

При отсутствии `initial` в качестве первого значения берётся первый элемент массива, а перебор стартует со второго.

Но такое использование требует крайней осторожности. Если массив пуст, то вызов `reduce` без начального значения выдаст ошибку. Поэтому рекомендуется всегда указывать начальное значение.

Метод [arr.reduceRight](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduceRight) работает аналогично, но проходит по массиву справа налево.
