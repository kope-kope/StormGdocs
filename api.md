> # Before you Start
>[Register as a partner](http://console.staging.storm.trium.ng/settings/api). You will be provided with test API and Secret keys, that you can use to test the endpoints
> You should include `/test ` in the URL when using test keys,for example,`https://api.woven.finance/bi/v1/test/transactions`.

# Example Requests
***
We give sample API calls close to every endpoint using cURL. You simply have to insert your specific values, and you can test the calls from the command line. You can [read this article](https://www.baeldung.com/curl-rest) to learn how to use cURL with APIs.

Not familiar with cURL? You can use [Postman](https://www.postman.com/downloads/). Postman is a collaboration environment for API development including making HTTP requests. Run the [Dart Invest APIs Collection](https://documenter.getpostman.com/view/11930516/TVYF6xeD) in Postman to make testing quicker and easier.

***

# Requests and Responses
***
The request and response bodies are formatted in JSON. The Content-type for all responses is set to `application/JSON`. 
Successful and Error responses are in the following formats:

<!-- tabs:start -->
#### ** Successful **

``` json
{
    "status": [integer],
    "data": [object]
}
```
#### ** Error **

``` json
{
    "status": [integer],
    "errors": [
      "code" : 200x
      "message" : [string]
    ]
}
```
<!-- tabs:end -->

The status key contains the HTTP status code to determine the result of an API call if it was a successful call. Typically, `2xx` indicates that a request was successful.

The data key has the result of a request. Its data type is an object (or an array) depending on the request made. A request to GET an investor's details returns an object in the data key, while a request to GET an investor's portfolio returns an array in the data key.

The error key contains the custom error code defined and the description of the error. Learn more about [Errors](#Errors)
****

# Authentication
***
API requests must be made over HTTPS.
API calls are further authenticated by including your access token in the authorization header of every request.

To generate your access token, call the [Access Token](#Access-Token) endpoint with your API and Secret keys. Generally, access token generated from test keys authenticates the test endpoints, while live endpoints are authenticated with access token generated from live keys.

Authorization headers should be set in this format: `Authorization: Bearer access_token`

> **Sample Authorization Header**
>
> Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwYXJ0bmVySWQiOiJkZDE3MmE3Zi0zM2I4LTQwZjQtODE5ZS0zMDg3M2EyZW
****

# Errors
***
We use standard HTTP response codes for success and failure notifications. In general, 200 HTTP codes correspond to success, 40X codes are for developer- or user-related failures which are further classified with custom error codes, and 50X codes are for Dart Invest-related issues.

#### Sample Error Schema
``` json
{
    "status": [integer],
    "errors": [
      "code" : 200x
      "message" : [string]
    ]
}
```

#### Custom Error Codes

***
<!-- panels:start -->

<!-- div:title-panel -->
## Access Token
***

*POST* `/token`

This endpoint allows you to generate an access token, for authorizing other API calls.

### Authorization

<div class="flat-table"><div class="card table-sm"><div class="card-body">


| Field   | Data type   | Description   |
|-  |-  |-  |
| Username  | String  | Set value to API key  |
| Password  | String  | Set value to Secret key   |

</div></div></div>


### Body Params
<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field   | Data type | Description   |
|-  |-  |-  |
| grant_type  | String  | Set as client_credentials   |
| scope   | String  | Set as profile  |

</div></div></div>

### Examples
<!-- div:right-panel -->
<!-- tabs:start -->

#### ** cURL **
``` bash
curl https://api.woven.finance/bi/v1/token
 --d scope=profile 
 -d grant_type=client_credentials 
 -u 9g3p7e5yp1m4zyk89r1vsz4kk0xh:4c0763b8f583427f9a1380eff9273076
```
#### ** Node **

``` javascript
var request = require('request');
var options = {
  'method': 'POST',
  'url': 'https://api.woven.finance/bi/v1/token',
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

``` json
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

****
<!-- div:title-panel -->
# Investors

***
The investors API endpoints alllows you to create investors, upload the relevant KYC documents and manage investors. The collection also serves important information about the investor including its cash balance and portfolio.

### Create Investor
***
*POST* `/partners/investors`

Create a new investor with a HTTP POST request. After successful creation, the status of the investor is set to `pending` and a UUID = `investorID` is assigned to this investor which will be used for subsequent management.

### Headers

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                         |
|---------------  |---------- |------------------------------------ |
| Authorization   | String    | Set value to Bearer `access_token`  |
| Content type    | String    | Set value to `application/json`|

</div></div></div>     

## Body

``` json
{
    "personal":{
        "title": "Mr.",
        "surname": "Schoon",
        "first_name": "Denji",
        "other_names": "Baton",
        "gender": "M",
        "phone": "08146349233",
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
curl -X POST https://api.woven.finance/bi/v1/partners/investors 
-d "@body.json" 
-H "Authorization:Bearer access_token" 
-H "Content-Type:application/json"
```
#### ** Node **

``` javascript
var request = require('request');
var options = {
  'method': 'POST',
  'url': 'https://api.woven.finance/bi/v1/partners/investors',
  'headers': {
    'authorization': 'Bearer access_token'
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

``` json
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

## Upload KYC
***
*PUT* `/partners/investors/:investor_id/kyc`

Upload investors' KYC documents with this endpoint. The file size limit per upload is set to 10MB. If successful, the status of the investor is updated to `pending_kyc_approval`. 

### Headers

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type | Description                         |
|---------------  |---------- |------------------------------------ |
| Authorization   | String    | Set value to Bearer `access_token`  |

</div></div></div>  

### Body Params

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field 	| File       	| Description                           	|
|-------	|------------	|---------------------------------------	|
| files 	| utility.pdf 	| File(s) must be in .pdf, .jpg or .png 	|

</div></div></div>  

<!-- tabs:start -->

#### ** cURL **

``` bash
curl -X PUT https://api.woven.finance/bi/v1/partners/investors/:investor_id/kyc
-F "files=@utility.pdf" 
-H "Authorization:Bearer access_token"
```
#### ** Node **

``` javascript
var request = require('request');
var fs = require('fs');
var options = {
  'method': 'PUT',
  'url': 'https://api.woven.finance/bi/v1/partners/investors/:investor_id/kyc',
  'headers': {
    'Authorization': 'Bearer {{token}}'
  },
  formData: {
    'files': {
      'value': fs.createReadStream('utility.pdf'),
      'options': {
        'filename': 'utility.pdf',
        'contentType': null
      }
    }
  }
};
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});

```
#### ** Response **

``` json
{
    "status": 200,
    "message": "Uploaded Successfully"
}

```
<!-- tabs:end -->



## Update Investor
***

*PUT* `/partners/investor/:`

A simple HTTP PUT request to the investors' endpoint and you can update the details for an investor within few seconds. However, this will be successful for an `Active` investor

### Headers
<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type | Description                         |
|---------------  |---------- |------------------------------------ |
| Authorization   | String    | Set value to Bearer `access_token`  |
| Content type    | String    | Set value to `application/json`     |

</div></div></div>   

## Body
``` json
{
    "personal":{
        "title": "Mr.",
        "surname": "Humpty",
        "first_name": "Sambat",
        "other_names": "Groot",
        "gender": "M",
        "phone": "08032221112",
        "date_of_birth": "01-15-2000",
        "email_address": "testcosec3@sharklasers.com"
    },
    "location":{
        "address": "some address",
        "nationality": "Nigerian",
        "state": "Enugu",
        "city": "Lagos"
    },
    "financial":{
        "bank_account_number": "3031308890",
        "bank_account_name": "Humpty Groot",
        "bank_code": "011"
    },
    "employment":{
        "company_name": "Not Applicable"
    },
    "next_of_kin":{
        "name": "Abba Moro"
    }
}

```
<!-- tabs:start -->

#### ** cURL **

``` bash
curl -X PUT https://api.woven.finance/bi/v1/partners/investor/:investor_id
-d "@body.json" 
-H "Authorization:Bearer access_token" 
-H "Content-Type:application/json"
```
#### ** Node **

``` javascript
var request = require('request');
var options = {
  'method': 'PUT',
  'url': 'https://api.woven.finance/bi/v1/partners/investor/investor_id',
  'headers': {
    'Authorization': 'Bearer access_token'
  },
  body: "{\n    \"personal\":{\n        \"title\": \"Mr.\",\n        \"surname\": \"Humpty\",\n        \"firstname\": \"Schun\",\n        \"othernames\": \"Groot\",\n        \"gender\": \"M\",\n        \"phone\": \"08032221112\",\n        \"date_of_birth\": \"01-15-2000\",\n        \"email_address\": \"testcosec3@sharklasers.com\"\n    },\n    \"location\":{\n        \"address\": \"some address\",\n        \"country\": \"nigeria\",\n        \"nationality\": \"Nigerian\",\n        \"state\": \"Enugu\",\n        \"city\": \"Lagos\"\n    },\n    \"financial\":{\n        \"bank_account_number\": \"3031308890\",\n        \"bank_account_name\": \"Humpty Groot\",\n        \"bank_code\": \"011\",\n        \"branch_code\": \"001\",\n        \"acc_type\": \"IND\"\n    },\n    \"employment\":{\n        \"companyName\": \"Not Applicable\"\n    },\n    \"next_of_kin\":{\n        \"name\": \"Abba Moro\"\n    }\n}"

};
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});

```
#### ** Response **

``` json
{
  "status": 200,
  "data": {
    "message": "Updated"
  }
}

```
<!-- tabs:end -->

## Change Status 
***
*PUT* `/partners/investor/status/:investor_id`

As a partner, this investor API endpoint allows you to activate or deactivate an investor, set the status to `true` to activate an investor, `false` to deactivate an investor. 

## Headers

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                         |
|---------------  |---------- |------------------------------------ |
| Authorization   | String    | Set value to Bearer `access_token`  |
| Content type    | String    | Set value to `application/json`     |

</div></div></div>  

## Body
``` json
{
    "status": true
}
```
<!-- tabs:start -->

#### ** cURL **

``` bash
curl -X PUT https://api.woven.finance/bi/v1/partners/investor/status/:investor_id
-d "@body.json" 
-H "Authorization:Bearer access_token" 
-H "Content-Type:application/json"
```
#### ** Node **

``` javascript
var request = require('request');
var options = {
  'method': 'PUT',
  'url': 'https://api.woven.finance/bi/v1/partners/investor/status/:investor_id',
  'headers': {
    'Authorization': 'Bearer access_token'
  },
  body: { 
    "status": true
        }

};
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});

```
#### ** Response **

``` json
{
    "status": 200,
    "data": {
        "status": true
    }
}
```
<!-- tabs:end -->

## Get all details
***
*GET* `/partners/investors`

Return information about all your investors

### Headers

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                         |
|---------------  |---------- |------------------------------------ |
| Authorization   | String    | Set value to Bearer `access_token`  |

</div></div></div>  


### Query Params

You can filter the list by the following fields:

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                                                                                           |
|---------------  |-----------  |-----------------------------------------------------------------------------------------------------  |
| status          | String      | Filter investor by status, status can be active, pending and pending_kyc_approval and inactive        |
| name            | String      | Specify surname of the investor                                                                       |
| page_number     | Integer     | Specify the page you want to retrieve, if not specified, we set to a value of 1 by default            |
| page_limit      | Number      | Specify the investor count retrieved per page, if not specified, we set to a value of 25 by default   |
| email           | String      | Specify the email of the investor                                                                     |
| investor_no     | Integer     | Specify the investor number of the investor you want to retrieve                                      |
| created_month   | Datetime    | Specify the month and year for which to start listing investors e.g `11-2020`                         |

</div></div></div>  

<!-- tabs:start -->

#### ** cURL **

``` bash
curl -X GET https://api.woven.finance/bi/v1/partners/investors?created_month=10-2020
-H "Authorization: Bearer access_token"
```
#### ** Node **

``` javascript
var request = require('request');
var options = {
  'method': 'GET',
  'url': 'https://api.woven.finance/bi/v1/partners/investors?created_month=10-2020',
  'headers': {
    'Authorization': 'Bearer access_token'
  }
};
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});

```
#### ** Response **

``` json
{
    "status": 200,
    "data": {
        "pagination": {
            "page_number": 1,
            "page_limit": 25,
            "total": 
        },
        "investors": [
            {
                "id": "dfaa1275-04ad-4ff9-9743-bf7495e4af9d",
                "status": "Active",
                "investor_no": "68339392",
                "registered_date": "2020-11-06T13:07:23.046Z",
                "approved_date": "2020-11-06T13:07:23.045Z",
                "updated_at": "2020-11-10T20:23:55.130Z",
                "personal": {
                    "title": "Mr.",
                    "surname": "Tosin",
                    "first_name": "Schoon",
                    "other_names": "Stoom",
                    "gender": "M",
                    "phone": "07061342599",
                    "date_of_birth": "05-21-1978",
                    "email_address": "testcosec8@sharklasers.com",
                    "home_phone": "080334551761"
                },
                "location": {
                    "address": "Test some address",
                    "country": "Nigeria",
                    "state": null,
                    "state_of_origin": "Abia",
                    "nationality": null,
                    "city": "Aba",
                    "lga": "Ohafia"
                },
                "financial": {
                    "bank_account_number": "3032308890",
                    "bank_account_name": null,
                    "bank_code": null,
                    "branch_code": null,
                    "bank_name": "First Bank of Nigeria",
                    "bvn": "22199952110",
                    "acc_type": null
                },
                "employment": {
                    "company_name": "Not Applicable",
                    "employment_type": "Employee",
                    "occupation": "worker"
                },
                "kyc": {
                    "identity_type": "International Passport",
                    "identity_number": "1122004456",
                    "expiry_date": "04-01-2030"
                },
                "next_of_kin": {
                    "name": "Abba Moro",
                    "address": "Somewhere in Lag",
                    "email": "shaq@yahoo.com",
                    "phone_number": "09012345678",
                    "relationship": "brother"
                }
            },
            {
                "id": "6c7033de-09c2-4da8-ac7e-882bc5910e10",
                "status": "Active",
                "investor_no": "89692072",
                "registered_date": "2020-11-04T15:06:22.272Z",
                "approved_date": "2020-11-04T15:06:22.272Z",
                "updated_at": "2020-11-06T10:42:07.439Z",
                "personal": {
                    "title": "Mr.",
                    "surname": "Tosin",
                    "first_name": "Schoon",
                    "other_names": "Stoom",
                    "gender": "M",
                    "phone": "07061342599",
                    "date_of_birth": "05-21-1978",
                    "email_address": "testcosec8@sharklasers.com",
                    "home_phone": "080334551761"
                },
                "location": {
                    "address": "Test some address",
                    "country": "Nigeria",
                    "state": null,
                    "state_of_origin": "Abia",
                    "nationality": null,
                    "city": "Aba",
                    "lga": "Ohafia"
                },
                "financial": {
                    "bank_account_number": "3032308890",
                    "bank_account_name": null,
                    "bank_code": null,
                    "branch_code": null,
                    "bank_name": "First Bank of Nigeria",
                    "bvn": "22199952131",
                    "acc_type": null
                },
                "employment": {
                    "company_name": "Not Applicable",
                    "employment_type": "Employee",
                    "occupation": "worker"
                },
                "kyc": {
                    "identity_type": "International Passport",
                    "identity_number": "1122004456",
                    "expiry_date": "04-01-2030"
                },
                "next_of_kin": {
                    "name": "Abba Moro",
                    "address": "Somewhere in Lag",
                    "email": "shaq@yahoo.com",
                    "phone_number": "09012345678",
                    "relationship": "brother"
                }
            }
        ]     
    }
}
```
<!-- tabs:end -->

## Get details by ID
***
*GET* `/partners/investors/:investor_id`

Return the details of a specific investor using the investor's id.

### Headers

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                         |
|---------------  |---------- |------------------------------------ |
| Authorization   | String    | Set value to Bearer `access_token`  |

</div></div></div>  

<!-- tabs:start -->

#### ** cURL **

``` bash
curl -X GET https://api.woven.finance/bi/v1/partners/investors/:investor_id
-H "Authorization: Bearer access_token"
```
#### ** Node **

``` javascript
var request = require('request');
var options = {
  'method': 'GET',
  'url': 'https://api.woven.finance/bi/v1/partners/investors/:investor_id',
  'headers': {
    'Authorization': 'Bearer access_token'
  }
};
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});


```
#### ** Response **

``` json
{
  "status": 200,
  "data": {
    "investor": {
      "id": "2612601a-3c3a-4657-a97a-b43990f7b4cc",
      "status": "Inactive",
      "investor_no": "87221380",
      "registered_date": "2020-10-15T15:04:43.488Z",
      "approved_date": "2020-10-15T15:04:43.487Z",
      "updated_at": "2020-10-15T17:02:19.136Z",
      "personal": {
        "title": "Mr.",
        "surname": "Humpty",
        "first_name": "Schoon",
        "other_names": "Stoom",
        "gender": "M",
        "phone": "08032221112",
        "date_of_birth": "01-15-2000",
        "email_address": "testcosec3@sharklasers.com",
        "home_phone": "080334551761"
      },
      "location": {
        "address": "some address",
        "country": "nigeria",
        "state": "Enugu",
        "state_of_origin": "Abia",
        "nationality": "Nigerian",
        "city": "Lagos",
        "lga": "Ohafia"
      },
      "financial": {
        "bank_account_number": "3031308890",
        "bank_account_name": "Humpty Groot",
        "bank_code": "011",
        "branch_code": "001",
        "bank_name": "First Bank of Nigeria",
        "bvn": "22199952109",
        "acc_type": "IND",
        "cscs_number": "0130580168",
        "chn": "C0124923AY",
        "vnuban": "8000892230"
      },
      "employment": {
        "company_name": "Not Applicable",
        "employment_type": "Employee",
        "occupation": "worker"
      },
      "kyc": {
        "identity_type": "International Passport",
        "identity_number": "1122004456",
        "expiry_date": "04-01-2030"
      },
      "next_of_kin": {
        "name": "Abba Moro",
        "address": "Somewhere in Lag",
        "email": "shaq@yahoo.com",
        "phone_number": "09012345678",
        "relationship": "brother"
      }
    }
  }
  ```
  <!-- tabs:end -->

## Get Transactions
***
*GET* `/partners/investors/:investor_id/transactions`

Fetch all transactions placed by a specific investor

### Headers

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                         |
|---------------  |---------- |------------------------------------ |
| Authorization   | String    | Set value to Bearer `access_token`  |

</div></div></div>

### Query Param

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                                                                                           |
|---------------  |-----------  |-----------------------------------------------------------------------------------------------------  |
| start_date    | Datetime            | Date should be in format **YYYY-MM-DD**                                                    |
| end_date    | Datetime            | Date should be in format **YYYY-MM-DD**                                                    |

</div></div></div>

<!-- tabs:start -->

#### ** cURL **

``` bash
curl -X GET https://api.woven.finance/bi/v1/partners/investors/:investor_id/transactions
-H "Authorization: Bearer access_token"
```
#### ** Node **

``` javascript
var request = require('request');
var options = {
  'method': 'GET',
  'url': 'https://api.woven.finance/bi/v1/partners/investors/:investor_id/transactions',
  'headers': {
    'authorization': 'Bearer access_token'
  }
};
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});

});


```
#### ** Response **

``` json
{
    "status": 200,
    "data": {
        "pagination": {
            "page_number": 1,
            "page_limit": 25,
            "total": 1
        },
        "transactions": [
            {
                "id": 301,
                "receipt_ref": "u54x9p1qkjcmcxt4",
                "transaction_ref": "6cf9b5c0-fcd4-49c3-8c67-a89926431d84",
                "investor_id": "cb8dfdde-7b95-4d8a-bc77-34d19c22f7b7",
                "investor_no": "48496885",
                "investor_name": "Oladokun Oluwatosin",
                "status": "open",
                "amount": null,
                "cscs_number": "0130580258",
                "instructions": "",
                "trade_date_limit": "12-31-2020",
                "trade_effective_date": "12-31-2020",
                "trade_price_limit": 0,
                "trade_action": "BUY",
                "stock_code": "WAPIC",
                "trade_units": "19",
                "confirmed_quantity": null,
                "message": null,
                "trade_date": null,
                "created_at": "2020-12-31T09:01:35.657Z",
                "updated_at": "2020-12-31T09:01:38.689Z"
            }
        ]
    }
}
  ```
  <!-- tabs:end -->


## Get Balance
***
*GET* `/partners/investors/:investorID/balances`

Get the cash balance for a specific investor

### Headers

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                         |
|---------------  |---------- |------------------------------------ |
| Authorization   | String    | Set value to Bearer `access_token`  |

</div></div></div>  

### Path Param

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                                                                                           |
|---------------  |-----------  |-----------------------------------------------------------------------------------------------------  |
| investorID        | String            | Specify the desired investor's id                                                             |

### 

</div></div></div>  

<!-- tabs:start -->

#### ** cURL **

``` bash
curl -X GET https://api.woven.finance/bi/v1/partners/investors/:investorID/balances
-H "Authorization: Bearer access_token"
```
#### ** Node **

``` javascript
var request = require('request');
var options = {
  'method': 'GET',
  'url': 'https://api.woven.finance/bi/v1/partners/investors/:investorID/balances',
  'headers': {
    'authorization': 'Bearer access_token'
  }
};
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});

});


```
#### ** Response **

``` json
{
    "status": 200,
    "data": {
        "balances": [
            {
                "cash_balance": "1844.00"
            }
        ]
    }
}
  ```
  <!-- tabs:end -->

 ## Get Portfolio
 ***

 *GET* `/partners/investors/:investorID/portfolio`

Get the details of the portfolio for an investor


### Headers

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                         |
|---------------  |---------- |------------------------------------ |
| Authorization   | String    | Set value to Bearer `access_token`  |

</div></div></div>  

### Field Param

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                                                                                           |
|---------------  |-----------  |-----------------------------------------------------------------------------------------------------  |
| investorID        | String            | Specify the desired investor's id|

</div></div></div>  

<!-- tabs:start -->

#### ** cURL **

``` bash
curl -X GET https://api.woven.finance/bi/v1/partners/investors/:investorID/portfolio
-H "Authorization: Bearer access_token"
```
#### ** Node **

``` javascript
var request = require('request');
var options = {
  'method': 'GET',
  'url': 'https://api.woven.finance/bi/v1/partners/investors/:investorID/portfolio',
  'headers': {
    'authorization': 'Bearer access_token'
  }
};
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});

});


```
#### ** Response **

``` json
{
    "status": 200,
    "data": [
        {
            "symbol": "TESTSTOCK1",
            "qty": "6.0000",
            "most_recent_price": "34.0500",
            "mkt_value": "204.3000"
        },
        {
            "symbol": "TESTSTOCK2",
            "qty": "15000.0000",
            "most_recent_price": "0.4400",
            "mkt_value": "6600.0000"
        }
    ]
}
```
  <!-- tabs:end -->
  ***
## Account Statement
***
*GET* `/partners/investors/:investorId/statement?start_date=2020-05-11&end_date=2021-01-29`

Fetch investor's brokerage account statement within a specified date range

### Headers

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                         |
|---------------  |---------- |------------------------------------ |
| Authorization   | String    | Set value to Bearer `access_token`  |

</div></div></div>

### Query Param

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                                                                                           |
|---------------  |-----------  |-----------------------------------------------------------------------------------------------------  |
| start_date    | Datetime            | Date should be in format **YYYY-MM-DD**                                                    |
| end_date    | Datetime            | Date should be in format **YYYY-MM-DD**                                                    |

</div></div></div>

<!-- tabs:start -->

#### ** cURL **

``` bash
curl -X GET https://api.woven.finance/bi/v1/partners/investors/:investor_id/statement
-H "Authorization: Bearer access_token"
```
#### ** Node **

``` javascript
var request = require('request');
var options = {
  'method': 'GET',
  'url': 'https://api.woven.finance/bi/v1/partners/investors/:investor_id/statement',
  'headers': {
    'authorization': 'Bearer access_token'
  }
};
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});

});


```
#### ** Response **

``` json
{
    "status": 200,
    "data": [
        {
            "txn_date": "2/1/2021 11:43:21 AM",
            "effective_date": "2/1/2021 12:00:00 AM",
            "description": "Stock Purchase: 50 WAPIC @NGN0.59 for A/C 142243",
            "amount": "-34.3402",
            "ledger_type": "NGN",
            "trans_type": "STKBBUY",
            "ref_01": "172245",
            "batch_no": "0",
            "branch_code": "000"
        },
        {
            "txn_date": "2/1/2021 11:41:38 AM",
            "effective_date": "2/1/2021 12:00:00 AM",
            "description": "Stock Purchase: 54 WAPIC @NGN0.59 for A/C 142243",
            "amount": "-36.7434",
            "ledger_type": "NGN",
            "trans_type": "STKBBUY",
            "ref_01": "172264",
            "batch_no": "0",
            "branch_code": "000"
        }
    ]
}
  ```
  <!-- tabs:end -->
  # Market Information
  ***

  Dart Invest's market information API endpoints provides market data from the Nigerian Stock Exchange to your investors get e.g top gainers, top losers, market news, list of all symbols and their prices.

  ## Top Gainers
  ***
  *GET* `/market/news?category=topgainers`

  Get details of the stocks that has gained the most value.

  ### Headers

  <div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                         |
|---------------  |---------- |------------------------------------ |
| Authorization   | String    | Set value to Bearer `access_token`  |

</div></div></div>

### Query Param

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                                                                                           |
|---------------  |-----------  |-----------------------------------------------------------------------------------------------------  |
| category        | String            | Set value to `topgainers`                                                   |

</div></div></div>

<!-- tabs:start -->

#### ** cURL **

``` bash
curl -X GET https://api.woven.finance/bi/v1/market/news?category=topgainers
-H "Authorization:Bearer access_token"
```
#### ** Node **

``` javascript
var request = require('request');
var options = {
  'method': 'GET',
  'url': 'https://api.woven.finance/bi/v1/market/news?category=topgainers',
  'headers': {
    'authorization': 'Bearer access_token'
  }
};
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});

});
```
#### ** Response **

``` json
{
    "status": 200,
    "data": [
        {
            "symbol": "TESTSTOCK1",
            "qty": "6.0000",
            "most_recent_price": "34.0500",
            "mkt_value": "204.3000"
        },
        {
            "symbol": "TESTSTOCK2",
            "qty": "15000.0000",
            "most_recent_price": "0.4400",
            "mkt_value": "6600.0000"
        }
    ]
}
```
  <!-- tabs:end -->

## Top Losers 
***

*GET* `/market/news?category=toplosers`

Get details of the stocks that has lost the most value
  
### Headers

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                         |
|---------------  |---------- |------------------------------------ |
| Authorization   | String    | Set value to Bearer `access_token`  |

</div></div></div>

### Query Param

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                                                                                           |
|---------------  |-----------  |-----------------------------------------------------------------------------------------------------  |
| category        | String            | Set value to `toplosers`                                                    |

</div></div></div>

<!-- tabs:start -->

#### ** cURL **

``` bash
curl -X GET https://api.woven.finance/bi/v1/market/news?category=toplosers
-H "Authorization:Bearer access_token"
```
#### ** Node **

``` javascript
var request = require('request');
var options = {
  'method': 'GET',
  'url': 'https://api.woven.finance/bi/v1/market/news?category=toplosers',
  'headers': {
    'authorization': 'Bearer access_token'
  }
};
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});

});
```
#### ** Response **

``` json
{
    "status": 200,`
    "data": [
        {
            "stock_code": "TESTLOSERS1",
            "name": "LOWER TESTING STOCK",
            "sector": "Insurance Carriers, Brokers and Services",
            "lclose": "0.32"
        },
        {
            "stock_code": "TESTLOSERS2",
            "name": "LEAST TESTING STOCK",
            "sector": "Road Transportation",
            "lclose": "0.22"
        }
    ]
}
```
  <!-- tabs:end -->

## Market News 
***
*GET* `/market/news?category=news`

### Headers

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                         |
|---------------  |---------- |------------------------------------ |
| Authorization   | String    | Set value to Bearer `access_token`  |

</div></div></div>

### Query Param

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                                                                                           |
|---------------  |-----------  |-----------------------------------------------------------------------------------------------------  |
| category        | String            | Set value to `news`                                                   |

</div></div></div>

<!-- tabs:start -->

#### ** cURL **

``` bash
curl -X GET https://api.woven.finance/bi/v1/market/news?category=news
-H "Authorization:Bearer access_token"
```
#### ** Node **

``` javascript
var request = require('request');
var options = {
  'method': 'GET',
  'url': 'https://api.woven.finance/bi/v1/market/news?category=news',
  'headers': {
    'authorization': 'Bearer access_token'
  }
};
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});

});
```
#### ** Response **

``` json
{
    "status": 200,
    "data": [
        {
            "id": "10011",
            "title": "Company set to test Gateway",
            "author": "Vanguard",
            "date": "11/5/2020 4:27:41 PM"
        },
        {
            "id": "10012",
            "title": "Acceptance tests are all acceptable, says Client",
            "author": "Vanguard",
            "date": "11/5/2020 4:01:06 PM"
        }
    ]
}
```
  <!-- tabs:end -->

## Symbols

***
*GET* `/market/symbols?category=trade`

Returns a full list of symbols available to trade via Dart Invest's APIs

### Headers
<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                         |
|---------------  |---------- |------------------------------------ |
| Authorization   | String    | Set value to Bearer `access_token`  |

</div></div></div>

### Query Param
<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                                                                                           |
|---------------  |-----------  |-----------------------------------------------------------------------------------------------------  |
| category        | String            | Set value to `trade`                                                    |

</div></div></div>


<!-- tabs:start -->

#### ** cURL **

``` bash
curl -X GET https://api.woven.finance/bi/v1/market/symbols?category=trade
-H "Authorization:Bearer access_token"
```
#### ** Node **

``` javascript
var request = require('request');
var options = {
  'method': 'GET',
  'url': 'https://api.woven.finance/bi/v1/market/symbols?category=trade',
  'headers': {
    'authorization': 'Bearer access_token'
  }
};
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});

});
```
#### ** Response **

``` json
{
    "status": 200,
    "data": {
        "symbols": [
            {
                "symbol": "TESTSYMBOL1"
            },
            {
                "symbol": "TESTSYMBOL2"
            }
        ]
    }
}
```
  <!-- tabs:end -->

## Price List

***
*GET* `/market/symbols?category=price`

Get the price and other important metrics for all available securities available to trade.

### Headers

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                         |
|---------------  |---------- |------------------------------------ |
| Authorization   | String    | Set value to Bearer `access_token`  |

</div></div></div>

### Query Params

<div class="flat-table"><div class="card table-sm"><div class="card-body">


| Field           | Data type   | Description                                                                                           |
|---------------  |-----------  |-----------------------------------------------------------------------------------------------------  |
| category        | String            | Set value to `price`                                                    |

</div></div></div>

<!-- tabs:start -->

#### ** cURL **

``` bash
curl -X GET https://api.woven.finance/bi/v1/market/symbols?category=price
-H "Authorization:Bearer access_token"
```
#### ** Node **

``` javascript
var request = require('request');
var options = {
  'method': 'GET',
  'url': 'https://api.woven.finance/bi/v1/market/symbols?category=price',
  'headers': {
    'authorization': 'Bearer access_token'
  }
};
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});

});
```
#### ** Response **

``` json
{
    "status": 200,
    "data": [
        {
            "date": "11/5/2020 12:00:00 AM",
            "symbol": "TESTSTOCK1",
            "name": "15.50% ACS JUL 2026",
            "sector": "DEBT",
            "p_close": "100.0000",
            "open": "100.0000",
            "high": "100.0000",
            "low": "100.0000",
            "percentage_spread": "0.0000",
            "close": "100.0000",
            "sign": "+",
            "change": "0.0000",
            "volume": "0",
            "value": "0",
            "percentage_change": "0.00",
            "average_price": "N/A",
            "week_high_52": "100",
            "Week_low_52": "100",
            "earnings_per_share": "",
            "p_e_ratio": "",
            "mkt_segment": "DEBT"
        },
        {
            "date": "11/5/2020 12:00:00 AM",
            "symbol": "TESTSTOCK2",
            "name": "11.25% ADB FEB 2021",
            "sector": "DEBT",
            "p_close": "100.0000",
            "open": "100.0000",
            "high": "100.0000",
            "low": "100.0000",
            "percentage_spread": "0.0000",
            "close": "100.0000",
            "sign": "+",
            "change": "0.0000",
            "volume": "0",
            "value": "0",
            "percentage_change": "0.00",
            "average_price": "N/A",
            "week_high_52": "100",
            "Week_low_52": "100",
            "earnings_per_share": "",
            "p_e_ratio": "",
            "mkt_segment": "DEBT"
        }
    ]
}
```
  <!-- tabs:end -->

***

# Transactions
***
The transactions API endpoints allow you to submit BUY and SELL trade requests at any time, monitor these requests and cancel at will. Once an order is placed, it stays open till it is executed as instructed, these requests can be further queried by the specified transaction reference.

## Create Transaction
***
*POST* `/transactions`

Submit a trade request and specify its properties.

### Headers

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                         |
|---------------  |---------- |------------------------------------ |
| Authorization   | String    | Set value to Bearer `access_token`  |
| Content type    | String    | Set value to `application/json`     |

</div></div></div>  

## Body
``` json
{
  "investor_id": "4d44fa7d-dbf7-4164-af87-156eb9ecdc1f",
  "transaction_ref":"p-00000123",
  "cscs_number": "67393940",
  "instructions": "Test instructions",
  "trade_date_limit": "04-04-2020",
  "trade_effective_date": "05-05-2020",
  "trade_action": "BUY",
  "trade_price_limit": "2.00",
  "trade_units": "2.00",
  "stock_code": "2344555",
  "trade_account_type":"INVESTOR"
}
```

## Body Params

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Fields                | Data type   | Description                                                                                                                                                                                                                                             |
|---------------------- |-----------  |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| investor_id           | String      | Specify the investor id                                                                                                                                                                                                                                 |
| transaction_ref       | String      | Unique string for a transaction request. This will be used to manage the transaction  When on test mode, set as "s-xxxxx" to simulate a successful transaction, "o-xxxxxx" to simulate an open transaction, "p-xxxxx" to simulate a placed transaction  |
| cscs_number           | String      | Specify the investor's CSCS number                                                                                                                                                                                                                      |
| instructions          | String      | Specify instructions                                                                                                                                                                                                                                    |
| trade_date_limit      | Datetime    | This is the date limit an order should stay in the jobbing book before it is cancelled. Specify date in 'YYYY-MM-DD'                                                                                                                                                                                                                          |
| trade_effective_date  | Datetime    |This is the day the investor wants the trade request to be executed. Specify date in 'YYYY-MM-DD'                                                                                                                                                                                                                      |
| trade_action          | String      | Can only be BUY or SELL                                                                                                                                                                                                                                 |
| trade_price_limit     | Float       | Specify price limit in Naira                                                                                                                                                                                                                            |
| trade_units           | Float       | Specify trade units                                                                                                                                                                                                                                     |
| symbol         | String      | Specify the Symbol of the security                                                                                                                                                                                                                      |
| trade_account_type    | String      | Set as INVESTOR                                                                                                                                                                                                                                         |

</div></div></div>

<!-- tabs:start -->

#### ** cURL **

``` bash
curl -X POST https://api.woven.finance/bi/v1/transactions
-d "@body.json" 
-H "Authorization:Bearer access_token" 
-H "Content-Type:application/json"
```
#### ** Node **

``` javascript
var request = require('request');
var options = {
  'method': 'POST',
  'url': 'https://api.woven.finance/bi/v1/transactions',
  'headers': {
    'Authorization': 'Bearer access_token'
  },
  body: "{\n  \"investor_id\": \"{{investor_id}}\",\n  \"transaction_ref\":\"s-00000123\",\n  \"cscs_number\": \"67393940\",\n  \"instructions\": \"Test instructions\",\n  \"trade_date_limit\": \"04-04-2020\",\n  \"trade_effective_date\": \"05-05-2020\",\n  \"trade_action\": \"BUY\",\n  \"trade_price_limit\": \"2.00\",\n  \"trade_units\": \"2.00\",\n  \"stock_code\": \"2344555\",\n  \"trade_account_type\":\"INVESTOR\"\n}"

};
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});

```
#### ** Response **

``` json
{
    "status": 200,
    "message": "success",
    "trade_status": "Success",
    "transaction_ref": "s-00000123"
}

```
<!-- tabs:end -->

## Get Transactions by Date
***

*GET* `/transactions?start_date=01-01-2020&end_date=12-12-2020`

Get the list of transactions initiated within a specified date range

### Headers

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                         |
|---------------  |---------- |------------------------------------ |
| Authorization   | String    | Set value to Bearer `access_token`  |

</div></div></div>

### Query Param

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                                                                                           |
|---------------  |-----------  |-----------------------------------------------------------------------------------------------------  |
| start_date    | Datetime            | Date should be in format **YYYY-MM-DD**                                                    |
| end_date    | Datetime            | Date should be in format **YYYY-MM-DD**                                                    |

</div></div></div>



<!-- tabs:start -->

#### ** cURL **

``` bash
curl -X GET https://api.woven.finance/bi/v1/transactions?start_date=01-01-2020&end_date=12-12-2020
-H "Authorization:Bearer access_token"
```
#### ** Node **

``` javascript
var request = require('request');
var options = {
  'method': 'GET',
  'url': 'https://api.woven.finance/bi/v1//transactions?start_date=01-01-2020&end_date=12-12-2020',
  'headers': {
    'authorization': 'Bearer access_token'
  }
};
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});

});
```
#### ** Response **

``` json
{
    "status": 200,
    "data": {
        "pagination": {
            "page_number": 1,
            "page_limit": 25,
            "total": 2
        },
        "transactions": [
            {
                "id": 31,
                "receipt_ref": "reohah1gkgxu2ew7",
                "transaction_ref": "s-1uMv44LN2saN8DfodJpO",
                "investor_id": "8b31e47a-41a8-4ee9-b18f-b471bb26b6ae",
                "status": "success",
                "amount": 5317.58,
                "cscs_number": "67393940",
                "instructions": "Alaye help me",
                "trade_date_limit": "10-30-2020",
                "trade_effective_date": "10-30-2020",
                "sOrderType": "MARKET",
                "trade_price_limit": 100,
                "trade_action": "BUY",
                "stock_code": "SEPLAT",
                "trade_units": 10,
                "created_at": "2020-10-31T15:21:24.199Z",
                "updated_at": "2020-10-31T15:21:24.199Z"
            },
            {
                "id": 28,
                "receipt_ref": "reohah1gkgqk4fil",
                "transaction_ref": "s-W2hojJ3xwKX2Xsg4z4qu",
                "investor_id": "a26a28c8-9168-401d-85f4-b1a1662d719e",
                "status": "success",
                "amount": 2641.9,
                "cscs_number": "67393940",
                "instructions": "Oga",
                "trade_date_limit": "10-26-2020",
                "trade_effective_date": "11-01-2020",
                "sOrderType": "MARKET",
                "trade_price_limit": 200,
                "trade_action": "SELL",
                "stock_code": "ZENITHBANK",
                "trade_units": 200,
                "created_at": "2020-10-26T13:08:38.926Z",
                "updated_at": "2020-10-26T13:08:38.926Z"
            }
        ]
    }
}
```
  <!-- tabs:end -->

## Get Transaction details
***

*GET* `/transactions?start_date=01-01-2020&end_date=12-12-2020&transaction_ref=s-xZ1wp0KGi0w3XVN8FwNq`

Returns the details of a transaction

### Headers

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                         |
|---------------  |---------- |------------------------------------ |
| Authorization   | String    | Set value to Bearer `access_token`  |

</div></div></div>

### Query Param

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                                                                                           |
|---------------  |-----------  |-----------------------------------------------------------------------------------------------------  |
| start_date    | Datetime            | Date should be in format **YYYY-MM-DD**                                                    |
| end_date    | Datetime            | Date should be in format **YYYY-MM-DD**                                                    |
| transaction_ref     | String            |Specify the desired transaction reference                                                    |

</div></div></div>

<!-- tabs:start -->

#### ** cURL **
``` bash
curl -X GET https://api.woven.finance/bi/v1/transactions?start_date=01-01-2020&end_date=12-12-2020&transaction_ref=s-xZ1wp0KGi0w3XVN8FwNq
-H "Authorization:Bearer access_token"
```
#### ** Node **

``` javascript
var request = require('request');
var options = {
  'method': 'GET',
  'url': 'https://api.woven.finance/bi/v1//transactions?start_date=01-01-2020&end_date=12-12-2020&transaction_ref=s-xZ1wp0KGi0w3XVN8FwNq',
  'headers': {
    'authorization': 'Bearer access_token'
  }
};
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});

});
```
#### ** Response **

``` json
{
    "status": 200,
    "data": {
        "pagination": {
            "page_number": 1,
            "page_limit": 25,
            "total": 1
        },
        "transactions": [
            {
                "id": 31,
                "receipt_ref": "reohah1gkgxu2ew7",
                "transaction_ref": "s-xZ1wp0KGi0w3XVN8FwNq",
                "investor_id": "8b31e47a-41a8-4ee9-b18f-b471bb26b6ae",
                "status": "success",
                "amount": 5317.58,
                "cscs_number": "67393940",
                "instructions": "Alaye help me",
                "trade_date_limit": "10-30-2020",
                "trade_effective_date": "10-30-2020",
                "sOrderType": "MARKET",
                "trade_price_limit": 100,
                "trade_action": "BUY",
                "stock_code": "SEPLAT",
                "trade_units": 10,
                "created_at": "2020-10-31T15:21:24.199Z",
                "updated_at": "2020-10-31T15:21:24.199Z"
            }
        ]
    }
}
```
<!-- tabs:end -->

## Cancel Transactions
***

*GET* `/transactions/:transactionRef`

This endpoint gives you the capability to cancel a trade before it is executed as instructed. 

### Headers

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                         |
|---------------  |---------- |------------------------------------ |
| Authorization   | String    | Set value to Bearer `access_token`  |

</div></div></div>

### Path Param

<div class="flat-table"><div class="card table-sm"><div class="card-body">

| Field           | Data type   | Description                                                                                           |
|---------------  |-----------  |-----------------------------------------------------------------------------------------------------  |
| transactionRef    | String            |Specify the transaction reference to cancel                                                   |

</div></div></div>



<!-- tabs:start -->

#### ** cURL **
``` bash
curl -X DELETE https://api.woven.finance/bi/v1/transactions/:transactionRef
-H "Authorization:Bearer access_token"
```
#### ** Node **

``` javascript
var request = require('request');
var options = {
  'method': 'GET',
  'url': 'https://api.woven.finance/bi/v1/transactions/:transactionRef',
  'headers': {
    'authorization': 'Bearer access_token'
  }
};
request(options, function (error, response) {
  if (error) throw new Error(error);
  console.log(response.body);
});

});
```
#### ** Response **

``` json
{
    "status": 200
}
```
<!-- tabs:end -->