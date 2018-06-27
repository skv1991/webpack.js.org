---
title: Concepts
sort: 1
contributors:
  - TheLarkInn
  - jhnns
  - grgur
  - johnstew
  - jimrfenner
  - TheDutchCoder
  - adambraimbridge
  - EugeneHlushko
  - jeremenichelli
  - arjunsajeev
  - byzyk
---

По сути, **webpack** это _сборщик статических ресурсов модулей_ для современных JavaScript приложений. Когда webpack обрабатывает ваше приложение, он строит внутреннее _дерево зависимостей_, которое отмечает каждый модуль, нужный для проекта и генерирует один или более _бандл_.

T> Узнайте больше о модулях в JavaScript и модулях webpack [здесь](/concepts/modules).

Начиная с версии 4, **webpack не требует файла конфигурации** сборки проекта, тем не менее, он [необычайно гибкий](/configuration) для лучшего соответствия вашм нуждам.

Чтобы приступить к изучению, вам нужно всего лишь понять его **ключевые концепции**:

- Вход
- Вывод
- Загрузчики
- Плагины

Этот документ предназначен дать вам **обобщенный** обзор данных концепций, предоставляя при этом ссылки на детальные примеры использования концепций в конкретных ситуациях.


## Вход

**Точка входа** указывает webpack на модуль, который ему нужно использовать для начала построения внутреннего *дерева зависимостей*, webpack определит, от каких остальных библиотек и модулей этот входной модуль зависит (прямо и косвенно).

По-умолчанию, он называется `./src/index.js`, но вы можете указать другой (или множество точек входа) изменяя свойство **entry** в [конфигурации webpack](/configuration). Например:

__webpack.config.js__

``` js
module.exports = {
  entry: './path/to/my/entry/file.js'
};
```

T> Изучить подробнее можно в разделе [входные точки](/concepts/entry-points).


## Вывод

Свойство **output** сообщает webpack куда складывать создаваемые *бандлы*, и как их именовать, по-умолчанию это `./dist/main.js` для вывода основного файла и в директорию `./dist` будут попадать любые другие сгенерированные файлы.

Вы можете настроить эту часть процесса, указав поле `output` в вашей конфигурации:

__webpack.config.js__

```javascript
const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  }
};
```

В примере выше, мы используем свойства `output.filename` и `output.path`, чтобы сообщить webpack имя нашего бандла и куда мы хотим его положить. В случае, если вас удивил модуль path, импортируемый в верхней части, это [модуль ядра Node.js](https://nodejs.org/api/modules.html), который используется для управления путями файлов.

T> Свойство `output` имеет [намного больше настраиваемых возможностей](/configuration/output) и если вы хотите узнать больше о его концепциях, то можете [прочитать больше в разделе Вывод](/concepts/output).


## Загрузчики

Out of the box, webpack only understands JavaScript files. **Loaders** allow webpack to process other types of files and converting them into valid [modules](/concepts/modules) that can be consumed by your application and added to the dependency graph.

W> Note that the ability to `import` any type of module, e.g. `.css` files, is a feature specific to webpack and may not be supported by other bundlers or task runners. We feel this extension of the language is warranted as it allows developers to build a more accurate dependency graph.

At a high level, **loaders** have two properties in your webpack configuration:

1. The `test` property identifies which file or files should be transformed.
2. The `use` property indicates which loader should be used to do the transforming.

__webpack.config.js__

```javascript
const path = require('path');

module.exports = {
  output: {
    filename: 'my-first-webpack.bundle.js'
  },
  module: {
    rules: [
      { test: /\.txt$/, use: 'raw-loader' }
    ]
  }
};
```

The configuration above has defined a `rules` property for a single module with two required properties: `test` and `use`. This tells webpack's compiler the following:

> "Hey webpack compiler, when you come across a path that resolves to a '.txt' file inside of a `require()`/`import` statement, **use** the `raw-loader` to transform it before you add it to the bundle."

W> It is important to remember that when defining rules in your webpack config, you are defining them under `module.rules` and not `rules`. For your benefit, webpack will warn you if this is done incorrectly.

You can check further customization when including loaders in the [loaders section](/concepts/loaders).


## Plugins

While loaders are used to transform certain types of modules, plugins can be leveraged to perform a wider range of tasks like bundle optimization, assets management and injection of environment variables.

T> Check out the [plugin interface](/api/plugins) and how to use it to extend webpacks capabilities.

In order to use a plugin, you need to `require()` it and add it to the `plugins` array. Most plugins are customizable through options. Since you can use a plugin multiple times in a config for different purposes, you need to create an instance of it by calling it with the `new` operator.

**webpack.config.js**

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin'); //installed via npm
const webpack = require('webpack'); //to access built-in plugins

module.exports = {
  module: {
    rules: [
      { test: /\.txt$/, use: 'raw-loader' }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
};
```

In the example above, the `html-webpack-plugin` generates an html file for your application injecting automatically all your generated bundles.

T> There are many plugins that webpack provides out of the box! Check out the [list of plugins](/plugins).

Using plugins in your webpack config is straightforward - however, there are many use cases that are worth further exploration, [learn more about them here](/concepts/plugins).


## Mode

By setting the `mode` parameter to either `development`, `production` or `none`, you can enable webpack's built-in optimizations that correspond to each environment. The default value is `production`.

```javascript
module.exports = {
  mode: 'production'
};
```

Learn more about the [mode configuration here](/concepts/mode) and what optimizations take place on each value.


## Browser Compatibility

webpack supports all browsers that are [ES5-compliant](https://kangax.github.io/compat-table/es5/) (IE8 and below are not supported). webpack needs `Promise` for `import()` and `require.ensure()`. If you want to support older browsers, you will need to [load a polyfill](/guides/shimming/) before using these expressions.
