# Оператор null-об'єднання '??'

[recent browser="new"]

Оператор null-об'єднання позначається, як два знаки питання `??`.

У цій статті ми говоритимемо, що вираз є "визначеним" коли він не є ні `null` ні `undefined`.

Результатом виразу `a ?? b` буде наступне:
- якщо `a` визначене, то `a`,
- якщо `a` не визначене, то `b`.


Тобто оператор `??` повертає перший аргумент, якщо він не `null/undefined`. В іншому випадку - другий.

Оператор null-об'єднання не є чимось абсолютно новим. Це лише гарний синтаксис, щоб отримати одне "визначене" значення з двох.

Ми можемо переписати вираз `result = a ?? b` використовуючи вже знайомі нам оператори:

```js
result = (a !== null && a !== undefined) ? a : b;
```

Здебільшого`??` використовується для надання значення за замовчуванням для потенційно невизначеної змінної.

Наприклад, ми побачимо `Анонім`, якщо `user` не визначений:

```js run
let user;

alert(user ?? "Анонім"); // Анонім
```

Звичайно, якщо змінна `user` має будь-яке значення, крім `null/undefined`, то ми побачимо його:

```js run
let user = "Тарас";

alert(user ?? "Анонім"); // Тарас
```

Ми також можемо використати послідовність `??` щоб вибрати перше значення зі списку, яке не є `null/undefined`.

Скажімо, у нас є дані користувача у змінних `firstName`, `lastName` або `nickName`. Всі вони можуть бути невизначеними, якщо користувач вирішив не вводити значення.

Виведем ім'я користувача, використовуючи одну з цих змінних, або покаже "Анонім", якщо всі вони не визначені.

Для цього використовуємо оператор `??`:

```js run
let firstName = null;
let lastName = null;
let nickName = "Суперкодер";

// покаже перше визначене значення:
*!*
alert(firstName ?? lastName ?? nickName ?? "Анонім"); // Суперкодер
*/!*
```

## Порівняння з ||

Оператор АБО `||` може використовуватись для того ж, що й `??`, як це було показано в [попередньому розділі](info:logical-operators#or-finds-the-first-truthy-value).

Наприклад, якщо в зазначеному вище коді замінити `??` на `||`, то буде той самий результат:

```js run
let firstName = null;
let lastName = null;
let nickName = "Суперкодер";

// показує перше істине значення:
*!*
alert(firstName || lastName || nickName || "Анонім"); // Суперкодер
*/!*
```

Оператор АБО `||` існує з самої появи JavaScript, тому раніше для рішення подібних задач використовували саме його.

З іншої сторони, відносно недавно був доданий оператор null-об'єднання `??` саме тому, що було багато незадоволених оператором `||`.

Важлива відмінність між ними полягає ось у чому:
- `||` повертає перше *істине* значення.
- `??` повертає перше *визначене* значення.

Іншими словами, оператор `||` не розрізняє `false`, `0`, порожній рядок `""` та `null/undefined`. Всі вони однакові -- хибні. Якщо будь-який з них є першим аргументом для `||`, то як результат отримаємо другий аргумент.

Однак на практиці може знадобитись використання значення за замовчуванням лише тоді, коли змінна має значення `null/undefined`. Тобто, коли значення насправді невідоме/не визначене.

Наприклад, розглянемо цей приклад:

```js run
let height = 0;

alert(height || 100); // 100
alert(height ?? 100); // 0
```

- `height || 100` перевіряє, чи має змінна `height` хибне значення, що є насправді.
    - отже, результатом є другий аргумент, `100`.
- `height ?? 100` перевіряє, чи змінна `height` містить `null/undefined`, оскільки це не так,
    - отже результатом є саме змінна `height`, тобто `0`.

Якщо нульова висота є допустимим значенням, яке не слід замінювати значенням за замовчуванням, тоді `??` робить саме те, що потрібно.

## Пріоритет

Пріоритет оператора `??` досить низький: `5` у [MDN таблиці](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence#Table). Отже `??` обчислюється перед `=` та `?`, але після більшості інших, таких як `+`, `*`.

Отже, якщо ми хочемо обрати значення за допомогою оператора `??` у виразі з іншими операторами, варто додати круглі дужки:

```js run
let height = null;
let width = null;

// важливо: використовуйте круглі дужки
let area = (height ?? 100) * (width ?? 50);

alert(area); // 5000
```

Інакше, якщо опустити дужки, то оператор `*` виконається першим, тому що у нього пріоритет вище, ніж у `??`, що призведе до неправильних результатів.

```js
// без круглих дужок
let area = height ?? 100 * width ?? 50;

// ...теж саме, що й попередній вираз (вірогідно, це не те, що потрібно):
let area = height ?? (100 * width) ?? 50;
```

### Використання ?? разом з && або ||

З міркувань безпеки, JavaScript забороняє використовувати оператор `??` разом з `&&` та `||`, якщо пріоритет не вказаний круглими дужками.

Наведений нижче код викликає синтаксичну помилку:

```js run
let x = 1 && 2 ?? 3; // Синтаксична помилка
```

Безумовно, обмеження спірне, але воно було додано до специфікації мови з метою уникнення помилок програмування, при переході на `??` з `||`.

Використовуйте круглі дужки, аби обійти обмеження:

```js run
*!*
let x = (1 && 2) ?? 3; // Працює
*/!*

alert(x); // 2
```

## Підсумки

- Оператор null-об'єднання `??` надає короткий спосіб обрати перше "визначене" значення зі списку.

    Він використовується для привласнення значенням за замовчуванням змінним:

    ```js
    // set height=100, якщо змінна height є null або undefined
    height = height ?? 100;
    ```

- Оператор `??` має дуже низький пріоритет, не на багато вищий за `?` та `=`, тому додавайте дужки, при використанні його у виразах.
- Заборонено використовувати його з `||` або `&&` без явних круглих дужок.
