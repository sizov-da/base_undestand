DB::connection('mongodb')->collection('test')->insert(['name' => 'Test MongoDB']);
Вы получаете ошибку, потому что Laravel по умолчанию использует SQLite, а вам нужно настроить **MongoDB**. Давайте исправим это. 🚀

---

## 🔹 **1️⃣ Настроим `.env` для MongoDB**
Откройте `.env` и измените конфигурацию базы данных:

```ini
DB_CONNECTION=mongodb
DB_HOST=mongo
DB_PORT=27017
DB_DATABASE=dev-api-con
DB_USERNAME=
DB_PASSWORD=
```

Если у вас есть логин и пароль для MongoDB, укажите их. В противном случае оставьте пустыми.

**Пример с авторизацией:**
```ini
DB_USERNAME=admin
DB_PASSWORD=secret
```

---

## 🔹 **2️⃣ Очистим кеш**
После изменения `.env` выполните:

```bash
php artisan config:clear && php artisan cache:clear
```

---

## 🔹 **3️⃣ Установим пакет MongoDB для Laravel**
Если у вас еще не установлен драйвер MongoDB, установите его:

```bash
composer require mongodb/mongodb
composer require jenssegers/mongodb
```

---

## 🔹 **4️⃣ Обновим конфигурацию базы данных в `config/database.php`**
Откройте `config/database.php` и найдите секцию `'connections'`. Добавьте туда поддержку MongoDB:

```php
'connections' => [

    'mongodb' => [
        'driver'   => 'mongodb',
        'host'     => env('DB_HOST', '127.0.0.1'),
        'port'     => env('DB_PORT', 27017),
        'database' => env('DB_DATABASE'),
        'username' => env('DB_USERNAME'),
        'password' => env('DB_PASSWORD'),
        'options'  => [
            'database' => 'admin' // Используется для аутентификации
        ]
    ],

    // Оставьте другие подключения без изменений...
]
```

---

## 🔹 **5️⃣ Запустите миграции в MongoDB**
Теперь попробуйте:

```bash
php artisan migrate
```


Если выводит `[{ "_id": ObjectId(...), "name": "Test MongoDB" }]`, то MongoDB подключен! 🎉

Ваша ошибка говорит о том, что Laravel по-прежнему использует **SQLite**, а не MongoDB. Давайте разберем это шаг за шагом.

---

### 🔹 **1️⃣ Проверьте текущий драйвер базы данных**
Выполните в `php artisan tinker`:

```php
DB::connection()->getDriverName();
```

**Ожидаемый результат для MongoDB**:
```php
"mongodb"
```

Если Laravel продолжает использовать `sqlite`, значит он **не видит изменения в `.env`**.

---

### 🔹 **2️⃣ Убедитесь, что `.env` настроен правильно**
Откройте `.env` и убедитесь, что он содержит:

```ini
DB_CONNECTION=mongodb
DB_DSN=mongodb://mongo:27017
DB_DATABASE=dev-api-con
DB_USERNAME=
DB_PASSWORD=
```

Если у вас есть имя пользователя и пароль, укажите их.

---

### 🔹 **3️⃣ Проверьте `config/database.php`**
Откройте `config/database.php` и убедитесь, что в `connections` есть **MongoDB**:

```php
'connections' => [
    'mongodb' => [
        'driver'   => 'mongodb',
        'dsn'      => env('DB_DSN', 'mongodb://mongo:27017'),
        'database' => env('DB_DATABASE', 'dev-api-con'),
        'options'  => [
            'username' => env('DB_USERNAME', null),
            'password' => env('DB_PASSWORD', null),
            'authSource' => 'admin'
        ]
    ],
    
    'sqlite' => [
        'driver' => 'sqlite',
        'database' => database_path('database.sqlite'),
        'prefix' => '',
    ],
    
    // Остальные подключения...
],
```

---

### 🔹 **4️⃣ Очистите кеш Laravel**
После изменения `.env` и `config/database.php` выполните:

```bash
php artisan config:clear
php artisan cache:clear
php artisan config:cache
```

Затем проверьте снова:
```php
DB::connection()->getDriverName();
```
Если все правильно, Laravel должен теперь использовать **MongoDB**.

---

### 🔹 **5️⃣ Попробуйте вставку снова**
```php
DB::connection('mongodb')->table('test')->insert(['name' => 'Test MongoDB']);
DB::connection('mongodb')->table('test')->get();
```

Если в консоли `php artisan tinker` теперь работает — значит, проблема была в конфигурации. 🚀