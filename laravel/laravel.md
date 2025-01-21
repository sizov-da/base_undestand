



# Запуск агрегаций в ручном режиме


Если пользуетесь докерам войдите в контейнер
```shell
docker exec -it php_app /bin/bash
```

Запуск из рабочего каталога laravel

```shell
php artisan queue:work --verbose
php artisan mongo:run-aggregation
php artisan mongo:run-mongo-aggregation-dogovor-proiz
```