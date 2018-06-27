---
title: Вывод
sort: 3
contributors:
  - TheLarkInn
  - chyipin
  - rouzbeh84
  - byzyk
  - skv1991
---

Настройка свойства `output` в конфигурации сообщает webpack как записывать на диск скомпилированные файлы. Обратите внимание, что в то время, как входных точек в `entry` может быть много, в конфигурации указывается только одна точка вывода в `output`.


## Использование

Минимальными требованиями для свойства `output` в конфигурации webpack является присвоение объекта в качестве значения, включая следующие два свойства:

- `filename` для указания имени файла(ов), получаемого на выходе.
- `path` для абсолютного пути к нужной директории, куда хотите положить файл(ы).

**webpack.config.js**

```javascript
module.exports = {
  output: {
    filename: 'bundle.js',
    path: '/home/proj/public/assets'
  }
};
```

Эта конфигурация на выходе создаст один файл `bundle.js` в директории `/home/proj/public/assets`.


## Множество точек входа

Если ваша конфигурация создает больше одного "куска" кода (как с множественными точками входа, или когда используется плагин вроде CommonsChunkPlugin), то нужно использовать [подстановки](/configuration/output#output-filename) чтобы быть уверенным, что каждый файл имеет уникальное имя.

```javascript
module.exports = {
  entry: {
    app: './src/app.js',
    search: './src/search.js'
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/dist'
  }
};

// запишет на диск: ./dist/app.js, ./dist/search.js
```


## Продвинутый пример

Вот более сложный пример использования CDN и хэшей для ресурсов:

**config.js**

```javascript
module.exports = {
  //...
  output: {
    path: '/home/proj/cdn/assets/[hash]',
    publicPath: 'http://cdn.example.com/assets/[hash]/'
  }
};
```

В случаях, где `publicPath` может выводить файлы, о которых webpack не знает во время компиляции, оно может быть оставлено пустым и установлено динамически во время запуска через переменную `__webpack_public_path__` в файле входной точки в проект:

```javascript
__webpack_public_path__ = myRuntimePublicPath;

// остальная часть входа в приложение
```
