

# тест здоровья ноды

создайте файл `test_health.py` в папке `tron_node` и вставьте в него следующий код:
```python
import requests

def get_tron_block_height(node_url):
    resp = requests.post(f"{node_url}/wallet/getnowblock")
    resp.raise_for_status()
    return resp.json()["block_header"]["raw_data"]["number"]

def get_reference_block_height():
    # Используем публичный API tronscan для получения актуальной высоты блока
    resp = requests.get("https://apilist.tronscanapi.com/api/block/latest")
    resp.raise_for_status()
    return resp.json()["number"]

address = "TQi6duE6msVsWnMRovcK8YcMD1pd3auDCj"
node_url = "http://localhost:8090"

# Баланс
payload = {"address": address}
response = requests.post(f"{node_url}/wallet/getaccount", json=payload)
data = response.json()
balance_sun = data.get("balance", 0)
balance_trx = balance_sun / 1_000_000
print(f"Баланс: {balance_trx} TRX")

# Синхронизация
local_height = get_tron_block_height(node_url)
ref_height = get_reference_block_height()
sync_percent = min(local_height / ref_height * 100, 100)
print(f"Синхронизация: {sync_percent:.2f}% (локальный блок: {local_height}, эталон: {ref_height})")
```

### Запуск теста
1. Убедитесь, что ваша TRON-нода запущена и доступна по адресу `http://localhost:8090`.
2. Запустите тестовый скрипт:
   ```bash
   python tron_node/test_health.py
   ```
### Ожидаемый вывод
```plaintext
Баланс: 1000.00 TRX
Синхронизация: 99.85% (локальный блок: 12345678, эталон: 12345679)
``` 



ssh -o ConnectTimeout=10 -o StrictHostKeyChecking=no root@91.92.66.199 "docker run -d --name tron-node-debug \
--restart=unless-stopped \
-p 8090:8090 -p 18888:18888 -p 50051:50051 \
-v tron-data:/java-tron/output-directory \
-e JAVA_OPTS='-Xmx6g -Xms2g -XX:+UseG1GC -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/java-tron/logs/ -XX:+PrintGCDetails -XX:+PrintGCTimeStamps -Dlogging.level.root=DEBUG -Dlogging.level.org.tron=DEBUG -Djava.net.preferIPv4Stack=true' \
tronprotocol/java-tron:latest"


ssh -o ConnectTimeout=10 -o StrictHostKeyChecking=no root@91.92.66.199 "docker run -d --name tron-node \
--restart=unless-stopped \
-p 8090:8090 -p 18888:18888 -p 50051:50051 \
-v tron-data:/java-tron/output-directory \
-e JAVA_OPTS='-Xmx6g -Xms2g -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/java-tron/logs/ -Dlogging.level.root=DEBUG -Dlogging.level.org.tron=DEBUG -Djava.net.preferIPv4Stack=true' \
tronprotocol/java-tron:latest"