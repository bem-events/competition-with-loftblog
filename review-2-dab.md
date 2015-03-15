# Верстка «лендинг» шаблона Halcyon Days

[Репозиторий проекта](https://github.com/oozywaters/Halcyon)

[Демо](http://104.236.43.137/halcyon/index.html)

Ревьювер: [Дмитрий Белицкий](https://ru.bem.info/authors/belitsky-dmitry/ )

Автор хорошо разобрался с синтаксисом BEMHTML, BEMJSON. Научился использовать i-bem.js. Задача сверстать страницу в BEMJSON и использовать, по-возможности, сторонние библиотеки  — решена успешно.
Для оптимизации можно посоветовать использовать bower для установки библиотек и инлайнинг js-файлов с помощью `borschik`'а. С дальнейшей минимизацией собранных файлов перед продакшеном.
 
Как это сделать? Воспользуемся возможностью вставлять файлы — [Нотация для js-include ](https://ru.bem.info/tools/optimizers/borschik/js-include/). Добавим `борщик` для обработки js-файлов в `development` окружении. Для этого отредактируем `.enb/make.js`, раскомментируем [строку с борщиком](https://github.com/oozywaters/Halcyon/blob/master/.enb/make.js#L151) и [закомментируем предыдущую](https://github.com/oozywaters/Halcyon/blob/master/.enb/make.js#L150). 

После этого поменяем вставленный код модуля на комментарий для борщика:

https://github.com/oozywaters/Halcyon/blob/master/common.blocks/flexslider/flexslider.js#L6
```js
modules.define('flexslider', ['jquery'], function(provide, $) {
  var jQuery = $;
  /*borschik:include:../../libs/flexslider/jquery.flexslider.js*/
  provide($);
});
```

Здесь мы можем использовать неминифицированные файлы, поскольку сборщик будет их минифицировать в последствии.

Также можем поступить с блоками [modernizr](https://github.com/oozywaters/Halcyon/tree/master/common.blocks/modernizr):

```js
/*borschik:include:../../libs/modernizr/modernizr.js*/
```

и [waypoints](https://github.com/oozywaters/Halcyon/tree/master/common.blocks/waypoints):

```js
modules.define('waypoints', ['jquery'], function(provide, $) {

  window.jQuery = window.$ = $;
  
  /*borschik:include:../../libs/waypoints/lib/jquery.waypoints.js*/

  provide($);

});
```

Для запуска `enb` в `production` режиме можно использовать переменную окружения `YENV`:

```sh
> YENV=production enb make
```

Стоит выносить все, касающееся внешнего вида блоков и их поведения из `bemjson` файлов в `bemhtml` шаблоны. Например свойство `js: true` [блока page](https://github.com/oozywaters/Halcyon/blob/master/desktop.bundles/index/index.bemjson.js#L3) из `index.bemjson.js` в `./desktop.blocks/page/page.bemhtml`:

```js
block('page')(
  js()(true),
  elem('footer')(tag()('footer')),
  elem('footer-sep')(tag()('hr')),
  elem('footer-rights')(tag()('p')),
  elem('footer-rights-link')(tag()('a'))
);
```

Как я уже говорил, этот проект решает задачу того, как можно скрестить существующий набор библиотек и js-плагинов c БЭМ-стеком. Использование BEMJSON для описания страницы, сборка, модульность — использованы многие удобные инструменты современного верстальщика.

Для дальнейшего развития отличной задачей будет воспроизвести ту же страницу, но уже на основе своих решений или решений, созданных БЭМ-сообществом, вместо `bootstrap` и jquery-плагинов.

Например [bem-grid](http://verybigman.github.io/bem-grid/promo.pages/index/index.html) для для создания модульной сетки.

[bem-components](https://ru.bem.info/libs/bem-components/) — как основа для любых контролов на странице.

Следует стремиться к максимально простому BEMJSON, описывающему верхнеуровнемыми блоками-компонентами, интерфейс. В нем не должно быть информации, относящейся к внешнему виду блоков, их поведению. Для этого есть BEMHTML-шаблоны, JavaScript, CSS. Эти блоки, при условии независимости, можно будет эффективно реиспользовать в дальнейших проектах. Что сократит время на разработку, освободив его для дальнейших поисков наилучшей методологии. 
