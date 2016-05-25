# Quick Start

As a hotel manager, by specifying your hotel id, startDate and endDate, you can find a list of hotels,which have added your hotel as competitor in that time period. Hoteliers may want to use this API to become aware of competitors list.

----

## Authentication

Each request should include hotelier credentials in HTTP basic auth format, a clientId query parameter, a startDate query parameter, an endDate query parameter, and a hotelId query parameter.

----

## Reading Competitor Set Data

To retreive competitor set data pass a hotel id and number of days along with the required authentication parameters to the ```public/v1/addCompSet``` endpoint via query parameters.  

```
https://int.services.expediapartnercentral.com/insights/public/v1/addCompSet?hotelId=1234567&startDate=2014-01-01&endDate=2016-05-10&clientId=insights-test
```


The response will contain the competitor set data in the following format: 

```JSON
{
    "data": {
        "HotelId": 0,
        "HotelName": "",
        "compSet": [
            {
                "eventDate": "",
                "hotelId": 0,
                "hotelName": ""
            }
        ],
        "endDate": "",
        "startDate": ""
    },
    "errorCode": "",
    "errorMsg": "",
    "status": ""
}
```