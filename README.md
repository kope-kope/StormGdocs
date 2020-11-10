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

## 