---
title: Плагины
sort: 5
contributors:
  - TheLarkInn
  - jhnns
  - rouzbeh84
  - johnstew
  - byzyk
  - skv1991
---

**Плагины** являются [фундаментом](https://github.com/webpack/tapable) webpack. webpack сам построен на **той же системе плагинов**, что вы используете в своей конфигурации webpack!

Они так же служат для тех ситуаций, когда нужно сделать **что-то ещё**, что [загрузчик](/concepts/loaders) не может.


## Анатомия

**Плагин** в webpack это JavaScript объект, который имеет метод [`apply`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply). Этот метод `apply` вызывается компилятором webpack, пердоставляя доступ к **всему** жизненному циклу процесса компиляции.

__ConsoleLogOnBuildWebpackPlugin.js__

```javascript
const pluginName = 'ConsoleLogOnBuildWebpackPlugin';

class ConsoleLogOnBuildWebpackPlugin {
  apply(compiler) {
    compiler.hooks.run.tap(pluginName, compilation => {
      console.log('Процесс сборки webpack начался!!!');
    });
  }
}
```

Первый параметр метода tap в хуках компилятора должен быть camel-case версией имени плагина. Для этого целесообразно использовать константу, чтобы ее можно было повторно использовать во всех хуках.

## Использование

Поскольку **плагины** могут принимать аргументы/опции, вы должны передать новый экземпляр (используя оператор `new`) в свойство `plugins` конфигурации webpack.

В зависимости от того, как вы используете webpack, есть множество способов использовать плагины.


### Конфимгурация

__webpack.config.js__

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin'); //установлено через npm
const webpack = require('webpack'); //для доступа к встроенным плагинам
const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    filename: 'my-first-webpack.bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        use: 'babel-loader'
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
};
```


### Node API

?> Даже когда используется Node API, пользователи должны передавать плагины через свойства `plugins` в конфигурации. Использование `compiler.apply` не является рекомендуемым способом.

__some-node-script.js__

```javascript
const webpack = require('webpack'); //для доступа к webpack во время выполнения
const configuration = require('./webpack.config.js');

let compiler = webpack(configuration);
compiler.apply(new webpack.ProgressPlugin());

compiler.run(function(err, stats) {
  // ...
});
```

T> Знали ли вы: Что пример выше очень сильно схож с [самим webpack при выполнении](https://github.com/webpack/webpack/blob/e7087ffeda7fa37dfe2ca70b5593c6e899629a2c/bin/webpack.js#L290-L292)! Есть множество отличных примеров использования, спрятанных внутри [исходного кода webpack](https://github.com/webpack/webpack), которые вы можете применять к своей конфигурации и скриптам!
