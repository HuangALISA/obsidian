## [Сводка](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/bind#summary "Permalink to Сводка")

Метод `**bind()**` создаёт новую функцию, которая при вызове устанавливает в качестве контекста выполнения `this` предоставленное значение. В метод также передаётся набор аргументов, которые будут установлены перед переданными в привязанную функцию аргументами при её вызове.

## [Синтаксис](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/bind#syntax "Permalink to Синтаксис")

```
fun.bind(thisArg[, arg1[, arg2[, ...]]])
```

### [Параметры](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/bind#parameters "Permalink to Параметры")

`thisArg`

Значение, передаваемое в качестве `this` в целевую функцию при вызове привязанной функции. Значение игнорируется, если привязанная функция конструируется с помощью оператора [`new`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/new).

`arg1, arg2, ...`

Аргументы целевой функции, передаваемые перед аргументами привязанной функции при вызове целевой функции.

## [Описание](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/bind#description "Permalink to Описание")

Метод `bind()` создаёт новую "**привязанную функцию**" (**ПФ**). **ПФ** - это "необычный функциональный объект" ( термин из **ECMAScript 6** ), который является обёрткой над исходным функциональным объектом. Вызов **ПФ** приводит к исполнению кода обёрнутой функции.

**ПФ** имеет следующие внутренние ( скрытые ) свойства:

-   **[[BoundTargetFunction]]** - оборачиваемый (целевой ) функциональный объект
-   **[[BoundThis]]** - значение, которое всегда передаётся в качестве значения **this** при вызове обёрнутой функции.
-   **[[BoundArguments]]** - список значений, элементы которого используются в качестве первого аргумента при вызове оборачиваемой функции.
-   **[[Call****]]** - внутренний метод. Выполняет код (функциональное выражение), связанный с функциональным объектом.

Когда **ПФ** вызывается, исполняется её внутренний метод **[[Call]]** со следующими аргументами **Call(_target_, _boundThis_, _args_).**

-   **_target_** - **[[BoundTargetFunction]]**;
-   _**boundThis**_ - **[[BoundThis]]**;
-   _**args**_ - **[[BoundArguments]].**

Привязанная функция также может быть сконструирована с помощью оператора [`new`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/new): это работает так, как если бы вместо неё конструировалась целевая функция. Предоставляемое значение `this` в этом случае игнорируется, хотя ведущие аргументы всё ещё передаются в эмулируемую функцию.

## [Примеры](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/bind#examples "Permalink to Примеры")

### [Пример: создание привязанной функции](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/bind#example:_creating_a_bound_function "Permalink to Пример: создание привязанной функции")

Простейшим способом использования `bind()` является создание функции, которая, вне зависимости от способа её вызова, вызывается с определённым значением `this`. Обычным заблуждением для новичков в JavaScript является извлечение метода из объекта с целью его дальнейшего вызова в качестве функции и ожидание того, что он будет использовать оригинальный объект в качестве своего значения `this` (например, такое может случиться при использовании метода как колбэк-функции). Однако, без специальной обработки, оригинальный объект зачастую теряется. Создание привязанной функции из функции, использующей оригинальный объект, изящно решает эту проблему:

```
this.x = 9;
var module = {
  x: 81,
  getX: function() { return this.x; }
};

module.getX(); // 81

var getX = module.getX;
getX(); // 9, поскольку в этом случае this ссылается на глобальный объект

// создаём новую функцию с this, привязанным к module
var boundGetX = getX.bind(module);
boundGetX(); // 81
```

Copy to Clipboard

### [Пример: частичные функции](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/bind#example:_partial_functions "Permalink to Пример: частичные функции")

Следующим простейшим способом использования `bind()` является создание функции с предопределёнными аргументами. Эти аргументы (если они есть) передаются после значения `this` и вставляются перед аргументами, передаваемыми в целевую функцию при вызове привязанной функции.

```
function list() {
  return Array.prototype.slice.call(arguments);
}

var list1 = list(1, 2, 3); // [1, 2, 3]

// Создаём функцию с предустановленным ведущим аргументом
var leadingThirtysevenList = list.bind(undefined, 37);

var list2 = leadingThirtysevenList(); // [37]
var list3 = leadingThirtysevenList(1, 2, 3); // [37, 1, 2, 3]
```

Copy to Clipboard

### [Пример: с `setTimeout`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/bind#example:_with_settimeout "Permalink to Пример: с setTimeout")

По умолчанию, внутри [`window.setTimeout()` (en-US)](https://developer.mozilla.org/en-US/docs/Web/API/setTimeout "Currently only available in English (US)") контекст `this` устанавливается в объект [`window`](https://developer.mozilla.org/ru/docs/Web/API/Window) (или `global`). При работе с методами класса, требующими `this` для ссылки на экземпляры класса, вы можете явно привязать `this` к колбэк-функции для сохранения экземпляра.

```
function LateBloomer() {
  this.petalCount = Math.ceil(Math.random() * 12) + 1;
}

// Объявляем цветение с задержкой в 1 секунду
LateBloomer.prototype.bloom = function() {
  window.setTimeout(this.declare.bind(this), 1000);
};

LateBloomer.prototype.declare = function() {
  console.log('Я прекрасный цветок с ' +
    this.petalCount + ' лепестками!');
};
```

Copy to Clipboard

### [Пример: привязывание функций, используемых в качестве конструкторов](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/bind#example:_bound_functions_used_as_constructors "Permalink to Пример: привязывание функций, используемых в качестве конструкторов")

**Предупреждение:** этот раздел демонстрирует возможности JavaScript и документирует некоторые граничные случаи использования метода `bind()`. Показанные ниже методы не являются лучшей практикой и, вероятно, их не следует использовать в рабочем окружении.

Привязанные функции автоматически подходят для использования вместе с оператором [`new`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/new) для конструирования новых экземпляров, создаваемых целевой функцией. Когда привязанная функция используется для конструирования значения, предоставляемое значение `this` игнорируется. Однако, предоставляемые аргументы всё так же вставляются перед аргументами конструктора:

```
function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function() {
  return this.x + ',' + this.y;
};

var p = new Point(1, 2);
p.toString(); // '1,2'


var emptyObj = {};
var YAxisPoint = Point.bind(emptyObj, 0/*x*/);
// не поддерживается полифилом, приведённым ниже,
// но отлично работает с родным bind:
var YAxisPoint = Point.bind(null, 0/*x*/);

var axisPoint = new YAxisPoint(5);
axisPoint.toString(); // '0,5'

axisPoint instanceof Point; // true
axisPoint instanceof YAxisPoint; // true
new Point(17, 42) instanceof YAxisPoint; // true
```

Copy to Clipboard

Обратите внимание, что вам не нужно делать ничего особенного для создания привязанной функции, используемой с оператором [`new`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/new). В итоге, для создания явно вызываемой привязанной функции, вам тоже не нужно делать ничего особенного, даже если вам требуется, чтобы привязанная функция вызывалась только с помощью оператора [`new`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/new).

```
// Пример может быть запущен прямо в вашей консоли JavaScript
// ...продолжение примера выше

// Всё ещё можно вызывать как нормальную функцию
// (хотя обычно это не предполагается)
YAxisPoint(13);

emptyObj.x + ',' + emptyObj.y;
// >  '0,13'
```

Copy to Clipboard

Если вы хотите поддерживать использование привязанной функции только с помощью оператора [`new`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/new), либо только с помощью прямого вызова, целевая функция должна предусматривать такие ограничения.

### [Пример: создание сокращений](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/bind#example:_creating_shortcuts "Permalink to Пример: создание сокращений")

Метод `bind()` также полезен в случаях, если вы хотите создать сокращение для функции, требующей определённое значение `this`.

Возьмём, например, метод [`Array.prototype.slice`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/slice), который вы можете использовать для преобразования массивоподобного объекта в настоящий массив. Вы можете создать подобное сокращение:

```
var slice = Array.prototype.slice;

// ...

slice.call(arguments);
```

Copy to Clipboard

С помощью метода `bind()`, это сокращение может быть упрощено. В следующем куске кода `slice` является функцией, привязанной к функции [`call()`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/call) объекта [`Function.prototype` (en-US)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/prototype "Currently only available in English (US)"), со значением `this`, установленным в функцию [`slice()`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/slice) объекта `Array.prototype`. Это означает, что дополнительный вызов `call()` может быть устранён:

```
// Тоже самое, что и slice в предыдущем примере
var unboundSlice = Array.prototype.slice;
var slice = Function.prototype.call.bind(unboundSlice);

// ...

slice(arguments);
```