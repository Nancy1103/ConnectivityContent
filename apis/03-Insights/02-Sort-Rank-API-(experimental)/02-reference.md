# Reference

## Authentication

The Sort Ranks API requires HTTP basic auth credentials to be supplied with every request.

Credentials must match a valid hotelier username/password.  If data for a specific hotel or list of hotels is being requested, the credentials must be authorized to access data for those hotels.

## Sort Rank Endpoint

```
GET /lodgingSort/v1/hops/HopsAverageRanks
```

The Sort Rank endpoint will return a hotel's average sort rank data for a range of check-in dates.  

### Request Parameters

| Name                | Parameter Type | Data Type | Example              | Description
|---------------------|----------------|-----------|----------------------|-----------------|
| `Authorization`     | Header         | String    | `Basic dGVzdDp0ZXN0` | Unique identifier for a property/hotel. |
| `hotelId`           | Query          | Integer   | `123`                | Hotel identifier |
| `cid`               | Query          | String    | `prague-hackathon`   | Client identifier |
| `searchDate`        | Query          | String    | `2016-05-15`   | Can send in a list of search dates. Format: YYYY-MM-DD. Maximum number of search dates: 2 Each search date should be in this range: from today to previous 90 days (from today - 90 days to today) both inclusive. i.e. today-90 <= searchDate <= today, for each searchDate |
| `checkin`           | Query          | String    | `2016-05-16`   | Checkin date starts on this day.  Checkin date range: from search date to next 90 days. i.e. searchDate<= checkIn <= searchDate+89 |
| `numDays`           | Query          | Integer   | `1`   | Number of checkin dates. Max is 90 days and numDays + checkIn < searchDate + 90. Average sort rank/price/compensation is returned for each Checkin date, where Checkin date starts = checkIn and Checkin date ends = checkIn + numDays - 1. |
| `currency`          | Query          | String    | `usd`   | Standard ISO currency code (for average price and average compensation). Currently only the default USD is supported

##### Sample Request with all parameters specified
```
https://int.services.expediapartnercentral.com/sort-ranks/lodgingSort/v1/hops/HopsAverageRanks?hotelId=1&searchDate=2016-05-15&checkin=2016-05-16&numDays=2
```

### Response

The API returns a hotel's average sort rank, average price, and average compensation in JSON format aggregated over:
- Search Date: A list of search date is sent in the request, and the aggregation is for each search date. 
- TPID: up to 3 TPIDs are defined for a hotel in top TPIDs and Regions list (see Sort Rank Report: Top TPIDs and Regions API), and the aggregation is over each TPID.
- Region: up to 3 Regions are defined for each of the top TPID in top TPIDs and Regions list, and the aggregation is over each Region.
- Checkin date: average sort rank/price/compensation is returned for each checkin date. Checkin date starts at the checkIn (as sent in the request) and ends at checkIn + numDays - 1.

### Response Format
|Name | Type | Description|
|-----|------|-------------| 
|hotelId | Integer | Hotel Identifier |
|searchDate | String |  YYYY-MM-DD |  
|tpid |   Integer | Point of Sale (POSa) |     
|checkinDate |   String | YYYY-MM-DD |
|regionId  |  Long  |  GAIA region ID |   
|avgRank | Double |  Average sort rank (for all searches done on the specified search date for the specified checkin date on the specified TPID and region).  |
| avgPrice  |  Double | Average price is returned in USD.    |
| avgComp | Double | Average compensation is returned in USD.  |


##### Sample response

```JSON
{
    "hotelId": 1,
    "searchDates": [
        {
            "searchDate": "2016-05-15",
            "tpids": [
                {
                    "regions": [
                        {
                            "data": [
                                {
                                    "avgComp": 76.67,
                                    "avgPrice": 306.69,
                                    "avgRank": 374.24,
                                    "checkinDate": "2016-05-16"
                                },
                                {
                                    "avgComp": 95.34,
                                    "avgPrice": 381.37,
                                    "avgRank": 272.98,
                                    "checkinDate": "2016-05-17"
                                }
                            ],
                            "regionId": 2114
                        },
                        {
                            "data": [
                                {
                                    "avgComp": 97.66,
                                    "avgPrice": 390.64,
                                    "avgRank": 3.0,
                                    "checkinDate": "2016-05-17"
                                }
                            ],
                            "regionId": 800053
                        },
                        {
                            "data": [],
                            "regionId": 6144914
                        }
                    ],
                    "tpid": 3101
                },
                {
                    "regions": [
                        {
                            "data": [
                                {
                                    "avgComp": 79.78,
                                    "avgPrice": 319.11,
                                    "avgRank": 436.18,
                                    "checkinDate": "2016-05-16"
                                },
                                {
                                    "avgComp": 97.58,
                                    "avgPrice": 390.3,
                                    "avgRank": 387.64,
                                    "checkinDate": "2016-05-17"
                                }
                            ],
                            "regionId": 2114
                        }
                    ],
                    "tpid": 3107
                },
                {
                    "regions": [
                        {
                            "data": [
                                {
                                    "avgComp": 71.94,
                                    "avgPrice": 287.75,
                                    "avgRank": 372.18,
                                    "checkinDate": "2016-05-16"
                                },
                                {
                                    "avgComp": 93.25,
                                    "avgPrice": 372.97,
                                    "avgRank": 276.72,
                                    "checkinDate": "2016-05-17"
                                }
                            ],
                            "regionId": 2114
                        }
                    ],
                    "tpid": 3109
                }
            ]
        }
    ]
}
```

#### Error Messages

Even when there are errors, the Sort Ranks API always returns a 200 response.  The error message is specified in an error field in the response.  The following are some sample error messages:

```JSON
{
  "error": "No clientId specified or unknown clientId"
}
```

```JSON
{
  "error": "No hotelId specified"
}
```

```JSON
{
  "error": "Search Date(s) out of range",
  "hotelId": 1
}
```

```JSON
{
  "error": "Exceeded max. number of Search Dates",
  "hotelId": 1
}
```

