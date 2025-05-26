
Эта архитектура известна под несколькими названиями:

# Чистая архитектура (Clean Architecture)

## Другие названия этой же архитектуры:

1. **Гексагональная архитектура** (Hexagonal Architecture)
    - Также известна как "Архитектура портов и адаптеров"

2. **Луковая архитектура** (Onion Architecture)
    - Акцентирует внимание на слоях, как в луковице

3. **DDD-архитектура** (Domain-Driven Design)
    - Принципы очень похожи


## Основные принципы:

1. Независимость от фреймворков
2. Тестируемость
3. Независимость от UI
4. Независимость от базы данных
5. Независимость от внешних сервисов

## Ключевые слои (изнутри наружу):
1. `Domain` (Ядро) - самый внутренний слой
2. `Application` (Прикладной слой)
3. `Infrastructure` (Инфраструктурный слой)
4. `UI` (Интерфейсный слой) - самый внешний слой

## Правило зависимостей:
- Зависимости всегда направлены внутрь
- Внутренние слои не знают о существовании внешних
- Внешние слои зависят от внутренних



# Детальная структура слоев

## 1. Domain/
```
Domain/
├── Entities/           # Доменные сущности
│   ├── Stage.php
│   └── ValueObjects/   # Объекты-значения
├── Events/            # Доменные события
│   ├── StageCreated.php
│   └── StageUpdated.php
├── Exceptions/        # Доменные исключения
│   └── StageException.php
├── Repositories/      # Интерфейсы репозиториев
│   └── StageRepositoryInterface.php
└── Services/         # Доменные сервисы
    └── StageService.php
```

## 2. Application/
```
Application/
├── Commands/          # Команды и их обработчики
│   ├── CreateStage/
│   │   ├── CreateStageCommand.php
│   │   └── CreateStageHandler.php
│   └── UpdateStage/
├── DTO/             # Объекты передачи данных
│   ├── StageDTO.php
│   └── Requests/
├── Events/           # Обработчики событий приложения
│   └── Handlers/
├── Exceptions/       # Исключения уровня приложения
│   └── StageApplicationException.php
├── Queries/          # Запросы и их обработчики
│   ├── GetStage/
│   │   ├── GetStageQuery.php
│   │   └── GetStageHandler.php
│   └── ListStages/
├── Services/         # Сервисы приложения
│   └── StageApplicationService.php
└── Validators/       # Валидаторы
    └── StageValidator.php
```

## 3. Infrastructure/
```
Infrastructure/
├── Database/         # Работа с базой данных
│   ├── Migrations/
│   ├── Seeders/
│   └── Models/      # Eloquent модели
├── Repositories/    # Реализации репозиториев
│   └── StageRepository.php
├── Services/       # Реализации внешних сервисов
│   ├── Cache/
│   └── Queue/
└── Providers/      # Service Providers
    └── StageServiceProvider.php
```

## 4. Ui/
```
Ui/
├── API/            # API endpoints
│   ├── Controllers/
│   │   └── StageController.php
│   ├── Requests/   # Form Requests
│   │   └── StageRequest.php
│   ├── Resources/  # API Resources
│   │   └── StageResource.php
│   └── Routes/
│       └── api.php
├── Console/        # Консольные команды
│   └── Commands/
├── Http/          # Web интерфейс
│   ├── Controllers/
│   ├── Middleware/
│   └── Views/
└── Responses/     # Форматтеры ответов
    └── ApiResponse.php
```

## Рекомендации по использованию:

### Domain/
- `Entities/`: Содержит основные бизнес-модели
    - Используйте Value Objects для инкапсуляции атрибутов
    - Реализуйте бизнес-правила внутри сущностей
- `Events/`: События, возникающие в домене
    - Именуйте в прошедшем времени (Created, Updated, etc.)
- `Repositories/`: Только интерфейсы, без реализации
    - Определяйте методы для работы с коллекциями сущностей

### Application/
- `Commands/`: Один обработчик для одной команды
    - Используйте DTO для передачи данных
    - Валидируйте входные данные
- `Queries/`: Отделяйте чтение от записи (CQRS)
    - Оптимизируйте запросы для чтения
- `Services/`: Оркестрация бизнес-процессов
    - Не дублируйте логику из доменного слоя

### Infrastructure/
- `Database/`:
    - Отделяйте Eloquent модели от доменных сущностей
    - Используйте фабрики для тестовых данных
- `Repositories/`:
    - Реализуйте маппинг между моделями и сущностями
    - Обрабатывайте исключения базы данных

### Ui/
- `API/`:
    - Используйте Resources для форматирования ответов
    - Валидируйте входные данные через Form Requests
- `Http/`:
    - Разделяйте логику API и web-интерфейса
    - Используйте middleware для общей функциональности

## Правила именования файлов:

1. **Сущности**: `{Name}.php`
2. **Интерфейсы**: `{Name}Interface.php`
3. **Репозитории**: `{Name}Repository.php`
4. **Команды**: `{Action}{Name}Command.php`
5. **Обработчики**: `{Action}{Name}Handler.php`
6. **DTO**: `{Name}DTO.php`
7. **Контроллеры**: `{Name}Controller.php`

### Именование доп правила
```
Сущности: существительные (Stage, User)
Сервисы: существительные (StageService)
Команды: глаголы (CreateStage, UpdateStage)
События: прошедшее время (StageCreated, StageUpdated)
```




Эта структура обеспечивает:
- Чёткую организацию кода
- Легкий поиск нужных файлов
- Масштабируемость проекта
- Удобство поддержки
- Соответствие принципам SOLID
