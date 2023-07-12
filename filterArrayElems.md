# Filter Array elements

## find и findIndex/findLastIndex

Представьте, что у нас есть массив объектов. Как нам найти объект с определённым условием?

Здесь пригодится метод [arr.find](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/find).

Синтаксис:

```js
let result = arr.find(function(item, index, array) {
  // если true - возвращается текущий элемент и перебор прерывается
  // если все итерации оказались ложными, возвращается undefined
});
```

Функция вызывается по очереди для каждого элемента массива:

- `item` - очередной элемент.

- `index` - его индекс.

- `array` - сам массив.

Если функция возвращает `true`, поиск прерывается и возвращается `item`. Если ничего не найдено, возвращается `undefined`.

У метода [arr.findIndex](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex) такой же синтаксис, но он возвращает индекс, на котором был найден элемент, а не сам элемент. Значение `-1` возвращается, если ничего не найдено.

Метод [arr.findLastIndex](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/findLastIndex) похож на `findIndex`, но ищет справа налево, наподобие `lastIndexOf`.

Например:

```js run
let users = [
  {id: 1, name: "Вася"},
  {id: 2, name: "Петя"},
  {id: 3, name: "Маша"},
  {id: 4, name: "Вася"}
];

// Найти индекс первого Васи
alert(users.findIndex(user => user.name == 'Вася')); // 0

// Найти индекс последнего Васи
alert(users.findLastIndex(user => user.name == 'Вася')); // 3
```

### filter

Метод `find` ищет один (первый) элемент, который заставит функцию вернуть `true`.

Если найденных элементов может быть много, можно использовать [arr.filter(fn)](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/filter).

Синтаксис схож с `find`, но `filter` возвращает массив из всех подходящих элементов:

```js
let results = arr.filter(function(item, index, array) {
  // если `true` -- элемент добавляется к results и перебор продолжается
  // возвращается пустой массив в случае, если ничего не найдено
});
```

Например:

```js run
let users = [
  {id: 1, name: "Вася"},
  {id: 2, name: "Петя"},
  {id: 3, name: "Маша"}
];

// возвращает массив, состоящий из двух первых пользователей
let someUsers = users.filter(item => item.id < 3);

alert(someUsers.length); // 2
```
