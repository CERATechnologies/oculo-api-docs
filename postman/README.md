# Oculo API Postman Collections

[Oculo](http://http://www.connect.oculo.com.au/) is a secure cloud based network designed especially for eye care practitioners to share clinical information, referrals and other clinical correspondence between eye care professionals. Oculo provides an [API](http://swagger.oculo.com.au) for creating referrals.

[Postman](https://www.getpostman.com/) is a GUI HTTP client. You can use it to chain requests by passing through API auth tokens and other handy things.

This project contains Postman collections that can be used to interact with the Oculo API.

## Installation
- Download [the Postman client](https://www.getpostman.com/)
- Import `Oculo API.postman_collection.json` via **File > Import ...**
- If one hasn't been created for you already, create an integration within Oculo and note the `client_id`, `access_key` and `secret_key`
- Import an environment via **File > Import ...** (create your own from `Sample.postman_environment.json` or use what has been provided to you)
- If required edit the environment in Postman and add your credentials via **(Gear Icon) > Manage Environments**

## Performing requests
- Start with an `Authorise` request; this will populate the `auth_token` global variable that will be used by subsequent requests
