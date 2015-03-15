# Ревью проекта "верcтки макета Velo Dashboard"

[Репозиторий проекта](https://github.com/gcor/bem-testpage ) | [Демо](http://gcor.ru/bem-psd/ )

Автор проект: [gcor](https://github.com/gcor )

Ревьювер: [Александр Белянский](https://ru.bem.info/authors/belyanskii-alexandr/ )

## Взгляд сверху (html)
### Блок header
#### Элемент logo:
```html
<div class="header__punkt header__logo">
        <img class="image header__logo-img" role="img" src="img/logo.png" alt="Velo">
</div>
```
Зачем нам тут нужен элемент `logo`?
Насколько я понял по смыслу, единсвенное что он делает - заменяет собой `padding`.
Плюс ко всему, кажется что лучше подобные вещи делать ссылкой на главную страницу, ведь это логотип сайта.

#### Элемент header-search:
```html
<div class="header__punkt header__search">
        <div class="search-input header__search-input">
                <span class="input i-bem input_js_inited" data-bem="{&quot;input&quot;:{}}">
                        <span class="input__box">
                                <input class="input__control" placeholder="Search places">
                        </span>
                </span>
                <div class="search-input__icon"></div>
        </div>
</div>
```

Так, ну с поиском не понятно почему ты не сделал модификатор для блока инпут, ведь можно было бы сделать тему и не надо было бы переопределять дефолтные стили. Плюс - иконка, кажется что это должна быть кнопка, которая будет отправлять данные из инпута, а не `div`.

#### Элемент header-info + блок user:
```html
<div class="header__punkt header__info">
        <div class="user">
                <div class="user__img">
                        <img class="image user__user-img" role="img" src="img/user.png" alt="user">
                </div>
                <a class="link link__control user__name i-bem" data-bem="{&quot;link&quot;:{}}" href="#">Anthon gcor</a>
                <button class="button button__control user__logout i-bem button_js_inited" data-bem="{&quot;button&quot;:{}}" role="button" type="button"></button>
        </div>
</div>
```
Опять попадаем на `div` с вложенной в него картинкой который никуда не ведет, кажется аватарка пользователя должна вести на его профиль, как и ссылка ниже.

### Блок aside
#### Элемент logo:
```html
<div class="aside__logo header__logo">
        <img class="image header__logo-img" role="img" src="img/logo.png" alt="Velo">
</div>
```
В блоке `aside` мы опять встречаем `logo`, причем к его элементу примиксован элемент `logo` блока `header`, хотя так делать нельзя. Ну и тут конечно тоже необходима ссылка.

#### Элементы list:
```html
<div class="aside__list aside__list_position_top">
        <a class="link link__control aside__link i-bem" data-bem="{&quot;link&quot;:{}}" href="#">
                <img class="image aside__icon" role="img" src="img/i_pen.png">
        </a>
        <a class="link link__control aside__link i-bem" data-bem="{&quot;link&quot;:{}}" href="#">
                <img class="image aside__icon" role="img" src="img/i_like.png">
        </a>
        <a class="link link__control aside__link i-bem" data-bem="{&quot;link&quot;:{}}" href="#">
                <img class="image aside__icon" role="img" src="img/i_earth.png">
        </a>
        <a class="link link__control aside__link i-bem" data-bem="{&quot;link&quot;:{}}" href="#">
                <img class="image aside__icon" role="img" src="img/i_attach.png">
        </a>
</div>
<div class="aside__list aside__list_position_bottom">
        <a class="link link__control aside__link i-bem" data-bem="{&quot;link&quot;:{}}" href="#">
                <img class="image aside__icon" role="img" src="img/i_hdd.png">
        </a>
        <a class="link link__control aside__link i-bem" data-bem="{&quot;link&quot;:{}}" href="#">
                <img class="image aside__icon" role="img" src="img/i_set.png">
        </a>
</div>
```
Эту часть кажется логичнее было бы сделать списком (ведь это элемент `list`) ну и конечно картинки хорошо было бы собрать в спрайт и выставлять через background, ведь иконки представляют из себя не контент, а оформление.

### Блок panorama
#### Элемент bg:
```html
<div class="panorama__bg">
        <img class="image panorama__image" role="img" src="img/panorama.jpg" alt="city">
</div>
```
Искренне непонятно его назначение, это уже не первый случай где у нас просто `div` с картинкой внутри. Кажется стоит чаще смотреть в сторону `css` свойства `background` :)

#### Элемент city:
```html
<div class="panorama__city">
        <div class="panorama__geo">St.Peterburg</div>
        <div class="panorama__geo-icon">
                <img class="image" role="img" src="img/i_location.png" alt="city">
        </div>
</div>
```
Тут ты и так позиционируешь элемент абсолютно, так зачем столько дивов?
Ну и опять картинка, которую по хорошему стоит показывать через псевдоэлемент для элемента `geo`.

### Блок stickers
#### Элемент sticker:
```html
<div class="stickers__sticker">
        <div class="stickers__icon stickers__icon_type_time">
                <img class="image" role="img" src="img/i_time.png" alt="time">
        </div>
        <div class="stickers__text">Best time to visit</div>
        <div class="stickers__mark-text">March</div>
        <div class="stickers__bg stickers__bg_type_time"></div>
</div>
```
Опять картинка вместо фонового изображения.

### Блок card:
```html
<div class="card places__card">
        <img class="image card__img" role="img" src="img/card-image.jpg">
        <img class="image card__author" role="img" src="img/card-user.jpg">
        <div class="card__info card__info_align_left">
                <div class="card__name">Anton gcor</div>
                <div class="card__place">Moscow, Russia</div>
        </div>
        <div class="card__info card__info_align_right">
                <div class="card__like">255</div>
        </div>
</div>
```
Ну тут допустим для `like` иконка реализованна через `::before` и все хорошо.
Из непонятного, почему нижняя белая плашка - это просто пустое место?
Кажется что эти карточки расчитаны только под определенные размеры картинки и аватарки, а в противном случае - резъедутся.

### Блок chat
#### Элемент list:
```html
<div class="chat__list">
        <a class="traveller chat__traveller" href="">
                <img class="image traveller__img" role="img" src="img/user-chat.png" alt="img">
                <div class="traveller__name">Jeremy Shaw</div>
                <div class="traveller__status"></div>
        </a>
</div>
```
И опять у нас непонятная семантика, `div` с ссылками внутри, которые в себе содержат богатый мир.
Кажется тут стоит использовать список с ссылками, а элемент status сделать псевдоэлементом, который меняет свой вид в зависимости от модификатора.



## Взгляд снизу (блоки, bemjson и шаблоны)
В первой части ревью я специально рассматривал все с точки зрения верстальщика, никак не завязываясь на БЭМ методологию, чтобы показать что в этой области еще есть куда расти и это хорошо.

Тут я постараюсь объяснить что стоит использовать в работе с БЭМ и как это может упростить жизнь.

### Блоки и их зависимости.
По коду проекта я не нашел ни одного файла `deps.js`, который должен содержать в себе описание зависимостей от других блоков, думаю это стоит взять во внимание. Блоки у нас бывают не только односложные, но и составные, для которых необходимо описывать в зависимостях из чего они состоят.

Материал по этой теме можно найти найти тут [bem.info: зависимости](https://ru.bem.info/tools/bem/bem-tools/depsjs/)

### Входные данные в виде bemjson и шаблоны для их обработки
Во время ревью появилось подозрение что bemjson воспринимают как альтернативу написания `html`, но по моему это неверно. Думаю что правильнее было бы воспринимать его как формат входных данных, с помощью которого можно описывать внешний вид страниц.

Данные используемые в bemjson являются view-ориентированными и могут обрабатываться в шаблонах, которые этот самый view и строят. Тут в качестве примера я хочу привести элемент `list`, блока `places`.

Вот что мы имеем в bemjson:
```js
{
    elem: 'list',
    content: [
        {
            image: '/img/card-image.jpg',
            author: '/img/card-user.jpg',
            name: 'Anton gcor',
            place: 'Moscow, Russia',
            like: '255'
        },
        {
            image: '/img/card-image.jpg',
            author: '/img/card-user.jpg',
            name: 'Anton gcor',
            place: 'Moscow, Russia',
            like: '255'
        },
        {
            image: '/img/card-image.jpg',
            author: '/img/card-user.jpg',
            name: 'Anton gcor',
            place: 'Moscow, Russia',
            like: '255'
        },
        {
            image: '/img/card-image.jpg',
            author: '/img/card-user.jpg',
            name: 'Anton gcor',
            place: 'Moscow, Russia',
            like: '255'
        },
        {
            image: '/img/card-image.jpg',
            author: '/img/card-user.jpg',
            name: 'Anton gcor',
            place: 'Moscow, Russia',
            like: '255'
        },
        {
            image: '/img/card-image.jpg',
            author: '/img/card-user.jpg',
            name: 'Anton gcor',
            place: 'Moscow, Russia',
            like: '255'
        }
    ].map(function(card) {
            return {
                block: 'card',
                mix: {block: 'places', elem: 'card'},
                content: [
                    {
                        block: 'image',
                        mix: {block: 'card', elem: 'img'},
                        url: '../..' + card.image
                    },
                    {
                        block: 'image',
                        mix: {block: 'card', elem: 'author'},
                        url: '../..' + card.author
                    },
                    {
                        elem: 'info',
                        elemMods: {align: 'left'},
                        content: [
                            {
                                elem: 'name',
                                content: card.name
                            },
                            {
                                elem: 'place',
                                content: card.place
                            }
                        ]
                    },
                    {
                        elem: 'info',
                        elemMods: {align: 'right'},
                        content: [
                            {
                                elem: 'like',
                                content: card.like
                            }
                        ]
                    }
                ]
            }
        })
}
```

Мы передаем в `content` данные, которые потом обрабатываем через `map`.
Все это можно упростить в разы, передав данные в контекст элемента:
```js
{
    elem: 'list',
    itemsList: [
        {
            image: '/img/card-image.jpg',
            author: '/img/card-user.jpg',
            name: 'Anton gcor',
            place: 'Moscow, Russia',
            like: '255'
        },
        {
            image: '/img/card-image.jpg',
            author: '/img/card-user.jpg',
            name: 'Anton gcor',
            place: 'Moscow, Russia',
            like: '255'
        },
        {
            image: '/img/card-image.jpg',
            author: '/img/card-user.jpg',
            name: 'Anton gcor',
            place: 'Moscow, Russia',
            like: '255'
        },
        {
            image: '/img/card-image.jpg',
            author: '/img/card-user.jpg',
            name: 'Anton gcor',
            place: 'Moscow, Russia',
            like: '255'
        },
        {
            image: '/img/card-image.jpg',
            author: '/img/card-user.jpg',
            name: 'Anton gcor',
            place: 'Moscow, Russia',
            like: '255'
        },
        {
            image: '/img/card-image.jpg',
            author: '/img/card-user.jpg',
            name: 'Anton gcor',
            place: 'Moscow, Russia',
            like: '255'
        }
    ]
}
```

А для элемента `list` достаточно написать небольшой `bemhtml` шаблон:
```js
block('places').elem('list')(

    content()(function() {
        var items = this.ctx.itemsList;

        return items.map(function(item) {
            return {
                block: 'card',
                mix: {block: 'places', elem: 'card'},
                content: [
                    {
                        block: 'image',
                        mix: {block: 'card', elem: 'img'},
                        url: '../..' + item.image
                    },
                    {
                        block: 'image',
                        mix: {block: 'card', elem: 'author'},
                        url: '../..' + item.author
                    },
                    {
                        elem: 'info',
                        elemMods: {align: 'left'},
                        content: [
                            {
                                elem: 'name',
                                content: item.name
                            },
                            {
                                elem: 'place',
                                content: item.place
                            }
                        ]
                    },
                    {
                        elem: 'info',
                        elemMods: {align: 'right'},
                        content: [
                            {
                                elem: 'like',
                                content: item.like
                            }
                        ]
                    }
                ]
            }
        })
    })

);
```

И описать все зависимости:
```js
[{
    shouldDeps: [
        {
            block: 'card',
            elems: [
                'author',
                'img',
                {
                    elem: 'info',
                    mods: {
                        align: ['left', 'right']
                    }
                },
                'like',
                'name',
                'place'
            ]
        },
        {
            block: 'places',
            elems: ['card']
        }
    ]
}]
```

Далее все можно сделать еще более красивее, написать шаблон для блока `card` и уже у него описать зависимости, поле для деятельности весьма обширное.


## Заключение.
Антон, тебе еще есть куда расти, погружение в БЭМ у тебя проходит довольно гладко, но стоит обратить внимание на верстку, ведь она является довольно важной частью web-интерфейсов. Я пришел в Яндекс как стажер и нам давали довольно забавные задания, решение которых предполагало использование наименьшего колличества кода, в качестве примера могу показать тебе свой [календарь](http://belyanskii.github.io/css-calend/).

Что же касается БЭМ мира, думаю тебе должны помочь примеры различных проектов, смотри как они написанны и старайся использовать у себя. Так же думаю стоит разобраться с `i-bem` и вообще со всем что касается `js` в мире блоков, это трудно на первый взгляд, особенно отложенная инициализация и остальная магия, но поверь, когда ты разберешься, использовать весь стек станет довольно легко и приятно :)

Вот небольшой список проектов на БЭМ, в которые заглядываю я:
 - [Библиотека bem-core](https://github.com/bem/bem-core) - её пишут отцы основатели, стоит разобраться в исходника :)
 - [bem-components](https://github.com/bem/bem-components) - пример написания независимых блоков для нескольких платформ с чистым `API` и красивым кодом
 - [bem-site-engine](https://github.com/bem/bem-site-engine) - сердце сайта bem.info, много непонятного, но от этого не менее хорошего кода.
