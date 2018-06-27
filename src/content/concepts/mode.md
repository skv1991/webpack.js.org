---
title: Режим
sort: 4
contributors:
  - EugeneHlushko
  - byzyk
  - skv1991
---

Предоставление опции `mode` в конфигурации сообщает webpack использовать соответствующие встроенные методы оптимизации.

`string`

T> Значением по-умолчанию для свойства `mode` является `production`.

## Использование

Просто предоставьте опцию `mode` в конфигурации:

```javascript
module.exports = {
  mode: 'production'
};
```


или передайте его, как аргумент в [CLI](/api/cli/):

```bash
webpack --mode=production
```

Поддерживаются следующие строковые значения:

Опция                 | Описание
--------------------- | -----------------------
`development`         | Предоставляет `process.env.NODE_ENV` со значением `development`. Включает `NamedChunksPlugin` и `NamedModulesPlugin`.
`production`          | Предоставляет `process.env.NODE_ENV` со значением `production`. Включает `FlagDependencyUsagePlugin`, `FlagIncludedChunksPlugin`, `ModuleConcatenationPlugin`, `NoEmitOnErrorsPlugin`, `OccurrenceOrderPlugin`, `SideEffectsFlagPlugin` и `UglifyJsPlugin`.
`none`                | Отклоняет любые опции оптимизации по-умолчанию

Если не указано, webpack установит `production` как значение по-умолчанию для `mode`. Поддерживаемыми значениями для mode являются:

T> Пожалуйста, запомните, что установка переменной окружения `NODE_ENV` автоматически не задаст значение для `mode`.


### Mode: development


```diff
// webpack.development.config.js
module.exports = {
+ mode: 'development'
- plugins: [
-   new webpack.NamedModulesPlugin(),
-   new webpack.DefinePlugin({ "process.env.NODE_ENV": JSON.stringify("development") }),
- ]
}
```


### Mode: production


```diff
// webpack.production.config.js
module.exports = {
+  mode: 'production',
-  plugins: [
-    new UglifyJsPlugin(/* ... */),
-    new webpack.DefinePlugin({ "process.env.NODE_ENV": JSON.stringify("production") }),
-    new webpack.optimize.ModuleConcatenationPlugin(),
-    new webpack.NoEmitOnErrorsPlugin()
-  ]
}
```


### Mode: none


```diff
// webpack.custom.config.js
module.exports = {
+  mode: 'none',
-  plugins: [
-  ]
}
```

Если вы хотите изменить поведение в соответствии с переменной **mode** в *webpack.config.js* вам нужно экспортировать функцию вместо объекта:

```javascript
var config = {
  entry: './app.js'
  //...
};

module.exports = (env, argv) => {

  if (argv.mode === 'development') {
    config.devtool = 'source-map';
  }

  if (argv.mode === 'production') {
    //...
  }

  return config;
};
```
