# Набор карточек в стиле гуглового «material» дизайн

[Репозиторий проекта](https://github.com/phamap/bem-card-world)

[Демо](http://phamap.github.io/bem-card-world/)

Ревьювер: [Гела Константинова](https://ru.bem.info/authors/konstantinova-gela/ )

**Общее**

Проект при запуске enb-сервера вылетает с ошибкой. Зачем-то был добавлен уровень блоков `hackaton`, которого в проекте нет - https://github.com/phamap/bem-card-world/commit/21eb4ce1b4ef43b04d893a4513d8bf7b39b0ac86#diff-94d5989e964d9e6be8bea46eb8da6cc0R36

В `package.json` остались старые данные по project-stub (https://github.com/phamap/bem-card-world/blob/master/package.json#L3)

_______________________________________________________________

**Папка img**

Картинки названы не по bem'у (достаточно было бы `img__man` и т.д. сделать).
Картинки не оптимизированы.

_______________________________________________________________

**Стили**

Хорошо бы стили разнести по группам (шрифты, позиционирование, отображение и пр.)
```css
.block
{
    font-size: 13px;
    font-weight: 700;

    position: absolute;
    top: 0;

    width: 10px;
    heigth: 10px;
}
```

`common.blocks/card/card.styl`: стили `min-width 320px` и `max-width 320px` можно сократить до `width: 320px`;
https://github.com/phamap/bem-card-world/blob/master/common.blocks/card/card.styl#L4
https://github.com/phamap/bem-card-world/blob/master/common.blocks/card/card.styl#L10

`common.blocks/card-header/card-header.styl`: лишнее свойство `position:relative;`
https://github.com/phamap/bem-card-world/blob/master/common.blocks/card-header/card-header.styl#L4

https://github.com/phamap/bem-card-world/blob/master/common.blocks/card-header/__logo/card-header__logo.styl#L4 - матч на тег, не смотря на то что там есть класс image. Должно быть так:
```css
.card-header__logo .image {...}
```

А вообще вложенность `card-header__logo` можно сократить до одного элемента с помощью миксов.
```js
{
    block: 'card-header',
    content: [
        {
            block : 'image',
            mix: [{ block: 'card-header', elem:'logo' }],
            url : '../../img/logo.jpg',
            title : 'Все подробности на bem.info'
        },
        ...
    ]
}
```
и, соответственно, немного переписать стили `card-header__logo.styl`.

Значение насыщенности шрифта `400` не нужно писать, оно используется по умолчанию https://github.com/phamap/bem-card-world/blob/master/common.blocks/card-header/__subtitle/card-header__subtitle.styl#L4. (http://htmlbook.ru/css/font-weight)

Не нужно устанавливать для блочных элементов `width:100%`, они по умолчанию растягиваются по всей ширине
https://github.com/phamap/bem-card-world/blob/master/common.blocks/card-image/card-image.styl#L4

Предполагаю удивление в этом месте https://github.com/phamap/bem-card-world/blob/master/common.blocks/card-image/card-image.styl#L3 и, как следствие, костыль в виде `margin-bottom:-4px;`
Костыль можно оторвать и вместо него написать для блока `image` в стилях `card-image` свойство `vertical-align: top;`
Это уберет непонятный отступ внизу блока.

Не поняла смысла `z-index` https://github.com/phamap/bem-card-world/blob/master/common.blocks/dots/dots.styl#L12 и https://github.com/phamap/bem-card-world/blob/master/common.blocks/card-popup/card-popup.styl#L8
И без них все хорошо :)

Отдельный + за `.dots::after` и `.card-popup__close`.

https://github.com/phamap/bem-card-world/blob/master/common.blocks/dots/dots.styl#L21 - Цвет `#555` повторяется несколько раз, его можно вынести в переменную. Кстати, оранжевому почему-то повезло больше https://github.com/phamap/bem-card-world/blob/master/common.blocks/dots/dots.styl#L1

https://github.com/phamap/bem-card-world/blob/master/common.blocks/card-aside/card-aside.styl - логичней бы было вынести эти стили в модификатор `card-aside_direct_left` и сделать наравне с `card-aside_direct_right`.

https://github.com/phamap/bem-card-world/blob/master/common.blocks/clearfix/clearfix.styl - лучше придерживаться одного написания всех стилей в stylus. 

https://github.com/phamap/bem-card-world/blob/master/common.blocks/header/__nameplate/header__nameplate.styl - неиспользуемый элемент.

Общее по написанию цветов: лучше сохранять единый стиль - или все цвета пишем заглавными буквами `#EFEFEF`, или все маленькими `#efefef`.

`0px` можно не писать, достаточно просто `0` - https://github.com/phamap/bem-card-world/blob/master/common.blocks/dots/dots.styl#L23
_______________________________________________________________

**bemjson**

Разные кодстайлы для объектов и массивов. Например тут https://github.com/phamap/bem-card-world/blob/master/desktop.bundles/index/index.bemjson.js#L5 все с пробелами, ниже - без - https://github.com/phamap/bem-card-world/blob/master/desktop.bundles/index/index.bemjson.js#L15. Подразумеваю, что заготовка с пробелами была из project-stub, но лучше все содержать в одном стиле.

Для `page` добавляется кастомная тема `mat-cards` https://github.com/phamap/bem-card-world/blob/master/desktop.bundles/index/index.bemjson.js#L11, которая сбрасывает `margin` и `padding`(которого по умолчанию и так нет), в ноль (https://github.com/phamap/bem-card-world/blob/master/common.blocks/page/_theme/page_theme_mat-cards.styl). Можно было заиспользовать тему `islands`, которая и так приезжает с библиотекой bem-components.

Я за то, чтобы не использовать при разработке какие-то внешние урлы, как например https://github.com/phamap/bem-card-world/blob/master/desktop.bundles/index/index.bemjson.js#L74
Очень неудобно смотреть проект в браузере, нажимать на ссылку и уходить на другой ресурс, а потом возвращаться обратно на проект.
Лучше, при разработке, для урлов использовать `#`.

Лишняя вложенность https://github.com/phamap/bem-card-world/blob/master/desktop.bundles/index/index.bemjson.js#L62
Сейчас dom-дерево выглядит так:
```html
<div class="card-content">
    <p>A dolore repellendus in magni tempora, nulla voluptatum deleniti nobis est doloremque, eaque labore vitae error ipsam.</p>
</div>
```
Мы можем легко избежать лишний уровень вложенности (лишний тег `<p>`) путем такого bemjson:
```js
{
    block: 'card-content',
    content: 'A dolore repellendus...'
}
```
Таким образом, на выходе получим такой html:
```html
<div class="card-content">A dolore repellendus in magni tempora, nulla voluptatum  deleniti nobis est doloremque, eaque labore vitae error ipsam.</div>
```
То есть, везде, где есть тег `<p>` можно избежать лишнего уровня вложенности. Если нужны дефолтные отступы тега `<p>`, то их можно написать на соответствующие блоки с контентом (например, в `card-content`).

https://github.com/phamap/bem-card-world/blob/master/desktop.bundles/index/index.bemjson.js#L540 и https://github.com/phamap/bem-card-world/blob/master/desktop.bundles/index/index.bemjson.js#L544 - не очень понятно, почему тут заголовки просто тегами, а ниже уже по бэму (есть элемент, которому задается тег) - https://github.com/phamap/bem-card-world/blob/master/desktop.bundles/index/index.bemjson.js#L519
Это плохо, потому что стили для тега прописаны тут https://github.com/phamap/bem-card-world/blob/master/common.blocks/card-popup/card-popup.styl#L10 и как только, вдруг, захочется поменять h4 на h5, а тег `<p>`, например, на `<span>`, придется менять это все и во входных данных и в стилях. Лучше всего выражать все в элементах блока (или блоках), на которые можно писать шаблоны с описанием тегов, а стили будут матчится на селекторы.

Пустое поле `content: ''` писать не нужно https://github.com/phamap/bem-card-world/blob/master/desktop.bundles/index/index.bemjson.js#L531

https://github.com/phamap/bem-card-world/blob/master/desktop.bundles/index/index.bemjson.js#L212 - лишний пробел в самом начале.

На ссылках `NO/YES` можно было бы упразднить модификатор `style: 'material'`, и добавить `theme:'islands'`. Эта тема несет с собой базовые фичи для ссылок, такие как удаление нижнего подчеркивания, `cursor:pointer` и пр. А `font-weight` перевесить на `.card-footer .link`
https://github.com/phamap/bem-card-world/blob/master/desktop.bundles/index/index.bemjson.js#L72

Рассинхрон ссылки и модификатора по смыслу - https://github.com/phamap/bem-card-world/blob/master/desktop.bundles/index/index.bemjson.js#L234 Ссылка `LEAVE`, а модификатор `NO`
По идее вместо модификаторов `.link_type_no` и `.link_type_yes` можно было сделать просто `.link_color_black` и `.link_color_orange`. А `margin-right: 8px;` можно вообще перевесить на все ссылки блока `card-footer` (для `:last-child` сделать `margin-right:0`). Почему не сделать этот `margin` первому блоку? Потому что ссылок можнт быть и три и пять, и отступы нужны всем, кроме последнего.

_______________________________________________________________

**js**

js, описывающий функциональность взаимодействия блока `dots` и `card-popup` можно было вынести в какой-то модификатор блока `card` (родитель обоих блоков). Например, `card_popup-inside_yes` и там уже написать js для двух внутренних блоков (`this.findBlockInside('card-popup')...`).

https://github.com/phamap/bem-card-world/blob/master/common.blocks/dots/dots.js#L7, https://github.com/phamap/bem-card-world/blob/master/common.blocks/card-popup/card-popup.js#L21 - неиспользуемый `event`.

https://github.com/phamap/bem-card-world/blob/master/common.blocks/dots/dots.js#L8 - мама дорогая! :) В БЭМ родитель может знать про своих потомков, в тоже время потомки ничего про своих родителей знать не должны. Поэтому эту упячку как раз и решил бы перенос всего кода в `card_popup-inside_yes`.

Про попап вопрос - почему не использовался `popup` из `bem-components`?

Не смотря на то, что по коду у попапа должна быть отложенная инициализация, он инитится при загрузке страницы (`class="card-popup i-bem card-popup_js_inited"`).

А вот в `dots.js` как раз бы и пригодилась отложенная инициализация, чтобы бы блок инитился только тогда, когда кликнули на точки.

_______________________________________________________________

**Шаблонизация**

К сожалению, в проекте совсем не использовались шаблоны, хотя есть вещи, которые хорошо бы убрать под капот.
Например:
https://github.com/phamap/bem-card-world/blob/master/desktop.bundles/index/index.bemjson.js#L35
https://github.com/phamap/bem-card-world/blob/master/desktop.bundles/index/index.bemjson.js#L40

**Вывод**

Автор уверенно владеет `css`, освоил основы написания структуры страницы(БЭМ-дерево) в формате BEMJSON и подключение сторонних скриптов и стилей. В качестве основы для проекта взял рекомендуемую заготовку для БЭМ проектов – `project-stub` и познакомился с библиотекой блоков bem-components. Использовал оболочку из `ymodules` и `i-bem.js` для написания клиентского `js`. Автор не использовал в проекте БЭМ-шаблонизаторы. Подводя итог, можно сказать, что автор смог заиспользовать базовый костяк БЭМ-стека и сверстал на нем красивые блоки-карточки в стиле «material» дизайн. Надеюсь, что автор продолжил знакомство с БЭМ, ведь все намеки на успешное продолжение использования БЭМ у него есть. Мне хочется пожелать автору удачи, упорства и Stay BEMed!
