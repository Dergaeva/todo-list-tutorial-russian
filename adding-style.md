# Добавление стиля

C Angular, мы можем добавить стиль к компонентам таким образом, чтобы это не повлияло на остальную часть приложения. Это хорошая практика инкапсуляции связанных компонентов.

Мы также можем указать общие правила стиля которые будут использоваться в приложении, что полезно для создания  внешнего вида интерфейса всех компонентов. Например, выбрать какая цветовая палитра будет использована в теме приложения. Затем,  поменять цвета или использовать другие темы,  внести изменения только в одном месте, а не в каждом компоненте.

Angular предлагает различные методы инкапсуляции стиля, но мы будем придерживаться стандартного.

Angular CLI уже сгенерировал общую таблицу стилей `src/style.css`. Вставьте следующий код в этот файл:

{% code-tabs %}
{% code-tabs-item title="src/style.css" %}
```css
html, body, div, span,
h1, p, ul, li {
  padding: 0;
  border: 0;
  font: inherit;
  vertical-align: baseline;
}

body {
  background: #f1f1f1;
  font-size: 16px;
  line-height: 22px;
  color: #404040;
  font-family: 'Lucida Grande', Verdana, sans-serif;
}

ol, ul {
  list-style: none;
}

.btn {
  background: lightseagreen;
  color: #fff;
  padding: 3px 10px;
  margin: 0 0 0 3px;
  border: none;
  border-radius: 5px;
  font-size: 12px;
  line-height: 24px;
  cursor: pointer;
  vertical-align: bottom;
}

.btn:hover {
  background: lightslategrey;
}

.btn-red {
  background: red;
}

.btn-red:hover {
  background: darkred;
}

.app-title {
  font-size: 52px;
  line-height: 52px;
  margin-bottom: 30px;
  font-weight: bold;
  text-align: center;
  letter-spacing: -0.8px;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> Откуда проекту знать, что нужно смотреть в этот файл? В Angular CLI файл конфигурации `.angular-cli.json` снизу `apps[0].styles`, вы можете указать файлы ,которые хотите добавить в проект. Откройте инструменты браузера и Вы увидите стили элемента:
>
> ```markup
> <html>
>  ...
>  <head>
>    ...
>    <style type="text/css">
>    ...Your style is here
>    </style>
>    ...
>  </head>
>  ...
> </html>
> ```

Мы добавили стили непосредственно к элементам \(`html, body, div, span, h1, p, ul, li`\), которые сразу же применились.Также добавили стили используя селекторы css-классов. Осталось добавить имена классов в соответствующие элементы.

В `app-root` добавить класс `app-title` в `h1`:

{% code-tabs %}
{% code-tabs-item title="src/app/app.component.ts" %}
```markup
template: `
  <h1 class="app-title">
    Welcome to {{ title }}!
  </h1>

  <app-list-manager></app-list-manager>
`,
```
{% endcode-tabs-item %}
{% endcode-tabs %}

В `input-button-unit` добавить `btn` класс в `button`:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
<button class="btn"
        (click)="submitValue(inputElementRef.value)">
  Save
</button>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Теперь добавим отдельные стили  для компонента.

Добавьте следующий стиль в `input-button-unit.component.css`:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.css" %}
```css
.todo-input {
  padding: 4px 10px 4px;
  font-size: 16px;
  font-family: 'Lucida Grande', Verdana, sans-serif;
  line-height: 20px;
  border: solid 1px #dddddd;
  border-radius: 5px;
  flex-grow: 1;
}

:host(:not([hidden])) {
  display: flex;
  justify-content: space-between;
  flex-grow: 1;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Как эти стили привязана к `input-button-unit` компонентам? Посмотрите файл `input-button-unit.component.ts`. Одно из свойств объекта передается в `@Component` с декоратором `styleUrls`. Это список стилей, которые будут использоваться в Angular, и которые инкапсулируются  внутри компонента.

Селектор :host применяется к элементу, который содержит этот компонент - `<app-input-button-unit>`. Этот элемент не является частью шаблона компонента. Он отображается в шаблоне родителя - это то, как мы можем контролировать стили изнутри компонента.

Нам нужно добавить `todo-input` класс  в `input` элемент:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
<input class="todo-input"
       #inputElementRef
       [value]="title"
       (keyup.enter)="submitValue($event.target.value)">
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Теперь давайте добавим стиль отдельно к `list-manager` компоненту.Для этого откройте файл `list-manager.component.css` и вставьте следующий стили во внутрь:

{% code-tabs %}
{% code-tabs-item title="src/app/list-manager/list-manager.component.css" %}
```css
.todo-app {
  position: relative;
  width: 400px;
  padding: 30px 30px 15px;
  background: white;
  border: 1px solid;
  border-color: #dfdcdc #d9d6d6 #ccc;
  border-radius: 2px;
  -webkit-box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
  margin: 20px auto;
}

.todo-app::before, .todo-app::after {
  content: '';
  position: absolute;
  z-index: -1;
  height: 4px;
  background: white;
  border: 1px solid #ccc;
  border-radius: 2px;
}

.todo-app::after {
  bottom: -3px;
  left: 0;
  right: 0;
  -webkit-box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
}

.todo-app::before {
  bottom: -5px;
  left: 2px;
  right: 2px;
  border-color: #c4c4c4;
  -webkit-box-shadow: 0 1px 2px rgba(0, 0, 0, 0.15);
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.15);
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Оборачиваем содержимое компонента в  `<div>` с `todo-app` классом.

{% code-tabs %}
{% code-tabs-item title="src/app/list-manager/list-manager.component.ts" %}
```markup
template: `
  <div class="todo-app">
    <app-input-button-unit (submit)="addItem($event)"></app-input-button-unit>

    <ul>
      <li *ngFor="let todoItem of todoList">
        <app-todo-item [item]="todoItem"></app-todo-item>
      </li>
    </ul>
  </div>
`,
```
{% endcode-tabs-item %}
{% endcode-tabs %}

В завершении, добавьте следующие стили в  `todo-item.component.css`:

{% code-tabs %}
{% code-tabs-item title="src/app/todo-item/todo-item.component.css" %}
```css
.todo-item {
  padding: 10px 0;
  border-top: solid 1px #ddd;
  min-height: 30px;
  line-height: 30px;
  display: flex;
  justify-content: space-between;
}

.todo-checkbox {
  flex-shrink: 0;
  margin: auto 1ex auto 0;
}

.todo-title {
  flex-grow: 1;
  padding-left: 11px;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Оберните содержимое `todo-item` компонента в `div` с `todo-item` классом:

```markup
<div class="todo-item">
  {{ item.title }}
</div>
```

Классы `todo-checkbox` и `todo-title` будут использованы позже.

Поменяйте стили на Ваше усмотрение - размер элементов, цвета - как пожелаете!

Примечание: Могут быть использованы SCSS файлы в проекте, -  это отличный способ написания стилей и хорошая возможностьупростить разработку. SCSS файлы компилируются с CSS после того как проект будет собран.

