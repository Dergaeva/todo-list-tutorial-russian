#Элемент ref - \#

В последней главе мы закончили  компонент ввода,  который может отображать и изменять заголовок нашего пункта todo. `input.component.ts` должен выглядеть так:

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

Сначала давайте удалим часть шаблона, который нам не нужен. Удалите эти строки:

{% code-tabs %}
{% code-tabs-item title="remove this from src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
<p>
  input-button-unit works!
  The title is: {{ title }}
</p>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Теперь мы хотим взять значение ввода \(которое пользователь набрал\) и изменить заголовок, при клике на кнопку «Сохранить».

Мы уже знаем, как создавать кнопку и отлаживать ее действия при клике. Теперь нам нужно передать методу некоторые данные из другого элемента. Будем использовать значение `input` элемента внутри `button` элемента.

Angular поможет нам в этом. **Мы можем хранить ссылку на элемент, который мы используем в переменной, с именем, которое мы выбираем,** например `inputElementRef`, **используя простой синтаксис - хэш.** Добавим `#inputElementRef` в `input` элемент, и используем его в событии  `click` для кнопки:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
template: `
  <input #inputElementRef
         [value]="title"
         (keyup.enter)="changeTitle($event.target.value)">

  <button (click)="changeTitle(inputElementRef.value)">
    Save
  </button>
`,
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Теперь мы можем использовать значение, введенное пользователем в `input` элемент в методе, который вызывается при нажатии кнопки `Сохранить`!

### Что означает `#`?

Angular позволяет определить новую локальную переменную с именем `inputElementRef` \(или любое имя, которое вы выберете\) которая содержит ссылку на элемент, ранее определенный, a затем использовать ее в любое время. В нашем случае, мы испозуем ее для достпупа к свойству `value` в `input`.

Вместо того, чтобы искать элементы спомощью запроса к DOM  \(что является плохой практикой, как мы обсуждали\), теперь мы можем поместить ссылки элементов в шаблон и получить доступ к каждому элементу, который хотим декларировать.

Затем мы создадим список пунктов todo

## Исследуйте ссылку на элемент

Как и в предыдущей главе, когда мы регистрировали $event, можем сделать тоже самое для `#inputElementRef`. 

![lab-icon](.gitbook/assets/lab%20%283%29.jpg) **Playground:** Измените метод `changeTitle`, чтобы он получил всю ссылку на элемент и отобразил ее в консоли:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
<input #inputElementRef
       [value]="title"              
       (keyup.enter)="changeTitle(inputElementRef)">

<button (click)="changeTitle(inputElementRef)">
  Save
</button>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

```typescript
changeTitle(inputElementReference) {
  console.log(inputElementReference);
  this.title = inputElementReference.value;
}
```

Не забудьте вернуть код после того, как закончите экспериментировать! Лучше всего передать методу именно то значение, которое ему нужно, вместо всего объекта.

## Ресурсы

[Angular Template Reference Variables](https://angular.io/guide/template-syntax#template-reference-variables--var-)

