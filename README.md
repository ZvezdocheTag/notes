# notes

### Работа с DevTools google chrome

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



