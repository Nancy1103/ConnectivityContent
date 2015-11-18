# Intro
This section contains examples on how to query the property resource of the product API.

Property read allows partners to retrieve several important settings related to their properties’ configuration in Expedia system. Some of these settings will help partners better understand how to manage room type and rate plan resources. 

It also enables partners to find out which properties are currently assigned to their accounts, by simply calling the /properties endpoint and iterating through all the properties returned.

## Single Property Request / Response
Request is a simple HTTP GET:
```HTTP
GET https://services.expediapartnercentral.com/products/v1/properties/2268140
Content-Type: application/json
Accept: application/json
Authorization: Basic [your encoded username:password in base64]
```
Response:
```json
{
  "entity": {
    "resourceId": 2268140,
    "name": "Eslington Villa",
    "status": "Active",
    "currency": "GBP",
    "address": {
      "line1": "199 Some Street",
      "line2": null,
      "city": "Glasgow",
      "state": null,
      "postalCode": "12345",
      "countryCode": "GBR"
    },
    "distributionModels": [
      "ExpediaCollect", "HotelCollect"
    ],
    "rateAcquisitionType": "SellLAR",
    "taxInclusive": true,
    "pricingModel": "PerDayPricing",
    "baseAllocationEnabled": false,
    "minLOSThreshold": 4,
    "cancellationTime": "18:00",
    "timezone": "(GMT) Greenwich Mean Time : Dublin, Edinburgh, Lisbon, London",
    "reservationCutOff": {
      "time": "05:00",
      "day": "nextDay"
    }
  }
}
```
## Multiple Properties Request/Response
When using GET for multiple properties, additional parameters can be provided to navigate through the result set. By default, only 20 properties are returned at a time. Partners who have more than 20 properties assigned to their accounts and want to get through all their properties have to use offset and limit parameters.
For example, a partner wanting to get 3 results at a time would do a request like this:
```HTTP
GET https://services.expediapartnercentral.com/products/v1/properties?offset=0&limit=3
Content-Type: application/json
Accept: application/json
Authorization: Basic [your encoded username:password in base64]
```
The Product API will sort all properties assigned to this account by resourceId, ascending, and return the 3 lowest property resource ids. Response:
```JSON
{
  "entity": [
    {
      "resourceId": 2268142,
      "name": "Eslington Villa",
      "status": "Active",
      "currency": "GBP",
      "address": {
        "line1": "199 Some Street",
        "city": "Glasgow",
        "postalCode": "PL99A9",
        "state": "Scotland",
        "countryCode": "GBR"
      },
      "distributionModels": [
        "ExpediaCollect", "HotelCollect"
      ],
      "rateAcquisitionType": "SellLAR",
      "taxInclusive": true,
      "pricingModel": "PerDayPricing",
      "baseAllocationEnabled": false,
      "minLOSThreshold": 4,
      "cancellationTime": "18:00",
      "timezone": "(GMT) Greenwich Mean Time : Dublin, Edinburgh, Lisbon, London",
      "reservationCutOff": {
        "time": "05:00",
        "day": "nextDay"
      }
    }, {
      "resourceId": 2268141,
      "name": "Hotel Downtown",
      "status": "Active",
      "currency": "CAD",
      "address": {
        "line1": "299 Some Other Street",
        "line2": "with a very long name",
        "city": "Montreal",
        "state": "Quebec",
        "postalCode": "H11H1H",
        "countryCode": "CAN"
      },
      "distributionModels": [
        "ExpediaCollect"
      ],
      "rateAcquisitionType": "SellLAR",
      "taxInclusive": false,
      "pricingModel": "OccupancyBasedPricing",
      "baseAllocationEnabled": false,
      "minLOSThreshold": 14,
      "cancellationTime": "18:00",
      "timezone": "(GMT-05:00) Eastern Time (US & Canada)",
      "reservationCutOff": {
        "time": "23:59",
        "day": "sameDay"
      }
    }, {
      "resourceId": 2268140,
      "name": "Villa by the Sea",
      "status": "Active",
      "currency": "EUR",
      "address": {
        "line1": "299 Some Other Street",
        "city": "Palma de Mallorca",
        "postalCode": "7610",
        "countryCode": "ESP"
      },
      "distributionModels": [
        "ExpediaCollect", "HotelCollect"
      ],
      "rateAcquisitionType": "SellLAR",
      "taxInclusive": true,
      "pricingModel": "OccupancyBasedPricing",
      "baseAllocationEnabled": false,
      "minLOSThreshold": 28,
      "cancellationTime": "18:00",
      "timezone": "(GMT+01:00) Amsterdam, Berlin, Bern, Rome, Stockholm, Vienna",
      "reservationCutOff": {
        "time": "05:00",
        "day": "nextDay"
      }
    }
  ]
}
```
Partners can then iterate through their properties by increasing the offset parameter. To get the next 3 properties, partners would issue a GET with offset=3, limit=3
```HTTP
GET https://services.expediapartnercentral.com/products/v1/properties?offset=3&limit=3
```
To get the next results, offset would become 6:
```HTTP
GET https://services.expediapartnercentral.com/products/v1/properties?offset=6&limit=3
```

