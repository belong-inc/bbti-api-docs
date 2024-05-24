# Belong BBTI API Documentation
This repository contains documentation for developers to use Belong BBTI APIs.

BBTI (Buy-Back, Trade-In) API manages transaction of mobile devices purchase offering.  
The API has endpoints for device registration, user registration, model list fetching, and price calculation.


## About API Disclosure
The BBTI API is currently provided only to trusted partners.

If you are interested in the API please contact us through our [contact page](https://about.belong.co.jp/contact) or our sales representatives.

# APIs

## Transactional API

### Standard API call flow of BBTI is done through two steps:

1. Send device information including the condition so that calculate grade and price.
1. Send user detail to go through eKYC process.

![standard_api_call](./diagrams/standard_api_call/standard_api_call.png)

### AppCode API call flow of BBTI is done through three steps:

1. Get model and price list CSV from bbti.
1. Send device information including the condition so that calculate grade and price.
1. Send user detail to go through eKYC process.

![appcode_api_call](./diagrams/appcode_api_call/appcode_api_call.png)

### Pricing

#### Standard flow
Device registration in BBTI API handles basic information (model, storage, sim status) as well as device conditions (status of water damage, LCD, camera, and e.t.c.).

The API calculate a grade of the device based on the condition, then calculate the price of device.

```text
${API_SERVER}/bbti/v1/pricing/devices
```

#### App code flow
Device registration in the BBTI API handles basic information (model, storage, SIM status) and device state (status of water damage, LCD, camera, etc.) in the form of predefined codes.

The API calculate a grade of the device based on the condition, then calculate the price of device.

```text
${API_SERVER}/bbti/v1/pricing/codes
```

#### User
User registration in BBTI handles user information for the registered device in previous step.

The user registration starts an eKYC process. User needs to complete the eKYC by filling out forms in the provided URL.

```text
${API_SERVER}/bbti/v1/user-kyc
```

#### Event notification
This endpoint is used to notify an event of BBTI such as application completion.


```text
${API_SERVER}/bbti/v1/transactions/{transaction_id}/event
```

#### Delivery
This endpoint is used to register delivery information to BBTI.

```text
${API_SERVER}/bbti/v1/delivery
```

## Pricing Only API (Not Transactional)

### Pricing only API call flow of BBTI is done through four steps:

1. Get the list of manufacturers.
1. Send manufacturer to get the list of series.
1. Send series to get the list of models.
1. Send the information required for pricing and get the price.

![pricing_only](./diagrams/pricing_only/pricing_only.png)

#### ListManufacturers
This endpoint is used to get a list of Manufacturers associated with Models available for BBTI.

```text
${API_SERVER}/gw/bbti/v1/manufacturers
```

#### ListSeries
This endpoint is used to get a list of Series associated with Models available for BBTI.

```text
${API_SERVER}/gw/bbti/v1/manufacturers/{manufacturerKey}/series
```

#### ListModels
This endpoint is used to get a list of Models available for BBTI.

```text
${API_SERVER}/gw/bbti/v1/manufacturers/{manufacturerKey}/series/{seriesKey}/models
```

#### GetPrice
This endpoint returns a price based on the given program_id, model, storage, and grade.

```text
${API_SERVER}/gw/bbti/v1/models/{modelKey}/pricing
```

## API FAQ

- What does `Transactional` mean?
  - `Transactional` means that Belong manages the trade-in information and eKYC status and updates the status.
- How should we use transactional and non-transactional APIs?
  - It is recommended that different API flows be used depending on whether transaction status is managed on the client side or the Belong side.
  - If you only need prices and want to cover eKYC, etc. on the client side, please use the Non-Transactional API.
- If transaction information is also managed on the API client side, can it be linked to transactions on the Belong side?
  - Yes. In that case, please set the client side transaction id as `submission_id` in the pricing api (`/pricing/devices` or `/pricing/codes`) payload.
  - This allows to search for transactions on the Belong side by transaction ID on the client side.

## Authentication
Authentication is done through API key and secret. Once the contract of API usage is settled, we will provide API key and secret.

API call is done through Bearer Token which can be obtained via API key and secret. 

### Sample
Bearer Token can be obtained by following command.

```shell
curl -H "Authorization: Basic ${CLIENT_AUTH_TOKEN}"  \
   -sS "${AUTH_SERVER}/auth/v1/token"
```

Set the extracted token to auth header and send request.

```shell
curl -H "Authorization: Bearer ${API_AUTH_TOKEN}" -X POST \
 -d '{ "test":"payload"}' \
 ${API_SERVER}/bbti/v1/pricing/devices
```

## Further Information.
More API details are described in [here](./sampledata/swagger) as Swagger document.

The API and docs is expected to be changed/enriched in the future.

# License
This project is licensed under the Apache-2.0 License.
