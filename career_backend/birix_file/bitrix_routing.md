В Bitrix24 Framework с версии 20.0.0 был внедрен новый механизм маршрутизации (роутинг), который позволяет гибко управлять URL-адресами и их обработкой. Новый роутер предоставляет возможность создавать пользовательские маршруты, упрощает поддержку ЧПУ (человеко-понятные URL) и облегчает интеграцию с внешними сервисами. Вот основные принципы работы нового роутера в Bitrix24:

### Основные концепции

1. **Маршруты (Routes):**
   Маршруты определяют, какие URL-адреса будут обрабатываться, и связывают их с соответствующими обработчиками (контроллерами). Маршруты могут содержать динамические параметры.

2. **Контроллеры (Controllers):**
   Контроллеры - это классы или методы, которые обрабатывают запросы, поступающие на определенные маршруты.

3. **Диспетчер (Dispatcher):**
   Диспетчер отвечает за сопоставление входящих запросов с определенными маршрутами и передачу их соответствующим контроллерам.

### Настройка маршрутов

Маршруты определяются в конфигурационных файлах. Обычно это файл `routes.php` в корневом каталоге модуля или компонента. Пример файла маршрутов:

```php
<?php
use Bitrix\Main\Routing\RoutingConfigurator;

return function (RoutingConfigurator $routes) {
    $routes->get('/my/custom/route/', 'MyModule\Controller\CustomController@myMethod')
        ->name('custom.route');
};
```

В этом примере определен маршрут, который обрабатывает GET-запросы к URL `/my/custom/route/` и передает их методу `myMethod` класса `CustomController`.

### Контроллеры

Контроллеры - это классы, которые содержат методы для обработки запросов. Пример контроллера:

```php
namespace MyModule\Controller;

use Bitrix\Main\Engine\Controller;

class CustomController extends Controller
{
    public function myMethodAction()
    {
        // Логика обработки запроса
        return [
            'status' => 'success',
            'message' => 'Hello, world!',
        ];
    }
}
```

В этом примере метод `myMethodAction` обрабатывает запрос и возвращает ответ в виде массива данных.

### Обработка запросов

Когда запрос поступает на сервер, роутер сопоставляет URL-адрес с определенными маршрутами. Если маршрут найден, запрос передается соответствующему контроллеру и методу.

### Пример использования

1. **Определение маршрута:**

   Создайте файл `routes.php` в корневом каталоге модуля или компонента:

    ```php
    use Bitrix\Main\Routing\RoutingConfigurator;

    return function (RoutingConfigurator $routes) {
        $routes->get('/my/custom/route/', 'MyModule\Controller\CustomController@myMethod')
            ->name('custom.route');
    };
    ```

2. **Создание контроллера:**

   Создайте контроллер в каталоге `lib/Controller/CustomController.php`:

    ```php
    namespace MyModule\Controller;

    use Bitrix\Main\Engine\Controller;

    class CustomController extends Controller
    {
        public function myMethodAction()
        {
            // Логика обработки запроса
            return [
                'status' => 'success',
                'message' => 'Hello, world!',
            ];
        }
    }
    ```

### Динамические параметры

Маршруты могут содержать динамические параметры, которые передаются в контроллер. Например:

```php
use Bitrix\Main\Routing\RoutingConfigurator;

return function (RoutingConfigurator $routes) {
    $routes->get('/product/{id}/', 'MyModule\Controller\ProductController@show')
        ->name('product.show');
};
```

В этом примере `{id}` - это динамический параметр, который будет передан в метод `showAction` контроллера `ProductController`:

```php
namespace MyModule\Controller;

use Bitrix\Main\Engine\Controller;

class ProductController extends Controller
{
    public function showAction($id)
    {
        // Логика обработки запроса с использованием $id
        return [
            'status' => 'success',
            'product_id' => $id,
        ];
    }
}
```

### Заключение

Новый роутер в Bitrix24 Framework предоставляет удобные инструменты для управления URL-адресами и обработки запросов. Он улучшает организацию кода, упрощает поддержку и расширяемость приложений на базе Bitrix24.