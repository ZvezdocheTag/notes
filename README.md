## Работа с DevTools google chrome

1. Вывод и определение выбранного елемента dom в console

  $0 - эта надпись в console выведет текущий выбраный элемент DOM дерева.

  Пример:

  $0.getBoundingClientRect() - получить координаты выбраного элемента на странице

  console.dir($0) - эта запись выведит элемент в виде объекта.

2. Поиск элементов по DOM с помощью CSS псевдоклассов

  В отладчике, при поиске элемента по DOM дереву, можно использовать CSS псевдоклассы которые помогут подсветить нужный элемент

  Пример:

  .section-inner p:nth-of-type(2) - найдет 2-й селектор `<p>`  у родителя `<div class="section-inner">`

3. Включить режим редактирования контента непосредственно на странице

  Вводим в console

  document.designMode = 'on';
  
4. Скриншоты загрузки страницы
 Переходим во вкладку Network, включаем иконку видеокамеры (capture screenshot), обновляем страницу и видим стадии загрузки 
 
 [Документация со слайдами](https://developers.google.com/web/tools/chrome-devtools/network-performance/resource-loading#filmstrip)
 
 
 
## Оптимизация работы сайта
<hr>

Цитата "И бесплатный совет, который относится уже не к клиентам, а больше к фронту вашего прокси или nginx сервера – статику надо отдавать через nginx. "

[Статья](https://habrahabr.ru/company/oleg-bunin/blog/311464/)

## Сборщики

#### Webpack

Ошибка: 

```
ERROR in ./~/request/lib/har.js
Module not found: Error: Cannot resolve module 'fs' in /absolute/path/to/project/node_modules/request/lib
 @ ./~/request/lib/har.js 3:9-22

ERROR in ./~/request/~/tunnel-agent/index.js
Module not found: Error: Cannot resolve module 'net' in /absolute/path/to/project/node_modules/request/node_modules/tunnel-agent
 @ ./~/request/~/tunnel-agent/index.js 3:10-24

ERROR in ./~/request/~/tunnel-agent/index.js
Module not found: Error: Cannot resolve module 'tls' in /absolute/path/to/project/node_modules/request/node_modules/tunnel-agent
 @ ./~/request/~/tunnel-agent/index.js 4:10-24
 ```
 
 Решение:
 
 **webpack.config.js**
 
 ```
 var path = require('path');

module.exports = {
  entry: 'index',
  output: {
    path: path.join(__dirname, 'scripts'),
    filename: 'bundle.js'
  },
  module: {
    loaders: [
      { test: /\.json$/, loader: 'json-loader' }
    ]
  },
  resolve: {
    extensions: ['', '.webpack.js', '.web.js', '.js']
  },
  node: {
    console: 'empty',
    fs: 'empty',
    net: 'empty',
    tls: 'empty'
  }
};
```

[Источник](https://github.com/request/request/issues/1529)

## JavaScript
 
Если в коде используется арихтектура построеная через prototype, при переборе свойств объекта в цикле for - in желательно добовлять проверку array.hasOwnPropery(i), чтобы исключить появления в результатах свойства добавленного через prototype

Пример:

```js

var man = {
  hand: 2,
  leg: 2,
  head: 1
}

for(var key in man) {
  if(man.hasOwnProperty(key)) {
    console.log(key, ":", man[key]);
  }
}

```

Источник: JavaScript Patterns, by
Stoyan Stefanov (O’Reilly).
Стр. 40

#### Добавление "динамических" узлов (элеметнов) в DOM

Стандартный способ:
```js
Element.innerHTML = <div>some content</div>
```

Более оптимальный способ вставки:
```js
Element.insertAdjacentHTML('afterbegin', '<div>some content</div>'  )
```

[Документация](https://developer.mozilla.org/ru/docs/Web/API/Element/insertAdjacentHTML)

## Email верстка








