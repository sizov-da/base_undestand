



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


Сгенерируй Swagger документацию:

```shell
php artisan l5-swagger:generate
```
Открой Swagger UI:
```shell
http://localhost:8000/api/documentation
```

Запросы в базу 

```shell
php artisan tinker
DB::connection('mongodb')->collection('dogovor_proiz')->find()
```



# Генерация документации
    php artisan l5-swagger:generate
# роуты





```shell
php artisan route:list
```