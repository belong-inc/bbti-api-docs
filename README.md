# Belong BBTI API Documentation
This repository contains documentation for developers to use Belong BBTI APIs.

BBTI (Buy-Back, Trade-In) API manages transaction of mobile devices trade-in offering.  
The API has endpoints for device registration, user registration, getting a list of models, calculating prices, registering delivery information, etc.

## About API Disclosure
The BBTI API is currently provided only to trusted partners.

If you are interested in the API please contact us through our [contact page](https://about.belong.co.jp/contact) or our sales representatives.

# Integration Steps Overview

## [1] Get the model information and prices

There are two ways to get the model information and prices.

- API
  - API clients can search for the available the `ListManufacturers`, `ListSeries`, `ListModels`, and `ListPrices` for trade-ins.
  - Please refer to the APIs section for API details.
- CSV
  - A new price list is issued each time a price update is made on the Belong side.
  - See [sampledata/price/price_list_sample.csv](./sampledata/price/price_list_sample.csv) for data details.

## [2] Get the price

- API
  - `ListPrices` API can be used to get the list of prices from the model key information retrieved in [1] and the selected conditions.
  - Please refer to the APIs section for API details.

## [3] Apply trade-in and register the transaction on the Belong side

In this step, you submit the trade-in target information to the Belong side and register the transaction of the trade-in.  
**This step is not necessary if you just want to retrieve prices.**

- Standard API
- AppCode API
  - Linking purchase application information in a pre-defined code format.
  - See the APIs section for API details.

After registering transaction information, an eKYC application, etc. is required to proceed with the trade-in.

# APIs

## Model Information

### ListManufacturers
This endpoint is used to get a list of Manufacturers associated with Models available for BBTI.

```text
GET https://${API_SERVER}/v1/manufacturers
```

### ListSeries
This endpoint is used to get a list of Series associated with Models available for BBTI.

```text
GET https://${API_SERVER}/v1/series
```

### ListModels
This endpoint is used to get a list of Models available for BBTI.

```text
GET https://${API_SERVER}/v1/models
```

## Price Information
This endpoint is used to get model and price information filtered by the specified manufacturer and model etc.

```text
GET https://${API_SERVER}/v1/prices
```

## Pricing

### Standard API
Device registration in BBTI API handles basic information (model, storage, etc.) as well as device conditions (functionality and appearance).
The API calculates a grade of the device based on the condition, then calculate the price of device.

```text
POST https://${API_SERVER}/v1/pricing/products
```

### App code API
Device registration in the BBTI API handles basic information (model, storage, SIM status) and device state (functionality and appearance, etc.) in the form of predefined code formats.
The API calculates a grade of the device based on the condition, then calculate the price of device.

```text
POST https://${API_SERVER}/v1/pricing/codes
```

## eKYC
User registration in BBTI handles user information for the registered device in previous step.
The user registration starts an eKYC process. User needs to complete the eKYC in the provided URL.


```text
POST https://${API_SERVER}/v1/kyc
```

## Delivery
This endpoint is used to register delivery information to BBTI.

```text
POST https://${API_SERVER}/v1/delivery/register
```

## Transaction

This endpoint is used to get the transaction status and status related information from BBTI.

```text
GET https://${API_SERVER}/v1/transactions/{transactionId}
```

## Authentication
Authentication and authorization are performed using the OAuth2 Client Credentials Flow.
API calls are made via the access token obtained above.

### Sample
Bearer Token can be obtained by following command.
Please use the `access_token` included in the response.

```shell
curl -sS \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -H "Authorization: Basic ${CLIENT_AUTH_TOKEN}" \
  --data-urlencode "grant_type=client_credentials" \
  "https://${AUTH_SERVER}/auth/v1/token"
```

Set the extracted token to auth header and send request.

```shell
curl -H "Authorization: Bearer ${API_AUTH_TOKEN}" -X POST \
 -d '{ "test":"payload"}' \
 https://${API_SERVER}/v1/pricing/products
```

## API FAQ

- If transaction information is also managed on the API client side, can it be linked to transactions on the Belong side?
  - Yes. In that case, please set the client side transaction id as `submissionId` in the pricing api (`/pricing/products` or `/pricing/codes`) payload.
  - This allows to search for transactions on the Belong side by transaction ID on the client side.

## Further Information
More API details are described in [here](./swagger) as Swagger document.

The API and docs are expected to be changed/enriched in the future.

# License
This project is licensed under the Apache-2.0 License.
