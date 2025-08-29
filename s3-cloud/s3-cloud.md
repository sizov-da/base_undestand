# Утилиты для подключения к S3

Вот основные инструменты для работы с Amazon S3:

1. **AWS CLI** - официальная утилита командной строки от Amazon
2. **s3cmd** - популярный инструмент командной строки с расширенными функциями
3. **mc (MinIO Client)** - утилита для работы с S3-совместимыми хранилищами
4. **rclone** - утилита для синхронизации файлов между локальной системой и S3
5. **AWS SDK** - библиотеки для работы с S3 из кода (Python, Java, Node.js и др.)
6. **CyberDuck** - графический клиент для macOS
7. **S3Browser** - графический интерфейс для Windows
8. **s3fs/Goofys** - утилиты для монтирования S3 бакетов как файловых систем

Большинство этих инструментов требуют настройки AWS-учетных данных (Access Key и Secret Key) для подключения.

```shell
s3cmd ls s3://contractors-y4rgckvx
```

# Основные команды s3cmd для работы с S3

Помимо команды `ls`, вот наиболее полезные команды s3cmd:

## Базовые операции с файлами
- **put** - загрузка файлов в S3
  ```shell
  s3cmd put file.txt s3://contractors-y4rgckvx/
  ```

- **get** - скачивание файлов
  ```shell
  s3cmd get s3://contractors-y4rgckvx/file.txt
  ```

- **del** - удаление файлов
  ```shell
  s3cmd del s3://contractors-y4rgckvx/file.txt
  ```

## Операции с бакетами
- **mb** - создание бакета
  ```shell
  s3cmd mb s3://новый-бакет
  ```

- **rb** - удаление бакета
  ```shell
  s3cmd rb s3://ненужный-бакет
  ```

## Дополнительные команды
- **sync** - синхронизация папок
  ```shell
  s3cmd sync локальная_папка/ s3://contractors-y4rgckvx/
  ```

- **info** - получение информации об объекте
  ```shell
  s3cmd info s3://contractors-y4rgckvx/file.txt
  ```

- **cp/mv** - копирование/перемещение файлов внутри S3
  ```shell
  s3cmd cp s3://contractors-y4rgckvx/file.txt s3://contractors-y4rgckvx/папка/
  ```

- **setacl** - установка прав доступа
  ```shell
  s3cmd setacl --acl-public s3://contractors-y4rgckvx/file.txt
  ```
  


