# AtomicDEX API (Master - v2.0)

## add\_delegation


The `add_delegation` method initiates your node's staking of a compatible coin. Currently QTUM and tQTUM (test tokens avalable at http://testnet-faucet.qtum.info/) have been integrated, but this functionality will be expanded to more coins in future.

Note: Only UTXOs of 100 QTUM / tQTUM will be actively staked. It is recomended to consolidate your balance into a single UTXO before initiating delegated staking. After running `add_delegation`, you will need to broadcast the returned hex via [`send_raw_transaction`](../atomicdex-api-legacy/send_raw_transaction.html) to complete the process. Staking will only work with legacy QTUM addresses (segwit addresses are not supported).

#### Arguments

| Structure | Type   | Description                     |
| --------- | ------ | ------------------------------- |
| coin      | string | the coin being staked           |
| staking_details.type      | string | the protocol being staked           |
| staking_details.address      | string | the delegated staker address           |


##### :pushpin: Examples

##### Command

```bash
curl --location --request POST "http://127.0.0.1:7783" \
--header "Content-Type: application/json" \
--data-raw "{
    \"userpass\": \"$userpass\",
    \"mmrpc\": \"2.0\",
    \"method\": \"add_delegation\",
    \"params\": {
        \"coin\": \"tQTUM\",
        \"staking_details\": {
            \"type\": \"Qtum\",
            \"address\": \"qcyBHeSct7Wr4mAw18iuQ1zW5mMFYmtmBE\"
        }
    },
    \"id\": 0
}"
```

<div style="margin-top: 0.5rem;">



##### Response (success)

```json
{
  "mmrpc": "2.0",
  "result": {
    "tx_hex": "01000000017fdeb56c5b601454731451aa4daa004a7e1993e196462159c5e5360545fb9965000000006b483045022100c4101e0a70560dd8480404a620ac48a36a509c779cd3eb294d5b07f0447109ea0220145096570e6661fa52bf5df4b23329108959cb58cb02f1629e01cefb2d55fca6012102641b541e35bc915e375c8038f1099a977bc6736aa7265e9f65b7270b70d34366ffffffff020000000000000000fd0301540310552201284ce44c0e968c000000000000000000000000d4ea77298fdac12c657a18b222adc8b307e18127000000000000000000000000000000000000000000000000000000000000000a0000000000000000000000000000000000000000000000000000000000000060000000000000000000000000000000000000000000000000000000000000004120bf35729611a42875b49e890b7330c94a5227259b2cd987f885aaea3a08bad3897266a39db2d34f8009efa95eb877083b1eefccf2257f02cc0aa2e8db9a7f3eea00000000000000000000000000000000000000000000000000000000000000140000000000000000000000000000000000000086c200c4a0ba030000001976a914c36ac1020b1eae632079692e7bef350d279489c988acb4db8061",
    "tx_hash": "308c91fd50ec0f724d8c9f5601676b93889ae072b369b7a3d62684d6c3c60e7b",
    "from": ["qbNeoqCbBu4hySDUzgmo666faYH3qgaeKz"],
    "to": ["qbNeoqCbBu4hySDUzgmo666faYH3qgaeKz"],
    "total_amount": "161.064",
    "spent_by_me": "161.064",
    "received_by_me": "160.16",
    "my_balance_change": "-0.904",
    "block_height": 0,
    "timestamp": 1635834804,
    "fee_details": {
      "type": "Qrc20",
      "coin": "tQTUM",
      "miner_fee": "0.004",
      "gas_limit": 2250000,
      "gas_price": 40,
      "total_gas_fee": "0.9"
    },
    "coin": "tQTUM",
    "internal_id": "",
    "transaction_type": "StakingDelegation"
  },
  "id": 0
}
```

##### Response (error - already delegating)

```json
{
  "mmrpc": "2.0",
  "error": "Already delegating to: qcyBHeSct7Wr4mAw18iuQ1zW5mMFYmtmBE",
  "error_path": "qtum_delegation",
  "error_trace": "qtum_delegation:222]",
  "error_type": "AlreadyDelegating",
  "error_data": "qcyBHeSct7Wr4mAw18iuQ1zW5mMFYmtmBE",
  "id": 0
}
```




</div>

## add\_node\_to\_version\_stat


The `add_node_to_version_stat` method adds a Node's name, IP address and PeerID to a local database to track which version of MM2 it is running. The name parameter is an arbitrary identifying string, such as "seed_alpha" or "dragonhound_DEV". The address parameter is the node's IP address or domain names. The Peer ID can be found in the MM2 log file after a connection has been initiated, and looks like the below:

`07 09:33:58, atomicdex_behaviour:610] INFO Local peer id: PeerId("12D3KooWReXsTVCKGAna1tzrD1jaUttTSs17ULFuvvzoGD9bqmmA")
`

Note: To allow collection of version stats, added nodes must open port 38890.

#### Arguments

| Structure | Type   | Description                     |
| --------- | ------ | ------------------------------- |
| name      | string | the name assigned to the node   |
| address   | string | the IP address of the node      |
| peer_id   | string | the node's unique Peer ID       |


##### :pushpin: Examples

##### Command

```bash
curl --url "http://127.0.0.1:7783" --data "{
    \"mmrpc\": \"2.0\",
    \"method\":\"add_node_to_version_stat\",
    \"userpass\":\"$userpass\",
    \"params\":{\"name\": \"seed1\",
    \"address\": \"168.119.236.241\",
    \"peer_id\": \"12D3KooWEsuiKcQaBaKEzuMtT6uFjs89P1E8MK3wGRZbeuCbCw6P\"}
}"
```

<div style="margin-top: 0.5rem;">



##### Response (success)

```json
{
  "mmrpc":"2.0",
  "result":"success",
  "id":null
}
```

##### Response (error - peer id already in database)

```json
{
  "mmrpc":"2.0",
  "error":"Database error: UNIQUE constraint failed: nodes.peer_id",
  "error_path":"lp_stats",
  "error_trace":"lp_stats:124]",
  "error_type":"DatabaseError",
  "error_data":"UNIQUE constraint failed: nodes.peer_id",
  "id":null
}
```

##### Response (error - name already in database)

```json
{
  "mmrpc":"2.0",
  "error":"Database error: UNIQUE constraint failed: nodes.name",
  "error_path":"lp_stats","error_trace":"lp_stats:124]",
  "error_type":"DatabaseError",
  "error_data":"UNIQUE constraint failed: nodes.name",
  "id":null
}

```

##### Response (error - invalid Peer ID)

```json
{
  "mmrpc":"2.0",
  "error":"Error on parse peer id 12D3RsaaWRmXsJsCKGAD5FJSsd7CSbbdrsd: decoding multihash failed",
  "error_path":"lp_stats",
  "error_trace":"lp_stats:121]",
  "error_type":"PeerIdParseError",
  "error_data":["12D3RsaaWRmXsJsCKGAD5FJSsd7CSbbdrsd","decoding multihash failed"],
  "id":null
}

```



</div>

## best\_orders

The `best_orders` method returns the best priced trades available on the orderbook. There are two options for the request, either `volume` or `number`.
If request is made by `volume`, the returned results will show the best prices for trades that can fill the requested volume.
If request is made by `number`, the returned results will show a list of the best prices, `number` pairs long (e.g. top 5 best priced orders).
For coins with segwit, they may appear twice in the output (once for each address). E.g. `LTC` and `LTC-segwit`

::: tip

The response of this method can contain coins that are not activated on the AtomicDEX API instance.
Activation will be required to proceed with the trade.

:::

##### Arguments

| Structure          | Type                       | Description                                            |
| ------------------ | -------------------------- | ------------------------------------------------------ |
| coin               | string                     | The ticker of the coin to get best orders              |
| action             | string                     | Whether to `buy` or `sell` the selected coin           |
| request_by.type    | string                     | Defines whether requesting by `volume` or by `number`  |
| request_by.value   | string                     | If `request_by.type` is `volume`, the amount of `coin` user is willing to buy or sell. If `request_by.type` is `number`, the number of best price trades to return  |


##### Response

| Structure              | Type         | Description                                                                   |
| ---------------------- | ---------    | ----------------------------------------------------------------------------- |
| result                 | object (map) | the `ticker -> array of order entries` map                                    |


where order entry has the following structure

| Structure                | Type              | Description                                                                          |
| ----------------------   | ----------------- | ------------------------------------------------------------------------------------ |
| coin                     | string            | the ticker of the coin                                                               |
| address                  | string            | the address offering the trade                                                       |
| price                    | string (decimal)  | the price the user is willing to buy or sell per one unit of the coin from request   |
| price_rat                | rational          | the price in num-rational crate format                                               |
| price_fraction           | object (fraction) | the price represented as an object                                                   |
| maxvolume                | string (decimal)  | the maximum amount of `base` the offer provider is willing to sell                   |
| max_volume_rat           | rational          | the max volume in num-rational crate format                                          |
| max_volume_fraction      | object (rational) | the max volume represented as an object                                              |
| min_volume               | string (decimal)  | the minimum amount of `base` coin the offer provider is willing to sell              |
| min_volume_rat           | rational          | the min volume in num-rational crate format                                          |
| min_volume_fraction      | object (rational) | the min volume represented as an object                                              |
| pubkey                   | string            | the pubkey of the offer provider                                                     |
| age                      | number            | the age of the offer (in seconds)                                                    |
| zcredits                 | number            | the zeroconf deposit amount (deprecated)                                             |
| netid                    | number            | the id of the network on which the request is made (default is `0`)                  |
| uuid                     | string            | the uuid of order                                                                    |
| is_mine                  | bool              | whether the order is placed by me                                                    |
| base_max_volume          | string (decimal)  | the maximum amount of `base` coin the offer provider is willing to buy or sell       |
| base_max_volume_rat      | rational          | the `base_max_volume` in num-rational crate format                                   |
| base_max_volume_fraction | object (rational) | the `base_max_volume` represented as an object                                       |
| base_min_volume          | string (decimal)  | the minimum amount of `base` coin the offer provider is willing to buy or sell       |
| base_min_volume_rat      | rational          | the `base_min_volume` in num-rational crate format                                   |
| base_min_volume_fraction | object (rational) | the `base_min_volume` represented as an object                                       |
| base_confs               | number            | the confirmations settings of `base` coin set by the offer provider                  |
| base_nota                | bool              | the notarisation settings of `base` coin set by the offer provider                   |
| rel_max_volume           | string (decimal)  | the maximum amount of `rel` coin the offer provider is willing to buy or sell        |
| rel_max_volume_rat       | rational          | the `rel_max_volume` max volume in num-rational crate format                         |
| rel_max_volume_fraction  | object (rational) | the `rel_max_volume` max volume represented as an object                             |
| rel_min_volume           | string (decimal)  | the minimum amount of `rel` coin the offer provider is willing to buy or sell        |
| rel_min_volume_rat       | rational          | the `rel_min_volume` in num-rational crate format                                    |
| rel_min_volume_fraction  | object (rational) | the `rel_min_volume` represented as an object                                        |
| rel_confs                | number            | the confirmations settings of `rel` coin set by the offer provider                   |
| rel_nota                 | bool              | the notarisation settings of `rel` coin set by the offer provider                    |
| original_tickers         | list (string)     | Tickers included in response when `orderbook_ticker` is configured for the queried coin in `coins` file |



##### :pushpin: Examples

##### Command (by number)

```bash
curl --url "http://127.0.0.1:7783" --data "{
    \"userpass\": \"$userpass\",
    \"method\": \"best_orders\",
    \"mmrpc\": \"2.0\",
    \"params\": {
        \"coin\": \"DGB\",
        \"action\": \"buy\",
        \"request_by\": { 
              \"type\": \"number\",
              \"value\": 100
        }
    }
}"
```

<div style="margin-top: 0.5rem;">



##### Response (by number - success)

```json
{
  "mmrpc": "2.0",
  "result": {
    "orders": {
      "TKL": [{
        "coin": "TKL",
        "address": {
          "address_type": "Transparent",
          "address_data": "RAwv8JhfvmFx2V3QpY7ehiYpBJ1eqxxdxR"
        },
        "price": {
          "decimal": "8.7753611926",
          "rational": [
            [1, [927133003, 10]],
            [1, [705032704, 1]]
          ],
          "fraction": {
            "numer": "43876805963",
            "denom": "5000000000"
          }
        },
        "pubkey": "02dbd8c73e2e80e4f3cf88d2f04a9d2d0df4269496608b14a3e17556fdcb01e0c1",
        "uuid": "416bb993-acac-42cf-ae54-bf57a21f7a3d",
        "is_mine": false,
        "base_max_volume": {
          "decimal": "2478.31474597274",
          "rational": [
            [1, [1635841741, 28851]],
            [1, [2755359744, 11]]
          ],
          "fraction": {
            "numer": "123915737298637",
            "denom": "50000000000"
          }
        },
        "base_min_volume": {
          "decimal": "0.0001",
          "rational": [
            [1, [1]],
            [1, [10000]]
          ],
          "fraction": {
            "numer": "1",
            "denom": "10000"
          }
        },
        "rel_max_volume": {
          "decimal": "21748.107044857509733489724",
          "rational": [
            [1, [2703103759, 3484586521, 294741]],
            [1, [2007498752, 2373086065, 13]]
          ],
          "fraction": {
            "numer": "5437026761214377433372431",
            "denom": "250000000000000000000"
          }
        },
        "rel_min_volume": {
          "decimal": "0.00087753611926",
          "rational": [
            [1, [927133003, 10]],
            [1, [2285707264, 11641]]
          ],
          "fraction": {
            "numer": "43876805963",
            "denom": "50000000000000"
          }
        },
        "conf_settings": {
          "base_confs": 7,
          "base_nota": false,
          "rel_confs": 2,
          "rel_nota": true
        }
      }],
      "XMY-segwit": [{
        "coin": "XMY-segwit",
        "address": {
          "address_type": "Transparent",
          "address_data": "MJx4vyc657NwQUMjzRgDcypvRw5NduzSz4"
        },
        "price": {
          "decimal": "88.3356107012",
          "rational": [
            [1, [1795694657, 51]],
            [1, [2500000000]]
          ],
          "fraction": {
            "numer": "220839026753",
            "denom": "2500000000"
          }
        },
        "pubkey": "025d81c96aa4269c5946c0bd8dad7785ae0f4f595e7aea2ec4f8fe71f77ebf74a9",
        "uuid": "999b34ca-c7b2-4caa-92e2-76349ed2d4b5",
        "is_mine": false,
        "base_max_volume": {
          "decimal": "2478.31474597274",
          "rational": [
            [1, [1635841741, 28851]],
            [1, [2755359744, 11]]
          ],
          "fraction": {
            "numer": "123915737298637",
            "denom": "50000000000"
          }
        },
        "base_min_volume": {
          "decimal": "0.0001",
          "rational": [
            [1, [1]],
            [1, [10000]]
          ],
          "fraction": {
            "numer": "1",
            "denom": "10000"
          }
        },
        "rel_max_volume": {
          "decimal": "218923.446595291331147485288",
          "rational": [
            [1, [1174424077, 4198508482, 1483482]],
            [1, [3151233024, 3334026680, 6]]
          ],
          "fraction": {
            "numer": "27365430824411416393435661",
            "denom": "125000000000000000000"
          }
        },
        "rel_min_volume": {
          "decimal": "0.00883356107012",
          "rational": [
            [1, [1795694657, 51]],
            [1, [3290337280, 5820]]
          ],
          "fraction": {
            "numer": "220839026753",
            "denom": "25000000000000"
          }
        },
        "conf_settings": {
          "base_confs": 7,
          "base_nota": false,
          "rel_confs": 3,
          "rel_nota": false
        }
      }],
      "LTC": [{
        "coin": "LTC",
        "address": {
          "address_type": "Transparent",
          "address_data": "Lgrta1iKRcy8zzygVkZeEXuxBqzssPWtae"
        },
        "price": {
          "decimal": "0.0001087673",
          "rational": [
            [1, [1087673]],
            [1, [1410065408, 2]]
          ],
          "fraction": {
            "numer": "1087673",
            "denom": "10000000000"
          }
        },
        "pubkey": "026da2fc632afabbb1b86d04a9a012db25eca74db38ba2eccd88552f27f4c0b245",
        "uuid": "8530300a-b11c-4eca-80ab-c4124aaf3b64",
        "is_mine": false,
        "base_max_volume": {
          "decimal": "24706.624279842",
          "rational": [
            [1, [986196625, 2876]],
            [1, [500000000]]
          ],
          "fraction": {
            "numer": "12353312139921",
            "denom": "500000000"
          }
        },
        "base_min_volume": {
          "decimal": "5.019891088590044985947063133864681756373468864263432116086360514603194158538457790163036133102504153",
          "rational": [
            [1, [5460000]],
            [1, [1087673]]
          ],
          "fraction": {
            "numer": "5460000",
            "denom": "1087673"
          }
        },
        "rel_max_volume": {
          "decimal": "2.6872728150328587666",
          "rational": [
            [1, [4244429513, 3128397295]],
            [1, [1156841472, 1164153218]]
          ],
          "fraction": {
            "numer": "13436364075164293833",
            "denom": "5000000000000000000"
          }
        },
        "rel_min_volume": {
          "decimal": "0.000546",
          "rational": [
            [1, [273]],
            [1, [500000]]
          ],
          "fraction": {
            "numer": "273",
            "denom": "500000"
          }
        },
        "conf_settings": {
          "base_confs": 7,
          "base_nota": false,
          "rel_confs": 2,
          "rel_nota": false
        }
      }],
      "LTC-segwit": [{
        "coin": "LTC-segwit",
        "address": {
          "address_type": "Transparent",
          "address_data": "Lgrta1iKRcy8zzygVkZeEXuxBqzssPWtae"
        },
        "price": {
          "decimal": "0.0001087673",
          "rational": [
            [1, [1087673]],
            [1, [1410065408, 2]]
          ],
          "fraction": {
            "numer": "1087673",
            "denom": "10000000000"
          }
        },
        "pubkey": "026da2fc632afabbb1b86d04a9a012db25eca74db38ba2eccd88552f27f4c0b245",
        "uuid": "8530300a-b11c-4eca-80ab-c4124aaf3b64",
        "is_mine": false,
        "base_max_volume": {
          "decimal": "24706.624279842",
          "rational": [
            [1, [986196625, 2876]],
            [1, [500000000]]
          ],
          "fraction": {
            "numer": "12353312139921",
            "denom": "500000000"
          }
        },
        "base_min_volume": {
          "decimal": "5.019891088590044985947063133864681756373468864263432116086360514603194158538457790163036133102504153",
          "rational": [
            [1, [5460000]],
            [1, [1087673]]
          ],
          "fraction": {
            "numer": "5460000",
            "denom": "1087673"
          }
        },
        "rel_max_volume": {
          "decimal": "2.6872728150328587666",
          "rational": [
            [1, [4244429513, 3128397295]],
            [1, [1156841472, 1164153218]]
          ],
          "fraction": {
            "numer": "13436364075164293833",
            "denom": "5000000000000000000"
          }
        },
        "rel_min_volume": {
          "decimal": "0.000546",
          "rational": [
            [1, [273]],
            [1, [500000]]
          ],
          "fraction": {
            "numer": "273",
            "denom": "500000"
          }
        },
        "conf_settings": {
          "base_confs": 7,
          "base_nota": false,
          "rel_confs": 2,
          "rel_nota": false
        }
      }],
      "MATIC": [{
        "coin": "MATIC",
        "address": {
          "address_type": "Transparent",
          "address_data": "0xf2ed2ac92489106c942c9e32c6a912ba61af93e3"
        },
        "price": {
          "decimal": "0.0104639634",
          "rational": [
            [1, [52319817]],
            [1, [705032704, 1]]
          ],
          "fraction": {
            "numer": "52319817",
            "denom": "5000000000"
          }
        },
        "pubkey": "02dbd8c73e2e80e4f3cf88d2f04a9d2d0df4269496608b14a3e17556fdcb01e0c1",
        "uuid": "95bb48ce-7411-4be7-a1b9-70e8f8d7887f",
        "is_mine": false,
        "base_max_volume": {
          "decimal": "8964.021726027",
          "rational": [
            [1, [424979275, 2087]],
            [1, [1000000000]]
          ],
          "fraction": {
            "numer": "8964021726027",
            "denom": "1000000000"
          }
        },
        "base_min_volume": {
          "decimal": "20.0825412816031",
          "rational": [
            [1, [1331989663, 46758]],
            [1, [1316134912, 2328]]
          ],
          "fraction": {
            "numer": "200825412816031",
            "denom": "10000000000000"
          }
        },
        "rel_max_volume": {
          "decimal": "93.7991952579513554118",
          "rational": [
            [1, [2658798179, 1822452630, 25]],
            [1, [1156841472, 1164153218]]
          ],
          "fraction": {
            "numer": "468995976289756777059",
            "denom": "5000000000000000000"
          }
        },
        "rel_min_volume": {
          "decimal": "0.21014297694968393172654",
          "rational": [
            [1, [3263923031, 2549837702, 569]],
            [1, [2067791872, 2170810533, 2710]]
          ],
          "fraction": {
            "numer": "10507148847484196586327",
            "denom": "50000000000000000000000"
          }
        },
        "conf_settings": {
          "base_confs": 7,
          "base_nota": false,
          "rel_confs": 3,
          "rel_nota": false
        }
      }]
    },
    "original_tickers": {
      "LTC": ["LTC-segwit"],
      "BTC": ["BTC-segwit"],
      "XMY": ["XMY-segwit"]
    }
  },
  "id": 0
}
```



</div>

##### Command (by volume)

```bash
curl --url "http://127.0.0.1:7783" --data "{
    \"userpass\": \"$userpass\",
    \"method\": \"best_orders\",
    \"mmrpc\": \"2.0\",
    \"params\": {
        \"coin\": \"BTC\",
        \"action\": \"buy\",
        \"request_by\": { 
              \"type\": \"volume\",
              \"value\": 0.01
        }
    }
}"
```

<div style="margin-top: 0.5rem;">



##### Response (by volume - success)

```json
{
    "mmrpc": "2.0",
    "result": {
        "orders": {
            "DASH": [{
                "coin": "DASH",
                "address": {
                    "address_type": "Transparent",
                    "address_data": "XefPeyw3KQYa5PUJeTMQRhMHQZGVy4YMWa"
                },
                "price": {
                    "decimal": "3333.333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333",
                    "rational": [
                        [1, [10000]],
                        [1, [3]]
                    ],
                    "fraction": {
                        "numer": "10000",
                        "denom": "3"
                    }
                },
                "pubkey": "0261eef15cbc141f555aff1aa40fb21de17a0a9e6897eee18c14c6032586b456b3",
                "uuid": "b17d7aee-2c0b-4311-935c-8c05e81f3813",
                "is_mine": false,
                "base_max_volume": {
                    "decimal": "0.097714296984",
                    "rational": [
                        [1, [3624352531, 2]],
                        [1, [445948416, 29]]
                    ],
                    "fraction": {
                        "numer": "12214287123",
                        "denom": "125000000000"
                    }
                },
                "base_min_volume": {
                    "decimal": "0.000002331",
                    "rational": [
                        [1, [2331]],
                        [1, [1000000000]]
                    ],
                    "fraction": {
                        "numer": "2331",
                        "denom": "1000000000"
                    }
                },
                "rel_max_volume": {
                    "decimal": "325.71432328",
                    "rational": [
                        [1, [4071429041]],
                        [1, [12500000]]
                    ],
                    "fraction": {
                        "numer": "4071429041",
                        "denom": "12500000"
                    }
                },
                "rel_min_volume": {
                    "decimal": "0.00777",
                    "rational": [
                        [1, [777]],
                        [1, [100000]]
                    ],
                    "fraction": {
                        "numer": "777",
                        "denom": "100000"
                    }
                },
                "conf_settings": null
            }],
            "LTC": [{
                "coin": "LTC",
                "address": {
                    "address_type": "Transparent",
                    "address_data": "LPCW5waySMa3BFZsxi2UrBjFnS464b97WU"
                },
                "price": {
                    "decimal": "10000",
                    "rational": [
                        [1, [10000]],
                        [1, [1]]
                    ],
                    "fraction": {
                        "numer": "10000",
                        "denom": "1"
                    }
                },
                "pubkey": "0261eef15cbc141f555aff1aa40fb21de17a0a9e6897eee18c14c6032586b456b3",
                "uuid": "07764da3-bbec-4e50-9711-2baf0f8bf30b",
                "is_mine": false,
                "base_max_volume": {
                    "decimal": "0.11423436",
                    "rational": [
                        [1, [2855859]],
                        [1, [25000000]]
                    ],
                    "fraction": {
                        "numer": "2855859",
                        "denom": "25000000"
                    }
                },
                "base_min_volume": {
                    "decimal": "0.000000777",
                    "rational": [
                        [1, [777]],
                        [1, [1000000000]]
                    ],
                    "fraction": {
                        "numer": "777",
                        "denom": "1000000000"
                    }
                },
                "rel_max_volume": {
                    "decimal": "1142.3436",
                    "rational": [
                        [1, [2855859]],
                        [1, [2500]]
                    ],
                    "fraction": {
                        "numer": "2855859",
                        "denom": "2500"
                    }
                },
                "rel_min_volume": {
                    "decimal": "0.00777",
                    "rational": [
                        [1, [777]],
                        [1, [100000]]
                    ],
                    "fraction": {
                        "numer": "777",
                        "denom": "100000"
                    }
                },
                "conf_settings": {
                    "base_confs": 1,
                    "base_nota": false,
                    "rel_confs": 2,
                    "rel_nota": false
                }
            }],
            "KMD": [{
                "coin": "KMD",
                "address": {
                    "address_type": "Transparent",
                    "address_data": "RDFjuFARxX8YzTEvFk2JfgzhLV9QcPWy5f"
                },
                "price": {
                    "decimal": "322580.6451612903225806451612903225806451612903225806451612903225806451612903225806451612903225806452",
                    "rational": [
                        [1, [10000000]],
                        [1, [31]]
                    ],
                    "fraction": {
                        "numer": "10000000",
                        "denom": "31"
                    }
                },
                "pubkey": "0261eef15cbc141f555aff1aa40fb21de17a0a9e6897eee18c14c6032586b456b3",
                "uuid": "adff2e1d-4514-49ea-a30b-9575711767cd",
                "is_mine": false,
                "base_max_volume": {
                    "decimal": "0.031",
                    "rational": [
                        [1, [31]],
                        [1, [1000]]
                    ],
                    "fraction": {
                        "numer": "31",
                        "denom": "1000"
                    }
                },
                "base_min_volume": {
                    "decimal": "0.000000024087",
                    "rational": [
                        [1, [24087]],
                        [1, [3567587328, 232]]
                    ],
                    "fraction": {
                        "numer": "24087",
                        "denom": "1000000000000"
                    }
                },
                "rel_max_volume": {
                    "decimal": "10000",
                    "rational": [
                        [1, [10000]],
                        [1, [1]]
                    ],
                    "fraction": {
                        "numer": "10000",
                        "denom": "1"
                    }
                },
                "rel_min_volume": {
                    "decimal": "0.00777",
                    "rational": [
                        [1, [777]],
                        [1, [100000]]
                    ],
                    "fraction": {
                        "numer": "777",
                        "denom": "100000"
                    }
                },
                "conf_settings": null
            }],
            "DAI-ERC20": [{
                "coin": "DAI-ERC20",
                "address": {
                    "address_type": "Transparent",
                    "address_data": "0xe5e6d27100474d34cc0f87ee387756395019019c"
                },
                "price": {
                    "decimal": "33333333.33333333333333333333333333333333333333333333333333333333333333333333333333333333333333333333",
                    "rational": [
                        [1, [100000000]],
                        [1, [3]]
                    ],
                    "fraction": {
                        "numer": "100000000",
                        "denom": "3"
                    }
                },
                "pubkey": "0261eef15cbc141f555aff1aa40fb21de17a0a9e6897eee18c14c6032586b456b3",
                "uuid": "15a47eff-607e-4729-896b-6acb309d5022",
                "is_mine": false,
                "base_max_volume": {
                    "decimal": "0.1075026242236026",
                    "rational": [
                        [1, [2258990909, 125149]],
                        [1, [937459712, 1164153]]
                    ],
                    "fraction": {
                        "numer": "537513121118013",
                        "denom": "5000000000000000"
                    }
                },
                "base_min_volume": {
                    "decimal": "0.0081585",
                    "rational": [
                        [1, [16317]],
                        [1, [2000000]]
                    ],
                    "fraction": {
                        "numer": "16317",
                        "denom": "2000000"
                    }
                },
                "rel_max_volume": {
                    "decimal": "3583420.80745342",
                    "rational": [
                        [1, [2184652735, 41716]],
                        [1, [50000000]]
                    ],
                    "fraction": {
                        "numer": "179171040372671",
                        "denom": "50000000"
                    }
                },
                "rel_min_volume": {
                    "decimal": "271950",
                    "rational": [
                        [1, [271950]],
                        [1, [1]]
                    ],
                    "fraction": {
                        "numer": "271950",
                        "denom": "1"
                    }
                },
                "conf_settings": null
            }],
            "NMC": [{
                "coin": "NMC",
                "address": {
                    "address_type": "Transparent",
                    "address_data": "MzYv2Nn8H5RYSz8E4PMkngpQJT5ruqWV4N"
                },
                "price": {
                    "decimal": "80000",
                    "rational": [
                        [1, [80000]],
                        [1, [1]]
                    ],
                    "fraction": {
                        "numer": "80000",
                        "denom": "1"
                    }
                },
                "pubkey": "0261eef15cbc141f555aff1aa40fb21de17a0a9e6897eee18c14c6032586b456b3",
                "uuid": "87e3e99c-481f-46cc-9a64-ccc89ba5e434",
                "is_mine": false,
                "base_max_volume": {
                    "decimal": "0.025",
                    "rational": [
                        [1, [1]],
                        [1, [40]]
                    ],
                    "fraction": {
                        "numer": "1",
                        "denom": "40"
                    }
                },
                "base_min_volume": {
                    "decimal": "0.000000097125",
                    "rational": [
                        [1, [777]],
                        [1, [3705032704, 1]]
                    ],
                    "fraction": {
                        "numer": "777",
                        "denom": "8000000000"
                    }
                },
                "rel_max_volume": {
                    "decimal": "2000",
                    "rational": [
                        [1, [2000]],
                        [1, [1]]
                    ],
                    "fraction": {
                        "numer": "2000",
                        "denom": "1"
                    }
                },
                "rel_min_volume": {
                    "decimal": "0.00777",
                    "rational": [
                        [1, [777]],
                        [1, [100000]]
                    ],
                    "fraction": {
                        "numer": "777",
                        "denom": "100000"
                    }
                },
                "conf_settings": null
            }],
            "DOGE": [{
                "coin": "DOGE",
                "address": {
                    "address_type": "Transparent",
                    "address_data": "D97eMzDnf7EGTT4KXA2k7vq6TMR7JDpe1D"
                },
                "price": {
                    "decimal": "20000000",
                    "rational": [
                        [1, [20000000]],
                        [1, [1]]
                    ],
                    "fraction": {
                        "numer": "20000000",
                        "denom": "1"
                    }
                },
                "pubkey": "0261eef15cbc141f555aff1aa40fb21de17a0a9e6897eee18c14c6032586b456b3",
                "uuid": "14de5083-daee-4d82-bc41-ac809076bf5f",
                "is_mine": false,
                "base_max_volume": {
                    "decimal": "0.02074779",
                    "rational": [
                        [1, [2074779]],
                        [1, [100000000]]
                    ],
                    "fraction": {
                        "numer": "2074779",
                        "denom": "100000000"
                    }
                },
                "base_min_volume": {
                    "decimal": "0.00777",
                    "rational": [
                        [1, [777]],
                        [1, [100000]]
                    ],
                    "fraction": {
                        "numer": "777",
                        "denom": "100000"
                    }
                },
                "rel_max_volume": {
                    "decimal": "414955.8",
                    "rational": [
                        [1, [2074779]],
                        [1, [5]]
                    ],
                    "fraction": {
                        "numer": "2074779",
                        "denom": "5"
                    }
                },
                "rel_min_volume": {
                    "decimal": "155400",
                    "rational": [
                        [1, [155400]],
                        [1, [1]]
                    ],
                    "fraction": {
                        "numer": "155400",
                        "denom": "1"
                    }
                },
                "conf_settings": null
            }],
            "ETH": [{
                "coin": "ETH",
                "address": {
                    "address_type": "Transparent",
                    "address_data": "0xe5e6d27100474d34cc0f87ee387756395019019c"
                },
                "price": {
                    "decimal": "400000",
                    "rational": [
                        [1, [400000]],
                        [1, [1]]
                    ],
                    "fraction": {
                        "numer": "400000",
                        "denom": "1"
                    }
                },
                "pubkey": "0261eef15cbc141f555aff1aa40fb21de17a0a9e6897eee18c14c6032586b456b3",
                "uuid": "19220788-3643-4fb2-9445-e13515ef811e",
                "is_mine": false,
                "base_max_volume": {
                    "decimal": "0.11209544",
                    "rational": [
                        [1, [1401193]],
                        [1, [12500000]]
                    ],
                    "fraction": {
                        "numer": "1401193",
                        "denom": "12500000"
                    }
                },
                "base_min_volume": {
                    "decimal": "0.0081585",
                    "rational": [
                        [1, [16317]],
                        [1, [2000000]]
                    ],
                    "fraction": {
                        "numer": "16317",
                        "denom": "2000000"
                    }
                },
                "rel_max_volume": {
                    "decimal": "44838.176",
                    "rational": [
                        [1, [5604772]],
                        [1, [125]]
                    ],
                    "fraction": {
                        "numer": "5604772",
                        "denom": "125"
                    }
                },
                "rel_min_volume": {
                    "decimal": "3263.4",
                    "rational": [
                        [1, [16317]],
                        [1, [5]]
                    ],
                    "fraction": {
                        "numer": "16317",
                        "denom": "5"
                    }
                },
                "conf_settings": null
            }]
        },
        "original_tickers": {
            "MONA": ["MONA-segwit"],
            "NMC": ["NMC-segwit"],
            "LTC": ["LTC-segwit"],
            "PIC": ["PIC-segwit"],
            "LCC": ["LCC-segwit"],
            "BSTY": ["BSTY-segwit"],
            "BTC": ["BTC-segwit"],
            "PPC": ["PPC-segwit"],
            "GLEEC-OLD": ["GLEEC-OLD-segwit"],
            "LBC": ["LBC-segwit"],
            "BTE": ["BTE-segwit"],
            "VTC": ["VTC-segwit"],
            "LTFN": ["LTFN-segwit"],
            "SYS": ["SYS-segwit"],
            "BTX": ["BTX-segwit"],
            "tBTC-TEST": ["tBTC-TEST-segwit"],
            "CDN": ["CDN-segwit"],
            "FTC": ["FTC-segwit"],
            "GRS": ["GRS-segwit"],
            "RIC": ["RIC-segwit"],
            "XMY": ["XMY-segwit"],
            "VIA": ["VIA-segwit"],
            "WHIVE": ["WHIVE-segwit"],
            "XEP": ["XEP-segwit"],
            "FJC": ["FJC-segwit"],
            "WCN": ["WCN-segwit"],
            "QTUM": ["QTUM-segwit"],
            "tQTUM": ["tQTUM-segwit"],
            "DGB": ["DGB-segwit"]
        }
    },
    "id": null
}
```



</div>

<div style="margin-top: 0.5rem;">




##### Error Responses

`InvalidRequest` - Invalid type (`number` value must be integer)
`InvalidRequest` - Invalid type (type must be either `volume` or `number`, action mut be either `buy` or `sell`)
`CoinIsWalletOnly` - Wallet only coins can not be traded.
`P2PError` - There is a connection problem.



</div>


## enable\_bch\_with\_tokens

The AtomicDEX-API supports Bitcoin Cash SLP tokens. Using this method, you can enable BCH/tBCH along with multiple SLP tokens in a single command.


| parameter              | Type     | Description                               |
| ---------------------- | -------- | ----------------------------------------- |
| ticker                 | string   | Ticker of the platform protocol coin. Options: `BCH` or `tBCH` |
| allow_slp_unsafe_conf  | boolean  | Optional. If `true`, allows bchd_urls to be empty. **Warning:** it is highly unsafe to do so as it may lead to invalid SLP transactions generation and tokens burning. Defaults to `false`. |
| bchd_urls              | string   | An array of strings. URLs of BCHD gRPC API servers that are used for SLP tokens transactions validation. It's recommended to add as many servers as possible. The URLs list can be found at https://bchd.fountainhead.cash/.          |
| mode                   | string   | Utxo RPC mode. Options: `{ "rpc":"Native" }` if running a native blockchain node, or `"rpc":"Electrum"` to use electrum RPCs. If using electrum, a list of electrum servers is required under `rpc_data.servers` |
| tx_history             | boolean  | If `true`, spawns a background loop to store the local cache of address(es) transactions. Defaults to `false`. |
| slp_tokens_requests    | string   | Array of SLP activation requests. SLP activation requests contain mandatory `ticker` and optional `required_confirmations` fields. If required_confirmations is not set for a token, then MM2 will use the confirmations setting from its coins config or platform coin. |
| required_confirmations | integer  | Optional. Confirmations to wait for steps in swap. Defaults to value in the coins file if not set. |
| requires_notarization  | boolean  | Optional. Has no effect on BCH. Defaults to `false`. |
| address_format.format  | string   | Optional. Overwrites the address format from coins file, if set. Options: `{"format":"standard"}` for legacy/standard address format, `{"format":"cashaddress"}` for cash address format |
| address_format.network | string   | Optional. Overwrites the address network from coins file, if set. Options: `{"network":"bitcoincash"}` for mainnet, `{"network":"bchreg"}` for regtest, or `{"network":"bchtest"}` for testnet |
| utxo_merge_params      | boolean  | Optional. If defined, will spawn a background loop that checks the number of UTXOs every `check_every seconds` and merges `max_merge_at_once` utxos into a single utxo if the total exceeds `merge_at`. Recommended for addresses with high trading activity or mining which can increase the number of UTXOs in the address, leading to delays in RPC response (or in extreme cases, connection timing out). |


### Example using Electrum servers on testnet with tx_history, cashaddress format and automated utxo merging.


```bash
curl --url "http://127.0.0.1:7783" --data "{
  \"userpass\":\"$userpass\",
  \"method\":\"enable_bch_with_tokens\",
  \"mmrpc\":\"2.0\",
  \"params\":{
    \"ticker\":\"BCH\",
    \"allow_slp_unsafe_conf\":false,
    \"bchd_urls\":[
      \"https://bchd.imaginary.cash:8335/\"
    ],
    \"mode\":{
      \"rpc\":\"Electrum\",
      \"rpc_data\":{
        \"servers\":[
          {
            \"url\":\"electrum1.cipig.net:10055\"
          },
          {
            \"url\":\"electrum2.cipig.net:10055\"
          },
          {
            \"url\":\"electrum3.cipig.net:10055\"
          },
          {
            \"url\":\"electrum1.cipig.net:20055\",
            \"protocol\": \"SSL\"
          },
          {
            \"url\":\"electrum2.cipig.net:20055\",
            \"protocol\": \"SSL\"
          },
          {
            \"url\":\"electrum3.cipig.net:20055\",
            \"protocol\": \"SSL\"
          }
        ]
      }
    },
    \"tx_history\":true,
    \"slp_tokens_requests\":[
      {
        \"ticker\":\"ASLP-SLP\",
        \"required_confirmations\": 4
      }
    ],
    \"required_confirmations\":5,
    \"requires_notarization\":false,
    \"address_format\":{
      \"format\":\"cashaddress\",
      \"network\":\"bitcoincash\"
    },
    \"utxo_merge_params\":{
      \"merge_at\":50,
      \"check_every\":10,
      \"max_merge_at_once\":25
    }
  }
}"
```


### Response

```json
{
  "mmrpc":"2.0",
  "result":{
    "current_block":1480481,
    "bch_addresses_infos":{
      "bitcoincash:qrf5vpn78s7rjexrjhlwyzzeg7gw98k7t5qx64fztj":{
        "derivation_method":{
          "type":"Iguana"
        },
        "pubkey":"036879df230663db4cd083c8eeb0f293f46abc460ad3c299b0089b72e6d472202c",
        "balances":{
          "spendable":"0.11398301",
          "unspendable":"0.00001"
        }
      }
    },
    "slp_addresses_infos":{
      "simpleledger:qrf5vpn78s7rjexrjhlwyzzeg7gw98k7t5va3wuz4v":{
        "derivation_method":{
          "type":"Iguana"
        },
        "pubkey":"036879df230663db4cd083c8eeb0f293f46abc460ad3c299b0089b72e6d472202c",
        "balances":{
          "ASLP":{
            "spendable":"5.2974",
            "unspendable":"0"
          }
        }
      }
    }
  },
  "id":null
}
```

### Errors (BCH already activated)
In this case, you need to [disable](../atomicdex-api-legacy/disable_coin.md) BCH and try again.
```json
{
  "mmrpc":"2.0",
  "error":"BCH",
  "error_path":"platform_coin_with_tokens",
  "error_trace":"platform_coin_with_tokens:281]",
  "error_type":"PlatformIsAlreadyActivated",
  "error_data":"BCH",
  "id":null
}
```

### Error (Platform config missing from coins file)
```json
{
  "mmrpc":"2.0",
  "error":"Platform BCH-wrong config is not found",
  "error_path":"platform_coin_with_tokens.prelude",
  "error_trace":"platform_coin_with_tokens:286] prelude:56]",
  "error_type":"PlatformConfigIsNotFound",
  "error_data":"BCH-wrong",
  "id":null
}
```

### Errors (Platform protocol invalid from coins file)
```json
{
  "mmrpc":"2.0",
  "error":"Unexpected platform protocol UTXO for BTC",
  "error_path":"platform_coin_with_tokens.prelude.bch_with_tokens_activation",
  "error_trace":"platform_coin_with_tokens:286] prelude:67] bch_with_tokens_activation:127]",
  "error_type":"UnexpectedPlatformProtocol",
  "error_data":{
    "ticker":"BTC",
    "protocol":{
      "type":"UTXO"
    }
  },
  "id":null
}
```

```json
{
  "mmrpc":"2.0",
  "error":"Unexpected token protocol ERC20 { platform: \"ETH\", contract_address: \"0xdAC17F958D2ee523a2206206994597C13D831ec7\" } for USDT-ERC20",
  "error_path":"platform_coin_with_tokens.prelude.slp_token_activation",
  "error_trace":"platform_coin_with_tokens:301] platform_coin_with_tokens:114] prelude:67] slp_token_activation:47]",
  "error_type":"UnexpectedTokenProtocol",
  "error_data":{
    "ticker":"USDT-ERC20",
    "protocol":{
      "type":"ERC20",
      "protocol_data":{
        "platform":"ETH",
        "contract_address":"0xdAC17F958D2ee523a2206206994597C13D831ec7"
      }
    }
  },
  "id":null
}
```


### Error (Unsafe configuration with empty bchd_urls param)
```json
{
  "mmrpc":"2.0",
  "error":"Error Using empty bchd_urls is unsafe for SLP users! on platform coin BCH creation",
  "error_path":"platform_coin_with_tokens.bch_with_tokens_activation",
  "error_trace":"platform_coin_with_tokens:290] bch_with_tokens_activation:212]",
  "error_type":"PlatformCoinCreationError",
  "error_data":{
    "ticker":"BCH",
    "error":"Using empty bchd_urls is unsafe for SLP users!"
  },
  "id":null
}
```


### Error (conf file not found when enabling in native mode)
```json
{
  "mmrpc":"2.0",
  "error":"Error bch:633] utxo:1704] utxo:995] Error parsing the native wallet configuration '/home/user/.Bitcoin Cash Testnet/Bitcoin Cash Testnet.conf': No such file or directory (os error 2) on platform coin tBCH creation",
  "error_path":"platform_coin_with_tokens.bch_with_tokens_activation",
  "error_trace":"platform_coin_with_tokens:290] bch_with_tokens_activation:212]",
  "error_type":"PlatformCoinCreationError",
  "error_data":{
    "ticker":"tBCH",
    "error":"bch:633] utxo:1704] utxo:995] Error parsing the native wallet configuration '/home/user/.Bitcoin Cash Testnet/Bitcoin Cash Testnet.conf': No such file or directory (os error 2)"
  },
  "id":null
}
```


### Error (Electrum server not responding)
```json
{
  "mmrpc":"2.0",
  "error":"Error bch:633] utxo:1667] Failed to connect to at least 1 of [ElectrumRpcRequest { url: \"bch0.kister.net:5100\", protocol: TCP, disable_cert_verification: false }, ElectrumRpcRequest { url: \"testnet.imaginary.cash:5000\", protocol: TCP, disable_cert_verification: false }, ElectrumRpcRequest { url: \"blackie.c3-soft.com:6000\", protocol: TCP, disable_cert_verification: false }, ElectrumRpcRequest { url: \"tbch.loping.net:6000\", protocol: TCP, disable_cert_verification: false }, ElectrumRpcRequest { url: \"electroncash.de:5000\", protocol: TCP, disable_cert_verification: false }] in 5 seconds. on platform coin tBCH creation",
  "error_path":"platform_coin_with_tokens.bch_with_tokens_activation",
  "error_trace":"platform_coin_with_tokens:290] bch_with_tokens_activation:212]",
  "error_type":"PlatformCoinCreationError",
  "error_data":{
    "ticker":"tBCH",
    "error":"bch:633] utxo:1667] Failed to connect to at least 1 of [ElectrumRpcRequest { url: \"bch0.kister.net:5100\", protocol: TCP, disable_cert_verification: false }, ElectrumRpcRequest { url: \"testnet.imaginary.cash:5000\", protocol: TCP, disable_cert_verification: false }, ElectrumRpcRequest { url: \"blackie.c3-soft.com:6000\", protocol: TCP, disable_cert_verification: false }, ElectrumRpcRequest { url: \"tbch.loping.net:6000\", protocol: TCP, disable_cert_verification: false }, ElectrumRpcRequest { url: \"electroncash.de:5000\", protocol: TCP, disable_cert_verification: false }] in 5 seconds."
  },
  "id":null
}
```




## enable\_erc20

The `enable_erc20` method allows you to activate additional ERC20 like tokens of a EVM type platform coin. Before using this method, you first need to use the [enable_eth_with_tokens](enable_eth_with_tokens.html) method.

| parameter                                | Type    | Description                                                                                        |
| ---------------------------------------- | ------- | -------------------------------------------------------------------------------------------------- |
| ticker                                   | string  | Ticker of the ERC20 like token coin.                                                               |
| activation_params.required_confirmations | integer | Optional. Confirmations to wait for steps in swap. Defaults to value in the coins file if not set. |

```bash
curl --url "http://127.0.0.1:7783" --data "{
  \"userpass\":\"$userpass\",
  \"method\":\"enable_erc20\",
  \"mmrpc\":\"2.0\",
  \"params\":{
    \"ticker\":\"BAT-ERC20\",
    \"activation_params\": {
      \"required_confirmations\": 3
    }
  }
}"
```

### Response

```json
{
  "mmrpc": "2.0",
  "result": {
    "balances": {
      "0x0d317904AF3BA3A993d557b6cba147FEA4DeB57E": {
        "spendable": "0",
        "unspendable": "0"
      }
    },
    "platform_coin": "ETH",
    "token_contract_address": "0x0d8775f648430679a709e98d2b0cb6250d2887ef",
    "required_confirmations": 3
  },
  "id": null
}
```

### Error - Platform coin is not yet activated

```json
{
  "mmrpc": "2.0",
  "error": "Platform coin ETH is not activated",
  "error_path": "token.lp_coins",
  "error_trace": "token:126] lp_coins:2797]",
  "error_type": "PlatformCoinIsNotActivated",
  "error_data": "ETH",
  "id": null
}
```

### Error - Token already activated

```json
{
  "mmrpc": "2.0",
  "error": "Token BAT-ERC20 is already activated",
  "error_path": "token",
  "error_trace": "token:119]",
  "error_type": "TokenIsAlreadyActivated",
  "error_data": "BAT-ERC20",
  "id": null
}
```

### Error - Token config not found in coins file

```json
{
  "mmrpc": "2.0",
  "error": "Token BATT-ERC20 config is not found",
  "error_path": "token.prelude",
  "error_trace": "token:122] prelude:79]",
  "error_type": "TokenConfigIsNotFound",
  "error_data": "BATT-ERC20",
  "id": null
}
```


## enable\_eth\_with\_tokens

The AtomicDEX-API supports ETH(Ethereum) and many other EVM type platform coins like AVAX(Avalanche), BNB(Binance), FTM(Fantom), MATIC(Polygon), ONE(Harmony), ETH-ARB20(Arbitrum) . Additionally, it supports ERC20 tokens on the ETH chain and associated ERC20 like tokens on the rest of the platform coin chains. Using this method, you can enable a platform coin along with multiple ERC20 like tokens of the platform coin chain in a single command.

| parameter                                    | Type                                                        | Description                                                                                                                                                                                                                                                                                                                                     |
| -------------------------------------------- | ----------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ticker                                       | string                                                      | Ticker of the platform protocol coin. Options: `ETH`, `AVAX`, `BNB`, `FTM`, `MATIC`, `ONE`, `ETH-ARB20`                                                                                                                                                                                                                                         |
| gas_station_url                              | string (optional for ETH/ERC20 and other gas model chains)  | url of [ETH gas station API](https://docs.ethgasstation.info/); The AtomicDEX API uses [eth_gasPrice RPC API](https://github.com/ethereum/wiki/wiki/JSON-RPC#eth_gasprice) by default; when this parameter is set, the AtomicDEX API will request the current gas price from Station for new transactions, and this often results in lower fees |
| gas_station_decimals                         | integer (optional for ETH/ERC20 and other gas model chains) | Defines the decimals used to denominate the gas station response to gwei units. For example, the ETH gas station uses 8 decimals, which means that "average": 860 is equal to 86 gwei. While the Matic gas station uses 9 decimals, so 860 would mean 860 gwei exactly. Defaults to `8`                                                         |
| gas_station_policy.policy                    | string (optional for ETH/ERC20 and other gas model chains)  | Defines the method of gas price calculation from the station response. `"MeanAverageFast"` will use the mean between average and fast fields. `"Average"` will return a simple average value. Defaults to `"MeanAverageFast"`.                                                                                                                  |
| mm2                                          | integer                                                     | Required if not set in `coins` file. Informs the AtomicDEX API whether or not the coin is expected to function. Accepted values are `0` or `1`                                                                                                                                                                                                  |
| priv_key_policy                              | string (optional)                                           | defaults to `ContextPrivKey`. value can be `ContextPrivKey`,`Trezor` when AtomicDEX-API is built for native platforms. value can be `ContextPrivKey`, `Trezor`, `Metamask` when the AtomicDEX-API is built targeting `wasm`                                                                                                                     |
| swap_contract_address                        | string                                                      | address of etomic swap smart contract                                                                                                                                                                                                                                                                                                           |
| fallback_swap_contract                       | string                                                      | address of backup etomic swap smart contract                                                                                                                                                                                                                                                                                                    |
| nodes                                        | array of objects                                            | objects describing each of the nodes to connect to                                                                                                                                                                                                                                                                                              |
| nodes.url                                    | string                                                      | url of a node                                                                                                                                                                                                                                                                                                                                   |
| nodes.gui_auth                               | bool (optional)                                             | must be set to `true` for nodes run officially by the Komodo Platform team                                                                                                                                                                                                                                                                      |
| rpc_mode                                     | string (optional)                                           | defaults to `Http`, value can be `Metamask` when the AtomicDEX-API is built targeting `wasm`                                                                                                                                                                                                                                                    |
| tx_history                                   | bool                                                        | If `true` the AtomicDEX API will preload transaction history as a background process. Must be set to `true` to use the [my_tx_history](../../../basic-docs/atomicdex-api-legacy/my_tx_history.html#my-tx-history) method                                                                                                                        |
| erc20_tokens_requests                        | array of objects                                            | objects describing each of the tokens to be enabled                                                                                                                                                                                                                                                                                             |
| erc20_tokens_requests.ticker                 | string                                                      | Ticker of the token to be enabled                                                                                                                                                                                                                                                                                                               |
| erc20_tokens_requests.required_confirmations | integer                                                     | when the token is involved, the number of confirmations for the AtomicDEX API to wait during the transaction steps of an atomic swap.                                                                                                                                                                                                           |
| required_confirmations                       | integer (optional, defaults to `3`)                         | when the platform coin is involved, the number of confirmations for the AtomicDEX API to wait during the transaction steps of an atomic swap                                                                                                                                                                                                    |
| requires_notarization                        | boolean (optional, defaults to `false`)                     | If `true`, coins protected by [Komodo Platform's dPoW security](https://satindergrewal.medium.com/delayed-proof-of-work-explained-9a74250dbb86) will wait for a notarization before progressing to the next atomic swap transactions step.                                                                                                      |

### Example

```bash
curl --url "http://127.0.0.1:7783" --data "{
  \"userpass\": \"$userpass\",
  \"method\": \"enable_eth_with_tokens\",
  \"mmrpc\": \"2.0\",
  \"params\": {
    \"ticker\": \"ETH\",
    \"gas_station_url\": \"https://ethgasstation.info/json/ethgasAPI.json\",
    \"gas_station_decimals\": 8,
    \"gas_station_policy\": {
      \"policy\": \"MeanAverageFast\"
    },
    \"mm2\": 1,
    \"priv_key_policy\": \"ContextPrivKey\",
    \"swap_contract_address\": \"0x24ABE4c71FC658C91313b6552cd40cD808b3Ea80\",
    \"fallback_swap_contract\": \"0x8500AFc0bc5214728082163326C2FF0C73f4a871\",
    \"nodes\": [
      {
        \"url\": \"http://eth1.cipig.net:8555\",
        \"gui_auth\": false
      },
      {
        \"url\": \"http://eth2.cipig.net:8555\",
        \"gui_auth\": false
      },
      {
        \"url\": \"http://eth3.cipig.net:8555\",
        \"gui_auth\": false
      },
      {
        \"url\": \"https://node.komodo.live:8080/ethereum\",
        \"gui_auth\": true
      }
    ],
    \"rpc_mode\": \"Http\",
    \"tx_history\": true,
    \"erc20_tokens_requests\": [
      {
        \"ticker\": \"APE-ERC20\",
        \"required_confirmations\": 4
      },
      {
        \"ticker\": \"BCH-ERC20\",
        \"required_confirmations\": 4
      },
      {
        \"ticker\": \"MINDS-ERC20\",
        \"required_confirmations\": 4
      },
      {
        \"ticker\": \"BUSD-ERC20\",
        \"required_confirmations\": 4
      }
    ],
    \"required_confirmations\": 5,
    \"requires_notarization\": false
  }
}"
```

### Response

```json
{
  "mmrpc": "2.0",
  "result": {
    "current_block": 16145371,
    "eth_addresses_infos": {
      "0x0d317904AF3BA3A993d557b6cba147FEA4DeB57E": {
        "derivation_method": { "type": "Iguana" },
        "pubkey": "042e6e6b1ca76a7cd1fd2e1ed13bdc83909ad17b17748781308abe55caf863bec6dac19a0ead812d54c8c07508e2f30a258628832c1337f4fdd423a879f67bc823",
        "balances": { "spendable": "0", "unspendable": "0" }
      }
    },
    "erc20_addresses_infos": {
      "0x0d317904AF3BA3A993d557b6cba147FEA4DeB57E": {
        "derivation_method": { "type": "Iguana" },
        "pubkey": "042e6e6b1ca76a7cd1fd2e1ed13bdc83909ad17b17748781308abe55caf863bec6dac19a0ead812d54c8c07508e2f30a258628832c1337f4fdd423a879f67bc823",
        "balances": {
          "MINDS-ERC20": { "spendable": "0", "unspendable": "0" },
          "APE-ERC20": { "spendable": "0", "unspendable": "0" },
          "BUSD-ERC20": { "spendable": "0", "unspendable": "0" },
          "BCH-ERC20": { "spendable": "0", "unspendable": "0" }
        }
      }
    }
  },
  "id": null
}
```

### Errors

#### Error (The platform coin you are trying to activate is already activated)

In this case, you need to [disable](../atomicdex-api-legacy/disable_coin.md) the platform coin and try again.

```json
{
  "mmrpc": "2.0",
  "error": "ETH",
  "error_path": "platform_coin_with_tokens",
  "error_trace": "platform_coin_with_tokens:297]",
  "error_type": "PlatformIsAlreadyActivated",
  "error_data": "ETH",
  "id": null
}
```

#### Error (Config of the platform coin you are trying to activate is not found)

```json
{
  "mmrpc": "2.0",
  "error": "Platform ETH config is not found",
  "error_path": "platform_coin_with_tokens.prelude",
  "error_trace": "platform_coin_with_tokens:302] prelude:79]",
  "error_type": "PlatformConfigIsNotFound",
  "error_data": "ETH",
  "id": null
}
```

#### Error (Parsing the protocol of the platform coin you are trying to activate failed)

```json
{
  "mmrpc": "2.0",
  "error": "Platform coin ETH protocol parsing failed: invalid type: null, expected adjacently tagged enum CoinProtocol",
  "error_path": "platform_coin_with_tokens.prelude",
  "error_trace": "platform_coin_with_tokens:302] prelude:82]",
  "error_type": "CoinProtocolParseError",
  "error_data": {
    "ticker": "ETH",
    "error": "invalid type: null, expected adjacently tagged enum CoinProtocol"
  },
  "id": null
}
```

#### Error (Unexpected platform protocol found for the platform coin you are trying to activate)

```json
{
  "mmrpc": "2.0",
  "error": "Unexpected platform protocol QTUM for ETH",
  "error_path": "platform_coin_with_tokens.prelude.eth_with_token_activation",
  "error_trace": "platform_coin_with_tokens:302] prelude:90] eth_with_token_activation:64]",
  "error_type": "UnexpectedPlatformProtocol",
  "error_data": { "ticker": "ETH", "protocol": { "type": "QTUM" } },
  "id": null
}
```

#### Error (Config of the token you are trying to activate is not found)

```json
{
  "mmrpc": "2.0",
  "error": "Token BTUSD-ERC20 config is not found",
  "error_path": "platform_coin_with_tokens.prelude",
  "error_trace": "platform_coin_with_tokens:314] platform_coin_with_tokens:109] prelude:79]",
  "error_type": "TokenConfigIsNotFound",
  "error_data": "BTUSD-ERC20",
  "id": null
}
```

#### Error (Parsing the protocol of the token you are trying to activate failed)

```json
{
  "mmrpc": "2.0",
  "error": "Token BUSD-ERC20 protocol parsing failed: unknown variant `TERC20`, expected one of `UTXO`, `QTUM`, `QRC20`, `ETH`, `ERC20`, `SLPTOKEN`, `BCH`, `TENDERMINT`, `TENDERMINTTOKEN`, `LIGHTNING`, `SOLANA`, `SPLTOKEN`, `ZHTLC`",
  "error_path": "platform_coin_with_tokens.prelude",
  "error_trace": "platform_coin_with_tokens:314] platform_coin_with_tokens:109] prelude:82]",
  "error_type": "TokenProtocolParseError",
  "error_data": {
    "ticker": "BUSD-ERC20",
    "error": "unknown variant `TERC20`, expected one of `UTXO`, `QTUM`, `QRC20`, `ETH`, `ERC20`, `SLPTOKEN`, `BCH`, `TENDERMINT`, `TENDERMINTTOKEN`, `LIGHTNING`, `SOLANA`, `SPLTOKEN`, `ZHTLC`"
  },
  "id": null
}
```

#### Error (Unexpected protocol is found in the config of the token you are trying to activate)

```json
{
  "mmrpc": "2.0",
  "error": "Unexpected token protocol QRC20 { platform: \"ETH\", contract_address: \"0x4Fabb145d64652a948d72533023f6E7A623C7C53\" } for BUSD-ERC20",
  "error_path": "platform_coin_with_tokens.prelude.erc20_token_activation",
  "error_trace": "platform_coin_with_tokens:314] platform_coin_with_tokens:109] prelude:90] erc20_token_activation:58]",
  "error_type": "UnexpectedTokenProtocol",
  "error_data": {
    "ticker": "BUSD-ERC20",
    "protocol": {
      "type": "QRC20",
      "protocol_data": {
        "platform": "ETH",
        "contract_address": "0x4Fabb145d64652a948d72533023f6E7A623C7C53"
      }
    }
  },
  "id": null
}
```

#### Misc Errors

| Structure                  | Type   | Description                                                   |
| -------------------------- | ------ | ------------------------------------------------------------- |
| PlatformCoinCreationError  | string | There was an error when trying to activate the platform coin  |
| PrivKeyNotAllowed          | string | The privkey is not allowed                                    |
| UnexpectedDerivationMethod | string | The derivation method used is unexpected                      |
| Transport                  | string | The request was failed due to a network error                 |
| InternalError              | string | The request was failed due to an AtomicDEX API internal error |


## enable\_slp

The `enable_slp` method allows you to activate additional SLP tokens. Before using this method, you first need to use the [enable_bch_with_tokens](enable_bch_with_tokens.html) method.


| parameter                                 | Type     | Description                               |
| ----------------------------------------- | -------- | ----------------------------------------- |
| ticker                                    | string   | Ticker of the SLP token. |
| activation_params.required_confirmations  | integer  | Optional. Confirmations to wait for steps in swap. Defaults to value in the coins file if not set. |

```bash
curl --url "http://127.0.0.1:7783" --data "{
  \"userpass\":\"$userpass\",
  \"method\":\"enable_slp\",
  \"mmrpc\":\"2.0\",
  \"params\":{
    \"ticker\":\"SPICE-SLP\",
    \"activation_params\": {
      \"required_confirmations\": 3
    }
  }
}"
```


### Response

```json

{
  "mmrpc":"2.0",
  "result":{
    "balances":{
      "simpleledger:qrf5vpn78s7rjexrjhlwyzzeg7gw98k7t5va3wuz4v":{
        "spendable":"0","unspendable":"0"
      }
    },
    "token_id":"7f8889682d57369ed0e32336f8b7e0ffec625a35cca183f4e81fde4e71a538a1",
    "platform_coin":"BCH",
    "required_confirmations":3
  },
  "id":null
}
```

### Error - BCH not yet activated

```json
{
  "mmrpc":"2.0",
  "error":"Platform coin BCH is not activated",
  "error_path":"token.lp_coins",
  "error_trace":"token:102] lp_coins:1924]",
  "error_type":"PlatformCoinIsNotActivated",
  "error_data":"BCH",
  "id":null
}
```

### Error - Token already activated

```json
{
  "mmrpc":"2.0",
  "error":"Token SPICE-SLP is already activated",
  "error_path":"token",
  "error_trace":"token:95]",
  "error_type":"TokenIsAlreadyActivated",
  "error_data":"SPICE-SLP",
  "id":null
}
```


### Error - Token config not found in coins file

```json
{
  "mmrpc":"2.0",
  "error":"Token SPICE-SLP-WRONG config is not found",
  "error_path":"token.prelude",
  "error_trace":"token:98] prelude:56]",
  "error_type":"TokenConfigIsNotFound",
  "error_data":"SPICE-SLP-WRONG",
  "id":null
}
```


## enable\_tendermint\_token

The `enable_tendermint_token` method allows you to activate additional Tendermint assets. Before using this method, you first need to use the [enable_tendermint_with_assets](enable_tendermint_with_assets.html) method.

| parameter                                | Type    | Description                                                                                        |
| ---------------------------------------- | ------- | -------------------------------------------------------------------------------------------------- |
| ticker                                   | string  | Ticker of the Tendermint asset.                                                                    |
| activation_params.required_confirmations | integer | Optional. Confirmations to wait for steps in swap. Defaults to value in the coins file if not set. |

```bash
curl --url "http://127.0.0.1:7783" --data "{
  \"userpass\":\"$userpass\",
  \"method\":\"enable_tendermint_token\",
  \"mmrpc\":\"2.0\",
  \"params\":{
    \"ticker\":\"ATOM-IBC_IRIS\",
    \"activation_params\": {
      \"required_confirmations\": 3
    }
  }
}"
```

### Response

```json
{
  "mmrpc": "2.0",
  "result": {
    "balances": {
      "iaa16drqvl3u8sukfsu4lm3qsk28jr3fahja9vsv6k": {
        "spendable": "0.028306",
        "unspendable": "0"
      }
    },
    "platform_coin": "IRIS"
  },
  "id": null
}
```

### Error - Platform coin is not yet activated

```json
{
  "mmrpc": "2.0",
  "error": "Platform coin IRIS is not activated",
  "error_path": "token.lp_coins",
  "error_trace": "token:126] lp_coins:2847]",
  "error_type": "PlatformCoinIsNotActivated",
  "error_data": "IRIS",
  "id": null
}
```

### Error - Token already activated

```json
{
  "mmrpc": "2.0",
  "error": "Token ATOM-IBC_IRIS is already activated",
  "error_path": "token",
  "error_trace": "token:119]",
  "error_type": "TokenIsAlreadyActivated",
  "error_data": "ATOM-IBC_IRIS",
  "id": null
}
```

### Error - Token config not found in coins file

```json
{
  "mmrpc": "2.0",
  "error": "Token UP-AND-ATOM config is not found",
  "error_path": "token.prelude",
  "error_trace": "token:122] prelude:79]",
  "error_type": "TokenConfigIsNotFound",
  "error_data": "UP-AND-ATOM",
  "id": null
}
```


## enable\_tendermint\_with\_assets

Use this method to activate Tendermint coins (COSMOS/IRIS/OSMOSIS) and IBC assets in a single command.

| parameter                            | Type                                    | Description                                                                                                                                                                                                                                                                                                                                     |
| ------------------------------------ | --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| ticker                               | string                                  | Ticker of the platform protocol coin. Options: `ATOM`, `IRIS`, `OSMOSIS`                                                                                                                                                                   |
| mm2                                  | integer                                 | Required if not set in `coins` file. Informs the AtomicDEX API whether or not the coin is expected to function. Accepted values are `0` or `1`                                                                                             |
| priv_key_policy                      | string (optional)                       | defaults to `ContextPrivKey`. value can be `ContextPrivKey`,`Trezor` when AtomicDEX-API is built for native platforms. value can be `ContextPrivKey`, `Trezor`, `Metamask` when the AtomicDEX-API is built targeting `wasm`                |
| tx_history                           | bool                                    | If `true` the AtomicDEX API will preload transaction history as a background process. Must be set to `true` to use the [my_tx_history](../../../basic-docs/atomicdex-api-legacy/my_tx_history.html#my-tx-history) method                   |
| tokens_params                        | array of objects                        | objects describing each of the tokens to be enabled                                                                                                                                                                                        |
| tokens_params.ticker                 | string                                  | Ticker of the token to be enabled                                                                                                                                                                                                          |
| tokens_params.required_confirmations | integer                                 | when the token is involved, the number of confirmations for the AtomicDEX API to wait during the transaction steps of an atomic swap                                                                                                       |
| required_confirmations               | integer (optional, defaults to `3`)     | when the platform coin is involved, the number of confirmations for the AtomicDEX API to wait during the transaction steps of an atomic swap                                                                                               |
| requires_notarization                | boolean (optional, defaults to `false`) | If `true`, coins protected by [Komodo Platform's dPoW security](https://satindergrewal.medium.com/delayed-proof-of-work-explained-9a74250dbb86) will wait for a notarization before progressing to the next atomic swap transactions step  |

### Example

```bash
curl --url "http://127.0.0.1:7783" --data "{
    \"method\":\"enable_tendermint_with_assets\",
    \"mmrpc\":\"2.0\",
    \"params\": {
        \"ticker\":\"IRIS\",
        \"tokens_params\": [
             {\"ticker\":\"ATOM-IBC_IRIS\"}
        ],
        \"rpc_urls\": [
            \"https://iris.komodo.live/\",
            \"https://rpc.irishub-1.irisnet.org\"
       ],
       \"tx_history\":true
    },
    \"userpass\":\"${userpass}\"
}"; echo


```

### Response

```json
{
  "mmrpc": "2.0",
  "result": {
    "ticker": "IRIS",
    "address": "iaa16drqvl3u8sukfsu4lm3qsk28jr3fahja9vsv6k",
    "current_block": 18036678,
    "balance": {
      "spendable": "198.692769",
      "unspendable": "0"
    },
    "tokens_balances": {
      "ATOM-IBC_IRIS": {
        "spendable": "0.028306",
        "unspendable": "0"
      }
    }
  },
  "id": null
}
```

#### Error (Config of the platform coin you are trying to activate is not found)

```json
{
  "mmrpc": "2.0",
  "error": "Platform WALDO config is not found",
  "error_path": "platform_coin_with_tokens.prelude",
  "error_trace": "platform_coin_with_tokens:302] prelude:79]",
  "error_type": "PlatformConfigIsNotFound",
  "error_data": "WALDO",
  "id": null
}
```

### Error (Platform coin is already activated)

```json
{
  "mmrpc": "2.0",
  "error": "IRIS",
  "error_path": "platform_coin_with_tokens",
  "error_trace": "platform_coin_with_tokens:297]",
  "error_type": "PlatformIsAlreadyActivated",
  "error_data": "IRIS",
  "id": null
}
```

#### Error (Parsing the protocol of the platform coin you are trying to activate failed)

```json
{
  "mmrpc": "2.0",
  "error": "Platform coin IRIS protocol parsing failed: invalid type: null, expected adjacently tagged enum CoinProtocol",
  "error_path": "platform_coin_with_tokens.prelude",
  "error_trace": "platform_coin_with_tokens:302] prelude:82]",
  "error_type": "CoinProtocolParseError",
  "error_data": {
    "ticker": "IRIS",
    "error": "invalid type: null, expected adjacently tagged enum CoinProtocol"
  },
  "id": null
}
```

#### Error (Unexpected platform protocol found for the platform coin you are trying to activate)

```json
{
  "mmrpc": "2.0",
  "error": "Unexpected platform protocol BCH { slp_prefix: \"simpleledger\" } for BCH",
  "error_path": "platform_coin_with_tokens.prelude.tendermint_with_assets_activation",
  "error_trace": "platform_coin_with_tokens:302] prelude:90] tendermint_with_assets_activation:92]",
  "error_type": "UnexpectedPlatformProtocol",
  "error_data": {
    "ticker": "BCH",
    "protocol": {
      "type": "BCH",
      "protocol_data": {
        "slp_prefix": "simpleledger"
      }
    }
  },
  "id": null
}
```

#### Error (Config of the token you are trying to activate is not found)

```json
{
  "mmrpc": "2.0",
  "error": "Token GALT config is not found",
  "error_path": "platform_coin_with_tokens.prelude",
  "error_trace": "platform_coin_with_tokens:314] platform_coin_with_tokens:109] prelude:79]",
  "error_type": "TokenConfigIsNotFound",
  "error_data": "GALT",
  "id": null
}
```

#### Error (Parsing the protocol of the token you are trying to activate failed)

```json
{
  "mmrpc": "2.0",
  "error": "Token BABYDOGE-BEP20 protocol parsing failed: unknown variant `WOOF`, expected one of `UTXO`, `QTUM`, `QRC20`, `ETH`, `ERC20`, `SLPTOKEN`, `BCH`, `TENDERMINT`, `TENDERMINTTOKEN`, `LIGHTNING`, `SOLANA`, `SPLTOKEN`, `ZHTLC`",
  "error_path": "platform_coin_with_tokens.prelude",
  "error_trace": "platform_coin_with_tokens:314] platform_coin_with_tokens:109] prelude:82]",
  "error_type": "TokenProtocolParseError",
  "error_data": {
    "ticker": "BABYDOGE-BEP20",
    "error": "unknown variant `WOOF`, expected one of `UTXO`, `QTUM`, `QRC20`, `ETH`, `ERC20`, `SLPTOKEN`, `BCH`, `TENDERMINT`, `TENDERMINTTOKEN`, `LIGHTNING`, `SOLANA`, `SPLTOKEN`, `ZHTLC`"
  },
  "id": null
}
```

#### Error (Unexpected protocol is found in the config of the token you are trying to activate)

```json
{
  "mmrpc": "2.0",
  "error": "Unexpected token protocol UTXO for KMD",
  "error_path": "platform_coin_with_tokens.prelude.tendermint_with_assets_activation",
  "error_trace": "platform_coin_with_tokens:314] platform_coin_with_tokens:109] prelude:90] tendermint_with_assets_activation:101]",
  "error_type": "UnexpectedTokenProtocol",
  "error_data": {
    "ticker": "KMD",
    "protocol": {
      "type": "UTXO"
    }
  },
  "id": null
}
```

#### Misc Errors

| Structure                  | Type   | Description                                                   |
| -------------------------- | ------ | ------------------------------------------------------------- |
| PlatformCoinCreationError  | string | There was an error when trying to activate the platform coin  |
| PrivKeyNotAllowed          | string | The privkey is not allowed                                    |
| UnexpectedDerivationMethod | string | The derivation method used is unexpected                      |
| Transport                  | string | The request was failed due to a network error                 |
| InternalError              | string | The request was failed due to an AtomicDEX API internal error |


## get\_public\_key\_hash

The `get_public_key_hash` method returns the [RIPEMD-160](https://en.bitcoin.it/wiki/RIPEMD-160) hash version of your public key


##### Arguments

| Structure | Type | Description |
| --------- | ---- | ----------- |
| (none)    |      |             |


##### Response

| Structure       | Type   | Description                        |
| --------------- | ------ | ---------------------------------- |
| public_key_hash | string | User's RIPEMD-160 public key hash  |


##### :pushpin: Examples

##### Command

```bash
curl --url "http://127.0.0.1:7783" --data "
{
     \"userpass\": \"${userpass}\",
     \"mmrpc\": \"2.0\",
     \"method\": \"get_public_key_hash\",
     \"params\": {},
     \"id\": 0
}
"
```

<div style="margin-top: 0.5rem;">



##### Response (success)

```json
{
  "mmrpc": "2.0",
  "result": {
    "public_key_hash": "05aab5342166f8594baf17a7d9bef5d567443327"
  },
  "id": 0
}
```



</div>


## get\_public\_key

The `get_public_key` method returns the compressed secp256k1 pubkey corresponding to the user's seed phrase.

##### Arguments

| Structure | Type | Description |
| --------- | ---- | ----------- |
| (none)    |      |             |

##### Response

| Structure  | Type   | Description      |
| ---------- | ------ | ---------------- |
| public_key | string | User's pubkey    |

##### :pushpin: Examples

##### Command

```bash
curl --url "http://127.0.0.1:7783" --data "
{
     \"userpass\": \"${userpass}\",
     \"mmrpc\": \"2.0\",
     \"method\": \"get_public_key\",
     \"params\": {},
     \"id\": 0
}
"
```

<div style="margin-top: 0.5rem;">



##### Response (success)

```json
{
  "mmrpc":"2.0",
  "result":{
    "public_key":"0366d28a7926fb20287132692c4cef7bc7e00e76da064948676f8549c0ed7114d3"
  },
  "id":0
}
```



</div>


## get_raw_transaction

The `get_raw_transaction` method takes `coin` and `tx_hash` as input, and returns the full signed raw transaction hex for any transaction that is confirmed or within the mempool.

### Arguments
|Structure|Type|Description|
|-------|---|--------|
|coin|string|the name of the coin the user desires to request for the transaction |
|tx_hash|string|hash of the transaction|

### Response
|Structure|Type|Description|
|--------|----|------|
|tx_hex|BytesJson|raw bytes of signed transaction|


##### Examples:

###### Request (RICK)
```bash
curl --url "http://127.0.0.1:7783" --data "{
    \"mmrpc\": \"2.0\",
    \"method\":\"get_raw_transaction\",
    \"userpass\":\"$userpass\",
    \"params\":{\"coin\": \"RICK\",
    \"tx_hash\": \"989360b0225b4e05fa13643e2e306c8eb5c52fa611615dfd30195089010b1c7b\"},
    \"id\":1
}"
```

<div style="margin-top: 0.5rem;">



###### Response (success)
```json
{
  "mmrpc":"2.0",
  "result":{
  "tx_hex":"0400008085202f89025655b6fec358091a4a6b34107e69b10bd7660056d8f2a1e5f8eef0db6aec960100000000494830450221008c89db5e2d93d7674fe152e37344dfd24a0b1d4d382a7e0bcfc5d8190a141d72022050ce4ef929429e7e1a6c4ebd3f72a1a2aa25da1e0df65553a2c657658077ed1d01feffffff79cc137b70c39c9c7c2b9230c818ec684ffe731bf1ae821f91ba9d3e526f55f00000000049483045022100868c71f4a8e1452a3bc8b1d053a846959ab7df63fb0d147e9173f69818bbb1f3022060c7e045a34cf6af61bc3a74dc2db7b8bfa4949bc5919acceed40fc07d8706d201feffffff0240043a0000000000232102afdbba3e3c90db5f0f4064118f79cf308f926c68afd64ea7afc930975663e4c4ac201efc01000000001976a914347f2aedf63bac168c2cc4f075a2850435e20ac188ac96d3c96036dd0e000000000000000000000000"
  },
  "id":0
}
```



</div>


###### Request (ETH)
```bash
curl --url "http://127.0.0.1:7783" --data "{
    \"mmrpc\": \"2.0\",
    \"method\":\"get_raw_transaction\",
    \"userpass\":\"$userpass\",
    \"params\":{\"coin\": \"ETH\",
    \"tx_hash\": \"0x529aca42b6b592cca5d400832c83854135b924cada6e1c41b85f27fa0a0984b9\"},
    \"id\":1
}"
```

<div style="margin-top: 0.5rem;">



###### Response (success)
```json
{
  "mmrpc":"2.0",
  "result":{
  "tx_hex":"f86e8227578503b6ed90e6825208943faaa59e42f616f859d5771cbc07a99412ae44b288026fe9cb1ec6e9a08026a08e04accc3733376cf7b8f8d51c8398fd244fca736277053a7e87093f6db67708a069cc7dbc57094c4cca7828e6f8d92a8221c457ac7b5d0b0562e9d8896f75d1a5"
  },
  "id":0
}
```



</div>



<div style="margin-top: 0.5rem;">

###### Error response (no such coin)


```json
{
  "mmrpc": "2.0",
  "error": "No such coin KMD",
  "error_path": "lp_coins",
  "error_trace": "lp_coins:2234] lp_coins:2156]",
  "error_type": "NoSuchCoin",
  "error_data": {
    "coin": "KMD"
  },
  "id": 0
}
```

###### Error (invalid hash)
```json
{
  "mmrpc": "2.0",
  "error": "Invalid  hash: Invalid input length",
  "error_path": "utxo_common",
  "error_trace": "utxo_common:1809]",
  "error_type": "InvalidHashError",
  "error_data": "Invalid input length",
  "id": 1
}
```

###### Error (invalid EC Signature)
```json
{
  "mmrpc": "2.0",
  "error": "Internal error: eth:3221] Crypto error (Invalid EC signature)",
  "error_path": "eth",
  "error_trace": "eth:543]",
  "error_type": "InternalError",
  "error_data": "eth:3221] Crypto error (Invalid EC signature)",
  "id": 1
}
```



</div>


### Error Types
|Structure|Type|Description|
|-----|-----|------|
|NoSuchCoin|string|The specified coin was not found or is not activated yet|
|InvalidHashError|string|The specified `hash` is not valid
|Transport|string|The request was failed due to a network error|
|HashNotExist|string|The specified `hash` is not exist|
|InternalError|string|The request was failed due to an AtomicDEX API internal error|


## get\_staking\_info


The `get_staking_info` method returns information about your node's staking. Currently QTUM and tQTUM (test tokens avalable at http://testnet-faucet.qtum.info/) have been integrated, but this functionality will be expanded to more coins in future.


#### Arguments

| Structure | Type   | Description                     |
| --------- | ------ | ------------------------------- |
| coin      | string | the coin being staked           |


##### :pushpin: Examples

##### Command

```bash
curl --location --request POST "http://127.0.0.1:7783" \
--header "Content-Type: application/json" \
--data-raw "{
    \"userpass\": \"$userpass\",
    \"mmrpc\": \"2.0\",
    \"method\": \"get_staking_infos\",
    \"params\": {
        \"coin\": \"tQTUM\"
    },
    \"id\": 0
}"
```

<div style="margin-top: 0.5rem;">



##### Response (not currently staking)

```json
{
  "mmrpc":"2.0",
  "result":{
    "staking_infos_details":{
      "type":"Qtum",
      "amount":"0",
      "staker":null,
      "am_i_staking":false,
      "is_staking_supported":true
    }
  },
  "id":0
}
```

##### Response (staking active)

```json
{
  "mmrpc": "2.0",
  "result": {
    "staking_infos_details": {
      "type": "Qtum",
      "amount": "160.16",
      "staker": "qcyBHeSct7Wr4mAw18iuQ1zW5mMFYmtmBE",
      "am_i_staking": true,
      "is_staking_supported": true
    }
  },
  "id": 0
}
```




</div>

## Signing\_and\_Verifying\_Messages

Cryptographically signed messages are a useful feature which can be used to [prove ownership of an address](https://www.coindesk.com/policy/2020/05/25/craig-wright-called-fraud-in-message-signed-with-bitcoin-addresses-he-claims-to-own/).
If your [`coins`](https://github.com/KomodoPlatform/coins) file contains the correct [message prefix](https://bitcoin.stackexchange.com/questions/77324/how-are-bitcoin-signed-messages-generated/77325#77325) definitions for a coin, you can sign messages with the AtomicDEX-API(https://github.com/KomodoPlatform/atomicDEX-API). This can generally be found [within a coin's github repository](https://github.com/KomodoPlatform/komodo/blob/master/src/main.cpp#L146) and is assigned via the `sign_message_prefix` value as below.

```json
{
        "coin": "RICK",
        "asset": "RICK",
        "fname": "RICK (TESTCOIN)",
        "sign_message_prefix": "Komodo Signed Message:\n",
        "rpcport": 25435,
        "txversion": 4,
        "overwintered": 1,
        "mm2": 1,
        "protocol": {
                "type": "UTXO"
        }
}
```

### Message Signing


#### Arguments

| Structure     | Type             | Description                                                                                                           |
| ------------- | ---------------- | --------------------------------------------------------------------------------------------------------------------- |
| coin          | string           | The coin to sign a message with                                                                                       |
| message       | string           | The message you want to sign                                                                                          |


#### Response

| Structure       | Type                       | Description                                                                                     |
| --------------- | -------------------------- | ----------------------------------------------------------------------------------------------- |
| signature       | string                     | The signature generated for the message                                                         |


##### Command

```bash
curl --url "http://127.0.0.1:7783" --data "
{
  \"userpass\": \"$userpass\",
  \"method\": \"sign_message\",
  \"mmrpc\": \"2.0\",
  \"id\": 0,
  \"params\": {
    \"coin\": \"KMD\",
    \"message\": \"Between subtle shading and the absence of light lies the nuance illusion\"
  }
}"
```

##### Response (success)

```json
{
  "mmrpc": "2.0",
  "result": {
    "signature": "H43eTmJxBKEPiHkrCe/8NsRidkKCIkXDxLyp30Ez/RwoApGdg89Hlvj9mTMSPGp8om5297zvdL8EVx3IdIe2swY="
  },
  "id": 0
}
```

#### :warning: Error types

**PrefixNotFound:** sign_message_prefix is not set in coin config file
**CoinIsNotFound:** Specified coin is not found
**InvalidRequest:** Message signing is not supported by the given coin type
**InternalError:** An internal error occured during the signing process


### Message Verification

#### Arguments

| Structure     | Type             | Description                                                                                                           |
| ------------- | ---------------- | --------------------------------------------------------------------------------------------------------------------- |
| coin          | string           | The coin to sign a message with                                                                                       |
| message       | string           | The message input via the `sign_message` method sign                                                                  |
| signature     | string           | The signature generated for the message                                                                               |
| address       | string           | The address used to sign the message                                                                                  |


#### Response

| Structure       | Type                       | Description                                                                                     |
| --------------- | -------------------------- | ----------------------------------------------------------------------------------------------- |
| is_valid        | boolean                    | `true` is message signature is valid; `false` if it is not.                                     |


##### Command

```bash
curl --url "http://127.0.0.1:7783" --data "
{
  \"userpass\": \"$userpass\",
  \"method\": \"verify_message\",
  \"mmrpc\": \"2.0\",
  \"id\": 0,
  \"params\": {
    \"coin\": \"KMD\",
    \"message\": \"Between subtle shading and the absence of light lies the nuance illusion\",
    \"signature\": \"H43eTmJxBKEPiHkrCe/8NsRidkKCIkXDxLyp30Ez/RwoApGdg89Hlvj9mTMSPGp8om5297zvdL8EVx3IdIe2swY=\",
    \"address\": \"RUYJYSTuCKm9gouWzQN1LirHFEYThwzA2d\"
  }
}"
```

##### Response (valid)

```json
{
  "mmrpc": "2.0",
  "result": {
    "is_valid": true
  },
  "id": 0
}
```

##### Response (not valid)

```json
{
  "mmrpc": "2.0",
  "result": {
    "is_valid": false
  },
  "id": 0
}
```


#### :warning: Error types

**PrefixNotFound:** sign_message_prefix is not set in coin config
**CoinIsNotFound:** Specified coin is not found
**InvalidRequest:** Message verification is not supported by the given coin type
**InternalError:** An internal error occured during the verification process
**SignatureDecodingError:** Given signature could not be decoded
**AddressDecodingError:** Given address could not be decoded


## my\_tx\_history

To use this method, you must activate your coin with `"tx_history": true`. The response will vary depending on the coin.
Currently only BCH & SLP tokens are supported in the master/release API. In the latest dev API, UTXO coins, QTUM, and Tendermint/Tendermint tokens are also supported.
For ZHTLC coins, you must use the [z_coin_tx_history](../atomicdex-api-20-dev/zhtlc_coins.html#z_coin_tx_history) method.
For all other coins, use the legacy [my_tx_history](../atomicdex-api-legacy/my_tx_history.html) method.


##### Arguments

| parameter                                 | Type     | Description                               |
| ----------------------------------------- | -------- | ----------------------------------------- |
| coin                                      | string   | Ticker of the coin to get history for.    |
| limit                                     | integer  | Optional. Limits the number of returned transactions. Defaults to `10`. Ignored if `max = true`. |
| paging_options.FromId                     | string   | Optional. AtomicDEX API will skip records until it reaches this ID, skipping the from_id as well; track the internal_id of the last displayed transaction to find the value of this field for the next page |
| paging_options.PageNumber                 | integer  | Optional. AtomicDEX API will return limit swaps from the selected page. Ignored if `FromId` . | 


##### Response

| Structure                                     | Type             | Description                                                                                                                                    |
| --------------------------------------------- | ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| transactions                                  | array of objects | transactions data                                                                                                                              |
| from_id                                       | string           | the from_id specified in the request; this value is null if from_id was not set                                                                |
| skipped                                       | number           | the number of skipped records (i.e. the position of `from_id` in the list + 1); this value is 0 if `from_id` was not set                       |
| limit                                         | number           | the limit that was set in the request; note that the actual number of transactions can differ from the specified limit (e.g. on the last page) |
| total                                         | number           | the total number of transactions available                                                                                                     |
| page_number                                   | number           | the page_number that was set in the request                                                                                                    |
| total_pages                                   | number           | total pages available with the selected limit                                                                                                  |
| current_block                                 | number           | the number of the latest block of coin blockchain                                                                                              |
| sync_status                                   | object           | provides the information that helps to track the progress of transaction history preloading at background                                      |
| sync_status.state                             | string           | current state of sync; possible values: `NotEnabled`, `NotStarted`, `InProgress`, `Error`, `Finished`                                          |
| sync_status.additional_info                   | object           | additional info that helps to track the progress; present for `InProgress` and `Error` states only                                             |
| sync_status.additional_info.blocks_left       | number           | present for ETH/ERC20 coins only; displays the number of blocks left to be processed for `InProgress` state                                    |
| sync_status.additional_info.transactions_left | number           | present for UTXO coins only; displays the number of transactions left to be processed for `InProgress` state                                   |
| sync_status.additional_info.code              | number           | displays the error code for `Error` state                                                                                                      |
| sync_status.additional_info.message           | number           | displays the error message for `Error` state                                                                                                   |


### Request (BCH from page 2)


```bash
curl --url "http://127.0.0.1:7783" --data "{
  \"userpass\":\"$userpass\",
  \"method\":\"my_tx_history\",
  \"mmrpc\":\"2.0\",
  \"params\": {
    \"coin\": \"BCH\",
    \"limit\": 2,
    \"paging_options\": {
      \"PageNumber\": 2
    }
  }
}"
```

<div style="margin-top: 0.5rem;">



#### Response
```json
{
  "mmrpc": "2.0",
  "result": {
    "coin": "BCH",
    "target": {
      "type": "iguana"
    },
    "current_block": 772607,
    "transactions": [
      {
        "tx_hex": "0100000001b7b45d92f8f3413a0e5656258e0a51f5c7e8230c0a08cef2ebec1ddbb8f7c28200000000d747304402203ca957fdfcfbba6123d78afe28b17fd4103cc04f6ada4110eb61c2a0350c29b802204215f203d583e8bcc79bd70f33af4f4e27500b5a5375efe75a1c31ec112f3c344120b3f71dbea00eeace7f09b0911de31e46f76a48036b86ccc207dac55540912e01004c6b6304dbf67563b175210315d9c51c657ab1be4ae9d3ab6e76a619d3bccfe830d5363fa168424c0d044732ac6782012088a914dde61fe24ea3cfa39379c475702692fa2f080900882103ed00156316c46094c0cbcf21a5ee549a1b3a50938c43096ef499ca28059edca6ac68ffffffff0133980200000000001976a91411a1563bfa55ae05fa621b2e245abe5a358c852e88acdbf67563",
        "tx_hash": "e2167df56142bccdb8c620297f1b6ca3f7c8a955332838430d4d0f62530870f9",
        "from": [
          "bitcoincash:ppaa62685yaucdf2a54g3rgtyc9g7yawrvvmqsfumc"
        ],
        "to": [
          "bitcoincash:qqg6z43mlf26up06vgdjufz6hedrtry99cvk5dgcnt"
        ],
        "total_amount": "0.00171035",
        "spent_by_me": "0",
        "received_by_me": "0.00170035",
        "my_balance_change": "0.00170035",
        "block_height": 766923,
        "timestamp": 1668615553,
        "fee_details": {
          "type": "Utxo",
          "coin": "BCH",
          "amount": "0.00001"
        },
        "coin": "BCH",
        "internal_id": "e2167df56142bccdb8c620297f1b6ca3f7c8a955332838430d4d0f62530870f9",
        "transaction_type": "StandardTransfer",
        "confirmations": 5685
      },
      {
        "tx_hex": "0100000001eccfa8c296e7b3e229be28a8ca6a5e5a7e89ee07a2d9441faaf5905679286a3c00000000d7473044022077d38ae45bb7257b152d4cb803aab62ca879cab60e9b3a7ca05ef099078e000402203106be31513c6526c14bdf40b28b4d38f78bb1958fc995e040ac4b2165d9d79141203bffadbc5bf035674f0d0f6e1d1a121fc6d404720679ff9b6610b298b41375a3004c6b6304bc847463b175210315d9c51c657ab1be4ae9d3ab6e76a619d3bccfe830d5363fa168424c0d044732ac6782012088a91457c7ce14c0444edc37ee52ed32b68890b0647cd3882103ed00156316c46094c0cbcf21a5ee549a1b3a50938c43096ef499ca28059edca6ac68ffffffff0163b10200000000001976a91411a1563bfa55ae05fa621b2e245abe5a358c852e88acbc847463",
        "tx_hash": "98ddc27aa161967519f53cb3e91146a23b76ac4e33605f8e827c69f4d9b6de37",
        "from": [
          "bitcoincash:ppnzkha52y53d7r7qn6mq4mcmaadmxzj4clfgneaxv"
        ],
        "to": [
          "bitcoincash:qqg6z43mlf26up06vgdjufz6hedrtry99cvk5dgcnt"
        ],
        "total_amount": "0.00177483",
        "spent_by_me": "0",
        "received_by_me": "0.00176483",
        "my_balance_change": "0.00176483",
        "block_height": 766752,
        "timestamp": 1668519015,
        "fee_details": {
          "type": "Utxo",
          "coin": "BCH",
          "amount": "0.00001"
        },
        "coin": "BCH",
        "internal_id": "98ddc27aa161967519f53cb3e91146a23b76ac4e33605f8e827c69f4d9b6de37",
        "transaction_type": "StandardTransfer",
        "confirmations": 5856
      }
    ],
    "sync_status": {
      "state": "Finished"
    },
    "limit": 2,
    "skipped": 2,
    "total": 16,
    "total_pages": 8,
    "paging_options": {
      "PageNumber": 2
    }
  },
  "id": null
}
```



</div>

### Request (TTT-SLP with FromId)

```bash
curl --url "http://127.0.0.1:7783" --data "{
  \"userpass\":\"$userpass\",
  \"method\":\"my_tx_history\",
  \"mmrpc\":\"2.0\",
  \"params\": {
    \"coin\": \"TTT-SLP\",
    \"limit\": 2,
    \"paging_options\": {
      \"FromId\": \"433b641bc89e1b59c22717918583c60ec98421805c8e85b064691705d9aeb970\"
    }
  }
}"
```



<div style="margin-top: 0.5rem;">



#### Response

```json
{
  "mmrpc": "2.0",
    "result": {
        "coin": "TTT-SLP",
        "target": {
            "type": "iguana"
        },
        "current_block": 772612,
        "transactions": [{
            "tx_hex": "0200000002365a29eb638da7fc57720ad6c99fdbc6cfb9c957920cfb62fd69e494b412c1c1020000006b483045022100de81bca8cfef2f95b3da8aa89edf4f5cc6cf489c565d0965b8142380ef3986f1022062d6ed47f2cd281f4860a27e835949aafbab89eeb0865fbf2280a283dfb7c417412102b9fdfedefde71b21523974b9f24a4b6a1b83c5640b839baa6eb14418cae08191ffffffffc1f73b403f893f93d95b8c7dfa1b59bb5445109d4c51107da1e08fb770e54136010000006a47304402203658375dac3b84ae17e72cf3f5157b8ad25e7caee0629fa8708868974f8d58b402206f38d016ed4e390d783627441685692d21b889d83919abd39368cba28f43f544412102b9fdfedefde71b21523974b9f24a4b6a1b83c5640b839baa6eb14418cae08191ffffffff040000000000000000406a04534c500001010453454e44205321508197ffed321c5fc9a1427e5c68b31d2c1ec92ae1c495f8acb08d8d66cd080000000000002710080000002278c569d322020000000000001976a914d346067e3c3c3964c395fee208594790e29ede5d88ac22020000000000001976a914580af35e3553d57b4b3a2036f4959f10246e98c788ac68955e03000000001976a914580af35e3553d57b4b3a2036f4959f10246e98c788ac00000000",
            "tx_hash": "7b58248f3486079951a57d6dbd41c019a83f2b876c9fa3afa6fcc5a7c595b837",
            "from": ["simpleledger:qpvq4u67x4fa276t8gsrday4nugzgm5ccu4usawss8"],
            "to": ["simpleledger:qpvq4u67x4fa276t8gsrday4nugzgm5ccu4usawss8", "simpleledger:qrf5vpn78s7rjexrjhlwyzzeg7gw98k7t5va3wuz4v"],
            "total_amount": "1480551016.67",
            "spent_by_me": "0",
            "received_by_me": "100",
            "my_balance_change": "100",
            "block_height": 772211,
            "timestamp": 1671817336,
            "fee_details": {
                "type": "Utxo",
                "coin": "BCH",
                "amount": "0.00000482"
            },
            "coin": "TTT-SLP",
            "internal_id": "57b78eb912a704921640a589d8bb42bb147dfb88c3d1b4b2e3df910be6b9ab31",
            "transaction_type": {
                "TokenTransfer": "5321508197ffed321c5fc9a1427e5c68b31d2c1ec92ae1c495f8acb08d8d66cd"
            },
            "confirmations": 402
        }],
        "sync_status": {
            "state": "Finished"
        },
    "limit":10,
    "skipped":0,
    "total":1,
    "total_pages":1,
    "paging_options":{
      "FromId":"433b641bc89e1b59c22717918583c60ec98421805c8e85b064691705d9aeb970"
    }
  },
  "id":null
}
```



</div>

### Request (IRIS with limit = 50)

```bash
curl --url "http://127.0.0.1:7783" --data "{
  \"userpass\":\"$userpass\",
  \"method\":\"my_tx_history\",
  \"mmrpc\":\"2.0\",
  \"params\": {
    \"coin\": \"IRIS\",
    \"limit\": 50
  }
}"
```

<div style="margin-top: 0.5rem;">



#### Response

```json
{
  "mmrpc": "2.0",
  "result": {
    "coin": "IRIS",
    "target": {
      "type": "iguana"
    },
    "current_block": 18120346,
    "transactions": [{
      "tx_hex": "0a2a6961613136647271766c33753873756b667375346c6d3371736b32386a72336661686a6139767376366b122a6961613136647271766c33753873756b667375346c6d3371736b32386a72336661686a6139767376366b1a110a05756972697312083130303030303030",
      "tx_hash": "B34A8D5AD74067F01A0207DF1851A14673C859D8A6F4FB0CBE292D2104C143CA",
      "from": ["iaa16drqvl3u8sukfsu4lm3qsk28jr3fahja9vsv6k"],
      "to": ["iaa16drqvl3u8sukfsu4lm3qsk28jr3fahja9vsv6k"],
      "total_amount": "10.044559",
      "spent_by_me": "10.044559",
      "received_by_me": "10",
      "my_balance_change": "-0.044559",
      "block_height": 18120218,
      "timestamp": 1673016440,
      "fee_details": {
        "type": "Tendermint",
        "coin": "IRIS",
        "amount": "0.044559",
        "gas_limit": 100000
      },
      "coin": "IRIS",
      "internal_id": "4644373032304131304637363034374441354438413433420000000000000000",
      "transaction_type": "StandardTransfer",
      "memo": "while you are out, buy milk",
      "confirmations": 129
    }, {
      "tx_hex": "0a2a6961613136647271766c33753873756b667375346c6d3371736b32386a72336661686a6139767376366b122a696161317a78733476776c36326b687174376e7a7276687a676b34377467366365706677707a673537711a4d0a446962632f3237333934464230393244324543434435363132334337344633364534433146393236303031434541444139434139374541363232423235463431453545423212053130303030",
      "tx_hash": "09ADDD3427A3BA4B0A94023456DF534DB5B9B6821EC17C7C1B2C168EFCF49F26",
      "from": ["iaa16drqvl3u8sukfsu4lm3qsk28jr3fahja9vsv6k"],
      "to": [],
      "total_amount": "0.051788",
      "spent_by_me": "0.051788",
      "received_by_me": "0",
      "my_balance_change": "-0.051788",
      "block_height": 17996530,
      "timestamp": 1672232661,
      "fee_details": {
        "type": "Tendermint",
        "coin": "IRIS",
        "amount": "0.051788",
        "gas_limit": 100000
      },
      "coin": "IRIS",
      "internal_id": "0000000000000000303941444444333432374133424134423041393430323334",
      "transaction_type": "FeeForTokenTx",
      "memo": null,
      "confirmations": 123817
    }, {
      "tx_hex": "0a2a6961613136647271766c33753873756b667375346c6d3371736b32386a72336661686a6139767376366b1240343133433843414333434142363945454632344432423643414238314146454344383044413745323731433237343637453142324635463337314446353241441a4061353539343834666536316665383630326465383632353964643263663031613865393437306437666635346262323536336233393035646462366238366535",
      "tx_hash": "4E30C074CED6825F3E1B6584C376A426C20FDEFC9A22EB17D8E7DA4139FA0AEB",
      "from": ["iaa16drqvl3u8sukfsu4lm3qsk28jr3fahja9vsv6k"],
      "to": [],
      "total_amount": "182.742425",
      "spent_by_me": "0.053103",
      "received_by_me": "182.689322",
      "my_balance_change": "182.636219",
      "block_height": 17981793,
      "timestamp": 1672138900,
      "fee_details": {
        "type": "Tendermint",
        "coin": "IRIS",
        "amount": "0.053103",
        "gas_limit": 100000
      },
      "coin": "IRIS",
      "internal_id": "3438353642314533463532383644454334373043303345340000000000000000",
      "transaction_type": {
        "CustomTendermintMsg": {
          "msg_type": "SignClaimHtlc",
          "token_id": null
        }
      },
      "memo": null,
      "confirmations": 138554
    }],
    "sync_status": {
      "state": "NotStarted"
    },
    "limit": 50,
    "skipped": 0,
    "total": 3,
    "total_pages": 1,
    "paging_options": {
      "PageNumber": 1
    }
  },
  "id": null
}
```



</div>

### Error cases


#### Error - Coin not active
```json
{
  "mmrpc": "2.0",
  "error": "TTT-SLP",
  "error_path": "my_tx_history_v2.lp_coins",
  "error_trace": "my_tx_history_v2:389] lp_coins:2847]",
  "error_type": "CoinIsNotActive",
  "error_data": "TTT-SLP",
  "id": null
}
```

#### Error - Coin not compatible
```json
{
  "mmrpc":"2.0",
  "error":"TTT-SLP",
  "error_path":"my_tx_history_v2",
  "error_trace":"my_tx_history_v2:336]",
  "error_type":"NotSupportedFor",
  "error_data":"TTT-SLP",
  "id":null
}
```

#### Error - Coin enabled without tx_history = true
```json
{
  "mmrpc":"2.0",
  "error":"Storage is not initialized for TTT-SLP",
  "error_path":"my_tx_history_v2",
  "error_trace":"my_tx_history_v2:343]",
  "error_type":"StorageIsNotInitialized",
  "error_data":"Storage is not initialized for TTT-SLP",
  "id":null
}
```


#### Error - Local database failed
```json
{
  "mmrpc":"2.0",
  "error":"SqliteFailure(Error { code: Unknown, extended_code: 1 }, Some(\"no such column: block_height\"))",
  "error_path":"my_tx_history_v2.sql_tx_history_storage",
  "error_trace":"my_tx_history_v2:351] sql_tx_history_storage:472]",
  "error_type":"StorageError",
  "error_data":"SqliteFailure(Error { code: Unknown, extended_code: 1 }, Some(\"no such column: block_height\"))",
  "id":null
}
```



## AtomicDEX API RPC Protocol v2.0

Starting with version [beta-2.1.3434](https://github.com/KomodoPlatform/atomicDEX-API/releases/tag/beta-2.1.3434), the AtomicDEX API supports the standardized protocol format called `mmrpc 2.0`.

It includes a uniform request, successful and error response formats. At the moment, only a few RPC methods support the `mmrpc 2.0` protocol.


#### Request

| Structure | Type              | Description                                                                                                                                                       |
| --------- | ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| mmrpc     | string            | the string specifying the version of the AtomicDEX API RPC protocol. Must be exactly "2.0"                                                                        |
| userpass  | string (optional) | your password for protected RPC methods. Skip this field if the specified `method` is public                                                                      |
| method    | string            | the name of the method to be invoked                                                                                                                              |
| params    | object (optional) | a structured value that holds the parameter values to be used during the invocation of the method. This field may be omitted if the method doesn't take arguments |
| id        | number (optional) | the identifier is established by the client. AtomicDEX API will reply with the same value in the Response object if the `id` field is included and not `NULL`     |

#### Response (Success)

| Structure | Type              | Description                                                                                 |
| --------- | ----------------- | ------------------------------------------------------------------------------------------- |
| mmrpc     | string            | the string specifying the version of the AtomicDEX API RPC protocol                         |
| result    | object            | the value of this field is determined by the method invoked on AtomicDEX API                |
| id        | number (optional) | the identifier established by the client. The same value as in the Request if it was passed |

#### Response (Error)

| Structure   | Type              | Description                                                                                 |
| ----------- | ----------------- | ------------------------------------------------------------------------------------------- |
| mmrpc       | string            | the string specifying the version of the AtomicDEX API RPC protocol                         |
| error       | string            | the common error description                                                                |
| error_path  | string            | the error path consisting of file names separated by a dot similar to JSON path notation    |
| error_trace | string            | the error path consisting of file and line number pairs separated by ']'                    |
| error_type  | string            | the string error identifier used to determine the cause of the error                        |
| error_data  | object            | an object containing the error data of the corresponding `error_type`                       |
| id          | number (optional) | the identifier established by the client. The same value as in the Request if it was passed |

#### :pushpin: Examples

##### Command

```bash
curl --url "http://127.0.0.1:7783" --data "{\"mmrpc\":\"2.0\",\"userpass\":\"$userpass\",\"method\":\"withdraw\",\"params\":{\"coin\":\"KMD\",\"to\":\"RJTYiYeJ8eVvJ53n2YbrVmxWNNMVZjDGLh\",\"amount\":\"10\"},\"id\":0}"
```

##### Response (success)

```json
{
  "mmrpc": "2.0",
  "result": {
    "tx_hex": "0400008085202f8901ef25b1b7417fe7693097918ff90e90bba1351fff1f3a24cb51a9b45c5636e57e010000006b483045022100b05c870fcd149513d07b156e150a22e3e47fab4bb4776b5c2c1b9fc034a80b8f022038b1bf5b6dad923e4fb1c96e2c7345765ff09984de12bbb40b999b88b628c0f9012102031d4256c4bc9f99ac88bf3dba21773132281f65f9bf23a59928bce08961e2f3ffffffff0200e1f505000000001976a91405aab5342166f8594baf17a7d9bef5d56744332788ac8cbaae5f010000001976a91405aab5342166f8594baf17a7d9bef5d56744332788ace87a5e5d000000000000000000000000000000",
    "tx_hash": "1ab3bc9308695960bc728fa427ac00d1812c4ae89aaa714c7618cb96d111be58",
    "from": ["R9o9xTocqr6CeEDGDH6mEYpwLoMz6jNjMW"],
    "to": ["R9o9xTocqr6CeEDGDH6mEYpwLoMz6jNjMW"],
    "total_amount": "60.10253836",
    "spent_by_me": "60.10253836",
    "received_by_me": "60.00253836",
    "my_balance_change": "-0.1",
    "block_height": 0,
    "timestamp": 1566472936,
    "fee_details": {
      "type": "Utxo",
      "amount": "0.1"
    },
    "coin": "RICK",
    "internal_id": ""
  },
  "id": 0
}
```

##### Response (error)

```json
{
  "mmrpc": "2.0",
  "error": "The amount 0.000005 is too small",
  "error_path": "utxo_common",
  "error_trace": "utxo_common:1379] utxo_common:301]",
  "error_type": "AmountIsTooSmall",
  "error_data": {
    "amount": "0.000005"
  },
  "id": 0
}
```


## recreate\_swap\_data

The `recreate_swap_data` can assist in the event of local stored swap data being lost due to storage errors related to low disk space or hardware failure, and if required, aid with the refunding of failed swaps.

To source the opposite side of the trade, please [contact the Komodo Support team on Discord](https://discord.gg/RRZ8hzc). You will need to provide details about the trade you are trying to recover, such as the coins and amounts being traded, the approximate time of the trade, any known transaction IDs involved in the trade, and if available the UUID of the trade. 

##### Arguments

| Structure | Type   | Description                                    |
| --------- | ------ | ---------------------------------------------- |
| swap      | object | Swap data from other side of trade. For example to recreate a Maker's swap data, the input would be the corresponding Taker's swap data            |

##### Response

| Structure | Type   | Description               |
| --------- | ------ | ------------------------- |
| result    | object | Opposite side's swap data. For example if a Taker's swap data is input, the reponse would be the corresponding Maker's swap data.            |

##### :pushpin: Examples

##### Command

```bash
curl --url "http://127.0.0.1:7783" --data "
{
     \"userpass\": \"${userpass}\",
     \"mmrpc\": \"2.0\",
     \"method\": \"recreate_swap_data\",
     \"params\": {
          \"swap\": {
              \"type\": \"Taker\",
              \"uuid\": \"f87fa9ce-0820-4675-b85d-db18c7bc9fb4\",
              \"my_order_uuid\": \"f87fa9ce-0820-4675-b85d-db18c7bc9fb4\",
              \"events\": [
                  {
                      \"timestamp\": 1638984440546,
                      \"event\": {
                          \"type\": \"Started\",
                          \"data\": {
                              \"taker_coin\": \"MORTY\",
                              \"maker_coin\": \"RICK\",
                              \"maker\": \"15d9c51c657ab1be4ae9d3ab6e76a619d3bccfe830d5363fa168424c0d044732\",
                              \"my_persistent_pub\": \"03b1e544ce2d860219bc91314b5483421a553a7b33044659eff0be9214ed58addd\",
                              \"lock_duration\": 7800,
                              \"maker_amount\": \"0.9090909090909090909090909090909090909090909090909090909090909090909090909090909090909090909090909091\",
                              \"taker_amount\": \"1\",
                              \"maker_payment_confirmations\": 1,
                              \"maker_payment_requires_nota\": false,
                              \"taker_payment_confirmations\": 1,
                              \"taker_payment_requires_nota\": false,
                              \"taker_payment_lock\": 1638992240,
                              \"uuid\": \"f87fa9ce-0820-4675-b85d-db18c7bc9fb4\",
                              \"started_at\": 1638984440,
                              \"maker_payment_wait\": 1638987560,
                              \"maker_coin_start_block\": 1207822,
                              \"taker_coin_start_block\": 1222573,
                              \"fee_to_send_taker_fee\": {
                                  \"coin\": \"MORTY\",
                                  \"amount\": \"0.00001\",
                                  \"paid_from_trading_vol\": false
                              },
                              \"taker_payment_trade_fee\": {
                                  \"coin\": \"MORTY\",
                                  \"amount\": \"0.00001\",
                                  \"paid_from_trading_vol\": false
                              },
                              \"maker_payment_spend_trade_fee\": {
                                  \"coin\": \"RICK\",
                                  \"amount\": \"0.00001\",
                                  \"paid_from_trading_vol\": true
                              }
                          }
                      }
                  },
                  {
                      \"timestamp\": 1638984456603,
                      \"event\": {
                          \"type\": \"Negotiated\",
                          \"data\": {
                              \"maker_payment_locktime\": 1639000040,
                              \"maker_pubkey\": \"0315d9c51c657ab1be4ae9d3ab6e76a619d3bccfe830d5363fa168424c0d044732\",
                              \"secret_hash\": \"4da9e7080175e8e10842e0e161b33cd298cab30b\",
                              \"maker_coin_swap_contract_addr\": null,
                              \"taker_coin_swap_contract_addr\": null
                          }
                      }
                  },
                  {
                      \"timestamp\": 1638984456814,
                      \"event\": {
                          \"type\": \"TakerFeeSent\",
                          \"data\": {
                              \"tx_hex\": \"0400008085202f89016383e8aced2256378bb126a1ca1a41e2f344d9295f65b3ea4b99055c5eb4a6cb000000006a47304402201c7e661e0dbeb9b3eb6e4e9e3194010e5772227017772b2e48c1b8d48ed3b21f02201c2eda64e74455fa1878a5c221f25d22fe626abd0078a26a9fc0f829e0921639012103b1e544ce2d860219bc91314b5483421a553a7b33044659eff0be9214ed58adddffffffff02bcf60100000000001976a914ca1e04745e8ca0c60d8c5881531d51bec470743f88ac74c3e90b000000001976a91483762a373935ca241d557dfce89171d582b486de88ac08ebb061000000000000000000000000000000\",
                              \"tx_hash\": \"fcb49167c79e8e014143643b94878866f7e80b26c5a5dcf693010543da70b5bc\"
                          }
                      }
                  },
                  {
                      \"timestamp\": 1638984457822,
                      \"event\": {
                          \"type\": \"MakerPaymentReceived\",
                          \"data\": {
                              \"tx_hex\": \"0400008085202f8901c41fdf6b9d8aea4b472f83e4fa0d99dfafc245e897d681fd2ca7df30707fbf48020000006b483045022100c7b294bd46cbf3b13530879a43c5cf67414047266d8b64c3c7263b5e75b989ba02201974f38d688b184bc44e628806c6ab2ac9092f394729d0ce838f14e1e76117c001210315d9c51c657ab1be4ae9d3ab6e76a619d3bccfe830d5363fa168424c0d044732ffffffff03a2296b050000000017a91491c45f69e1760c12a1f90fb2a811f6dfde35cc35870000000000000000166a144da9e7080175e8e10842e0e161b33cd298cab30bac503d64000000001976a9141462c3dd3f936d595c9af55978003b27c250441f88ac09ebb061000000000000000000000000000000\",
                              \"tx_hash\": \"6287e0d30951cd859bfb837eb1e5409f7596e75ffeb2e61fd6df1843bfd0203d\"
                          }
                      }
                  },
                  {
                      \"timestamp\": 1638984457826,
                      \"event\": {
                          \"type\": \"MakerPaymentWaitConfirmStarted\"
                      }
                  },
                  {
                      \"timestamp\": 1638984503611,
                      \"event\": {
                          \"type\": \"MakerPaymentWaitConfirmFailed\",
                          \"data\": {
                              \"error\": \"An error\"
                          }
                      }
                  },
                  {
                      \"timestamp\": 1638984503615,
                      \"event\": {
                          \"type\": \"Finished\"
                      }
                  }
              ],
              \"maker_amount\": \"0.9090909090909090909090909090909090909090909090909090909090909090909090909090909090909090909090909091\",
              \"maker_coin\": \"RICK\",
              \"taker_amount\": \"1\",
              \"taker_coin\": \"MORTY\",
              \"gui\": \"atomicDEX 0.5.1 iOS\",
              \"mm_version\": \"1b065636a\",
              \"success_events\": [
                  \"Started\",
                  \"Negotiated\",
                  \"TakerFeeSent\",
                  \"MakerPaymentReceived\",
                  \"MakerPaymentWaitConfirmStarted\",
                  \"MakerPaymentValidatedAndConfirmed\",
                  \"TakerPaymentSent\",
                  \"TakerPaymentSpent\",
                  \"MakerPaymentSpent\",
                  \"Finished\"
              ],
              \"error_events\": [
                  \"StartFailed\",
                  \"NegotiateFailed\",
                  \"TakerFeeSendFailed\",
                  \"MakerPaymentValidateFailed\",
                  \"MakerPaymentWaitConfirmFailed\",
                  \"TakerPaymentTransactionFailed\",
                  \"TakerPaymentWaitConfirmFailed\",
                  \"TakerPaymentDataSendFailed\",
                  \"TakerPaymentWaitForSpendFailed\",
                  \"MakerPaymentSpendFailed\",
                  \"TakerPaymentWaitRefundStarted\",
                  \"TakerPaymentRefunded\",
                  \"TakerPaymentRefundFailed\"
              ]
          }
     },
     \"id\": 0
}
"
```

<div style="margin-top: 0.5rem;">



##### Response (success)

```json
{
    "mmrpc": "2.0",
    "result": {
        "swap": {
            "type": "Maker",
            "uuid": "f87fa9ce-0820-4675-b85d-db18c7bc9fb4",
            "my_order_uuid": "f87fa9ce-0820-4675-b85d-db18c7bc9fb4",
            "events": [
                {
                    "timestamp": 1638984440546,
                    "event": {
                        "type": "Started",
                        "data": {
                            "taker_coin": "MORTY",
                            "maker_coin": "RICK",
                            "taker": "b1e544ce2d860219bc91314b5483421a553a7b33044659eff0be9214ed58addd",
                            "secret": "0000000000000000000000000000000000000000000000000000000000000000",
                            "secret_hash": "4da9e7080175e8e10842e0e161b33cd298cab30b",
                            "my_persistent_pub": "0315d9c51c657ab1be4ae9d3ab6e76a619d3bccfe830d5363fa168424c0d044732",
                            "lock_duration": 7800,
                            "maker_amount": "0.9090909090909090909090909090909090909090909090909090909090909090909090909090909090909090909090909091",
                            "taker_amount": "1",
                            "maker_payment_confirmations": 1,
                            "maker_payment_requires_nota": false,
                            "taker_payment_confirmations": 1,
                            "taker_payment_requires_nota": false,
                            "maker_payment_lock": 1639000040,
                            "uuid": "f87fa9ce-0820-4675-b85d-db18c7bc9fb4",
                            "started_at": 1638984440,
                            "maker_coin_start_block": 1207822,
                            "taker_coin_start_block": 1222573,
                            "maker_payment_trade_fee": null,
                            "taker_payment_spend_trade_fee": null
                        }
                    }
                },
                {
                    "timestamp": 1638984456603,
                    "event": {
                        "type": "Negotiated",
                        "data": {
                            "taker_payment_locktime": 1638992240,
                            "taker_pubkey": "03b1e544ce2d860219bc91314b5483421a553a7b33044659eff0be9214ed58addd",
                            "maker_coin_swap_contract_addr": null,
                            "taker_coin_swap_contract_addr": null
                        }
                    }
                },
                {
                    "timestamp": 1638984457822,
                    "event": {
                        "type": "TakerFeeValidated",
                        "data": {
                            "tx_hex": "0400008085202f89016383e8aced2256378bb126a1ca1a41e2f344d9295f65b3ea4b99055c5eb4a6cb000000006a47304402201c7e661e0dbeb9b3eb6e4e9e3194010e5772227017772b2e48c1b8d48ed3b21f02201c2eda64e74455fa1878a5c221f25d22fe626abd0078a26a9fc0f829e0921639012103b1e544ce2d860219bc91314b5483421a553a7b33044659eff0be9214ed58adddffffffff02bcf60100000000001976a914ca1e04745e8ca0c60d8c5881531d51bec470743f88ac74c3e90b000000001976a91483762a373935ca241d557dfce89171d582b486de88ac08ebb061000000000000000000000000000000",
                            "tx_hash": "fcb49167c79e8e014143643b94878866f7e80b26c5a5dcf693010543da70b5bc"
                        }
                    }
                },
                {
                    "timestamp": 1638984457822,
                    "event": {
                        "type": "MakerPaymentSent",
                        "data": {
                            "tx_hex": "0400008085202f8901c41fdf6b9d8aea4b472f83e4fa0d99dfafc245e897d681fd2ca7df30707fbf48020000006b483045022100c7b294bd46cbf3b13530879a43c5cf67414047266d8b64c3c7263b5e75b989ba02201974f38d688b184bc44e628806c6ab2ac9092f394729d0ce838f14e1e76117c001210315d9c51c657ab1be4ae9d3ab6e76a619d3bccfe830d5363fa168424c0d044732ffffffff03a2296b050000000017a91491c45f69e1760c12a1f90fb2a811f6dfde35cc35870000000000000000166a144da9e7080175e8e10842e0e161b33cd298cab30bac503d64000000001976a9141462c3dd3f936d595c9af55978003b27c250441f88ac09ebb061000000000000000000000000000000",
                            "tx_hash": "6287e0d30951cd859bfb837eb1e5409f7596e75ffeb2e61fd6df1843bfd0203d"
                        }
                    }
                },
                {
                    "timestamp": 1638984503611,
                    "event": {
                        "type": "TakerPaymentValidateFailed",
                        "data": {
                            "error": "Origin Taker error event: MakerPaymentWaitConfirmFailed(SwapError { error: \"An error\" })"
                        }
                    }
                },
                {
                    "timestamp": 1638984503611,
                    "event": {
                        "type": "MakerPaymentWaitRefundStarted",
                        "data": {
                            "wait_until": 1639003740
                        }
                    }
                }
            ],
            "maker_amount": "0.9090909090909090909090909090909090909090909090909090909090909090909090909090909090909090909090909091",
            "maker_coin": "RICK",
            "taker_amount": "1",
            "taker_coin": "MORTY",
            "gui": "nogui",
            "mm_version": "",
            "success_events": [
                "Started",
                "Negotiated",
                "TakerFeeValidated",
                "MakerPaymentSent",
                "TakerPaymentReceived",
                "TakerPaymentWaitConfirmStarted",
                "TakerPaymentValidatedAndConfirmed",
                "TakerPaymentSpent",
                "TakerPaymentSpendConfirmStarted",
                "TakerPaymentSpendConfirmed",
                "Finished"
            ],
            "error_events": [
                "StartFailed",
                "NegotiateFailed",
                "TakerFeeValidateFailed",
                "MakerPaymentTransactionFailed",
                "MakerPaymentDataSendFailed",
                "MakerPaymentWaitConfirmFailed",
                "TakerPaymentValidateFailed",
                "TakerPaymentWaitConfirmFailed",
                "TakerPaymentSpendFailed",
                "TakerPaymentSpendConfirmFailed",
                "MakerPaymentWaitRefundStarted",
                "MakerPaymentRefunded",
                "MakerPaymentRefundFailed"
            ]
        }
    },
    "id": null
}
```



</div>


## remove\_delegation


The `remove_delegation` method stops your node's staking of a compatible coin. Currently QTUM and tQTUM (test tokens avalable at http://testnet-faucet.qtum.info/) have been integrated, but this functionality will be expanded to more coins in future.

Note: After running `remove_delegation`, you will need to broadcast the returned hex via [`send_raw_transaction`](../atomicdex-api-legacy/send_raw_transaction.html) to complete the process.

#### Arguments

| Structure | Type   | Description                     |
| --------- | ------ | ------------------------------- |
| coin      | string | the coin being staked           |


##### :pushpin: Examples

##### Command

```bash
curl --location --request POST "http://127.0.0.1:7783" \
--header "Content-Type: application/json" \
--data-raw "{
    \"userpass\": \"$userpass\",
    \"mmrpc\": \"2.0\",
    \"method\": \"remove_delegation\",
    \"params\": {
        \"coin\": \"tQTUM\"
    },
    \"id\": 0
}"
```

<div style="margin-top: 0.5rem;">



##### Response (success)

```json
{
  "mmrpc": "2.0",
  "result": {
    "tx_hex": "01000000015c7f32b1b3396ce1bed4f6c161bcc3a5bf5c58e4338c66a24c9de1deffc5b94e000000006a47304402203fcdf1e48f6e43fd718b4aab79c56a7ff81b12304339ddf6d871a3f26f217a7502200c22fa8e2bcc33d16f4bf62feb71f637acbefdd34135314e6aa526e6655cba73012102641b541e35bc915e375c8038f1099a977bc6736aa7265e9f65b7270b70d34366ffffffff020000000000000000225403a086010128043d666e8b140000000000000000000000000000000000000086c280584f00000000001976a914c36ac1020b1eae632079692e7bef350d279489c988acb8d98061",
    "tx_hash": "3564859a7ff902e8d65387c44f6049943582e0b9e29161bf1075a00097e535ae",
    "from": ["qbNeoqCbBu4hySDUzgmo666faYH3qgaeKz"],
    "to": ["qbNeoqCbBu4hySDUzgmo666faYH3qgaeKz"],
    "total_amount": "0.096",
    "spent_by_me": "0.096",
    "received_by_me": "0.052",
    "my_balance_change": "-0.044",
    "block_height": 0,
    "timestamp": 1635834296,
    "fee_details": {
      "type": "Qrc20",
      "coin": "tQTUM",
      "miner_fee": "0.004",
      "gas_limit": 100000,
      "gas_price": 40,
      "total_gas_fee": "0.04"
    },
    "coin": "tQTUM",
    "internal_id": "",
    "transaction_type": "RemoveDelegation"
  },
  "id": 0
}
```



</div>

## remove\_node\_from\_version\_stat

The `remove_node_from_version_stat` method removes a Node (by name) from the local database which tracks which version of MM2 it is running. The name parameter is an arbitrary identifying string, such as "seed_alpha" or "dragonhound_DEV".

#### Arguments

| Structure | Type   | Description                     |
| --------- | ------ | ------------------------------- |
| name      | string | the name assigned to the node   |


##### :pushpin: Examples

##### Command

```bash
curl --url "http://127.0.0.1:7783" --data "{\"mmrpc\": \"2.0\",\"method\":\"remove_node_from_version_stat\",\"userpass\":\"$userpass\",\"params\":{\"name\": \"dragonhound_DEV\"}}"
```

<div style="margin-top: 0.5rem;">



##### Response (success)

```json
{
  "mmrpc":"2.0",
  "result":"success",
  "id":null
}
```




</div>

## start\_simple\_market\_maker\_bot

The AtomicDEX API allows for simple bot trading via the `start_simple_market_maker_bot` method. This method takes as input a url to a price service, and configuration parameters of the pair to trade at a defined spread percentage value. It will update orders every 30 seconds (or higher values if defined with the `bot_refresh_rate` parameter). 

Note: If using a custom prices API endpoint, please ensure it conforms to the same schema as the url in the example.

For convenience, an online [tool for generating configs](https://stats.kmd.io/atomicdex/makerbot_config_form/) is available.

#### Arguments

| Structure                       | Type   | Description                     |
| ------------------------------- | ------- | ----------------------------------------- |
| price_url                       | string  | Link to a price service API               |
| bot_refresh_rate                | float   | Bot loop interval in seconds (optional, 30 sec default)       |
| cfg.name                        | string  | The name assigned to this configuration (e.g. the pair being configured) |
| cfg.name.base                   | string  | Ticker of the coin you wish to sell       |
| cfg.name.rel                    | string  | Ticker of the coin you wish to buy        |
| cfg.name.max                    | boolean | Set to `true` if you would like to trade your whole balance (optional)  |
| cfg.name.max_volume.percentage  | string  | Percentage of balance to trade (optional; can not use at same time as `max_volume.usd`; if greater than 1.0 `max=true` is implied)       |
| cfg.name.max_volume.usd         | string  | Maximum USD trade volume value to trade (optional; can not use at same time as `max_volume.percentage`; if greater than full balance `max=true` is implied)       |
| cfg.name.min_volume.percentage  | string  | Minimum percentage of balance to accept in trade (optional, can not use at same time as `min_volume.usd`)       |
| cfg.name.min_volume.usd         | float   | Minimum USD trade volume of trades accepted for order (optional, can not use at same time as `min_volume.percentage`)       |
| cfg.name.min_base_price         | float   | Minimum USD price of base coin to accept in trade (optional)       |
| cfg.name.min_rel_price          | float   | Minimum USD price of rel coin to accept in trade (optional)       |
| cfg.name.min_pair_price         | float   | Minimum USD price of pair (base/rel) to accept in trade (optional)       |
| cfg.name.spread**               | string  | Target price in relation to prices API value  |
| cfg.name.base_confs             | integer | number of required blockchain confirmations for base coin atomic swap transaction; default to base coin configuration if not set       |
| cfg.name.base_nota              | boolean | whether dPoW notarization is required for base coin atomic swap transaction; default to base coin configuration if not set       |
| cfg.name.rel_confs              | integer | number of required blockchain confirmations for rel coin atomic swap transaction; default to rel coin configuration if not set       |
| cfg.name.rel_nota               | boolean | whether dPoW notarization is required for rel coin atomic swap transaction; default to base coin configuration if not set       |
| cfg.name.enable                 | boolean | Bot will ignore this config entry if set to false       |
| cfg.name.price_elapsed_validity | float   | Will cancel current orders for this pair and not submit a new order if last price update time has been longer than this value in seconds (optional; defaults to 5 minutes)      |
| cfg.name.check_last_bidirectional_trade_thresh_hold | boolean | Will readjust the calculated cex price if a precedent trade exists for the pair (or reversed pair), applied via a [VWAP logic](https://www.investopedia.com/terms/v/vwap.asp#:~:text=VWAP%20is%20calculating%20the%20sum,periods%20there%20are%20(10))) (optional; defaults to false)      |

* Percentage values are within the range of 0-1, such that 0.25 = 25%
* For spread, a value of 1.05 equates to 5% over the value returned from the prices API url.
* At least one of the optional fields `max`, `max_volume.usd` or `max_volume.percentage` must be present, or orders will not be placed.

##### :pushpin: Examples

As demonstrated below, multiple configs can be included within the same command. It is recommended to not exceed 500-1000 simultaneous orders placed to avoid decreased performance.

In the example below, the first config lets the bot know we want to:
- Sell DASH in exchange for KMD
- Use whole of available DASH balance, with minimum trade volume accepted as 25% of your balance
- Sets the sell price at 2.5% over the value returned from the prices API (spread).
- Only accepts values from the prices API that have been updated within the last 30 seconds
- Waits for 3 confirmations and does not wait for a notarisation to progress to the next steps in the atomic swap process
- Checks trade history within the local AtomicDEX API database to never create trades with a sell price that is less than the average trading price.

The second config tells the bot to:
- Sell DASH in exchange for DGB
- Trade at most 50% of your DASH balance, with minimum trade volume accepted at least $20 USD.
- Only place an order when the DASH price is $250 USD or more.
- Sets the sell price at 4% over the value returned from the prices API (spread).
- Only accepts values from the prices API that have been updated within the last 60 seconds
- Waits for 1 confirmation and does not wait for a notarisation to progress to the next steps in the atomic swap process
- Ignores your trade history and average trading price, creating/updating orders regardless.

The third config tells the bot to:
- Sell DASH in exchange for LTC
- Trade at most $500 USD worth of DASH, with minimum trade volume accepted at least $50 USD.
- Only place an order when the DASH price is $250 USD or more.
- Sets the sell price at 5% over the value returned from the prices API (spread).
- Only accepts values from the prices API that have been updated within the last 60 seconds
- Waits for 1 confirmation and does not wait for a notarisation to progress to the next steps in the atomic swap process
- Ignores your trade history and average trading price, creating/updating orders regardless.


##### Command

```bash
curl --location --request POST 'http://127.0.0.1:7783' \
--header 'Content-Type: application/json' \
--data-raw "{
    \"userpass\": \"${userpass}\",
    \"mmrpc\": \"2.0\",
    \"method\": \"start_simple_market_maker_bot\",
    \"params\": {
        \"price_url\": \"https://prices.komodo.live:1313/api/v2/tickers?expire_at=600\",
        \"bot_refresh_rate\": 60,
        \"cfg\": {
            \"DASH/KMD\": {

                \"base\": \"DASH\",
                \"rel\": \"KMD\",
                \"max\": true,
                \"min_volume\": {\"percentage\": \"0.25\"},
                \"spread\": \"1.025\",
                \"base_confs\": 3,
                \"base_nota\": false,
                \"rel_confs\": 3,
                \"rel_nota\": false,
                \"enable\": true,
                \"price_elapsed_validity\": 30.0,
                \"check_last_bidirectional_trade_thresh_hold\": true
            },
             \"DASH/DGB\": {
                \"base\": \"DASH\",
                \"rel\": \"DGB\",
                \"min_volume\": {\"usd\": \"20\"},
                \"min_base_price\": \"250\",
                \"spread\": \"1.04\",
                \"base_confs\": 1,
                \"base_nota\": false,
                \"rel_confs\": 1,
                \"rel_nota\": false,
                \"enable\": true,
                \"price_elapsed_validity\": 60.0,
                \"check_last_bidirectional_trade_thresh_hold\": false
            }
        },
             \"DASH/LTC\": {
                \"base\": \"DASH\",
                \"rel\": \"LTC\",
                \"max_volume\": {\"usd\": \"500\"},
                \"min_volume\": {\"usd\": \"50\"},
                \"min_base_price\": \"250\",
                \"spread\": \"1.04\",
                \"base_confs\": 1,
                \"base_nota\": false,
                \"rel_confs\": 1,
                \"rel_nota\": false,
                \"enable\": true,
                \"price_elapsed_validity\": 60.0,
                \"check_last_bidirectional_trade_thresh_hold\": false
            }
        }
    },
    \"id\": 0
}"

```

As we have `\"bot_refresh_rate\": 60,` in the above command, our bot loop will update order prices every 60 seconds, as long as the price service returns data that is no more than 30 seconds old (for DASH/KMD) or no more than 60 seconds old (for DASH/DGB).



<div style="margin-top: 0.5rem;">



##### Response (success)

```json
{
    "mmrpc":"2.0",
    "result":{
        "result":"Success"
    },
    "id":0
}

```

##### Response (error - bot already started)

```json
{
    "mmrpc":"2.0",
    "error":"The bot is already started",
    "error_path":"simple_market_maker",
    "error_trace":"simple_market_maker:770]",
    "error_type":"AlreadyStarted",
    "id":0
}
```



</div>


## start\_version\_stat\_collection


The `start_version_stat_collection` method initiates storing version statistics for nodes previously registered via the `add_node_to_version_stat` method.

#### Arguments

| Structure | Type    | Description                                      |
| --------- | ------- | ------------------------------------------------ |
| interval  | integer | polling rate (in seconds) to check node versions |


##### :pushpin: Examples

##### Command

```bash
curl --url "http://127.0.0.1:7783" --data "{\"mmrpc\": \"2.0\",\"method\":\"start_version_stat_collection\",\"userpass\":\"$userpass\",\"params\":{\"interval\": 600}}"
```

<div style="margin-top: 0.5rem;">



##### Response (success)

```json
{
  "mmrpc":"2.0",
  "result":"success",
  "id":null
}
```

##### Response (error - invalid peer id unable to parse)

```json
{
  "mmrpc":"2.0",
  "error":"Database error: UNIQUE constraint failed: nodes.peer_id",
  "error_path":"lp_stats",
  "error_trace":"lp_stats:124]",
  "error_type":"DatabaseError",
  "error_data":"UNIQUE constraint failed: nodes.peer_id",
  "id":null
}
```




</div>


## stop\_simple\_market\_maker\_bot

The `stop_simple_market_maker_bot` method tells the bot to finish placing orders at the end of the current loop (30 seconds minimum & 30 seconds by default). This method takes as input a url to a price service, and configuration parameters of the pairs to trade at a defined spread percentage value. 

At the end of the final loop, orders placed by the bot will be cancelled. Users should wait until the loop ends before exiting the AtomicDEX API, otherwise orders will not cancel, and will reappear on the orderbook next time AtomicDEX API starts. 

#### Arguments

| Structure                       | Type    | Description                               |
| ------------------------------- | ------- | ----------------------------------------- |
| (none   )                       |         |                                           |

##### :pushpin: Examples


##### Command

```bash
curl --location --request POST 'http://127.0.0.1:7783' \
--header 'Content-Type: application/json' \
--data-raw "{
    \"userpass\": \"${userpass}\",
    \"mmrpc\": \"2.0\",
    \"method\": \"stop_simple_market_maker_bot\",
    \"params\": {},
    \"id\": 0
}"
```

<div style="margin-top: 0.5rem;">



##### Response (success)

```json
{
    "mmrpc":"2.0",
    "result":{
        "result":"Success"
    },
    "id":0
}

```

##### Response (error - bot already stopped)

```json
{
    "mmrpc":"2.0",
    "error":"The bot is already stopped",
    "error_path":"simple_market_maker",
    "error_trace":"simple_market_maker:813]",
    "error_type":"AlreadyStopped",
    "id":0
}
```



</div>

## stop\_version\_stat\_collection

The `stop_version_stat_collection` method stops the collection of version stats at the end of the current loop interval.

##### Arguments

| Structure | Type | Description |
| --------- | ---- | ----------- |
| (none)    |      |             |

##### Response

| Structure | Type   | Description      |
| --------- | ------ | ---------------- |
| result    | string | success or error |

##### :pushpin: Examples

##### Command

```bash
curl --url "http://127.0.0.1:7783" --data "{\"mmrpc\": \"2.0\",\"method\":\"stop_version_stat_collection\",\"userpass\":\"$userpass\",\"params\":{}}
```

<div style="margin-top: 0.5rem;">



##### Response (success)

```json
{
  "mmrpc":"2.0",
  "result":"success",
  "id":null
}
```



</div>

<div style="margin-top: 0.5rem;">



##### Response (error - stats collection not running)

```json
{
  "mmrpc":"2.0",
  "error":"start_version_stat_collection is not running",
  "error_path":"lp_stats",
  "error_trace":"lp_stats:395]",
  "error_type":"NotRunning",
  "id":null
}

```



</div>


## Telegram Alerts for Market Maker Bot

The AtomicDEX API Market Maker bot can be configured to send status update alerts via telegram!

To set this up, you can add some additional parameters to your MM2.json as shown in the example below

```json
{
  "gui": "MarketMakerBot",
  "netid": 7777,
  "rpc_password": "YOUR_PASSWORD",
  "passphrase": "YOUR SEED PHRASE",
  "userhome": "/${HOME#\"/\"}",
  "dbdir": "/path/to/your/atomicdex/DB",
  "message_service_cfg": {
    "telegram": {
      "api_key": "YOUR:TELEGRAM_API_TOKEN",
      "chat_registry": {
        "default": "YOUR_TELEGRAM_CHAT_ID",
        "maker_bot": "YOUR_TELEGRAM_CHAT_ID",
        "swap_events": "YOUR_TELEGRAM_CHAT_ID"
      }
    }
  }
}
```

The extra fields required are:

| Structure                       | Type    | Description                               |
| ------------------------------- | ------- | ----------------------------------------- |
| api_key                         | string  | A Telegram bot API token                  |
| chat_registry.default           | string  | A Telegram Chat ID                        |
| chat_registry.maker_bot         | string  | A Telegram Chat ID                        |
| chat_registry.swap_events       | string  | A Telegram Chat ID                        |


You can use the same Telegram chat ID for all three `chat_registry` subfields, or sent your alerts to a different chat ID if you want to separate the alerts by type.

To get a Telegram bot API token, you need to [have chat with the BotFather](https://sean-bradley.medium.com/get-telegram-chat-id-80b575520659)

To get a Telegram chat ID, check out [this guide](https://sean-bradley.medium.com/get-telegram-chat-id-80b575520659)


## trade\_preimage

The `trade_preimage` method returns the approximate fee amounts that are paid per the whole swap.
Depending on the parameters, the function returns different results:

- If the `swap_method` is `buy` or `sell`, then the result will include the `taker_fee` and the `fee_to_send_taker_fee`.
  The `taker_fee` amount is paid from the `base` coin balance if the `swap_method` is `sell`, else it is paid from the `rel` coin balance;
- If the `max` field is true, then the result will include the `volume`.

::: tip Note

This method can be used instead of **max_taker_vol**, if the `max` field is true and the `swap_method` is `buy` or `sell`.
Use the resulting `volume` as an argument of the `buy` or `sell` requests.

:::

::: warning Important

Use the `trade_preimage` request with `max = true` and `swap_method = "setprice"` arguments to approximate the fee amounts **only**. Do not use the resulting `volume` as an argument of the `setprice`.

:::

#### Arguments

| Structure   | Type                                  | Description                                                                                                                          |
| ----------- | ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| base        | string                                | the base currency of the request                                                                                                     |
| rel         | string                                | the rel currency of the request                                                                                                      |
| swap_method | string                                | the name of the method whose preimage is requested. Possible values: `buy`, `sell`, `setprice`                                       |
| price       | numeric string or rational            | the price in `rel` the user is willing to pay per one unit of the `base` coin                                                        |
| volume      | numeric string or rational (optional) | the amount the user is willing to trade; ignored if `max = true` **and** `swap_method = setprice`, otherwise, it must be set         |
| max         | bool (optional)                       | whether to return the maximum available volume for `setprice` method; must not be set or `false` if `swap_method` is `buy` or `sell` |

#### Result

| Structure                                    | Type                                 | Description                                                                                                                                    |
| -------------------------------------------- | ------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| result                                       | object                               | an object containing the relevant information                                                                                                  |
| result.base_coin_fee                         | object (`ExtendedFeeInfo`)           | the approximate miner fee is paid per the whole swap concerning the `base` coin                                                                |
| result.rel_coin_fee                          | object (`ExtendedFeeInfo`)           | the approximate miner fee is paid per the whole swap concerning the `rel` coin                                                                 |
| result.volume                                | string (numeric, optional)           | the max available volume that can be traded (in decimal representation); empty if the `max` argument is missing or false                       |
| result.volume_rat                            | rational (optional)                  | the max available volume that can be traded (in rational representation); empty if the `max` argument is missing or false                      |
| result.volume_fraction                       | fraction (optional)                  | the max available volume that can be traded (in fraction representation); empty if the `max` argument is missing or false                      |
| result.taker_fee                             | object (optional, `ExtendedFeeInfo`) | the dex fee to be paid by Taker; empty if `swap_method` is `setprice`                                                                          |
| result.fee_to_send_taker_fee                 | object (optional, `ExtendedFeeInfo`) | the approximate miner fee is paid to send the dex fee; empty if `swap_method` is `setprice`                                                    |
| result.total_fees                            | array of `ExtendedFeeInfo` objects   | each element is a sum of fees required to be paid from user's balance of corresponding `ExtendedFeeInfo.coin`; the elements are unique by coin |

Where the `ExtendedFeeInfo` has

| Structure             | Type             | Description                                                                                                                                                   |
| --------------------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| coin                  | string           | the fee is paid from the user's balance of this coin. This coin name may differ from the `base` or `rel` coins. For example, ERC20 fees are paid by ETH (gas) |
| amount                | string (numeric) | fee amount (in decimal representation)                                                                                                                        |
| amount_rat            | rational         | fee amount (in rational representation)                                                                                                                       |
| amount_fraction       | fraction         | fee amount (in fraction representation)                                                                                                                       |
| paid_from_trading_vol | boolean          |  If `true`, fees are deducted from the payment amount for the spend/refund UTXO HTLC transaction. If `false`, fees are not deducted from the traded volume. This is where an additional miner fee is needed to broadcast a swap transaction and/or where gas paid (e.g in ETH for an ERC20 trade) - in this case, user requires a sufficient current ETH balance to cover the fees before they can initiate the swap.                                             |

#### :warning: Error types

##### NotSufficientBalance

The `available` balance of the `coin` is not sufficient to start the swap.

| Structure       | Type                       | Description                                                                                |
| --------------- | -------------------------- | ------------------------------------------------------------------------------------------ |
| coin            | string                     | the name of the coin which balance is not sufficient                                       |
| available       | string (numeric)           | the balance available for swap, including the amount locked by other swaps                 |
| required        | string (numeric)           | the amount required to start the swap. This amount is necessary but may not be sufficient  |
| locked_by_swaps | string (numeric, optional) | the amount locked by other swaps                                                        |

##### NotSufficientBaseCoinBalance

The available balance of the base `coin` is not sufficient to pay transaction fees.

For example, ERC20 fees are paid by ETH (gas), and this error type is returned if the ETH coin balance is not sufficient to start the swap.

| Structure       | Type                       | Description                                                                                |
| --------------- | -------------------------- | ------------------------------------------------------------------------------------------ |
| coin            | string                     | the name of the base coin which balance is not sufficient                                  |
| available       | string (numeric)           | the balance available for swap, including the amount locked by other swaps                 |
| required        | string (numeric)           | the amount required to start the swap. This amount is necessary but may not be sufficient |
| locked_by_swaps | string (numeric, optional) | the amount is locked by other swaps                                                        |

##### VolumeTooLow

The specified `volume` is too low. Required at least `threshold`.

::: tip Note

If the `coin` field returned in Response is the same as the `rel` argument in Request, then the base volume threshold can be calculated as follows:
`base_coin_threshold = rel_vol_threshold / price`

:::

| Structure | Type             | Description                                        |
| --------- | ---------------- | -------------------------------------------------- |
| coin      | string           | either `base` or `rel` coin specified in Request   |
| volume    | string (numeric) | the amount the user was willing to trade in `coin` |
| threshold | string (numeric) | the `volume` has not to be less than this amount   |

##### NoSuchCoin

The specified coin was not found or is not activated yet.

| Structure | Type   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| coin      | string | either `base` or `rel` coin specified in Request |

##### CoinIsWalletOnly

The specified coin is wallet only and cannot be participated in the swap.

| Structure | Type   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| coin      | string | either `base` or `rel` coin specified in Request |

##### BaseEqualRel

The coin is wallet only and cannot be participated in the swap.

| Structure | Type   | Description |
| --------- | ------ | ----------- |
| (none)    |        |             |

##### InvalidParam

Incorrect use of the `param` parameter in Request.

| Structure | Type   | Description                                      |
| --------- | ------ | ------------------------------------------------ |
| param     | string | the name of the parameter in Request             |
| reason    | string | the reason why the parameter is used incorrectly |

##### PriceTooLow

The specified `price` is too low.

| Structure | Type             | Description                                                                        |
| --------- | ---------------- | ---------------------------------------------------------------------------------- |
| price     | string (numeric) | the price in `rel` the user was willing to receive per one unit of the `base` coin |
| threshold | string (numeric) | the `price` has not to be less than this amount                                    |

##### Transport

The request was failed due to a network error.

| Structure | Type   | Description                     |
| --------- | ------ | ------------------------------- |
| (none)    | string | the transport error description |

##### InternalError

The request was failed due to a AtomicDEX API internal error.

| Structure | Type   | Description                    |
| --------- | ------ | ------------------------------ |
| (none)    | string | the internal error description |

#### :pushpin: Examples

##### Command (setprice)

```bash
curl --url "http://127.0.0.1:7783" --data "{\"mmrpc\":\"2.0\",\"userpass\":\"$userpass\",\"method\":\"trade_preimage\",\"params\":{\"base\":\"BTC\",\"rel\":\"RICK\",\"price\":\"1\",\"volume\":\"0.1\",\"swap_method\":\"setprice\"},\"id\":0}"
```

##### Response

```json
{
  "mmrpc": "2.0",
  "result": {
    "base_coin_fee": {
      "coin": "KMD",
      "amount": "0.00001",
      "amount_fraction": {
        "numer": "1",
        "denom": "100000"
      },
      "amount_rat": [ [ 1, [ 1 ] ], [ 1, [ 100000 ] ] ],
      "paid_from_trading_vol": false
    },
    "rel_coin_fee": {
      "coin": "DGB",
      "amount": "0.00030782",
      "amount_fraction": {
        "numer": "15391",
        "denom": "50000000"
      },
      "amount_rat": [ [ 1, [ 15391 ] ], [ 1, [ 50000000 ] ] ],
      "paid_from_trading_vol": true
    },
    "volume": "1138.46868712",
    "volume_fraction": {
      "numer": "14230858589",
      "denom": "12500000"
    },
    "volume_rat": [ [ 1, [ 1345956701, 3 ] ], [ 1, [ 12500000 ] ] ],
    "total_fees": [
      {
        "coin": "KMD",
        "amount": "0.00001",
        "amount_fraction": {
          "numer": "1",
          "denom": "100000"
        },
        "amount_rat": [ [ 1, [ 1 ] ], [ 1, [ 100000 ] ] ],
        "required_balance": "0.00001",
        "required_balance_fraction": {
          "numer": "1",
          "denom": "100000"
        },
        "required_balance_rat": [ [ 1, [ 1 ] ], [ 1, [ 100000 ] ] ]
      },
      {
        "coin": "DGB",
        "amount": "0.00030782",
        "amount_fraction": {
          "numer": "15391",
          "denom": "50000000"
        },
        "amount_rat": [ [ 1, [ 15391 ] ], [ 1, [ 50000000 ] ] ],
        "required_balance": "0",
        "required_balance_fraction": {
          "numer": "0",
          "denom": "1"
        },
        "required_balance_rat": [ [ 0, [] ], [ 1, [ 1 ] ] ]
      }
    ]
  },
  "id": 0
}

```

##### Command (buy)

```bash
curl --url "http://127.0.0.1:7783" --data "{\"mmrpc\":\"2.0\",\"userpass\":\"$userpass\",\"method\":\"trade_preimage\",\"params\":{\"base\":\"BTC\",\"rel\":\"RICK\",\"price\":\"1\",\"volume\":\"0.1\",\"swap_method\":\"buy\"},\"id\":0}"
```

##### Response

```json
{
  "mmrpc": "2.0",
  "result": {
    "base_coin_fee": {
      "amount": "0.00042049",
      "amount_fraction": {
        "denom": "100000000",
        "numer": "42049"
      },
      "amount_rat": [ [ 1, [ 42049 ] ], [ 1, [ 100000000 ] ] ],
      "coin": "BTC",
      "paid_from_trading_vol": true
    },
    "rel_coin_fee": {
      "amount": "0.0001",
      "amount_fraction": {
        "denom": "10000",
        "numer": "1"
      },
      "amount_rat": [ [ 1, [ 1 ] ], [ 1, [ 10000 ] ] ],
      "coin": "RICK",
      "paid_from_trading_vol": false
    },
    "taker_fee": {
      "amount": "0.00012870012870012872",
      "amount_fraction": {
        "denom": "7770",
        "numer": "1"
      },
      "amount_rat": [ [ 1, [ 1 ] ], [ 1, [ 7770 ] ] ],
      "coin": "RICK",
      "paid_from_trading_vol": false
    },
    "fee_to_send_taker_fee": {
      "amount": "0.0001",
      "amount_fraction": {
        "denom": "10000",
        "numer": "1"
      },
      "amount_rat": [ [ 1, [ 1 ] ], [ 1, [ 10000 ] ] ],
      "coin": "RICK",
      "paid_from_trading_vol": false
    },
    "total_fees": [
      {
        "coin": "RICK",
        "amount": "0.001307001287001287001287001287001287001287001287001287001287001287001287001287001287001287001287001287",
        "amount_fraction": {
          "numer": "50777",
          "denom": "38850000"
        },
        "amount_rat": [ [ 1, [ 50777 ] ], [ 1, [ 38850000 ] ] ],
        "required_balance": "0.001307001287001287001287001287001287001287001287001287001287001287001287001287001287001287001287001287",
        "required_balance_fraction": {
          "numer": "50777",
          "denom": "38850000"
        },
        "required_balance_rat": [ [ 1, [ 50777 ] ], [ 1, [ 38850000 ] ] ]
      },
      {
        "coin": "tBTC",
        "amount": "0.00042049",
        "amount_fraction": {
          "denom": "100000000",
          "numer": "42049"
        },
        "amount_rat": [ [ 1, [ 42049 ] ], [ 1, [ 100000000 ] ] ],
        "required_balance": "0",
        "required_balance_fraction": {
          "numer": "0",
          "denom": "1"
        },
        "required_balance_rat": [ [ 0, [] ], [ 1, [ 1 ] ] ]
      }
    ]
  },
  "id": 0
}
```

##### Command (sell, max)

```bash
curl --url "http://127.0.0.1:7783" --data "{\"mmrpc\":\"2.0\",\"userpass\":\"$userpass\",\"method\":\"trade_preimage\",\"params\":{\"base\":\"BTC\",\"rel\":\"RICK\",\"price\":\"1\",\"volume\":\"2.21363478\",\"swap_method\":\"sell\"},\"id\":0}"
```

##### Response

```json
{
  "mmrpc": "2.0",
  "result": {
    "base_coin_fee": {
      "amount": "0.00042049",
      "amount_fraction": {
        "denom": "100000000",
        "numer": "42049"
      },
      "amount_rat": [ [ 1, [ 42049 ] ], [ 1, [ 100000000 ] ] ],
      "coin": "BTC",
      "paid_from_trading_vol": false
    },
    "rel_coin_fee": {
      "coin": "RICK",
      "amount": "0.00001",
      "amount_fraction": {
        "numer": "1",
        "denom": "100000"
      },
      "amount_rat": [ [ 1, [ 1 ] ], [ 1, [ 100000 ] ] ],
      "paid_from_trading_vol": true
    },
    "taker_fee": {
      "amount": "0.0028489508108108107",
      "amount_fraction": {
        "denom": "1850000000",
        "numer": "5270559"
      },
      "amount_rat": [ [ 1, [ 5270559 ] ], [ 1, [ 1850000000 ] ] ],
      "coin": "BTC",
      "paid_from_trading_vol": false
    },
    "fee_to_send_taker_fee": {
      "amount": "0.00033219",
      "amount_fraction": {
        "denom": "100000000",
        "numer": "33219"
      },
      "amount_rat": [ [ 1, [ 33219 ] ], [ 1, [ 100000000 ] ] ],
      "coin": "BTC",
      "paid_from_trading_vol": false
    },
    "total_fees": [
      {
        "coin": "RICK",
        "amount": "0.00001",
        "amount_fraction": {
          "numer": "1",
          "denom": "100000"
        },
        "amount_rat": [ [ 1, [ 1 ] ], [ 1, [ 100000 ] ] ],
        "required_balance": "0",
        "required_balance_fraction": {
          "numer": "0",
          "denom": "1"
        },
        "required_balance_rat": [ [ 0, [] ], [ 1, [ 1 ] ] ]
      },
      {
        "coin": "BTC",
        "amount": "0.0036016308108108106",
        "amount_fraction": {
          "denom": "1850000000",
          "numer": "6663017"
        },
        "amount_rat": [ [ 1, [ 6663017 ] ], [ 1, [ 1850000000 ] ] ],
        "required_balance": "0.0036016308108108106",
        "required_balance_fraction": {
          "denom": "1850000000",
          "numer": "6663017"
        },
        "required_balance_rat": [ [ 1, [ 6663017 ] ], [ 1, [ 1850000000 ] ] ]
      }
    ]
  },
  "id": 0
}
```

##### Command (ERC20 and QRC20)

```bash
curl --url "http://127.0.0.1:7783" --data "{\"mmrpc\":\"2.0\",\"userpass\":\"$userpass\",\"method\":\"trade_preimage\",\"params\":{\"base\":\"BAT\",\"rel\":\"QC\",\"price\":\"1\",\"volume\":\"2.21363478\",\"swap_method\":\"setprice\"},\"id\":0}"
```

##### Response

```json
{
  "mmrpc": "2.0",
  "result": {
    "base_coin_fee": {
      "amount": "0.0045",
      "amount_fraction": {
        "denom": "2000",
        "numer": "9"
      },
      "amount_rat": [ [ 1, [ 9 ] ], [ 1, [ 2000 ] ] ],
      "coin": "ETH",
      "paid_from_trading_vol": false
    },
    "rel_coin_fee": {
      "amount": "0.00325",
      "amount_fraction": {
        "denom": "4000",
        "numer": "13"
      },
      "amount_rat": [ [ 0, [ 13 ] ], [ 1, [ 4000 ] ] ],
      "coin": "QTUM",
      "paid_from_trading_vol": false
    },
    "total_fees": [
      {
        "amount": "0.003",
        "amount_fraction": {
          "denom": "1000",
          "numer": "3"
        },
        "amount_rat": [ [ 1, [ 3 ] ], [ 1, [ 1000 ] ] ],
        "required_balance": "0.003",
        "required_balance_fraction": {
          "denom": "1000",
          "numer": "3"
        },
        "required_balance_rat": [ [ 1, [ 3 ] ], [ 1, [ 1000 ] ] ],
        "coin": "ETH"
      },
      {
        "amount": "0.00325",
        "amount_fraction": {
          "denom": "4000",
          "numer": "13"
        },
        "amount_rat": [ [ 0, [ 13 ] ], [ 1, [ 4000 ] ] ],
        "required_balance": "0.00325",
        "required_balance_fraction": {
          "denom": "4000",
          "numer": "13"
        },
        "required_balance_rat": [ [ 0, [ 13 ] ], [ 1, [ 4000 ] ] ],
        "coin": "QTUM"
      }
    ]
  },
  "id": 0
}
```

##### Response (NotSufficientBalance error)

```json
{
  "mmrpc": "2.0",
  "error": "Not enough BTC for swap: available 0.000015, required at least 0.10012, locked by swaps None",
  "error_path": "maker_swap",
  "error_trace": "maker_swap:1540] maker_swap:1641]",
  "error_type": "NotSufficientBalance",
  "error_data": {
    "coin":"BTC",
    "available":"0.000015",
    "required":"0.10012",
    "locked_by_swaps":"0"
  },
  "id": 0
}
```

##### Response (VolumeTooLow error)

```json
{
  "mmrpc": "2.0",
  "error":"The volume 0.00001 of the RICK coin less than minimum transaction amount 0.0001",
  "error_path": "maker_swap",
  "error_trace": "maker_swap:1599]",
  "error_type": "VolumeTooLow",
  "error_data": {
    "coin": "RICK",
    "volume": "0.00001",
    "threshold": "0.0001"
  },
  "id": 0
}
```

##### Response (Transport error)

```json
{
  "mmrpc": "2.0",
  "error": "Transport error: JsonRpcError { client_info: 'coin: tBTC', request: JsonRpcRequest { jsonrpc: '2.0', id: '31', method: 'blockchain.estimatefee', params: [Number(1), String('ECONOMICAL')] }, error: Transport('rpc_clients:1237] rpc_clients:1239] ['rpc_clients:2047] common:1385] future timed out']') }",
  "error_path": "taker_swap.utxo_common",
  "error_trace": "taker_swap:1599] utxo_common:1990] utxo_common:166]",
  "error_type": "Transport",
  "error_data": "JsonRpcError { client_info: 'coin: tBTC', request: JsonRpcRequest { jsonrpc: '2.0', id: '31', method: 'blockchain.estimatefee', params: [Number(1), String('ECONOMICAL')] }, error: Transport('rpc_clients:1237] rpc_clients:1239] ['rpc_clients:2047] common:1385] future timed out']') }",
  "id": 0
}
```

##### Response (incorrect use of "max" error)

```json
{
  "mmrpc": "2.0",
  "error": "Incorrect use of the 'max' parameter: 'max' cannot be used with 'sell' or 'buy' method",
  "error_path": "taker_swap",
  "error_trace": "taker_swap:1602]",
  "error_type": "InvalidParam",
  "error_data": {
    "param": "max",
    "reason": "'max' cannot be used with 'sell' or 'buy' method"
  },
  "id": 0
}
```


## update\_version\_stat\_collection


The `update_version_stat_collection` method updates the polling interval for version stats collection. Note: the new interval will take effect after the current interval loop has completed.

#### Arguments

| Structure | Type    | Description                                      |
| --------- | ------- | ------------------------------------------------ |
| interval  | integer | polling rate (in seconds) to query node versions |


##### :pushpin: Examples

##### Command

```bash
curl --url "http://127.0.0.1:7783" --data "{\"mmrpc\": \"2.0\",\"method\":\"update_version_stat_collection\",\"userpass\":\"$userpass\",\"params\":{\"interval\": 900}}"

```

<div style="margin-top: 0.5rem;">



##### Response (success)

```json
{
  "mmrpc":"2.0",
  "result":"success",
  "id":null
}
```



</div>

<div style="margin-top: 0.5rem;">



##### Response (error - stats collection not running)

```json
{
  "mmrpc":"2.0",
  "error":"start_version_stat_collection is not running",
  "error_path":"lp_stats",
  "error_trace":"lp_stats:374]",
  "error_type":"NotRunning",
  "id":null
}

```



</div>


## withdraw

The `withdraw` method generates, signs, and returns a transaction that transfers the `amount` of `coin` to the address indicated in the `to` argument.

This method generates a raw transaction which should then be broadcast using [send_raw_transaction](../../../basic-docs/atomicdex-api-legacy/send_raw_transaction.html).

#### Arguments

| Structure     | Type             | Description                                                                                                                               |
| ------------- | ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| coin          | string           | the name of the coin the user desires to withdraw                                                                                         |
| to            | string           | coins are withdrawn to this address                                                                                                       |
| amount        | string (numeric) | the amount the user desires to withdraw, ignored when `max=true`                                                                          |
| memo          | string           | Optional. Adds a transaction memo for compatible coins (e.g. Tendermint ecosystem).                                                       |
| max           | bool             | withdraw the maximum available amount                                                                                                     |
| fee.type      | string           | type of transaction fee; possible values: `UtxoFixed`, `UtxoPerKbyte`, `EthGas`                                                           |
| fee.amount    | string (numeric) | fee amount in coin units, used only when type is `UtxoFixed` (fixed amount not depending on tx size) or `UtxoPerKbyte` (amount per Kbyte) |
| fee.gas_price | string (numeric) | used only when fee type is EthGas; sets the gas price in `gwei` units                                                                     |
| fee.gas       | number (integer) | used only when fee type is EthGas; sets the gas limit for transaction                                                                     |

#### Response

| Structure                 | Type                       | Description                                                                                                                                                                                 |
| ------------------------- | -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| from                      | array of strings           | coins are withdrawn from this address; the array contains a single element, but transactions may be sent from several addresses (UTXO coins)                                                |
| to                        | array of strings           | coins are withdrawn to this address; this may contain the `my_address` address, where change from UTXO coins is sent                                                                        |
| my_balance_change         | string (numeric)           | the expected balance of change in `my_address` after the transaction broadcasts                                                                                                             |
| received_by_me            | string (numeric)           | the amount of coins received by `my_address` after the transaction broadcasts; the value may be above zero when the transaction requires that the AtomicDEX API send change to `my_address` |
| spent_by_me               | string (numeric)           | the amount of coins spent by `my_address`; this value differ from the request amount, as the transaction fee is added here                                                                  |
| total_amount              | string (numeric)           | the total amount of coins transferred                                                                                                                                                       |
| fee_details               | object                     | the fee details of the generated transaction; this value differs for utxo and ETH/ERC20 coins, check the examples for more details                                                          |
| tx_hash                   | string                     | the hash of the generated transaction                                                                                                                                                       |
| tx_hex                    | string                     | transaction bytes in hexadecimal format; use this value as input for the `send_raw_transaction` method                                                                                      |
| coin                      | string                     | the name of the coin the user wants to withdraw                                                                                                                                             |
| kmd_rewards               | object (optional)          | an object containing information about accrued rewards; always exists if the coin is `KMD`                                                                                                  |
| kmd_rewards.amount        | string (numeric, optional) | the amount of accrued rewards                                                                                                                                                               |
| kmd_rewards.claimed_by_me | bool (optional)            | whether the rewards been claimed by me                                                                                                                                                      |

#### :warning: Error types

##### NotSufficientBalance

The `available` balance is not sufficient to transfer the specified amount.

| Structure | Type             | Description                                                                                                                                            |
| --------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| coin      | string           | the name of the coin which balance is not sufficient. This coin name may differ from the requested coin. For example, ERC20 fees are paid by ETH (gas) |
| available | string (numeric) | the balance available for transfer                                                                                                                     |
| required  | string (numeric) | the amount required to transfer the specified amount. This amount is necessary but may not be sufficient                                               |

##### ZeroBalanceToWithdrawMax

The available balance is zero.

| Structure | Type | Description |
| --------- | ---- | ----------- |
| (none)    |      |             |

##### AmountTooLow

The specified amount is too low. Required at least `threshold`.

| Structure | Type             | Description                                          |
| --------- | ---------------- | ---------------------------------------------------- |
| amount    | string (numeric) | the amount the user was willing to transfer          |
| threshold | string (numeric) | the `amount` has not to be less than the `threshold` |

##### InvalidAddress

The specified `to` address is not valid.

| Structure | Type   | Description           |
| --------- | ------ | --------------------- |
| (none)    | string | the error description |

##### InvalidFeePolicy

The specified `fee` is not valid.

| Structure | Type   | Description           |
| --------- | ------ | --------------------- |
| (none)    | string | the error description |

##### NoSuchCoin

The specified coin was not found or is not activated yet.

| Structure | Type   | Description                                   |
| --------- | ------ | --------------------------------------------- |
| coin      | string | the not found `coin` specified in the Request |

##### Transport

The request was failed due to a network error.

| Structure | Type   | Description                     |
| --------- | ------ | ------------------------------- |
| (none)    | string | the transport error description |

##### InternalError

The request was failed due to an AtomicDEX API internal error.

| Structure | Type   | Description                    |
| --------- | ------ | ------------------------------ |
| (none)    | string | the internal error description |

#### :pushpin: Examples

##### Command (BTC, KMD, and other BTC-based forks)

```bash
curl --url "http://127.0.0.1:7783" --data "{\"userpass\":\"$userpass\",\"mmrpc\":\"2.0\",\"method\":\"withdraw\",\"params\":{\"coin\":\"KMD\",\"to\":\"RJTYiYeJ8eVvJ53n2YbrVmxWNNMVZjDGLh\",\"amount\":\"10\"},\"id\":0}"
```

<div style="margin-top: 0.5rem;">

##### Response (KMD success)

```json
{
  "mmrpc": "2.0",
  "result": {
    "block_height": 0,
    "coin": "KMD",
    "fee_details": {
      "type": "Utxo",
      "amount": "0.00001"
    },
    "from": ["R9o9xTocqr6CeEDGDH6mEYpwLoMz6jNjMW"],
    "my_balance_change": "-10.00001",
    "received_by_me": "0.34417325",
    "spent_by_me": "10.34418325",
    "to": ["RJTYiYeJ8eVvJ53n2YbrVmxWNNMVZjDGLh"],
    "total_amount": "10.34418325",
    "tx_hash": "3a1c382c50a7d12e4675d12ed7e723ce9f0167693dd75fd772bae8524810e605",
    "tx_hex": "0400008085202f890207a8e96978acfb8f0d002c3e4390142810dc6568b48f8cd6d8c71866ad8743c5010000006a47304402201960a7089f2d93480fff68ce0b7ca7bb7a32a52915753ac7ae780abd6162cb1d02202c9b11d442e5f72a532f44ceb10122898d486b1474a10eb981c60c5538b9c82d012102031d4256c4bc9f99ac88bf3dba21773132281f65f9bf23a59928bce08961e2f3ffffffff97f56bf3b0f815bb737b7867e71ddb8198bba3574bb75737ba9c389a4d08edc6000000006a473044022055199d80bd7e2d1b932e54f097c6a15fc4b148d21299dc50067c1da18045f0ed02201d26d85333df65e6daab40a07a0e8a671af9d9b9d92fdf7d7ef97bd868ca545a012102031d4256c4bc9f99ac88bf3dba21773132281f65f9bf23a59928bce08961e2f3ffffffff0200ca9a3b000000001976a91464ae8510aac9546d5e7704e31ce177451386455588acad2a0d02000000001976a91405aab5342166f8594baf17a7d9bef5d56744332788ac00000000000000000000000000000000000000",
    "kmd_rewards": {
      "amount": "0.0791809",
      "claimed_by_my": true
    }
  },
  "id": 0
}
```

##### Response (NotSufficientBalance error)

```json
{
  "mmrpc": "2.0",
  "error": "Not enough RICK to withdraw: available 69.75066225, required at least 1000.00001",
  "error_path": "utxo_common",
  "error_trace": "utxo_common:1379] utxo_common:449]",
  "error_type": "NotSufficientBalance",
  "error_data": {
    "coin": "RICK",
    "available": "69.75066225",
    "required": "1000.00001"
  },
  "id": 0
}
```



</div>

##### Command (BTC, KMD, and other BTC-based forks, fixed fee)

```bash
curl --url "http://127.0.0.1:7783" --data "{
  \"mmrpc\": \"2.0\",
  \"userpass\": \"$userpass\",
  \"method\": \"withdraw\",
  \"params\":{
    \"coin\": \"RICK\",
    \"to\":\"R9o9xTocqr6CeEDGDH6mEYpwLoMz6jNjMW\",
    \"amount\":\"1.0\",
    \"fee\": {
      \"type\":\"UtxoFixed\",
      \"amount\":\"0.1\"
    }
  },
  \"id\":0
}"
```

<div style="margin-top: 0.5rem;">


##### Response (success)

```json
{
  "mmrpc": "2.0",
  "result": {
    "tx_hex": "0400008085202f8901ef25b1b7417fe7693097918ff90e90bba1351fff1f3a24cb51a9b45c5636e57e010000006b483045022100b05c870fcd149513d07b156e150a22e3e47fab4bb4776b5c2c1b9fc034a80b8f022038b1bf5b6dad923e4fb1c96e2c7345765ff09984de12bbb40b999b88b628c0f9012102031d4256c4bc9f99ac88bf3dba21773132281f65f9bf23a59928bce08961e2f3ffffffff0200e1f505000000001976a91405aab5342166f8594baf17a7d9bef5d56744332788ac8cbaae5f010000001976a91405aab5342166f8594baf17a7d9bef5d56744332788ace87a5e5d000000000000000000000000000000",
    "tx_hash": "1ab3bc9308695960bc728fa427ac00d1812c4ae89aaa714c7618cb96d111be58",
    "from": ["R9o9xTocqr6CeEDGDH6mEYpwLoMz6jNjMW"],
    "to": ["R9o9xTocqr6CeEDGDH6mEYpwLoMz6jNjMW"],
    "total_amount": "60.10253836",
    "spent_by_me": "60.10253836",
    "received_by_me": "60.00253836",
    "my_balance_change": "-0.1",
    "block_height": 0,
    "timestamp": 1566472936,
    "fee_details": {
      "type": "Utxo",
      "amount": "0.1"
    },
    "coin": "RICK",
    "internal_id": ""
  },
  "id": 0
}
```

##### Response (InvalidFeePolicy error - attempt to use EthGas for UTXO coin)

```json
{
  "mmrpc": "2.0",
  "error": "Invalid fee policy: Expected 'UtxoFixed' or 'UtxoPerKbyte' fee types, found EthGas",
  "error_path": "utxo_common",
  "error_trace": "utxo_common:1371]",
  "error_type": "InvalidFeePolicy",
  "error_data": "Expected 'UtxoFixed' or 'UtxoPerKbyte' fee types, found EthGas",
  "id": 0
}
```



</div>

##### Command (BTC, KMD, and other BTC-based forks, 1 RICK per Kbyte)

```bash
curl --url "http://127.0.0.1:7783" --data "{
  \"mmrpc\": \"2.0\",
  \"userpass\": \"$userpass\",
  \"method\": \"withdraw\",
  \"params\":{
    \"coin\":\"RICK\",
    \"to\":\"R9o9xTocqr6CeEDGDH6mEYpwLoMz6jNjMW\",
    \"amount\":\"1.0\",
    \"fee\": {
      \"type\":\"UtxoPerKbyte\",
      \"amount\":\"1\"
    }
  },
  \"id\":0
}"
```

<div style="margin-top: 0.5rem;">


##### Response (success)

```json
{
  "mmrpc": "2.0",
  "result": {
    "tx_hex": "0400008085202f890258be11d196cb18764c71aa9ae84a2c81d100ac27a48f72bc6059690893bcb31a000000006b483045022100ef11280e981be280ca5d24c947842ca6a8689d992b73e3a7eb9ff21070b0442b02203e458a2bbb1f2bf8448fc47c51485015904a5271bb17e14be5afa6625d67b1e8012102031d4256c4bc9f99ac88bf3dba21773132281f65f9bf23a59928bce08961e2f3ffffffff58be11d196cb18764c71aa9ae84a2c81d100ac27a48f72bc6059690893bcb31a010000006b483045022100daaa10b09e7abf9d4f596fc5ac1f2542b8ecfab9bb9f2b02201644944ddc0280022067aa1b91ec821aa48f1d06d34cd26fb69a9f27d59d5eecdd451006940d9e83db012102031d4256c4bc9f99ac88bf3dba21773132281f65f9bf23a59928bce08961e2f3ffffffff0200e1f505000000001976a91405aab5342166f8594baf17a7d9bef5d56744332788acf31c655d010000001976a91405aab5342166f8594baf17a7d9bef5d56744332788accd7c5e5d000000000000000000000000000000",
    "tx_hash": "fd115190feec8c0c14df2696969295c59c674886344e5072d64000379101b78c",
    "from": ["R9o9xTocqr6CeEDGDH6mEYpwLoMz6jNjMW"],
    "to": ["R9o9xTocqr6CeEDGDH6mEYpwLoMz6jNjMW"],
    "total_amount": "60.00253836",
    "spent_by_me": "60.00253836",
    "received_by_me": "59.61874931",
    "my_balance_change": "-0.38378905",
    "block_height": 0,
    "timestamp": 1566473421,
    "fee_details": {
      "type": "Utxo",
      "amount": "0.38378905"
    },
    "coin": "RICK",
    "internal_id": ""
  },
  "id": 0
}
```



</div>

##### Command (ETH, ERC20, and other ETH-based forks)

```bash
curl --url "http://127.0.0.1:7783" --data "{
  \"mmrpc\": \"2.0\",
  \"userpass\": \"$userpass\",
  \"method\": \"withdraw\",
  \"params\": {
    \"coin\": \"ETH\",
    \"to\": \"0xbab36286672fbdc7b250804bf6d14be0df69fa28\",
    \"amount\": 10
  },
  \"id\": 0
}"
```

<div style="margin-top: 0.5rem;">


##### Response (success)

```json
{
  "mmrpc": "2.0",
  "result": {
    "block_height": 0,
    "coin": "ETH",
    "fee_details": {
      "type": "Eth",
      "coin": "ETH",
      "gas": 21000,
      "gas_price": "0.000000001",
      "total_fee": "0.000021"
    },
    "from": ["0xbab36286672fbdc7b250804bf6d14be0df69fa29"],
    "my_balance_change": "-10.000021",
    "received_by_me": "0",
    "spent_by_me": "10.000021",
    "to": ["0xbab36286672fbdc7b250804bf6d14be0df69fa28"],
    "total_amount": "10.000021",
    "tx_hash": "8fbc5538679e4c4b78f8b9db0faf9bf78d02410006e8823faadba8e8ae721d60",
    "tx_hex": "f86d820a59843b9aca0082520894bab36286672fbdc7b250804bf6d14be0df69fa28888ac7230489e80000801ba0fee87414a3b40d58043a1ae143f7a75d7f47a24e872b638281c448891fd69452a05b0efcaed9dee1b6d182e3215d91af317d53a627404b0efc5102cfe714c93a28"
  },
  "id": 0
}
```


</div>

##### Command (ETH, ERC20, and other ETH-based forks, with gas fee)

```bash
curl --url "http://127.0.0.1:7783" --data "{
  \"mmrpc\": \"2.0\",
  \"userpass\": \"$userpass\",
  \"method\": \"withdraw\",
  \"params\": {
    \"coin\": \"COIN_NAME\",
    \"to\": \"RECIPIENT_ADDRESS\",
    \"amount\": \"AMOUNT\",
    \"fee\": {
      \"type\": \"EthGas\",
      \"gas_price\": \"3.5\",
      \"gas\": 55000
    }
  },
  \"id\":0
}"
```

<div style="margin-top: 0.5rem;">


##### Response (success)

```json
{
  "mmrpc": "2.0",
  "result": {
    "tx_hex": "f86d820b2884d09dc30082d6d894bab36286672fbdc7b250804bf6d14be0df69fa29888ac7230489e80000801ca0ef0167b0e53ed50d87b6fd630925f2bce6ee72e9b5fdb51c6499a7caaecaed96a062e5cb954e503ff83f2d6ce082649fdcdf8a77c8d37c7d26d46d3f736b228d10",
    "tx_hash": "a26c4dcacf63c04e385dd973ca7e7ca1465a3b904a0893bcadb7e37681d38c95",
    "from": ["0xbAB36286672fbdc7B250804bf6D14Be0dF69fa29"],
    "to": ["0xbAB36286672fbdc7B250804bf6D14Be0dF69fa29"],
    "total_amount": "10",
    "spent_by_me": "10.0001925",
    "received_by_me": "10",
    "my_balance_change": "-0.0001925",
    "block_height": 0,
    "timestamp": 1566474670,
    "fee_details": {
      "type": "Eth",
      "coin": "ETH",
      "gas": 55000,
      "gas_price": "0.0000000035",
      "total_fee": "0.0001925"
    },
    "coin": "ETH",
    "internal_id": ""
  },
  "id": 0
}
```

##### Response (InvalidFeePolicy error - attempt to use UtxoFixed or UtxoPerKbyte for ETH coin)

```json
{
  "mmrpc": "2.0",
  "error": "Invalid fee policy: Expected 'EthGas' fee type, found UtxoFixed",
  "error_path": "eth",
  "error_trace": "eth:535]",
  "error_type": "InvalidFeePolicy",
  "error_data": "Expected 'EthGas' fee type, found UtxoFixed",
  "id": 0
}
```


</div>

##### Command (max = true)

```bash
curl --url "http://127.0.0.1:7783" --data "{
  \"mmrpc\": \"2.0\",
  \"userpass\": \"$userpass\",
  \"method\": \"withdraw\",
  \"params\": {
    \"coin\": \"ETH\",
    \"to\": \"0xbab36286672fbdc7b250804bf6d14be0df69fa28\",
    \"max\": true
  },
  \"id\": 0
}"
```

<div style="margin-top: 0.5rem;">


###### Response (success)

```json
{
  "mmrpc": "2.0",
  "result": {
    "block_height": 0,
    "coin": "ETH",
    "fee_details": {
      "type": "Eth",
      "coin": "ETH",
      "gas": 21000,
      "gas_price": "0.000000001",
      "total_fee": "0.000021"
    },
    "from": ["0xbab36286672fbdc7b250804bf6d14be0df69fa29"],
    "my_balance_change": "-10.000021",
    "received_by_me": "0",
    "spent_by_me": "10.000021",
    "to": ["0xbab36286672fbdc7b250804bf6d14be0df69fa28"],
    "total_amount": "10.000021",
    "tx_hash": "8fbc5538679e4c4b78f8b9db0faf9bf78d02410006e8823faadba8e8ae721d60",
    "tx_hex": "f86d820a59843b9aca0082520894bab36286672fbdc7b250804bf6d14be0df69fa28888ac7230489e80000801ba0fee87414a3b40d58043a1ae143f7a75d7f47a24e872b638281c448891fd69452a05b0efcaed9dee1b6d182e3215d91af317d53a627404b0efc5102cfe714c93a28"
  },
  "id": 0
}
```


</div>

###### Command (QRC20)

```bash
curl --url "http://127.0.0.1:7783" --data "{
  \"mmrpc\": \"2.0\",
  \"userpass\": \"$userpass\",
  \"method\": \"withdraw\",
  \"params\": {
    \"coin\":\"QRC20\",
    \"to\":\"qHmJ3KA6ZAjR9wGjpFASn4gtUSeFAqdZgs\",
    \"amount\":10
  },
  \"id\":0
}"
```

<div style="margin-top: 0.5rem;">


###### Response (success)

```json
{
  "mmrpc": "2.0",
  "result": {
    "block_height": 0,
    "coin": "QRC20",
    "timestamp": 1608725061,
    "fee_details": {
      "type": "Qrc20",
      "coin": "tQTUM",
      "miner_fee": "0.00000447",
      "gas_limit": 100000,
      "gas_price": 40,
      "total_gas_fee": "0.04"
    },
    "from": ["qXxsj5RtciAby9T7m98AgAATL4zTi4UwDG"],
    "my_balance_change": "-10",
    "received_by_me": "0",
    "spent_by_me": "10",
    "to": ["qHmJ3KA6ZAjR9wGjpFASn4gtUSeFAqdZgs"],
    "total_amount": "10",
    "tx_hash": "8fbc5538679e4c4b78f8b9db0faf9bf78d02410006e8823faadba8e8ae721d60",
    "tx_hex": "f86d820a59843b9aca0082520894bab36286672fbdc7b250804bf6d14be0df69fa28888ac7230489e80000801ba0fee87414a3b40d58043a1ae143f7a75d7f47a24e872b638281c448891fd69452a05b0efcaed9dee1b6d182e3215d91af317d53a627404b0efc5102cfe714c93a28"
  },
  "id": 0
}
```


</div>

###### Command (QRC20, with gas fee)

```bash
curl --url "http://127.0.0.1:7783" --data "{
  \"mmrpc\": \"2.0\",
  \"userpass\": \"$userpass\",
  \"method\": \"withdraw\",
  \"params\": {
    \"coin\": \"QRC20\",
    \"to\": \"qHmJ3KA6ZAjR9wGjpFASn4gtUSeFAqdZgs\",
    \"amount\": 10,
    \"fee\": {
      \"type\": \"Qrc20Gas\",
      \"gas_limit\": 250000,
      \"gas_price\": 40
    }
  },
  \"id\":0
}"
```

<div style="margin-top: 0.5rem;">


```json
{
  "mmrpc": "2.0",
  "result": {
    "block_height": 0,
    "coin": "QRC20",
    "timestamp": 1608725061,
    "fee_details": {
      "type": "Qrc20",
      "coin": "tQTUM",
      "miner_fee": "0.00000447",
      "gas_limit": 250000,
      "gas_price": 40,
      "total_gas_fee": "0.1"
    },
    "from": ["qXxsj5RtciAby9T7m98AgAATL4zTi4UwDG"],
    "my_balance_change": "-10",
    "received_by_me": "0",
    "spent_by_me": "10",
    "to": ["qHmJ3KA6ZAjR9wGjpFASn4gtUSeFAqdZgs"],
    "total_amount": "10",
    "tx_hash": "8fbc5538679e4c4b78f8b9db0faf9bf78d02410006e8823faadba8e8ae721d60",
    "tx_hex": "f86d820a59843b9aca0082520894bab36286672fbdc7b250804bf6d14be0df69fa28888ac7230489e80000801ba0fee87414a3b40d58043a1ae143f7a75d7f47a24e872b638281c448891fd69452a05b0efcaed9dee1b6d182e3215d91af317d53a627404b0efc5102cfe714c93a28"
  },
  "id": 0
}
```


</div>

###### Command (COSMOS, with memo)

```bash
curl --url "http://127.0.0.1:7783" --data "{
  \"mmrpc\": \"2.0\",
  \"userpass\": \"$userpass\",
  \"method\": \"withdraw\",
  \"params\": {
    \"coin\": \"IRIS\",
    \"to\": \"iaa16drqvl3u8sukfsu4lm3qsk28jr3fahja9vsv6k\",
    \"amount\": 13,
    \"memo\": \"It was a bright cold day in April, and the clocks were striking thirteen.\"
  },
  \"id\":0
}"
```

<div style="margin-top: 0.5rem;">


```json
{
  "mmrpc": "2.0",
  "result": {
    "tx_hex": "0ade010a8b010a1c2f636f736d6f732e62616e6b2e763162657461312e4d736753656e64126b0a2a6961613136647271766c33753873756b667375346c6d3371736b32386a72336661686a6139767376366b122a6961613136647271766c33753873756b667375346c6d3371736b32386a72336661686a6139767376366b1a110a05756972697312083133303030303030124949742077617320612062726967687420636f6c642064617920696e20417072696c2c20616e642074686520636c6f636b73207765726520737472696b696e6720746869727465656e2e188f85b50812680a500a460a1f2f636f736d6f732e63727970746f2e736563703235366b312e5075624b657912230a2103d8064eece4fa5c0f8dc0267f68cee9bdd527f9e88f3594a323428718c391ecc212040a020801181d12140a0e0a0575697269731205333835353310a08d061a40a9ac8c4112d7d7252062e289d222a438258a7c49c6657fdcbf831d62fc5eb2d05af46d6b86881335b3bc7ca98b2bfc3ef02ec5adf6768de9a778b282f9cc868e",
    "tx_hash": "E00982A2A8442D7140916A34E29E287A0B1CBB4B38940372D1966BA7ACDE5BD6",
    "from": ["iaa16drqvl3u8sukfsu4lm3qsk28jr3fahja9vsv6k"],
    "to": ["iaa16drqvl3u8sukfsu4lm3qsk28jr3fahja9vsv6k"],
    "total_amount": "13.038553",
    "spent_by_me": "13.038553",
    "received_by_me": "13",
    "my_balance_change": "-0.038553",
    "block_height": 0,
    "timestamp": 0,
    "fee_details": {
      "type": "Tendermint",
      "coin": "IRIS",
      "amount": "0.038553",
      "gas_limit": 100000
    },
    "coin": "IRIS",
    "internal_id": "e00982a2a8442d7140916a34e29e287a0b1cbb4b38940372d1966ba7acde5bd6",
    "transaction_type": "StandardTransfer"
  },
  "id": 0
}
```

You can see the memo is included on the block explorer: https://irishub.iobscan.io/#/txs/E00982A2A8442D7140916A34E29E287A0B1CBB4B38940372D1966BA7ACDE5BD6


</div>


