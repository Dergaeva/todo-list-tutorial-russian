# Event binding

Мы хотим, чтобы наше приложение реагировало на действия пользователя. А именно, обновлять заголовок пункта todo всякий раз, когда пользователь меняет его, или добавлять новый элемент, когда пользователь нажимает кнопку «Сохранить» или клавишу «Ввод».

У нас еще нет полного списка, но на данный момент мы будем использовать другой способ для проверки действия. В дальнейшем мы изменим его на правильную функциональность.

`input-button-unit` компонент должен выглядеть так:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-input-button-unit',
  template: `
    <p>
      input-button-unit works!
      The title is: {{ title }}
    </p>
    
    <input [value]="title">
    <button>Save</button>
  `,  
  styleUrls: ['./input-button-unit.component.css']  
})    
export class InputButtonUnitComponent implements OnInit {
  title = 'Hello World';           

  constructor() { }                     

  ngOnInit() {
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Действие

Во-первых, давайте имплементируем `changeTitle`. Он получит новое название в качестве аргумента. Лучшая практика заключается в том, чтобы наши пользовательские методы были написаны после методов жизненного цикла \(`ngOnInit` в этом случае\):

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
changeTitle(newTitle: string) {
  this.title = newTitle;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Привязка к событиям

Так же, как привязываемся к свойствам элемента, мы можем привязываться к событиям, которые выполняются элементами. Снова, Angular дает нам простой способ сделать это. **Вы просто оборачиваете имя события скобками и передаете ему метод, который должен быть выполнен, когда событие выполнится**.

Попробуем простой пример, где название меняется, когда пользователь нажимает кнопку. Обратите внимание на скобки вокруг `click`. \(Мы также изменим привязку значения ввода обратно на `title`.\)

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
template: `
  <p>
    input-button-unit works!
    The title is: {{ title }}
  </p>
  
  <input [value]="title">
  <button (click)="changeTitle('Button Clicked!')">
    Save
  </button>
`,
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> Событие называется `click` , а не `onClick` - в Angular вы удаляете  `on` префикс в событиях элементов.

Перейдите в браузер и посмотрите результат - нажмите кнопку «Сохранить».

## Event Data

Мы передаем статическую строку вызову метода: `Button Clicked!'` Но мы хотим передать значение, введенное пользователем в поле ввода!

В следующей главе мы узнаем, как использовать свойства одного элемента в другом элементе в том же шаблоне. И после, мы сможем завершить реализацию события клика на кнопке «Сохранить».
Но теперь мы привяжем метод к событию  input: когда пользователь нажимает Enter, вызывается метод `changeTitle`.

### 'keyup' событие

Когда пользователь печатает, срабатывают события клавиатуры. Например `keydown` и `keyup`. Мы можем отлавить `keyup` событие \(когда нажата клавиша\) и поменять название:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
<input [value]="title" (keyup)="changeTitle('Button Clicked!')">
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Теперь, когда пользователь вводит данные, название изменяется на «Button Clicked!». Но это по-прежнему статическая строка.

**Совет:** Когда элемент становится длинным из-за его атрибутов, вам следует для более легкого понимания, разделить его на несколько строк:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
<input [value]="title" 
       (keyup)="changeTitle('Button Clicked!')">
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### The $event object

Теперь мы просто реагируем, когда происходит событие «keyup». Angular позволяет самому получить объект события. Он передается привязке события как `$event` - поэтому мы можем использовать его, когда вызываем `changeTitle()`.

Объект события, выполненный на событиях «keyup», имеет ссылку на элемент, который выполнил событие - input элемент. Ссылка хранится в свойстве события `target`. Как мы видели ранее, элемент ввода имеет свойство `value`, которое содержит текущую строку, которая находится в поле ввода. Мы можем передать `$ event.target.value` метод:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
<input [value]="title" 
       (keyup)="changeTitle($event.target.value)">
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Проверьте это в браузере. Теперь с каждым нажатием клавиши вы можете увидеть изменения названия и отобразить вводимое значение.

### Нажатие клавиши Enter

Вы можете ограничить изменение только специальным нажатием клавиши, в нашем случае это клавиша Enter. Angular делает это очень легко для нас. Событие `keyup` имеет свойства, которые являются более конкретными событиями. Поэтому просто добавьте имя ключа, который вы хотите прослушать, - `keyup.enter`:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
<input [value]="title" 
       (keyup.enter)="changeTitle($event.target.value)">
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Теперь заголовок будет изменяться только тогда, когда пользователь нажимает клавишу Enter во время ввода данных.

### Explore the $event

 ![lab-icon](.gitbook/assets/lab%20%281%29.jpg)**Playground: **Вы можете изменить метод changeTitle для записи объекта `$ event` в консоли. Таким образом, вы можете изучить его и посмотреть, какие у него свойства.

Измените метод `changeTitle`:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
changeTitle(event): void {
  console.log(event);
  this.title = event.target.value; // the original functionality still works
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Теперь измените аргумент, который вы передаете в шаблоне, чтобы передать весь `$event`:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
<input [value]="title" 
       (keyup.enter)="changeTitle($event)">
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Попробуйте!

Вы можете сделать то же самое с элементом `button`:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
  <button (click)="changeTitle($event)">
    Save
  </button>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**Не забудьте изменить код перед тем, как продолжить!**

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-input-button-unit',
  template: `
    <p>
      input-button-unit works!
      The title is: {{ title }}
    </p>
    
    <input [value]="title" 
           (keyup.enter)="changeTitle($event.target.value)">
       
    <button (click)="changeTitle('Button Clicked!')">
      Save
    </button>
  `,  
  styleUrls: ['./input-button-unit.component.css']  
})    
export class InputButtonUnitComponent implements OnInit {
  title = 'Hello World';           

  constructor() { }                     

  ngOnInit() {
  }
  
  changeTitle(newTitle: string) {
    this.title = newTitle;
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

