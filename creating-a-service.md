# Создание Сервиса

В Angular, сервис \ (обычно \) это класс JavaScript, который отвечает за выполнение конкретной задачи, требуемой вашим приложением.В нашем todo-list приложении, мы создадим сервис, который будет отвечать за сохранение и управление всеми задачами, и будем использовать его внутри компонентов.

## Создадим сервис спомощью Angular CLI:

```text
ng g s services/todo-list
```

Эта команда будет генерировать сервис в файле `src/app/services/todo-list.service.ts`. Сервис - это простой класс, называемый `TodoListService` у которого есть декоратор `@Injectable`, что позволяет использовать внедрение зависимостей.

{% code-tabs %}
{% code-tabs-item title="src/app/services/todo-list.service.ts" %}
```text
import { Injectable } from '@angular/core';

@Injectable()
export class TodoListService {

  constructor() { }

}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Предоставление сервиса

В версии 6 Angular CLI вам не нужно предоставлять сервис самостоятельно - CLI добавляет его в корневой каталог `NgModule`. Но вы можете продолжить читать, чтобы понять, что происходит и что это значит.

Чтобы начать пользоваться сервисом, нужно предоставить его в `NgModule`. У нас всего один `NgModule` в нашем приложении - `AppModule` расположенном в  `/src/app/app.module.ts`. Это пустой класс, которому предшествует `@NgModule` декоратор, которому мы передаем объект конфигурации. Одним из свойств этого объекта является `providers` список, который в настоящее время пуст. Добавим наш новый сервис в список.

{% code-tabs %}
{% code-tabs-item title="src/app/app.module.ts" %}
```typescript
@NgModule({
  declarations: [
    AppComponent,
    InputButtonUnitComponent,
    TodoItemComponent,
    ListManagerComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [TodoListService],
  bootstrap: [AppComponent]
})
export class AppModule { }
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Массив `providers` сообщает Angular, как предоставить сервис, который мы ищем \ (обычно в компоненте или другом сервисе \) На этот раз рецепт прост: Когда мы запрашиваем класс «TodoListComponent», мы ожидаем получить экземпляр этого класса. Angular будет создавать только один экземпляр, к которому мы можем получить доступ из любого места в нашем приложении \ (a Singleton \), поэтому мы можем использовать его для обмена данными между различными частями приложения.

Убедитесь, что сервис импортирован:

{% code-tabs %}
{% code-tabs-item title="src/app/app.module.ts" %}
```typescript
import { TodoListService } from './services/todo-list.service';
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Совместное использование данных

Теперь мы можем переместить массив `todoList` из` ListManagerComponent` в новый сервис. Перейдите к сгенерированному файлу сервиса, `src/app/services/todo-list.service.ts`, и добавьте этот код внутри класса `TodoListService`  чуть выше `constructor`:

{% code-tabs %}
{% code-tabs-item title="src/app/services/todo-list.service.ts" %}
```typescript
private todoList: TodoItem[] = [  {title: 'install NodeJS'},
  {title: 'install Angular CLI'},
  {title: 'create new app'},
  {title: 'serve app'},
  {title: 'develop app'},
  {title: 'deploy app'},
];
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Убедитесь, что интерфейс TodoItem импортирован:

{% code-tabs %}
{% code-tabs-item title="src/app/services/todo-list.service.ts" %}
```typescript
import { TodoItem } from '../interfaces/todo-item';
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Создание метода для возврата списка

Мы добавим метод getTodoList, который вернет массив todoList. Сервис будет выглядеть так:

{% code-tabs %}
{% code-tabs-item title="src/app/services/todo-list.service.ts" %}
```typescript
import { Injectable } from '@angular/core';
import { TodoItem } from '../interfaces/todo-item';

@Injectable()
export class TodoListService {

  private todoList = [
    {title: 'install NodeJS'},
    {title: 'install Angular CLI'},
    {title: 'create new app'},
    {title: 'serve app'},
    {title: 'develop app'},
    {title: 'deploy app'},
  ];

  constructor() { }

  getTodoList() {
    return this.todoList;
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Внедрить и использовать услугу

После создания сервиса мы можем ввести его в наш компонент «list-manager». В Angular внедрять зависимости очень просто. Мы передаем его как параметр в конструкторе - тип параметра - это имя класса сервиса. Angular присваивает экземпляр, который он создал, для имени параметра, и мы можем использовать его из конструктора. Прежде чем приступить к  реализации, давайте посмотрим, как это работает:

```typescript
constructor(todoListService: TodoListService) {
  todoListService.getTodoList();
}
```

Typescript помогает нам -  путем указания ярлыка для определения параметра члена класса. Добавляя  `private` или `public` перед именем параметра он автоматически присваивает `this`. То есть вместо объявления и назначения свойства: 

```typescript
export class ListManagerComponent implements OnInit {
  todoListService: TodoListService;
  
  constructor(todoListService:TodoListService) { 
    this.todoListService = todoListService;
  }
}
```

...мы можем уменьшить количество кода следующим образом:

```typescript
export class ListManagerComponent implements OnInit {
  
  constructor(private todoListService:TodoListService) { }
}
```

Итак, давайте продолжим и используем сервис в компоненте `list-manager`.

* Удалите список  из компонента, сохраните только объявление свойства todoList.
* Внесите «TodoListService» с помощью конструктора. 

```typescript
export class ListManagerComponent implements OnInit {
  todoList: TodoItem[];
  
  constructor(private todoListService:TodoListService) { }
```

* Убедитесь, что «TodoListService» импортирован.

{% code-tabs %}
{% code-tabs-item title="src/app/list-manager/list-manager.component.ts" %}
```typescript
import { TodoListService } from '../services/todo-list.service';
```
{% endcode-tabs-item %}
{% endcode-tabs %}

* Получите список из сервиса в методе `ngOnInit`.

{% code-tabs %}
{% code-tabs-item title="src/app/list-manager/list-manager.component.css" %}
```typescript
ngOnInit() {
  this.todoList = this.todoListService.getTodoList();
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Вам не нужно ничего менять в шаблоне, поскольку мы используем список  с тем же свойством, которое использовали ранее. Кажется, что ничего не изменилось, но вы можете убедиться, что список приходит из сервиса, изменив его \ (добавив элемент, изменив заголовок и т. д. \).

