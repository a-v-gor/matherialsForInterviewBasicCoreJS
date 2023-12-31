# Data types

Есть восемь основных типов данных в JavaScript.

Переменная в JavaScript может содержать любые данные. В один момент там может быть строка, а в другой - число:

```js
// Не будет ошибкой
let message = "hello";
message = 123456;
```

Языки программирования, в которых такое возможно, называются "динамически типизированными". Это значит, что типы данных есть, но переменные не привязаны ни к одному из них.

## Число

```js
let n = 123;
n = 12.345;
```

*Числовой* тип данных (`number`) представляет как целочисленные значения, так и числа с плавающей точкой.

Существует множество операций для чисел, например, умножение `*`, деление `/`, сложение `+`, вычитание `-` и так далее.

Кроме обычных чисел, существуют так называемые "специальные числовые значения", которые относятся к этому типу данных: `Infinity`, `-Infinity` и `NaN`.

- `Infinity` представляет собой математическую [бесконечность](https://ru.wikipedia.org/wiki/Бесконечность#В_математике) ∞. Это особое значение, которое больше любого числа.

    Мы можем получить его в результате деления на ноль:

    ```js
    alert( 1 / 0 ); // Infinity
    ```

    Или задать его явно:

    ```js
    alert( Infinity ); // Infinity
    ```

- `NaN` означает вычислительную ошибку. Это результат неправильной или неопределённой математической операции, например:

    ```js
    alert( "не число" / 2 ); // NaN, такое деление является ошибкой
    ```

    Значение `NaN` "прилипчиво".  Любая математическая операция с `NaN` возвращает `NaN`:

    ```js
    alert( NaN + 1 ); // NaN
    alert( 3 * NaN ); // NaN
    alert( "не число" / 2 - 1 ); // NaN
    ```

    Если где-то в математическом выражении есть `NaN`, то оно распространяется на весь результат (есть только одно исключение: `NaN ** 0` равно `1`).

### Математические операции безопасны

Математические операции в JavaScript "безопасны". Мы можем делать что угодно: делить на ноль, обращаться с нечисловыми строками как с числами и т.д.

Скрипт никогда не остановится с фатальной ошибкой (не "умрёт"). В худшем случае мы получим `NaN` как результат выполнения.

Специальные числовые значения относятся к типу "число". Конечно, это не числа в привычном значении этого слова.

### Способы записи числа

Представьте, что нам надо записать число 1 миллиард. Самый очевидный путь:

```js
let billion = 1000000000;
```

Но в реальной жизни мы обычно опускаем запись множества нулей, так как можно легко ошибиться. Укороченная запись может выглядеть как `"1 млрд"` или `"7.3 млрд"` для 7 миллиардов 300 миллионов. Такой принцип работает для всех больших чисел.

В JavaScript можно использовать букву `"e"`, чтобы укоротить запись числа. Она добавляется к числу и заменяет указанное количество нулей:

```js
let billion = 1e9;  // 1 миллиард, буквально: 1 и 9 нулей

alert( 7.3e9 );  // 7.3 миллиардов (7,300,000,000)
```

Сейчас давайте запишем что-нибудь очень маленькое. К примеру, 1 микросекунду (одна миллионная секунды):

```js
let ms = 0.000001;
```

Записать микросекунду в укороченном виде нам поможет `"e"`.

```js
let ms = 1e-6; // шесть нулей, слева от 1
```

Если мы подсчитаем количество нулей `0.000001`, их будет 6. Естественно, верная запись `1e-6`.

### Шестнадцатеричные, двоичные и восьмеричные числа

[Шестнадцатеричные](https://ru.wikipedia.org/wiki/%D0%A8%D0%B5%D1%81%D1%82%D0%BD%D0%B0%D0%B4%D1%86%D0%B0%D1%82%D0%B5%D1%80%D0%B8%D1%87%D0%BD%D0%B0%D1%8F_%D1%81%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%B0_%D1%81%D1%87%D0%B8%D1%81%D0%BB%D0%B5%D0%BD%D0%B8%D1%8F) числа широко используются в JavaScript для представления цветов, кодировки символов и многого другого. Естественно, есть короткий стиль записи: `0x`, после которого указывается число.

Например:

```js
alert( 0xff ); // 255
alert( 0xFF ); // 255 (то же самое, регистр не имеет значения)
```

Не так часто используются двоичные и восьмеричные числа, но они также поддерживаются `0b` для двоичных и `0o` для восьмеричных:

```js
let a = 0b11111111; // бинарная форма записи числа 255
let b = 0o377; // восьмеричная форма записи числа 255

alert( a == b ); // true, с двух сторон число 255
```

Есть только 3 системы счисления с такой поддержкой. Для других систем счисления мы рекомендуем использовать функцию `parseInt`.

### Неточные вычисления

Внутри JavaScript число представлено в виде 64-битного формата [IEEE-754](https://ru.wikipedia.org/wiki/IEEE_754-1985). Для хранения числа используется 64 бита: 52 из них используется для хранения цифр, 11 для хранения положения десятичной точки и один бит отведён на хранение знака.

Если число слишком большое, оно переполнит 64-битное хранилище, JavaScript вернёт бесконечность:

```js
alert( 1e500 ); // Infinity
```

Наиболее часто встречающаяся ошибка при работе с числами в JavaScript - это потеря точности.

Посмотрите на это (неверное!) сравнение:

```js
alert( 0.1 + 0.2 == 0.3 ); // *!*false*/!*
```

Да-да, сумма `0.1` и `0.2` не равна `0.3`.

Странно! Что тогда, если не `0.3`?

```js
alert( 0.1 + 0.2 ); // 0.30000000000000004
```

Ой! Здесь гораздо больше последствий, чем просто некорректное сравнение. Представьте, вы делаете интернет-магазин и посетители формируют заказ из 2-х позиций за `$0.10` и `$0.20`. Итоговый заказ будет `$0.30000000000000004`. Это будет сюрпризом для всех.

Но почему это происходит?

Число хранится в памяти в бинарной форме, как последовательность бит - единиц и нулей. Но дроби, такие как `0.1`, `0.2`, которые выглядят довольно просто в десятичной системе счисления, на самом деле являются бесконечной дробью в двоичной форме.

Другими словами, что такое `0.1`? Это единица делённая на десять — `1/10`, одна десятая. В десятичной системе счисления такие числа легко представимы, по сравнению с одной третьей: `1/3`, которая становится бесконечной дробью `0.33333(3)`.

Деление на `10` гарантированно хорошо работает в десятичной системе, но деление на `3` - нет. По той же причине и в двоичной системе счисления, деление на `2` обязательно сработает, а `1/10` становится бесконечной дробью.

В JavaScript нет возможности для хранения точных значений 0.1 или 0.2, используя двоичную систему, точно также, как нет возможности хранить одну третью в десятичной системе счисления.

Числовой формат IEEE-754 решает эту проблему путём округления до ближайшего возможного числа. Правила округления обычно не позволяют нам увидеть эту "крошечную потерю точности", но она существует.

Пример:

```js
alert( 0.1.toFixed(20) ); // 0.10000000000000000555
```

И когда мы суммируем 2 числа, их "неточности" тоже суммируются.

Вот почему `0.1 + 0.2` - это не совсем `0.3`.

Можно ли обойти проблему? Конечно, наиболее надёжный способ — это округлить результат используя метод [toFixed(n)](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed):

```js
let sum = 0.1 + 0.2;
alert( sum.toFixed(2) ); // 0.30
```

Помните, что метод `toFixed` всегда возвращает строку. Это гарантирует, что результат будет с заданным количеством цифр в десятичной части. Также это удобно для форматирования цен в интернет-магазине `$0.30`. В других случаях можно использовать унарный оператор `+`, чтобы преобразовать строку в число:

```js
let sum = 0.1 + 0.2;
alert( +sum.toFixed(2) ); // 0.3
```

Также можно временно умножить число на 100 (или на большее), чтобы привести его к целому, выполнить математические действия, а после разделить обратно. Суммируя целые числа, мы уменьшаем погрешность, но она всё равно появляется при финальном делении:

```js
alert( (0.1 * 10 + 0.2 * 10) / 10 ); // 0.3
alert( (0.28 * 100 + 0.14 * 100) / 100); // 0.4200000000000001
```

Таким образом, метод умножения/деления уменьшает погрешность, но полностью её не решает.

Иногда можно попробовать полностью отказаться от дробей. Например, если мы в нашем интернет-магазине начнём использовать центы вместо долларов. Но что будет, если мы применим скидку 30%? На практике у нас не получится полностью избавиться от дроби. Просто используйте округление, чтобы отрезать "хвосты", когда надо.

Забавный пример.
Попробуйте выполнить его:

```js
// Привет! Я – число, растущее само по себе!
alert( 9999999999999999 ); // покажет 10000000000000000
```

Причина та же – потеря точности. Из 64 бит, отведённых на число, сами цифры числа занимают до 52 бит, остальные 11 бит хранят позицию десятичной точки и один бит – знак. Так что если 52 бит не хватает на цифры, то при записи пропадут младшие разряды.

Интерпретатор не выдаст ошибку, но в результате получится «не совсем то число», что мы и видим в примере выше. Как говорится: «как смог, так записал».

Два нуля

Другим забавным следствием внутреннего представления чисел является наличие двух нулей: `0` и `-0`.

Все потому, что знак представлен отдельным битом, так что, любое число может быть положительным и отрицательным, включая нуль.

В большинстве случаев это поведение незаметно, так как операторы в JavaScript воспринимают их одинаковыми.

## BigInt

В JavaScript тип `number` не может безопасно работать с числами, большими, чем <code>2<sup>53</sup>-1</code>  (т. е. `9007199254740991`) или меньшими, чем <code>-(2<sup>53</sup>-1)</code> для отрицательных чисел. Технически, тип `number` может хранить и гораздо большие значения (вплоть до <code>1.7976931348623157 * 10<sup>308</sup></code>), однако за пределами безопасного диапазона <code>±(2<sup>53</sup>-1)</code> многие из чисел не могут быть представлены с помощью этого типа данных из-за ограничений, вызванных внутренним представлением чисел в двоичной форме. Например, нечётные числа, большие, чем <code>(2<sup>53</sup>-1)</code>, невозможно хранить при помощи типа `number`, они с разной точностью будут автоматически округляться до чётных значений. В то же время некоторые чётные числа, большие, чем <code>(2<sup>53</sup>-1)</code>, при помощи типа `number` хранить технически возможно (однако не стоит этого делать во избежание дальнейших ошибок).

Для большинства случаев достаточно безопасного диапазона чисел от <code>-(2<sup>53</sup>-1)</code> до <code>(2<sup>53</sup>-1)</code>. Но иногда нам нужен диапазон действительно гигантских целых чисел без каких-либо ограничений или пропущенных значений внутри него. Например, в криптографии или при использовании метки времени ("timestamp") с микросекундами.

Тип `BigInt` был добавлен в JavaScript, чтобы дать возможность работать с целыми числами произвольной длины.

Чтобы создать значение типа `BigInt`, необходимо добавить `n` в конец числового литерала:

```js
// символ "n" в конце означает, что это BigInt
const bigInt = 1234567890123456789012345678901234567890n;
```

### "Поддержка"

В данный момент `BigInt` поддерживается только в браузерах Firefox, Chrome, Edge и Safari, но не поддерживается в IE.

## Строка

Строка (`string`) в JavaScript должна быть заключена в кавычки.

```js
let str = "Привет";
let str2 = 'Одинарные кавычки тоже подойдут';
let phrase = `Обратные кавычки позволяют встраивать переменные ${str}`;
```

В JavaScript существует три типа кавычек.

1. Двойные кавычки: `"Привет"`.
2. Одинарные кавычки: `'Привет'`.
3. Обратные кавычки: \`Привет\`.

Двойные или одинарные кавычки являются "простыми", между ними нет разницы в JavaScript.

Обратные же кавычки имеют расширенную функциональность. Они позволяют нам встраивать выражения в строку, заключая их в `${…}`. Например:

```js
let name = "Иван";

// Вставим переменную
alert( `Привет, *!*${name}*/!*!` ); // Привет, Иван!

// Вставим выражение
alert( `результат: *!*${1 + 2}*/!*` ); // результат: 3
```

Выражение внутри `${…}` вычисляется, и его результат становится частью строки. Мы можем положить туда всё, что угодно: переменную `name`, или выражение `1 + 2`, или что-то более сложное.

Обратите внимание, что это можно делать только в обратных кавычках. Другие кавычки не имеют такой функциональности встраивания!

```js
alert( "результат: ${1 + 2}" ); // результат: ${1 + 2} (двойные кавычки ничего не делают)
```

Ещё одно преимущество обратных кавычек — они могут занимать более одной строки, вот так:

```js run
let guestList = `Guests:
 * John
 * Pete
 * Mary
`;

alert(guestList); // список гостей, состоящий из нескольких строк
```

Обратные кавычки также позволяют задавать "шаблонную функцию" перед первой обратной кавычкой. Используемый синтаксис: <code>func&#96;string&#96;</code>. Автоматически вызываемая функция `func` получает строку и встроенные в неё выражения и может их обработать. Подробнее об этом можно прочитать в [документации](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Template_literals). Если перед строкой есть выражение, то шаблонная строка называется "теговым шаблоном". Это позволяет использовать свою шаблонизацию для строк, но на практике теговые шаблоны применяются редко.

### Спецсимволы

Многострочные строки также можно создавать с помощью одинарных и двойных кавычек, используя так называемый "символ перевода строки", который записывается как `\n`:

```js run
let guestList = "Guests:\n * John\n * Pete\n * Mary";

alert(guestList); // список гостей, состоящий из нескольких строк
```

Есть и другие, реже используемые спецсимволы. Вот список:

| Символ | Описание |
|-----------|-------------|
|`\n`|Перевод строки|
|`\r`|В текстовых файлах Windows для перевода строки используется комбинация символов `\r\n`, а на других ОС это просто `\n`. Это так по историческим причинам, ПО под Windows обычно понимает и просто `\n`. |
|`\'`, `\"`|Кавычки|
|`\\`|Обратный слеш|
|`\t`|Знак табуляции|
|`\b`, `\f`, `\v`| Backspace, Form Feed и Vertical Tab — оставлены для обратной совместимости, сейчас не используются. |

Как вы можете видеть, все спецсимволы начинаются с обратного слеша, `\` — так называемого "символа экранирования".

### Длина строки

Свойство `length` содержит длину строки:

```js run
alert( `My\n`.length ); // 3
```

Обратите внимание, `\n` — это один спецсимвол, поэтому тут всё правильно: длина строки `3`.

### Доступ к символам

Получить символ, который занимает позицию `pos`, можно с помощью квадратных скобок: `[pos]`. Также можно использовать метод [str.at(pos)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/at). Первый символ занимает нулевую позицию.

Квадратные скобки всегда возвращают `undefined` для отрицательных индексов.

### Строки неизменяемы

Содержимое строки в JavaScript нельзя изменить.

Можно создать новую строку и записать её в ту же самую переменную вместо старой.

Нет отдельного типа данных для одного символа.

Строка может содержать ноль символов (быть пустой), один символ или множество.

## Булевый (логический) тип

Булевый тип (`boolean`) может принимать только два значения: `true` (истина) и `false` (ложь).

Например:

```js
let nameFieldChecked = true; // да, поле отмечено
let ageFieldChecked = false; // нет, поле не отмечено
```

Булевые значения также могут быть результатом сравнений:

```js
let isGreater = 4 > 1;

alert( isGreater ); // true (результатом сравнения будет "да")
```

## Значение "null"

Специальное значение `null` не относится ни к одному из типов, описанных выше.

Оно формирует отдельный тип, который содержит только значение `null`:

```js
let age = null;
```

В JavaScript `null` не является "ссылкой на несуществующий объект" или "нулевым указателем", как в некоторых других языках.

Это просто специальное значение, которое представляет собой "ничего", "пусто" или "значение неизвестно".

## Значение "undefined"

Специальное значение `undefined` также стоит особняком. Оно формирует тип из самого себя так же, как и `null`.

Оно означает, что "значение не было присвоено".

Если переменная объявлена, но ей не присвоено никакого значения, то её значением будет `undefined`:

```js
let age;

alert(age); // выведет "undefined"
```

Технически мы можем присвоить значение `undefined` любой переменной, но так делать не рекомендуется. Обычно `null` используется для присвоения переменной "пустого" или "неизвестного" значения, а `undefined` — для проверок, была ли переменная назначена.

## Объекты и символы

Тип `object` (объект) — особенный.

Все остальные типы называются "примитивными", потому что их значениями могут быть только простые значения (будь то строка, или число, или что-то ещё). В объектах же хранят коллекции данных или более сложные структуры.

Тип `symbol` (символ) используется для создания уникальных идентификаторов в объектах.

## Оператор typeof

Оператор `typeof` возвращает тип аргумента.

У него есть две синтаксические формы:

```js
// Обычный синтаксис
typeof 5 // Выведет "number"
// Синтаксис, напоминающий вызов функции (встречается реже)
typeof(5) // Также выведет "number"
```

Если передается выражение, то нужно заключать его в скобки, т.к. typeof имеет более высокий приоритет, чем бинарные операторы:

```js
typeof 50 + " Квартир"; // Выведет "number Квартир"
typeof (50 + " Квартир"); // Выведет "string"
```

Вызов `typeof x` возвращает строку с именем типа:

```js
typeof undefined // "undefined"

typeof 0 // "number"

typeof 10n // "bigint"

typeof true // "boolean"

typeof "foo" // "string"

typeof Symbol("id") // "symbol"

typeof Math // "object"  (1)

typeof null // "object"  (2)

typeof alert // "function"  (3)
```

Последние три строки нуждаются в пояснении:

1. `Math` — это встроенный объект, который предоставляет математические операции и константы.

2. Результатом вызова `typeof null` является `"object"`. Это официально признанная ошибка в `typeof`, ведущая начало с времён создания JavaScript и сохранённая для совместимости. Конечно, `null` не является объектом. Это специальное значение с отдельным типом.

3. Вызов `typeof alert` возвращает `"function"`, потому что `alert` является функцией. Мы изучим функции в следующих главах, где заодно увидим, что в JavaScript нет специального типа "функция". Функции относятся к объектному типу. Но `typeof` обрабатывает их особым образом, возвращая `"function"`. Так тоже повелось от создания JavaScript. Формально это неверно, но может быть удобным на практике.
