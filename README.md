# 👏 React Code Review 

# Чеклист ревьюера

## Можно ли упростить инфраструктуру проекта? Как бы вы посоветовали ее реорганизовать?

### 📁 package.json

Let's take a look to your `package.json`:
```
{
    "name": "react-router", 
    "version": "1.0.0",
    "private": true,
    ...
}
```
![](https://img.shields.io/badge/Need-Improve-%23A3A6B4) name and version attribute [is optional](https://github.com/npm/npm/issues/18781) for private packages.
```
    "react": "^16.2.0",
    "react-dom": "^16.2.0",
    "react-redux": "^5.0.7",
    "react-router": "^4.2.0",
    "react-router-dom": "^4.2.2",   
    "redux": "^4.0.0",
    "redux-thunk": "^2.2.0",
```

These packages should be preset in dependencies - you will use it in your app.

```
    "@babel/core": "7.8.7",
    "@babel/preset-env": "7.8.7",
    "@babel/preset-react": "7.8.3",
    "babel-loader": "8.0.6",
    "babel-jest": "25.1.0",
    "webpack": "4.42.0",
    "webpack-cli": "3.3.11",
    "enzyme": "3.11.0",
    "enzyme-adapter-react-16": "1.15.2",
    "eslint": "6.8.0",
    "eslint-plugin-react": "^7.19.0",
    "jest": "25.1.0",
    "react-scripts": "1.1.1",
    "react-test-renderer": "16.13.0",
    "webpack-dev-server": "3.10.3"
```


![](https://img.shields.io/badge/Need-Fix-%23FE781B) These packages used for development purpose: build, transpose, test, etc. Your app will run without that packages. Better to move it to `devDependencies`.
(_only to show my skills, I will never give full solution to a student_)

```
"scripts": { 
    "build": "webpack --mode production",
}
```
![](https://img.shields.io/badge/Need-Improve-%23A3A6B4) Better to remove the build target directory before each build. Please note that `rm -rf` will work only in systems that have [rm](https://en.wikipedia.org/wiki/Rm_\(Unix\)) command in [CLI](https://en.wikipedia.org/wiki/Command-line_interface). For example in my own system, windows 10, I have [del](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/del) instead of rm. To unify this process please use [rimraf](https://www.npmjs.com/package/rimraf) package instead of rm command.

![](https://img.shields.io/badge/Need-Improve-%23A3A6B4) You can increase readability of `eslint --ext .jsx --ext .js src/` by replacing it with `eslint --ext .jsx,.js src/`. [Learn more about ESLint CLI](https://eslint.org/docs/user-guide/command-line-interface)

```
"devDependencies": {
    "husky": "^0.14.3",
  }
```

![](https://img.shields.io/badge/Need-Improve-%23A3A6B4) Husky package doesn't have correct config for your project. Please read [Husky reference](https://www.npmjs.com/package/husky) to learn more about husky.

### 📁 .gitignore

![](https://img.shields.io/badge/Need-Fix-%23FE781B) better to add build target directory to ignore because it's generated code and not supposed to be a part of code base.

### 📁 webpack.config.js

```
 devtool: `source-map`,
```

![](https://img.shields.io/badge/Need-Improve-%23A3A6B4) Better to avoid using backtick for not string template purposes. Make this code consistent with other - use regular single quote.

![](https://img.shields.io/badge/Need-Fix-%23FE781B) better to add build target directory to ignore because it's generated code and not supposed to be a part of code base.
### Overall 

- ![](https://img.shields.io/badge/Need-Improve-%23A3A6B4) it would be great if you add [Editor config](https://editorconfig.org/) to unify IDE config.
- ![](https://img.shields.io/badge/Need-Improve-%23A3A6B4) Only for my opinion, it's would be handful to group all store entities (like actions and reducers) under parent directory. It will increase code readability because, as I can see, it's a good practice for most of the developers in industry.
- ![](https://img.shields.io/badge/Need-Improve-%23A3A6B4) You can use `overlay` [key](https://webpack.js.org/configuration/dev-server/#devserveroverlay) in webpack dev server to show compile errors on page if it preset.

- ![](https://img.shields.io/badge/Need-Improve-%23A3A6B4) Better to output build result to not `public` directory to easily remove it before next build.

- It is possible to simplify the project infrastructure, but this will harm code readability, so I suggest following my recommendations instead of simplify infrastructure. 

## Корректно ли написаны тесты для компонент? Корректно ли валидируются пропсы?

- Корректно ли выполняется рендер компонент?
- Правильно ли настроены все роуты, в том числе и защищенные?
- Оптимально ли написаны компоненты? Правильно ли выделены функциональные компоненты?
- Корректно ли описаны `Actions`, `Reducers`?


## Что проверяем
```
# Реализовано простое приложение, без подключения к БД.

## Задание:
Роуты:
+ `/` - главная
+ `/login` - страница ввода логина и пароля
+ `/profile` - страница с произвольным текстом, недоступная без авторизации
+ `/kvazavr` - 404

В шапке есть ссылки:

+ На главную (`/`)
+ Профиль (`/profile`)
+ Логин (`/login`)
+ 404 (`/kvazavr`)

Если url битый - редирект на `/kvazavr`

Если пользователь не авторизован -- перекидывает на страницу `/login`.
Моковые данные для входа: user: student, password: student

Если данные инвалидны, то:
Имя пользователя или пароль некорректны

Если данные валидны: редирект на `/profile`
```
