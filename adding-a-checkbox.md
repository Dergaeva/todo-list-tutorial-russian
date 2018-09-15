# Добавление чекбокса

Теперь мы можем взаимодействовать с нашим списком задач, удаляя элементы. Но что, если мы хотим, чтобы заполненные пункты оставались в списке списке, в виде зачеркнутого текста? Добавим чекбокс!

Расмотрим это подробнее:

* Добавление чекбокса
* Добавим функциональности,- при клике на чекбокс  CSS класс добавляет линию  к элементам, например ~~strikethrough~~ .
* Отредактируем заголовок текста, чтобы он отрабатывал при клике на чекбокс.
* Добавим новый  CSS класс.

Продолжим и добавим чекбокс в `todo-item.component.ts` файл. Поместите следующий код  перед `{{ item.title }}`:

{% code-tabs %}
{% code-tabs-item title="src/app/todo-item/todo-item.component.ts" %}
```markup
<input type="checkbox"/>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Теперь, чтобы чекбокс мог что-то делать, нам нужно добавить `click` обработчик событий, который мы будем называть `completeItem`. Мы также добавим css-класс и обернем элемент спомощью интерполяции. Давайте сделаем это прямо сейчас:

{% code-tabs %}
{% code-tabs-item title="src/app/todo-item/todo-item.component.ts" %}
```markup
<div>
  <input type="checkbox"
         class="todo-checkbox"
         (click)="completeItem()"/>
  {{ item.title }}
</div>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

При клике на чекбокс, будет запущен `completeItem` метод. Давайте поговорим о том, что должен выполнять этот метод. Мы хотим   переключать стили CSS  на заголовке элемента поэтому, когда чекбокс установлен, текст заголовка станет зачеркнутым. Мы также хотим сохранить статус элемента в локальном хранилище. Для достижения этой цели , выделим обновленное событие с новым статусом элемента и отследим его в родительском элементе.

```javascript
export class TodoItemComponent implements OnInit {
  @Input() item: TodoItem;
  @Output() remove: EventEmitter<TodoItem> = new EventEmitter();
  @Output() update: EventEmitter<any> = new EventEmitter();

  // put this method below ngOnInit
  completeItem() {
    this.update.emit({
      item: this.item,
      changes: {completed: !this.item.completed}
    });
  }
```

Одну минуточку! Как это повлияет на название списка когда мы только нажали на чекбокс? Что ж, в Angular есть замечательная директива  NgClass. С помощью нее применяется и удаляется  CSS класс согласно логике \(true or false\). Есть множество вариантов как использовать эту дерективу \(смотри [NgClass directive documentation](https://angular.io/api/common/NgClass)\) но мы сосредоточимся следующем применении:

```markup
<some-element [ngClass]="{'first': true, 'second': true, 'third': false}">...</some-element>
```

«Первый» и «Второй» класс будут применены к элементу т.к их значение true, в то время как  «третий» класс не будет применен т.к. его значение false. Так вот сейчас наш предыдущий код вступает в игру. Метод `completeItem` будет переключаться между  true и false значениями, таким образом диктуя какой из классов будет применен или удален.

Давайте обернем заголовок элемента в  `<span>`, и используем NgClass для применения стилей:

```markup
<span class="todo-title" [ngClass]="{'todo-complete': isComplete}">
  {{ item.title }}
</span>
```

И наконец-то, добавим CSS в `item.component.css` файл:

```css
  .todo-complete {
    text-decoration: line-through;
  }
```

Вуаля! При выборе чекбокса, заголовок должен стать перечеркнутым, а снятие флажка должно удалять линию.

