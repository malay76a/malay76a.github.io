---
layout: post
title:  "Введение"
date:   2019-09-28 00:00:00 +0300
categories: web-components
---

Front-end развивается очень быстро. Каждый день появляются новые библиотеки, а другие отмирают и использование их считается дурным тоном (привет `jQuery`, `Backbone`). Все это связанно с желанием разработчиков делать свою работу быстро, с меньшим количеством багов, с ясной с первого взгляда структурой и удобством расширения проекта. А так же с желанием бизнеса получать решение своих идей в короткие сроки. На текущий момент, такими решениями являются Angular, React, Vue. Эти технологии хороши, но в случае, если проект только начинает свою жизнь или его переписывают с нуля. Но зачастую со стороны бизнеса не заметна необходимость, что-то менять во взрощенном им в течение многих лет проекте или цена этих изменений в краткосрочной перспективе неясна или несоразмерна с их ожиданиями. В таком случае я предлагаю обратить внимание на стек технологий под названием "веб-компоненты".

В html 5 появились семантические теги такие как header, footer, nav... Основная идея заключается в следующем:

> Семантические элементы HTML5 доступно описывают свой смысл или назначение как для браузеров, так и для веб-разработчиков.

Идея прекрасная, но не рабочая. Зачастую разработчики просто игнорируют данные теги т.к. фактической пользы от их использования — нет. Для доказательства того, что семантические теги не влияют на браузеры и разработчиков, я предлагаю обратиться к сайтам поисковых систем и разработчиков браузеров.

Возьмем страницу поиска [Google](https://www.google.ru/), [Yandex](https://yandex.ru/) и [Bing](https://www.bing.com/), посмотрев в верстку вы вряд ли найдете, например, тег nav. Я думаю, уж если гиганты, которые имеют огромные ресурсы разработки, которые внедряют и развивают спецификации которыми мы пользуемся, пренебрегают семантикой тегов html 5, то о какой теоретической пользе от нее может быть речь. Если взять страницу [Firefox](https://www.mozilla.org/ru/) (RU) и [Apple](https://www.apple.com/), то тут есть "частичная" интеграция семантической верстки.

Есть частные случаи, когда семантика важна, например в [apple watch](https://developer.apple.com/videos/play/wwdc2018/239/). Но это очень специфичный кейс.

Про `SEO` я так и не нашёл достоверной информации об улучшении рейтинга в поисковой выдаче при использовании семантики. Для меня seo сродни магии, я понимаю, что мы работаем с "черным ящиком". Вкидываем в него что-то и ждем результат.

Есть некоторые сложившиеся "издревле" хорошие практики того, как улучшить ранжирование в поисковой выдаче. Но с семантикой такого нет и вряд ли будет. Почему? На мой взгляд из-за неторопливости системы т.е. результат ранжирования мы увидим предположительно не раньше чем через 2 недели, плюс на ранжирование влияют внешние ссылки и мы не можем гарантировать, что не появится новых ссылок на этот сайт для данного поискового слова или фразы, плюс ко всему наши конкуренты не стоят на месте и так же вносят изменения в свои сайты, что бы добиться лучших позиций при ранжировании и еще множество и множество зависимостей которые будут влиять на поисковую выдачу, а значит изолированный тест не возможен. Со стороны разработчиков поисковых систем я не видел информации, что семантика на сайте влияет на поисковую выдачу.

Ну и последнее — доступность. Хотите нормально управлять доступностью? Используйте `aria` атрибуты. И это не мои слова, а представителя из Яндекс, который на одном из митапов посвященных `БЭМ` и его применению в `react`, [рассказал об этом](https://www.youtube.com/watch?v=4D90HexDyOY).

В сухом остатке — семантическое теги это чисто визуальное представление структуры сайта для разработчика не реализующий никакой логики и предоставляющие базовую доступность элементов.

С недавних пор у разработчиков появилась возможность создавать "семантичные теги" и при этом они будут нести определенную логику — это веб-компоненты. Веб-компоненты - это шаг со стороны стандартов и нативных технологий к созданию пользовательских, переиспользуемых, изолированных "виджетов".

По факту, разработчикам дали возможность создавать свои теги или расширять функционал существующих. При этом каждый тег будет описывать себя так, как удобно в текущей разработке.

Когда семантические теги лишь приблизительно описывали свое наполнение, пользовательские элементы описывают точно без угрозы для SEO, валидации структуры и общей функциональности сайта.

Изначально, веб-компоненты были только лишь подходом при реализации компонентов в вебе совмещая 4 спецификации. 

1. [Shadow DOM](https://w3c.github.io/webcomponents/spec/shadow/). Основным понятием в данной технологии является [shadow tree](https://w3c.github.io/webcomponents/spec/shadow/#shadow-tree) которое является деревом node и прикрепляется к корневому элементу и не может существовать без него. Такое дерево можно рассматривать как среднее между частью документа и фрагментом. В то же время - это абстракция инкапсуляции и позволяет изолировать все что находится в shadow tree от внешнего контекста;
2. [HTML-шаблоны](https://www.w3.org/TR/html50/scripting-1.html#the-template-element). Данная технология предоставила 2 тега <template> и <slot>. Эти два тега инкапсулируют и скрывают из визуализации все что в них находится и являются способом многократного использования;
3. [Custom element](https://html.spec.whatwg.org/#custom-elements). Эта технология предоставляет возможность определять в браузере пользователя свои теги html и определять их поведение;
4. [HTML-импорты](https://w3c.github.io/webcomponents/spec/imports/). С этой технологий не [все так однозначно](https://caniuse.com/#search=import), можно сказать, что она изжила себя на текущий момент. Данная проблема возникла из-за отказа со стороны Mozila в реализации в своем браузере данной спецификации. [Причиной](https://hacks.mozilla.org/2014/12/mozilla-and-web-components/)  послужило то, что механизм импортов уже реализован в браузере - это ES6 модули.

### Ссылки:

1. [https://github.com/w3c/webcomponents/](https://github.com/w3c/webcomponents/)
2. [https://html.spec.whatwg.org/multipage/sections.html#the-nav-element](https://html.spec.whatwg.org/multipage/sections.html#the-nav-element)
3. [https://html5book.ru/html5-semantic-elements/#part9](https://html5book.ru/html5-semantic-elements/#part9)
4. [https://medium.com/@stasonmars/секреты-использования-семантической-верстки-в-html5-c7cd5e6f1ebb](https://medium.com/@stasonmars/%D1%81%D0%B5%D0%BA%D1%80%D0%B5%D1%82%D1%8B-%D0%B8%D1%81%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D1%8F-%D1%81%D0%B5%D0%BC%D0%B0%D0%BD%D1%82%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%BE%D0%B8%CC%86-%D0%B2%D0%B5%D1%80%D1%81%D1%82%D0%BA%D0%B8-%D0%B2-html5-c7cd5e6f1ebb)
5. [https://ru.megaindex.com/blog/semantic-markup](https://ru.megaindex.com/blog/semantic-markup)
6. [https://web-standards.ru/articles/pointless-semantic/](https://web-standards.ru/articles/pointless-semantic/)