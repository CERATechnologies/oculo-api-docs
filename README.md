# The Oculo API: Integrating with the Oculo platform

Draft 1.0.2

## Overview

Integrating with the Oculo API for the purpose of sending clinical communications.

## Prerequisites

Providers will be set up in Oculo with active roles for their practice(s). 

Integrations will be set up in Oculo for each of the practices the provider wishes to use.

## High level overview

The provider initiates an Oculo referral request from their clinical information system (CIS).

Using the provider's access key and secret key, the CIS requests a time-based auth token. Successful authentication returns a time based (2 hours) API token to be used in subsequent requests.

The CIS then sends an HTTPS POST request to Oculo containing the patient and clinical information pertinent to the referral.
Oculo returns two URLs to reference this referral - one to query the status of the referral, the second to open it for the user.

If possible, the CIS has the ability to open a browser with this second URL and the user can complete the referral process in Oculo. Alternatively the provider can log in to Oculo, find the draft and complete it.


## Using the API

The following examples will be given using curl. Please note that the examples also use https://master.oculo.com.au which is the Oculo test environment. The production environment is found at https://oculo.com.au

The detailed API documentation can be found at: [http://swagger.oculo.com.au](http://swagger.oculo.com.au)

#### Acquiring a Security Token

Using your ```Access Key``` and ```Secret Key``` you can now get a temporary API token to begin interacting with the Oculo platform. The ```client_id``` for the integration will be provided prior to testing.

###### Example request
``` sh
curl \
	-d access_key='GyT1HwkPkMnZFwnPeN5B' \
	-d secret_key='zbQWxFDjsqErPsP8ybawVtk_jFW_tFSz' \
	-d client_id=OCULO_TEST \
	https://master.oculo.com.au/api/v1/auth
```

###### Example response
``` JSON
{
 "auth_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhY2Nlc3Nfa2V5IjoiR3lUMUh3a1BrTW5aRnduUGVONUIiLCJleHBpcmVzIjoiMjAxNi0xMS0xMSAwMjoxNTowMCBVVEMifQ.A8ANIiAdrM1RroYXzvk15YFPXWU-zU0LEgOCHoPFkpI",
   "expires_in":"2016-11-11T02:15:00.513Z",
   "client_id":"TEST_SYSTEM"
}
```

The auth_token returned will be used in all future API requests.



#### Sending a Referral Request

###### Example request
``` sh
curl 
	-H "Accept: application/json" \
	-H "Content-type: application/json" \
	-H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhY2Nlc3Nfa2V5IjoiR3lUMUh3a1BrTW5aRnduUGVONUIiLCJleHBpcmVzIjoiMjAxNi0xMS0xMSAwMjoxNTowMCBVVEMifQ.A8ANIiAdrM1RroYXzvk15YFPXWU-zU0LEgOCHoPFkpI" \
	-d '{"referral_request": {"sender_system":"TEST_SYSTEM", "message": "referral message", "patient_request": {"first_name": "Susan", "surname": "Little", "title": "Ms", "sex": "female","street_address": "1 Foo St", "suburb": "Melbourne" }}}' \
	https://master.oculo.com.au/api/v1/referral_requests
```

###### Example response
``` JSON
{
   "id":"30c8e93b-3f7c-4780-9634-5b650d474bdd",
   "referral_request":{
      "sender_system":"TEST_SYSTEM",
      "message":"referral message",
      "patient_request":{
         "first_name":"Susan",
         "surname":"Little",
         "title":"Ms",
         "sex":"female",
         "street_address":"1 Foo St",
         "suburb":"Melbourne"
      }
   },
   "_links":{
      "self":{
         "href":"https://master.oculo.com.au/api/v1/referral_requests/30c8e93b-3f7c-4780-9634-5b650d474bdd"
      },
      "edit_referral_request":{
         "href":"https://master.oculo.com.au/referral_requests/30c8e93b-3f7c-4780-9634-5b650d474bdd"
      }
   }
}
```

**Important: The response includes 2 URLs.The second, ```edit_referral_request``` is used to continue the referral process in Oculo.**

#### The current state of the Referral Request.

It is possible to check the status of the communication in Oculo by sending a GET request to the URL provided in the ```_links -> self -> href``` section of the response above.

###### Example request
``` sh
curl 
	-H "Accept: application/json" \
	-H "Content-type: application/json" \
	-H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhY2Nlc3Nfa2V5IjoiWFZZQWItdmpqR01NN1p5R3hfWDQiLCJleHBpcmVzIjoiMjAxNi0xMS0xNSAwNTo0NzoyNiBVVEMifQ.fzU5PedoCaKEWiwcQDwR0CTym-Y7oPpmkWsLmiJMLDo" \
	https://master.oculo.com.au/api/v1/referral_requests/30c8e93b-3f7c-4780-9634-5b650d474bdd
```

###### Example response
``` JSON
{
   "id":"30c8e93b-3f7c-4780-9634-5b650d474bdd",
   "referral_request":{
      "sender_system":"TEST_SYSTEM",
      "message":"referral message",
      "patient_request":{
         "first_name":"Susan",
         "surname":"Little",
         "title":"Ms",
         "sex":"female",
         "street_address":"1 Foo St",
         "suburb":"Melbourne"
      }
   },
   "_links":{
      "self":{
         "href":"https://master.oculo.com.au/api/v1/referral_requests/30c8e93b-3f7c-4780-9634-5b650d474bdd"
      },
      "edit_referral_request":{
         "href":"https://master.oculo.com.au/referral_requests/30c8e93b-3f7c-4780-9634-5b650d474bdd"
      }
   }
}
```

#### Continuing the Referral process in Oculo

Using the ```edit_referral_request -> href``` link, log into oculo and continue processing the referral.

You have the option of selecting a matching patient if they match an existing patient, or creating a new patient. Once complete, continue modifying the Oculo Referral as normal.

