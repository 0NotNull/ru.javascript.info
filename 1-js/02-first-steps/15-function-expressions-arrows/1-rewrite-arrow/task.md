
# Перепишите с использованием Функции-стрелки

Замените код Функционального Выражения стрелочной функцией:

```js run
function ask(question, yes, no) {
  if (confirm(question)) yes()
  else no();
}

ask(
  "Вы согласны?",
  function() { alert("Вы согласились."); },
  function() { alert("Вы отменили выполнение."); }
);
```
