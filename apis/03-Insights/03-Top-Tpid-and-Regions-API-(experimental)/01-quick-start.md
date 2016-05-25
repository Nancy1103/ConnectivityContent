# Quick Start

Rank Optimizer is a tool in Expedia Partner Central (EPC) that shows hoteliers their average placement position (aka sort rank) in hotel search result, with the goal of helping them understand the main factors that influence placement and how to optimize for visibility and booking. In order to provide the most relevant information, average sort rank is shown for the Point of Sales (POSa) and regions (aka, top TPIDs and Regions) where the hotel is most popular. This API provides the top TPIDs and region data.

----

## Authentication
Basic Authentication in HTTP header, using your Expedia Partner Central (EPC) credentials.  Please work with your account manager to activate your credentials for test and production API access.

----

## Reading Top TPID and Region Data

To retreive sort rank data is to simply pass a hotel id and client id to the <lodgingSort/v1/hops/HopsTopTpidsAndRegions> endpoint via a query parameter.  

```
https://int.services.expediapartnercentral.com/top-tpids/lodgingSort/v1/hops/HopsTopTpidsAndRegions?hotelId=1
```

The response will contain a sorted list of top regions for each of the top tpids for the specified hotel. 

```JSON
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