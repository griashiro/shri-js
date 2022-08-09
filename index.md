---

layout: yandex2

style: |
    /* собственные стили можно писать здесь!! */
    .slide em {
        font-style: italic;
    }

    .clearfix::after {
        content: ""; /* Генерируем пустой элемент */
        clear: both; /* Отменяем обтекание*/
        display: block; /* Блочный элемент */
    }

---

# ![](themes/yandex2/images/logo-{{ site.presentation.lang }}.svg){:.logo}

## {{ site.presentation.title }}
{:.title}

### ![](themes/yandex2/images/title-logo-{{ site.presentation.lang }}.svg){{ site.presentation.service }}

{% if site.presentation.nda %}
![](themes/yandex2/images/title-nda.svg)
{:.nda}
{% endif %}

<div class="authors">
{% if site.author %}
<p>{{ site.author.name }}{% if site.author.position %}, {{ site.author.position }}{% endif %}</p>
{% endif %}

{% if site.author2 %}
<p>{{ site.author2.name }}{% if site.author2.position %}, {{ site.author2.position }}{% endif %}</p>
{% endif %}

</div>

## План

- ...Типы данных
- ...Метапрограммирование
- ...Прототипное наследование
- ...Контекст
- ...Коллекции
- ...Итераторы и генераторы


## Типы Данных
{:.section}


## Типы Данных

...<b>Типы данных</b> — множество значений и операций над ними

...JavaScript использует <b>два класса</b> данных:

- ...Примитивы
- ...Объекты (ссылки)


## Примитивные типы данных

- ...`boolean`
- ...`string`
- ...`number`
- ...`bigint`
- ...`null`
- ...`undefined`
- ...`symbol`

## Все остальное — объект
{:.blockquote}

## Оператор typeof

Вернет строку, которая укажет тип операнда

```js
typeof 42 // "number"
typeof 100500n // "bigint"
typeof 'JavaScript' // "string"
```
{:.next}

```js
typeof {} // "object"
typeof [] // "object"
```
{:.next}

```js
// две особенности typeof
typeof null // "object"
typeof function () {} // "function"
```
{:.next}

## Методы примитивов

```js
(42).toString(2) // "101010"
```

```js
// JavaScript создаст за кулисами обертку
(new Number(42)).toString(2)
```
{:.next}


## Обертки над примитивами

```js
const number = new Number(42)
const string = new String('JavaScript')
const boolean = new Boolean(false)
```

```js
// Ведут себя странно:
if (boolean) {
    // выполнится, несмотря на false
}
```
{:.next}

```js
// Обертка без new — это функция для преобразования типа
Number('137') // 137
String(42)    // "42"
Boolean(null) // false
```
{:.next}

## Ссылочные типы данных

- ...`Object`
- ...`Array`
- ...`Function`
- ...`RegExp`
- ...`Date`
- ...`Error`
- ...и много-много других


## Объект — это ссылка
{:.blockquote}


## Литералы для ссылочных типов

```js
new Object()
new Array(4, 8, 15, 16, 23, 42)
new Function('a, b', 'return a + b')
new RegExp('\\w', 'g')
```

```js
{}
[4, 8, 15, 16, 23, 42]
function (a, b) { return a + b }
/\w/g
```
{:.next}


## Защищенный конструктор

Большинство встроенных объектов можно вызывать без `new`

```js
new Array(3, 14, 15) // [3, 14, 15]
Array(3, 14, 15)     // [3, 14, 15]
```
{:.next}

```js
// но не Date
typeof Date()     // "string"
typeof new Date() // "object"
```
{:.next}

## Встроенное свойство [[Class]]

```js
const array = []
const date = new Date()
const error = new Error()

Object.prototype.toString.call(array) // "[object Array]"
Object.prototype.toString.call(date)  // "[object Date]"
Object.prototype.toString.call(error) // "[object Error]"
```
{:.next}

## Symbol.toStringTag

```js
Object.prototype.toString.call({}) // "[object Object]"
```
{:.next}

```js
const apple = {
    [Symbol.toStringTag]: '🍎'
}
```
{:.next}

```js
Object.prototype.toString.call(apple) // "[object 🍎]"
```
{:.next}
<!--
## Преобразование типов
{:.section}

- ...Как работает?
- ...Когда срабатывает?


## Как работает
{:.section}

### Часть 2.1


## Виды преобразований

- ...ToBoolean
- ...ToString
- ...ToNumber
- ...ToPrimitive
- ...JSON.stringify

## ToBoolean

```js
Boolean(something)
```

- ...Любой объект всегда `true`
- ...Все остальное тоже `true`, кроме <b>7 ложных значений</b>

## 7 ложных значений

- ...`false`
- ...`null`
- ...`undefined`
- ...`''`
- ...`0`, `-0`, `NaN`


## ToString

```js
String(something)
```

- ...Для примитивов добавит <b>"кавычки"</b>
- ...Для объектов — <b>ToPrimitive</b>

## ToNumber

```js
Number(something)
```

- ...`true`/`false` преобразует в `1`/`0`
- ...Строку <b>попробует</b> конвертировать в число
- ...Для объектов — <b>ToPrimitive</b>


## Преобразование строки в число

- ...<b>Пустая строка</b> — это всегда `0`
- ...<b>Отбросит</b> пробелы и <b>попробует</b> распознать число
- ...Если есть хотя бы один <b>неверный</b> символ — вернет `NaN`

```js
Number(' 42\n') // 42
Number('42px')  // NaN
```
{:.next}

```js
// parseInt, parseFloat распознаю число до первого лишнего символа
parseInt('42px') // 42
```
{:.next}


## Алгоритм ToPrimitive

...Вызывает <b>Symbol.toPrimitive</b>

...Либо использует методы <b>valueOf</b> и <b>toString</b>

- ...Вызвать `valueOf` или `toString`
- ...Если метод вернул <b>примитив</b>, то <b>подставить</b> его в выражение
- ...При необходимости <b>преобразовать</b> примитив к другому типу
- ...Если метод вернул <b>объект</b>, то вызвать <b>другой</b> метод
- ...Второй метод тоже вернул объект? Ошибка!

## Symbol.toPrimitive

...Метод, который принимает одно из <b>трех</b> значений

```js
const object = {
    [Symbol.toPrimitive](hint) {
        switch (hint) {
            case 'string': return 'строка'
            case 'number': return 'число'
            case 'default': return 'нет предпочтений'
        }
    }
}
```
{:.next}


## Алгоритм ToPrimitive без Symbol.toPrimitive

```js
const apple = {
    valueOf () { return '🍎' },
    toString () { return {} }
}
```

```js
'Apple: ' + apple // "Apple: 🍎" (valueOf)
```
{:.next}

```js
String(apple) // "🍎" (toString → valueOf)
```
{:.next}


## JSON.stringify

```js
// Сигнатура
JSON.stringify(value, replacer, space)
```

- ...Попробует вызвать `toJSON`
- ...Свойства: `function`, `undefined`, `symbol`
    - ...В объекте удалятся
    - ...В массиве превратятся в `null`
- ...`NaN`, `Infinity` тоже превратятся `null`
- ...Встроенные объекты без `toJSON` превратятся в `{}`

## JSON.parse
```js
// Сигнатура
JSON.parse(value, reviver)
```

```js
const string = JSON.stringify({ date: new Date })

JSON.parse(string) // { date: "1997-09-23T12:00:00.000Z" }
```
{:.next}

```js
const dateTimeReviver = (key, value) =>
    key === 'date' ? Date.parse(value) : value
```
{:.next}

```js
JSON.parse(string, dateTimeReviver) // { date: 875016000000 }
```
{:.next}


## Когда срабатывает
{:.section}

### Часть 2.2


## Явное и Неявное
{:.blockquote}


## ToBoolean

- ...`Boolean()`
- ...`!!`
- ...`if`, `for`, `while`, `do/while`
- ...Тернарный оператор `? :`
- ...Левая часть `&&` и `||`

## Операторы && и ||

- ...Преобразуют в логический тип <b>левую часть</b>
- ...<b>Вернут</b> либо левое, либо правое значение
- ...Используют короткий цикл вычислений
- ...`&&` старше чем `||`

```js
// && - if условие
isComputed && doSomething()
const data = object && object.data
```
{:.next}

```js
// || - значение по-умолчанию
const data = value || {}
```
{:.next}


## ToString

- ...`String()`
- ...Доступ к полю объекта: `object[value]`
- ...Шаблонные строки: `${value}`


## ToNumber

- ...`Number()`
- ...Математические и битовые операции
- ...Кроме бинарного `+`
    - ...Если один оператор строка,
    - ...второй преобразуется в строку,
    - ...но для объекта сработает `valueOf`


## Необычные примеры преобразований

```js
// преобразование к числу с помощью +
+new Date('9/23/1997') // 874958400000
```
{:.next}

```js
// битовое отрицание: ~x === -(x + 1)
~42 // -43
```
{:.next}

```js
if (~array.indexOf(value)) {
    // выполнится, когда что-то нашлось
}
```
{:.next}

```js
// отбросить дробную часть
3.1415 | 0 // 3
~~3.1415   // 3
```
{:.next}


## Сравнения

- ...Сравнение <b>без преобразования</b>:  `===`, `!==`
- ...`==` и `===` сравнивают объекты <b>по ссылке</b>
- ...`==`, `!=`, `<=`, `>=`, `<`, `>` предпочитают <b>числа</b>
- ...Особый случай: `null == undefined`

### ...[JavaScript: загадочное дело выражения null >= 0](https://habr.com/ru/company/ruvds/blog/337732/) -->

## Что истина, а что ложь?

- ...Любой объект всегда `true`
- ...Все остальное тоже `true`, кроме <b>7 ложных значений</b>

## 7 ложных значений

- ...`false`
- ...`null`
- ...`undefined`
- ...`''`
- ...`0`, `-0`, `NaN`

## NaN

```js
const number = NaN
number === number // false
```


```js
isNaN(NaN)      // true
isNaN('string') // true
```
{:.next}


```js
Object.is(NaN, NaN) // true
```
{:.next}

```js
Object.is(+0, -0) // false
```
{:.next}

<!--
## Особенности symbol

- ...Преобразование к <b>логическому типу</b> — всегда `true`
- ...Явное или неявное преобразование к <b>числу</b> - <b>ошибка</b>
- ...Преобразование к <b>строке</b> — только явно, через `String()` -->


## Метапрограммирование
{:.section}

## Свойства объекта

- ...<b>[[Class]]</b>
- ...<b>[[Prototype]]</b>
- ...<b>[[Extensible]]</b>

```js
const object = {}
Object.preventExtensions(object)
Object.isExtensible(object) // false
```
{:.next}

```js
object.apple = '🍎' // ошибка в 'use strict'
object // {}
```
{:.next}

## Флаги свойств

```js
// Свойства данные
{
    value,
    writable,
    enumerable,
    configurable,
}
```
{:.next style="float:left;" }

```js
// Свойства-аксессоры
{
    get,
    set,
    enumerable,
    configurable,
}
```
{:.next .image-right}

## Свойства-данные

```js
const object = {}

Object.defineProperty(object, 'property', {
    value: '🍎', // свойство
    writable: true,
    enumerable: true,
    configurable: true,
})

object // { property: "🍎" }
```

## Свойства-данные

```js
const object = {}

Object.defineProperty(object, 'method', {
    value: function () {}, // метод
    writable: true,
    enumerable: true,
    configurable: true,
})

object // { method: [Function: value] }
```


## Свойства-аксессоры

```js
const object = {}

Object.defineProperty(object, 'property', {
    get: function () { return this.data },
    set: function (data) { this.data = data},
    enumerable: true,
    configurable: true,
})
```

```js
object.property = '🍎' // вызов set
object.property        // вызов get

object // { property: [Getter/Setter], data: "🍎" }
```
{:.next}


## Свойства-аксессоры

```js
const object = {
    get property () {
        return this.data
    }
    set property (data) {
        this.data = data
    }
}
```

```js
object.property = '🍎' // вызов set
object.property        // вызов get

object // { property: [Getter/Setter], data: "🍎" }
```
{:.next}

## Object.getOwnPropertyDescriptor/s

```js
const object = {apple: '🍎'}
```
{:.next}

```js
Object.getOwnPropertyDescriptor(object, 'apple')
// {value: "🍎", writable: true, enumerable: true, configurable: true}
```
{:.next}

```js
Object.getOwnPropertyDescriptors(object)
// {apple: {value: "🍎", writable: true, enumerable: true, configurable: true}}
```
{:.next}

## Особенности Object.defineProperty

- ...При создании свойства, <b>неуказанные</b> флаги — <b>false</b>
- ...При изменение — только перезапись <b>указанных</b> флагов

```js
const object = {}
```
{:.next}

```js
Object.defineProperty(object, 'apple', { value: '🍎', configurable: true })
Object.getOwnPropertyDescriptor(object, 'object')
// { value: "🍎", writable: false, enumerable: false, configurable: true }
```
{:.next}

```js
Object.defineProperty(object, 'apple', { writable: true })
Object.getOwnPropertyDescriptor(object, 'apple')
// { value: "🍎", writable: true, enumerable: false, configurable: true }
```
{:.next}

## Символы

```js
const s1 = Symbol('apple')
```
{:.next}

```js
const object = {}
object[s1] = '🍎'
object // {Symbol(apple): "🍎"}
```
{:.next}

```js
const s2 = Symbol('apple')
object[s2] // undefined
```
{:.next}

```js
object[s1] // "🍎"
object[s2] = '🍎'
object // {Symbol(apple): "🍎", Symbol(apple): "🍎"}
```
{:.next}

## Глобальный реестр символов

...Реестр символы <b>глобальный</b> для <b>всего окружения</b>

```js
const s1 = Symbol.for('lib.apple')
```
{:.next}

```js
const object = {}
object[s1] = '🍎'
object // {Symbol(lib.apple): "🍎"}
```
{:.next}

```js
const s2 = Symbol.for('lib.apple')
object[s2] // '🍎'
```
{:.next}

```js
Symbol.keyFor(s1) // "lib.apple"
s1.description // "lib.apple"
```
{:.next}

## Встроенные символы

- Symbol.iterator
- Symbol.toStringTag
- Symbol.toPrimitive
- Symbol.species
- Symbol.hasInstance
- Symbol.isConcatSpreadable
- Symbol.unscopables
- Symbol.match, matchAll, replace, search, split
- ......


## Информация о свойствах объекта

- ...<b>in</b> — проверит объект и всю цепочку прототипов
- ...<b>Object.hasOwn</b> — наличие свойства именно в этом объекте
- ...<b>({}).hasOwnProperty</b> — предшественник hasOwn
- ...<b>Object.keys</b> — только перечисляемые свойства
- ...<b>Object.getOwnPropertyNames</b> — все свойства
- ...<b>Object.getOwnPropertySymbols</b> — все символы
- ...<b>Object.getOwnPropertyDescriptors</b> — все дескрипторы

## Proxy и Reflection API

- ...Proxy — возможность перехватить <b>низкоуровневую операцию</b> JavaScript
- ...Reflection API — поведение <b>по-умолчанию</b> для перехваченной операции

```js
const object = {}

const proxy = new Proxy(object, {
    set (trapTarget, key, value, receiver) {
        return Reflect.set(trapTarget, key, value, receiver)
    }
}

proxy.apple = '🍎'
```
{:.next}

## Шаблонные строки

```js
const size = 42;
const style = `width: ${size}px;`
```
{:.next}

```js
console.log(style) // "width: 42px;"
```
{:.next}


## Тегированные шаблоны

```js
function tag (strings, ......values) {
    console.log(strings, values)
    return '🔥'
}
```
{:.next}

```js
tag`width: ${42}px;`
```
{:.next}

```js
["width: ", "px;", raw: Array(2)] [42]
"🔥"
```
{:.next}

```js
String.raw`\n` // "\\n"
```
{:.next}


## Прототипное наследование
{:.section}


## Классы и Объекты
{:.blockquote}

## [[Prototype]]

- ...<b>Служебное</b> свойство для <b>переиспользования кода</b>
- ...Проставляется <b>вручную</b> или при вызове функции с <b>new</b>
- ...Поиск свойства произойдет по <b>цепочке прототипов</b>
- ...У <b>встроенных</b> объектов уже есть своя небольшая <b>иерархия</b>


## ____proto____

...<b>Нестандартное</b> свойство для доступа прямо к <b>[[Prototype]]</b>

```js
const prototype = {question: 42}
const object = {}

object.__proto__ = prototype

object.question // 42
```
{:.next}


## Object.setPrototypeOf

```js
const prototype = {question: 42}
const object = {}

Object.setPrototypeOf(object, prototype)

object.question // 42
```


## Object.getPrototypeOf

```js
const prototype = {question: 42}
const object = {}

Object.setPrototypeOf(object, prototype)
```

```js
const proto = Object.getPrototypeOf(object)

proto === object.__proto__  // true
proto === prototype         // true
```
{:.next}


## Object.create

...Создает объект с <b>выставленным</b> прототипом

```js
const prototype = {question: 42}
const object = Object.create(prototype)

object.question // 42
```
{:.next}

## Динамическое изменение прототипа

...Изменения прототипа <b>сразу</b> влияет на потомков

```js
const prototype = {question: 42}
const object = Object.create(prototype)

object.question // 42
```
{:.next}

```js
prototype.question = 'forty two'
object.question // "forty two"
```
{:.next}

## Затенение свойств прототипа

- ...Свойства в цепочке прототипов доступны <b>только на чтение</b>
- ...При записи создается новое свойство в <b>дочернем объекте</b>

```js
const prototype = {question: 42}
const object = Object.create(prototype)

object.question = 78
```
{:.next}

```js
prototype.question // 42
object.question // 78
```
{:.next}


## Двойственная природа функций

Либо <b>просто функция</b>, либо <b>конструктор</b>

...Функция-конструктор:

- ...Название с <b>Большой Буквы</b> (просто соглашение)
- ...Вызывается <b>только</b> вместе с <b>new</b>
- ...Обращается к <b>this</b>
- ...Использует свойство <b>prototype</b>


## Оператор new

- ...Создается <b>новый объект</b>
- ...Он же назначается в качестве <b>this</b>
- ...Для него же выставляется <b>[[Prototype]]</b>
- ...Функция конструктор возвращает <b>новый объект</b>
- ...При возвращении <b>другого</b> объекта, созданный объект <b>отбросится</b>

## Связывание через new

```js
function Language (name) {
    this.name = name
}
```

```js
new Language('JavaScript') // { name: "JavaScript" }
```
{:.next}

```js
function Language (name) {
    // this = {}
    // this.[[Prototype]] = Language.prototype
    this.name = name
    // return this
}
```
{:.next}


## Связь prototype и [[Prototype]]

...<b>[[Prototype]]</b> указывает на объект в свойстве <b>prototype</b>

```js
const proto = {question: 42}
```
{:.next}

```js
function Class () {}
Class.prototype = proto
```
{:.next}

```js
const object = new Class()
object.question // 42
```
{:.next}

```js
object.__proto__ === Class.prototype // true
```
{:.next}


## Потеря prototype

...<b>[[Prototype]]</b> выставляется в <b>момент вызова new</b>

```js
function Class () {}
Class.prototype = {}

const object = new Class()
object.question // undefined
```
{:.next}

```js
Class.prototype = {question: 42}
object.question // undefined
```
{:.next}

```js
object.__proto__ === Class.prototype // false
```
{:.next}

## Защищенный конструктор

```js
function Constructor () {
    if (typeof new.target === 'undefined') {
        throw new Error('Следует вызывать только с new!')
    }
}
```
{:.next}

```js
function Constructor () {
    if (typeof new.target === 'undefined') {
        return new Constructor()
    }
}
```
{:.next}

## Свойство constructor

- ...Ссылка на <b>конструктор</b>
- ...<b>По-умолчанию</b>, содержится в <b>prototype</b>
- ...Используется для получения класса <b>через экземпляр</b>


## Свойство constructor

```js
function Class () {}
Class.prototype // {constructor: ƒ}
```

```js
Class.prototype.constructor === Class // true
```
{:.next}

```js
const first = new Class()
const second = new first.constructor
```
{:.next}

```js
first.__proto__ === second.__proto__ // true
```
{:.next}

## Потеря constructor

```js
function Class () {}
```

```js
Class.prototype = { getValue: function () {} }
```
{:.next}

```js
Class.prototype.getValue = function () {}
```
{:.next}

```js
Class.prototype = {
    constructor: Class,
    getValue: function () {}
}
```
{:.next}

```js
Object.defineProperty(Class.prototype, 'constructor', {..., enumerable: false})
```
{:.next}


## Иерархия встроенных объектов

```js
const object = {}
const object = Object.create(Object.prototype)
```
{:.next}

```js
{
    [[Prototype]]: null,
    valueOf: ......,
    toString: ......,
    hasOwnProperty: ......,
    propertyIsEnumerable: ......,
    isPrototypeOf: ......,
}
```
{:.next}

```js
const hashTable = Object.create(null)
```
{:.next}


## instanceof

...Просматривает цепочку <b>прототипов</b>

```js
[] instanceof Array // true
```
{:.next}

```js
[] instanceof Object // true
```
{:.next}

```js
// прототипы в разных фреймах — разные
[] instanceof window.frames[0].Array // false
```
{:.next}


## ООП в JavaScript

- ...<b>Прототипное наследование</b> — это <b>must have</b>
- ...Но ООП через прототипы — это <b>боль</b> и <b>страдания</b>

## Примеси (mixins)

```js
const base = {
    getFruit () {
        return this.fruit
    }
}

const apple = { fruit: '🍎' }
```
{:.next}

```js
Object.assign(apple, base)
```
{:.next}

```js
apple.getFruit() // "🍎"
```
{:.next}

## Классы: свойства и методы

```js
class ES6Class {
    #private
    public = '🍿'

    constructor (value) { this.#private = value }

    #format (...value) { console.log('ES6:', ...value) }

    print () { this.#format(this.#private, this.public) }
}
```
{:.next}


```js
const instance = new ES6Class('🍏')
instance.print() // "ES6: 🍏"
```
{:.next}

## Классы: статические свойства и методы

```js
class ES6Class {
    static value = '🍎'
    static getValue () { console.log(this.value) }

    constructor (value) { this.value = value }

    getValue() { console.log(this.value) }
}
```
{:.next}

```js
const instance = new ES6Class('🍏')
```
{:.next}

```js
ES6Class.getValue() // "🍎"
instance.getValue() // "🍏"
```
{:.next}

## Классы: наследование


```js
class Base {
    constructor (value) { this.value = value }
    getValue () { return this.value }
}
```
{:.next}

```js
class ES6Class extends Base {
    constructor (value) { super(value) }
    getValue () { console.log('👉', super.getValue()) }
}
```
{:.next}

```js
const instanсe = new ES6Class(42)
instanсe.getValue() // "👉 42"
```
{:.next}

## Особенности классов

- ...Вызываются только вместе с <b>new</b>
- ...<b>Не всплывают</b>, в отличие от функций

```js
function ES5Class () {}
class ES6Class {}

ES5Class()
ES6Class() // TypeError: ... cannot be invoked without 'new'
```
{:.next}

```js
class Child extends Parent {} // ReferenceError: ... before initialization
class Parent {}
```
{:.next}


## Классы ES6 — это синтаксический сахар

```js
class SyntaxSugar extends Base {}
```
{:.next}

```js
function Base () {}

Base.prototype.getValue = function () {
    console.log(this.value)
}
```
{:.next}

```js
SyntaxSugar.prototype.value = 42
```
{:.next}

```js
new SyntaxSugar().getValue() // "42"
```
{:.next}

```js
typeof SyntaxSugar // "function"
```
{:.next}


## Контекст
{:.section}


## Контекст

...this — способ <b>связать</b> функцию с объектом

...Способы связывания:

- ...По-умолчанию
- ...Неявное связывание
- ...Явное связывание
- ...Связывание через new


## Связывание по-умолчанию

...this — <b>глобальный</b> объект или <b>undefined</b>

```js
globalThis.value = 42

function something () {
    return this.value
}
```
{:.next}

```js
something() // 42
```
{:.next}

```js
'use strict'
something() // TypeError: Cannot read property 'value' of undefined
```
{:.next}

## globalThis

...Универсальный глобальный контекст

```js
// браузер
globalThis === window // true
```
{:.next}

```js
// node
globalThis === global // true
```
{:.next}

### ...[Есть особенности в браузерах из-за соображений безопасности](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/globalThis)

## Неявное связывание

...<b>Одновременный</b> доступ и вызов метода

```js
const object = {
    value: 42,
    getValue: function () { return this.value }
}
```
{:.next}

```js
object.getValue() // 42
```
{:.next}

```js
// неявная потеря this
const method = object.getValue
method() // undefined
```
{:.next}


## Явное связывание

Использование <b>call</b>, <b>apply</b>, <b>bind</b>

```js
const object = {
    value: 42,
    getValue: function () { return this.value }
}
```
{:.next}

```js
const method = object.getValue
method.call(object) // 42
```
{:.next}

## call/apply/bind

```js
const fruit = {
    init: function (name, emoji) {
        this.name = name
        this.emoji = emoji
    }
}
```

```js
const method = fruit.init
```
{:.next}

```js
method.call(fruit, 'apple', '🍎')
method.apply(fruit, ['apple', '🍎'])
method.bind(fruit)('apple', '🍎')
```
{:.next}

## Приоритетность связываний

- ...Связывание через new
- ...Явное связывание
- ...Неявное связывание
- ...По-умолчанию


```js
const object = {
    method: function () { console.log(this) }
}

object.method() // {method: ƒ}
new object.method() // method {}
```
{:.next}


## Подходы для работы с контекстом

- ...Принять <b>this</b> и использовать <b>bind</b>, где необходимо
- ...Использовать лексическую область видимости: <b>self</b> и <b>() => {}</b>

## Проблема потери контекста

В callback происходит <b>потеря this</b>

```js
const object = {
    value: 42,
    method: function () {
        const callback = function () { console.log(this.value) }
        setTimeout(callback, 200)
    }
}

object.method() // undefined
```

## Проблема потери контекста
<!-- {:.fullscreen} -->

Использование <b>bind</b>

```js
const object = {
    value: 42,
    method: function () {
        const callback = function () { console.log(this.value) }
        setTimeout(callback.bind(this), 200)
    }
}

object.method() // 42
```

## Проблема потери контекста

Лексическое окружение: <b>self</b>

```js
const object = {
    value: 42,
    method: function () {
        const self = this
        const callback = function () { console.log(self.value) }
        setTimeout(callback, 200)
    }
}

object.method() // 42
```

## Проблема потери контекста

Лексическое окружение: <b>() => {}</b>

```js
const object = {
    value: 42,
    method: function () {
        const callback = () => { console.log(this.value) }
        setTimeout(callback, 200)
    }
}

object.method() // 42
```


## Коллекции
{:.section}


## Array.isArray

...Универсальный способ <b>определить</b> массив

```js
Array.isArray([3, 14, 15]) // true
```
{:.next}

```js
const HTMLCollection = document.querySelectorAll('div')
Array.isArray(HTMLCollection) // false
```
{:.next}

## Array.from

...Создать настоящий массив из <b>псевдомассива</b>

```js
const HTMLCollection = document.querySelectorAll('div')
```
{:.next}

```js
// кража метода
[].slice.call(HTMLCollection, 0, 3) // [div, div, div]
```
{:.next}

```js
Array.from(HTMLCollection).slice(0, 3) // [div, div, div]
```
{:.next}


## Array.of

...Функция для создания и инициализации нового массива

```js
Array(3) // [empty × 3]
Array(4, 8, 15) // [4, 8, 15]
```
{:.next}

```js
Array.of(3) // [3]
Array.of(4, 8, 15) // [4, 8, 15]
```
{:.next}

```js
[3] // [3]
[4, 8, 15] // [4, 8, 15]
```
{:.next}

## Удобные методы массивов

```js
['🍎', '🍐', '🍊'].includes('🍎') // true
```
{:.next}

```js
['🍎', '🍐', '🍊'].find(x => x === '🍊') // "🍊"
```
{:.next}

```js
['🍎', '🍐', '🍊'].findIndex(x => x === '🍊') // 2
```
{:.next}

```js
[4, [8, [15, 16]], [23], 42].flat() // [4, 8, Array(2), 23, 42]
```
{:.next}

```js
[4, [8, [15, 16]], [23], 42].flat(2) // [4, 8, 15, 16, 23, 42]
```
{:.next}

```js
[4, 8, 15, 16, 23, 42].flatMap(x => [x]) // [4, 8, 15, 16, 23, 42]
```
{:.next}

```js
[4, 8, 15, 16, 23, 42].at(-1) // 42
```
{:.next}

## Проверка на null и undefined

```js
const object = { value: 0 }
```
{:.next}

```js
object.value || 'default' // "default"
```
{:.next}

```js
// Оператор нулевого слияния (??)
object.value ?? 'default' // 0
```
{:.next}

```js
// Опциональная цепочка (?.)
object?.data?.value
object?.data?.[value]
object?.method?.()
```
{:.next}

## Множества и словари до ES6

...У обычных объектов ключом можем быть только <b>строка</b>

```js
const set = {}

set.bazzinga = undefined
set.bazzinga // undefined
'bazzinga' in set // true
```
{:.next style="float:left; width: 600px;"}

```js
const map = {}

const apple = {}
const orange = {}

map[apple] = '🍎'
map[orange] = '🍊'
map // {[object Object]: "🍊"}
```
{:.next.image-right}

## Set и Map

...Ключом можем быть любой <b>примитив</b> или <b>ссылка</b>

```js
const set = new Set()

const object = {}
set.add(object)

set.has({}) // false
set.has(object) // true
```
{:.next style="float:left; width: 600px;"}

```js
const map = new Map()

const apple = {}
const orange = {}

map.set(apple, '🍎')
map.set(orange, '🍊')

map.get(apple) // "🍎"
map.get(orange) // "🍊"
```
{:.next .image-right}

## WeakSet и WeakMap

...В качестве ключей выступают только <b>ссылки</b>

```js
const map = new WeakMap()

let elem = document.querySelector('.button')
map.set(elem, {data: '...'})
```
{:.next}

```js
map.get(elem) // {data: "..."}
```
{:.next}

```js
elem.parentNode.removeChild(elem)
elem = null
// в этой точке ассоциативный массив со слабыми ссылками оказывается пустым
```
{:.next}

## Порядок свойств

Объекты, множества, словари — это <b>упорядоченные</b> коллекции

```js
const object = {}

object['78'] = 0
object['one'] = 0
object['42'] = 0
object['two'] = 0

console.log(Object.keys(object))
// [ '42', '78', 'one', 'two' ]
```
{:.next}
{:style="float:left; width: 600px;"}

```js
const set = new Set()

set.add('78')
set.add('one')
set.add('42')
set.add('two')

console.log(...set.keys())
// [ '78', 'one', '42', 'two' ]
```
{:.next}
{:.image-right}


## Итераторы и Генераторы
{:.section}


## Философия

- ...Итератор — <b>интерфейс</b> с методом <b>next</b> и признаком <b>done</b>
- ...Генератор — <b>подход</b>, когда вычисляется только следующий элемент

## Итератор в JavaScript

...Это штука с методом <b>next</b>, которая возвращает `{ value, done }`

```js
const iterator = ['🍎', '🍏'][Symbol.iterator]()
```
{:.next}

```js
iterator.next() // {value: "🍎", done: false}
```
{:.next}

```js
iterator.next() // {value: "🍏", done: false}
```
{:.next}
```js
iterator.next() // {value: undefined, done: true}
```
{:.next}
```js
iterator.next() // {value: undefined, done: true}
```
{:.next}

## Генератор в JavaScript

...Штука, которая <b>возвращает итератор</b>

```js
function* generator () {
    yield '🍎'
    yield '🍏'
}
```
{:.next}

```js
const iterator = generator()
```
{:.next}

```js
iterator.next() // {value: "🍎", done: false}
iterator.next() // {value: "🍏", done: false}
iterator.next() // {value: undefined, done: true}
```
{:.next}

## for-of

...Синтаксический сахар для вызова интерфейса итератора

```js
for (const apple of iterator) {
    console.log(apple) // "🍎" "🍏"
}
```
{:.next}

## Интерфейс итератора — Symbol.iterator

...<b>Symbol.iterator</b> позволяет определить интерфейс итератора

```js
const object = {
    *[Symbol.iterator] () {
        yield '🍎'
        yield '🍏'
    }
}
```
{:.next}

```js
for (const apple of object) {
    console.log(apple) // "🍎" "🍏"
}
```
{:.next}

## Symbol.asyncIterator — async интерфейс

```js
const object = {
    async *[Symbol.asyncIterator] () {
        yield '🍎'
        yield '🍏'
    }
}

```
{:.next}

```js
for await (const apple of object) {
    console.log(apple) // "🍎" "🍏"
}
```
{:.next}

## Symbol.iterator из коробки

...<b>Symbol.iterator</b> уже реализован во <b>встроенных</b> объектах

- ...`String`
- ...`Array`, `Set`, `Map`
- ...Псевдомассивах `arguments`, `HTMLCollection` и т.д.

```js
const nodeList = document.querySelectorAll('div')

for (const node of nodeList) {
    console.log(node)
}
```
{:.next}

## Использует символы, вместо кодовых единиц

...Полная поддержка <b>Unicode</b>

```js
const string = '🎉'
```
{:.next}

```js
for (let i = 0; i < string.length; ++i) {
    console.log(string[i]) // что-то не то, причем два раза...
}
```
{:.next}

```js
for (const char of string) {
    console.log(char) // "🎉"
}
```
{:.next}

## keys, values, entries для коллекций

- ...`Object.keys`, `Object.values`, `Object.entries` — вернут <b>массив</b>
- ...Методы коллекций `keys`, `values`, `entries` — вернут <b>итератор</b>

```js
Object.keys({'🍎': 'red'}) // [ "🍎" ]
```
{:.next}

```js
const map = new Map([ ['🍎', 'red'] ])
map.keys() // MapIterator {"🍎"}
```
{:.next}

## Итератор по-умолчанию

- ...<b>Array</b>, <b>Set</b> — `values`
- ...<b>Map</b> — `entries`

```js
for (const [key, value] of map.entries()) {
    // ...
}
```
{:.next}

```js
for (const [key, value] of map) {
    // ...
}
```
{:.next}


## Object.fromEntries

```js
const object = {red: '🍎', green: '🍏'}
const entries = Object.entries(object)
entries // [[ "red", "🍎" ], [ "green", "🍏" ]]
```
{:.next}

```js
Object.fromEntries(entries) // {red: "🍎", green: "🍏"}
```
{:.next}

```js
const map = new Map(entries)
Object.fromEntries(map) // {red: "🍎", green: "🍏"}
```
{:.next}


## Как превратить итератор в массив?

```js
const set = new Set([4, 8, 15, 16])
```
{:.next}
```js
const array = [...set]
```
{:.next}


## Итератор is Корутина
{:.blockquote}

## Итератор — это корутина (сопрограмма)

...**Cooperative concurrently executing routines**

...Корутина — это функция, которая:
- ...приостанавливает работу
- ...запоминает текущее состояние
- ...имеет несколько точек входа и выхода

## Общение итератора с внешним миром

...Через <b>аргумент</b> в методе <b>next</b>

```js
function* generator () {
    const value = yield '🍎'
    yield 42 + value
}
```
{:.next}

```js
const iterator = generator()
```
{:.next}

```js
iterator.next()   // { value: "🍎", done: false }
iterator.next(17) // { value: 59, done: false }
iterator.next()   // { value: undefined, done: true }
```
{:.next}

## Общение итератора с внешним миром

...Через <b>аргумент</b> в методе <b>throw</b>

```js
// где-то в генераторе
try {
    choice = yield "It's time to choose..." // "🍎"
} catch (e) {
    choice = e
}
yield choice
```
{:.next}

```js
// текущая реальность
iterator.next() // { value: "It's time ...", done: false }
iterator.next('🍎') // { value: "🍎", done: false }
```
{:.next}

## Общение итератора с внешним миром

Через <b>аргумент</b> в методе <b>throw</b>

```js
// где-то в генераторе
try {
    choice = yield "It's time to choose..."
} catch (e) {
    choice = e // "🍏"
}
yield choice
```

```js
// альтернативная реальность
iterator.next() // { value: "It's time ...", done: false }
iterator.throw('🍏') // { value: "🍏", done: false }
```

## return завершает генератор

```js
function* generatorA () {
    yield '🍏'
}
```
{:.next}

```js
iteratorA.next() // { value: "🍏", done: false }
iteratorA.next() // { value: undefined, done: true }
```
{:.next}

```js
function* generatorB () {
    return '🍏'
}
```
{:.next}

```js
iteratorB.next() // { value: "🍏", done: true }
```
{:.next}

## Делегирование генераторов

Генераторы можно делегировать через <b>yield*</b>

```js
function* apples () {
    yield '🍎'
    yield '🍏'
}

const iterator = fruits()
iterator.next() // { value: "🍎", done: false }
iterator.next() // { value: "🍏", done: false }
iterator.next() // { value: "🍋", done: false }
```
{:style="float:left; width: 600px;"}

```js
function* fruits () {
    yield* apples()
    yield '🍋'
}
```
{:.image-right}

## Делегирование генераторов и return

При делегирование результат <b>return</b> вернется в <b>yield*</b>

```js
function* apples () {
    yield '🍎'
    return '🍏'
}

const iterator = fruits()
iterator.next() // { value: "🍎", done: false }
iterator.next() // { value: "🍏", done: false }
iterator.next() // { value: undefined, done: true }
```
{:style="float:left; width: 600px;"}

```js
function* fruits () {
    const result = yield* apples()
    yield result
}
```
{:.image-right}


<!--
## Модули
{:.section}

## Модули до начала времен

...Давным давно, были только <b>IIFE</b> и <b>замыкания</b>

```js
const module = (function (fruits) {
    function _wash (fruit) { /* тщательно моем фрукт */ }
    return {
        addApple: function (apple) {
            fruits.push(_wash(apple))
        }
    }
}(['🍎', '🍐', '🍊']))

module.addApple('🍏')
```
{:.next}

## CommonJS

...Система модулей в <b>NodeJS</b>

```js
// module.js
module.exports = {
    apple: '🍎'
}

// program.js
const {apple} = require('./module')

console.log(apple) // "🍎"
```
{:.next}

## ES6 модули

- ...Скорее всего, потребуется <b>сборщик</b>
- ...Совместимы с <b>CommonJS</b>
- ...Работают в <b>use strict</b>
- ...Исполняются <b>один раз</b> при импорте

### [Понимание (всех) «модульных» форматов](https://habr.com/ru/post/501198/)
{:.next}

## export

...Можем экспортировать <b>переменную</b>, <b>функцию</b> или <b>класс</b>

```js
export const object = {}

export function executable () {}

export class Class {}

export { Apples, Oranges }
```
{:.next}

## import

...Можем импортировать <b>часть свойств</b> или <b>весь модуль</b> целиком

```js
import { useState, useEffect } from 'react'

useState()
useEffect()
```
{:.next}

```js
import * as React from 'react'

React.useState()
React.useEffect()
```
{:.next}

## as (переименование)

```js
// внутри модуля
export apples as oranges
export { pears as lemons }
```
{:.next}

```js
// внутри модуля
import { oranges as apples, lemons as pears } from 'module'
```
{:.next}

## default

- ...Внутри модуля может быть только <b>один default</b>
- ...Позволяет экспортировать <b>анонимные</b> объекты

```js
// внутри модуля
export default () => { /* ... */ }
export { something as default }
```
{:.next}

```js
// внутри программы
import React from 'react'
import React, { useState, useEffect } from 'react'
import { default as React, useState, useEffect} from 'react'

```
{:.next}

## import без привязки

...Выполняется ради <b>побочных эффектов</b>

```js
// импорт стилей
import 'style.css'
```
{:.next}

```js
// импорт модулей, изменяющих глобальный объект
import 'es6-polyfill.js'
```
{:.next}

## Реэкспорт зависимостей

...Когда требуется <b>передать дальше</b> часть зависимостей

```js
export { Component } from './Component'
```
{:.next}

## Функция import

...Загрузка модуля в <b>момент исполнения</b> программы

```js
const promise = import('./bundle.js')

promise
    .then((bundle) => {
        const { render } = bundle
        // ...
    })
    .catch((error) => {
        console.error('Ошибка загрузки модуля')
    })
```
{:.next}

 -->

## Рекомендации
{:.section}

## Что читать?

- ...[Фрисби — JavaScript для профессиональных ... (4-е издание)](https://www.ozon.ru/product/javascript-dlya-professionalnyh-veb-razrabotchikov-4-e-mezhdunarodnoe-izd-frisbi-mett-317133183)
- ...[Флэнаган — JavaScript. Полное руководство (7-е издание)](https://www.ozon.ru/product/javascript-polnoe-rukovodstvo-flenagan-devid-351996284)
- ...[Симпсон — Вы не знаете JS](https://github.com/getify/You-Dont-Know-JS)
- ...[JavaScript for impatient programmers](https://exploringjs.com/impatient-js/toc.html)

...*Дополнительно:*
- ...[JS is Weird](https://jsisweird.com/)
- ...[ECMAScript proposals](https://github.com/tc39/proposals)
- ...[JavaScript Utilities in single line of code](https://1loc.dev/)

## Ответы на вопросы 👏
{:.section}


<!-- ## Контакты
{:.contacts}

{% if site.author %}

<figure markdown="1">

### {{ site.author.name }}

{% if site.author.position %}
{{ site.author.position }}
{% endif %}

</figure>

{% endif %}

{% if site.author2 %}

<figure markdown="1">

### {{ site.author2.name }}

{% if site.author2.position %}
{{ site.author2.position }}
{% endif %}

</figure>

{% endif %} -->

<!-- разделитель контактов -->
<!-- ------- -->
<!-- - {:.telegram}@author -->
