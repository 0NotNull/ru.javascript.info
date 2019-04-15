
# Fetch: Прерывание запроса

Прервать выполнение метода `fetch` немного сложно. Помните, метод `fetch` возвращает промис. А в JavaScript в целом нет понятия "отмены" промиса. Итак, как можно отменить вызов `fetch`?

Для таких целей существует специальный встроенный объект: `AbortController`.

Использовать его достаточно просто:

- Шаг 1: создаем контроллер:

    ```js
    let controller = new AbortController();
    ```

    Контроллер - чрезвычайно простой объект. Он имеет единственный метод `abort()` и единственное свойство `signal`, которое генерирует событие, когда вызывается `abort()`:

    Мы даже можем использовать его без `fetch` для своих задач, например, так:

    ```js run
    let controller = new AbortController();
    let signal = controller.signal;

    // срабатывает при вызове controller.abort()
    signal.addEventListener('abort', () => alert("прервать!"));

    controller.abort(); // прервать!

    alert(signal.aborted); // true (после прерывания)
    ```

- Шаг 2: передайте свойство `signal` опцией в метод `fetch`:

    ```js
    let controller = new AbortController();
    fetch(url, {
      signal: controller.signal
    });
    ```

    Теперь метод `fetch` слушает сигнал.

- Шаг 3: чтобы прервать выполнение `fetch`, вызовите `controller.abort()`:

    ```js
    controller.abort();
    ```

    Вот и всё: `fetch` получает событие из `signal` и прерывает запрос.

Когда `fetch` прерывается, его промис отклоняется с ошибкой `AbortError`, поэтому мы должны обработать ее:

```js run async
// прервать через 1 секунду
let controller = new AbortController();
setTimeout(() => controller.abort(), 1000);

try {
  let response = await fetch('/article/fetch-abort/demo/hang', {
    signal: controller.signal
  });
} catch(err) {
  if (err.name == 'AbortError') { // обработь ошибку от вызова abort()
    alert("Прервано!");
  } else {
    throw err;
  }
}
```

**`AbortController` - масштабируемый, поэтому позволяет отменить несколько вызовов `fetch` одновременно.**

Например, здесь мы запрашиваем много `url` параллельно, и контроллер прерывает их все:

```js
let urls = [...]; // список `url` для запроса параллельно

let controller = new AbortController();

let fetchJobs = urls.map(url => fetch(url, {
  signal: controller.signal
}));

let results = await Promise.all(fetchJobs);

// вызов откуда-нибудь еще:
// controller.abort() прерывает все вызовы `fetch`
```

Если у нас есть собственные задачи, отличные от `fetch`, мы можем использовать один `AbortController` для их остановки вместе с `fetch`.


```js
let urls = [...];
let controller = new AbortController();

let ourJob = new Promise((resolve, reject) => {
  ...
  controller.signal.addEventListener('abort', reject);
});

let fetchJobs = urls.map(url => fetch(url, {
  signal: controller.signal
}));

let results = await Promise.all([...fetchJobs, ourJob]);

// вызов откуда-нибудь еще:
// controller.abort() прерывает все вызовы `fetch` и наши задачи
```
