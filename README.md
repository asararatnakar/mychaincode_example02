# chaincode_example02

modified __chaincode_example02__ to insert new records on each invoke, instead of updating a single record.
These changes are made for messuring Performance and latencies etc., using tools (JMeter and SoapUI) where we can't pass dynamic arguments.


Here are the sample JSON Requests for Deploy, Invoke and Query

##registrar
POST: http://127.0.0.1:5000/registrar
```
{
  "enrollId": "test_user0",
  "enrollSecret": "MS9qrN8hFjlE"
}
```

## Deploy
Deploying chaincode with a 1KB value for first key i.e,  a0 (In chaincode **'a'** is changed to a0)
POST: http://127.0.0.1:5000/chaincode

```
{
  "jsonrpc": "2.0",
  "method": "deploy",
  "params": {
    "type": 1,
    "chaincodeID":{
        "path":"https://github.com/asararatnakar/mychaincode_example02"
    },
    "ctorMsg": {
        "function":"init",
        "args":["a","Yh1WWZlw1gGd2qyMNaHqBCt4zuBrnT4cvZ5iMXRRM3YBMXLZmmvyVr0ybWfiX4N3UMliEVA0d1dfTxvKs0EnHAKQe4zcoGVLzMHd8jPQlR5ww3wHeSUGOutios16lxfuQTdnsFcxhXLiGwp83ahyBomdmJ3igAYTyYw2bwXqhBeL9fa6CTK43M2QjgFhQtlcpsh7XMcUWnjJhvMHAyH67Z8Ugke6U8GQMO5aF1Oph0B2HlIQUaHMq2i6wKN8ZXyx7CCPr7lKnIVWk4zn0MLZ16LstNErrmsGeo188Rdx5Yyw04TE2OSPSsaQSDO6KrDlHYnT2DahsrY3rt3WLfBZBrUGhr9orpigPxhKq1zzXdhwKEzZ0mi6tdPqSzMKna7O9STstf2aFdrnsoovOm8SwDoOiyqfT5fc0ifVZSytVNeKE1C1eHn8FztytU2itAl1yDYSfTZQv42tnVgDjWcLe2JR1FpfexVlcB8RUhSiyoThSIFHDBZg8xyULPmp4e6acOfKfW2BXh1IDtGR87nBWqmytTOZrPoXRPq2QXiUjZS2HflHJzB0giDbWEeoZoMeF11364Xzmo0iWsBw0TQ2cHapS4cR49IoEDWkC6AJgRaNb79s6vythxX9CqfMKxIpqYAbm3UAZRS7QU7MiZu2qG3xBIEegpTrkVNneprtlgh3uTSVZ2n2JTWgexMcpPsk0ILh10157SooK2P8F5RcOVrjfFoTGF3QJTC2jhuobG3PIXs5yBHdELe5yXSEUqUm2ioOGznORmVBkkaY4lP025SG1GNPnydEV9GdnMCPbrgg91UebkiZsBMM21TZFbUqP70FDAzMWZKHDkDKCPoO7b8EPXrz3qkyaIWBymSlLt6FNPcT3NkkTfg7wl4DZYDvXA2EYu0riJvaWon12KWt9aOoXig7Jh4wiaE1BgB3j5gsqKmUZTuU9op5IXSk92EIqB2zSM9XRp9W2I0yLX1KWGVkkv2OIsdTlDKIWQS9q1W8OFKuFKxbAEaQwhc7Q5Mm","b","0"]
    },
    "secureContext":"test_user0"
  },
  "id": 1
}
```

##Query
**NOTE:** Changee the chaincode name based on deploy 
Querying for a0 as whic inserted as part of Deploy

```
{
  "jsonrpc": "2.0",
  "method": "query",
  "params": {
      "type": 1,
      "chaincodeID":{
          "name":"8f19ec1f6e6227f8360c1c1e12e5a3b41678ebd467955846df3943a833b1220e83848bf5697c83b9b1c4b627ade4b0616f500fd0f6603bd7a3cff63ecc8d9fa2"
      },
      "ctorMsg": {
         "function":"query",
         "args":["a0"]
      },
    "secureContext":"test_user0"
  },
  "id": 5
}
```

Also you can query for counter (i.e, b)

```
{
  "jsonrpc": "2.0",
  "method": "query",
  "params": {
      "type": 1,
      "chaincodeID":{
          "name":"8f19ec1f6e6227f8360c1c1e12e5a3b41678ebd467955846df3943a833b1220e83848bf5697c83b9b1c4b627ade4b0616f500fd0f6603bd7a3cff63ecc8d9fa2"
      },
      "ctorMsg": {
         "function":"query",
         "args":["b"]
      },
    "secureContext":"test_user0"
  },
  "id": 5
}
```

##Invokes 
each invoke you pass these arguments and internally it changed to the counter (b value) and also append counter to the first argument(ie., a1, a2 ..... aN)
**NOTE:** Change the chaincode as per the deploy

```
{
  "jsonrpc": "2.0",
  "method": "invoke",
  "params": {
      "type": 1,
      "chaincodeID":{
          "name":"8f19ec1f6e6227f8360c1c1e12e5a3b41678ebd467955846df3943a833b1220e83848bf5697c83b9b1c4b627ade4b0616f500fd0f6603bd7a3cff63ecc8d9fa2"
      },
      "ctorMsg": {
         "function":"invoke",
         "args":["a","Yh1WWZlw1gGd2qyMNaHqBCt4zuBrnT4cvZ5iMXRRM3YBMXLZmmvyVr0ybWfiX4N3UMliEVA0d1dfTxvKs0EnHAKQe4zcoGVLzMHd8jPQlR5ww3wHeSUGOutios16lxfuQTdnsFcxhXLiGwp83ahyBomdmJ3igAYTyYw2bwXqhBeL9fa6CTK43M2QjgFhQtlcpsh7XMcUWnjJhvMHAyH67Z8Ugke6U8GQMO5aF1Oph0B2HlIQUaHMq2i6wKN8ZXyx7CCPr7lKnIVWk4zn0MLZ16LstNErrmsGeo188Rdx5Yyw04TE2OSPSsaQSDO6KrDlHYnT2DahsrY3rt3WLfBZBrUGhr9orpigPxhKq1zzXdhwKEzZ0mi6tdPqSzMKna7O9STstf2aFdrnsoovOm8SwDoOiyqfT5fc0ifVZSytVNeKE1C1eHn8FztytU2itAl1yDYSfTZQv42tnVgDjWcLe2JR1FpfexVlcB8RUhSiyoThSIFHDBZg8xyULPmp4e6acOfKfW2BXh1IDtGR87nBWqmytTOZrPoXRPq2QXiUjZS2HflHJzB0giDbWEeoZoMeF11364Xzmo0iWsBw0TQ2cHapS4cR49IoEDWkC6AJgRaNb79s6vythxX9CqfMKxIpqYAbm3UAZRS7QU7MiZu2qG3xBIEegpTrkVNneprtlgh3uTSVZ2n2JTWgexMcpPsk0ILh10157SooK2P8F5RcOVrjfFoTGF3QJTC2jhuobG3PIXs5yBHdELe5yXSEUqUm2ioOGznORmVBkkaY4lP025SG1GNPnydEV9GdnMCPbrgg91UebkiZsBMM21TZFbUqP70FDAzMWZKHDkDKCPoO7b8EPXrz3qkyaIWBymSlLt6FNPcT3NkkTfg7wl4DZYDvXA2EYu0riJvaWon12KWt9aOoXig7Jh4wiaE1BgB3j5gsqKmUZTuU9op5IXSk92EIqB2zSM9XRp9W2I0yLX1KWGVkkv2OIsdTlDKIWQS9q1W8OFKuFKxbAEaQwhc7Q5Mm","b"]
      },
      "secureContext":"test_user0"
  },
  "id": 3
}
```


NOTE: 

Already there is an example02 modified available [here](https://github.com/asararatnakar/fabric/blob/master/examples/chaincode/go/chaincode_example02/chaincode_example02.go) for GoSDK 
