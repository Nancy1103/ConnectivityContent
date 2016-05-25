# Quick Start

The Sort Rank API displays a hotel's average sort position on Expedia top TPIDs and Regions for the next 90 days. Hoteliers can use this information to make informed decisions to increase visibility and/or to view/analyze the effect of an Accelerator on sort.

----

## Authentication
Basic Authentication in HTTP header, using your Expedia Partner Central (EPC) credentials.  Please work with your account manager to activate your credentials for test and production API access.

----

## Reading Sort Rank Data

The most basic call to retreive sort rank data is to simply pass a hotel id and client id to the <lodgingSort/v1/hops/HopsAverageRanks> endpoint via a query parameter.  

```
https://int.services.expediapartnercentral.com/sort-ranks/lodgingSort/v1/hops/HopsAverageRanks?hotelId=1
```

The response will contain the hotel's average sort position for check in dates starting from today for the next 90 days in that hotel's top TPIDs and regions.

----

To retreive a smaller range of checkin data, the search date, check-in date and number of days can be specified.

```
https://int.services.expediapartnercentral.com/sort-ranks/lodgingSort/v1/hops/HopsAverageRanks?hotelId=1&searchDate=2016-05-15&checkin=2016-05-16&numDays=1
```

The response will contain only the specified checkin dates: 

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