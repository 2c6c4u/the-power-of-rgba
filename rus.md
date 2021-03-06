# Сила цветовой CSS-функции `rgba()`

Одна из наиболее интересных мне вещей в CSS — новая [функция][2] [color-mod][1]. Она позволит нам манипулировать цветами прямо в браузере. Например, при наведении на кнопку мы сможем изменить её цвет примерно так — `color: color(black darkness(50%));`, не используя препроцессоры вроде Sass.

Но пока поддержка таких CSS-функций не реализована, мы можем [временно][3] использовать PostCSS для их компиляции в обычные цвета. Или мы можем поэкспериментировать и исследовать силу функции `rgba()`, чтобы менять цвета на лету! Давайте посмотрим, как её можно использовать.

## Как это работает

![Как это работает. Часть 1.][4]

Когда в графическом редакторе мы помещаем чёрный и белый блоки над большим синим блоком (как в примере), то в результате получаем, соответственно, более светлый или более тёмный синий цвет.

Это происходит потому, что при увеличении прозрачности цвета будут смешиваться на основе того, белый это или чёрный. Знаете, что произойдет, если изменить синий фон на зелёный? Верно!

![Как это работает. Часть 2.][5]

Как видите, при изменении цвета фона маленькие блоки стали выглядеть иначе: мы получили светлый и тёмный зелёный. Также мы можем изменить значение прозрачности, чтобы выбрать более светлый или тёмный оттенок. Давайте отработаем этот базовый навык, чтобы перейти к реальным примерам.


## Применение концепции

Чтобы сохранить лаконичность предыдущего примера, поиграем с прозрачностью. В реальном дизайне нам может понадобиться альфа-значение `rgba()`.


    .header {
        background: rgba(0, 0, 0, 0.5); /* Чёрный с непрозрачностью 50% */
    }


Здесь мы взяли `background` вместо `opacity`, потому что при использовании `opacity` будут затронуты все потомки, а нам это не нужно. Если мы используем альфа-канал для свойства `background`, мы обеспечиваем изменение только нужного элемента.

Примечание: в демо для скорости мы будем использовать `rgba()`-функции Sass. Например:


    .elem {
        background: rgba(white, 0.5);
    }

скомпилируется в:


    .elem {
        background: rgba(255, 255, 255, 0.5);
    }


### Оформление шапки сайта

Первый вариант использования `rgba()` — это стилизация шапок веб-приложений. В [Trello][6] используются цвета фона с `rgba()` для дочерних элементов шапки (логотип, поле поиска, кнопки):

    .search {
        background: rgba(255, 255, 255, 0.5); /* Белый с 50% альфа-каналом */
    }

![Devtools в Trello][7]

Так мы получим возможность стилизовать шапку, меняя только её фон, и вместе с фоном будут изменены дочерние элементы.

В нашем примере мы можем сделать что-то похожее на шапку Trello и поиграть с фоном через панель разработки.

![Шапка Trello][8]

Обратите внимание, как отличается цвет фона потомков в двух шапках. Если мы хотим исследовать элемент и изменить фон родителя, мы можем очень просто настроить шапку.

![Стилизация шапки][9]

<p data-height="350" data-theme-id="0" data-slug-hash="XpEwRy" data-default-tab="result" data-user="FMRobot" data-embed-version="2" data-pen-title="XpEwRy" class="codepen"><a href="http://codepen.io/FMRobot/pen/XpEwRy/">Посмотреть пример шапки можно на CodePen</a>.</p>


### Шапка статьи

В этом примере использование `rgba()` будет полезно для:

* Рамки верхнего и нижнего краёв
* Цвета фона отцентрованной обёртки
* Цвета фона ссылок категории

![Заголовок статьи с комментариями][14]


    .parent {
        background: #5aaf4c; /* фон родителя */
        box-shadow:
            inset 0 8px 0 0 rgba(255, 255, 255, 0.5),
            inset 0 -8px 0 0 rgba(255, 255, 255, 0.5); /* верхняя и нижняя рамки */
    }
    
    .contain {
        background: rgba(0, 0, 0, 0.1);
    }
    
    .category {
        background: rgba(255, 255, 255, 0.5);
    }

<p data-height="600" data-theme-id="0" data-slug-hash="MJVdvX" data-default-tab="result" data-user="FMRobot" data-embed-version="2" data-pen-title="MJVdvX" class="codepen"><a href="http://codepen.io/FMRobot/pen/MJVdvX/">Посмотреть пример шапки статьи можно на CodePen.</a></p>


### Кнопки

В темах для кнопок мы можем использовать `rgba()` для создания эффектов по наведению или фокусу через рамки и тени.

    .button {
        background: #026aa7;
        box-shadow: inset 0 -4px 0 0 rgba(0, 0, 0, 0.2);
    }
    
    .button:hover {
        box-shadow: inset 0 -4px 0 0 rgba(0, 0, 0, 0.6), 0 0 8px 0 rgba(0, 0, 0, 0.5);
    }
    
    .button:focus {
        box-shadow: inset 0 3px 5px 0 rgba(0, 0, 0, 0.2);
    }

<p data-height="350" data-theme-id="0" data-slug-hash="egMaeP" data-default-tab="result" data-user="FMRobot" data-embed-version="2" data-pen-title="Кнопки" class="codepen"><a href="http://codepen.io/FMRobot/pen/egMaeP/">Посмотреть пример кнопок можно на CodePen.</a></p>


### Градиенты

Другой полезный приём — это заливка фона сплошным цветом и добавление псевдоэлемента с `rgba()`-цветами для точек перехода цветов градиента.

![Градиенты][17]

    .elem {
        background: green;
    }
    
    .elem:before {
        content: "";
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
        background: linear-gradient(to right, rgba(255, 255, 255, 0.2), rgba(255, 255, 255, 0.7));
    }

Это также даёт нам возможность анимировать градиенты изменением одного только цвета фона.

    .elem {
        /* остальные стили */
        animation: bg 2s ease-out alternate infinite;
    }
    
    @keyframes bg
        to {
            background: green;
        }
    }

<iframe src="https://player.vimeo.com/video/188392007" width="640" height="288" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>

<p data-height="550" data-theme-id="0" data-slug-hash="rjvNpV" data-default-tab="result" data-user="FMRobot" data-embed-version="2" data-pen-title="rjvNpV" class="codepen"><a href="http://codepen.io/FMRobot/pen/rjvNpV/">Посмотреть пример градиентов можно на CodePen.</a></p>


### Вложенный элемент

Если у нас есть навигация списком в шапке, мы можем добавить для неё цвет фона с `rgba()`. Это сделает фон немного прозрачным, поэтому он будет смешиваться с фоном родителя.

<p data-height="400" data-theme-id="0" data-slug-hash="qRYBxR" data-default-tab="result" data-user="FMRobot" data-embed-version="2" data-pen-title="qRYBxR" class="codepen"><a href="http://codepen.io/FMRobot/pen/qRYBxR/">Посмотреть пример вложенных элементов можно на CodePen.</a></p>

То же может использоваться для создания динамических эффектов при наведении:

<p data-height="300" data-theme-id="0" data-slug-hash="wgjvmY" data-default-tab="result" data-user="FMRobot" data-embed-version="2" data-pen-title="wgjvmY" class="codepen"><a href="http://codepen.io/FMRobot/pen/wgjvmY/">Посмотреть пример эффектов при наведении можно на CodePen.</a></p>


### Тёмные/светлые вариации цветовой схемы

Мы можем использовать эту идею для генерации различных оттенков определённой цветовой палитры через размещение псевдоэлемента над каждым цветным блоком с определённым значением `rgba()`.

<p data-height="400" data-theme-id="0" data-slug-hash="zNjYaq" data-default-tab="result" data-user="FMRobot" data-embed-version="2" data-pen-title="zNjYaq" class="codepen"><a href="http://codepen.io/FMRobot/pen/zNjYaq/">Посмотреть пример [цветовой палитры][21] можно на [CodePen][12].</a></p>


### Эффекты изображения

Чтобы сделать изображение темнее или светлее, мы можем использовать псевдоэлемент с `rgba()`-фоном.

![Примеры эффектов][22]

При использовании цветного фона мы можем создавать оттенки цвета. Кроме этого, мы можем использовать свойство `mix-blend-mode`, чтобы смешать фон с изображением для получения разных эффектов.

Важно проверить поддержку `mix-blend-mode` браузерами перед использованием:

    .elem:before {
        background: rgba(0, 0, 0, 1);
        mix-blend-mode: color;
    }

Если `mix-blend-mode` не поддерживается, изображение будет чёрным, и пользователь не сможет его увидеть. Лучше использовать этот эффект как прогрессивное улучшение, но не полагаться на него. В этом может помочь `@support` или Modernizr.

    @supports (mix-blend-mode: color) {
        /* здесь будут ваши расширенные стили */
    }

<p data-height="500" data-theme-id="0" data-slug-hash="apGbjm" data-default-tab="result" data-user="FMRobot" data-embed-version="2" data-pen-title="apGbjm" class="codepen">See the Pen <a href="http://codepen.io/FMRobot/pen/apGbjm/">Посмотреть пример изображений можно на CodePen.</a></p>


## Создание тем с CSS-переменными

Когда вы используете [CSS-переменные][24] (кастомные свойства) для родительских элементов, при изменении переменной меняются ещё и все дочерние элементы. Например:

    :root {
        --brand-color: #026aa7;
    }

    /* Проверка поддержки CSS-переменных */
    @supports (--color: red) {
        .elem {
            background: var(--brand-color);
        }
    }
    
    var colors = ["#026aa7", "#5aaf4c", "#00bcd4", "#af4c4c"];
    var colorOptions = document.querySelectorAll(".option");
    var colorLabels = document.querySelectorAll(".option + label");
    
    for (var i = 0; i < colorOptions.length; i++) {
    
        /* Добавим слушателя события на каждую радио-кнопку */
        colorOptions[i].addEventListener('click', changeColor);
    
        /* Добавим значение каждой радио-кнопке из массива colors[] */
        colorOptions[i].value = colors[i];
    
        colorLabels[i].style.background = colors[i];
    }
    
    function changeColor(e) {
        /* Вызовем корневой элемент и установим значение определённого свойства. В нашем случае: --brand-color */
        document.documentElement.style.setProperty('--brand-color', e.target.value);
    }

Сочетая CSS-переменные и `rgba()`, мы можем делать раскладки и цвета более динамичными без переопределения нового цвета для каждого элемента.

<p data-height="550" data-theme-id="0" data-slug-hash="PWeodd" data-default-tab="result" data-user="FMRobot" data-embed-version="2" data-pen-title="PWeodd" class="codepen"><a href="http://codepen.io/FMRobot/pen/PWeodd/">Посмотреть пример шапки с CSS-переменными можно на CodePen.</a></p>


## Что нужно принимать во внимание

### Запасной цвет

Хотя глобальная поддержка CSS-цветов составляет примерно [97.39%][26], важно обеспечить запасной вариант для браузеров, в которых они не поддерживаются.

    .elem {
        background: #fff;
        background: rgba(255, 255, 255, 0.5); /* браузеры без поддержки проигнорируют эту декларацию */
    }


### Проверка контрастности цвета

Мы должны обеспечивать между элементами контраст, соответствующий гайдлайнам доступности. Иногда при использовании `rgba()` может получиться цвет, который будет резать глаза, но вы можете использовать инструменты вроде [проверки контраста][27] от Lea Verou и определить, соответствуют ли цвета стандартам доступности.

 [1]: https://drafts.csswg.org/css-color/#modifying-colors
 [2]: https://github.com/postcss/postcss-color-function
 [3]: https://cloudfour.com/thinks/building-themes-with-css4-color-features/
 [4]: img/basic-idea.jpg
 [5]: img/basic-idea-green.jpg
 [6]: https://trello.com
 [7]: img/trello.jpg
 [8]: img/Screen-Shot-2016-10-22-at-6.38.58-AM.png
 [9]: img/theming-header.gif
 [10]: http://codepen.io/shadeed/pen/79bb883e8f17c41a90f46c8fdf1a40e2/
 [11]: http://codepen.io/shadeed
 [12]: http://codepen.io
 [13]: img/Screen-Shot-2016-10-22-at-6.56.33-AM.png
 [14]: img/article-header.jpg
 [15]: http://codepen.io/shadeed/pen/fc254c1f120cc38a1b199f96d1d07a85/
 [16]: http://codepen.io/shadeed/pen/f822dd1cac006c00d5418cddac19efac/
 [17]: img/gradients.jpg
 [18]: http://codepen.io/shadeed/pen/808cbbdee59ddffdd39c91c2f1a290f1/
 [19]: http://codepen.io/shadeed/pen/760a044dd7e6a7fed972ce3ee08dc02a/
 [20]: http://codepen.io/shadeed/pen/eb5a955b69c2e31b3dcc0e9131db21b2/
 [21]: http://codepen.io/shadeed/pen/c2a60d0ed9e0313a76cc2c24e05402c7/
 [22]: img/images.jpg
 [23]: http://codepen.io/shadeed/pen/759699e8fc8d5253540fd33880cafa81/
 [24]: https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_variables
 [25]: http://codepen.io/shadeed/pen/a78a3d28eee617784cc75e2fa3dfad76/
 [26]: http://caniuse.com/#search=rgba
 [27]: http://leaverou.github.io/contrast-ratio/
