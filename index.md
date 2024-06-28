---

layout: yandex2

style: |
    #qr-to-tg {
        width: 470px;
    }

    #tg-link a {
        color: black;
    }
---

# ![](themes/yandex2/images/logo-{{ site.presentation.lang }}.svg){:.logo}

## Продвинутый JavaScript
{:.fullscreen}
![](pictures/title.jpeg)

<!-- ## {{ site.presentation.title }}
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
</div> -->

## План лекции

- ...Символы
- ...Тегированные шаблоны
- ...Proxy и Reflextion
- ...Генераторы и итераторы
- ...Полезные методы массивов
- ...Множества и словари
- ...Новые возможности классов
- ...Другие полезные возможности


## <b>Символы</b>
{:.section}


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

### [Symbol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)


## Глобальный реестр символов

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
object[s2] // "🍎"
```
{:.next}

```js
Symbol.keyFor(s1) // "lib.apple"
s1.description // "lib.apple"
```
{:.next}

### [Symbol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol)


## Well-Known Symbols

- ...Symbol.iterator
- ...Symbol.toStringTag
- ...Symbol.hasInstance
- ...Symbol.toPrimitive
- ...Symbol.match, matchAll, replace, search, split
- ......

### ...[Well-Known Symbols](https://262.ecma-international.org/#sec-well-known-symbols)

## Symbol.toStringTag

```js
String({}) // "[object Object]"
```
{:.next}

```js
const apple = {
    [Symbol.toStringTag]: '🍎'
}
```
{:.next}

```js
String(apple) // "[object 🍎]"
```
{:.next}

### ...[Examples of Well-Known Symbols](https://exploringjs.com/js/book/ch_symbols.html#publicly-known-symbols)


## Symbol.hasInstance

```js
const NullInstance = {
    [Symbol.hasInstance](x) {
        return x === null
    }
}
```
{:.next}

```js
null instanceof NullInstance // true
```
{:.next}

### ...[Examples of Well-Known Symbols](https://exploringjs.com/js/book/ch_symbols.html#publicly-known-symbols)

## Где может быть полезно?

- ...Разработка библиотек
- ...Модификация поведения через Well-Known
- ...Разные необычные сценарии использования

### ...[Use cases for symbols](https://exploringjs.com/js/book/ch_symbols.html#use-cases-for-symbols)


## <b>Шаблонные строки<br>Тегированные шаблоны</b>
{:.section}


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

### [Template literals (Template strings)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)


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

### [Template literals (Template strings)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)

## Где может быть полезно?

...Шаблонные строки выполняют простую подстановку

...Тегированные шаблоны выполняют произвольное форматирование

...Могут быть полезны для:
- ...Форматирование значений (преобразование, экранирование и т.д.)
- ...Написания библиотек для работы с Markdown, CSS, SQL и т.д.

### ...[Advanced String Manipulation with Tagged Templates In JavaScript](https://claritydev.net/blog/javascript-advanced-string-manipulation-tagged-templates)


## <b>Proxy и Reflection</b>
{:.section}

## Proxy и Reflection

- ...Proxy — возможность перехватить операцию JavaScript
- ...Reflection — поведение по-умолчанию для перехваченной операции

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

### [Proxy и Reflect](https://learn.javascript.ru/proxy)

## Где может быть полезно?

...Можно перехватить:
- ...Чтение, запись, удаление свойств
- ...Вызов ф-ции, вызов new
- ...и т.д.

...Позволяет реализовать:
- ...Перегрузку операций
- ...Валидацию, кеширование
- ...и т.д.

### ...[A practical guide to Javascript Proxy](https://dev.to/tombarr/a-practical-guide-to-javascript-proxy-4cpa)

## <b>Итераторы и Генераторы</b>
{:.section}


## Терминология

...Генератор — объект, который последовательно вычисляет свои значения

...Итератор — интерфейс для доступа к элементам коллекции

...В JavaScript:
- ...Генераторная ф-ция возвращает генератор
- ...Генератор поддерживает протоколы Iterable и Iterator
- ...Iterable — реализация функции через `Symbol.iterator`
- ...Iterator — реализация объекта с методами `next`, `return`, `throw`

### [Iterable & Iterator Interfaces](https://262.ecma-international.org/#sec-common-iteration-interfaces)

## Генератор — это корутина (сопрограмма)

...**Cooperative concurrently executing routines**

...Корутина — это функция, которая:
- ...приостанавливает работу
- ...запоминает текущее состояние
- ...имеет несколько точек входа и выхода

### [Generator (computer programming)](https://en.wikipedia.org/wiki/Generator_(computer_programming))

## Протоколы Iterable и Iterator

```js
const iterator = ['🍎', '🍏'][Symbol.iterator]()
```
{:.next}

```js
iterator.next() // {value: "🍎", done: false}
iterator.next() // {value: "🍏", done: false}
```
{:.next}

```js
iterator.next() // {value: undefined, done: true}
iterator.next() // {value: undefined, done: true}
```
{:.next}

### [Iteration protocols](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols)

## Генераторная функция

...Возвращает объект с поддержкой протоколов Iterable и Iterator

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

## Делегирование генератора

Генераторы можно делегировать через <b>yield*</b>

```js
function* fruits () {
    yield* apples()
    yield '🍋'
}

const iterator = fruits()
iterator.next() // { value: "🍎", done: false }
iterator.next() // { value: "🍏", done: false }
iterator.next() // { value: "🍋", done: false }
```
{:style="float:left; width: 600px;"}

```js
function* apples () {
    yield '🍎'
    yield '🍏'
}
```
{:.image-right}

## Делегирование генератора и return

При делегирование результат <b>return</b> вернется в <b>yield*</b>

```js
function* fruits () {
    const result = yield* apples()
    yield result
}

const iterator = fruits()
iterator.next() // { value: "🍎", done: false }
iterator.next() // { value: "🍏", done: false }
iterator.next() // { value: undefined, done: true }
```
{:style="float:left; width: 600px;"}

```js
function* apples () {
    yield '🍎'
    return '🍏'
}
```
{:.image-right}


## for-of

...Ожидает, что объект реализует Iterable


```js
function* generator () {
    yield '🍎'
    yield '🍏'
}

const iterator = generator()
```
{:.next}

```js
for (const apple of iterator) {
    console.log(apple) // "🍎" "🍏"
}
```
{:.next}

### [for...of](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of)


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

### [Обработка Unicode в String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/Symbol.iterator)


## Symbol.iterator

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

## Symbol.asyncIterator

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

## Symbol.iterator у встроенных объектов

...<b>Symbol.iterator</b> уже реализован во <b>встроенных</b> объектах

- ...`String`
- ...`Array`, `Set`, `Map`
- ...Псевдо массивы `arguments`, `HTMLCollection` и т.д.

```js
const nodeList = document.querySelectorAll('div')

for (const node of nodeList) {
    console.log(node)
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


## Как превратить итератор в массив?

```js
const set = new Set([4, 8, 15, 16])
```
{:.next}

```js
const array = [...set]
```
{:.next}

```js
const array = Array.from(set)
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


## Взаимодействие через next

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

## Взаимодействие через throw

```js
// где-то в генераторе
try {
    choice = yield "Select fruit" // "🍎"
} catch (e) {
    choice = e
}
yield choice
```
{:.next}

```js
iterator.next() // { value: "Select fruit", done: false }
iterator.next('🍎') // { value: "🍎", done: false }
```
{:.next}

## Взаимодействие через throw

```js
// где-то в генераторе
try {
    choice = yield "Select fruit"
} catch (e) {
    choice = e // "🍏"
}
yield choice
```

```js
iterator.next() // { value: "Select fruit", done: false }
iterator.throw('🍏') // { value: "🍏", done: false }
```


## Взаимодействие через return

```js
function* generator () {
    yield '🍎'
}

const iterator = generator()
```
{:.next}

```js
iterator.return('🍏') // { value: "🍏", done: true }
```
{:.next}

## Где может быть полезно?

- ...Работа с асинхронностью
- ...Ленивые вычисления, загрузка, пагинация
- ...Создание бесконечных последовательностей
- ...Навигация по произвольным коллекциям


## <b>Массивы</b>
{:.section}


## Array.isArray

...Универсальный способ определить массив

```js
Array.isArray([3, 14, 15]) // true
```
{:.next}

```js
const HTMLCollection = document.querySelectorAll('div')
```
{:.next}

```js
Array.isArray(HTMLCollection) // false
```
{:.next}

### ...[instanceof vs. Array.isArray()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/isArray#instanceof_vs._array.isarray)


## Array.from

...Создать настоящий массив из <b>псевдо массива</b>

```js
const HTMLCollection = document.querySelectorAll('div')
```
{:.next}

```js
Array.from(HTMLCollection).slice(0, 3) // [div, div, div]
```
{:.next}

```js
Array.from([3, 14, 15], (x) => x + x) // [6, 28, 30]
```
{:.next}

### [Array.from()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from)


## Array.fromAsync

```js
async function* generator () {
    yield Promise.resolve('🍏')
}

const asyncIterator = generator()
```
{:.next}

```js
Array.fromAsync(asyncIterator).then((result) => {
    console.log(result) // ['🍏']
})
```
{:.next}

### [Array.fromAsync()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/fromAsync)

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
// но конструктор Array, всё ещё может быть полезен
Array(16).fill().map(_ => <Skeleton />)
```
{:.next}

### [Array.of()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/of)


## Доступ к элементам

```js
const array = [4, 8, 15, 16, 23, 42]
```
{:.next}

```js
array[4] // 23
```
{:.next}

```js
array[array.length - 1] // 42
```
{:.next}

```js
[4, 8, 15, 16, 23, 42].at(-1) // 42
```
{:.next}

### [Array.prototype.at()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/at)


## Поиск в массиве

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
['🍎', '🍐', '🍊'].findLast(x => x === '🍊') // "🍊"
```
{:.next}

```js
['🍎', '🍐', '🍊'].findLastIndex(x => x === '🍊') // 2
```
{:.next}

## flat и flatMap

```js
[4, [8, [15, 16]], [23], 42].flat() // [4, 8, Array(2), 23, 42]
```
{:.next}

```js
[4, [8, [15, 16]], [23], 42].flat(2) // [4, 8, 15, 16, 23, 42]
```
{:.next}

```js
// сперва map, затем flat
[4, 8, 15, 16, 23, 42].flatMap(x => [x]) // [4, 8, 15, 16, 23, 42]
```
{:.next}


## Копирующие методы

...toSorted, toReversed, toSpliced, with

```js
const array = ['2', '3', '1']

array.toSorted() // ['1', '2', '3']
array.with(0, '🍏') // ['🍏', '3', '1']
```
{:.next}

```js
console.log(array) // ['2', '3', '1']
```
{:.next}

### [Копирующие методы массивов](https://proghunter.ru/articles/new-array-methods-when-copying-to-javascript-in-ecmascript-2023)<br>[Copying methods and mutating methods](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array#copying_methods_and_mutating_methods)

## <b>Множества и словари</b>
{:.section}


## Множества и словари до ES6

```js
const set = {}

set.bazinga = undefined
set.bazinga // undefined
'bazinga' in set // true
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

...В качестве ключей выступают только <b>ссылки</b> или <b>символы</b>

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

### [Use cases for WeekSet and WeekMap](https://stackoverflow.com/a/29416340)

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

### [Keyed Collections](https://262.ecma-international.org/#sec-structured-data)

## Методы для операций на множествах

- intersection
- union
- difference
- symmetricDifference
- isSubsetOf
- isSupersetOf
- isDisjointFrom
{:.next style="float:left; width: 600px;"}

```js
const a = new Set(['🍏', '🍎', '🍐'])
const b = new Set(['🍐', '🍋'])

a.union(b)
// Set(4) {'🍏', '🍎', '🍐', '🍋'}
```
{:.next .image-right}

### [The JavaScript Set methods](https://web.dev/blog/set-methods)

## <b>Классы</b>
{:.section}

## Приватные свойства и методы

```js
class Awesome {
    publicField = null
    #privateField = null

    constructor (a, b) {
        this.publicField = a
        this.#privateField = b
    }

    #privateMethod () {}
    publicMethod () {}
}
```
{:.next}

## Статические свойства и методы

```js
class Awesome {
    static value = '🍎'
    static printValue () { console.log(this.value) }

    constructor (value) { this.value = value }
    printValue () { console.log(this.value) }
}
```
{:.next}

```js
const instance = new Awesome('🍏')
Awesome.printValue() // 🍎
instance.printValue() // 🍏
```
{:.next}

## Блок статической инициализации

```js
class Awesome {
    static redApple
    static greenApple

    static {
        this.redApple = '🍎'
        this.greenApple = '🍏'
    }
}
```
{:.next}


## <b>Кое-что ещё</b>
{:.section}


## Числовой разделитель и BigInt

```js
const million = 1_000_000
```
{:.next}

```js
BigInt('100500') // 100500n

typeof 100500n // 'bigint'

100 + 100n // TypeError: Cannot mix BigInt and other types
```
{:.next}

### ...[BigInt](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt)

## isNaN vs Number.isNaN

```js
isNaN(NaN) // true
```
{:.next}

```js
isNaN('I am not a NaN') // true
```
{:.next}

```js
Number.isNaN('I am not a NaN') // false
```
{:.next}

```js
Number.parseInt === parseInt // true
```
{:.next}

### [Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)

## Сравнение через Object.is

```js
NaN === NaN // false
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

## Обработка null и undefined

```js
const object = { value: 0 }
```
{:.next}

```js
object.value || 'default' // "default"
```
{:.next}

```js
// Операторы нулевого слияния и присваивания
object.value ?? 'default'
object.value ??= 'default'
```
{:.next}

```js
// Опциональная цепочка (?.)
object?.data?.value
object?.data?.[value]
object?.method?.()
```
{:.next}


## replaceAll и matchAll

```js
'a a a'.replace('a', 'b') // 'b a a'
```
{:.next}

```js
'a a a'.replace(/a/g, 'b') // 'b b b'
```
{:.next}

```js
'a a a'.replaceAll('a', 'b') // 'b b b'
```
{:.next}

```js
const html = '<h1>Something</h1>'
const regex = /<(.*?)>/g
```
{:.next}

```js
html.matchAll(regex) // RegExpStringIterator
Array.from(html.matchAll(regex)) // [Array(2), Array(2)]
```
{:.next}

### ...[replace и replaceAll](https://exploringjs.com/js/book/ch_regexps.html#replace-replaceAll) / [match и matchAll](https://learn.javascript.ru/regexp-methods#str-matchall-regexp)

## globalThis

...Универсальный глобальный контекст

```js
// браузер
globalThis === window // true
```
{:.next}

```js
// воркер
globalThis === self // true
```
{:.next}

```js
// node
globalThis === global // true
```
{:.next}

### ...[In browsers, globalThis does not point directly to the global object](https://exploringjs.com/js/book/ch_variables-assignment.html#globalThis)


## structuredClone

```js
const object = { this: { is: { nested: 'data' } } }

const deepCopy = structuredClone(object)
```
{:.next}

...Ограничения:
- ...Ф-ции
- ...Прототипы
- ...DOM узлы и т.д.

### [Глубокое копирование в JavaScript](https://web.dev/articles/structured-clone)


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


## Object.groupBy и Map.groupBy

```js
const inventory = [
    { name: "🍏", type: "fruit", quantity: 4 },
    { name: "🍎", type: "fruit", quantity: 5 },
    { name: "🥦", type: "vegetables", quantity: 7 },
]
```
{:.next}

```js
Object.groupBy(inventory, ({ type }) => type)
// { fruit: Array(2), vegetables: Array(1) }
```
{:.next}

```js
Map.groupBy(inventory, ({ type }) => type)
// Map(2) { 'fruit' => Array(2), 'vegetables' => Array(1) }
```
{:.next}

### [Object.groupBy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/groupBy) и [Map.groupBy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/groupBy)


## Что ещё почитать?

- ...[Exploring JavaScript](https://exploringjs.com/js/book/index.html)
- ...[Симпсон — Вы не знаете JavaScript](https://github.com/getify/You-Dont-Know-JS)
- ...[Фрисби — JavaScript для профессиональных ... (4-е издание)](https://www.ozon.ru/product/javascript-dlya-professionalnyh-veb-razrabotchikov-4-e-mezhdunarodnoe-izd-frisbi-mett-317133183)
- ...[Флэнаган — JavaScript. Полное руководство (7-е издание)](https://www.ozon.ru/product/javascript-polnoe-rukovodstvo-flenagan-devid-351996284)


## <b>The End</b> 👏
{:.section}

![](pictures/gbiz-dev.png)
{:.image-right#qr-to-tg}

### [http://t.me/GbizDev](http://t.me/GbizDev) (тег #ШРИ)
{:#tg-link}


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
