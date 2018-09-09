# Добавим элементы

Мы хотим добавить элементы в наш список. С Angular, мы можем сделать это легко и увидеть добавленный элемент немедленно. Сделаем это внутри `input-button-unit` компонента, который создали ранее. Изменения наступят при нажатии на клавишу Enter или нажав кнопку отправки, значение в поле ввода станет заголовком нового элемента, и новый элемент будет добавлен в список.

Но мы не хотим, чтобы `input-button-unit` компонент отвечал за добавление нового элемента в список. Мы хотим чтобы у него была минимальная ответственность, и **делегируем действие его родительскому компоненту**. Одно из преимуществ такого подхода заключается в том, что этот компонент может быть повторно использован, и могжет быть привязан к другому действию в разных ситуациях.

Например, в нашем случае, мы сможем использовать `input-button-unit` внутри `todo-item` компонента. Тогда будет поле ввода для каждого элемента и сможем редактировать заголовок элемента. В этом случае, нажатие клавиши «Ввод» или кнопка «Сохранить» будут иметь разные действия.

Так что мы действительно хотим сделать, это  **вызвать событие** с `input-button-unit` компонента каждый раз, когда заголовок изменяется. С Angular, мы можем легко определять и вызывать события из наших компонентов!

## @Output\(\)

Добавьте строку внутри `InputButtonUnitComponent` класса, которая определяет выход для компонента:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit.component.ts" %}
```typescript
@Output() submit: EventEmitter<string> = new EventEmitter();
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Свойство вывода называется `submit`. Это тип `EventEmitter` у которого есть метод `emit`. `EventEmitter` это Общий Тип - мы передаем ему другой тип, который будет использоваться внутри, в этом случае `string`. Это тип объекта, который будет выделен спомощью `emit` метода. 

Убедитесь, что `Output` и `EventEmitter` добавлены в импорт в первой строке файла:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit.component.ts" %}
```typescript
import { Component, OnInit, Input, Output, EventEmitter } from '@angular/core';
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Сейчас, когда мы вызовем `this.submit.emit()`, произойдет событие в родительском компоненте. Даввайте вызовем его в `changeTitle` методе:

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

Нисего не меняется в `todo-input` компоненте. События выполняющиеся на `keyup.enter` и `click` попрежнему вызывают тот же метод, но сам метод изменился.

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

Теперь все, что нам нужно сделать, это поймать событие в родительском компоненте и дпбавить к нему логику. Перейдите к `app-root` компоненту и привяжите  `submit` событие к  `<app-input-butoon-unit>` компоненту:

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

**Примечание:** Используется **ES6 Object Property Value Shorthand**  для создания объекта item todo. Если мы используем переменную с тем же именем что и свойство, мы можем использовать сокращенную запись. В нашем случае, `{ title }` эквивалентно `{ title: title }`. Если строка хранится в переменной под другим именем , мы не могли бы использовать короткую запись. Например: 

{% code-tabs %}
{% code-tabs-item title="code for example" %}
```typescript
addItem(value: string) {    
  this.todoList.push({ title: value });
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

