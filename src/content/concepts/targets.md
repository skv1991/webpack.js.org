---
title: Цели
sort: 10
contributors:
  - TheLarkInn
  - rouzbeh84
  - johnstew
  - srilman
  - byzyk
  - skv1991
---

Поскольку JavaScript может быть написан и для сервера и для браузера, webpack представляет множество _целей_ для развертки, которые можно настроить в вашей [конфигурации](/configuration) webpack.

W> Свойство `target` в конфигурации webpack не стоит путать со свойством `output.libraryTarget`. Чтобы узнать больше, смотри [наш гайд](/concepts/output) по свойству `output`.

## Использование

Для установки свойства `target`, вы просто устанавливаете его значение в конфигурации webpack:

**webpack.config.js**

```javascript
module.exports = {
  target: 'node'
};
```

В примере выше, используя `node` webpack будет осуществлять компиляцию под использование в Node.js-подобном окружении (использует Node.js метод `require` для загрузки кусков и не трогает никаких встроенных модулей вроде `fs` или `path`).

Каждая _цель_ имеет различные добавления для развертывания/окружения, поддерживаемых для для удовлетворения своих потребностей. Ознакомьтесь с [доступными целями](/configuration/target).

?>Дальнейшее расширение для других популярных значений в целях

## Multiple Targets

Хоть webpack и **не** поддерживает передачу множества строк в свойство `target`, вы можете создать изоморфную библиотеку путем объединения двух отдельных конфигураций:

**webpack.config.js**

```javascript
const path = require('path');
const serverConfig = {
  target: 'node',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'lib.node.js'
  }
  //…
};

const clientConfig = {
  target: 'web', // <=== может быть опущено, так как по-умолчанию значение 'web'
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'lib.js'
  }
  //…
};

module.exports = [ serverConfig, clientConfig ];
```

Пример выше создаст `lib.js` и `lib.node.js` файлы в вашей директории `dist`.

## Ресурсы

Как видно из опций выше, есть несколько различных _целей_ для разворачивания проекта, из которых можно выбрать. Ниже представлен список примеров, и ресурсов на которые можно ссылаться.

*  **[compare-webpack-target-bundles](https://github.com/TheLarkInn/compare-webpack-target-bundles)**: Отличный ресурс для тестирования и просмотра различных _целей_ в webpack. Так же отлично для сообщения об ошибках.
* **[Boilerplate of Electron-React Application](https://github.com/chentsulin/electron-react-boilerplate)**: Хороший пример сборки основного процесса и рендеринга в Electron.

?> Необходимо найти свежие примеры таких целей в webpack, которые используются в реальных проектах или шаблонах.
