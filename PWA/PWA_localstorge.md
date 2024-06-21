

# pwa-and-localstorage


https://amorev.ru/pwa-and-localstorage/

PWA и localstorage
19.08.2019
В последнее время все больше и больше в компании погружаемся в процесс разработки прогрессивных вэб приложений (PWA). Суть их проста - WEB приложение, с недавних пор, можно устанавливать как нативные - отлично работает на андроиде, windows и linux. Чуть хуже работает на iOs и macos. Но сегодня речь не о совместимости


Почти всегда, ранее, когда нужно было сохранять какую-нибудь информацию в браузере мы использовали localStorage. Его встроенные ограничения подходили нам более чем полностью практически всегда (за исключением некоторых проектов). Но в случае с PWA ситуация интересная - localStorage не работает в PWA режиме. То есть когда приложение запускается "как нативное" в интерфейсе телефона. Проблема не критичная, но какое-то время на то, чтобы понять в чем дело, ушло.

Решение довольно простое - использовать IndexedDB.
        
```javascript 
        import { openDB } from 'idb'
        
        const dbPromise = openDB('keyval-store', 1, {
        upgrade (db) {
            db.createObjectStore('keyval')
            }
        })
        
        const idbKeyval = {
            async get (key) {
                return (await dbPromise).get('keyval', key)
            },
        async set (key, val) {
            return (await dbPromise).put('keyval', val, key)
            },
        async delete (key) {
            return (await dbPromise).delete('keyval', key)
            },
        async clear () {
            return (await dbPromise).clear('keyval')
            },
        async keys () {
            return (await dbPromise).getAllKeys('keyval')
            }
        }

        let data = idbKeyval.get('key') // получение значения
        idbKeyval.set('key', '123') // сохранение значения
        idbKeywal.delete('key') // удаление значения
```
Само расширение устанавливается простой командой

        npm install -s idb

В конечном итоге мы можем использовать idbKeyval как хранилище типа "Ключ-Значения" и полностью заменить себе, тем самым localstorage с поддержкой функциональности уже везде.

P.S. Еще есть расширение https://www.npmjs.com/package/localforage которое из коробки реализует API Localstorage, используя под капотом WebSQL или IndexedDB

