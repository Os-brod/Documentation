# Crypto Swap API Documentation

**Base URL**: `https://yourwebsite.com/api/v1/`

## Endpoints

### 1. Get Available Cryptocurrencies

* **Endpoint:** `available_cryptos/`
* **Method:** `POST`
* **Rate Limit:** 10 requests per minute
* **Request Body:** None
* **Response:** JSON object with available cryptocurrencies

### 2. Get Exchange Rate

* **Endpoint:** `get_exchange_rate/`
* **Method:** `POST`
* **Rate Limit:** 12 requests per minute
* **Request Body:** JSON object with `from_crypto` and `to_crypto`
* **Response:** JSON object with the exchange rate

### 3. Generate Address

* **Endpoint:** `generate_address/`
* **Method:** `POST`
* **Rate Limit:** 3 requests per minute
* **Request Body:** JSON object with `from_crypto`, `to_crypto`, `user_address`, and optional `referral`
* **Response:** JSON object with deposit instructions and generated ID

### 4. Register Referral

* **Endpoint:** `register_referral/`
* **Method:** `POST`
* **Rate Limit:** 3 requests per minute
* **Request Body:** JSON object with `to_crypto`, `user_address`, and `referral`
* **Response:** JSON object with successful registration message

### 5. Get Address by UUID

* **Endpoint:** `get_address_by_uuid/<uuid:uuid>/`
* **Method:** `GET`
* **Rate Limit:** 3 requests per minute
* **Request Params:** `uuid`
* **Response:** JSON object with the address corresponding to the given UUID

### 6. Get Latest Transaction

* **Endpoint:** `get_latest_transaction/`
* **Method:** `POST`
* **Request Body:** JSON object with `uuid`, `latest_known_transaction_id`, and `latest_known_transaction_status`
* **Response:** JSON object with the details of the latest transaction

## Error Handling

* All endpoints use a generic error handler to return a JSON object with an error message when something goes wrong.

## Note

* All requests are protected by IP-based rate limiting. Exceeding the limit will result in a block.
* The `POST` method must be used with all requests except for the `get_address_by_uuid` endpoint, which uses `GET`.

Please ensure your request payloads are structured correctly and that all required data is included. If you are unsure of the proper formatting or if you encounter an error, please refer to the specific endpoint details above.

Always check the HTTP status code and message included in the server response to confirm the result of your request. Successful requests will return a 200 status code, while unsuccessful requests will return an error code and message.

## Status Codes

* `200`: Success
* `400`: Bad Request (Client Error)
* `404`: Not Found
* `429`: Too Many Requests (Rate Limit Exceeded)