# Function.prototype.apply()

## [Общие сведения](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/apply#summary "Permalink to Общие сведения")

Метод **`apply()`** вызывает функцию с указанным значением `this` и аргументами, предоставленными в виде массива (либо [массивоподобного объекта (en-US)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide#working_with_array-like_objects "Currently only available in English (US)")).

**Примечание:** хотя синтаксис этой функции практически полностью идентичен функции [`call()`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/call), фундаментальное различие между ними заключается в том, что функция `call()` принимает список аргументов, в то время как функция `apply()` принимает единичный массив аргументов.

## [Синтаксис](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/apply#syntax "Permalink to Синтаксис")

```
fun.apply(thisArg, [argsArray])
```

### [Параметры](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/apply#parameters "Permalink to Параметры")

`thisArg`

Опциональный параметр. Значение `this`, предоставляемое для вызова функции _`fun`_. Обратите внимание, что `this` может не быть реальным значением, видимым этим методом: если метод является функцией в [нестрогом режиме (en-US)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode "Currently only available in English (US)"), значения [`null`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/null) и [`undefined`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/undefined) будут заменены глобальным объектом, а примитивные значения будут упакованы в объекты.

`argsArray`

Опциональный параметр. Массивоподобный объект, определяющий аргументы, с которыми функция _`fun`_ должна быть вызвана, либо [`null`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/null) или [`undefined`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/undefined), если в функцию не надо передавать аргументы. Начиная с ECMAScript 5 эти аргументы могут быть обобщёнными массивоподобными объектами, а не только массивом. Смотрите ниже информацию по [совместимости с браузерами](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/apply#browser_compatibility).

## [Описание](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/apply#description "Permalink to Описание")

Вы можете присваивать различные объекты `this` при вызове существующей функции. `this` ссылается на текущий объект, вызывающий объект. С помощью `apply()` вы можете написать метод один раз, а затем наследовать его в других объектах без необходимости переписывать метод для каждого нового объекта.

Метод `apply` очень похож на метод [`call()`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/call), за исключением поддерживаемого типа аргументов. Вы можете использовать массив аргументов вместо набора именованных параметров. Вместе с `apply` вы можете использовать литерал массива, например, `_fun_.apply(this, ['есть', 'бананы'])`, либо объект [`Array`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array), например, `_fun_.apply(this, new Array('есть', 'бананы'))`.

Также вы можете использовать в качестве параметра `argsArray` псевдомассив [`arguments` (en-US)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments "Currently only available in English (US)"). `arguments` является локальной переменной функции. Он может использоваться для всех неопределённых аргументов вызываемого объекта. Таким образом, вы не обязаны знать, сколько и какие аргументы требует вызываемый объект при использовании метода `apply()`. Вы можете использовать псевдомассив `arguments` для передачи всех аргументов в вызываемый объект. Вызываемый объект самостоятельно разберётся с обработкой аргументов.

Начиная с 5-го издания ECMAScript, вы также можете использовать любой вид массивоподобного объекта, что на практике означает, что он должен иметь свойство `length` и целочисленные свойства в диапазоне `(0...length)`. В качестве примера, теперь вы можете использовать [`NodeList`](https://developer.mozilla.org/ru/docs/Web/API/NodeList) или свой собственный объект вида `{ 'length': 2, '0': 'есть', '1': 'бананы' }`.

null

## [Примеры](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/apply#examples "Permalink to Примеры")

### [Пример: использование `apply()` для связи конструкторов объекта в цепочку](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/apply#example_using_apply_to_chain_constructors "Permalink to Пример: использование apply() для связи конструкторов объекта в цепочку")

Вы можете использовать метод `apply()` для объединения в цепочку [конструкторов](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/new) объекта, как в Java. В следующем примере мы создадим в объекте [`Function`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function) глобальный метод `construct()`, который позволит нам использовать массивоподобные объекты с конструктором вместо списка аргументов.

```
Function.prototype.construct = function (aArgs) {
  var oNew = Object.create(this.prototype);
  this.apply(oNew, aArgs);
  return oNew;
};
```

Copy to Clipboard

**Примечание:** метод [`Object.create()`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/create), использованный в этом примере, относительно новый. В качестве альтернативного способа можно рассмотреть возможность использования замыкания:

```
Function.prototype.construct = function(aArgs) {
  var fConstructor = this, fNewConstr = function() { fConstructor.apply(this, aArgs); };
  fNewConstr.prototype = fConstructor.prototype;
  return new fNewConstr();
};
```

Copy to Clipboard

Пример использования:

```
function MyConstructor() {
  for (var nProp = 0; nProp < arguments.length; nProp++) {
    this['property' + nProp] = arguments[nProp];
  }
}

var myArray = [4, 'Привет, мир!', false];
var myInstance = MyConstructor.construct(myArray);

alert(myInstance.property1);                // выведет 'Привет, мир!'
alert(myInstance instanceof MyConstructor); // выведет 'true'
alert(myInstance.constructor);              // выведет 'MyConstructor'
```

Copy to Clipboard

**Примечание:** этот неродной метод `Function.construct()` не будет работать с некоторыми родными конструкторами (вроде конструктора [`Date`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Date), к примеру). В этих случаях вы можете использовать метод [`Function.prototype.bind()`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/bind) (например, представьте, что вы имеете следующий массив, который можно использовать с конструктором [`Date`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Date): `[2012, 11, 4]`; в этом случае вы напишите что-то вроде: `new (Function.prototype.bind.apply(Date, [null].concat([2012, 11, 4])))()` — так или иначе, это не самый изящный способ и, вероятно, его не стоит использовать в рабочем окружении).

### [Пример: использование `apply()` и встроенных функций](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/apply#example_using_apply_and_built-in_functions "Permalink to Пример: использование apply() и встроенных функций")

Умное использование метода `apply()` позволяет вам использовать встроенные функции для некоторых задач, для которых в противном случае пришлось бы писать цикл по массиву значений. В качестве примера давайте используем [`Math.max()`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Math/max)/[`Math.min()`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Math/min) для нахождения максимального/минимального значения в массиве.

```
/* мин/макс числа в массиве */
var numbers = [5, 6, 2, 3, 7];

/* используем apply к Math.min/Math.max */
var max = Math.max.apply(null, numbers); /* Это эквивалентно Math.max(numbers[0], ...)
                                            или Math.max(5, 6, ...) */
var min = Math.min.apply(null, numbers);

/* сравним с простым алгоритмом с циклом */
max = -Infinity, min = +Infinity;

for (var i = 0; i < numbers.length; i++) {
  if (numbers[i] > max) {
    max = numbers[i];
  }
  if (numbers[i] < min) {
    min = numbers[i];
  }
}
```

Copy to Clipboard

Но будьте осторожны: при использовании метода `apply()` таким образом вы рискуете выйти за пределы ограничения на количество аргументов в движке JavaScript. Последствия применения функции с очень большим количеством аргументов (думается, больше десяти тысяч аргументов) различаются от движка к движку (JavaScriptCore имеет жёстко зашитое [ограничение на количество аргументов в 65536](https://bugs.webkit.org/show_bug.cgi?id=80797)), поскольку этот предел (на самом деле, это природа поведения любого чрезвычайно огромного стека) не определён. Некоторые движки будут выкидывать исключение. Хуже того, другие просто отбрасывают реально переданные функции аргументы сверх лимита. (Для иллюстрации последнего случая: если такой движок имеет ограничение в четыре элемента [реальное ограничение, конечно же, гораздо выше], это выглядело бы так, как если бы в примере выше в метод `apply()` были переданы аргументы `5, 6, 2, 3`, а не весь массив.) Если ваш массив значений может вырасти до десятков тысяч, используйте смешанный подход: применяйте вашу функцию к порциям массива:

```
function minOfArray(arr) {
  var min = Infinity;
  var QUANTUM = 32768;

  for (var i = 0, len = arr.length; i < len; i += QUANTUM) {
    var submin = Math.min.apply(null, arr.slice(i, Math.min(i + QUANTUM, len)));
    min = Math.min(submin, min);
  }

  return min;
}

var min = minOfArray([5, 6, 2, 3, 7]);
```