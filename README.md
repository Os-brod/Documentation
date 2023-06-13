# Crypto Swap API Documentation

**Base URL**: `https://www.cryptofuse.net/crypto_swap/api/`

## Endpoints

### 1. Get Available Cryptocurrencies

* **Endpoint:** `available_cryptos/`
* **Method:** `POST`
* **Rate Limit:** 10 requests per minute
* **Request Body:** None
* **Response:** JSON object with available cryptocurrencies

```javascript
async function fetchAvailableCryptos() {
    try {
        const response = await fetch("https://www.cryptofuse.net/crypto_swap/api/available_cryptos/", {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
            },
        });
        
        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error('Error fetching available cryptos:', error);
    }
}

fetchAvailableCryptos();
```

```
{
'available_cryptos': 
    [
    ['NANO', 'Nano'], 
    ['BAN', 'Banano'], 
    ['SOL', 'Solana']
    ]
}
```


### 2. Get Exchange Rate

* **Endpoint:** `get_exchange_rate/`
* **Method:** `POST`
* **Rate Limit:** 12 requests per minute
* **Request Body:** JSON object with `from_crypto` and `to_crypto`
* **Response:** JSON object with the exchange rate

```javascript
async function fetchExchangeRate(fromCrypto, toCrypto) {
    try {
        const response = await fetch("https://www.cryptofuse.net/crypto_swap/api/get_exchange_rate/", {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({
                from_crypto: fromCrypto,
                to_crypto: toCrypto,
            }),
        });
        
        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error('Error fetching exchange rate:', error);
    }
}

fetchExchangeRate('NANO', 'BAN');
```

```
{'exchange_rate': '0.00644087981495262853616993048285242972'}
```

### 3. Generate Address

* **Endpoint:** `generate_address/`
* **Method:** `POST`
* **Rate Limit:** 3 requests per minute
* **Request Body:** JSON object with `from_crypto`, `to_crypto`, `user_address`, and optional `referral`
* **Response:** JSON object with deposit instructions and generated ID

```javascript
async function generateAddress(fromCrypto, toCrypto, userAddress, referral) {
    try {
        const response = await fetch(`https://www.cryptofuse.net/crypto_swap/api/generate_address/`, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({
                from_crypto: fromCrypto,
                to_crypto: toCrypto,
                user_address: userAddress,
                referral: referral
            }),
        });
        
        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error('Error generating address:', error);
    }
}

generateAddress('BAN', 'NANO', 'userAddress', 'referralCode');

```
```
{
'success': 'Deposit to BAN address: ban_3ewf7er5xegujz5a7fzzzfykkzwtp7eedc679b8wuaaiu8fksyzmgmctwrxn', '
id': 'ba2525a7-59b7-4465-b8e2-591d6daaaaf6', 
'transaction_url': 'https://www.cryptofuse.net/crypto_swap/address/ba2525a7-59b7-4465-b8e2-591d6daaaaf6/'
}
```

### 4. Register Referral

* **Endpoint:** `register_referral/`
* **Method:** `POST`
* **Rate Limit:** 3 requests per minute
* **Request Body:** JSON object with `to_crypto`, `user_address`, and `referral`
* **Response:** JSON object with successful registration message
 
```javascript
async function registerReferral(toCrypto, userAddress, referral) {
    try {
        const response = await fetch(`https://www.cryptofuse.net/crypto_swap/api/register_referral/`, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({
                to_crypto: toCrypto,
                user_address: userAddress,
                referral: referral
            }),
        });
        
        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error('Error registering referral:', error);
    }
}

registerReferral('BAN', 'userAddress', 'referralCode');
```
```
{'success': 'Referral registered successfully. Your referral link is: cryptofuse.net?ref=referralCode'}
```

### 5. Get Address by UUID

* **Endpoint:** `get_address_by_uuid/<uuid:uuid>/`
* **Method:** `GET`
* **Rate Limit:** 3 requests per minute
* **Request Params:** `uuid`
* **Response:** JSON object with the address corresponding to the given UUID

```javascript
async function getAddressByUUID(uuid) {
    try {
        const response = await fetch(`https://www.cryptofuse.net/crypto_swap/api/get_address_by_uuid/${uuid}/`, {
            method: 'GET',
            headers: {
                'Content-Type': 'application/json',
            }
        });
        
        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error('Error getting address by UUID:', error);
    }
}

const uuid = 'f83c6733-2a27-4e7c-b6e7-f3fd01ee4ab3'; //sample uuid

getAddressByUUID(uuid);
```

```
{
'success': 'Successfully retrieved address by uuid', 
'our_address': 'nano_3nbdsbhaysadgr6xzgy4zw111ebraubo3z18bppqn97cjnzsnuciy4f98ek1', 
'from_crypto': 'NANO', 
'to_crypto': 'BAN', 
'qr_code': 'iVBORAAAAA5CYII='
}
```


### 6. Get Latest Transaction

* **Endpoint:** `get_latest_transaction/`
* **Method:** `POST`
* **Request Body:** JSON object with `uuid`. Optionally, you can also include latest_known_transaction_id and latest_known_transaction_status.
* **Response:** JSON object with the details of the latest transaction


#### Usage
To use this endpoint, you should first call it with only the uuid parameter. The server will respond with details of the latest transaction.

After you've made the initial call, you can make additional calls by including the latest_known_transaction_id and latest_known_transaction_status parameters. This will let the server know the details of the last known transaction, and the server will respond only if there has been a change in the transaction status. This will result in less rate limits.

```javascript
async function getLatestTransaction(uuid, latestKnownTransactionId, latestKnownTransactionStatus) {
    try {
        const response = await fetch(`https://cryptofuse.net/crypto_swap/api/get_latest_transaction/`, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify({
                uuid: uuid,
                latest_known_transaction_id: latestKnownTransactionId,
                latest_known_transaction_status: latestKnownTransactionStatus
            }),
        });
        
        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error('Error getting the latest transaction:', error);
    }
}

sample_uuid = 'f83c6733-2a27-4e7c-b6e7-f3fd01ee4ab3'; //sample uuid

getLatestTransaction(sample_uuid, 'sample-transaction-id', 'sample-status');
```


```
{
'success': 'Found latest transaction', 
'new_transaction': True, 
'transaction': {
    'id': 'ec5c1f31-5795-4655-8e24-9b276cf6ba36', 
    'sending_amount': '1.5514731988', 
    'received_amount': '0.0100000000', 
    'from_crypto': 'NANO', 
    'to_crypto': 'BAN', 
    'status': 'COMPLETED', 
    'created_at': '2023-05-26T14: 12: 40.664000+00: 00', 
    'from_hash': '0C6881858D1E76C4FFDE47D08DBBECF8C04746FD5EBA886ABDEF5D1E134DD7E0',
    'to_hash': '92C491791FCC0653D83DC17EE42FB9AF74785E1A240114CA097C83B0A0851036', 
    'user_address': 'ban_1em9ej7jdcmuhwhwydwohb63xf5bjajzob5hp4x9fzzyhants5jckn684e3f'
    }
}
```
#### Now inputting id and status back into the function, and the transaction was not updated:

```
{'new_transaction': False}
```


## Error Handling

* All endpoints use a generic error handler to return a JSON object with an error message when something goes wrong.

## Note

* All requests are protected by IP-based rate limiting. Exceeding the limit will result in a block.
* The `POST` method must be used with all requests except for the `get_address_by_uuid` endpoint, which uses `GET`.

Please ensure your request payloads are structured correctly and that all required data is included. If you are unsure of the proper formatting or if you encounter an error, please refer to the specific endpoint details above.

Always check the HTTP status code and message included in the server response to confirm the result of your request. Successful requests will return a 200 status code, while unsuccessful requests will return an error code and message.

## Status Codes

* `200`: Success
* `301`: Moved permanently (Missing www?)
* `400`: Bad Request (Client Error)
* `404`: Not Found
* `429`: Too Many Requests (Rate Limit Exceeded)
