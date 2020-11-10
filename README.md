# Introduction
Figure out how to integrate StormGarden's APIs into your application
 

# Basics
The Stormgarden API allows you (has a Stormgarden partner) to programmatically onboard investors with a top broker enabling them to buy and sell securities listed on the Nigerian Stock Exchange through your application.

> # Before you Start
[Register as a partner](http://console.staging.storm.trium.ng/settings/api). You will be provided with test API and Secret keys, that you can use to test the endpoints. You should include `/test ` 

# Example Requests
We give sample API calls close to every endpoint using cURL. You simply have to insert  your specific values, and you can test the calls from the command line. You can [read this article](https://linuxize.com/post/curl-rest-api/) to learn how to use cURL with APIs.

Not familiar with cURL? You can use [Postman](https://www.postman.com/downloads/). Postman is a collaboration environment for API development including making HTTP requests. Run the [Stormgarden APIs Collection](https://documenter.getpostman.com/view/11930516/TVYF6xeD) in Postman to make testing quicker and easier.

# Requests and Responses
The request and response bodies are formatted in JSON. The Content-type for all responses is set to `application/json`. 
Successful and Error responses are in the following formats:

<!-- tabs:start -->
#### ** Successful **

```JSON
{
    "status": [integer],
    "data": [object]
}
```
#### ** Error **

```JSON
{
    "status": [integer],
    "errors": [array]
}
```
<!-- tabs:end -->

The status key contains the HTTP status code in order to determine the result of an API call, if it was successful call. Typicall, `2xx` indicates that a request was successful.

The data key has the result of a request. Its data type is an object (or an array) depending on the request made. A request to GET an investor's details returns an object in the data key, while a request to GET an investor's portfolio returns an array in the data key.

The error key contains the description of the error(s) with the request made.

# Authentication

API requests must be made over HTTPS.
API calls are further authenticated by including your access token in the authorization header of every request.

To generate your access token, call the [Access Token](#Access_Token) endpoint with your API and Secret keys. Generally, access token generated from test keys authenticates the test endpoints, while live endpoints are authenticated with access token generated from live keys.

Authorization headers should be set in this format : `Authorization: Bearer access_token`




> **Sample Authorization Header**
>
> Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwYXJ0bmVySWQiOiJkZDE3MmE3Zi0zM2I4LTQwZjQtODE5ZS0zMDg3M2EyZWE0NjIiLCJpZCI6Ijk5NzFhZjdjLTc0NjEtNDI3ZS05OWUwLWM5ZDRjM2M2NzMzYiIsIm1vZGUiOiJsaXZlIiwiaWF0IjoxNjA0NDE0NzI1LCJleHAiOjE2MDk1OTg3MjV9.FoHqZEfrTegIF8w72BZDrmhtb_Wt7rDsAvYxLD1ey2A 

# Errors
Some common errors that you might encounter during integration are discussed with their causes below

**You cannot access this resource**
1. You will get the error message when you don't authenticate your API calls with a valid access token. 
2. You may also get the error message when access token generated from test keys are used to authenticate API calls to a live endpoint.


| **Error Codes** | **Description** |
|-------------|-------------|
| 400         |  Th           |
| 422           |             |
|             |             |

# Access Token

The `/token ` endpoint allows you to generate an access token, for authorizing other API calls.

## Authorization

| Field 	| Required ? 	| Description 	|
|-	|-	|-	|
| Username 	| yes 	| Set value to API key 	|
| Password 	| yes 	| Set value to Secret key 	|

## Body Params
| Field 	| Required 	| Description 	|
|-	|-	|-	|
| grant_type 	| yes 	| Set as client_credentials 	|
| scope 	| yes 	| Set as profile 	|

<!-- tabs:start -->

#### ** cURL **

``` bash
curl https://api.staging.storm.trium.ng/token
 --d scope=profile 
 -d grant_type=client_credentials 
 -u 9g3p7e5yp1m4zyk89r1vsz4kk0xh:4c0763b8f583427f9a1380eff9273076
```
#### ** node.js **

``` javascript
var request = require('request');
var options = {
  'method': 'POST',
  'url': 'https://api.staging.storm.trium.ng/token',
  'headers': {
  },
  auth:{
    username: api_key,
    password: secret_key,
  },
  form: {
    'grant_type': 'client_credentials',
    'scope': 'profile'
  }
};
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});
```
#### ** Response **

```json
{
    "status": 201,
    "data": {
        "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwYXJ0bmVySWQiOiJkZDE3MmE3Zi0zM2I4LTQwZjQtODE5ZS0zMDg3M2EyZWE0NjIiLCJpZCI6ImY1ZTg3OGVhLTA4MjEtNDZmYS04YjYxLTE3MzdhNWJhN2U2OCIsIm1vZGUiOiJsaXZlIiwiaWF0IjoxNjA0OTk4NjYyLCJleHAiOjE2MTAxODI2NjJ9.NK-ELjQr18_bwpFLIXiQLeAaahgfqOlPRPw1t3Sccu8",
        "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwYXJ0bmVySWQiOiJkZDE3MmE3Zi0zM2I4LTQwZjQtODE5ZS0zMDg3M2EyZWE0NjIiLCJpYXQiOjE2MDQ5OTg2NjIsImV4cCI6MTYxMDE4MjY2Mn0.iWt11LCIf7l20q9YyjyoP5n08Xs-TPCe99mx5TBC63Q",
        "token_type": "Bearer",
        "expires_in": 1607590662030
    }
}

```
<!-- tabs:end -->

# Investors
The investors API enables you to create and manage investors

## Headers
| Field         	| Required 	| Description                        	|
|---------------	|----------	|------------------------------------	|
| Authorization 	| String   	| Set value to Bearer `access_token` 	|
| Content type  	| String   	| Set value to `application/json`    	|

## Body
``` json
{
    "personal":{
        "title": "Mr.",
        "surname": "Schoon",
        "first_name": "Denji",
        "other_names": "Baton",
        "gender": "M",
        "phone": 1,
        "date_of_birth": "05-21-1978",
        "email_address": "abc@testmenow.com",
        "home_phone": "080334588888"
    },
    "location":{
        "address": "Ajibade Street",
        "country": "Nigeria",
        "state": "Enugu",
        "nationality": "Nigerian",
        "state_of_origin": "Abia",
        "city": "Aba",
        "lga": "Ohafia"
    },
    "financial":{
        "bank_account_number": "3032308890",
        "bank_account_name": "Humpty Groot",
        "bank_name": "First Bank of Nigeria",
        "bank_code": "011",
        "bvn": "22310000101"
    },
    "employment":{
        "company_name": "Not Applicable",
        "employment_type": "Employee",
        "occupation": "worker"
    },
    "kyc":{
        "identity_type": "International Passport",
        "identity_number": "1122004456",
        "expiry_date": "02-01-2030",
        "politically_exposed": "No"
    },
    "next_of_kin":{
        "name": "Abba Moro",
        "address": "Somewhere in Lag",
        "email": "shaq@yahoo.com",
        "phone_number": "09012345678",
        "relationship": "brother"
    }
}
```
<!-- tabs:start -->

#### ** cURL **

``` bash
curl -X POST https://api.staging.storm.trium.ng/partners/investors 
-d "@body.json" 
-H "Authorization:Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwYXJ0bmVySWQiOiJkZDE3MmE3Zi0zM2I4LTQwZjQtODE5ZS0zMDg3M2EyZWE0NjIiLCJpZCI6ImY1ZTg3OGVhLTA4MjEtNDZmYS04YjYxLTE3MzdhNWJhN2U2OCIsIm1vZGUiOiJsaXZlIiwiaWF0IjoxNjA0OTk4NjYyLCJleHAiOjE2MTAxODI2NjJ9.NK-ELjQr18_bwpFLIXiQLeAaahgfqOlPRPw1t3Sccu8" 
-H "Content-Type:application/json"
```
#### ** node.js **

``` javascript
var request = require('request');
var options = {
  'method': 'POST',
  'url': 'https://api.staging.storm.trium.ng/partners/investors',
  'headers': {
    'authorization': 'Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwYXJ0bmVySWQiOiJkZDE3MmE3Zi0zM2I4LTQwZjQtODE5ZS0zMDg3M2EyZWE0NjIiLCJpZCI6ImY1ZTg3OGVhLTA4MjEtNDZmYS04YjYxLTE3MzdhNWJhN2U2OCIsIm1vZGUiOiJsaXZlIiwiaWF0IjoxNjA0OTk4NjYyLCJleHAiOjE2MTAxODI2NjJ9.NK-ELjQr18_bwpFLIXiQLeAaahgfqOlPRPw1t3Sccu8'
  },
  body: "{\n    \"personal\":{\n\"title\":\"Mr.\",\n\"surname\":\"Tosin\",\n\"first_name\": \"Schoon\",\n\"other_names\": \"Stoom\",\n        \"gender\": \"M\",\n        \"phone\": \"07061342599\",\n        \"date_of_birth\": \"05-21-1978\",\n        \"email_address\": \"testcosec8@sharklasers.com\",\n        \"home_phone\": \"080334551761\"\n    },\n    \"location\":{\n        \"address\": \"Test some address\",\n        \"country\": \"Nigeria\",\n        \"state_of_origin\": \"Abia\",\n        \"city\": \"Aba\",\n        \"lga\": \"Ohafia\"\n    },\n    \"financial\":{\n        \"bank_account_number\": \"3032308890\",\n        \"bank_name\": \"First Bank of Nigeria\",\n        \"bvn\": \"22310000111\"\n    },\n    \"employment\":{\n        \"company_name\": \"Not Applicable\",\n        \"employment_type\": \"Employee\",\n        \"occupation\": \"worker\"\n    },\n    \"kyc\":{\n        \"identity_type\": \"International Passport\",\n        \"identity_number\": \"1122004456\",\n        \"expiry_date\": \"04-01-2030\",\n        \"politically_exposed\": \"no\"\n    },\n    \"next_of_kin\":{\n        \"name\": \"Abba Moro\",\n        \"address\": \"Somewhere in Lag\",\n        \"email\": \"shaq@yahoo.com\",\n        \"phone_number\": \"09012345678\",\n        \"relationship\": \"brother\"\n    }\n
};
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});
});
```
#### ** Response **

```json
{
  "status": 201,
  "data": {
    "status": "Pending",
    "investor_no": 1382231,
    "investor_id": "cd3ec9f9-2b17-40dd-9d31-1aae69f295fe"
  }
}

```
<!-- tabs:end -->

