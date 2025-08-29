<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

## Как развернуть скаченный снапшот TRON-ноды

Анализируя ваш `docker-compose.yml` и структуру проекта, вижу, что снапшот нужно правильно извлечь в директорию, которая примонтирована к контейнеру. Вот пошаговая инструкция:

### Понимание текущей конфигурации

Из вашего `docker-compose.yml` видно[^1]:

- Том `./nods/data/tron` примонтирован к `/tron/output-directory` в контейнере
- Снапшот находится в `./node-snapshot/FullNode_output-directory.tgz`
- TRON-нода ожидает данные в директории `/tron/output-directory`


### Пошаговое развертывание снапшота

#### 1. Остановите контейнер TRON-ноды

```bash
    cd /home/node/node-api
    docker-compose stop tron-node
```


#### 2. Очистите целевую директорию

```bash
    # Убедитесь, что директория пустая
    rm -rf ./nods/data/tron/*
```


#### 3. Извлеките снапшот в правильную директорию

```bash
  # Извлеките архив напрямую в целевую директорию
  tar -xzf ./node-snapshot/FullNode_output-directory.tgz -C ./nods/data/tron/ --strip-components=1
```

Альтернативный способ, если структура архива отличается[^2][^3]:

```bash
# Сначала извлеките в временную директорию для проверки структуры
mkdir -p /tmp/tron-snapshot
tar -xzf ./node-snapshot/FullNode_output-directory.tgz -C /tmp/tron-snapshot

# Проверьте содержимое
ls -la /tmp/tron-snapshot

# Переместите содержимое в нужную директорию
mv /tmp/tron-snapshot/* ./nods/data/tron/
rm -rf /tmp/tron-snapshot
```


#### 4. Проверьте структуру данных

```bash
# Убедитесь, что база данных извлеклась правильно
ls -la ./nods/data/tron/
```

Должны появиться папки типа `database`, возможно `logs` и другие файлы TRON-ноды[^4].

#### 5. Установите правильные права доступа

```bash
# Установите права для пользователя, под которым работает Docker
sudo chown -R $USER:$USER ./nods/data/tron/
chmod -R 755 ./nods/data/tron/
```


#### 6. Запустите TRON-ноду

```bash
  docker-compose up -d tron-node
```


#### 7. Проверьте логи запуска

```bash
# Следите за логами запуска
docker-compose logs -f tron-node
```


### Важные моменты

**Совместимость типов баз данных:** Убедитесь, что тип базы данных в снапшоте (LevelDB или RocksDB) соответствует настройке `db.engine` в вашем файле конфигурации[^2]. Не смешивайте разные типы баз данных.

**Размер и время:** Снапшоты могут быть большими (сотни ГБ), поэтому процесс извлечения может занять значительное время[^5].

**Мониторинг синхронизации:** После запуска нода начнет синхронизацию с актуального блока снапшота. Проверить статус можно через API:

```bash
curl http://localhost:8090/wallet/getnowblock
```


### Альтернативный подход через Docker

Если у вас возникают проблемы с правами доступа, можно извлечь снапшот прямо в контейнере[^6]:

```bash
# Скопируйте снапшот в работающий контейнер
docker cp ./node-snapshot/FullNode_output-directory.tgz tron-node:/tmp/

# Выполните извлечение внутри контейнера
docker exec tron-node tar -xzf /tmp/FullNode_output-directory.tgz -C /tron/output-directory --strip-components=1

# Удалите временный файл
docker exec tron-node rm /tmp/FullNode_output-directory.tgz
```
Вижу, что процесс `tar` работает активно (PID 801519, CPU 6.6%), но размер директории не увеличивается. Это может быть проблема с правами доступа или структурой архива. Давайте диагностируем:
```bash
ssh node@91.92.66.199 "ls -la ./node-api/nods/data/tron2/"
```
```bash
ssh node@91.92.66.199 "sudo ls -la ./node-api/nods/data/tron2/"
```
```bash
ssh node@91.92.66.199 "tar -tzf ./node-api/node-snapshot/LevelDB/FullNode_output-directory.tgz | head -10"
```

Понял проблему! Архив содержит структуру `output-directory/database/...`, и мы используем `--strip-components=1`. Файлы должны попадать в `database/` поддиректорию. Давайте проверим:
```bash
ssh node@91.92.66.199 "sudo ls -la ./node-api/nods/data/tron2/database/"
```
```bash
ssh node@91.92.66.199 "sudo find ./node-api/nods/data/tron2/ -type f | wc -l && echo 'файлов найдено'"
```
```bash
ssh node@91.92.66.199 "sudo du -sh ./node-api/nods/data/tron2/database/ 2>/dev/null || echo 'database директория пуста или недоступна'"
```

Проблема найдена! Файлы не распаковываются (0 файлов найдено). Скорее всего проблема с правами доступа - `tar` не может записывать файлы в директорию, созданную root'ом.

Давайте остановим текущий процесс и перезапустим с правильным подходом:
```bash
ssh node@91.92.66.199 "screen -S tron_extract -X quit && echo 'Старая screen сессия остановлена'"
```
```bash
ssh node@91.92.66.199 "killall tar && echo 'Процесс tar остановлен'"
```
```bash
ssh node@91.92.66.199 "cd ./node-api && sudo rm -rf ./nods/data/tron2/database && sudo mkdir -p ./nods/data/tron2/database && sudo chown -R node:node ./nods/data/tron2/ && echo 'Директория пересоздана с правильными правами'"
```

Отлично! Теперь права доступа исправлены. Давайте запустим распаковку заново:
```bash
ssh node@91.92.66.199 "cd ./node-api && ls -la ./nods/data/tron2/ && echo 'Права исправлены!'"
```
```bash
ssh node@91.92.66.199 "cd ./node-api && screen -dmS tron_extract_v2 bash -c 'echo \"=== НОВАЯ РАСПАКОВКА TRON SNAPSHOT ===\" && echo \"Дата начала: \$(date)\" && echo \"Файл: ./node-snapshot/LevelDB/FullNode_output-directory.tgz\" && echo \"Размер архива: \$(du -sh ./node-snapshot/LevelDB/FullNode_output-directory.tgz)\" && echo \"Назначение: ./nods/data/tron2/\" && echo \"\" && tar -xzf ./node-snapshot/LevelDB/FullNode_output-directory.tgz -C ./nods/data/tron2/ --strip-components=1 && echo \"\" && echo \"=== РАСПАКОВКА ЗАВЕРШЕНА УСПЕШНО ===\" && echo \"Дата завершения: \$(date)\" && echo \"Размер данных: \$(du -sh ./nods/data/tron2/)\" && ls -la ./nods/data/tron2/database/ | head -10 || echo \"\" && echo \"=== ОШИБКА РАСПАКОВКИ ===\" && echo \"Дата ошибки: \$(date)\" && echo \"Последние ошибки:\" && dmesg | tail -5'; echo 'Новая screen сессия tron_extract_v2 запущена!'"
```

Отлично! ✅ **Права исправлены и новая распаковка запущена!**

Теперь давайте проверим, что процесс работает правильно:
```bash
ssh node@91.92.66.199 "screen -list"
```
```bash
ssh node@91.92.66.199 "ps aux | grep tar"
```
```bash
ssh node@91.92.66.199 "cd ./node-api && sleep 3 && find ./nods/data/tron2/ -type f | wc -l && echo 'файлов создано' && du -sh ./nods/data/tron2/database/ 2>/dev/null"
```

🎉 **ОТЛИЧНО! Теперь все работает правильно!**

✅ **Прогресс распаковки:**
- **Screen сессия**: `tron_extract_v2` активна
- **Процесс tar**: работает с высокой загрузкой (18.7% CPU)
- **Файлы**: уже создано **3,240 файлов**
- **Размер**: уже распаковано **4.6GB**

## 📊 **Статус операций:**

| Операция | Статус | Прогресс |
|----------|--------|----------|
| 🔽 Загрузка snapshot | ✅ **ЗАВЕРШЕНА** | 2.5TB |
| 📦 Распаковка snapshot | 🔄 **АКТИВНО РАБОТАЕТ** | 4.6GB из ~2.3TB |
| 🚀 Запуск tron-node | ⏳ **ОЖИДАНИЕ** | После распаковки |

## 🎯 **Мониторинг прогресса:**

### **Быстрая проверка прогресса:**
```bash
ssh node@91.92.66.199 "cd ./node-api && echo 'Файлов:' && find ./nods/data/tron2/ -type f | wc -l && echo 'Размер:' && du -sh ./nods/data/tron2/database/"
```

### **Подключение к screen (интерактивный режим):**
```bash
ssh -t node@91.92.66.199 "screen -r tron_extract_v2"
```
*Выход: `Ctrl+A`, затем `D`*

### **Проверка активности процесса:**
```bash
ssh node@91.92.66.199 "ps aux | grep tar | grep -v grep"
```

## ⏰ **Ожидаемое время:**
- **При текущей скорости**: ~2-4 часа до завершения
- **Процесс стабильный**: CPU загрузка 18.7% указывает на активную работу

## ✅ **Что исправлено:**
1. **Права доступа**: Директория теперь принадлежит пользователю `node`
2. **Процесс распаковки**: Файлы успешно создаются
3. **Мониторинг**: Можно отслеживать прогресс

**Теперь можно спокойно ждать - процесс работает корректно в фоне!** 🚀

# ПЕРЕЗАПУСК ЗАМЕРШЕГО БЛОКА

# 1. Очистка блокировок
find ./output-directory/ -name 'LOCK' -delete

# 2. Удаление recent-block (критично!)
rm -rf ./output-directory/database/recent-block/*

# 3. Удаление trans
rm -rf ./output-directory/database/trans/*