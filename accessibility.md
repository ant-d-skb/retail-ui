# Чек-лист доступности для фронтов


## Соблюдайте семантическую разметку

1. Используйте атрибут `lang=ru` для тега `<html>`.
Он помогает скринридерам и парсерам понять, на каком языке страница, и как её озвучивать. Так же `lang` можно указывать для блоков разметки на другом языке, например, если абзац текста на английском.

2. Следите за иерархией заголовков. На странице должен быть только один заголовок 1-го уровня, остальные заголовки должны быть вложенными в зависимости от уровня.
```html
<header>
  <h1>Страница отчёта</h1>
</header>
<main>
  <article>
    <h2>Отчёт 1</h2>
  </article>
  <article>
    <h2>Отчёт 2</h2>
    <div>
      <h3>Подпункт 1</h3>
    </div>
  </article>
</main>
```

3. Используйте для разметки списков теги `<ul>`, `<ol>`, `<li>`; для таблиц `<table>`, `<tr>`, `<th>`, `<td>`.
```html
<ul>
  <li>Первый пункт</li>
  <li>Второй пункт</li>
</ul>
```
```html
<table>
  <tr>
    <th>Заголовок первого столбца</th>
    <th>Заголовок второго столбца</th>
  </tr>
  <tr>
    <td>Данные</td>
    <td>Данные</td>
  </tr>
  <tr>
    <td>Данные</td>
    <td>Данные</td>
  </tr>
</table>
```

4. Если компонент ведёт на другую страницу, используйте элемент `<a>` независимо от его внешнего вида. Если интерактивный компонент оставляет пользователя на странице, используйте элемент `<button>`.

5. Для разметки областей страницы используйте теги `<header>`, `<main>`, `<article>`, `<nav>`, `<section>`, `<footer>`.
В отличии от тега `<h1>` элемент `<header>` можно использовать несколько раз на странице, например, для заголовка модального окна. Так же, несколько раз можно использовать `<footer>`, `<article>`, `<section>`, `<main>`.

6. Не забывайте использовать редкие теги: `<abbr>` для аббревиатур, `<blockquote>` для цитат, группу тегов `<dl>`, `<dd>`, `<dt>` для определений.
```html
<abbr title="Индивидульный налоговый номер">ИНН</abbr>
```
```html
<dl>
    <dt>Кадровый учёт</dt>
    <dd>Это учёт кадров</dd>
</dl>
```

7. Добавляя атрибут `title` для страницы, помимо названия сервиса дополняйте описанием страницы, например: «Экстерн — создание отчёта».

8. Не скрывайте элементы при помощи `display:none`, а информативные элементы не добавляйте через `:before` или `:after` — скринридер не читает элементы скрытые через `display:none` или добавленные через стили. Для визуального скрытия элемента лучше используйте следующие стили:
```css
.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  border: 0;
}
```

9. Если вы не используете необходимый тэг, но хотите придать элементу семантичное значение, не используйте атрибут `role`. Скринридер прочитает элемент как кнопку, но вам придётся добавлять управление фокусом, обработку нажатий клавиш с клавиатуры. Используйте необходимый тэг вместо этого:
```html
/** Плохо */
<div role="button">Нажми на меня!</div>

/** Хорошо! */
<Button>Нажми на меня!</Button>
```


## ARIA атрибуты
1. Если у инпута нет лейбла, задайте его при помощи `aria-label`:
```html
<Input aria-label="Введите имя (обязательно)" />
```

2. Если нет возможности обернуть `<Input />` в `<label>`, используйте `aria-labeledby`:
```html
<label id="label">Я лейбл для поля!</label>
<Input aria-labeledby="label" />
```

3. Если хотите добавить описание элементу, используйте `aria-describedby`:
```html
<span id="button-description">По нажатию на кнопку отправится отчёт, после чего вам придёт уведомление на почту</span>
<Button aria-describedby="button-description">Нажми!</Button>
```

4. Для дизейбла элемента используйте `aria-disabled`, а визуально и интерактивно блокируйте элемент при помощи стилей и JS. Скринридер остановится на этом элементе, тогда как атрибут `disabled` скринридер проигнорирует.
```html
<Input aria-disabled="true"/>
```


## Управляемость с клавиатуры

1. Все элементы управления и ввода (те, с которыми может взаимодействовать пользователь) должны быть фокусируемые. Если элемент по умолчанию не имеет фокуса - можно сделать его фокусируемым при помощи атрибута `tabindex=”0”`.

2. Если при взаимодействии с контентом страницы открывается `<Modal>` или `<SidePage>`, то при нажатии на `tab` должен фокусироваться контент модального окна\сайдпейджа в первую очередь, на первый интерактивный элемент или на заголовок в случае скрин-ридера.

3. Тестируйте выполнение основных сценариев с клавиатуры: перемещение при помощи `tab`, `shift+tab`, управление контролами при помощи стрелочек клавиатуры, `enter`, `escape`.

4. Не блокируйте зум на сайте. Делайте странички сайта адаптивными для изменений масштабирования. Поддерживайте масштабирование минимум до 150%.

5. При переключении клавишей `tab` элементы формы должны переключаться в нужной последовательности, один за одним.

6. Не злоупотребляйте атрибутом tabindex. Если на странице появятся новые элементы, легко запутаться в установленных значениях. При ошибках использования, например, повторяющихся значениях tabindex, навигация по DOM путается — пользователю скринридера будет сложно перемещаться по странице.

7. Старайтесь чтобы визуальное расположение элементов на сайте повторялось в разметке, DOM-дереве. Есть стили (например, `flex-direction: row-reverse`), которые меняют визуальный порядок элементов на странице. При это в `DOM` порядок остаётся прежним, и скринридер прочитает его как в `DOM`, но на странице эти элементы будут расположены по-другому. Это же правило касается блоков с `position: absolute | sticky | fixed` – стилей, выносящих блоки из потока.


## Изображения

1. Оставляйте пустой атрибут `alt` для декоративных картинок и заполняйте его для информативных элементов. Не стоит писать в `alt` большие объёмы текста, в общих чертах описывайте самое важное.

2. Для сложных изображений\графиков дублируйте информацию в текстовом варианте.

```html
<picture>
  <img src=".../images/graph.png" alt="На графике изображено количество принятых отчётов с первого раза. С первого раза принимаются 80% отчётов за год, 10% со второй попытки и оставшиеся 10% с третьей или больше.">
</picture>
```

## Валидация

1. Не используйте символ * как обозначение обязательности поля:

    __Ваше имя*:__

    Лучше явно подписать обязательность:

    __Ваше имя (обязательно):__

    Либо явно написать «__обязательные поля обозначены символом *__»

    Дело в том, что символ * голосовые ассистенты читают как «star» или «звёздочка», в зависимости от языка

2. Лучше всего валидировать формы по нажатию на кнопку `submit`, в случае ошибки переносить фокус на текст ошибки. Если же валидация формы происходит после потери фокуса, пользователь скринридера не узнает об этом. Ему придётся искать ошибку наугад.

3. Текст об ошибке лучше размещать рядом с полем, визуально отделять его цветом и иконкой. Так же, текст ошибки лучше размещать в `<label>` этого поля, для того, чтобы после фокусировки на поле при помощи скринридера, он прочитал ошибку и пользователь мог исправить проблему.

```html
<h1>Пользовательские данные</h1>
<span>Обязательные поля обозначены символом *</span>

<form>
  <label htmlFor="name">Имя* {isNameError && "содержит ошибку"}</label>
  <Input id="name" placeholder="Введите ваше имя" />
  <label htmlFor="lastname">Фамилия {isLastNameError && "содержит ошибку"}</label>
  <Input id="lastname" placeholder="Введите вашу фамилию" />

  <Button onClick={checkErrors}>Отправить</Button>
</form>
```


## Примеры доступных компонентов

1. В кнопку-иконку добавьте текст с описанием действия и скройте его с помощью специального класса. Этим же способом можно подписывать отдельно стоящие иконки.
```html
<Button>
  <Icon />
  <span className="visually-hidden">Нажми на меня!</span>
</Button>
```

```css
.visually-hidden {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  border: 0;
}
```

2. Указывайте `<label>` для каждого поля формы. Если поле нельзя обернуть в `<label>`, используйте атрибуты `htmlFor` и `id`:

```html
<label htmlFor="input-id">Имя Фамилия</label>
<input id="input-id"/>
```

Либо используйте `aria-label`:

```html
<input aria-label="Имя Фамилия" id="input-id"/>
```

Основывайте собственные компоненты на встроенных компонентах браузера — они учитывают все кейсы и сложности работы с компонентом, особенно работу с клавиатуры. Реализуйте [стандартные паттерны](https://www.w3.org/TR/wai-aria-practices/#aria_ex).


## Инструменты для проверки вёрстки

1. Браузеры имеют встроенные инструменты проверки вёрстки на контрастность и семантику разметки. Используйте их при вёрстке.

2. [Расширение браузера aXe](https://www.deque.com/axe/) проверит и выведет ошибки в панель разработчика браузера. Также его можно внедрить как шаг в CI.

3. [Расширение Lighthouse](https://developers.google.com/web/tools/lighthouse/) от Google позволяет провести аудит сайта на основные правила доступности.


## Библиотеки для проверки доступности в коде

1. [@storybook/addon-a11y](https://storybook.js.org/addons/@storybook/addon-a11y) — аддон для Storybook, находящий ошибки в вёрстке, а также позволяющий имитировать некоторые заболевания глаз.

2. [eslint-plugin-jsx-a11y](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y) — плагин для ESLint, проверяющий код на доступность.

3. [jest-axe](https://github.com/nickcolley/jest-axe) — инструмент для написания тестов на доступность.