Я веду переговоры с несколькими компаниями, поэтому точно назвать цифру сложно. Однако минимальная сумма, с которой я готов рассматривать предложения, составляет 260,000 рублей "на руки". У меня уже есть несколько предложений, которые превышают эту сумму, так что конечная цифра может быть немного выше.

Доброе утро! У меня сейчас ещё 2 оффера на руках от крупных банков на 270 и 260 оклада на руки + схожие премии.

Проекты там тоже адекватные, но после собеса с вашим лидом был больший мэтч, чем с другими компаниями. Есть больше новых для меня вещей, а также почувствовал, что команда по софтам очень хорошая.

Я понимаю, что это больше, чем вы предлагаете, но так как ваш проект для меня сейчас в приоритете, хотелось бы прийти к компромиссу




Вопросы с собесов: https://brave-interviews-frontend.notion.site/Frontend-33de609d7e8b4a6b8d7dc7f23070c28b?pvs=4
Роамап (там много прям супер базовых и скучных вещей, можешь по ним быстро пробежаться): https://www.notion.so/brave-interviews-frontend/Frontend-Roadmap-4fb8a827811f436ab5febe759a2079cc?pvs=4#5829fdf72378444ca50e0accaa5454b7


по факту можно отсюда подучить все и идти на собесы
https://brave-interviews-frontend.notion.site/Frontend-33de609d7e8b4a6b8d7dc7f23070c28b?pvs=4

База вопросов по фронтенду:
 1. [основы JavaScript](../questions_file/my_work_questions.md)


Основы JavaScript
1. [Реальный DOM](../questions_file/real_DOM.md)
2. [Виртуальный DOM](../questions_file/virtual_DOM.md)
2. [Виртуальный DOM](../questions_file/virtual_DOM.md)




Вопросы по JavaScript:

1. Что такое замыкание?  

    замыкание - это функция, которая запоминает своё лексическое окружение во время объявления и имеет к нему доступ во время выполнения. 
    ```javascript
    function makeCounter() {
        let count = 0;
        return function() {
            return count++;
        }
    }
    let counter = makeCounter();
    console.log(counter()); // 0
    console.log(counter()); // 1
    console.log(counter()); // 2
    ```
   
2. Что такое this?

   this - это контекст выполнения функции. Значение this зависит от того, как вызвана функция. 
   ```javascript
    function sayHi() {
         console.log(this.name);
    }
    let user = { name: "John" };
    let admin = { name: "Admin" };
    user.f = sayHi;
    admin.f = sayHi;
    user.f(); // John
    admin.f(); // Admin
    admin['f'](); // Admin
    ```
   
3. Что такое прототипное наследование?

    Прототипное наследование - это способ организации наследования в JavaScript. Каждый объект имеет ссылку на прототип, и если у объекта нет свойства, то оно ищется в прототипе. 
    ```javascript
     let animal = {
          eats: true
     };
     let rabbit = {
          jumps: true
     };
     rabbit.__proto__ = animal;
     console.log(rabbit.eats); // true
     console.log(rabbit.jumps); // true
     ```
   
4. Что такое Event Loop?

    Event Loop - это механизм, который позволяет JavaScript выполнять несколько задач одновременно. Он состоит из стека вызовов, очереди событий и цикла обработки событий. 
    ```javascript
    console.log('1');
    setTimeout(() => console.log('2'), 0);
    console.log('3');
    // 1
    // 3
    // 2
    ```
    
5. Что такое Promise?

    Promise - это объект, который представляет результат асинхронной операции. Он может находиться в трёх состояниях: ожидание, выполнение и завершение. 
    ```javascript
    let promise = new Promise((resolve, reject) => {
        setTimeout(() => resolve('done'), 1000);
    });
    promise.then(console.log); // done
    ```
6. Что такое async/await?

    async/await - это синтаксический сахар для работы с промисами. async - объявляет асинхронную функцию, а await - останавливает выполнение функции до тех пор, пока промис не выполнится. 
    ```javascript
    async function f() {
        let promise = new Promise((resolve, reject) => {
            setTimeout(() => resolve('done'), 1000);
        });
        let result = await promise;
        console.log(result);
    }
    f(); // done
    ```
7. Что такое CORS?

    CORS (Cross-Origin Resource Sharing) - это механизм, который позволяет веб-страницам запрашивать ресурсы с других доменов. Он предотвращает атаки CSRF и позволяет разработчикам создавать более безопасные веб-приложения. 
    ```javascript
    fetch('https://api.example.com/data')
        .then(response => response.json())
        .then(data => console.log(data))
        .catch(error => console.log(error));
    ```
8. Что такое REST?

    REST (Representational State Transfer) - это архитектурный стиль для построения распределенных систем. Он основан на принципах, таких как клиент-сервер, отсутствие состояния и кэширование. 
    ```javascript
    GET /api/users
    POST /api/users
    PUT /api/users/:id
    DELETE /api/users/:id
    ```
9. Что такое MVC?

    MVC (Model-View-Controller) - это шаблон проектирования, который разделяет приложение на три компонента: модель, представление и контроллер. Модель отвечает за данные, представление - за отображение данных, а контроллер - за обработку запросов. 
    ```javascript
    class Model {
        constructor() {
            this.data = [];
        }
        add(item) {
            this.data.push(item);
        }
    }
    class View {
        render(data) {
            console.log(data);
        }
    }
    class Controller {
        constructor(model, view) {
            this.model = model;
            this.view = view;
        }
        addItem(item) {
            this.model.add(item);
            this.view.render(this.model.data);
        }
    }
    let model = new Model();
    let view = new View();
    let controller = new Controller(model, view);
    controller.addItem('item');
    ```
10. Что такое SPA?
    
    SPA (Single Page Application) - это веб-приложение, которое загружает только одну HTML-страницу и динамически обновляет её содержимое без перезагрузки. Оно обычно использует AJAX для загрузки данных и роутинг для навигации. 
    ```javascript
    <div id="app"></div>
    <script>
        let app = document.getElementById('app');
        app.innerHTML = 'Hello, World!';
    </script>
    ```

11. Что такое SSR?
    
    SSR (Server-Side Rendering) - это процесс отображения веб-страниц на сервере и отправки готового HTML-кода клиенту. Он улучшает производительность и SEO веб-приложений. 
    ```javascript
    const express = require('express');
    const app = express();
    app.get('/', (req, res) => {
        res.send('Hello, World!');
    });
    app.listen(3000, () => {
        console.log('Server is running on port 3000');
    });
    ```
12. Что такое SEO?

    SEO (Search Engine Optimization) - это процесс оптимизации веб-страниц для поисковых систем. Он включает в себя использование ключевых слов, мета-тегов, ссылок и других методов для улучшения рейтинга страницы в поисковых результатах. 
    ```html
    <meta name="description" content="Web development tutorials">
    <meta name="keywords" content="web development, tutorials, programming">
    <title>Web Development Tutorials</title>
    ```
13. Что такое PWA?
    
    PWA (Progressive Web App) - это веб-приложение, которое сочетает в себе преимущества веб-сайтов и нативных приложений. Оно может работать офлайн, отправлять уведомления и обновляться автоматически. 
    ```html
    <link rel="manifest" href="/manifest.json">
    <script>
        if ('serviceWorker' in navigator) {
            navigator.serviceWorker.register('/sw.js');
        }
    </script>
    ```

14. Что такое Webpack?
15. Что такое Babel?
16. Что такое ESLint?
17. Что такое TypeScript?
18. Что такое JSX?
19. Что такое Virtual DOM?
20. Что такое React?
21. Что такое Redux?
22. Что такое MobX?
23. Что такое Vue?
24. Что такое Angular?
25. Что такое Web Components?
26. Что такое Shadow DOM?
27. Что такое CSS-in-JS?
28. Что такое SSR?
29. Что такое CSR?
30. Что такое HOC?
31. Что такое Render Props?
32. Что такое Hooks?
33. Что такое Context?
34. Что такое Refs?
35. Что такое Portal?
36. Что такое Code Splitting?
37. Что такое Lazy Loading?
38. Что такое CSRF атака?
