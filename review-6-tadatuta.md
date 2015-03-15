# Компонент выезжающего двухуровневого меню в виде БЭМ-библиотеки из нескольких составных блоков

[Репозиторий проекта](https://github.com/Guria/bem-drawer-menu) | [Демо](http://guria.github.io/bem-drawer-menu/kg-menu-demo.html)

Автор проект: [Guria](https://github.com/Guria)

Ревьювер: [Владимир Гриненко](https://ru.bem.info/authors/grinenko-vladimir/ )

## js
1. Лучше использовать универсальное событие `pointerclick`, которое в себя включает еще и полифил для тач устройств

  ```
  this.liveBindTo('menu-button', 'pointerclick', function(e) {
  ```

2. Для `ymodules` порядок модулей в собранном `js` файле не имеет значения, поэтому при добавления модуля в deps.js его можно декларировать в `shouldDeps`:

  Было:

  ```
  ([
    {
      mustDeps : [
        { block : 'i-bem', elem : 'dom' },
        { block : 'keyboard', elem : 'codes' },
        { block : 'menu', mods : { mode : 'radio' } },
        { elems : ['variables'] },
        'dom',
        'jquery',
      ],
      shouldDeps : [
        { elems : ['panel', 'menu-button']},
        { block : 'kg-menu', mods : { fix : 'scroll' } },
      ]
    }
  ]);
  ```

  Стало:

  ```
  ([
    {
      mustDeps : [
        { block : 'i-bem', elem : 'dom' },
        { elems : ['variables'] }
      ],
      shouldDeps : [
        { block : 'keyboard', elem : 'codes' },
        { block : 'menu', mods : { mode : 'radio' } },
        'dom',
        'jquery',
        { elems : ['panel', 'menu-button']},
        { block : 'kg-menu', mods : { fix : 'scroll' } },
      ]
    }
  ]);
  ```

3. Старайся избегать копипаста, если видишь, что части твоего кода повторяются – выноси в отдельные функции, у тебя несколько раз повторяется:

  ```
  this.delMod(this.findElem('item', 'active', true), 'active');
  this.delMod(this.findElem('submenu', 'active', true), 'active');
  ```

  Я вынес это в метод `_removeActiveMod`:

  ```
      _removeActiveMod: function() {
        this
          .delMod(this.findElem('item', 'active', true), 'active')
          .delMod(this.findElem('submenu', 'active', true), 'active');
      },
  ```
  p.s. Для многих методов API i-bem.js можно использовать `chaining` http://ejohn.org/blog/ultra-chaining-with-jquery/


## BH
1. У тебя есть шаблон:

  ```
  module.exports = function(bh) {
    bh.match('kg-menu', function(ctx, json) {
      ctx.js(true);
      if(Array.isArray(json.items)) {
        ctx
          .mix({ elem : 'aperture' })
          .content({
            elem : 'panel',
            items : json.items || [],
            settingsItem : json.settingsItem || undefined,
            versionItem : json.versionItem || undefined
          });
      }
    });
  };
  ```

  Строка `items : json.items || []` лишняя, выше есть проверка на `isArray`, поэтому `ИЛИ` не отработает никогда.

## deps.js
1. Во многих случаях можно написать немного короче:

  Было:

  ```
  ([
    {
      shouldDeps : [
        { elems: 'group' },
        { elem: 'items', mods: { type: [ 'system', 'main' ] }},
      ]
    }
  ]);
  ```

  Стало:

  ```
  ({
    shouldDeps : [
      { elems: 'group' },
      { elem: 'items', mods: { type: [ 'system', 'main' ] }},
    ]
  });
  ```

**Итого:**

Получился отличный компонент уровня проекта!
Что можно улучшить, чтобы получить блок для универсальной библиотеки:
- Я бы предложил попробовать избавиться от необходимости руками разбивать блок на две отдельные части в `BEMJSON API` и руками же следить за их связью. В качестве примера см. реализацию [dropdown](https://ru.bem.info/libs/bem-components/v2.0.0/desktop/dropdown/ ) — есть `switcher`, который может быть либо предефайненым шорткатом, либо выражен произвольным `BEMJSON` и есть поле с содержимым. При инициализации блока, содержимое может уехать куда угодно в DOM-дереве, но пользователю не придется самому за этим следить.
- Сейчас блок заточен под конкретный дизайн/расположение. Для общего компонента стоило бы предоставить лишь абстрактную штуку, которая принимает `switcher` и умеет показывать какое-то количество уровней с произвольным содержимым. Содержимое задавать произвольным `BEMJSON`. А компонент с конкретным внешним видом и простым `BEMJSON API` уже делать на основе такого абстрактного — композиция рулит.

В целом чувствуется глубокое погружение в тему. Впрочем, это еще по ответам на форуме было понятно ;)

**Отправил тебе [PR](https://github.com/Guria/bem-drawer-menu/pull/2) с дополнениями, о которых написал в этом ревью.** 
