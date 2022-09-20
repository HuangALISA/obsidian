# Function.prototype.call()

## [Сводка](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/call#summary "Permalink to Сводка")

Метод `**call()**` вызывает функцию с указанным значением `this` и индивидуально предоставленными аргументами.

**Примечание:** хотя синтаксис этой функции практически полностью идентичен функции [`apply()`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/apply), фундаментальное различие между ними заключается в том, что функция `call()` принимает **список аргументов**, в то время, как функция `apply()` **- одиночный массив аргументов**.

## [Синтаксис](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/call#syntax "Permalink to Синтаксис")

```
fun.call(thisArg[, arg1[, arg2[, ...]]])
```

### [Параметры](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/call#parameters "Permalink to Параметры")

`thisArg`

Значение `this`, предоставляемое для вызова функции _`fun`_. Обратите внимание, что `this` может не быть реальным значением, видимым этим методом: если метод является функцией в [нестрогом режиме (en-US)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode "Currently only available in English (US)"), значения [`null`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/null) и [`undefined`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/undefined) будут заменены глобальным объектом, а примитивные значения будут упакованы в объекты.

`arg1, arg2, ...`

Аргументы для объекта.

## [Описание](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/call#description "Permalink to Описание")

Вы можете присваивать различные объекты `this` при вызове существующей функции. `this` ссылается на текущий объект, вызвавший объект. С помощью `call` вы можете написать метод один раз, а затем наследовать его в других объектах, без необходимости переписывать метод для каждого нового объекта.

## [Примеры](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/call#examples "Permalink to Примеры")

### [Пример: использование `call` для связи конструкторов объекта в цепочку](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/call#example_using_call_to_chain_constructors_for_an_object "Permalink to Пример: использование call для связи конструкторов объекта в цепочку")

Вы можете использовать метод `call` для объединения в цепочку [конструкторов](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/new) объекта, как в Java. В следующем примере для объекта продукта `Product` объявлен конструктор с двумя параметрами, названием `name` и ценой `price`. Продукт инициализирует свойства `name` и `price`, а специализированные функции определяют ещё категорию `category`.

```
function Product(name, price) {
  this.name = name;
  this.price = price;

  if (price < 0) {
    throw RangeError('Нельзя создать продукт ' +
                      this.name + ' с отрицательной ценой');
  }
}

function Food(name, price) {
  Product.call(this, name, price);
  this.category = 'еда';
}

Food.prototype = Object.create(Product.prototype);

function Toy(name, price) {
  Product.call(this, name, price);
  this.category = 'игрушка';
}

Toy.prototype = Object.create(Product.prototype);

var cheese = new Food('фета', 5);
var fun = new Toy('робот', 40);
```

Copy to Clipboard

### [Пример: использование `call` для вызова анонимной функции](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/call#example_using_call_to_invoke_an_anonymous_function "Permalink to Пример: использование call для вызова анонимной функции")

В этом чисто искусственном примере, мы создаём анонимную функцию и используем `call` для вызова её на каждом элементе массива. Главная задача анонимной функции здесь — добавить функцию печати в каждый объект, способную напечатать правильный индекс объекта в массиве. Передача объекта в качестве значения `this` не является острой необходимостью, но мы делаем это в целях объяснения.

```
var animals = [
  { species: 'Лев', name: 'Король' },
  { species: 'Кит', name: 'Фэйл' }
];

for (var i = 0; i < animals.length; i++) {
  (function(i) {
    this.print = function() {
      console.log('#' + i + ' ' + this.species
                  + ': ' + this.name);
    }
    this.print();
  }).call(animals[i], i);
}
```