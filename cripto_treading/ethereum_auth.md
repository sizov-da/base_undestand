Перезапустите Ethereum-узел с обновленной конфигурацией:

```bash
docker-compose restart eth-node
```
Проверьте доступность статических узлов:
```bash
python check_static_nodes.py

```
Запустите непрерывный мониторинг:

```bash
python monitor_eth_sync.py

```
Если синхронизация не начнется в течение 24 часов, рассмотрите возможность полной очистки данных и перезапуска:

```bash
docker-compose down
rm -rf ./data/ethereum/*
docker-compose up -d
```

# Ethereum JSON-RPC Authentication Setup

In this project, the Ethereum node (Nethermind) is configured with JWT authentication for the JSON-RPC API. There are two ways to work with this:

## Method 1: Disable Authentication (Less Secure)

If you want to disable authentication for local development, uncomment the following line in the `docker-compose.yaml` file in the `eth-node` section:

```yaml
- --JsonRpc.AdditionalRpcUrls=http://0.0.0.0:8551|Engine|no-auth,http://0.0.0.0:8545|Eth,Net,Web3,Engine|no-auth
```

Then restart the container:

```bash
docker-compose restart eth-node
```

After this, you can make requests without authentication:

```bash
curl -X POST -H "Content-Type: application/json" \
  --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}' \
  http://localhost:8545
```

## Method 2: Use JWT Authentication (More Secure)

To use JWT authentication, you can use one of the provided scripts:

### Python Script (requires PyJWT):

```bash
pip install pyjwt requests
python eth_rpc_auth.py eth_blockNumber
```

### Bash Script (requires jq and openssl):

```bash
chmod +x eth_rpc_auth.sh
./eth_rpc_auth.sh eth_blockNumber
```

Both scripts accept a JSON-RPC method and optional parameters:

```bash
python eth_rpc_auth.py eth_getBalance 0x742d35Cc6634C0532925a3b844Bc454e4438f44e latest
./eth_rpc_auth.sh eth_getBalance 0x742d35Cc6634C0532925a3b844Bc454e4438f44e latest
```

## Example Requests

```bash
# Get current block number
python eth_rpc_auth.py eth_blockNumber

# Get account balance
python eth_rpc_auth.py eth_getBalance 0x742d35Cc6634C0532925a3b844Bc454e4438f44e latest

# Get peer count
python eth_rpc_auth.py net_peerCount

# Get client version
python eth_rpc_auth.py web3_clientVersion
```

## Additional Information

The JWT secret file is located at `./jwtsecret/jwt.hex` and contains the following secret:

```
49885ce1a837d71c52be7faff0a952e1905b888092e0e7dbba73c639be6165d7
```

This secret is used to sign JWT tokens that are required for authentication with the Ethereum node's JSON-RPC API.

# Синхронизация Ethereum-узла
Чтобы проверить статус синхронизации ноды, можно использовать метод JSON-RPC `eth_syncing`. Вот пример команды:

```bash
python eth_rpc_auth.py eth_syncing
```

### Возможные результаты:
1. **Если нода синхронизируется**, вы получите объект с информацией о процессе синхронизации:
   ```json
   {
     "jsonrpc": "2.0",
     "id": 1,
     "result": {
       "startingBlock": "0x384",
       "currentBlock": "0x386",
       "highestBlock": "0x454A8C"
     }
   }
   ```
    - `startingBlock`: блок, с которого началась синхронизация.
    - `currentBlock`: текущий блок, до которого нода синхронизировалась.
    - `highestBlock`: последний доступный блок в сети.

2. **Если нода уже синхронизирована**, результат будет:
   ```json
   {
     "jsonrpc": "2.0",
     "id": 1,
     "result": false
   }
   ```

Это означает, что нода полностью синхронизирована.


# Сздание аккаунта Ethereum


```bash
# Создание нового аккаунта Ethereum с использованием метода personal_newAccount
python eth_rpc_auth.py personal_newAccount "ajklhsaduiorewp34170JsdfaLD"

```
Вижу, что теперь ваш запрос успешно выполнился и вы создали аккаунт Ethereum! В ответе от сервера вы получили адрес нового созданного кошелька:

```
0x49388ffa41cec4fd57d76eb1f4b8ad7c50cee0b8
```
ответ от сервера выглядит примерно так:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x49388ffa41cec4fd57d76eb1f4b8ad7c50cee0b8"
}
```

Это означает, что модуль `personal` был успешно активирован на вашем удаленном сервере, и метод `personal_newAccount` теперь работает корректно.

Ключевые моменты:
1. Аккаунт был создан на удаленном сервере (91.92.66.199)
2. Пароль для защиты приватного ключа успешно установлен
3. Возвращенный адрес `0x49388ffa41cec4fd57d76eb1f4b8ad7c50cee0b8` - это публичный адрес вашего нового кошелька

Файлы ключей для этого кошелька будут сохранены на удаленном сервере в директории `/nethermind/keystore`, которую вы монтируете в контейнер в вашем `docker-compose.yaml`.

Теперь вы можете использовать этот адрес для отправки и получения транзакций, а также обращаться к нему через другие RPC-методы, такие как `eth_getBalance`.

# Проверка аккаунта Ethereum и его баланса

```bash
# Проверка баланса созданного аккаунта
python eth_rpc_auth.py net_version
```
```json
{
  "jsonrpc": "2.0",
  "result": "100", // Версия сети (например, 100 для Ethereum 77 Testnet)
  "id": 1
}
```

#### Проверка баланса аккаунта Ethereum

```bash
python eth_rpc_auth.py eth_getBalance 0xDB6f479AFa71e7af872E4D09044D54F003C3148F latest
```
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": "0x0"
}
```

доступные токены
```bash
python eth_rpc_auth.py list_tokens 0xDB6f479AFa71e7af872E4D09044D54F003C3148F
```
```bash
Нативная валюта (xDAI): 0.0
ID сети: 100
Нода синхронизируется: {'startingBlock': '0x0', 'currentBlock': '0x26879a9', 'highestBlock': '0x26879a9'}

ERC-20 токены:
STAKE: 0.0
  Контракт STAKE существует
USDC: 0.0
  Контракт USDC существует
WETH: 0.0
  Контракт WETH существует
USDT: 0.0
  Контракт USDT существует
COW: 0.0
  Контракт COW существует

```

**Как использовать:**

После добавления этого кода вы сможете отправлять транзакции из командной строки:

*   **Для простого перевода ETH** (например, 0.1 ETH, где 1 ETH = 10^18 wei):
    ```bash
    python eth_rpc_auth.py send_tx YOUR_FROM_ADDRESS RECIPIENT_ADDRESS 100000000000000000
    ```

*   **Для вызова функции контракта** (отправляя 0 ETH, с указанием данных вызова):
    ```bash
    python eth_rpc_auth.py send_tx YOUR_FROM_ADDRESS CONTRACT_ADDRESS 0 0xdataForContractCall
    ```

*   **С указанием газа и цены газа:**
    ```bash
    python eth_rpc_auth.py send_tx YOUR_FROM_ADDRESS RECIPIENT_ADDRESS 100000000000000000 "0x" 21000 50000000000 
    ```
    (Здесь "0x" - это пустые данные, 21000 - лимит газа, 50 gwei - цена газа)

Убедитесь, что адрес отправителя (`YOUR_FROM_ADDRESS`) существует на узле, к которому вы подключаетесь, и что узел имеет возможность подписывать транзакции от имени этого адреса (например, аккаунт разблокирован). Если транзакция будет успешно отправлена в сеть, RPC-метод `eth_sendTransaction` вернет хеш транзакции.
Похоже, вы столкнулись с ошибкой, которая означает, что аккаунт, с которого вы пытаетесь отправить транзакцию (`0xDB6f479AFa71e7af872E4D09044D54F003C3148F`), заблокирован на узле Ethereum. Для отправки транзакций с этого аккаунта его необходимо сначала разблокировать, предоставив пароль, который вы использовали при создании аккаунта.

Ошибка:
```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32020,
    "message": "Can only sign without passphrase when account is unlocked."
  },
  "id": 1
}
```

**Как разблокировать аккаунт:**

Вы можете использовать RPC-метод `personal_unlockAccount`. Этот метод принимает адрес аккаунта, пароль и, опционально, продолжительность, на которую аккаунт будет разблокирован (в секундах). Если продолжительность не указана или равна `null` или `0`, аккаунт будет разблокирован на время одной сессии (обычно до перезапуска узла или до явной блокировки).

**Команда для разблокировки аккаунта с помощью вашего скрипта `eth_rpc_auth.py`:**

```bash
python eth_rpc_auth.py personal_unlockAccount YOUR_FROM_ADDRESS "YOUR_PASSWORD" 300
```

Замените:
*   `YOUR_FROM_ADDRESS` на `0xDB6f479AFa71e7af872E4D09044D54F003C3148F`
*   `"YOUR_PASSWORD"` на пароль, который вы установили для этого аккаунта (в вашем предыдущем примере это был `"ajklhsaduiorewp34170JsdfaLD"`)
*   `300` — это время в секундах (5 минут), на которое аккаунт будет разблокирован. Вы можете изменить это значение или убрать его, чтобы разблокировать на время сессии.

**Пример:**

```bash
python eth_rpc_auth.py personal_unlockAccount 0xDB6f479AFa71e7af872E4D09044D54F003C3148F "ajklhsaduiorewp34170JsdfaLD" 300
```

Если команда выполнится успешно, вы получите ответ:
```json
{
  "jsonrpc": "2.0",
  "result": true,
  "id": 1
}
```

После этого вы сможете повторно выполнить команду для отправки транзакции:

```bash
python eth_rpc_auth.py send_tx 0xDB6f479AFa71e7af872E4D09044D54F003C3148F 0x98764840B4711E01a53aC466bb04E0246cBf4C30 1000000000000000 "0x" 21000 6000000000
```

**Важно:**
*   Разблокировка аккаунта на узле — это операция, связанная с безопасностью. Убедитесь, что вы доверяете окружению, в котором выполняется узел, и что RPC-порт не доступен из недоверенных сетей.
*   Некоторые узлы могут иметь ограничения на использование модуля `personal` или требовать специальной конфигурации для его активации. Судя по вашим предыдущим успехам с `personal_newAccount`, он у вас активен.

как заблокировать обратно аккаунт после использования:

```bash

python eth_rpc_auth.py personal_lockAccount 0xDB6f479AFa71e7af872E4D09044D54F003C3148F
```
```json
{
  "jsonrpc": "2.0",
  "result": true,
  "id": 1
}
```
# Ethereum Node Authentication and Monitoring
