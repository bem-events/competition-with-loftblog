# Аналог jQuery UI slider в виде БЭМ блока

[Репозиторий проекта](https://github.com/kuflash/bem-range) | [Демо](http://bem-range-demo.bitballoon.com)

Автор проект: [kuflash](https://github.com/kuflash)

Ревьювер: [Евгений Константинов](https://ru.bem.info/authors/konstantinov-eugeny/ )

1. Начнем с js. В самом начале ты [сохраняешь результат поиска элемента](https://github.com/sipayRT/bem-range/blob/master/common.blocks/range/range.js#L13):

  ```
  this.control = this.findElem('control');
  ```

  Тут есть пару советов:
  - для сохранения каких-либо данных в this лучше использовать подчеркивание, чтобы обозначить их приватность.
  - вместо `findElem` можно использовать кешируемый метод `elem`. Т.е. данную строку можно записать как:

    ```
    this._control = this.elem('control');
  ```

2. При инициализации блока у тебя [осуществляется подписка одного элемента на разные события](https://github.com/sipayRT/bem-range/blob/master/common.blocks/range/range.js#L15-L17). Такие выражения можно упрощать, используя расширенные возможности API i-bem'a.

  было:

  ```
  this.bindTo('control', 'change', this._onChange, this);
  this.bindTo('control', 'mousedown', this._onMouseDown, this);
  this.bindTo('control', 'mouseup', this._onMouseUp, this);
  ```

  стало:

  ```
  this.bindTo('control', {
    change : this._onChange,
    mousedown : this._onMouseDown,
    mouseup : this._onMouseUp
  }, this);
  ```

3. Теперь рассмотрим приватный метод [_updateFillTrack()](https://github.com/sipayRT/bem-range/blob/master/common.blocks/range/range.js#L41). Тут для определения максимального/минимального значения ты используешь конструкцию:

  ```
  var max = Number(this._control.attr('max'));
  ```

  Такого кода можно избежать, если заранее "прокинуть" эти значения в параметры блока. В твоем случае нужно просто в `BEMHTML` блока добавить:

  ```
  js()(function(){
    var ctx = this.ctx;

    return {
      min: this.ctx.min || 0,
      max: ctx.max || 100,
      value: ctx.val
    };
  })
  ```

  Это даст тебе возможность в дальнейшем в клиентском `js` использовать хранилище параметров. В твоем случае запись `Number(this._control.attr('max'))` можно будет заменить на `this.params.max`. Конечно же, `this.params` тоже можно предварительно сохранить в переменную для многократного использования.

  было:

  ```
  var max = Number(this._control.attr('max'));
  var min = Number(this._control.attr('min'));
  ```

  стало:

  ```
  var params = this.params,
      max = params.max,
      min = params.min;
  ```

4. Далее часть про [глобальное накапливание стилей](https://github.com/sipayRT/bem-range/blob/master/common.blocks/range/range.js#L48-L57) . Для анимации псевдоэлементов ты генеришь тег `style` и дописываешь туда стили для всех имеющихся блоков `range` на странице. А при изменении положения ползунка одного из этих блоков на каждый `change` ты переписываешь стили **всех** блоков, т.к. они находятся в одном теге. Поэтому лучше дописывать стили персонально для каждого блока, а не генерить глобально тэг `style`.

У тебя во многих методах встречается конструкция `if (this.hasMod(this.elem('value'), 'type', 'tooltip'))`. Это говорит о том, что модификатор `_type_tooltip` элемента `value` опциональный. Поэтому, тут лучше подойдет вариант с добавлением модификатора блоку, а не элементу. В таком случае у тебя будет возможность дописать js-реализацию этого модификатора в отдельном файле. Плюсы такого подхода:
     * если тебе не нужна дополнительная функциональность - ты просто не добавляешь модификатор и лишний код не приезжает;
     * избавление от копипаста (как раз твой случай с повторением одинаковых проверок в каждом методе);
     * код, разделенный на функиональные части, легче поддерживать и удобнее читать.

      Файловая структура при таком подходе будет иметь такой вид:

  ```
  range
  |
  |- _type
      range_type_tooltip.js
  range.js
  ```

  Таким образом, весь код, который относится к тултипу, переносится в отдельный файл. В таком виде реализация тултипа будет подключаться только тогда, когда она реально необходима. Код файла `range_type_tooltip` будет выглядеть примерно так:
  ```
  /**
  * @module range
  */
  modules.define('range', function(provide, Range) {
  /**
  * @exports
  * @class Range
  * @bem
  */
    provide(Range.decl({ modName : 'type', modVal : 'tooltip' }, /** @lends Range.prototype */{
      /**
      * @override
      */
      _onChange : function(event) {
        this.__base.apply(this, arguments); // выполняем код метода _onChange() из range.js
        this._updateTooltipPosition(event.originalEvent); // доопределяем тултип
      }
    }));
  });
  ```
  Обрати внимание, что метод `_updateTooltipPosition()` тебе теперь можно полностью перенести на уровень модификатора.

5. BEMHTML
  В шаблоне блока ты используешь матч [match(this.ctx.title)](https://github.com/sipayRT/bem-range/blob/master/common.blocks/range/range.bemhtml#L7). Это говорит о том, что `title` у тебя обязательный аргумент. И если добавить блок без тайтла, то блок не будет показан на страницу, т.к. ни на что не сматчится. Но `<input type='range' />` не предусматривает такого обязательного параметра. Поэтому нужно учесть этот момент при написании шаблонов. В данном случае ты можешь дописать текущий вариант как-то так:

  ```
  block('range')(
    // ...
    content()(function() {
      var ctx = this.ctx;

      return {
        elem: 'box',
        content: // ...
      };
    }),

    content()(
      match(this.ctx.title)(function() {
          return [
            {
              elem: 'title',
              tag: 'label',
              attrs: { for: this.generateId() },
              content: this.ctx.title
            },
            applyNext()
          ];
        }
      )
    )
  );
  ```

  Но т.к. (опять же) это опциональный параметр, то я бы посоветовал перенести его на уровень `title` в отдельный шаблон (как ты это сделал для элемента `box`).

**Итог:**
В целом, если не вглядываться в детали реализации, получился работоспособный продукт. В проекте использовался `BEMHTML` и `i-bem`, что говорит о понимании происходящих процессов и заинтересованности к БЭМ-технологиям. Так же для написания стилей использовался препроцессор `Stylus`. Для дальнейшего развития я бы посоветовал автору переделать этот блок, взяв за основу блоки [progressbar](https://ru.bem.info/libs/bem-components/v2.0.0/desktop/progressbar/) и [popup](https://ru.bem.info/libs/bem-components/v2.0.0/desktop/popup/). Используя API этих блоков и дописав немного своего кода, можно получить составной блок `range` с более простой реализацией.
