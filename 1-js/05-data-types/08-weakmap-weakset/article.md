
# WeakMap и WeakSet

`WeakSet` - особый тип множества `Set`, который не мешает внутренним механизмам JavaScript удалить его элементы из памяти. `WeakMap` - то же самое для объекта `Map`.

Как мы знаем из главы <info:garbage-collection>, движок JavaScript хранит значения в памяти до тех пор, пока они достижимы (то есть, эти значения могут быть использованы).

Например:
```js
let john = { name: "John" };

// объект доступен, переменная john -- это ссылка на него

// перепишем ссылку
john = null;

*!*
// объект будет удалён из памяти
*/!*
```

Обычно свойства объекта, элементы массива или другой структуры данных считаются достижимыми и сохраняются в памяти до тех пор, пока эта структура данных содержится в памяти.

Например, если мы поместим объект в массив, то до тех пор, пока массив существует, объект также будет существовать в памяти, несмотря на то, что других ссылок на него нет.

Например:

```js
let john = { name: "John" };

let array = [ john ];

john = null; // перезаписываем ссылку на объект

*!*
// объект john хранится в массиве, поэтому он не будет удалён сборщиком мусора
// мы можем взять его значение как array[0]
*/!*
```

Аналогично, если мы используем объект как ключ в `Map`, то до тех пор, пока существует `Map`, также будет существовать и этот объект. Он занимает место в памяти и не может быть удалён сборщиком мусора.

Например:

```js
let john = { name: "John" };

let map = new Map();
map.set(john, "...");

john = null; // перезаписываем ссылку на объект

*!*
// объект john сохранён внутри объекта `Map`,
// он доступен через map.keys()
*/!*
```

`WeakMap/WeakSet` - принципиально другие в этом аспекте. Они не предотвращают удаление объектов сборщиком мусора, когда эти объекты выступают в качестве ключей.

Давайте посмотрим, что это означает на примере `WeakMap`.

## WeakMap

Первое его отличие от `Map` в том, что ключи в `WeakMap` должны быть объектами, а не примитивными значениями:

```js run
let weakMap = new WeakMap();

let obj = {};

weakMap.set(obj, "ok"); // работает (объект в качестве ключа)

*!*
// нельзя использовать строку в качестве ключа
weakMap.set("test", "Whoops"); // Ошибка, потому что "test" не объект
*/!*
```

Теперь, если мы используем объект в качестве ключа и если больше нет ссылок на этот объект, то он будет удалён из памяти (и из объекта `WeakMap`) автоматически.

```js
let john = { name: "John" };

let weakMap = new WeakMap();
weakMap.set(john, "...");

john = null; // перезаписываем ссылку на объект

// объект john удалён из памяти!
```

Сравните это поведение с поведением обычного `Map`, пример которого был приведён ранее. Теперь `john` существует только как ключ в `WeakMap` и может быть удалён оттуда автоматически.

`WeakMap` не поддерживает перебор и методы `keys()`, `values()`, `entries()`, так что нет способа взять все ключи или значения из него.

В `WeakMap` присутствуют только следующие методы:

- `weakMap.get(key)`
- `weakMap.set(key, value)`
- `weakMap.delete(key)`
- `weakMap.has(key)`

К чему такие ограничения? Из-за особенностей технической реализации. Если объект станет недостижим (как объект `john` в примере выше), то он будет автоматически удалён сборщиком мусора. Но нет информации, *в какой момент произойдет эта очистка*.

Решение о том, когда делать сборку мусора, принимает движок JavaScript. Он может посчитать необходимым как удалить объект прямо сейчас, так и отложить эту операцию, чтобы удалить большее количество объектов за раз позже. Так что технически количество элементов в объекте `WeakMap` не известно. Движок может произвести очистку сразу или потом, или сделать это частично. По этой причине методы для доступа ко всем сразу ключам/значениям не доступны.

Но для чего же нам нужна такая структура данных?

## Пример: дополнительные данные


В основном `WeakMap` используется в качестве *дополнительного хранилища данных*.

В коде могут быть объекты, управляемые из других мест. Они, возможно, относятся к каким-то сторонним библиотекам, и нам нужно хранить о них некоторую информацию, которая актуальна только в период нахождения таких объектов в оперативной памяти.

И когда сборщик мусора удаляет эти объекты из памяти, то ассоциированные с ними данные тоже должны автоматически исчезнуть.

```js
weakMap.set(john, "secret documents");
// если объект john удаляется сборщиком мусора, то "secret documents" тоже автоматически очистится
```

Давайте рассмотрим один пример.

Предположим, у нас есть код, в котором для каждого пользователя ведётся счётчик посещений. Эти данные хранятся во множестве: объект, представляющий пользователя, является ключом, а количество визитов -- значением. Когда пользователь покидает страницу, то его объект удаляется сборщиком мусора, и больше нет смысла хранить соответствующий счётчик посещений.

Вот пример реализации счётчика посещений с использованием `Map`:

```js
// 📁 visitsCount.js
let visitsCountMap = new Map(); // map: пользователь => число визитов

// увеличиваем счётчик
function countUser(user) {
  let count = visitsCountMap.get(user) || 0;
  visitsCountMap.set(count + 1);
}
```

Давайте представим, как мы используем эту функцию в коде:

```js
// 📁 main.js
let john = { name: "John" };

countUser(john); //ведёт подсчет посещений
countUser(john);

// пользователь покинул нас
john = null;
```

И сейчас появилась проблема: объект `john` должен быть удалён сборщиком мусора, но он продолжает оставаться в памяти, как и его счётчик посещений в `visitsCountMap`.

Нам нужно очищать `visitsCountMap` при удалении объекта пользователя, иначе объём занимаемой памяти будет бесконечно увеличиваться. Такая зачистка может стать весьма утомительной задачей в сложных приложениях.

Проблемы можно избежать, если использовать `WeakMap`:

```js
// 📁 visitsCount.js
let visitsCountMap = new WeakMap(); // map: пользователь => число визитов

// увеличиваем счётчик
function countUser(user) {
  let count = visitsCountMap.get(user) || 0;
  visitsCountMap.set(count + 1);
}
```

Сейчас мы не должны самостоятельно очищать `visitsCountMap`. После удаления объекта `john` из памяти вся ассоциированная с ним дополнительная информация будет также удалена из `WeakMap`.

## Применение в кешировании

Another common example is caching: when a function result should be remembered ("cached"), so that future calls on the same object reuse it.

We can use `Map` for it, like this:

```js run
// 📁 cache.js
let cache = new Map();

// calculate and remember the result
function process(obj) {
  if (!cache.has(obj)) {
    let result = /* calculate the result for */ obj;

    cache.set(obj, result);
  }

  return cache.get(obj);
}

*!*
// Usage in another file:
*/!*
// 📁 main.js
let obj = {/* some object */};

let result1 = process(obj); // calculated

// ...later, from another place of the code...
let result2 = process(obj); // taken from cache

// ...later, when the object is not needed any more:
obj = null;

alert(cache.size); // 1 (Ouch! It's still in cache, taking memory!)
```

Now for multiple calls of `process(obj)` with the same object, it only calculates the result the first time, and then just takes it from `cache`. The downside is that we need to clean `cache` when the object is not needed any more.

If we replace `Map` with `WeakMap`, then the cached result will be removed from memory automatically after the object gets garbage collected:

```js run
// 📁 cache.js
*!*
let cache = new WeakMap();
*/!*

// calculate and remember the result
function process(obj) {
  if (!cache.has(obj)) {
    let result = /* calculate the result for */ obj;

    cache.set(obj, result);
  }

  return cache.get(obj);
}

// 📁 main.js
let obj = {/* some object */};

let result1 = process(obj);
let result2 = process(obj);

// ...later, when the object is not needed any more:
obj = null;

// Can't get cache.size, as it's a WeakMap, but it's 0 or soon be 0
// When obj gets garbage collected, cached data will be removed as well
```

## WeakSet

Объект `WeakSet` ведёт себя похоже:

- Он аналогичен объекту `Set`, но мы можем добавлять в `WeakSet` только объекты (не примитивные значения).
- Объект присутствует в множестве только до тех пор, пока доступен где-то ещё.
- Как и `Set`, он поддерживает `add`, `has` и `delete`, но не `size`, `keys()` и не является перебираемым.


Being "weak", it also serves as an additional storage. But not for an arbitrary data, but rather for "yes/no" facts. A membership in `WeakSet` may mean something about the object.

For instance, we can use `WeakSet` to keep track of users that visited our site:

```js run
let visitedSet = new WeakSet();

let john = { name: "John" };
let pete = { name: "Pete" };
let mary = { name: "Mary" };

visitedSet.add(john); // John visited us
visitedSet.add(pete); // Then Pete
visitedSet.add(john); // John again

// visitedSet has 2 users now

// check if John visited?
alert(visitedSet.has(john)); // true

// check if Mary visited?
alert(visitedSet.has(mary)); // false

// John object is not needed any more
john = null;

// visitedSet will be cleaned automatically
```

The most notable limitation of `WeakMap` and `WeakSet` is the absence of iterations, and inability to get all current content. That may appear inconvenient, but does not prevent `WeakMap/WeakSet` from doing their main job -- be an "additional" storage of data for objects which are stored/managed at another place.

## Итого

`WeakMap` is `Map`-like collection that allows only objects as keys and removes them once they become inaccessible by other means.

`WeakSet` is `Set`-like collection that only stores objects and removes them once they become inaccessible by other means.

Both of them do not support methods and properties that refer to all keys or their count. Only individial get/has/set/remove operations with a given key are allowed.

`WeakMap` and `WeakSet` are used as "secondary" data structures in addition to the "main" object storage. Once the object is removed from the main storage, if it is only found as the key of `WeakMap` or in a `WeakSet`, it will be cleaned up automatically.
