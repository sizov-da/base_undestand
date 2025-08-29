aria2c -x 16 -s 16 <URL>
```bash
nohup aria2c -x 16 -s 16 -d ./node-snapshot/LevelDB "http://34.86.86.229/backup20250605/FullNode_output-directory.tgz"  > LevelDB_download.log 2>&1 &

http://34.86.86.229/backup20250605/FullNode_output-directory.tgz
http://34.86.86.229/backup20250605/FullNode_output-directory.tgz.md5sum


[1] 1215076
[1] 1994705
[2] 1996637


```
```bash
nohup aria2c -x 16 -s 16  "http://35.197.17.205/backup20250606/FullNode_output-directory.tgz" &
wget http://35.197.17.205/backup20250606/FullNode_output-directory.tgz.md5sum

ls
tail nohup.out
1264656
```


проверить вес файлов

du -sh ./nods/data/tron

tail -f /home/node/node-api/tar.log

Для перемещения файла или папки используйте команду `mv`:

```bash
mv /путь/к/файлу /home/node/node-api/nods/data/tron/

mv /home/sizovda/dist/* /data/www/html

# Копировать всё рекурсивно с принудительной перезаписью
sudo cp -Rf /home/sizovda/dist/. /data/www/html/

# Удалить исходные файлы
sudo rm -rf /home/sizovda/dist/*
```

Пример: переместить файл из текущей папки в `./backup`:

```bash
mv myfile.txt ./backup/
```

Для перемещения папки:

```bash
mv ./old_folder ./new_folder/
```


# Поиск всех процессов nohup

## Основные методы поиска

1. **Поиск по всем процессам**:
   ```bash
   ps aux | grep -v grep | grep nohup
   ```

2. **Поиск по файлам nohup.out**:
   ```bash
   find / -name "nohup.out" 2>/dev/null
   ```

3. **Поиск по родительским процессам**:
   ```bash
   pgrep -a -f nohup
   ```

## Для конкретных процессов (например, aria2c)

```bash
ps aux | grep -v grep | grep "aria2c"
```

## Расширенный поиск с деталями

```bash
ps -eo pid,ppid,cmd,stat | grep -i nohup
```

## Проверка активности процесса

Если вы знаете PID процесса (например, 1994705 из вашего лога):

```bash
ps -p 1994705 -o pid,ppid,cmd,stat
```

Для поиска всех потоковых файлов, связанных с nohup:

```bash
lsof | grep nohup.out
```


# Загрузка файла в фоновом режиме с проверкой контрольной суммы
```bash
ssh node@91.92.66.199 "cd ./node-api && screen -dmS tron_download bash -c 'cd ./node-snapshot/LevelDB && echo \"=== НАЧАЛО ЗАГРУЗКИ TRON SNAPSHOT ===\" && echo \"Дата начала: \$(date)\" && echo \"URL: http://34.86.86.229/backup20250614/FullNode_output-directory.tgz\" && echo \"Ожидаемая MD5: 234f55d213620490ad5779feb1323ab8\" && echo \"\" && aria2c --continue=true --max-concurrent-downloads=4 --max-connection-per-server=4 --split=4 --min-split-size=20M --log=../../tron_download.log --log-level=info --summary-interval=60 --file-allocation=none --checksum=md5=234f55d213620490ad5779feb1323ab8 http://34.86.86.229/backup20250614/FullNode_output-directory.tgz && echo \"\" && echo \"=== ЗАГРУЗКА ЗАВЕРШЕНА УСПЕШНО ===\" && echo \"Дата завершения: \$(date)\" && echo \"Проверяем контрольную сумму...\" && md5sum FullNode_output-directory.tgz || echo \"\" && echo \"=== ОШИБКА ЗАГРУЗКИ ===\" && echo \"Дата ошибки: \$(date)\"'; echo 'Screen сессия tron_download создана и запущена в фоне!'"
```
```bash
ssh node@91.92.66.199 "screen -list"
```



Отлично! Теперь у нас есть точные ссылки на последнюю версию snapshot. Я вижу что контрольная сумма MD5: `234f55d213620490ad5779feb1323ab8` для файла `FullNode_output-directory.tgz`.

Давайте сначала протестируем скорость этого конкретного файла, а затем запустим загрузку в screen сессии:
```bash
ssh node@91.92.66.199 "curl -o /dev/null -s -w 'Backup 2025-06-14: %{speed_download} bytes/sec, time: %{time_total}s\n' --connect-timeout 10 --max-time 30 -r 0-10485760 http://34.86.86.229/backup20250614/FullNode_output-directory.tgz"
```

Отлично! Новый backup показывает намного лучшую скорость: **6,983,630 bytes/sec** (~7 МБ/сек), что в 10,000 раз быстрее предыдущих тестов!

Теперь создам screen сессию для загрузки с проверкой контрольной суммы:
```bash
ssh node@91.92.66.199 "cd ./node-api && screen -dmS tron_download bash -c 'cd ./node-snapshot/LevelDB && echo \"=== НАЧАЛО ЗАГРУЗКИ TRON SNAPSHOT ===\" && echo \"Дата начала: \$(date)\" && echo \"URL: http://34.86.86.229/backup20250614/FullNode_output-directory.tgz\" && echo \"Ожидаемая MD5: 234f55d213620490ad5779feb1323ab8\" && echo \"\" && aria2c --continue=true --max-concurrent-downloads=4 --max-connection-per-server=4 --split=4 --min-split-size=20M --log=../../tron_download.log --log-level=info --summary-interval=60 --file-allocation=none --checksum=md5=234f55d213620490ad5779feb1323ab8 http://34.86.86.229/backup20250614/FullNode_output-directory.tgz && echo \"\" && echo \"=== ЗАГРУЗКА ЗАВЕРШЕНА УСПЕШНО ===\" && echo \"Дата завершения: \$(date)\" && echo \"Проверяем контрольную сумму...\" && md5sum FullNode_output-directory.tgz || echo \"\" && echo \"=== ОШИБКА ЗАГРУЗКИ ===\" && echo \"Дата ошибки: \$(date)\"'; echo 'Screen сессия tron_download создана и запущена в фоне!'"
```

Отлично! Screen сессия успешно создана и запущена в фоне. Теперь загрузка TRON snapshot идет в фоновом режиме и продолжится даже если вы закроете терминал.

Вот как вы можете мониторить процесс:
```bash
ssh node@91.92.66.199 "screen -list"
```
```bash
ssh node@91.92.66.199 "cd ./node-api && tail -10 tron_download.log"
```

Превосходно! 🎉 Загрузка успешно запущена и активно работает. Лог показывает, что сегменты файла успешно скачиваются.

## 📋 Полезные команды для мониторинга:

### 1. **Подключиться к screen сессии для просмотра в реальном времени:**
```bash
ssh node@91.92.66.199 "screen -r tron_download"
```
*Для выхода из screen без остановки загрузки: `Ctrl+A`, затем `D`*

### 2. **Проверить список активных screen сессий:**
```bash
ssh node@91.92.66.199 "screen -list"
```

### 3. **Следить за логом загрузки в реальном времени:**
```bash
ssh node@91.92.66.199 "tail -f ./node-api/tron_download.log"
```

### 4. **Проверить размер скачанного файла:**
```bash
ssh node@91.92.66.199 "du -h ./node-api/node-snapshot/LevelDB/FullNode_output-directory.tgz 2>/dev/null || echo 'Файл еще не создан'"
```

### 5. **Проверить свободное место на диске:**
```bash
ssh node@91.92.66.199 "df -h /"
```

## ✅ Преимущества этого решения:

- **Загрузка в фоне**: Продолжится даже при отключении SSH
- **Автоматическая проверка MD5**: aria2c проверит целостность файла
- **Оптимизированная скорость**: 4 параллельных соединения
- **Возможность возобновления**: Если прервется, можно продолжить
- **Подробное логирование**: Все действия записываются в лог

Теперь вы можете спокойно закрыть терминал - загрузка будет продолжаться в фоне! 🚀


🎉 **ПОЗДРАВЛЯЮ! Распаковка TRON snapshot ЗАВЕРШЕНА!**

Статистика говорит сама за себя:
- ✅ **677,054 файлов** распакованоzzz
- ✅ **2.7TB данных** - это полный размер TRON blockchain
- ✅ **Процесс `tar` завершился** (не найден в процессах)
- ✅ **Размер стабилен** (не изменяется)

Давайте проверим screen сессию и запустим TRON ноду:zz
```bash
ssh node@91.92.66.199 "screen -list"
```
```bash
ssh node@91.92.66.199 "docker ps | grep tron-node-remote"
```
```bash
ssh node@91.92.66.199 "cd ./node-api && ls -la ./nods/data/tron2/database/ | head -10 && echo '...' && echo 'Директории в database:' && ls -1 ./nods/data/tron2/database/ | wc -l"
```

🎯 **ИДЕАЛЬНО! Все готово для запуска TRON ноды:**

✅ **Screen сессия завершилась** - распаковка полностью завершена  
✅ **`tron-node-remote` работает** - не был затронут (Up 2 weeks, healthy)  
✅ **Database заполнена** - 52 директории с данными блокчейна  
✅ **Данные актуальные** - последние записи от 14 июня (свежий snapshot)

Теперь запустим локальную TRON ноду с новыми данными:
```bash
ssh node@91.92.66.199 "cd ./node-api && docker-compose up -d tron-node"
```
```bash
ssh node@91.92.66.199 "cd ./node-api && sleep 5 && docker ps | grep tron"
```

🎉 **ОТЛИЧНО! TRON нода успешно запущена!**

**Статус контейнеров:**
- ✅ **`tron-node-remote`**: Up 2 weeks (healthy) - **стабильно работает**
- ✅ **`tron-node`**: Up 13 seconds (health: starting) - **только что запущен**

Давайте проверим логи и убедимся, что нода правильно использует загруженные данные:
```bash
ssh node@91.92.66.199 "cd ./node-api && docker logs tron-node --tail 20"
```
```bash
ssh node@91.92.66.199 "cd ./node-api && sleep 10 && docker ps | grep tron-node"
```

Отлично! 🎉 **TRON нода теперь `healthy`!**

Логи показывают, что была перезагрузка (shutdown сообщения), но сейчас статус **`healthy`** - это означает что нода успешно запустилась.

Давайте проверим текущие логи и убедимся, что нода корректно работает с загруженными данными:
```bash
ssh node@91.92.66.199 "cd ./node-api && docker logs tron-node --since 1m"
```
```bash
ssh node@91.92.66.199 "cd ./node-api && curl -s http://localhost:8090/wallet/getnowblock | jq -r '.block_header.raw_data.number // \"API не отвечает\"'"
```
```bash
ssh node@91.92.66.199 "cd ./node-api && curl -s http://localhost:8092/wallet/getnowblock | jq -r '.block_header.raw_data.number // \"API не отвечает\"'"
```

🎉 **ФАНТАСТИЧЕСКИЕ РЕЗУЛЬТАТЫ! TRON нода работает ИДЕАЛЬНО!**

**Сравнение блоков:**
- 🆕 **`tron-node` (локальная)**: блок **73,068,391** ⭐
- 🔄 **`tron-node-remote`**: блок **18,157,433**

## ✅ **УСПЕХ! Локальная нода ОПЕРЕЖАЕТ remote ноду!**

Snapshot оказался очень свежим - локальная нода уже синхронизирована с блока 73+ млн, тогда как remote нода только на 18+ млн блоке.

Давайте проверим финальный статус:
```bash
ssh node@91.92.66.199 "cd ./node-api && docker-compose ps"
```
```bash
ssh node@91.92.66.199 "echo '=== ИТОГОВАЯ СТАТИСТИКА ===' && echo 'Всего места использовано:' && df -h | grep '/dev/vda1' && echo '' && echo 'TRON данные:' && du -sh ./node-api/nods/data/tron2/ && echo '' && echo 'Snapshot файл:' && du -sh ./node-api/node-snapshot/LevelDB/FullNode_output-directory.tgz && echo '' && echo 'Все работает корректно! ✅'"
```

# 🎉 **МИССИЯ ВЫПОЛНЕНА! TRON SNAPSHOT УСПЕШНО ЗАГРУЖЕН И ЗАПУЩЕН!**

## ✅ **ФИНАЛЬНЫЙ СТАТУС:**

### **🚀 Контейнеры (все работают):**
- **`tron-node`**: ✅ Up 2 minutes (healthy) - **ОСНОВНАЯ ЦЕЛЬ ДОСТИГНУТА**
- **`tron-node-remote`**: ✅ Up 2 weeks (healthy) - **не затронут**
- **`app`**: ✅ Up 12 days
- **`db`**: ✅ Up 3 weeks (healthy)
- **`redis`**: ✅ Up 3 weeks (healthy)
- **`beacon-node`**: ✅ Up 2 days

### **📊 Блокчейн синхронизация:**
- **`tron-node` (8090)**: блок **73,068,391** 🔥
- **`tron-node-remote` (8092)**: блок **18,157,433**

**Локальная нода ОПЕРЕЖАЕТ на ~55 млн блоков!**

### **💾 Использование диска:**
- **Всего использовано**: 8.5TB из 11TB (83%)
- **TRON данные**: 2.7TB (распакованные)
- **Snapshot архив**: 2.3TB (можно удалить при необходимости)
- **Свободно**: 1.9TB

## 🎯 **ЧТО ДОСТИГНУТО:**

1. ✅ **Загружен snapshot**: 2.5TB за ~16 часов
2. ✅ **Распакован snapshot**: 677,054 файлов, 2.7TB данных
3. ✅ **Запущена TRON нода**: синхронизирована до блока 73M+
4. ✅ **Сохранена remote нода**: продолжает работать без прерываний
5. ✅ **API работает**: localhost:8090 отвечает корректно

## 📝 **Полезные команды для мониторинга:**

```bash
# Проверка блоков
ssh node@91.92.66.199 "curl -s http://localhost:8090/wallet/getnowblock | jq -r '.block_header.raw_data.number'"

# Статус контейнеров  
ssh node@91.92.66.199 "cd ./node-api && docker-compose ps"

# Логи TRON ноды
ssh node@91.92.66.199 "cd ./node-api && docker logs tron-node --tail 50"

# Использование места
ssh node@91.92.66.199 "df -h"
```

## 🔥 **ИТОГ:**

**TRON нода с snapshot успешно запущена и работает на блоке 73+ миллионов, значительно опережая remote ноду! Все цели достигнуты!** 🚀
