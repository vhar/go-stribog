Go GOST (Stribog) Signature Verifier REST API Server
====================================================

# Описание
REST API сервис для проверки открепленных цифровых подписей использующих алгоритмы хеширования ГОСТ Р 34.11-94, ГОСТ Р 34.11-2012 (256/512 бит)

# Методы
В сервисе реализовано два метода проверки:  
1. **Verify File** - проверка с загрузкой файлов на сервер
2. **Verify URL** - проверка файлов расположенных на удаленном сервере  

Оба метода возвращают одинаковую json структуру отчета ***payload*** в случае успеха или структуру ошибки ***error*** содержащую поля ***message*** с описанием ошибки и ***code*** c HTTP кодом ошибки
Пример отчета:
```
{
    "payload":{
        "Signer": {
            "CommonName": "АО \"РОГА И КОПЫТА\"",
            "CountryName": "RU",
            "StateOrProvinceName": "64 Саратовская область",
            "LocalityName": "Арбатов",
            "Surname": "Паниковский",
            "GivenName": "Михаил Самуэльевич",
            "Title": "Уполномоченный по рогам",
            "EmailAddress": "misha@panikovsky.gov"
        },
        "Certificate": {
            "IssuerName": "УЦ Федерального казначейства",
            "NotBefore": "03.02.2015 08:26",
            "NotAfter": "03.05.2016 08:26",
            "EncriptionAlgorithm": "ГОСТ Р 34.11/34.10-2001",
            "DigestAlgorithm": "ГОСТ Р 34.11-94"
        },
        "SigningTime": "09.22.2016 09:50:00 UTC",
        "Validity": false
    }
}
```
> Поле Validity возвращает true в случае если файл подписи был создан для переданного документа и последний после этого не модифицировался.

Пример ошибки
```
{
    "error":{
        "code": 422,
        "message": "Ошибка загрузки файла документа."
    }
}
```

# Метод Verify File
Для проверки подписи данным методом необходимо отправить запрос методом **POST** на URN **vfile**  
Content-Type: multipart/form-data  
В теле запроса необходимо передать проверяемый документ в поле ***document*** и файл подписи в поле ***signature***

# Метод Verify URL
Для проверки подписи данным методом необходимо отправить запрос методом **POST** на URN **vurl**  
Content-Type: application/json  
В теле запроса в поле ***document*** указать URL на проверяемый документ и в поле ***signature*** URL на файл с проверяемой подписью.  
Дополнительно в теле запроса можно передать поле ***referer*** если сервер, на котором расположены проверяемые файлы не позволяет обращаться у ним напрямую.  

# Конфигурация
Файл конфигурации **config.yaml** должен быть расположен с корне проекта.  
Пример файла с комментариями ***config-example.yaml***
