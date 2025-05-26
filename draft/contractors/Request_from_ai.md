

**Domain**
- `app/MediaCatalog/Domain/MediaItem.php` \- доменный объект с публичными свойствами `id`, `title`, `guid`, `type`. Инкапсулирует сущность медиа.
- `app/MediaCatalog/Domain/MediaCatalog.php` \- сервисный класс (доменный сервис). Инъектирует `MediaCatalogRepositoryInterface` и возвращает пагинированные `MediaItem` (сложная бизнес-логика над репозиторием).

**Application**
- `app/MediaCatalog/Application/Mappers/DictorMapper.php` \- маппер, преобразует Eloquent-модель `DictorModel` в доменный объект `MediaItem`.
- `app/MediaCatalog/Application/Repositories/MediaCatalogRepositoryInterface.php` \- интерфейс репозитория, определяет методы для получения пагинированных данных (`fetchDictors`, `fetchMovies` и пр.).

**Infrastructure**
- `app/MediaCatalog/Infrastructure/ApiMediaCatalogRepository.php` \- реализация интерфейса репозитория. Выполняет запросы к БД через Eloquent, мапит результаты и возвращает `LengthAwarePaginator` с доменными `MediaItem`.
- `app/MediaCatalog/Infrastructure/Models/Dictor.php` \- Eloquent-модель для таблицы `dictors`, описывает связи и поля, используется внутри `ApiMediaCatalogRepository`.





вот пример коллекции sections
```json
{
    "_id" : ObjectId("67ddeadd21f0c7f2a306a6c4"),
    "name" : "New Section Name",
    "active" : true,
    "roles" : [
        {
            "role" : "manager",
            "access" : "owner"
        },
        {
            "role" : "executor",
            "access" : "preview"
        },
        {
            "role" : "verifier",
            "access" : "preview"
        },
        {
            "role" : "editor",
            "access" : "preview"
        }
    ],
  "stages": [
    "67ca136b78900f2691085706",
    "67ca1376b4751c96ac04a3d5",
    "67ca13cc3510b5f8640b8756"
  ],
    "vertical_id" : "vertical123",
    "project_id" : "project456",
    "files" : [
        "file1",
        "file2"
    ],
    "sectionData" : [
        "data1",
        "data2"
    ],
    "updated_at" : ISODate("2025-03-24T02:22:44.265+0000"),
    "created_at" : ISODate("2025-03-21T22:40:29.329+0000")
}
```

у нее есть блок роли и блок стадии, в блоке роли есть поле role и access, в блоке стадии есть массив стадий на которой можно показывать секцию, нужно написать запрос который вернет      все секции для пользователя  если роль пользователя  совпадают с ролью и доступом в блоке роли, и если стадия есть в массиве стадий в блоке стадии


данные заявки/заказа\
```JSON
{
  "_id" : ObjectId("67c024ea4091aa2b630d81d8"),
  "stage_id" : "67ca13cc3510b5f8640b8756",
}
```


```json

"section_buttons" : [   {
            "name" : "Кто менеджер",
            "targetStage" : "67ca139178900f2691085707",
            "buttonKey" : "button1",
            "imageBase64" : "iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAYAAADgdz34AAAACXBIWXMAAAsTAAALEwEAmpwYAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAFnSURBVHgBtVW7cYNAEH3WOKYCKqACKnDgmMAxFShwTOBYFShQBY4dOCZwTEBMQAWuwLfirQ2YXe40ozfzRtxpf3ez+w64Mx4i7Qoy53oM7MmbE2SBNZkZNpLoEvge+J2SoAw8YapYHD9Z7ci1nuiJyWX/NfALEagCB7KBXT1YwHFmX2EHJQ07fseioM/g+UmlLY0KpEOLa2GcuqbBEfuQAPnGfuPFkMyd4ThHTtvKSNyRVxz4qz0u3TLasa+n/OD3Vsdox2WMuUggsAZHHM6YrkBsnp1C+nnMRy5yJ4HOhCR5wzRYHsZ5zAP+V2oh2/lfsZjow1bWFeSuXzDJgXTHGX4jFKuYv5sDpqvwoK3cwp6VE4xZ0vbauwavTXPGaLccVVMaxCFzYtSWg0pFig4p9JpNqQCWYpeiRyUixE4xl2s5stcxUmkDR65TH5wef0IX9eDsPZlSUQ37FJL8QiY9mWvc/OjfHT8vHGIh2Flx5wAAAABJRU5ErkJggg=="
        },
        {
            "name" : "На обработке",
            "targetStage" : "67ca13bb78900f2691085708",
            "buttonKey" : "button2",
            "imageBase64" : "iVBORw0KGgoAAAANSUhEUgAAABkAAAAYCAYAAAAPtVbGAAAACXBIWXMAAAsTAAALEwEAmpwYAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAFcSURBVHgBvVa7lcIwENyjAFfgCqjgKnAFBBe7AgfEV4GDiy8gpgICxw4UO7jYgSu4CtDALE+AVxKf53lvnmy0H2u1IyGyAD4y7QrPT8+Sz/+ek6fj80tJELjhaKHz/PH8ezRJweA13x2DgBPn154VR2DHZMmVaYKD5+jZS3wVwvme9gf6J7GnQ5vrQLT026cMaxr+ynPQD6wtA3x1T5byHOA3kJcqrAKDikbolkny8C3Xq4bfjgk2cw4wHiV/FQ3tb4OVEik5yjRIHjRQa8wPjHcHOM11Bn4LWzPcO6v7VAInrG4m55wgMAiu4Tv2ASv5Elt4piBj5QpbewwSWjDLldp41UBKbHcbH5bLcdwYzls5t/dW4lAhdnOT2A8V0itiTDXFpfbJ88eAlrxOGYYHZC4KeeCAVIe3HvW5lxYuq44jNIDar0n9CPPSSl2/KsIqYuMY3FkGi/yRWARHJBRlz/KwdEIAAAAASUVORK5CYII="
        },
        {
            "name" : "Демо запись",
            "targetStage" : "67cab1023472fae93508c5d2",
            "buttonKey" : "button3",
            "imageBase64" : "iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAYAAADgdz34AAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAACxEAAAsRAX9kX5EAAAGHaVRYdFhNTDpjb20uYWRvYmUueG1wAAAAAAA8P3hwYWNrZXQgYmVnaW49J++7vycgaWQ9J1c1TTBNcENlaGlIenJlU3pOVGN6a2M5ZCc/Pg0KPHg6eG1wbWV0YSB4bWxuczp4PSJhZG9iZTpuczptZXRhLyI+PHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj48cmRmOkRlc2NyaXB0aW9uIHJkZjphYm91dD0idXVpZDpmYWY1YmRkNS1iYTNkLTExZGEtYWQzMS1kMzNkNzUxODJmMWIiIHhtbG5zOnRpZmY9Imh0dHA6Ly9ucy5hZG9iZS5jb20vdGlmZi8xLjAvIj48dGlmZjpPcmllbnRhdGlvbj4xPC90aWZmOk9yaWVudGF0aW9uPjwvcmRmOkRlc2NyaXB0aW9uPjwvcmRmOlJERj48L3g6eG1wbWV0YT4NCjw/eHBhY2tldCBlbmQ9J3cnPz4slJgLAAABcklEQVRIS7WVsXHDMAxFXzKAJtAEmsATpEitIrUmcJFahWtPkCITuHbhWkVqF6lVaAJNkObjDoZJWCn873g2CfADpD5AeDJe4kIFnUar+QL8aqTIAjRADwyOOGIFvjXWaCQJsAOOIl6Bi8u40XoHvGm+AJ/ATyQqoQdmjX2SPSIfnX8fHSJ2crwqw63Yac+s/0U0wPTIKUGnvZO47rCXwxgNuqbipgDjGKIBHfFaIeqVWfY9kP0qXwBe9duJ+FKRm6njXMtOWMRhKrsJQFI4C/Au+wh8VU6K47gJ4Cu0hhX4AA4SwbkiBgvQ4gIYStcT0STZE20WwDLPtN/qavbASacpVW7xNkzDR7/o0LkayT4y4phLyU6SWEmKW2XaOLnfYXD9JyK7cw/rSyWOm1Zxd7wNsD5WbRWEZleSYA3dlmZn8O16zLLRN7H+M5fa9X8fnEVze0IfPji1AGjjoFE7xaLn8lQr0iyAh2Xsi2jTo/90/AHRzGMfk8+hxwAAAABJRU5ErkJggg=="
        }
        ]
```


```curl
curl -X 'PUT' \
  'http://localhost:4010/api/sections/67e2fe828a724da6950aa9c2' \
  -H 'accept: */*' \
  -H 'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbG9jYWxob3N0OjQwMTAvYXBpL2xvZ2luIiwiaWF0IjoxNzQyOTQwMzQzLCJleHAiOjE3NzQ0NzYzNDMsIm5iZiI6MTc0Mjk0MDM0MywianRpIjoiSlZHSkl0Y2xkR3VhenJ4WCIsInN1YiI6IjY3YzFhNDBhYzlmZTY5YjJjODA5ZDY5MiIsInBydiI6IjIzYmQ1Yzg5NDlmNjAwYWRiMzllNzAxYzQwMDg3MmRiN2E1OTc2ZjcifQ.lj-9HD0JywHyuw7HDh09Og3qepKdUTrfCG7Nsf_eHu0' \
  -H 'Content-Type: multipart/form-data' \
  -H 'X-CSRF-TOKEN: ' \
  -F 'file=' \
  -F 'data={"name":"New Section","active":true,"type":"technicalSpecifications","project_id":"67e1158bc32edc7d28025502","sectionData":["data1","data2"]}'
```