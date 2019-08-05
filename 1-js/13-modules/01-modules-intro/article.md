
# Модули, введение

<<<<<<< HEAD
По мере роста нашего приложения, мы обычно хотим разделить его на много файлов, так называемых "модулей". Модуль обычно содержит класс или библиотеку с функциями.
=======
As our application grows bigger, we want to split it into multiple files, so called "modules". A module usually contains a class or a library of functions.
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a

Долгое время в JavaScript отсутствовал синтаксис модулей на уровне языка. Это не было проблемой, потому что первые скрипты были маленькими и простыми. В модулях не было необходимости.

Но со временем скрипты становились всё более и более сложными, поэтому сообщество придумало несколько вариантов организации кода в модули. Появились библиотеки для динамической подгрузки модулей.

Например:

- [AMD](https://ru.wikipedia.org/wiki/Asynchronous_module_definition) -- одна из самых старых модульных систем, изначально реализована библиотекой [require.js](http://requirejs.org/).
- [CommonJS](http://wiki.commonjs.org/wiki/Modules/1.1) -- модульная система, созданная для сервера Node.js.
- [UMD](https://github.com/umdjs/umd) -- ещё одна модульная система, предлагается как универсальная, совместима с AMD и CommonJS.

<<<<<<< HEAD
Теперь все они постепенно становятся частью истории, хотя их и можно найти в старых скриптах.
=======
Now all these slowly become a part of history, but we still can find them in old scripts.

The language-level module system appeared in the standard in 2015, gradually evolved since then, and is now supported by all major browsers and in Node.js. So we'll study it from now on.
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a

Система модулей на уровне языка появилась в стандарте JavaScript в 2015 году и постепенно эволюционировала. На данный момент она поддерживается большинством браузеров и Node.js. Далее мы будем изучать именно её.

<<<<<<< HEAD
## Что такое модуль?

Модуль - это просто файл. Один скрипт - это один модуль.
=======
A module is just a file. One script is one module.

Modules can load each other and use special directives `export` and `import` to interchange functionality, call functions of one module from another one:
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a

Модули могут загружать друг друга и использовать директивы `export` и `import`, чтобы обмениваться функциональностью, вызывать функции одного модуля из другого:

- `export` отмечает переменные и функции, которые должны быть доступны вне текущего модуля.
- `import` позволяет импортировать функциональность из других модулей.

Например, если у нас есть файл `sayHi.js`, который экспортирует функцию:

```js
// 📁 sayHi.js
export function sayHi(user) {
  alert(`Hello, ${user}!`);
}
```

...Тогда другой файл может импортировать её и использовать:

```js
// 📁 main.js
import {sayHi} from './sayHi.js';

alert(sayHi); // function...
sayHi('John'); // Hello, John!
```

<<<<<<< HEAD
Директива `import` загружает модуль по пути `./sayHi.js` относительно текущего файла и записывает эксортированную функцию `sayHi` в соответствующую переменную.

Давайте запустим пример в браузере.
=======
The `import` directive loads the module by path `./sayHi.js` relative the current file and assigns exported function `sayHi` to the corresponding variable.

Let's run the example in-browser.
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a

Так как модули поддерживают ряд специальных ключевых слов, и у них есть ряд особенностей, то необходимо явно сказать браузеру, что скрипт является модулем, при помощи атрибута `<script type="module">`.

Вот так:

[codetabs src="say" height="140" current="index.html"]

<<<<<<< HEAD
Браузер автоматически загрузит и запустит импортированный модуль (и те, которые он импортирует, если надо), а затем запустит скрипт.
=======
The browser automatically fetches and evaluates the imported module (and its imports if needed), and then runs the script.
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a

## Основные возможности модулей

Чем отличаются модули от "обычных" скриптов?

Есть основные возможности и особенности, работающие как в браузере, так и в серверном JavaScript.

### Всегда "use strict"

В модулях всегда используется режим `use strict`. Например, присваивание к необъявленной переменной вызовет ошибку.

```html run
<script type="module">
  a = 5; // ошибка
</script>
```

<<<<<<< HEAD
### Своя область видимости переменных
=======
### Module-level scope
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a

Каждый модуль имеет свою собственную область видимости. Другими словами, переменные и функции, объявленные в модуле, не видны в других скриптах.

В следующем примере импортированы 2 скрипта, и `hello.js` пытается использовать переменную `user`, объявленную в `user.js`. В итоге ошибка:

[codetabs src="scopes" height="140" current="index.html"]

Модули должны экспортировать функционал, предназначенный для использования извне. А другие модули могут его импортировать.

<<<<<<< HEAD
Так что нам надо импортировать `user.js` в `hello.js` и взять из него нужный функционал, вместо того чтобы полагаться на глобальные переменные.
=======
So we should import `user.js` into `hello.js` and get the required functionality from it instead of relying on global variables.
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a

Правильный вариант:

[codetabs src="scopes-working" height="140" current="hello.js"]

В браузере также существует независимая область видимости для каждого скрипта `<script type="module">`:

```html run
<script type="module">
  // Переменная доступна только в этом модуле
  let user = "John";
</script>

<script type="module">
  *!*
  alert(user); // Error: user is not defined
  */!*
</script>
```

Если нам нужно сделать глобальную переменную уровня всей страницы, можно явно присвоить её объекту `window`, тогда получить значение переменной можно обратившись к `window.user`. Но это должно быть исключением, требующим веской причины.

### Код в модуле выполняется только один раз при импорте

Если один и тот же модуль используется в нескольких местах, то его код выполнится только один раз, после чего экспортируемая функциональность передаётся всем импортёрам.

Это очень важно для понимания работы модулей. Давайте посмотрим примеры.

Во-первых, если при запуске модуля возникают побочные эффекты, например выдаётся сообщение, то импорт модуля в нескольких местах покажет его только один раз - при первом импорте:

```js
// 📁 alert.js
alert("Модуль выполнен!");
```

```js
// Импорт одного и того же модуля в разных файлах

// 📁 1.js
import `./alert.js`; // Модуль выполнен!

// 📁 2.js
<<<<<<< HEAD
import `./alert.js`; // (ничего не покажет)
```

На практике, задача кода модуля - это обычно инициализация, создание внутренних структур данных, а если мы хотим, чтобы что-то можно было использовать много раз, то экспортируем это.
=======
import `./alert.js`; // (shows nothing)
```

In practice, top-level module code is mostly used for initialization, creation of internal data structures, and if we want something to be reusable -- export it.
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a

Теперь более продвинутый пример.

Давайте представим, что модуль экспортирует объект:

```js
// 📁 admin.js
export let admin = {
  name: "John"
};
```

Если модуль импортируется в нескольких файлах, то код модуля будет выполнен только один раз, объект `admin` будет создан и в дальнейшем будет передан всем импортёрам.

Все импортёры получат один-единственный объект `admin`:

```js
// 📁 1.js
import {admin} from './admin.js';
admin.name = "Pete";

// 📁 2.js
import {admin} from './admin.js';
alert(admin.name); // Pete

*!*
// Оба файла, 1.js и 2.js, импортируют один и тот же объект
// Изменения, сделанные в 1.js, будут видны в 2.js
*/!*
```

<<<<<<< HEAD
Ещё раз заметим -- модуль выполняется только один раз. Генерируется экспорт и после передаётся всем импортёрам, поэтому, если что-то изменится в объекте `admin`, то другие модули тоже увидят эти изменения.

Такое поведение позволяет *конфигурировать* модули при первом импорте. Мы можем установить его свойства один раз, и в дальнейших импортах он будет уже настроенным.
=======
So, let's reiterate -- the module is executed only once. Exports are generated, and then they are shared between importers, so if something changes the `admin` object, other modules will see that.

Such behavior allows to *configure* modules on first import. We can setup its properties once, and then in further imports it's ready.
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a

Например, модуль `admin.js` предоставляет определённую функциональность, но ожидает передачи учётных данных в объект `admin` извне:

```js
// 📁 admin.js
export let admin = { };

export function sayHi() {
  alert(`Ready to serve, ${admin.name}!`);
}
```

<<<<<<< HEAD
В `init.js`, первом скрипте нашего приложения, мы установим `admin.name`. Тогда все это увидят, включая вызовы, сделанные из самого `admin.js`:
=======
In `init.js`, the first script of our app, we set `admin.name`. Then everyone will see it, including calls made from inside `admin.js` itself:
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a

```js
// 📁 init.js
import {admin} from './admin.js';
admin.name = "Pete";
```

<<<<<<< HEAD
Другой модуль тоже увидит `admin.name`:
=======
Another module can also see `admin.name`:
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a

```js
// 📁 other.js
import {admin, sayHi} from './admin.js';

alert(admin.name); // *!*Pete*/!*

sayHi(); // Ready to serve, *!*Pete*/!*!
```

### import.meta

Объект `import.meta` содержит информацию о текущем модуле.

Содержимое зависит от окружения. В браузере он содержит ссылку на скрипт или ссылку на текущую веб-страницу, если модуль встроен в HTML:

```html run height=0
<script type="module">
  alert(import.meta.url); // ссылка на html страницу для встроенного скрипта
</script>
```

<<<<<<< HEAD
### В модуле "this" неопределён

Это незначительная особенность, но для полноты картины нам нужно упомянуть об этом.
=======
### In a module, "this" is undefined
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a

В модуле на верхнем уровне `this` не определён (undefined).

<<<<<<< HEAD
Сравним с не-модульными скриптами, там `this` - глобальный объект:
=======
In a module, top-level `this` is undefined.

Compare it to non-module scripts, where `this` is a global object:
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a

```html run height=0
<script>
  alert(this); // window
</script>

<script type="module">
  alert(this); // undefined
</script>
```

## Особенности в браузерах

Есть и несколько других, именно браузерных особенностей скриптов с `type="module"` по сравнению с обычными скриптами.

<<<<<<< HEAD
Если вы читаете материал в первый раз или, если не собираетесь использовать модули в браузерах, то сейчас можете пропустить эту секцию.
=======
You may want skip this section for now if you're reading for the first time, or if you don't use JavaScript in a browser.
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a

### Модули являются отложенными (deferred)

Модули *всегда* выполняются в отложенном (deferred) режиме, точно так же, как скрипты с атрибутом `defer` (описан в главе [](info:script-async-defer)). Это верно и для внешних и встроенных скриптов-модулей.

<<<<<<< HEAD
Другими словами:
- загрузка внешних модулей, таких как `<script type="module" src="...">`, не блокирует обработку HTML.
- модули, даже если загрузились быстро, ожидают полной загрузки HTML документа, и только затем выполняются.
- сохраняется относительный порядок скриптов: скрипты, которые идут раньше в документе, выполняются раньше.
=======
In other words:
- downloading of external module scripts `<script type="module" src="...">` doesn't block HTML processing, they load in parallel with other resources.
- module scripts wait until the HTML document is fully ready (even if they are tiny and load faster than HTML), and then run.
- relative order of scripts is maintained: scripts that go first in the document, execute first.
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a

Как побочный эффект, модули всегда видят полностью загруженную HTML-страницу, включая элементы под ними.

Например:

```html run
<script type="module">
*!*
  alert(typeof button); // object: скрипт может 'видеть' кнопку под ним
*/!*
  // так как модули являются отложенными, то скрипт начнёт выполнятся только после полной загрузки страницы
</script>

Сравните с обычным скриптом ниже:

<script>
*!*
  alert(typeof button); // Ошибка: кнопка не определена, скрипт не видит элементы под ним
*/!*
  // обычные скрипты запускаются сразу, не дожидаясь полной загрузки страницы
</script>

<button id="button">Кнопка</button>
```

Пожалуйста, обратите внимание: второй скрипт выполнится раньше, чем первый! Поэтому мы увидим сначала `undefined`, а потом `object`.

Это потому, что модули начинают выполняться после полной загрузки страницы. Обычные скрипты запускаются сразу же, поэтому сообщение из обычного скрипта мы видим первым.

При использовании модулей нам стоит иметь в виду, что HTML-страница будет показана браузером до того, как выполнятся модули и JavaScript-приложение будет готово к работе. Некоторые функции могут ещё не работать. Нам следует разместить "индикатор загрузки" или что-то ещё, чтобы не смутить этим посетителя.

<<<<<<< HEAD
### Атрибут async работает во встроенных скриптах
=======
When using modules, we should be aware that HTML-page shows up as it loads, and JavaScript modules run after that, so the user may see the page before the JavaScript application is ready. Some functionality may not work yet. We should put "loading indicators", or otherwise ensure that the visitor won't be confused by that.
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a

Для не-модульных скриптов атрибут `async` работает только на внешних скриптах. Скрипты с ним запускаются сразу по готовности, они не ждут другие скрипты или HTML-документ.

<<<<<<< HEAD
Для модулей атрибут `async` работает на любых скриптах.
=======
For non-module scripts, `async` attribute only works on external scripts. Async scripts run immediately when ready, independently of other scripts or the HTML document.

For module scripts, it works on any scripts.
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a

Например, в скрипте ниже есть `async`, поэтому он выполнится сразу после загрузки, не ожидая других скриптов.

Скрипт выполнит импорт (загрузит `./analytics.js`) и сразу запустится, когда будет готов, даже если HTML документ ещё не будет загружен, или если другие скрипты ещё загружаются.

Это очень полезно, когда модуль ни с чем не связан, например для счётчиков, рекламы, обработчиков событий.

```html
<!-- загружаются зависимости (analytics.js) и скрипт запускается -->
<!-- модуль не ожидает загрузки документа или других тэгов <script> -->
<script *!*async*/!* type="module">
  import {counter} from './analytics.js';

  counter.count();
</script>
```

### Внешние скрипты

<<<<<<< HEAD
Внешние скрипты с атрибутом `type="module"` имеют два отличия:
=======
External scripts that have `type="module"` are different in two aspects:
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a

1. Внешние скрипты с одинаковым атрибутом `src` запускаются только один раз:
    ```html
    <!-- скрипт my.js загрузится и будет выполнен только один раз -->
    <script type="module" src="my.js"></script>
    <script type="module" src="my.js"></script>
    ```

<<<<<<< HEAD
2. Внешний скрипт, который загружается с другого домена, требует указания заголовков [CORS](mdn:Web/HTTP/CORS). Другими словами, если модульный скрипт загружается с другого домена, то удалённый сервер должен установить заголовок `Access-Control-Allow-Origin` означающий, что загрузка скрипта разрешена.
=======
2. External scripts that are fetched from another origin (e.g. another site) require [CORS](mdn:Web/HTTP/CORS) headers, as described in the chapter <info:fetch-crossorigin>. In other words, if a module script is fetched from another origin, the remote server must supply a header `Access-Control-Allow-Origin` allowing the fetch.
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a
    ```html
    <!-- another-site.com должен указать заголовок Access-Control-Allow-Origin -->
    <!-- иначе, скрипт не выполнится -->
    <script type="module" src="*!*http://another-site.com/their.js*/!*"></script>
    ```

    Это обеспечивает лучшую безопасность по умолчанию.

### Не допускаются "голые" модули

В браузере `import` должен содержать относительный или абсолютный путь к модулю. Модули без пути называются "голыми" (bare). Они не разрешены в `import`.

Например, этот `import` неправильный:
```js
import {sayHi} from 'sayHi'; // Ошибка, "голый" модуль
// путь должен быть, например './sayHi.js' или абсолютный
```

Другие окружения, например Node.js, допускают использование "голых" модулей, без путей, так как в них есть свои правила, как работать с такими модулями и где их искать. Но браузеры пока не поддерживают "голые" модули.

### Совместимость, "nomodule"

Старые браузеры не понимают атрибут `type="module"`. Скрипты с неизвестным атрибутом `type` просто игнорируются. Мы можем сделать для них "резервный" скрипт при помощи атрибута `nomodule`:

```html run
<script type="module">
  alert("Работает в современных браузерах");
</script>

<script nomodule>
  alert("Современные браузеры понимают оба атрибута - и type=module, и nomodule, поэтому пропускают этот тег script")
  alert("Старые браузеры игнорируют скрипты с неизвестным атрибутом type=module, но выполняют этот.");
</script>
```

<<<<<<< HEAD
## Инструменты сборки

В реальной жизни модули в браузерах редко используются в "сыром" виде. Обычно, мы объединяем модули вместе, используя специальный инструмент, например [Webpack](https://webpack.js.org/) и после выкладываем код на рабочий сервер.

Одно из преимуществ использования сборщика -- он предоставляет больший контроль над тем, как модули ищутся, позволяет использовать "голые" модули и многое другое "своё", например CSS/HTML-модули.
=======
## Build tools
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a

Сборщик делает следующее:

1. Берёт "основной" модуль, который мы собираемся поместить в `<script type="module">` в HTML.
2. Анализирует зависимости (импорты, импорты импортов и так далее)
3. Собирает один файл со всеми модулями (или несколько файлов, это можно настроить), перезаписывает встроенный `import` функцией импорта от сборщика, чтобы всё работало. "Специальные" типы модулей, такие как HTML/CSS тоже поддерживаются.
4. В процессе могут происходить и другие трансформации и оптимизации кода:
    - Недоступный код удаляется.
    - Неиспользуемые экспорты удаляются ("tree-shaking").
    - Специфические операторы для разработки, такие как `console` и `debugger`, удаляются.
    - Современный синтаксис JavaScript также может быть трансформирован в предыдущий стандарт, с похожей функциональностью, например, с помощью [Babel](https://babeljs.io/).
    - Полученный файл можно минимизировать (удалить пробелы, заменить названия переменных на более короткие и т.д.).

Если мы используем инструменты сборки, то они объединяют модули вместе в один или несколько файлов, и заменяют `import/export` на свои вызовы. Поэтому итоговую сборку можно подключать и не без атрибута `type="module"`, как обычный скрипт:

```html
<!-- Предположим, что мы собрали bundle.js, используя например утилиту Webpack -->
<script src="bundle.js"></script>
```

<<<<<<< HEAD
Хотя и "как есть" модули тоже можно использовать, а сборщик настроить позже при необходимости.
=======
If we use bundle tools, then as scripts are bundled together into a single file (or few files), `import/export` statements inside those scripts are replaced by special bundler functions. So the resulting "bundled" script does not contain any `import/export`, it doesn't require `type="module"`, and we can put it into a regular script:

```html
<!-- Assuming we got bundle.js from a tool like Webpack -->
<script src="bundle.js"></script>
```

That said, native modules are also usable. So we won't be using Webpack here: you can configure it later.
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a

## Итого

Подводя итог, основные понятия:

<<<<<<< HEAD
1. Модуль - это файл. Чтобы работал `import/export`, нужно для браузеров указывать атрибут `<script type="module">`. У модулей есть ряд особенностей:
    - Отложенное (deferred) выполнение по умолчанию.
    - Атрибут async работает во встроенных скриптах.
    - Для загрузки внешних модулей с другого источника, он должен ставить заголовки CORS.
    - Дублирующиеся внешние скрипты игнорируются.
2. У модулей есть своя область видимости, обмениваться функциональностью можно через `import/export`.
3. В модулях всегда включена директива `use strict`.
4. Код в кодулях выполняется только один раз. Экспортируемая функциональность создётся один раз и передаётся всем импортёрам.

Когда мы используем модули, каждый модуль реализует свою функциональность и экспортирует её. Затем мы используем `import`, чтобы напрямую импортировать её туда, куда необходимо. Браузер загружает и анализирует скрипты автоматически.
=======
1. A module is a file. To make `import/export` work, browsers need `<script type="module">`. Modules have several differences:
    - Deferred by default.
    - Async works on inline scripts.
    - To load external scripts from another origin (domain/protocol/port), CORS headers are needed.
    - Duplicate external scripts are ignored.
2. Modules have their own, local top-level scope and interchange functionality via `import/export`.
3. Modules always `use strict`.
4. Module code is executed only once. Exports are created once and shared between importers.

When we use modules, each module implements the functionality and exports it. Then we use `import` to directly import it where it's needed. Browser loads and evaluates the scripts automatically.
>>>>>>> fb38a13978f6e8397005243bc13bc1a20a988e6a

В реальной жизни часто используется сборщик [Webpack](https://webpack.js.org), чтобы объединить модули: для производительности и других "плюшек".

В следующей главе мы увидим больше примеров и вариантов импорта/экспорта.
