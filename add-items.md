# Добавим элементы

Мы хотим добавить элементы в наш список. С Angular, мы можем сделать это легко и увидеть добавленный элемент немедленно. Сделаем это внутри `input-button-unit` компонента, который создали ранее. Изменения наступят при нажатии на клавишу Enter или нажав кнопку отправки, значение в поле ввода станет заголовком нового элемента, который  будет добавлен в список.

Но мы не хотим, чтобы `input-button-unit` компонент отвечал за добавление нового элемента в список. Нам нужно чтобы у него была минимальная ответственность, и **делегируем действие его родительскому компоненту**. Одно из преимуществ такого подхода заключается в том, что этот компонент может быть повторно использован и привязан к другому событию в разных ситуациях.

Например, в нашем случае, мы сможем использовать `input-button-unit` внутри `todo-item` компонента. Тогда будет поле ввода для каждого элемента и сможем редактировать заголовок элемента. В этом случае, нажатие клавиши «Ввод» или кнопка «Сохранить» будут иметь разные результаты.

Так что мы действительно хотим сделать, это  **вызывать событие** в `input-button-unit` компоненте каждый раз, когда заголовок изменяется. С Angular, мы можем легко определять и вызывать события из наших компонентов!

## @Output\(\)

Добавьте строку внутри `InputButtonUnitComponent` класса, которая определяет выход для компонента:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit.component.ts" %}
```typescript
@Output() submit: EventEmitter<string> = new EventEmitter();
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Свойство вывода называется `submit`. Это тип `EventEmitter` у которого есть метод `emit`. `EventEmitter` это Общий Тип - мы передаем ему другой тип, который будет использоваться внутри, в данном случае `string`. Это тип объекта, который будет выделен спомощью `emit` метода. 

Убедитесь, что `Output` и `EventEmitter` добавлены в импорт в первой строке файла:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit.component.ts" %}
```typescript
import { Component, OnInit, Input, Output, EventEmitter } from '@angular/core';
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Сейчас, когда мы вызовем `this.submit.emit()`, произойдет событие в родительском компоненте. Давайте вызовем его в `changeTitle` методе:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit.component.ts" %}
```typescript
changeTitle(newTitle: string) {
  this.submit.emit(newTitle);
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Мы делегируем все родительскому компоненту - даже фактически меняя заголовок элемента, при необходимости. 

Передаем `newTitle` при вызове события. Чтобы мы не передавали в  `emit()` это будет доступно для родителя в `$event`.

Ничего не меняется в `todo-input` компоненте. События выполняющиеся на `keyup.enter` и `click` попрежнему вызывают тот же метод, но сам метод изменился.

Поначалу имя метода может показаться неуместным. Давайте перейдем к чему-то более подходящему: `submitValue`. Вы можете использовать инструменты IDE для рефакторинга имени метода - убедитесь, что он также изменен в шаблоне.

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit.component.ts" %}
```typescript
submitValue(newTitle: string) {
  this.submit.emit(newTitle);
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
template: `
  <input #inputElementRef
         [value]="title"
         (keyup.enter)="submitValue($event.target.value)">

  <button (click)="submitValue(inputElementRef.value)">
    Save
  </button>
`,
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Прослушивание события

Теперь нам нужно поймать событие в родительском компоненте и добавить к нему логику. Перейдите к `app-root` компоненту и привяжим `submit` событие к  `<app-input-butoon-unit>` компоненту:

{% code-tabs %}
{% code-tabs-item title="src/app/app.component.ts" %}
```markup
<app-input-button-unit (submit)="addItem($event)"></app-input-button-unit>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Все, что осталось - это реализовать `addItem` метод, который получает строку ,и создает объект со свойством  `title`, затем добавьте его в список:

{% code-tabs %}
{% code-tabs-item title="src/app/app.component.ts" %}
```typescript
addItem(title: string) {    
  this.todoList.push({ title });
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Попробуйте - введите новый заголок в поле ввода и отправьте его!

**Примечание:** Используется **ES6 Object Property Value Shorthand**  для создания объекта item todo. Если мы используем переменную с тем же именем что и свойство, мы можем использовать сокращенную запись. В нашем случае, `{ title }` эквивалентно `{ title: title }`. Если строка хранится в переменной под другим именем , мы не можем использовать короткую запись. Например: 

{% code-tabs %}
{% code-tabs-item title="code for example" %}
```typescript
addItem(value: string) {    
  this.todoList.push({ title: value });
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

