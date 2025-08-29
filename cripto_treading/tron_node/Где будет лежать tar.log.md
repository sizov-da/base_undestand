<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

```sh
docker exec tron-node grep -i -E "(error|exception|failed|stuck)" /java-tron/logs/tron.log | tail -20
```
ошибка по конкретному блоку 
```shell
docker exec tron-node grep -A5 -B5 "74600964" /java-tron/logs/tron.log | grep -i -E "(error|exception|failed|invalid)"
```
### Где будет лежать tar.log

Если вы запускаете команду распаковки с помощью `nohup` и перенаправляете вывод в файл `tar.log`, например:

```bash
nohup tar -xzf ./node-snapshot/FullNode_output-directory.tgz -C ./nods/data/tron/ --strip-components=1 > tar.log 2>&1 &
```

то файл `tar.log` будет создан в **текущей рабочей директории**, из которой вы запускали команду.
Например, если вы находитесь в `/home/node/node-api`, то после выполнения команды файл появится по пути:

```
/home/node/node-api/tar.log
```


#### Как проверить

- Выполните команду `pwd` перед запуском — это и будет директория, где появится `tar.log`.
- Если хотите явно указать путь, используйте абсолютный путь, например:

```bash
nohup tar -xzf ... > /home/node/node-api/logs/tar.log 2>&1 &
```


#### Итог

- По умолчанию: файл будет лежать там, где вы запускали команду.
- Можно задать любой путь к файлу, указав его явно при запуске.

<div style="text-align: center">⁂</div>

[^1]: docker-compose.yml

