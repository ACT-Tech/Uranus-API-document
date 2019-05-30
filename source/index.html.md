---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - Json


toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# <span style="color:#e01b84">Overview</span>
Create a payment gateway for cryptocurrencies.
To bridge the gap between cryptocurrency owners, who are looking for ways to use their coins for purchasing goods and services, and merchants, that can easily utilize the benefits of blockchain to grow their businesses.

# <span style="color:#e01b84">Goals</span>
* <span style="color:#e01b84"><b>BTC, ETH, ADA, and XRP:</b></span> The system must support these common cryptocurrencies.
* <span style="color:#e01b84"><b>The system has higher security and reliability requirements:</b></span> Because every transaction has financial impacts on the users and the business. Security should be of a high stake because it is very hard to gain trust and super easy to get it lost.
* <span style="color:#e01b84"><b>Traffic and Scalability:</b><</span> Speed up the payment process. 
* <span style="color:#e01b84"><b>Backup payment processors:</b><</span> Handle retries and backup Payment, the System and the log files.
* <span style="color:#e01b84"><b>Operations and customer support:</b></span> Easy to operate and support when the customers need.
* <span style="color:#e01b84"><b>Analytics:</b><</span> To support improve system architecture and answer any question of the marketing team or the manager.

# <span style="color:#e01b84">REST API</span>
You must send an authorization header with every request (Except for Public APIs). App token will be shown after you create an app.
Using the HTTP Authorization header like this: <br>
`Authorization: Token <app_token>`<br>
The tokens can be generated on the payment system's console. The merchant will need to define the application name when generating a new token. It will help the merchant can filter reports better.

## <span style="color:#e01b84">Response format</span>
* Format: JSON standard
* Timezone: Coordinated Universal Time (UTC), UTC +0 with ISO 8601 format.
* Support UTF-8 encoding: YES
* Number format in JSON: Double value will be return 1.1 or "1.1"

## <span style="color:#e01b84">Environments</span>
<b>Name</b> | <b>Link (Notify if they are updated)</b>
--------- | -----------
Live (Mainnet) | <a href='https://payment.act-tech.io/'>https://payment.act-tech.io/</a>
Live (Mainnet) | <a href='https://payment-sandbox.act-tech.io/'>https://payment-sandbox.act-tech.io/</a>

## <span style="color:#e01b84">Error code</span>
<b>Error Code</b> | <b>HTTP Status</b> | <b>Message</b> | <b>Note</b>
---------- | ---------- | ---------- | ----------
1000 | 401 | Missing Authorization Header |
1001 | 401 | Invalid Authorization Header |
4000 | 400 | Please check the parameters again | Default
4001 | 400 | Price amount is required
4002 | 400 | Price currency is required
4220 | 422 | The data is invalid | Default

## <span style="color:#e01b84">Status code</span>
<b>HTTP Status</b> | <b>Description</b>
---------- | ----------
200 (OK) | API response success
201 (Created) | Record is created
401 (Unauthorized) | API credentials are not valid
404 (Not Found) | Page, action not found
404 (Not Found) | Record not found
400 (Bad Request) | Missing params
422 (Unprocessable Entity) | The value of params is invalid
500 (Internal Server Error) | Something wrong in servers
429 (Too Many Requests) | API request limit is exceeded
Response example: <br>
`JSON
  {
    "message": "The order is not found",
    "error_code": 404
  }`

## <span style="color:#e01b84">Merchant API<span>

### <span style="color:#e01b84">Create Order<span>
<b>Method</b> | <b>HTTP request</b> | <b>Description</b>
---------- | ---------- | ----------
URLs relative to `https://<Server URL>/api/v1/`, unless otherwise noted |
post | POST  /orders | Create Order

<b>Parameters:</b> <br>

<b>Parameter name</b> | <b>Value</b> | <b>Description</b>
--------- | --------- | ----------
price_amount Or invoice_price_amount | double | The price set by the merchant. Example: 1050.99
price_currency Or invoice_price_currency | string | Currency code which defines the currency in which you wish to price your merchandise. Eg, USD, JPY (It also can be BTC, ADA)
Optional query parameters | |
order_id | string | Merchant's custom order ID
title | string | Max 150 characters. Order title
description | string | More details about this order. Max 500 characters
callback_url | string | Send an automated message to Merchant URL when order status is changed or the number of confirmation is enough. The data is the same order/invoice response.
success_url | string | Redirect to Merchant URL after successful payment
cancel_url | string | Redirect to Merchant URL when buyer cancels the order
pay_currency | string | Cryptocurrency code which will be paid. Eg, BTC, ADA. If you want the user to process payment on your system, please send this param

<b>Response</b> <br>
If successful, this method returns a response body with the following structure <br>

```shell
{
    "id": 53,
    "status": "unconfirmed",
    "invoice_price_currency": "USD",
    "invoice_price_amount": 0.5,
    "wallet_address": "367f4YWz1VCFaqBqwbTrzwi2b1h2U3w1AF",
    "invoice_created_at": "2018-11-15T05:49:36.553+07:00",
    "order_id": "ORDER-1539318129916178-4",
    "token": "DkNB5m5a7VZySCWr2jZfEw",
    "payment_url": "https://.../invoices/f7eb404c-...-be1fd9bbc464",
    "title": "Order 4",
    "description": "1 × Espresso Con Panna",
    "pay_amount": 16.26,
    "pay_currency": "ADA",
    "wallet_address": "Ae2tdPwUPEZ9Z3...UHZdqHMXUvHMp5i5SE5D2YxkB",
    "qr_code_url": "https://paym...ices/qr_code/Ae5D2xkB",
    "invoice_confirmed_at": null,
    "invoice_received_amount": 0,
  }
```

<!-- <aside class="padding-code" style='background-color: #E5E9EB'>
{ <br>
    <span class='greenText'>"id":</span><span class="pinkText">53,</span> <br>
    <span class='greenText'>"status":</span><span class="pinkText">"unconfirmed",</span>  <br>
    <span class='greenText'>"invoice_price_currency":</span><span class="pinkText">"USD",</span> <br>
    <span class='greenText'>"invoice_price_amount":</span><span class="pinkText">0.5,</span> <br>
    <span class='greenText'>"wallet_address":</span><span class="pinkText">"367f4YWz1VCFaqBqwbTrzwi2b1h2U3w1AF",</span> <br>
    <span class='greenText'>"invoice_created_at":</span><span class="pinkText">"2018-11-15T05:49:36.553+07:00",</span> <br>
    <span class='greenText'>"order_id":</span><span class="pinkText">"ORDER-1539318129916178-4",</span> <br>
    <span class='greenText'>"token":</span><span class="pinkText">"DkNB5m5a7VZySCWr2jZfEw",</span> <br>
    <span class='greenText'>"payment_url":</span><span class="pinkText">"https://.../invoices/f7eb404c-...-be1fd9bbc464",</span> <br>
    <span class='greenText'>"title":</span><span class="pinkText">"Order 4",</span> <br>
    <span class='greenText'>"description":</span><span class="pinkText">"1 × Espresso Con Panna",</span> <br>
    <span class='greenText'>"pay_amount":</span><span class="pinkText">16.26,</span> <br>
    <span class='greenText'>"pay_currency":</span><span class="pinkText">"ADA",</span> <br>
    <span class='greenText'>"wallet_address":</span><span class="pinkText">"Ae2tdPwUPEZ9Z3...UHZdqHMXUvHMp5i5SE5D2YxkB",</span> <br>
    <span class='greenText'>"qr_code_url":</span><span class="pinkText">"https://paym...ices/qr_code/Ae5D2xkB",</span> <br>
    <span class='greenText'>"invoice_confirmed_at":</span><span class="pinkText">null,</span> <br>
    <span class='greenText'>"invoice_received_amount":</span><span class="pinkText">0,</span> <br>
  }
</aside> -->

<b>Response data</b> | <b>Value</b> | <b>Description</b>
---------- | ---------- | ----------
payment_url | String URL | The URL to payment when using ACT-Payment screen UI
wallet_address | String URL | The wallet address which will be received payment amount.
qr_code_url | String URL | The QR code URL of wallet address
status | string | Status of order (invoice)
pay_amount | double | The cryptocurrency amount which the buyer needs to pay
invoice_created_at | DateTime | The time when the order (invoice) was created
token | string | The token is used when need to verify the payment again. Maybe the merchants’ systems don’t need to store it.

### <span style="color:#e01b84">Create Order Status<span>

```shell
{ 
"id":53, 
"status":"paid", 
"invoice_price_currency":"USD", 
"invoice_price_amount":0.5, 
"created_at":"2018-11-15T05:49:36.553+07:00", 
"order_id":"ORDER-1539318129916178-4", 
"token":"DkNB5m5a7VZySCWr2jZfEw", 
"payment_url":"https://.../invoices/f7eb404c-...-be1fd9bbc464", 
"title":"Order 4", 
"description":"1 × Espresso Con Panna", 
"pay_amount":16.26, 
"pay_currency":"ADA", 
"wallet_address":"Ae2tdPwUPEZ9Z3...UHZdqHMXUvHMp5i5SE5D2YxkB", 
"qr_code_url":"https://paym...ices/qr_code/Ae5D2xkB", 
"invoice_confirmed_at": "2018-11-15T05:55:36.553+07:00", 
"invoice_received_amount":16.3 
}
```

<b>Method</b> | <b>HTTP Request</b> | <b>Description</b>
--------- | --------- | ----------
URIs relative to `https://<Server URL>/api/v1/`, unless otherwise noted | |
get | GET  `/orders/:id` | Get Order detail
<b>Parameter</b> <br>

<b>Parameter name</b> | <b>Value</b> | <b>Description</b>
--------- | ---------- | ----------
Required parameters | |
id | integer | Order ID

<b>Reponsise</b> <br>
If successful, this method returns a response body with the following structure <br>


<!-- <aside class="padding-code" style='background-color: #E5E9EB'>
{ <br>
    <span class='greenText'>"id":</span><span class="pinkText">53,</span> <br>
    <span class='greenText'>"status":</span><span class="pinkText">"paid",</span>  <br>
    <span class='greenText'>"invoice_price_currency":</span><span class="pinkText">"USD",</span> <br>
    <span class='greenText'>"invoice_price_amount":</span><span class="pinkText">0.5,</span> <br>
    <span class='greenText'>"created_at":</span><span class="pinkText">"2018-11-15T05:49:36.553+07:00",</span> <br>
    <span class='greenText'>"order_id":</span><span class="pinkText">"ORDER-1539318129916178-4",</span> <br>
    <span class='greenText'>"token":</span><span class="pinkText">"DkNB5m5a7VZySCWr2jZfEw",</span> <br>
    <span class='greenText'>"payment_url":</span><span class="pinkText">"https://.../invoices/f7eb404c-...-be1fd9bbc464",</span> <br>
    <span class='greenText'>"title":</span><span class="pinkText">"Order 4",</span> <br>
    <span class='greenText'>"description":</span><span class="pinkText">"1 × Espresso Con Panna",</span> <br>
    <span class='greenText'>"pay_amount":</span><span class="pinkText">16.26,</span> <br>
    <span class='greenText'>"pay_currency":</span><span class="pinkText">"ADA",</span> <br>
    <span class='greenText'>"wallet_address":</span><span class="pinkText">"Ae2tdPwUPEZ9Z3...UHZdqHMXUvHMp5i5SE5D2YxkB",</span> <br>
    <span class='greenText'>"qr_code_url":</span><span class="pinkText">"https://paym...ices/qr_code/Ae5D2xkB",</span> <br>
    <span class='greenText'>"invoice_confirmed_at":</span><span class="pinkText">2018-11-15T05:55:36.553+07:00",</span> <br>
    <span class='greenText'>"invoice_received_amount":</span><span class="pinkText">16.3</span> <br>
  }
</aside> <br> -->

<b>Response data</b> | <b>Value</b> | <b>Description</b>
--------- | ---------- | ----------
nvoice_confirmed_at | DateTime | The time when the blockchain network confirmed the transaction (Normally 6 blocks for a Bitcoin transaction)
invoice_received_amount | Double | The real amount which the system
 | | was received for this order (invoice).

## <span style="color:#e01b84">Reporting API<span>

### <span style="color:#e01b84">List the orders<span>

```shell
{ 
"message":"Success", 
"data": [, 
{ 
"id":6, 
"status":"paid", 
"invoice_price_currency":"USD", 
"invoice_price_amount":1.0, 
"created_at":"2018-10-23T16:33:24.485+09:00", 
"order_id":"ORDER-1540280002934932-6", 
"token":"uwxHYa4Q4Kg-UR-0UCHZ3w", 
"payment_url":"https://payment.act-te...8b3-8655-69ee5542fc1a", 
"title":"Order #6", 
"description":"1 × 1 × Brewed Coffee", 
"pay_amount":0.0001544, 
"pay_currency":"BTC", 
"wallet_address":"2MvvXu8JdcZdo9pAkw2fMvAZxrmTbhrviap", 
"wallet_url":"https://payment.act-t...cZdo9pAkw2fMvAZxrmTbhrviap", 
}, 
{ "id":5, 
"status":"expired", 
"invoice_price_currency":"USD", 
"invoice_price_amount":3.0, 
"created_at":"2018-10-23T16:17:59.203+09:00", 
"order_id":"ORDER-1540279075370022-5", 
"token":"B5CO-64aXVk_vb-Op3QK5w", 
"payment_url":"https://payment.act-tech...0b-45d1-b919-3f9110032efd", 
"title":"Order #5", 
"description":"1 × Caramel Macchiato", 
"pay_amount":0.0004635, 
"pay_currency":"BTC", 
"wallet_address":"2N47sHtU9wewejLGh3Hv8H89gNX53xNa4PH", 
"wallet_url":"https://payment.act-tech...9wewejLGh3Hv8H89gNX53xNa4PH", 
} 
] 
}
```

<b>Method</b> | <b>HTTP request</b> | <b>Description</b>
--------- | ---------- | ----------
URIs relative to `https://<Server URL>/api/v1/`, unless otherwise noted | |
get | GET  /report/orders | Report for orders

<b>Parameter</b> <br>

<b>Parameter name</b> | <b>Value</b> | <b>Description</b>
--------- | ---------- | ----------
Optional query parameters | |
date_range | string | See Date range value
from | date | Start date range with format YYYY-MM-DD. It can’t be greater today.
to | date | End date range with format YYYY-MM-DD. It must be greater from value and can’t be greater today.
price_currency | string | Filter orders which use price_currency
pay_currency | string | Cryptocurrency code: BTC or ADA. Filter orders which were paid with pay_currency
status | string | Filter orders with status.
amount | double | Filter orders which amount
keyword | string | Search title and description or wallet address
<b>Operations</b> | |
= | | The content of a string or boolean is equal to the other or a value is equal to another.
< | | A value is less than another
<= | | A value is less than or equal to another.
> | | A value is later than another.
>= | | A value is later than or equal to another.
and | | Return items that match both clauses.
or | | Return items that match either clause.

Date range value <br>

<b>Value</b> | <b>Description</b>
---------- | ----------
TODAY | Today only
YESTERDAY | Yesterday only.
LAST_7_DAYS | The last 7 days not including today.
THIS_MONTH | All days in the current month.
LAST_MONTH | All days in the previous month.
ALL_TIME | The entire available time range.
CUSTOM_DATE | A custom date range. Use with parameters from and to
LAST_14_DAYS | The last 14 days not including today.
LAST_30_DAYS | The last 30 days not including today.

<b>Response</b> <br>

If successful, this method returns a response body with the result

<!-- <aside class="padding-code" style='background-color: #E5E9EB'>
{ <br>
    <span class='greenText'>"message":</span><span class="pinkText">"Success",</span> <br>
    <span class='greenText'>"data":</span><span class="pinkText"> [,</span>  <br>
{ <br>
    <span class='greenText'>"id":</span><span class="pinkText">6,</span> <br>
    <span class='greenText'>"status":</span><span class="pinkText">"paid",</span>  <br>
    <span class='greenText'>"invoice_price_currency":</span><span class="pinkText">"USD",</span> <br>
    <span class='greenText'>"invoice_price_amount":</span><span class="pinkText">1.0,</span> <br>
    <span class='greenText'>"created_at":</span><span class="pinkText">"2018-10-23T16:33:24.485+09:00",</span> <br>
    <span class='greenText'>"order_id":</span><span class="pinkText">"ORDER-1540280002934932-6",</span> <br>
    <span class='greenText'>"token":</span><span class="pinkText">"uwxHYa4Q4Kg-UR-0UCHZ3w",</span> <br>
    <span class='greenText'>"payment_url":</span><span class="pinkText">"https://payment.act-te...8b3-8655-69ee5542fc1a",</span> <br>
    <span class='greenText'>"title":</span><span class="pinkText">"Order #6",</span> <br>
    <span class='greenText'>"description":</span><span class="pinkText">"1 × 1 × Brewed Coffee",</span> <br>
    <span class='greenText'>"pay_amount":</span><span class="pinkText">0.0001544,</span> <br>
    <span class='greenText'>"pay_currency":</span><span class="pinkText">"BTC",</span> <br>
    <span class='greenText'>"wallet_address":</span><span class="pinkText">"2MvvXu8JdcZdo9pAkw2fMvAZxrmTbhrviap",</span> <br>
    <span class='greenText'>"wallet_url":</span><span class="pinkText">"https://payment.act-t...cZdo9pAkw2fMvAZxrmTbhrviap",</span> <br>
  }, <br>
  {
    <span class='greenText'>"id":</span><span class="pinkText">5,</span> <br>
    <span class='greenText'>"status":</span><span class="pinkText">"expired",</span>  <br>
    <span class='greenText'>"invoice_price_currency":</span><span class="pinkText">"USD",</span> <br>
    <span class='greenText'>"invoice_price_amount":</span><span class="pinkText">3.0,</span> <br>
    <span class='greenText'>"created_at":</span><span class="pinkText">"2018-10-23T16:17:59.203+09:00",</span> <br>
    <span class='greenText'>"order_id":</span><span class="pinkText">"ORDER-1540279075370022-5",</span> <br>
    <span class='greenText'>"token":</span><span class="pinkText">"B5CO-64aXVk_vb-Op3QK5w",</span> <br>
    <span class='greenText'>"payment_url":</span><span class="pinkText">"https://payment.act-tech...0b-45d1-b919-3f9110032efd",</span> <br>
    <span class='greenText'>"title":</span><span class="pinkText">"Order #5",</span> <br>
    <span class='greenText'>"description":</span><span class="pinkText">"1 × Caramel Macchiato",</span> <br>
    <span class='greenText'>"pay_amount":</span><span class="pinkText">0.0004635,</span> <br>
    <span class='greenText'>"pay_currency":</span><span class="pinkText">"BTC",</span> <br>
    <span class='greenText'>"wallet_address":</span><span class="pinkText">"2N47sHtU9wewejLGh3Hv8H89gNX53xNa4PH",</span> <br>
    <span class='greenText'>"wallet_url":</span><span class="pinkText">"https://payment.act-tech...9wewejLGh3Hv8H89gNX53xNa4PH",</span> <br>
   } <br>
  ] <br>
   }
</aside> -->

## <span style="color:#e01b84">Public API<span>

Current exchange rates for Merchants. This endpoint is public, authentication is not required

### <span style="color:#e01b84">List the supported coins<span>

<b>Method</b> | <b>HTTP request</b> | <b>Description</b>
---------- | ---------- | ----------
URIs relative to https://<Server URL>/api/v1/, unless otherwise noted | | 
get | GET  /listings | Displays all supported coins

<b>Response</b> <br>
If successful, this method returns a response body with the result

<aside class="padding-code" style='background-color: #E5E9EB'>

[<span class="pinkText">"BTC", "ADA"</span>] <br>

</aside>

<b>Example</b> <br>

<aside class="padding-code" style='background-color: #E5E9EB'>

<a href="https://payment.act-tech.io/api/v1/listings">https://payment.act-tech.io/api/v1/listings</a>

</aside>

### <span style="color:#e01b84">Get Exchange Rate<span>

<b>Method</b> | <b>HTTP request</b> | <b>Description</b>
---------- | ---------- | ----------
URIs relative to https://<Server URL>/api/v1/, unless otherwise noted | | 
get | GET  `/rates/:from/:to` | Get an exchange rate

```shell
{ 
"from":String, 
"to":String, 
"rate":double, 
"time":DateTime, 
}

```



<b>Prameters</b>

<b>Parameter name</b> | <b>Value</b> | <b>Description</b>
---------- | ---------- | ----------
Required parameters | | 
from | string | ISO Symbol. Example: JPY, USD
to | string | ISO Symbol. Example: BTC, ADA
Option parameters | |
source | string | The source to get the exchange rate data. Default: Cryptocompare

<b>Response</b> <br>

If successful, this method returns a response body with the result

<!-- <aside class="padding-code" style='background-color: #E5E9EB'>
{ <br>
    <span class='greenText'>"from":</span><span class="pinkText">String,</span> <br>
    <span class='greenText'>"to":</span><span class="pinkText">String,</span> <br>
    <span class='greenText'>"rate":</span><span class="pinkText">double,</span> <br>
    <span class='greenText'>"time":</span><span class="pinkText">DateTime,</span> <br>
  }
</aside> <br> -->

<b>Example</b> <br>

<aside class="padding-code" style='background-color: #E5E9EB'>

<a href="https://payment.act-tech.io/api/v1/ping">https://payment.act-tech.io/api/v1/ping</a>

</aside>

# <span style="color:#e01b84">Real-Time Notifications<span>

```shell
{ 
"id":12, 
"order_id":"ORDER-1545492017181432-35", 
"token":"kmeJKu7LHOkH82XDEFW89Q", 
"status":"paid", 
"invoice_received_amount":"0.0007723", 
}
```

<b>Data post callback</b> <br>

If successful, the system will post to callback URL with data

<!-- <aside class="padding-code" style='background-color: #E5E9EB'>
{ <br>
    <span class='greenText'>"id":</span><span class="pinkText">12,</span> <br>
    <span class='greenText'>"order_id":</span><span class="pinkText">"ORDER-1545492017181432-35",</span> <br>
    <span class='greenText'>"token":</span><span class="pinkText">"kmeJKu7LHOkH82XDEFW89Q",</span> <br>
    <span class='greenText'>"status":</span><span class="pinkText">"paid",</span> <br>
    <span class='greenText'>"invoice_received_amount":</span><span class="pinkText">"0.0007723",</span> <br>
  }
</aside> -->

<b>Name</b> | <b>Description</b>
---------- | ----------
id | The invoice ID on the payment system
order_id | The invoice ID on the merchant system
token | The invoice token. It helps to avoid invalid data
status | The new status of the invoice
invoice_received_amount | The received amount





