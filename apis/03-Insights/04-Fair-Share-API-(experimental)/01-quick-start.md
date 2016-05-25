# Quick Start

The FairShare API returns a hotel’s expected fair share and actual market share per stay dates. 
The equation for fair share is my room count /(my room count + my competitors' total room count). 
The equation for actual share is my bookings /(my bookings + my competitors’ total bookings)

----

## Authentication
Each request should include hotelier credentials in HTTP basic auth format, a clientId query parameter, a dayNum query parameter, and a hotelId query parameter.

----

## Reading Fair Share Data

To retreive fair share data pass a hotel id and number of days along with the required authentication parameters to the ```public/v1/fairShare``` endpoint via query parameters.  

```
https://int.services.expediapartnercentral.com/insights/public/v1/fairShare?hotelId=759&dayNum=2&clientId=insights-test
```

The response will contain the fair share data in the following format: 

```JSON
{
    "data": {
        "compSetRoomCount": 3322,
        "daily": [
            {
                "bookedRooms": 0,
                "compSetBookedRooms": 0,
                "date": "2016-05-18"
            },
            {
                "bookedRooms": 0,
                "compSetBookedRooms": 0,
                "date": "2016-05-19"
            }
        ],
        "fairshare": 0.024,
        "roomCount": 83
    },
    "errorCode": null,
    "errorMsg": null,
    "status": "Success"
}
```