# How to flatten nested array

[arr.flat(depth)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/flat)/[arr.flatMap(fn)](mdn:js/Array/flatMap) создаёт новый плоский массив из многомерного массива.

Метод flat() возвращает новый массив, в котором все элементы вложенных подмассивов были рекурсивно "подняты" на указанный уровень depth.

Синтаксис:

```js
var newArray = arr.flat(depth);
```

Пример:

```js
var arr4 = [1, 2, [3, 4, [5, 6, [7, 8, [9, 10]]]]];
arr4.flat(Infinity);
// [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

Метод flatMap() сначала применяет функцию к каждому элементу, а затем преобразует полученный результат в плоскую структуру и помещает в новый массив. Это идентично map функции, с последующим применением функции flat с параметром depth ( глубина ) равным 1, но flatMap часто бывает полезным, так как работает немного более эффективно.

Синтаксис:

```js
var new_array = arr.flatMap(function callback(currentValue[, index[, array]]) {
    // возвращает элемент для new_array
}[, thisArg])
```
