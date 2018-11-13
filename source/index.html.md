---
title: Markeplace_HC

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='https://market.homecredit.ru'>Маркетлейс "Товары в рассрочку"</a>

includes:
  - errors

search: true
---

# Общие положения
Добро пожаловать на страницу интеграции с Маркетплейсом Хоум Кредит!

В документации детально описан процесс работы с клиентом от момента выбора товара и до момента взаиморасчета с партнером.

На странице содержатся требования к выгружаемому каталогу товаров и сервису API,который необходимо реализовать партнеру при осуществлении интеграции с Маркетплейсом.

<aside class="notice">
Доступ в Личный кабинет партнера (далее ЛК) предоставляется по согласованию с менеджером, реквизиты доступа высылаются на заявленный адрес электронной почты (e-mail).
</aside>

# Предоставление каталога 

<aside class="warning">
Партнер предоставляет каталог товаров в формате XML.
</aside>

<aside class="notice">
Базовый сценарий предполагает, что на каждый регион необходимо задать свой каталог. Возможен вариант когда каталог товаров един для всех регионов, данный сценарий необходимо предварительно согласовать с менеджером.
</aside>


##Описание параметров выгрузки

Ниже описаны основные параметры для выгрузки каталога. Для успешной синхронизации каталога в нем должны присутствовать все обязательные параметры, а также соблюдаться структура выбранного формата.

Выгруженный каталог может содержать в себе любые другие параметры и элементы, например всю структуру YML (стандарт, разработанный Яндексом для принятия и размещения информации в базе данных Яндекс.Маркета), что не будет вызывать ошибок обработки каталога со стороны Маркетплейс.

##Каталог в формате XML

Каталог обязательно должен иметь XML-заголовок, который начинается с первой строки, с нулевого элемента, в заголовке присутствуют атрибуты версии и используемая кодировка. Допустимые кодировки: UTF-8 или windows-1251. Пример заголовка:

За основу предложенной структуры XML-каталога использован формат выгрузки от [Яндекс.Маркет](https://yandex.ru/support/partnermarket/export/vendor-model.html), соответственно, если уже существует синхронизация с платформой Яндекс, достаточно внести незначительные дополнения, чтобы целевую выгрузку можно было задействовать для платформы Marketplace (минимально достаточно добавить атрибут credit в параметр offer, чтобы обозначить товары, которые планируется продавать в рассрочку)


##Информация о наличии товаров

 Базовый сценарий предполагает, что товар может поступить в любую точку самовывоза в рамках региона за указанный в delivery-options срок.

Если товар есть не во всех точках самовывоза или существует техническое ограничение на перевозку товаров из одного пункта в другой, предусмотрен формат указания списка точек самовывоза для отдельно взятого товара, для этого необходимо использовать параметр points и перечислить в нем точки самовывоза:
 
Список точек самовывоза партнер загружает через Личный кабинет, данный процесс описан в разделе[Предоставление информации о точках самовывоза](#19530bd8b7)





##Процесс загрузки каталога

Подготовленный каталог необходимо указать в настройках аккаунта, которые задаются в Личном кабинете. Для каждого региона необходимо указать свой URL до целевого каталога. В случае, если партнер присутствует во всех регионах и имеет единый товарный каталог, необходимо выбрать “Все регионы” (необходимо предварительное согласование менеджера).

![](image_catalogue.jpeg)


#Предоставление информации о точках самовывоза

Точки самовывоза передаются в виде CSV-таблицы с указанием адреса и режима работы. Необходимо использовать разделитель столбцов "," (запятая).

Загрузка и актуализация списка происходит через Личный кабинет. Если для какого-то регионов не указаны точки самовывоза, Покупателю будет предложен вариант доставки курьером.

При возможности разместить CSV на внешнем источнике, можно указать URL до целевого файла.
![](image_selfservice.jpeg)

##Описание параметров CSV с точками самовывоза


#Процесс покупки товара

##Покупка товара с доставкой курьером
Данный сценарий предполагает, что Покупатель подписывает договор без участия партнера, после чего инициируется процесс доставки.
![](image_delivery_curier.jpeg)
##Покупка товара с последующим самовывозом
Данный сценарий предполагает, что в точке самовывоза партнера присутствует кредитный специалист, который имеет доступ в систему Банка и полностью сопровождает процесс подписания договора с Покупателем.
![](image_delivery_self.jpeg)

##Проверка наличия товара у партнера
В момент когда Покупатель выбрал предложение, партнеру поступает запрос на проверку наличия данного товара в регионе. В ответ партнер должен уведомить о возможности зарезервировать товар, а также прислать условия доставки и самовывоза. Расчет параметров получения товара клиентом должен производиться на основе доступных товаров. (Например, если доступно только 3 из 5 переданных в запросе товаров, стоимость доставки должна быть только для этих трех товаров).



`Method: POST

Path: /order/check

Входные параметры:`

# Authentication

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Параметр для формата XML | Тип | 
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

