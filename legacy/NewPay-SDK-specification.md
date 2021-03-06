
## NewPay SDK Specification

### What's NewID

[NewID definition](NewID-specification.md)

### Data format for DApps 

#### Request Common Header

| Field            | Type   | Notes                                      |
| ---              | ---    | ---                                        |
| app_id           | string | Application Id                             |
| req_id           | string | request Id                             |
| method           | string | choices: login,pay,proof.submit     |
| callback_url  | string | redirect schema of Mobile Native App       |
| bundle_source    | array  | package name of 3rd party Id               |
| protocol_version | int    | protocol version, default:2                |
| timestamp        | int    | timestamp                                  |
| nonce            | string | random string                              |
| signature        | string | secp256r1 signature hex string of application owner, format: "0xf9559857bb89e106de1c97bf640a481ff77a6f51e9ba8e8487d43999af0369c4e89eecca9ae085c44506137bc12ef16b24347c6b93b04fee5ef8572818382138". |

The timestamp and nonce fields is for preventing the replay attack.

The sign message is generated by json string exclude signature field.

Example
```
{
  "app_id": "",
  ...
}
```

#### Response Common Header

| Field            | Type   | Notes                                      |
| ---              | ---    | ---                                        |
| req_id           | string | request Id                             |
| error_code       | int    | Error Code, 1:success, others:fail         |
| error_message    | string | error message     |
| data    | list or dictionary | contain data     |
| signature        | string | secp256r1 signature hex string of authenticated user, format: "0xf9559857bb89e106de1c97bf640a481ff77a6f51e9ba8e8487d43999af0369c4e89eecca9ae085c44506137bc12ef16b24347c6b93b04fee5ef8572818382138". |

Example
```
{
  "req_id": "1",
  "error_code": 1,
  "error_message": "...",
  "data":[...]
  ...
}
```


#### Login

##### Request

| Field            | Type   | Notes                                                      |
| ---              | ---    | ---                                                        |
| scope            | int    | profile type Id. 1: base profile including name,head,newid; 2: advance profile including cellphone  |

Example
```
{
  "app_id": "",
  ...
  "scope": 1
}
```

##### Response

| Field            | Type   | Notes                                                      |
| ---              | ---    | ---                                                        |
| newid            | string | NewID              |
| name        | string | the user name      |
| country_code     | string | the country code   |
| cellphone        | string | the cellphone      |
| avatar      | string | the avatar of user |

Example
```
{
  "req_id": "1",
  "error_code": 1,
  "error_message": "...",
  "data": {
    "newid": "NEWID...",
    ...
  }
}
```

#### Pay

##### Request

| Field            | Type   | Notes                                                      |
| ---              | ---    | ---                                                        |
| merchant_newid     | string | NewID of merchant                                               |
| order_number     | string | order number                                               |
| order_description     | string | order description                                               |
| order_items     | list | order item list                                               |
| symbol     | string | symbol of digital token, such as NEW,BTC,ETH                                               |
| quantity     | string | quantity of digital token, unit is the minimum unit of given digital token                                               |

Example
```
{
  "app_id": "",
  ...
  "seller": "NEWID...",
  "order_number": "20190510....",
  "description": "....",
  "order_items": [
    {
      "product_id": "...",
      "name": "...",
      "url": "...",
      "quantity": 1,
    }
  ],
}
```

##### Response

| Field            | Type   | Notes                                                      |
| ---              | ---    | ---                                                        |
| txid     | string | Transaction ID                                               |
 

#### Proof

TBD

