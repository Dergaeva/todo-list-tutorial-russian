# Разворачиваем на GitHub Pages

Это старая версия - мы должны проверить это :\)

Для того, чтобы развернуть наши изменения на GitHub pages мы будем использовать angular-cli-ghpages пакет [https://github.com/angular-buch/angular-cli-ghpages](https://github.com/angular-buch/angular-cli-ghpages)

* Вы должны быть зарегистрированы на GitHub
* Нужно создать репозиторий для вашего проекта.
* Необходимо зафиксировать все изменения, внесенные в проект.
* Необходимо установить angular-cli-ghpages

## Создание GitHub пользователя

Если Вы уже зарегистрированы на GitHub можете пропустить этот шаг. Создаем пользователя на  GitHub: [https://github.com/](https://github.com/) Заполните форму регистрации и убедитесь, что вы подтвердили свой адрес электронной почты.

## Создайте свой репозиторий приложений

После входа на GitHub. Нажмите кнопку  `Start a project` , и назовите репозиторий `ng-girls-todo` или любое другое имя, которое вам нравится.

## Подключение к репозиторию

Зафиксируйте все изменения, выполнив эту команду в каталоге проекта.

```text
git add -A && git commit -m "Your Message"
```

Выполните следующую команду для подключения вашего кода к репозиторию. Убедитесь, что заменили {YOUR\_USERNAME} и {YOUR\_REPO} на Ваше имя пользователя и название репозитория на github.
```text
git remote add origin https://github.com/{YOUR_USERNAME}/{YOUR_REPO}.git
git push -u origin master
```

## Разворачиваем GitHub Pages
Для начала установите angular-cli-ghpages.

```text
npm i -g angular-cli-ghpages
```

Затем просто запустите:

```text
ng build --prod --base-href="/[your-repo-name]/"
angular-cli-ghpages
```

Ваше приложение будет доступно по адресу \[[https://\[your-GH-username\].github.io/\[repo-name\]\(https://\[your-GH-username\].github.io/\[repo-name\)\](https://[your-GH-username].github.io/[repo-name]%28https://[your-GH-username].github.io/[repo-name%29\)\]

Для получения дополнительной информации смотрите [https://github.com/angular-buch/angular-cli-ghpages](https://github.com/angular-buch/angular-cli-ghpages).

## Популярные ошибки

На \ (windows \) вы, возможно, столкнулись с проблемой, например:

```text
An error occurred!
 Error: Unspecified error (run without silent option for detail)
    at C:\Users\<myuser>\AppData\Roaming\nvm\v8.9.1\node_modules\angular-cli-ghpages\node_modules\gh-pages\lib\index.js:232:19
    at _rejected (C:\Users\<myuser>\AppData\Roaming\nvm\v8.9.1\node_modules\angular-cli-ghpages\node_modules\q\q.js:844:24)
    ...
```

Попробуйте отладить его с помощью  `angular-cli-ghpages -S` . Если вы получите следующую ошибку:

```text
fatal: could not read Username for \'https://github.com\': No error\n',
```

вы можете сделать следующее

1. Создайте токен личного доступа здесь: [https://github.com/settings/tokens](https://github.com/settings/tokens)
2. Выполните следующую команду и замените токен, организацию \ (ваш пользователь \), репозиторий, имя пользователя и адрес электронной почты:

   ```text
   angular-cli-ghpages --repo=https://<personal-access-token>@github.com/organisation/your-repo.git --name="Displayed Username" --email=mail@example.org
   ```

