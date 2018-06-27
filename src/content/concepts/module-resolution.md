---
title: Распознавание модулей
sort: 8
contributors:
  - pksjce
  - pastelsky
  - byzyk
  - skv1991
---

Распознаватель (англ. resolver) это библиотека, помогающая найти расположение модуля по его абсолютному пути.
Модуль может быть подключен, как зависимость из другого модуля, например:

```js
import foo from 'path/to/module';
// или
require('path/to/module');
```

Модуль зависимости может быть из кода приложения или библиотек третьих лиц. Распознаватель помогает
webpack найти код модуля,  который нужно включить в бандл для каждой конструкции вроде `require`/`import`.
webpack использует [enhanced-resolve](https://github.com/webpack/enhanced-resolve) для распознавания путей файлов при сборке модулей.


## Resolving rules in webpack

Using `enhanced-resolve`, webpack can resolve three kinds of file paths:


### Absolute paths

```js
import '/home/me/file';

import 'C:\\Users\\me\\file';
```

Since we already have the absolute path to the file, no further resolution is required.


### Relative paths

```js
import '../src/file1';
import './file2';
```

In this case, the directory of the resource file where the `import` or `require` occurs is taken to be the context directory. The relative path specified in the `import/require` is joined to this context path to produce the absolute path to the module.


### Module paths

```js
import 'module';
import 'module/lib/file';
```

Modules are searched for inside all directories specified in [`resolve.modules`](/configuration/resolve/#resolve-modules).
You can replace the original module path by an alternate path by creating an alias for it using [`resolve.alias`](/configuration/resolve/#resolve-alias) configuration option.

Once the path is resolved based on the above rule, the resolver checks to see if the path points to a file or a directory. If the path points to a file:

* If the path has a file extension, then the file is bundled straightaway.
* Otherwise, the file extension is resolved using the [`resolve.extensions`](/configuration/resolve/#resolve-extensions) option, which tells the resolver which extensions (eg - `.js`, `.jsx`) are acceptable for resolution.

If the path points to a folder, then the following steps are taken to find the right file with the right extension:

* If the folder contains a `package.json` file, then fields specified in [`resolve.mainFields`](/configuration/resolve/#resolve-mainfields) configuration option are looked up in order, and the first such field in `package.json` determines the file path.
* If there is no `package.json` or if the main fields do not return a valid path, file names specified in the [`resolve.mainFiles`](/configuration/resolve/#resolve-mainfiles) configuration option are looked for in order, to see if a matching filename exists in the imported/required directory .
* The file extension is then resolved in a similar way using the `resolve.extensions` option.

webpack provides reasonable [defaults](/configuration/resolve) for these options depending on your build target.


## Resolving Loaders

This follows the same rules as those specified for file resolution. But the [`resolveLoader`](/configuration/resolve/#resolveloader) configuration option can be used to have separate resolution rules for loaders.


## Caching

Every filesystem access is cached, so that multiple parallel or serial requests to the same file occur faster. In [watch mode](/configuration/watch/#watch), only modified files are evicted from the cache. If watch mode is off, then the cache gets purged before every compilation.


See [Resolve API](/configuration/resolve) to learn more on the configuration options mentioned above.
