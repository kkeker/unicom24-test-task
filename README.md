Документация на готовый API для интеграции
==

**Требования к окружению:**
API постоена на основе асинхронного протокола обмена JSON-сообщениями - [JSON-RPC v2](http://www.jsonrpc.org/specification).
Статья на [Хабрахабр](https://habrahabr.ru/post/150803/)

Примеры библиотек для реализации клиентской части:

**Node.JS:** [Вариант 1](https://www.npmjs.com/package/jsonrpc-node), [Вариант 2](https://www.npmjs.com/package/node-json-rpc)

**PHP:** [Вариант 1](https://github.com/sergeyfast/eazy-jsonrpc)

**Java SE 7/8:** [Вариант 1](https://github.com/arteam/simple-json-rpc), [Вариант 2](https://github.com/briandilley/jsonrpc4j)

**Go:** [Вариант 1](https://golang.org/pkg/net/rpc/jsonrpc/) 

**Python 2/3:** [Вариант 1](https://pypi.python.org/pypi/json-rpc), [Вариант 2](https://github.com/gerold-penz/python-jsonrpc)

**Ruby:** [Вариант 1](https://github.com/chriskite/jimson), [Вариант 2](https://github.com/movitto/rjr)

**Параметры подключения:**

_URL:_ [https://example.com:123/api/v1/](https://example.com:123/api/v1/)


*****
**МЕТОДЫ**
*****

**Авторизация в сервисе [auth.login]:**

_Пример запроса:_

    {
        "jsonrpc": "2.0",
        "method": "auth.login",
        "params": {
            "login": "*****",
            "password": "*****"
        },
        "id": 1
    }
    
Где **login**(Type: String) и **password**(Type: String) переменные, выданные при [подключении сервиса](support@example.com).

_Пример ответа:_

    {
        "jsonrpc": "2.0",
        "result": {
            "auth": true,
            "token": "16fd2706-8baf-433b-82eb-8c7fada847da",
            "error": null
        },
        "id": 1
    }
    
Где **auth**(Type: Boolean) принимает значение true/false для авторизации, **token**(Type: String) имеет формат [UUIDv4](https://ru.wikipedia.org/wiki/UUID), **error**(Type: String or Null) содержит или описание ошибки или null.

**Поиск по VIN-номеру [car.info]:**

_Пример запроса:_

    {
        "jsonrpc": "2.0",
        "method": "auto.info",
        "params": {
            "vinNum": "4USBT53544LT26841",
            "carNum": ""
        },
        "id": 1
    }
    
Где обязательным является [HTTP-заголовок](https://ru.wikipedia.org/wiki/%D0%A1%D0%BF%D0%B8%D1%81%D0%BE%D0%BA_%D0%B7%D0%B0%D0%B3%D0%BE%D0%BB%D0%BE%D0%B2%D0%BA%D0%BE%D0%B2_HTTP) для каждого запроса:
    
    "Auth-Token": "16fd2706-8baf-433b-82eb-8c7fada847da" 
    
При этом авторизационный токен получается из обращения к методу **auth.login**.
Параметр **vinNum**(Type: String) содержит уникальный [идентификационный номер транспортного средства](https://ru.wikipedia.org/wiki/%D0%98%D0%B4%D0%B5%D0%BD%D1%82%D0%B8%D1%84%D0%B8%D0%BA%D0%B0%D1%86%D0%B8%D0%BE%D0%BD%D0%BD%D1%8B%D0%B9_%D0%BD%D0%BE%D0%BC%D0%B5%D1%80_%D1%82%D1%80%D0%B0%D0%BD%D1%81%D0%BF%D0%BE%D1%80%D1%82%D0%BD%D0%BE%D0%B3%D0%BE_%D1%81%D1%80%D0%B5%D0%B4%D1%81%D1%82%D0%B2%D0%B0).
Параметр **carNum**(Type: String) содержит [номер государственной регистрации транспортного средства](https://ru.wikipedia.org/wiki/%D0%90%D0%B2%D1%82%D0%BE%D0%BC%D0%BE%D0%B1%D0%B8%D0%BB%D1%8C%D0%BD%D1%8B%D0%B9_%D0%BD%D0%BE%D0%BC%D0%B5%D1%80).
Параметры **carNum**(Type: String) и **vinNum**(Type: String) являются опциональными, но хотя бы один из них должен быть указан. Поиск происходит или по одному или по другому параметру при условии что значения не пусты.

_Пример ответа:_

    {
        "jsonrpc": "2.0",
        "result": {
            "brand": "BMW",
            "model": "Z4 3.0i",
            "issueYear": "2014",
            "engineV": "2.0",
            "vin": "4USBT53544LT26841",
            "carNum": "C065MK777",
            "rudder": "right",
            "weight": "1.5",
            "owners": [
                {
                    "firstName": "Kirill",
                    "lastName": "Keker",
                    "bought": "2014",
                    "sails": "2015"
                },
                {
                     "firstName": "Vasiliy",
                     "lastName": "Pupkin",
                     "bought": "2015",
                     "sails": ""
                }
            ],
            "techInspect": [
                {
                    "year": "2015"
                }
            ],
            "taxiUse": [
            ],
            "crashInjury": [
            ],
            "pledges": [
            ],
            "other": [
            ]
            "error": null
        },
        "id": 1
    }       

Описание параметров:

    brand - Марка (Type: String)
    model - Модель (Type: String)
    issueYear - Год выпуска (Type: String)
    engineV - Объем двигателя в литрах(Type: Float)
    vin - VIN-номер (Type: String)
    carNum -Номер госрегистрации (Type: String)
    rudder - Расположение руля left/right (Type: String)
    weight - Масса в тоннах (Type: Float)
    owners - Инфоомация о владельцах (Type: Array)
    techInspect - Информация о техосмотре (Type: Array)
    taxiUse - Информация об использовании в службах такси (Type: Array)
    crashInjury - Информация о ДТП (Type: Array)
    pledges - Информация о залогах транспортного средства (Type: Array)
    other - Прочие ограничения и замечания (Type: Array)
    error - Тестовое сообщение об ошибке (Type: String or Null)


