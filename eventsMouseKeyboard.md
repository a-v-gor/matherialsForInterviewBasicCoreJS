# События мыши и клавиатуры

## События мыши

Сразу заметим: эти события бывают не только из-за мыши, но и эмулируются на других устройствах, в частности, на мобильных, для совместимости.

### Типы событий мыши

Мы уже видели некоторые из этих событий:

`mousedown/mouseup`
: Кнопка мыши нажата/отпущена над элементом.

`mouseover/mouseout`
: Курсор мыши появляется над элементом и уходит с него.

`mousemove`
: Каждое движение мыши над элементом генерирует это событие.

`click`
: Вызывается при `mousedown` , а затем `mouseup`  над одним и тем же элементом, если использовалась левая кнопка мыши.

`dblclick`
: Вызывается двойным кликом на элементе.

`contextmenu`
: Вызывается при попытке открытия контекстного меню, как правило, нажатием правой кнопки мыши. Но, заметим, это не совсем событие мыши, оно может вызываться и специальной клавишей клавиатуры.

...Есть также несколько иных типов событий, которые мы рассмотрим позже.

### Порядок событий

Как вы можете видеть из приведённого выше списка, действие пользователя может вызвать несколько событий.

Например, клик мышью вначале вызывает `mousedown`, когда кнопка нажата, затем `mouseup` и `click`, когда она отпущена.

В случае, когда одно действие инициирует несколько событий, порядок их выполнения фиксирован. То есть обработчики событий вызываются в следующем порядке: `mousedown` -> `mouseup` -> `click`.

### Кнопки мыши

События, связанные с кликом, всегда имеют свойство `button`, которое позволяет получить конкретную кнопку мыши.

Обычно мы не используем его для событий `click` и `contextmenu`, потому что первое происходит только при щелчке левой кнопкой мыши, а второе - только при щелчке правой кнопкой мыши.

С другой стороны, обработчикам `mousedown` и `mouseup` может потребоваться `event.button`, потому что эти события срабатывают на любую кнопку, таким образом `button` позволяет различать "нажатие правой кнопки" и "нажатие левой кнопки".

Возможными значениями `event.button` являются:

| Состояние кнопки | `event.button` |
|--------------|----------------|
| Левая кнопка (основная) | 0 |
| Средняя кнопка (вспомогательная) | 1 |
| Правая кнопка (вторичная) | 2 |
| Кнопка X1 (назад) | 3 |
| Кнопка X2 (вперёд) | 4 |

Большинство мышек имеют только левую и правую кнопку, поэтому возможные значения это 0 или 2. Сенсорные устройства также генерируют аналогичные события, когда кто-то нажимает на них.

Также есть свойство `event.buttons`, в котором все нажатые в данный момент кнопки представлены в виде целого числа, по одному биту на кнопку. На практике это свойство используется очень редко, вы можете найти подробную информацию по адресу [MDN](https://developer.mozilla.org/ru/docs/Web/API/MouseEvent/buttons), если вам это когда-нибудь понадобится.

#### Устаревшее свойство `event.which`

В старом коде вы можете встретить `event.which` свойство - это старый нестандартный способ получения кнопки с возможными значениями:

- `event.which == 1` – левая кнопка,
- `event.which == 2` – средняя кнопка,
- `event.which == 3` – правая кнопка.

На данный момент `event.which` устарел, нам не следует его использовать.

Средняя кнопка сейчас -- скорее экзотика, и используется очень редко.

### Модификаторы: shift, alt, ctrl и meta

Все события мыши включают в себя информацию о нажатых клавишах-модификаторах.

Свойства события:

- `shiftKey`: `key:Shift`
- `altKey`: `key:Alt` (или `key:Opt` для Mac)
- `ctrlKey`: `key:Ctrl`
- `metaKey`: `key:Cmd` для Mac

Они равны `true`, если во время события была нажата соответствующая клавиша.

Например, кнопка внизу работает только при комбинации `key:Alt+Shift`+клик:

```html
<button id="button">Нажми Alt+Shift+Click на мне!</button>

<script>
  button.onclick = function(event) {
    if (event.altKey && event.shiftKey) {
      alert('Ура!');
    }
  };
</script>
```

В Windows и Linux клавишами-модификаторами являются `key:Alt`, `key:Shift` и `key:Ctrl`. На Mac есть ещё одна: `key:Cmd`, которой соответствует свойство `metaKey`.

В большинстве приложений, когда в Windows/Linux используется `key:Ctrl`, на Mac используется `key:Cmd`.

То есть, когда пользователь Windows нажимает `key:Ctrl+Enter` и `key:Ctrl+A`, пользователь Mac нажимает `key:Cmd+Enter` или `key:Cmd+A`, и так далее.

Поэтому, если мы хотим поддерживать такие комбинации, как `key:Ctrl`+клик, то для Mac имеет смысл использовать `key:Cmd`+клик. Это удобней для пользователей Mac.

Даже если мы и хотели бы заставить людей на Mac использовать именно `key:Ctrl`+клик, это довольно сложно. Проблема в том, что левый клик в сочетании с `key:Ctrl` интерпретируется как *правый клик* на MacOS и генерирует событие `contextmenu`, а не `click` как на Windows/Linux.

Поэтому, если мы хотим, чтобы пользователям всех операционных систем было удобно, то вместе с `ctrlKey` нам нужно проверять `metaKey`.

Для JS-кода это означает, что мы должны проверить `if (event.ctrlKey || event.metaKey)`.

Комбинации клавиш хороши в качестве дополнения к рабочему процессу. Так что, если посетитель использует клавиатуру – они работают.

Но если на их устройстве его нет – тогда должен быть способ жить без клавиш-модификаторов.

### Координаты: clientX/Y, pageX/Y

Все события мыши имеют координаты двух видов:

1. Относительно окна: `clientX` и `clientY`.

2. Относительно документа: `pageX` и `pageY`.

Относительные координаты документа `pageX/Y` отсчитываются от левого верхнего угла документа и не меняются при прокрутке страницы, в то время как `clientX/Y` отсчитываются от левого верхнего угла текущего окна. Когда страница прокручивается, они меняются.

### Отключаем выделение

Двойной клик мыши имеет побочный эффект, который может быть неудобен в некоторых интерфейсах: он выделяет текст.

Если зажать левую кнопку мыши и, не отпуская кнопку, провести мышью, то также будет выделение, которое в интерфейсах может быть "не кстати".

В данном случае самым разумным будет отменить действие браузера по умолчанию при событии `mousedown`, это отменит оба этих выделения:

```html
До...
<b ondblclick="alert('Клик!')" onmousedown="return false">
  Сделайте двойной клик на мне
</b>
...После
```

Теперь выделенный жирным элемент не выделяется при двойном клике, а также на нём нельзя начать выделение, зажав кнопку мыши.

Заметим, что текст внутри него по-прежнему можно выделить, если начать выделение не на самом тексте, а до него или после. Обычно это нормально воспринимается пользователями.

Если мы хотим отключить выделение для защиты содержимого страницы от копирования, то мы можем использовать другое событие: `oncopy`.

```html
<div oncopy="alert('Копирование запрещено!');return false">
  Уважаемый пользователь,
  Копирование информации запрещено для вас.
  Если вы знаете JS или HTML, вы можете найти всю нужную вам информацию в исходном коде страницы.
</div>
```

### Движение мыши: mouseover/out, mouseenter/leave

В этой главе мы более подробно рассмотрим события, возникающие при движении указателя мыши над элементами страницы.

#### События mouseover/mouseout, relatedTarget

Событие `mouseover` происходит в момент, когда курсор оказывается над элементом, а событие `mouseout` -- в момент, когда курсор уходит с элемента.

Эти события являются особенными, потому что у них имеется свойство `relatedTarget`. Оно "дополняет" `target`. Когда мышь переходит с одного элемента на другой, то один из них будет `target`, а другой `relatedTarget`.

Для события `mouseover`:

- `event.target` -- это элемент, *на который* курсор перешёл.

- `event.relatedTarget` -- это элемент, *с которого* курсор ушёл (`relatedTarget` -> `target`).

Для события `mouseout` наоборот:

- `event.target` -- это элемент, *с которого* курсор ушёл.

- `event.relatedTarget` -- это элемент, *на который* курсор перешёл (`target` -> `relatedTarget`).

Свойство `relatedTarget` может быть `null`.

Это нормально и означает, что указатель мыши перешёл не с другого элемента, а из-за пределов окна браузера. Или же, наоборот, ушёл за пределы окна.

Если, например, написать `event.relatedTarget.tagName`, то при отсутствии `event.relatedTarget` будет ошибка.

#### Пропуск элементов

Событие `mousemove` происходит при движении мыши. Однако, это не означает, что указанное событие генерируется при прохождении каждого пикселя.

Браузер периодически проверяет позицию курсора и, заметив изменения, генерирует события `mousemove`.

Это означает, что если пользователь двигает мышкой очень быстро, то некоторые DOM-элементы могут быть пропущены.

Мы должны иметь в виду, что указатель мыши не "посещает" все элементы на своём пути. Он может и "прыгать".

В частности, возможно, что указатель запрыгнет в середину страницы из-за пределов окна браузера. В этом случае значение `relatedTarget` будет `null`, так как курсор пришёл "из ниоткуда".

Если указатель "официально" зашёл на элемент, то есть было событие `mouseover`, то при выходе с него обязательно будет `mouseout`.

#### Событие mouseout при переходе на потомка

Важная особенность события `mouseout` - оно генерируется в том числе, когда указатель переходит с элемента на его потомка.

То есть, визуально указатель всё ещё на элементе, но мы получим `mouseout`!

**По логике браузера, курсор мыши может быть только над одним элементом в любой момент времени - над самым глубоко вложенным и верхним по z-index.**

Таким образом, если курсор переходит на другой элемент (пусть даже дочерний), то он покидает предыдущий.

Обратите внимание на важную деталь.

Событие `mouseover`, происходящее на потомке, всплывает. Поэтому если на родительском элементе есть такой обработчик, то оно его вызовет.

Вы можете наглядно увидеть это в примере ниже: `<div id="child">` находится внутри `<div id="parent">`. На родителе определены обработчики событий `mouseover/out`, которые выводят информацию о них в текстовое поле.

При переходе мышью с внешнего элемента на внутренний, вы увидите сразу два события: `mouseout [target: parent]` (ушли с родителя) и `mouseover [target: child]` (перешли на потомка, событие всплыло).

При переходе с родителя элемента на потомка - на родителе сработают два обработчика: и `mouseout` и `mouseover`:

```js
parent.onmouseout = function(event) {
  /* event.target: внешний элемент */
};
parent.onmouseover = function(event) {
  /* event.target: внутренний элемент (всплыло) */
};
```

Если код внутри обработчиков не смотрит на `target`, то он подумает, что мышь ушла с элемента `parent` и вернулась на него обратно. Но это не так! Мышь никуда не уходила, она просто перешла на потомка.

Если при уходе с элемента что-то происходит, например, запускается анимация, то такая интерпретация происходящего может давать нежелательные побочные эффекты.

Чтобы этого избежать, можно смотреть на `relatedTarget` и, если мышь всё ещё внутри элемента, то игнорировать такие события.

Или же можно использовать другие события: `mouseenter` и `mouseleave`, с ними такая проблема не возникает.

### События mouseenter и mouseleave

События `mouseenter/mouseleave` похожи на `mouseover/mouseout`. Они тоже генерируются, когда курсор мыши переходит на элемент или покидает его.

Но есть и пара важных отличий:

1. Переходы внутри элемента, на его потомки и с них, не считаются.

2. События `mouseenter/mouseleave` не всплывают.

События `mouseenter/mouseleave` предельно просты и понятны.

Когда указатель появляется над элементом -- генерируется `mouseenter`, причём не имеет значения, где именно указатель: на самом элементе или на его потомке.

Событие `mouseleave` происходит, когда курсор покидает элемент.

### Делегирование событий

События `mouseenter/leave` просты и легки в использовании. Но они не всплывают. Таким образом, мы не можем их делегировать.

Представьте ситуацию, когда мы хотим обрабатывать события, сгенерированные при движении курсора по ячейкам таблицы. И в таблице сотни ячеек.

Очевидное решение -- определить обработчик на родительском элементе `<table>` и там обрабатывать возникающие события. Но, так как `mouseenter/leave` не всплывают, то если событие происходит на ячейке `<td>`, то только обработчик на `<td>` может поймать его.

Обработчики событий `mouseenter/leave` на `<table>` срабатывают, если курсор оказывается над таблицей в целом или же уходит с неё. Невозможно получить какую-либо информацию о переходах между ячейками внутри таблицы.

Что ж, не проблема -- будем использовать `mouseover/mouseout`.

Начнём с простых обработчиков, которые выделяют текущий элемент под указателем мыши:

```js
// выделим элемент под мышью
table.onmouseover = function(event) {
  let target = event.target;
  target.style.background = 'pink';
};

table.onmouseout = function(event) {
  let target = event.target;
  target.style.background = '';
};
```

В нашем случае мы хотим обрабатывать переходы именно между ячейками `<td>`: вход на ячейку и выход с неё. Прочие переходы, в частности, внутри ячейки `<td>` или вообще вне любых ячеек, нас не интересуют, хорошо бы их отфильтровать.

Можно достичь этого так:

- Запоминать текущую ячейку `<td>` в переменную, которую назовём `currentElem`.

- На `mouseover` -- игнорировать событие, если мы всё ещё внутри той же самой ячейки `<td>`.

- На `mouseout` -- игнорировать событие, если это не уход с текущей ячейки `<td>`.

## Клавиатура: keydown и keyup

Прежде чем перейти к клавиатуре, обратите внимание, что на современных устройствах есть и другие способы "ввести что-то". Например, распознавание речи (это особенно актуально на мобильных устройствах) или Копировать/Вставить с помощью мыши.

Поэтому, если мы хотим корректно отслеживать ввод в поле `<input>`, то одних клавиатурных событий недостаточно. Существует специальное событие `input`, чтобы отслеживать любые изменения в поле `<input>`. И оно справляется с такой задачей намного лучше.

События клавиатуры должны использоваться, если мы хотим обрабатывать взаимодействие пользователя именно с клавиатурой (в том числе виртуальной). К примеру, если нам нужно реагировать на стрелочные клавиши `key:Up` и `key:Down` или горячие клавиши (включая комбинации клавиш).

### События keydown и keyup

Событие `keydown` происходит при нажатии клавиши, а `keyup` -- при отпускании.

#### event.code и event.key

Свойство `key` объекта события позволяет получить символ, а свойство `code` -- "физический код клавиши".

К примеру, одну и ту же клавишу `key:Z` можно нажать с клавишей `key:Shift` и без неё. В результате получится два разных символа: `z` в нижнем регистре и `Z` в верхнем регистре.

Свойство `event.key` -- это непосредственно символ, и он может различаться. Но `event.code` всегда будет тот же:

| Клавиша          | `event.key` | `event.code` |
|--------------|-------------|--------------|
| `key:Z`          |`z` (нижний регистр)         |`KeyZ`        |
| `key:Shift+Z`    |`Z` (Верхний регистр)          |`KeyZ`        |

Если пользователь работает с разными языками, то при переключении на другой язык символ изменится с `"Z"` на совершенно другой. Получившееся станет новым значением `event.key`, тогда как `event.code` останется тем же: `"KeyZ"`.

У каждой клавиши есть код, который зависит от её расположения на клавиатуре. Подробно о клавишных кодах можно прочитать в [спецификации о кодах событий UI](https://www.w3.org/TR/uievents-code/).

Например:

- Буквенные клавиши имеют коды по типу `"Key<буква>"`: `"KeyA"`, `"KeyB"` и т.д.

- Коды числовых клавиш строятся по принципу: `"Digit<число>"`: `"Digit0"`, `"Digit1"` и т.д.

- Код специальных клавиш -- это их имя: `"Enter"`, `"Backspace"`, `"Tab"` и т.д.

Существует несколько широко распространённых раскладок клавиатуры, и в спецификации приведены клавишные коды к каждой из них.

Можно их прочитать в [разделе спецификации, посвящённом буквенно-цифровым клавишам](https://www.w3.org/TR/uievents-code/#key-alphanumeric-section).

Регистр важен: `"KeyZ"`, а не `"keyZ"`

А что, если клавиша не буквенно-цифровая? Например, `Shift` или `F1`, или какая-либо другая специальная клавиша? В таких случаях значение свойства `event.key` примерно тоже, что и у `event.code`:

| Клавиша          | `event.key` | `event.code` |
|--------------|-------------|--------------|
| `key:F1`      |`F1`          |`F1`        |
| `key:Backspace`      |`Backspace`          |`Backspace`        |
| `key:Shift`|`Shift`          |`ShiftRight` или `ShiftLeft`        |

`event.code` точно указывает, какая именно клавиша нажата. Так, большинство клавиатур имеют по две клавиши `key:Shift`: слева и справа. `event.code` уточняет, какая именно из них была нажата, в то время как `event.key` сообщает о "смысле" клавиши: что вообще было нажато (`Shift`).

`event.code` может содержать неправильный символ при неожиданной раскладке. Одни и те же буквы на разных раскладках могут сопоставляться с разными физическими клавишами, что приводит к разным кодам.  К счастью, это происходит не со всеми кодами, а с несколькими, например `KeyA`, `KeyQ`, `KeyZ` (как мы уже видели), и не происходит со специальными клавишами, такими как `Shift`. Вы можете найти полный список проблемных кодов в [спецификации](https://www.w3.org/TR/uievents-code/#table-key-code-alphanumeric-writing-system).

Чтобы отслеживать символы, зависящие от раскладки, `event.key` надёжнее.

С другой стороны, преимущество `event.code` заключается в том, что его значение всегда остаётся неизменным, будучи привязанным к физическому местоположению клавиши, даже если пользователь меняет язык. Так что горячие клавиши, использующие это свойство, будут работать даже в случае переключения языка.

Хотим поддерживать клавиши, меняющиеся при раскладке? Тогда `event.key` - верный выбор.

Или мы хотим, чтобы горячая клавиша срабатывала даже после переключения на другой язык? Тогда `event.code` может быть лучше.

#### Автоповтор

При долгом нажатии клавиши возникает автоповтор: `keydown` срабатывает снова и снова, и когда клавишу отпускают, то отрабатывает `keyup`. Так что ситуация, когда много `keydown`и один `keyup`, абсолютно нормальна.

Для событий, вызванных автоповтором, у объекта события свойство `event.repeat` равно `true`.

#### Действия по умолчанию

Действия по умолчанию весьма разнообразны, много чего можно инициировать нажатием на клавиатуре.

Предотвращение стандартного действия с помощью `event.preventDefault()` работает практически во всех сценариях, кроме тех, которые происходят на уровне операционной системы. Например, комбинация `key:Alt+F4` инициирует закрытие браузера в Windows, что бы мы ни делали в JavaScript.

#### "Дела минувших дней"

В прошлом существовало также событие `keypress`, а также свойства `keyCode`, `charCode`, `which` у объекта события.

Но количество браузерных несовместимостей при работе с ними было столь велико, что у разработчиков спецификации не было другого выхода, кроме как объявить их устаревшими и создать новые, современные события (которые и описываются в этой главе). Старый код ещё работает, так как браузеры продолжают поддерживать и `keypress`, и `keyCode` с `charCode`, и `which`, но более нет никакой необходимости в их использовании.
