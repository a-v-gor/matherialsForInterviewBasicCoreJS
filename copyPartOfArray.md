# How to copy a part of array

## slice

Метод [arr.slice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice) намного проще, чем похожий на него `arr.splice`.

Синтаксис:

```js
arr.slice([start], [end])
```

Он возвращает новый массив, в который копирует все элементы с индекса `start` до `end` (не включая `end`). `start` и `end` могут быть отрицательными, в этом случае отсчёт позиции будет вестись с конца массива.

Можно вызвать `slice` без аргументов: `arr.slice()` создаёт копию `arr`. Это часто используют, чтобы создать копию массива для дальнейших преобразований, которые не должны менять исходный массив.
