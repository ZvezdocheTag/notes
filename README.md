## Работа с DevTools google chrome

1. *Вывод и определение выбранного елемента dom в console*

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

  `document.designMode = 'on'`; или `document.body.contentEditable=true;`
  
4. Скриншоты загрузки страницы
 Переходим во вкладку Network, включаем иконку видеокамеры (capture screenshot), обновляем страницу и видим стадии загрузки 
 
 [Документация со слайдами](https://developers.google.com/web/tools/chrome-devtools/network-performance/resource-loading#filmstrip)
 
5. Конструкция < $ > как в jquery, выполняет такую же функцию в браузере. $(‘tagName’), $(‘.class’), $(‘#id’) и $(‘.class #id’) возвращают первый элемент DOM, совпадающий с селектором.

6. Есть здесь и ещё одна конструкция: $$. Её использование выглядит как $$(‘tagName’) или $$(‘.class’). Она позволяет выбрать все элементы DOM, соответствующие селектору и поместить их в массив.

7. Поиск обработчиков событий, привязанных к элементу. В результате её выполнения будет выдан массив объектов, содержащий список событий, на которые может реагировать элемент.

    ```
      getEventListeners($(‘selector’))
    ```
    Обработчик для конкретного события:
    ```
    getEventListeners($(‘selector’)).eventName[0].listener
    ```
    
[Origin](https://medium.freecodecamp.com/10-tips-to-maximize-your-javascript-debugging-experience-b69a75859329#.pp0h16vp5)
## Оптимизация работы сайта
<hr>

Цитата "И бесплатный совет, который относится уже не к клиентам, а больше к фронту вашего прокси или nginx сервера – статику надо отдавать через nginx. "

[Статья](https://habrahabr.ru/company/oleg-bunin/blog/311464/)
## Оформление сайта (мета информация и т.д.)
Open Graph
```
<!-- Open Graph -->
<meta property="og:type" content="business.business">
<meta property="og:title" content="Etch Software">
<meta property="og:url" content="http://etchapps.com">
<meta property="og:image" content="http://etchapps.com/logo.png">
<meta property="business:contact_data:street_address" content="18A Ivy Lane">
<meta property="business:contact_data:locality" content="Canterbury">
<meta property="business:contact_data:region" content="Kent">
<meta property="business:contact_data:postal_code" content="CT1 1TU">
<meta property="business:contact_data:country_name" content="United Kingdom">
```
Twitter Card
```
<!-- Twitter Card -->
<meta name="twitter:card" content="summary">
<meta name="twitter:site" content="@etch">
<meta name="twitter:title" content="Etch Apps">
<meta name="twitter:description" content="A small team of designers and developers, who help brands with big ideas.">
```
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

Определения `this` происходит в момент вызова функции
```js
class Horn {
  constructor(words) {
    this.words = words;
  }
  makeSound() {
    return this.words;
  }
}

var mc2 = new Horn("don");
console.log(mc2.makeSound()) // - "don"

// В данной конструкции при вызове функции
// this будет установлен на глобальны window
var mc2Update = mc2.makeSound;
console.log(mc2Update()) // - "undefined"

```

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

### Подготовка собеседование
### Задачи
1. Объясните, для чего предназначена и каким образом работает следующая функция:
  ```js
  function bind(method, context) {
      var args = Array.prototype.slice.call(arguments, 2);
      return function() {
          var a = args.concat(Array.prototype.slice.call(arguments, 0));
          return method.apply(context, a);
      }
  }
  ```
  
2. Напишите код функции reversePrint(), которая выведет значения переданного ей односвязного списка в обратном порядке (4, 3, 2, 1). Для вывода значений используйте функцию console.log().
  ```js
  function reversePrint(linkedList) {
    // ...
  }

  var someList = {
      value: 1,
      next: {
          value: 2,
          next: {
              value: 3,
              next: {
                  value: 4,
                  next: null
              }
          }
      }
  };

  reversePrint(someList);
  ```

### Вопросы
1.Что такое bubbling и capturing при обработке событий?
[Ответ]()
2.Что такое замыкание в JavaScript?
3.Чем отличаются операторы сравнения "==" и "==="?

Ответ:
```js
123 == "123"; // true
1 == true; // true

//Сравнение с приведением типов
123 === "123"; // false
1 === true;    // false
```

### Источники
[Yandex вакансии](https://yandex.ua/jobs/vacancies/dev)


## Email верстка

## React

Examples:
Fetch data from API with redux
https://stackoverflow.com/questions/39813984/how-to-fetch-data-through-api-in-redux

React-Redux connect explained:
http://www.sohamkamani.com/blog/2017/03/31/react-redux-connect-explained/

Dummy guide redux and thunk: 
https://medium.com/@stowball/a-dummys-guide-to-redux-and-thunk-in-react-d8904a7005d3

Connect create-react-app with API:
https://www.fullstackreact.com/articles/using-create-react-app-with-a-server/


# Заметка по  баг-фиксам верстки

### Баг #1: При hover и изменение opacity прыгает элемент 

 Событие: `hover`; <br>
 Свойство: `opacity`;

Решение:

    -webkit-backface-visibility: hidden;

### Баг #2: Печатная версия сайта, не отображается цвет или картинка

Описание: При верстке печатной версии страницы встречаются проблемы с отображение картинок заданных через background,
возможны несколько вариантов решения:

 Задаем блоку content: 

```css
    background: url('/images/logo.png');
    
    @media print {
      content: url('/images/logo.png');
    }
```
 Актуально для -webkit-:

```css
     background: url('/images/logo.png');
     
     @media print {
       -webkit-print-color-adjust: exact;
     }
```

### Баг #3: Печатная версия сайта, при печати странице отображается лишняя пустая страница

Медиавыражение: `@media print`

Решение 1:

```css
    html, body {
        height: auto;
    }
 ```
Решение 2:

 ```css
    * {
        page-break-after: avoid;
        page-break-before: avoid;
    }
 ```
    
### Баг #4: Телефоны в safari и edge отображаются с подчеркиванием

Есть несколько вариантов убрать автоформатирование телефонных номеров

Вариант 1, убираем глобально на странице:

```html
    <meta name="format-detection" content="telephone=no">
 ```
 
 Вариант 2, через CSS:
 
 ```css
    a[x-apple-data-detectors] {
     color: inherit !important;
     text-decoration: none !important;
     font-size: inherit !important;
     font-family: inherit !important;
     font-weight: inherit !important;
     line-height: inherit !important;
   }
   
   // Убрать у конкретного элемента
   a[x-apple-data-detectors].class-name
```
##### Ссылки

1. [Ответ с stackoverflow](http://stackoverflow.com/questions/3736807/how-do-i-remove-the-blue-styling-of-telephone-numbers-on-iphone-ios)

### Баг #5: При клике на элемент который содержит svg > use, событие не проходит (Edge)

Дата: 13.10.2016; <br>
Событие: `click`; <br>
Браузер: `Edge`; 

Пример:

```html

<div class="prvd-star" title="Добавить в закладки">
 <svg class="svg-star-icon">
     <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="/images/svg/icons.svg#svg-icon-star"></use>
 </svg>
</div>

```
```js

$(document).on('click',function(e) {
 console.log(e.target); // При клике на prvd-star, в консоле мы не увидим наш элемент
});

```

Решение:
Добавляем нашему элементу с классом `prvd-star` псевдоэлемент :after или :before, для перекрытия внутри лежащего svg,
теперь событие будет воспроизводится.

```css

.prvd-star {
    position: relative;
}
.prvd-star:after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}

```

[Источник: Ответ с stackoverflow](http://stackoverflow.com/questions/35041903/ms-edge-cant-detect-delegated-events-for-use-svg-elements)








    





