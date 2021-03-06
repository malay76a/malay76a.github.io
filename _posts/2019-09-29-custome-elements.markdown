---
layout: post
title:  "Пользовательские элементы"
date:   2019-09-29 00:00:00 +0300
categories: web-components
---

Как часто вам приходилось верстать попап для очередного сайта? По факту это же просто, слой затемнения бекграунда слушающий клик для закрытия попапа, слой с контентом, кнопка закрытия. В большинстве случаев разница на разных сайтах не существенная, в основном лишь стилистика. У вас никогда не возникало ощущения, что все эти элементы - это стандарт сегодняшних дней почти на любом сайте и почему нет стандартных элементов которые решают этот вопрос, ведь это "стандартное" поведение для современного сайта?

Разработчикам не хотелось каждый раз придумывать очередной попап или карусели и в какой-то момент роль поставщика универсальных компонентов взял на себя `jQuery`.

Для примера, возьмем [slick](https://kenwheeler.github.io/slick/) слайдер. Прошу обратить внимание на одну из его инициализаций на странице:

{% highlight javascript %}
$(".multiple-items").slick({
    infinite: true,
    slidesToShow: 3,
    slidesToScroll: 3
});
{% endhighlight %}
    
Любая валидная структура `html` для этой `jQuery` библиотеки автоматически становится компонентом слайдер:

{% highlight html %}
<div class="multiple-items">
    <div>
        <h3>1</h3>
    </div>
    <div>
        <h3>2</h3>
    </div>
    <div>
        <h3>3</h3>
    </div>
    <!-- .... -->
</div>
{% endhighlight %}    

На мой взгляд, это отличный пример компонента который решает поставленную перед ним задачу вне зависимости от сайта и используемого стека технологий (ну за исключением необходимости так же подключить `jQuery`) - преобразовать выделенную ему структуру в компонент-слайдер.

А теперь давайте представим, как бы выглядел такой же компонент слайдера, но семантично понятный в `html`:

{% highlight html %}
<uc-slider
    infinite="true"
    slidesToShow="3"
    slidesToScroll="3">
    <div>
        <h3>1</h3>
    </div>
    <div>
        <h3>2</h3>
    </div>
    <div>
        <h3>3</h3>
    </div>
    <!-- .... -->
</uc-slider>
{% endhighlight %} 

На первый взгляд кажется мало что изменилось, но обратите внимание, сейчас сразу глядя на структуру можно сказать что это какой-то слайдер с определённым поведением задаваемым через атрибуты. Вспомним, например, тег `input`. У него через атрибут `type` кардинально меняется поведение, у каждого оно свое и свои доступные атрибуты, но при этом достаточно увидеть тег `input` и понимаем, что этот блок о взаимодействии с пользователем. То же поведение и в примере со слайдеров, через его атрибуты мы можем управлять слайдером, но при этом семантически это всегда слайдер.

Пользовательские элементы предоставляют разработчикам возможность создавать собственные полнофункциональные элементы DOM. Определив пользовательский элемент, разработчики сообщают браузеру, как правильно конструировать элемент и как элементы этого класса должны реагировать на изменения.

Пользовательские элементы определяются на странице тегом содержащим дефис и обязательно с закрывающим тегом.

{% highlight html %}
<custom-element text="lorem"></custom-element>
{% endhighlight %}

Наличие дефис в названии пользовательского элемента [обязательно в соответствии со спецификацией](https://html.spec.whatwg.org/#valid-custom-element-name) для того что бы избежать ситуации с конфликтом имен в будущем.

Далее нам надо определить логику пользовательского элемента для этого нужно расширить стандартный элемент.

{% highlight javascript %}
// Описание API пользовательских элементов 
// в текущем примере расширяется абстрактный HTMLElement 
// this ссылается на сам элемент 
class CustomElement extends HTMLElement { 
    // конструктор срабатывает при регистрации компонента 
    // обязателен вызов метода super() 
    // определяют/задают значения атрибутов/свойств 
    // добавляют Shadow DOM 
    constructor() { 
        super(); 
    } 

    // вызывается после инициализации элемента 
    // в этом методе можно получить доступ к DOM элемента 
    // в этом методе вызывают рендеринг внутренней структуры элемента 
    // в том числе асинхронные вызовы так же определяют в нем 
    connectedCallback() { 
        this._render();
    }

    // вызывается при удалении элемента из DOM 
    // типичное использование, например, удаление обработчиков событий
    disconnectedCallback() {} 

    // observer для атрибутов пользовательского элемента 
    // возвращает массив строк с названием атрибутов 
    // изменение которых будет вызывать attributeChangedCallback 
    static get observedAttributes() { 
        return ["text"];
    }

    // вызывается при изменении атрибута пользовательского элемента 
    // первый параметр - название атрибута 
    // второй параметр - старое значение атрибута 
    // третий параметр - новое значение 
    // сравнение нового и старого значения помогает для определения 
    // необходимости в ререндеринге элемента 
    attributeChangedCallback(name, oldValue, newValue) {} 

    // вызывается когда пользовательский элемент перемещается 
    // в другое окно (мне сложно привести пример такого кейса) 
    adoptedCallback() {} 

    // вынесение рендеринга в отдельный метод 
    // не является стандартным методом API 
    // но рекомендуется, как хорошая практика 
    _render() {} 
}
{% endhighlight %}

И после регистрируем этот компонент на определенное имя тега.

{% highlight javascript %}
// первый параметр - это название пользовательского элемента в DOM 
// второй параметр - класс который реализует логику данного элемента 

customElements.define("сustom-element", CustomElement);
{% endhighlight %}

Вообще у customElements есть несколько методов для регистрации пользовательского элемента, а так же дополнительные методы - утилиты:

{% highlight javascript %}
// первый параметр - это название пользовательского элемента в DOM 
// второй параметр - класс который реализует логику данного элемента 

customElements.define('name', Constructor) 

// первый параметр - это название пользовательского элемента в DOM 
// второй параметр - класс который реализует логику данного элемента 
// третий параметр - объект указывающий на расширяемый базовый элемент
	
customElements.define('name', Constructor, { extends: baseLocalName }) 

// возвращает конструктор пользовательского элемента в соответствии 
// с переданным в параметре именем. Возвращает undefined если такой 
// если такой параметр не определен 

customElements.get('name') 

// возвращает промис который, будет выполнен когда элемент с указанным 
// именем будет определен. Если такой элемент уже определен, то возвращает 
// промис который будет немедленно выполнен

customElements.whenDefined('name') 

// пытается обновить все элементы shadow dom 

customElements.upgrade(root)
{% endhighlight %}

В методе `_render()`, нас никто не ограничивает, как работать с внутренней структурой. Мы можем, как задать эту структуру через `innerHTML`:

{% highlight javascript %}
this.innerHTML = `<p>${this.text}</p>`;
{% endhighlight %}

Так и через создание элементов стандартным API и добавление их через `appendChild()`:

{% highlight javascript %}
const paragraph = document.createElement("p"); 
paragraph.textContent = "Lorem"; 
this.appendChild(paragraph);
{% endhighlight %}

Прошу обратить внимание на то, что если вам необходимо создать список, то не надо вызывать `appendChild()` на каждый элемент этого списка. Для это создайте [DocumentFragment](https://developer.mozilla.org/ru/docs/Web/API/DocumentFragment), наполните его элементами и его уже и передавайте в `appendChild()`.

Основные правила метода `constructor()`:

1. в конструкторе первой строкой обязательно вызвать `super()`. Это позволит установить правильную цепочку прототипов.
2. `constructor()` используют для задания первоначальных переменных и добавления слушателей событий.

Для всего остального используют `connectedCallback()`.

### Ссылки:

1. [HTML Living Standard: Custom elements](https://html.spec.whatwg.org/#custom-elements)