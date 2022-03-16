title: Web3-подобная библиотека на веб-странице
---

# Web3-подобная библиотека запросов API основанная на TIP-32 стандарте

Веб-расширение позволяет взаимодействовать с DApp (децентрализованное приложение) на основе TIP-32 стандарта. Полная спецификация доступна [здесь](https://docs.google.com/document/u/1/d/1NVSznTRUEZ53rj8je9xdXZbcyHIybURlcGtCQAoHhuo).

**Ограничения**
- DApp не может открывать более 15 диалогов запросов одновременно
- Каждый запрос имеет время жизни. Если пользователь не будет способен подтвердить или отклонить запрос, то после 5 минут DApp получит ошибку таймаута
- Некоторые методы не могут быть выполнены по схеме *ever_MODULE_METHOD*. Для примера, `ever_net_subscribe_collection` чтобы избежать случаев, когда DApp может делать большое количество подписок и заставить зависнуть браузер пользователя
- DApp не может создать более 5 подписок одновременно

DApp может выполнять EVERSCALE SDK методы напрямую по схеме которая описана в TIP-32:

```js
  window.everscale
    .request({
      method: "ever_MODULE_METHOD",
      params: {}
    }).then((result) => {
       console.log(result);
    })
```

Например:

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

# Демонстрационная страница

Все методы EVERSCALE SDK и методы из спецификации TIP-32 доступны для тестирования на [демо странице](https://tip32-demopage.pertinaxwallet.com/)

# Наиболее часто используемые методы размещены ниже:

## wallet_getSdkVersion
DApp может получить версию EVERSCALE SDK которая используется в веб-расширении кошелька.

использует приватные ключи - **нет**
должно быть разрешено - **нет**
обязательные параметры - **нет**

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
DApp может запросить необходимые разрешения у пользователя. Не все методы требуют разрешения. Все методы, которые должны быть разрешены пользователем, перечислены ниже:
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

использует приватные ключи - **нет**
должно быть разрешено - **нет**
обязательные параметры - **permissions как массив**

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
DApp может запросить список методов, разрешенных пользователем

использует приватные ключи - **нет**
должно быть разрешено - **нет**
обязательные параметры - **нет**

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
DApp может запросить текущую учётную запись

использует приватные ключи - **нет**
должно быть разрешено - **да**
обязательные параметры - **нет**

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
DApp может запросить текущую точку подключения к блокчейн

использует приватные ключи - **нет**
должно быть разрешено - **да**
обязательные параметры - **нет**

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
DApp может инициализировать диалог транзакции

использует приватные ключи - **да**
должно быть разрешено - **да**
обязательные параметры - **destination как строка, amount как число, message как строка**

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
DApp может инициализировать диалог подписания сообщения

использует приватные ключи - **да**
должно быть разрешено - **да**
обязательные параметры - **data как строка**

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
DApp может получать публичный ключ для метода ever_encryptMessage

использует приватные ключи - **да**
должно быть разрешено - **да**
обязательные параметры - **нет**

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
DApp может инициализировать диалог для получения подписи пользователя

использует приватные ключи - **да**
должно быть разрешено - **да**
обязательные параметры - **data как строка**

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
DApp может запускать метод generate_random_bytes из модуля crypto

использует приватные ключи - **нет**
должно быть разрешено - **нет**
обязательные параметры - **length как число**

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
DApp может инициализировать диалог шифрования сообщения

**Nonce** может быть получен при помощи метода `ever_crypto_generate_random_bytes`с длиной 24 символов, **their_public** может быть получен при помощи метода `ever_getNaclBoxPublicKey` (публичный ключ Nacl для получателя)

использует приватные ключи - **да**
должно быть разрешено - **да**
обязательные параметры - **decrypted как строка, nonce как строка, their_public как строка**

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
DApp может инициализировать диалог дешифрования сообщения

**Nonce** должен быть таким же как и для метода `ever_encryptMessage`, **their_public** может быть получен при помощи метода `ever_getNaclBoxPublicKey` (публичный ключ Nacl отправителя)

использует приватные ключи - **да**
должно быть разрешено - **да**
обязательные параметры - **encrypted как строка, nonce как строка, their_public как строка**

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
DApp может подписываться на события блокчейна

использует приватные ключи - **нет**
должно быть разрешено - **да**
обязательные параметры - **collection как строка, filter как объект, result как строка**

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
DApp может отписываться от событий блокчейна

использует приватные ключи - **нет**
должно быть разрешено - **да**
обязательные параметры - **handle как число**

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
