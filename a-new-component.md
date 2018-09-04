# Новый компонент

В этой главе мы создадим совершенно новый компонент. Это позволит нам добавить элемент в todo list, который будет содержать HTML элементы `input` и `button`. Назовем его Input-Button-Unit.

Используем Angular CLI для создания всех необходимых файлов и шаблонов. Angular CLI выполняет команды в окне терминала, что означает, что мы должны остановить процесс `ng serve`. Вместо этого, можно открыть новое окно терминала или вкладку и запустить дополнительные команды оттуда. Изменения будут немедленно отображены в браузере.

Откройте другую вкладку терминала и запустите:

```text
ng g c input-button-unit
```

Как мы видели ранее, `ng`  команда также используется в Angular CLI. `g` это сокращенное `generate`. `c` сокращенное `component`. `input-button-unit` -  имя которое мы присваиваем компоненту.

Таким образом, полная версия команды \(не запускайте ее\):

```text
ng generate component input-button-unit
```

Давайте посмотрим, что Angular CLI создал для нас.

А именно, новую папку `src/app/input-button-unit` с тремя файлами \(или четырьмя, если вы не используете встроенный шаблон\):

* `input-button-unit.component.css` - здесь будут размещены стили, отдельно для данного компонента.
* `input-button-unit.component.spec.ts` - файл для тестирования компонента. Не будем использоваться в учебном пособии.
* `input-button-unit.component.ts` - файл компонента, в котором будет определяться логика.
* `input-button-unit.component.html` -  файл шаблона HTML, если вы не используете встроенный шаблон.

Откройте файл `input-button-unit.component.ts`. Вы увидите Angular CLI создал для конфигурацию компонента, включая селектор,  имя которого начинается с префикса `app`, и шаблон по умолчанию:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
@Component({
  selector: 'app-input-button-unit',
  template: `
    <p>
      input-button-unit works!
    </p>
  `,
  styleUrls: ['./input-button-unit.component.css']
})
```
{% endcode-tabs-item %}
{% endcode-tabs %}

> Префикс `app` будет добавлен в селектор элемента всех компонентов, которые вы создадите.Это необходимо для того, чтобы избежать конфликта имен с другими компонентами и элементами HTML. Например, если вы создадите компонент с именем input, он не будет конфликтовать с HTML's `<input />` элементом, т.к. имя селектора будет  `app-input`.
>
> `app` префикс по умолчанию, который удобен для вашего основного приложения. Однако, если вы пишете библиотеку компонентов, которые будут использоваться в других проектах, лучше выбрать другой префикс. Например,  [Angular Material](https://material.angular.io/) библиотека использует префикс `mat`. Вы можете создать проект с указанием префикса на выбор, используя флаг `--prefix`, или изменить его позже в файле`.angular-cli.json`.

Мы будем использовать этот компонент как есть и наблюдать за результатом!

Откройте файл корневого компонента, `app.component.ts` и добавьте app-input-button-unit тег внутрь шаблона\(помните, что мы рефакторим корневой компонент во встроенном шаблоне\):

{% code-tabs %}
{% code-tabs-item title="src/app/app.component.ts" %}
```markup
template: `
  <h1>
    Welcome to {{ title }}!
  </h1>

  <app-input-button-unit></app-input-button-unit>
`,
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Проверьте, что нового в браузере!

Давайте добавим контент в наш новый компонент. Для начала, добавим `title` который будет использован как названия:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```typescript
export class InputButtonUnitComponent implements OnInit {
  title = 'Hello World';
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Он не будет вмешиваться в `title` компонента `app-root`, поскольку содержимое каждого компонента инкапсулировано внутри него.

Затем добавьте интерполяцию элемента title в шаблон:

{% code-tabs %}
{% code-tabs-item title="src/app/input-button-unit/input-button-unit.component.ts" %}
```markup
template: `
  <p>
    input-button-unit works!
    The title is: {{ title }}
  </p>
`,
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Проверьте результат!

Этот компонент не достаточно пока что функционален. В следующих главах, мы узнаем о классе компонента, а затем реализуем его логику.

