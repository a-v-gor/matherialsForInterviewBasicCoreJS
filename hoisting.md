# Поднятие (hoisting)

Поднятие, или hoisting — это механизм в JavaScript, в котором переменные и объявления функций, передвигаются вверх своей области видимости перед тем, как код будет выполнен.

Как следствие, это означает то, что совершенно неважно где были объявлены функция или переменные, все они передвигаются вверх своей области видимости, вне зависимости от того локальная она или же глобальная.

Стоит отметить то, что механизм "поднятия" передвигает только объявления функции или переменной. Назначения переменным остаются на своих местах.

## Undefined vs ReferenceError

В JavaScript необъявленной переменной при выполнении кода назначается значение undefined, а также и тип undefined.

В JavaScript при попытке доступа к предварительно необъявленной переменной появляется ReferenceError.

## «Поднятие» переменных

В JavaScript мы можем объявлять и инициализировать наши переменные одновременно. **JavaScript непреклонно сначала объявляет, а уже затем инициализирует наши переменные.**

Все переменные и объявления функций поднимаются вверх своей области видимости. Объявление переменных происходит перед выполнением кода.

Однако необъявленные переменные не существуют до тех пор, пока код, назначающий их, не будет выполнен. Следовательно, указание значения для необъявленной переменной тут же создаёт её как глобальную переменную, когда назначение будет выполнено. Это говорит о том, что все необъявленные переменные это по факту глобальные переменные.

## ES5

### var

Областью видимости переменной, объявленной через var, является её настоящий контекст выполнения. Это подходит как и для замыкания функции, так и для переменных, объявленных вне любой функции, то есть глобальных.

#### Глобальные переменные

```js
console.log(hoist); // Вывод: undefined

var hoist = 'The variable has been hoisted.';
```

JavaScript «поднял» объявление переменной.

Поэтому мы можем использовать переменные перед тем, как объявим их. Тем не менее, в этом вопросе нужно быть аккуратными, так как «поднятая» переменная инициализируется со значением `undefined`. Лучшим вариантом, как говорилось выше, было бы объявить и инициализировать нашу переменную перед её непосредственным использованием.

#### Переменные в области видимости функции

Объявление переменной, область видимости которой заканчивается в функции, «поднимается» наверх функции.

Чтобы избегать таких ловушек, нам всегда нужно убеждаться в том, что мы объявляем и инициализируем переменные перед их использованием.

### Strict Mode, или «Строгий режим»

Запуск кода в strict mode:

- Устраняет некоторые скрытые ошибки в JavaScript, изменяя их на явную выдачу ошибок, которые будут в последствии выданы движком.

- Устраняет ошибки, которые затрудняют движкам JavaScript выполнять оптимизацию.

- Запрещает некоторый синтаксис, который с большой долей вероятности будет уже идти из коробки в будущих версиях JavaScript.

Вместо того, чтобы указать, что мы пропустили объявление нашей переменной, `use strict` остановил нас на полпути, выдав `Reference error`.

## ES6

### let

Переменные, объявленные через `let`, заключены в область видимости блока, а не функции. Область видимости переменной привязана к блоку, в котором она объявлена, а не к функции, в которой она объявлена.

### const

const была представлена в es6 для того, чтобы можно было сделать неизменные переменные - переменные, значение которых не может быть изменено или переназначено.

С const, как и с let, переменные поднимаются вверх блока кода.

JavaScript «поднимает» переменные let и const. Разница лишь в том, как он инициализирует их. Переменные, объявленные с `let` и `const`, остаются неинициализированными в начале выполнения, в то время как переменные, объявленные с `var`, инициализируются со значением `undefined`.

## Поднятие функций

JavaScript функции могут классифицироваться как объявленные функции, так и как функциональные выражения.

Объявленные функции полностью поднимаются вверх кода.

Функциональные выражения не поднимаются.

## Порядок по приоритетам

Назначение переменных имеет приоритет перед объявлением функции.

Объявление функции имеет приоритет перед объявлением переменной.

```js
var double = 22;

function double(num) {
  return (num*2);
}

console.log(typeof double); // Вывод: number
```

```js
var double;

function double(num) {
  return (num*2);
}

console.log(typeof double); // Вывод: function
```

## «Поднятие» классов

Классы в JavaScript могут также классифицироваться как объявления и выражения.

### Объявления классов

JavaScript-классы при объявлении «поднимаются». Тем не менее, они остаются неинициализированными до определения. Это говорит о том, что вам надо объявить класс перед тем, как его использовать.

Выражения классов не «поднимаются».

[Источник](https://medium.com/@stasonmars/%D1%80%D0%B0%D0%B7%D0%B1%D0%B8%D1%80%D0%B0%D0%B5%D0%BC%D1%81%D1%8F-%D1%81-%D0%BF%D0%BE%D0%B4%D0%BD%D1%8F%D1%82%D0%B8%D0%B5%D0%BC-hoisting-%D0%B2-javascript-7d2d27bc51f1)
[Оригинал](https://scotch.io/tutorials/understanding-hoisting-in-javascript)