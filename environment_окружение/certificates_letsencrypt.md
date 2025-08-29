

        certbot certonly
        
80 port available open

        systemctl stop nginx


choose one when asked
    
        1
Для поиска всех правил для домена dev-api.pl.eksmo.net в конфигурации nginx можно использовать следующие команды:

1. Поиск по всем файлам конфигурации nginx:
```bash
grep -r "dev-api.pl.eksmo.net" /etc/nginx/
```

2. Более точный поиск с отображением только имён файлов, содержащих этот домен:
```bash
find /etc/nginx/ -type f -name "*.conf" -o -name "*" | xargs grep -l "dev-api.pl.eksmo.net"
```

3. Просмотр полной конфигурации виртуального хоста с этим доменом:
```bash
sudo nginx -T | grep -A 10 "server_name.*dev-api.pl.eksmo.net"
```

4. Если нужно найти блок server целиком:
```bash
sudo nginx -T | sed -n '/server {/,/}/p' | grep -B 5 -A 20 "dev-api.pl.eksmo.net"
```

Обычно правила для домена находятся в директориях:
- `/etc/nginx/sites-available/`
- `/etc/nginx/sites-enabled/`
- `/etc/nginx/conf.d/`