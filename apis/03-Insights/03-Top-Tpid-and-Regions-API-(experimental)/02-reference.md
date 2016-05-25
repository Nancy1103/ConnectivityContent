# Reference

## Authentication

The Top TPID and Regions API requires HTTP basic auth credentials to be supplied with every request.

Credentials must match a valid hotelier username/password.  If data for a specific hotel or list of hotels is being requested, the credentials must be authorized to access data for those hotels.

## Top TPID and Regions Endpoint

```
GET lodgingSort/v1/hops/HopsTopTpidsAndRegions
```

The Top TPID and Regions endpoint will return a sorted list of top regions for each of the top tpids for a specified hotel. 


### Request Parameters

| Name                | Parameter Type | Data Type | Example              | Description
|---------------------|----------------|-----------|----------------------|-----------------|
| `Authorization`     | Header         | String    | `Basic dGVzdDp0ZXN0` | Unique identifier for a property/hotel. |
| `hotelId`           | Query          | Integer   | `123`                | Hotel identifier |
| `cid`               | Query          | String    | `prague-hackathon`   | Client identifier |

##### Sample Request with all parameters specified
```
https://int.services.expediapartnercentral.com/top-tpids/lodgingSort/v1/hops/HopsTopTpidsAndRegions?hotelId=1
```

### Response Format
|Name | Type | Description|
|-----|------|-------------| 
|hotelId | Integer | Unique identifier for a property/hotel. |
|tpid |    Integer | Point of Sale (POSa) |
| regionId |   Long    | GAIA region ID |

##### Sample Response

```
{
    "hopsTpidsList": [
        {
            "sortedRegionList": [
                2114,
                800053,
                6144914
            ],
            "tpid": 3101
        },
        {
            "sortedRegionList": [
                2114
            ],
            "tpid": 3109
        },
        {
            "sortedRegionList": [
                2114
            ],
            "tpid": 3107
        }
    ],
    "hotelId": 1
}

```

#### Error Messages

When there are errors, the Top TPID and Regions API always returns a 200 response.  The error message is specified in an error field in the response.  The following are some sample error messages:

```JSON
{
    "error":"No hotelId specified"
}
```

```JSON
{
    "error":"No clientId specified or unknown clientId"
}
```

```JSON
{
    "error":"No data found",
    "hotelId":1234567890
}
```
