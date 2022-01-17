title: Web3-like library in page
---

# Web3-like library request API based on EVERIP-32 (formerly TIP-32) standard

The wallet browser extension allows interaction with DApp based on EVERIP-32 (formerly TIP-32) standard. Full specification places [here](https://docs.google.com/document/u/1/d/1NVSznTRUEZ53rj8je9xdXZbcyHIybURlcGtCQAoHhuos).

**Restrictions**
- DApp can't open more than 15 dialogs simultaneously
- Each request has a life time. If the user won't be able to confirm or decline the request, after 5 minutes DApp will receive the timeout error
- Some method can't be invoked by scheme *ever_MODULE_METHOD*. For example, `ever_net_subscribe_collection` to avoid cases when DApp can do a huge subscription number and force to hang a user web-browser
- DApp can't create more than 5 subscriptions simultaneously

DApp can run EVERSCALE SDK method directly by the scheme, that is described in EVERIP-32 (formerly TIP-32):

```js
  window.everscale
    .request({
      method: "ever_MODULE_METHOD",
      params: {}
    }).then((result) => {
       console.log(result);
    })
```

For example:

```js
  window.everscale
    .request({
      method: "ever_abi_decode_message",
      params: {abi: abiContract(HelloEventsContract.abi), message: boc}
    })
    .then((decoded) => {
       console.log(`New event ${decoded.name} with type ${decoded.body_type}:`, decoded.value);
    })
```

# Demo page

All methods EVERSCALE SDK and methods from specification EVERIP-32 (formerly TIP-32) are available for the testing on [demo page](https://everip32-demopage.pertinaxwallet.com/)

# The most frequently used methods places below:

## wallet_getSdkVersion

DApp can get SDK version that uses by the wallet browser extension.

use private keys - **no**
must be allowed - **no**
required parameters - **no**

```js
  window.everscale
    .request({
      method: "wallet_getSdkVersion",
      params: {},
    })
    .then((result) => {
      console.log(result);
      //
    })
    .catch((error) => {
      console.log(error);
    });
```

## wallet_requestPermissions
DApp can request needed permissions from user. Not all methods demands to be permitted. The all methods, that must be allowed by user, lists below:
- [ever_account](#ever-account)
- [ever_endpoint](#ever-endpoint)
- [ever_sendTransaction](#ever-sendTransaction)
- [ever_signMessage](#ever-signMessage)
- [ever_getNaclBoxPublicKey](#ever-getNaclBoxPublicKey)
- [ever_getSignature](#ever-getSignature)
- [ever_encryptMessage](#ever-encryptMessage)
- [ever_decryptMessage](#ever-decryptMessage)
- [ever_subscribe](#ever-subscribe)
- [ever_unsubscribe](#ever-unsubscribe)

use private keys - **no**
must be allowed - **no**
required parameters - **permissions as array**

```js
  window.everscale
    .request({
      method: "wallet_requestPermissions",
      params: {"permissions": ["ever_account", "ever_endpoint", "ever_sendTransaction", "ever_signMessage", "ever_getSignature", "ever_subscribe"]},
    })
    .then((result) => {
      console.log(result);
      //
    })
    .catch((error) => {
      console.log(error);
    });
```

## wallet_getPermissions
DApp can request the permissions list granted by user

use private keys - **no**
must be allowed - **no**
required parameters - **no**

```js
  window.everscale
    .request({
      method: "wallet_getPermissions",
      params: {},
    })
    .then((result) => {
      console.log(result);
      //
    })
    .catch((error) => {
      console.log(error);
    });
```

## ever_account
DApp can request the selected account

use private keys - **no**
must be allowed - **yes**
required parameters - **no**

```js
  window.everscale
    .request({
      method: "ever_account",
      params: {},
    })
    .then((result) => {
      console.log(result);
      //
    })
    .catch((error) => {
      console.log(error);
    });
```

## ever_endpoint
DApp can request the selected endpoint

use private keys - **no**
must be allowed - **yes**
required parameters - **no**

```js
  window.everscale
    .request({
      method: "ever_endpoint",
      params: {},
    })
    .then((result) => {
      console.log(result);
      //
    })
    .catch((error) => {
      console.log(error);
    });
```

## ever_sendTransaction
DApp can initialize the transaction dialog

use private keys - **yes**
must be allowed - **yes**
required parameters - **destination as string, amount as number, message as string**

```js
  window.everscale
    .request({
      method: "ever_sendTransaction",
      params: {"destination": "-1:3333333333333333333333333333333333333333333333333333333333333333", "amount": 1, "message": "test"},
    })
    .then((result) => {
      console.log(result);
      //
    })
    .catch((error) => {
      console.log(error);
    });
```

## ever_signMessage
DApp can initialize the signing message dialog

use private keys - **yes**
must be allowed - **yes**
required parameters - **data as string**

```js
  window.everscale
    .request({
      method: "ever_signMessage",
      params: {"data": ""},
    })
    .then((result) => {
      console.log(result);
      //
    })
    .catch((error) => {
      console.log(error);
    });
```

## ever_getNaclBoxPublicKey
DApp can obtain the public key for ever_encryptMessage method

use private keys - **yes**
must be allowed - **yes**
required parameters - **no**

```js
  window.everscale
    .request({
      method: "ever_getNaclBoxPublicKey",
      params: {},
    })
    .then((result) => {
      console.log(result);
      //
    })
    .catch((error) => {
      console.log(error);
    });
```

## ever_getSignature
DApp can initialize the dialog for obtaining user signature

use private keys - **yes**
must be allowed - **yes**
required parameters - **data as string**

```js
  window.everscale
    .request({
      method: "ever_getSignature",
      params: {"data": "It is my signature"},
    })
    .then((result) => {
      console.log(result);
      //
    })
    .catch((error) => {
      console.log(error);
    });
```

## ever_crypto_generate_random_bytes
DApp can run generate_random_bytes method from crypto module

use private keys - **no**
must be allowed - **no**
required parameters - **length as number**

```js
  window.everscale
    .request({
      method: "ever_crypto_generate_random_bytes",
      params: {"length": 24},
    })
    .then((result) => {
      console.log(result);
      //
    })
    .catch((error) => {
      console.log(error);
    });
```

## ever_encryptMessage
DApp can initialize the encrypting message dialog

**Nonce** can be obtained from `ever_crypto_generate_random_bytes` method with length 24, **their_public** can be obtained from `ever_getNaclBoxPublicKey` (the Nacl public recipient key)

use private keys - **yes**
must be allowed - **yes**
required parameters - **decrypted as string, nonce as string, their_public as string**

```js
  window.everscale
    .request({
      method: "ever_encryptMessage",
      params: {"decrypted": "test", "nonce": "b6306029c60f32d739ba1804c4d719eb334c9bd5b4e3d2d8", "their_public": "9950d2f1a3cee9fcbc6614aba64636215c31edee31061de77c93d2fa62a67732"},
    })
    .then((result) => {
      console.log(result);
      //
    })
    .catch((error) {
      console.log(error);
    });
```

## ever_decryptMessage
DApp can initialize the decrypting message dialog

**Nonce** must be the same as for `ever_encryptMessage` method, **their_public** can be obtained from `ever_getNaclBoxPublicKey` (the Nacl public sender key)

use private keys - **yes**
must be allowed - **yes**
required parameters - **encrypted as string, nonce as string, their_public as string**

```js
  window.everscale
    .request({
      method: "ever_decryptMessage",
      params: {"encrypted": "hKiMqdgBHkQdvFiQaE0TT/X/0Zg=", "nonce": "b6306029c60f32d739ba1804c4d719eb334c9bd5b4e3d2d8", "their_public": "b8e902c15096bf030fc8a0b3549bca15ca4bc74c3612964a72d93a2b00420308"},
    })
    .then((result) => {
      console.log(result);
      //
    })
    .catch((error) => {
      console.log(error);
    });
```

## ever_subscribe
DApp can subscribe on blockchain events

use private keys - **no**
must be allowed - **yes**
required parameters - **collection as string, filter as object, result as string**

```js
  window.everscale
    .request({
      method: "ever_subscribe",
      params: {
              collection: "messages",
              filter: {
                  src: {
                      eq: "-1:67f4bf95722e1bd6df845fca7991e5e7128ce4a6d25f6d4ef027d139a11a7964",
                  },
                  msg_type:{ eq: 2 }
              },
              result: "boc",
      }
    })
    .then((subscriptionId) => {
      window.everscale.on("message", (message) => {
        if (message.type === "ever_subscription") {
          const { data } = message;
          if (data.subscription === subscriptionId) {
            if ("result" in data && typeof data.result === "object") {
              const boc = data.result;
              window.everscale
                .request({
                  method: "ever_abi_decode_message",
                  params: {abi: abiContract(HelloEventsContract.abi), message: boc}
                })
                .then((decoded) => {
              	   console.log(`New event ${decoded.name} with type ${decoded.body_type}:`, decoded.value);
                })
            } else {
              console.error(`Something went wrong: ${data.result}`);
            }
          }
        }
      });
    })
    .catch((error) => {
      console.error(
        `Error making events subscription: ${error.message}.
         Code: ${error.code}. Data: ${error.data}`
      );
    });
```

## ever_unsubscribe
DApp can unsubscribe on blockchain events

use private keys - **no**
must be allowed - **yes**
required parameters - **handle as number**

```js
  window.everscale
    .request({
      method: "ever_unsubscribe",
      params: {
        handle: 1
      }
    })
    .then((result) => {
      console.log(result);
    });
```
