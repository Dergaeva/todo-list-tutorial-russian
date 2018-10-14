# Класс

Класс представляет собой специальную программную структуру. Он определяется с помощью **членов** которые могут быть **свойствами** \(переменными\) и **методами** \(функциями\). Затем создаются экземпляры класса, обычно с названием `new` оператор класса: `let myInstance = new myClass();`. Созданный экземпляр - это объект, с помощью которого вы можете вызвать методы класса и получить, и установить значения его свойств. Несколько экземпляров могут быть созданы из одного класса.

## В Angular...

Angular отслеживает создание экземпляров классов, которые вы определяете - если они признаны как строительные блоки Angular. Декораторы отвечают за соединение с Angular.

Каждый раз, когда вы используете компонент в шаблоне, создается новый экземпляр. Например, здесь будут созданы три экземпляра класса InputButtonUnitComponent:

{% code-tabs %}
{% code-tabs-item title="src/app/app.component.ts" %}
```markup
// example only

template: `
  <app-input-button-unit></app-input-button-unit>
  <app-input-button-unit></app-input-button-unit>
  <app-input-button-unit></app-input-button-unit>
`,
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Давайте посмотрим на класс `InputButtonUnitComponent`.

## выполнение OnInit

Во-первых, вы видите, что что-то было добавлено в объявление класса:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
export class InputButtonUnitComponent implements OnInit {
  ...
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

`OnInit` является **интерфейсом** - структура, определенная, но не реализованная как класс. Она определяет, какие свойства и / или методы должны существовать в классе, который его реализует. В этом случае, `OnInit` является интерфейсом компонентов Angular, который реализуют метод `ngOnInit`. Этот метод является **метод жизненного цикла компонентов**. Angular вызовет этот метод после создания экземпляра компонента.

Angular CLI добавляет это утверждение, чтобы напомнить нам, что лучше всего инициализировать элементы компонента через  `ngOnInit` метод. Вы также можете увидеть, что он добавил метод в тело класса:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
ngOnInit() {
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Вы можете использовать этот метод без явного указания того, что класс реализует `OnInit` интерфейс, но полезно использовать утверждение. Чтобы узнать почему, удалите метод `ngOnInit`. IDE сообщит вам, что есть ошибка - вы должны имплементоровать `ngOnInit`.Откуда это известно? Из-за отсутствия `implements OnInit`.

## конструктор

Другой метод, который мы не видели в `app-root` компоненте является конструктор.Это метод, который вызывает JavaScript при создании экземпляра класса. Все, что находится внутри этого метода, используется для создания экземпляра. Поэтому он вызывается до `ngOnInit`.

> Важной особенностью Angular, который использует конструктор, является внедрение зависимостей. Мы расмотрим это позже, когда начнем использовать сервисы.

## Свойства

Свойство `title`, которое мы добавили, используется для хранения значения, в нашем случае тип строка. Каждый экземпляр класса будет иметь собственное свойство `title`, что означает, что вы можете изменить значение `title` в одном экземпляре, но в остальных случаях оно останется прежним.

В TypeScript,мы должны объявлять членов класса либо в классе вне любого метода, или передать их конструктору - как мы увидим, когда будем использовать сервис.
Вы можете объявить свойство без его инициализации:

```typescript
title: string;
```

Затем вы можете присвоить значение на более позднем этапе, например, в конструкторе или в методе ngOnInit. Здесь мы явно отметили, что `title` имеет тип` string`. \ (Тип выводится с помощью TypeScript, когда мы сразу присваиваем значение, поэтому нет необходимости добавлять этот тип в этом случае. \)

Когда вы ссылаетесь на член класса из метода класса, вы должны добавлять префикс `this`. Это специальное свойство, указывающее на текущий экземпляр.

Попробуйте установить другое значение для `title` изнутри конструктора. Посмотрите результат в браузере:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
title = 'Hello World';

constructor() { 
  this.title = 'I Love Angular';
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Попробуйте изменить значение `title` внутри метода` ngOnInit`. Какое значение будет отображаться на экране?

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
title: string = 'Hello World';

constructor() { 
  this.title = 'I Love Angular';
}

ngOnInit() { 
  this.title = 'Angular CLI Rules!';
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Методы

Давайте добавим метод, который изменяет значение `title` в соответствии с аргументом, который мы добавим. Назовем его `changeTitle`. Метод будет иметь один параметр типа `string`. Добавьте его внутри тела класса ** \ (но не внутри другого метода \):

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
changeTitle(newTitle: string) {
  this.title = newTitle;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

** Примечание: ** Функции и методы могут возвращать значение, которое может использоваться при вызове метода. Например:

{% code-tabs %}
{% code-tabs-item title="code for example" %}
```typescript
function multiply (x: number, y: number) {
  return x * y;
}

let z = multiptly(4, 5);
console.log(z);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Метод `changeTitle` пока что нигде не используется. Мы можем вызвать его другим способом или в шаблоне \(которые мы рассмотрим в следующих главах\). Вызовем его в конструкторе.

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
constructor() { 
  this.changeTitle('My First Angular App');
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

![lab-icon](.gitbook/assets/lab%20%281%29.jpg) **Playground**: Вы можете попробовать вызвать метод с разными аргументами \ (строка, переданная внутри скобок \) из `ngOnInit`. Попробуйте вызвать его до или после присвоения значения непосредственно заголовку. Попробуйте вызвать его несколько раз из того же метода. Посмотрите результат в браузере.

## Совет для дебага

Вы всегда можете использовать `console.log(someValue)` внутри методов класса. Затем значение, которое вы передали в качестве аргумента, будет отображено в консоли браузера. Таким образом вы можете видеть порядок выполнения методов и значение аргумента, который вы передаете \ (если это переменная \). Например:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
constructor() { 
  console.log('in constructor');
  this.changeTitle('My First Angular App');
  console.log(this.title);
}

changeTitle(newTitle: string) {
  console.log(newTitle);
  this.title = newTitle;
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Консоль браузера входит в состав инструментов Dev Tools. Вы можете увидеть, как открыть консоль в разных браузерах здесь: [https://webmasters.stackexchange.com/questions/8525/how-do-i-open-the-javascript-console-in-different-browsers](https://webmasters.stackexchange.com/questions/8525/how-do-i-open-the-javascript-console-in-different-browsers)

