# Экспорт и импорт

<<<<<<< HEAD
Директивы экспорт и импорт имеют несколько вариантов вызова.
=======
Export and import directives have several syntax variants.
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca

В предыдущей главе мы видели простое использование, давайте теперь посмотрим больше примеров.

## Экспорт до объявления

Мы можем пометить любое объявление как экспортируемое, разместив `export` перед ним, будь то переменная, функция или класс.

Например, все следующие экспорты допустимы:

```js
// экспорт массива
*!*export*/!* let months = ['Jan', 'Feb', 'Mar','Apr', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];

// экспорт константы
*!*export*/!* const MODULES_BECAME_STANDARD_YEAR = 2015;

// экспорт класса
*!*export*/!* class User {
  constructor(name) {
    this.name = name;
  }
}
```

````smart header="Не ставится точка с запятой после экспорта класса/функции"
Обратите внимание, что `export` перед классом или функцией не делает их [функциональным выражением](info:function-expressions-arrows). Это всё также объявление функции, хотя и экспортируемое.

<<<<<<< HEAD
Большинство руководств по стилю кода в JavaScript не рекомендуют ставить точку с запятой после объявлений функций или классов.

Поэтому в конце `export class` и `export function` не нужна точка с запятой:
=======
Most JavaScript style guides don't recommend semicolons after function and class declarations.

That's why there's no need for a semicolon at the end of `export class` and `export function`:
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca

```js
export function sayHi(user) {
  alert(`Hello, ${user}!`);
} *!* // без ; в конце */!*
```

````

## Экспорт отдельно от объявления

Также можно написать `export` отдельно.

Здесь мы сначала объявляем, а затем экспортируем:

```js  
// 📁 say.js
function sayHi(user) {
  alert(`Hello, ${user}!`);
}

function sayBye(user) {
  alert(`Bye, ${user}!`);
}

*!*
export {sayHi, sayBye}; // список экспортируемых переменных
*/!*
```

...Или, технически, мы также можем расположить `export` выше функций.

## Импорт *

<<<<<<< HEAD
Обычно мы располагаем список того, что хотим импортировать, в фигурных скобках `import {...}`, например вот так:
=======
Usually, we put a list of what to import in curly braces `import {...}`, like this:
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca

```js
// 📁 main.js
*!*
import {sayHi, sayBye} from './say.js';
*/!*

sayHi('John'); // Hello, John!
sayBye('John'); // Bye, John!
```

<<<<<<< HEAD
Но если импортировать нужно много чего, мы можем импортировать всё сразу в виде объекта, используя `import * as <obj>`. Например:
=======
But if there's a lot to import, we can import everything as an object using `import * as <obj>`, for instance:
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca

```js
// 📁 main.js
*!*
import * as say from './say.js';
*/!*

say.sayHi('John');
say.sayBye('John');
```

На первый взгляд "импортировать всё" выглядит очень удобно, не надо писать лишнего, зачем нам вообще может понадобиться явно перечислять список того, что нужно импортировать?

Для этого есть несколько причин.

1. Современные инструменты сборки ([webpack](http://webpack.github.io) и другие) собирают модули вместе и оптимизируют их, ускоряя загрузку и удаляя неиспользуемый код.

<<<<<<< HEAD
    Предположим, мы добавили в наш проект стороннюю библиотеку `say.js` с множеством функций:
=======
    Let's say, we added a 3rd-party library `say.js` to our project with many functions:
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca
    ```js
    // 📁 say.js
    export function sayHi() { ... }
    export function sayBye() { ... }
    export function becomeSilent() { ... }
    ```

<<<<<<< HEAD
    Теперь, если из этой библиотеки в проекте мы используем только одну функцию:
=======
    Now if we only use one of `say.js` functions in our project:
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca
    ```js
    // 📁 main.js
    import {sayHi} from './say.js';
    ```
<<<<<<< HEAD
    ...Тогда оптимизатор увидит, что другие функции не используются, и удалит остальные из собранного кода, тем самым делая код меньше. Это называется "tree-shaking".

2. Явно перечисляя то, что хотим импортировать, мы получаем более короткие имена функций: `sayHi()` вместо `say.sayHi()`.
3. Явное перечисление импортов делает код более понятным, позволяет увидеть, что именно и где используется. Это упрощает поддержку и рефакторинг кода.
=======
    ...Then the optimizer will see that and remove the other functions from the bundled code, thus making the build smaller. That is called "tree-shaking".

2. Explicitly listing what to import gives shorter names: `sayHi()` instead of `say.sayHi()`.
3. Explicit list of imports gives better overview of the code structure: what is used and where. It makes code support and refactoring easier.
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca

## Импорт "как"

Мы также можем использовать `as`, чтобы импортировать под другими именами.

<<<<<<< HEAD
Например, для краткости импортируем `sayHi` в локальную переменную `hi`, а  `sayBye` импортируем как `bye`:
=======
For instance, let's import `sayHi` into the local variable `hi` for brevity, and import `sayBye` as `bye`:
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca

```js
// 📁 main.js
*!*
import {sayHi as hi, sayBye as bye} from './say.js';
*/!*

hi('John'); // Hello, John!
bye('John'); // Bye, John!
```

## Экспортировать "как"

Аналогичный синтаксис существует и для `export`.

Давайте экспортируем функции, как `hi` и `bye`:

```js
// 📁 say.js
...
export {sayHi as hi, sayBye as bye};
```

<<<<<<< HEAD
Теперь `hi` и `bye` -- официальные имена для внешнего кода, их нужно использовать при импорте:
=======
Now `hi` and `bye` are official names for outsiders, to be used in imports:
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca

```js
// 📁 main.js
import * as say from './say.js';

say.*!*hi*/!*('John'); // Hello, John!
say.*!*bye*/!*('John'); // Bye, John!
```

## Экспорт по умолчанию

<<<<<<< HEAD
На практике модули встречаются в основном одного из двух типов:

1. Модуль, содержащий библиотеку или набор функций, как `say.js` выше.
2. Модуль, который объявляет что-то одно, например модуль `user.js` экспортирует только `class User`.
=======
In practice, there are mainly two kinds of modules.

1. Module that contains a library, pack of functions, like `say.js` above.
2. Module that declares a single entity, e.g. a module `user.js` exports only `class User`.
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca

По большей части, удобнее второй подход, когда каждая "вещь" находится в своём собственном модуле.

Естественно, требуется много файлов, если для всего делать отдельный модуль, но это не проблема. Так даже удобнее: навигация по проекту становится проще, особенно, если у файлов хорошие имена, и они структурированы по папкам.

<<<<<<< HEAD
Модули предоставляют специальный синтаксис `export default` ("эспорт по умолчанию") для второго подхода.

Ставим `export default` перед тем, что нужно экспортировать:
=======
Modules provide special `export default` ("the default export") syntax to make "one thing per module" way look better.

Put `export default` before the entity to export:
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca

```js
// 📁 user.js
export *!*default*/!* class User { // просто добавьте "default"
  constructor(name) {
    this.name = name;
  }
}
```

<<<<<<< HEAD
Заметим, в файле может быть не более одного `export default`.

...И потом импортируем без фигурных скобок:
=======
There may be only one `export default` per file.

...And then import it without curly braces:
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca

```js
// 📁 main.js
import *!*User*/!* from './user.js'; // не {User}, просто User

new User('John');
```

<<<<<<< HEAD
Импорты без фигурных скобок выглядят красивее. Обычная ошибка начинающих: забывать про фигурные скобки. Запомним: фигурные скобки необходимы в случае именованных экспортов, для `export default` они не нужны.
=======
Imports without curly braces look nicer. A common mistake when starting to use modules is to forget curly braces at all. So, remember, `import` needs curly braces for named exports and doesn't need them for the default one.
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca

| Именованный экспорт | Экспорт по умолчанию |
|--------------|----------------|
| `export class User {...}` | `export default class User {...}` |
| `import {User} from ...` | `import User from ...`|

<<<<<<< HEAD
Технически в одном модуле может быть как экспорт по умолчанию, так и именованные экспорты, но на практике обычно их не смешивают. То есть, в модуле находятся либо именованные экспорты, либо один экспорт по умолчанию.

Так как в файле может быть максимум один `export default`, то экспортируемая сущность не обязана иметь имя.
=======
Technically, we may have both default and named exports in a single module, but in practice people usually don't mix them. A module has either named exports or the default one.

As there may be at most one default export per file, the exported entity may have no name.
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca

Например, всё это -- полностью корректные экспорты по умолчанию:

```js
export default class { // у класса нет имени
  constructor() { ... }
}
```

```js
export default function(user) { // у функции нет имени
  alert(`Hello, ${user}!`);
}
```

```js
// экспортируем значение, не создавая переменную
export default ['Jan', 'Feb', 'Mar','Apr', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];
```

<<<<<<< HEAD
Это нормально, потому что может быть только один `export default` на файл, так что `import` без фигурных скобок всегда знает, что импортировать.

Без `default` такой экспорт выдал бы ошибку:
=======
Not giving a name is fine, because `export default` is only one per file, so `import` without curly braces knows what to import.

Without `default`, such export would give an error:
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca

```js
export class { // Ошибка! (необходимо имя, если это не экспорт по умолчанию)
  constructor() {}
}
```     

<<<<<<< HEAD
### Имя "default"

В некоторых ситуациях для обозначения экспорта по умолчанию в качестве имени используется `default`.

Например, чтобы экспортировать функцию отдельно от её объявления:
=======
### The "default" name

In some situations the `default` keyword is used to reference the default export.

For example, to export a function separately from its definition:
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca

```js
function sayHi(user) {
  alert(`Hello, ${user}!`);
}

<<<<<<< HEAD
// то же самое, как если бы мы добавили "export default" перед функцией
export {sayHi as default};
```

Или, ещё ситуация, давайте представим следующее: модуль `user.js` экспортирует одну сущность "по умолчанию" и несколько именованных (редкий, но возможный случай):
=======
// same as if we added "export default" before the function
export {sayHi as default};
```

Or, another situation, let's say a module `user.js` exports one main "default" thing and a few named ones (rarely the case, but happens):
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca

```js
// 📁 user.js
export default class User {
  constructor(name) {
    this.name = name;
  }
}

export function sayHi(user) {
  alert(`Hello, ${user}!`);
}
```

Вот как импортировать экспорт по умолчанию вместе с именованным экспортом:

```js
// 📁 main.js
import {*!*default as User*/!*, sayHi} from './user.js';

new User('John');
```

<<<<<<< HEAD
И, наконец, если мы импортируем всё как объект `import *`, тогда его свойство `default` - как раз и будет экспортом по умолчанию:
=======
And, finally, if importing everything `*` as an object, then the `default` property is exactly the default export:
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca

```js
// 📁 main.js
import * as user from './user.js';

<<<<<<< HEAD
let User = user.default; // экспорт по умолчанию
new User('John');
```

### Довод против экспортов по умолчанию

Именованные экспорты "включают в себя" своё имя. Эта информация является частью модуля, говорит нам, что именно экспортируется.

Именованные экспорты вынуждают нас использовать правильное имя при импорте:
=======
let User = user.default; // the default export
new User('John');
```

### A word agains default exports

Named exports are explicit. They exactly name what they import, so we have that information from them, that's a good thing.

Named exports enforce us to use exactly the right name to import:
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca

```js
import {User} from './user.js';
// import {MyUser} не сработает, должно быть именно имя {User}
```

<<<<<<< HEAD
...В то время как для экспорта по умолчанию мы выбираем любое имя при импорте:
=======
...While for a default export, we always choose the name when importing:
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca

```js
import User from './user.js'; // сработает
import MyUser from './user.js'; // тоже сработает
// можно импортировать с любым именем, и это будет работать
```

<<<<<<< HEAD
Так что члены команды могут использовать разные имена для импорта одной и той же вещи, и это не очень хорошо.
=======
So team members may use different names to import the same thing, and that's not good.
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca

Обычно, чтобы избежать этого и соблюсти единообразие кода, есть правило: имена импортируемых переменных должны соответствовать именам файлов. Вот так:

```js
import User from './user.js';
import LoginForm from './loginForm.js';
import func from '/path/to/func.js';
...
```

<<<<<<< HEAD
Тем не менее, в некоторых командах это считают серьёзным доводом против экспортов по умолчанию и предпочитают использовать именованные экспорты везде. Даже если экспортируется только одна вещь, она всё равно экспортируется с именем, без использования `default`.
=======
Still, some teams consider it a serous drawback of default exports. So they prefer to always use named exports. Even if only a single thing is exported, it's still exported under a name, without `default`.
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca

Это также немного упрощает реэкспорт (смотрите ниже).

## Реэкспорт

Синтаксис "реэкспорта" `export ... from ... ` позволяет импортировать что-то и тут же экспортировать, возможно под другим именем, вот так:

```js
<<<<<<< HEAD
export {sayHi} from './say.js'; // реэкспортировать sayHi

export {default as User} from './user.js'; // реэкспортировать default
```

Зачем это нужно? Рассмотрим практический пример использования.

Представим, что мы пишем "пакет": папку со множеством модулей, из которой часть функционала экспортируется наружу (инструменты вроде NPM позволяют нам публиковать и распространять такие пакеты), а многие модули - просто вспомогательные, для внутреннего использования в других модулях пакета.

Структура файлов может быть такой:
=======
export {sayHi} from './say.js'; // re-export sayHi

export {default as User} from './user.js'; // re-export default
```

Why that may be needed? Let's see a practical use case.

Imagine, we're writing a "package": a folder with a lot of modules, with some of the functionality exported outside (tools like NPM allow to publish and distribute such packages), and many modules are just "helpers", for the internal use in other package modules.

The file structure could be like this:
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca
```
auth/
    index.js  
    user.js
    helpers.js
    tests/
        login.js
    providers/
        github.js
        facebook.js
        ...
```

Мы бы хотели сделать функционал нашего пакета доступным через единую точку входа: "главный файл" `auth/index.js`. Чтобы можно было использовать его следующим образом:

```js
import {login, logout} from 'auth/index.js'
```

<<<<<<< HEAD
Идея в том, что внешние разработчики, которые будут использовать наш пакет, не должны разбираться с его внутренней структурой, рыться в файлах внутри нашего пакета. Всё, что нужно, мы экспортируем в `auth/index.js`, а остальное скрываем от любопытных взглядов.

Так как нужный функционал может быть разбросан по модулям нашего пакета, мы можем импортировать их в `auth/index.js` и тут же экспортировать наружу.
=======
The idea is that outsiders, developers who use our package, should not meddle with its internal structure, search for files inside our package folder. We export only what's necessary in `auth/index.js` and keep the rest hidden from prying eyes.

As the actual exported functionality is scattered among the package, we can import it into `auth/index.js` and export from it:
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca

```js
// 📁 auth/index.js

<<<<<<< HEAD
// импортировать login/logout и тут же экспортировать
import {login, logout} from './helpers.js';
export {login, logout};

// импортировать экспорт по умолчанию как User и тут же экспортировать
=======
// import login/logout and immediately export them
import {login, logout} from './helpers.js';
export {login, logout};

// import default as User and export it
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca
import User from './user.js';
export {User};
...
```

<<<<<<< HEAD
Теперь пользователи нашего пакета могут писать `import {login} from "auth/index.js"`.

Запись `export ... from ...`-- это просто более короткий вариант такого импорта-экспорта:

```js
// 📁 auth/index.js

// импортировать login/logout и тут же экспортировать
export {login, logout} from './helpers.js';

// импортировать экспорт по умолчанию как User и тут же экспортировать
=======
Now users of our package can `import {login} from "auth/index.js"`.

The syntax `export ... from ...` is just a shorter notation for such import-export:

```js
// 📁 auth/index.js
// import login/logout and immediately export them
export {login, logout} from './helpers.js';

// import default as User and export it
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca
export {default as User} from './user.js';
...
```

<<<<<<< HEAD
### Реэкспорт экспорта по умолчанию

При реэкспорте экспорт по умолчанию нужно обрабатывать особым образом.

Например, у нас есть `user.js`, из которого мы хотим реэкспортировать класс `User`:
=======
### Re-exporting the default export

The default export needs separate handling when re-exporting.

Let's say we have `user.js`, and we'd like to re-export class `User` from it:
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca

```js
// 📁 user.js
export default class User {
  // ...
}
```

<<<<<<< HEAD
1. `export User from './user.js'` не будет работать. Казалось бы, что такого? Но возникнет синтаксическая ошибка!

    Чтобы реэкспортировать экспорт по умолчанию, мы должны написать `export {default as User}`, как в примере выше. Такая вот особенность синтаксиса.

2. `export * from './user.js'` реэкспортирует только именованные экспорты, исключая экспорт по умолчанию.

    Если мы хотим реэкспортировать и именованные экспорты и экспорт по умолчанию, то понадобятся две инструкции:
    ```js
    export * from './user.js'; // для реэкспорта именованных экспортов
    export {default} from './user.js'; // для реэкспорта по умолчанию
    ```

Такое особое поведение реэкспорта с экспортом по умолчанию - одна из причин того, почему некоторые разработчики их не любят.

## Итого
=======
1. `export User from './user.js'` won't work. What can go wrong?... But that's a syntax error!

    To re-export the default export, we should write `export {default as User}`, as in the example above.    

2. `export * from './user.js'` re-exports only named exports, ignores the default one.

    If we'd like to re-export both named and the default export, then two statements are needed:
    ```js
    export * from './user.js'; // to re-export named exports
    export {default} from './user.js'; // to re-export the default export
    ```

Such oddities of re-exporting the default export is one of the reasons, why some developers don't like them.
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca

Вот все варианты `export`, которые мы разобрали в этой и предыдущей главах.

<<<<<<< HEAD
Вы можете проверить себя, читая их и вспоминая, что они означают:

- Перед объявлением класса/функции/...:
  - `export [default] class/function/variable ...`
- Отдельный экспорт:
  - `export {x [as y], ...}`.
- Реэкспорт:
  - `export {x [as y], ...} from "module"`
  - `export * from "module"` (не реэкспортирует `export default`).
  - `export {default [as y]} from "module"` (реэкспортирует только `export default`).

Импорт:

- Именованные экспорты из модуля:
  - `import {x [as y], ...} from "module"`
- Экспорт по умолчанию:  
  - `import x from "module"`
  - `import {default as x} from "module"`
- Всё сразу:
  - `import * as obj from "module"`
- Только подключить модуль (его код запустится), но не присваивать его переменной:
  - `import "module"`

Мы можем поставить `import/export` в начало или в конец скрипта, это не имеет значения.

То есть, технически, такая запись вполне корректна:
=======
Here are all types of `export` that we covered in this and previous chapters.

You can check yourself by reading them and recalling what they mean:

- Before declaration of a class/function/..:
  - `export [default] class/function/variable ...`
- Standalone export:
  - `export {x [as y], ...}`.
- Re-export:
  - `export {x [as y], ...} from "module"`
  - `export * from "module"` (doesn't re-export default).
  - `export {default [as y]} from "module"` (re-export default).

Import:

- Named exports from module:
  - `import {x [as y], ...} from "module"`
- Default export:  
  - `import x from "module"`
  - `import {default as x} from "module"`
- Everything:
  - `import * as obj from "module"`
- Import the module (its code runs), but do not assign it to a variable:
  - `import "module"`

We can put `import/export` statements at the top or at the bottom of a script, that doesn't matter.

So, technically this code is fine:
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca
```js
sayHi();

// ...

<<<<<<< HEAD
import {sayHi} from './say.js'; // импорт в конце файла
=======
import {sayHi} from './say.js'; // import at the end of the file
>>>>>>> 5cb9760abb8499bf1e99042d866c3c1db8cd61ca
```

На практике импорты, чаще всего, располагаются в начале файла. Но это только для большего удобства.

**Обратите внимание, что инструкции import/export не работают внутри `{...}`.**

Условный импорт, такой как ниже, работать не будет:
```js
if (something) {
  import {sayHi} from "./say.js"; // Ошибка: импорт должен быть на верхнем уровне
}
```

...Но что, если нам в самом деле нужно импортировать что-либо в зависимости от условий? Или в определённое время? Например, загрузить модуль, только когда он станет нужен?

Мы рассмотрим динамические импорты в следующей главе.
