# Sample Messages
This section contains sample messages illustrating how to interact with the product API.

## Property Resource Examples
This section contains examples on how to query the property resource of the product API.

Property read allows partners to retrieve several important settings related to their properties’ configuration in Expedia system. Some of these settings will help partners better understand how to manage room type and rate plan resources. 

It also enables partners to find out which properties are currently assigned to their accounts, by simply calling the /properties endpoint and iterating through all the properties returned.

### Multiple Properties Read
When using GET for multiple properties, additional parameters can be provided to navigate through the result set. By default, 
only 20 properties are returned at a time. Partners who have more than 20 properties assigned to their accounts and want to get through all their properties have to use offset and limit parameters.
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
	  "parnerCode": "ABC123",
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
	  "parnerCode": "DEF456",
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
	  "parnerCode": "GHI789",
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

### Single Property Read
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
    "parnerCode": "ABC123",
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

##	Room Type Resource Examples
The room type resource describes the configuration of a specific room type/class/category in the Expedia system. Room types belong to properties, and room types contain one to many rate plans. A room type resource will contain information such as bed types, smoking preferences, occupancy settings by age categories, etc.

Against the room type resource, partners can retrieve a list of room types or a specific room type. Partners can also create new room types (one at a time), and edit an existing room type (one at a time, full overlay operation).

### All Room Types Read
This example shows how to do a read request to retrieve all room types under a given property. We'll illustrate how to add the status param on the request to get all active and inactive room types. In this example, the property has 2 active and 2 inactive room types, so the request will return 4 room types, sorted by resourceId.

Request:
```HTTP
GET https://services.expediapartnercentral.com/products/v1/properties/1780041/roomTypes?status=all
Content-Type: application/json
Accept: application/json
Authorization: Basic [your encoded username:password in Base64]
```
Response:
```JSON
{
    "entity": [
        {
            "resourceId": 204830,
            "partnerCode": "Standard Room ",
            "name": {
                "value": "Standard Room"
            },
            "status": "Inactive",
            "maxOccupants": 2,
            "occupancyByAge": [
                {
                    "ageCategory": "Adult",
                    "minAge": 18,
                    "maxOccupants": 2
                }
            ],
            "bedTypes": [
                {
                    "id": "1.14",
                    "name": "1 king bed"
                },
                {
                    "id": "1.15",
                    "name": "1 queen bed"
                }
            ],
            "smokingPreferences": [
                {
                    "id": "2.1",
                    "name": "Non-Smoking"
                },
                {
                    "id": "2.2",
                    "name": "Smoking"
                }
            ],
            "roomSize": {
              "squareFeet": 1023,
              "squareMeters": 95
            },
            "views": [
              "Ocean View",
              "Beach View"
            ],
            "wheelchairAccessibility": false
        },
        {
            "resourceId": 204832,
            "partnerCode": "Superior Room ",
            "name": {
                "value": "Superior Room"
            },
            "status": "Inactive",
            "maxOccupants": 3,
            "occupancyByAge": [
                {
                    "ageCategory": "Adult",
                    "minAge": 18,
                    "maxOccupants": 2
                },
                {
                    "ageCategory": "ChildAgeA",
                    "minAge": 2,
                    "maxOccupants": 2
                },
                {
                    "ageCategory": "Infant",
                    "minAge": 0,
                    "maxOccupants": 2
                }
            ],
            "bedTypes": [
                {
                    "id": "1.14",
                    "name": "1 king bed"
                },
                {
                    "id": "1.15",
                    "name": "1 queen bed"
                }
            ],
            "smokingPreferences": [
                {
                    "id": "2.1",
                    "name": "Non-Smoking"
                },
                {
                    "id": "2.2",
                    "name": "Smoking"
                }
            ],
            "roomSize": {
              "squareFeet": 1023,
              "squareMeters": 95
            },
            "views": [
              "Ocean View"
            ],
            "wheelchairAccessibility": false
        },
        {
            "resourceId": 209857,
            "partnerCode": "Deluxe Suite",
            "name": {
                "attributes": {
                    "typeOfRoom": "Suite",
                    "roomClass": "Deluxe"
                },
                "value": "Deluxe Suite"
            },
            "status": "Active",
            "maxOccupants": 3,
            "occupancyByAge": [
                {
                    "ageCategory": "Adult",
                    "minAge": 17,
                    "maxOccupants": 2
                },
                {
                    "ageCategory": "ChildAgeA",
                    "minAge": 0,
                    "maxOccupants": 2
                }
            ],
            "bedTypes": [
                {
                    "id": "1.14",
                    "name": "1 king bed"
                },
                {
                    "id": "1.21",
                    "name": "2 double beds"
                }
            ],
            "smokingPreferences": [
                {
                    "id": "2.1",
                    "name": "Non-Smoking"
                },
                {
                    "id": "2.2",
                    "name": "Smoking"
                }
            ],
            "roomSize": {
              "squareFeet": 1023,
              "squareMeters": 95
            },
            "views": [
              "Beach View"
            ],
            "wheelchairAccessibility": false
        },
        {
            "resourceId": 211705,
            "partnerCode": "211705",
            "name": {
                "value": "Junior Suite"
            },
            "status": "Active",
            "maxOccupants": 3,
            "occupancyByAge": [
                {
                    "ageCategory": "Adult",
                    "minAge": 18,
                    "maxOccupants": 2
                },
                {
                    "ageCategory": "ChildAgeA",
                    "minAge": 0,
                    "maxOccupants": 2
                }
            ],
            "bedTypes": [
                {
                    "id": "1.14",
                    "name": "1 king bed"
                },
                {
                    "id": "1.21",
                    "name": "2 double beds"
                }
            ],
            "smokingPreferences": [
                {
                    "id": "2.1",
                    "name": "Non-Smoking"
                },
                {
                    "id": "2.2",
                    "name": "Smoking"
                }
            ],
            "roomSize": {
              "squareFeet": 1023,
              "squareMeters": 95
            },
            "wheelchairAccessibility": false
        }
    ]
}
```

###	Single Room Type Read
This example shows how to do a read request for a single room type with 2 age categories, a choice of 2 bed types, and supporting both smoking and non-smoking

Request:
```HTTP
GET https://services.expediapartnercentral.com/products/v1/properties/1780044/roomTypes/200616960
Content-Type: application/json
Accept: application/json
Authorization: Basic [your encoded username:password in Base64]
```
Response:
```JSON
{
  "entity": {
    "resourceId": 211705,
    "partnerCode": "MySuiteCode123",
    "name": {
      "attributes": {
        "typeOfRoom": "Suite",
        "roomClass": "Deluxe",
        "includeBedType": true,
        "featuredAmenity": "Terrace",
        "view": "Ocean View"
      },
      "value": "Deluxe Suite, 1 King Bed with Sofabed, Terrace, Ocean View"
    },
    "status": "Active",
    "maxOccupants": 4,
    "occupancyByAge": [
      {
        "ageCategory": "Adult",
        "minAge": 18,
        "maxOccupants": 4
      }, {
        "ageCategory": "ChildAgeA",
        "minAge": 0,
        "maxOccupants": 3
      }
    ],
    "bedTypes": [
      {
        "id": "1.67",
        "name": "1 king and 1 sofa bed"
      }
    ],
    "smokingPreferences": [
      {
        "id": "2.1",
        "name": "Non-Smoking"
      }, {
        "id": "2.2",
        "name": "Smoking"
      }
    ],
    "roomSize": {
      "squareFeet": 1023,
      "squareMeters": 95
    },
    "views": [
      "Ocean View",
      "Beach View"
    ],
    "wheelchairAccessibility": false
  }
}
```

### Room Type Create (Predefined Room Name)
When creating a new room type, partners have the choice to pick a predefined name, or provide a set of attributes that can be included in the room name.

This example creates a room type with a predefined name, 3 age categories, a single bedding configuration and non-smoking
```HTTP
POST https://services.expediapartnercentral.com/products/v1/properties/1780041/roomTypes
Accept: application/json
Content-Type: application/json
```
```JSON
{
  "partnerCode": "JS001",
  "name": {
    "value": "Junior Suite"
  },
  "maxOccupants": 5,
  "occupancyByAge": [
    {
      "ageCategory": "Adult",
      "minAge": 18,
      "maxOccupants": 2
    }, {
      "ageCategory": "ChildAgeA",
      "minAge": 3,
      "maxOccupants": 2
    }, {
      "ageCategory": "Infant",
      "minAge": 0,
      "maxOccupants": 2
    }
  ],
  "bedTypes": [
    {
      "id": "1.88",
      "name": "2 queen and 1 sofa bed"
    }
  ],
  "smokingPreferences": [
    {
      "id": "2.1",
      "name": "Non-Smoking"
    }
  ],
  "roomSize": {
    "squareFeet": 1023,
    "squareMeters": 95
  },
  "views": [
    "Ocean View",
    "Beach View"
  ],
  "wheelchairAccessibility": false
}
```
When successful, the API will respond with what Expedia created. Please note that room types are always created as inactive, and will become active automatically when the first active rate plan is created.
```JSON
{
  "entity": {
    "resourceId": 201171339,
    "partnerCode": "JS001",
    "name": {
      "value": "Junior Suite"
    },
    "status": "Inactive",
    "maxOccupants": 5,
    "occupancyByAge": [
      {
        "ageCategory": "Adult",
        "minAge": 18,
        "maxOccupants": 2
      }, {
        "ageCategory": "ChildAgeA",
        "minAge": 3,
        "maxOccupants": 2
      }, {
        "ageCategory": "Infant",
        "minAge": 0,
        "maxOccupants": 2
      }
    ],
    "bedTypes": [
      {
        "id": "1.88",
        "name": "2 queen and 1 sofa bed"
      }
    ],
    "smokingPreferences": [
      {
        "id": "2.1",
        "name": "Non-Smoking"
      }
    ],
    "roomSize": {
      "squareFeet": 1023,
      "squareMeters": 95
    },
    "views": [
      "Ocean View",
      "Beach View"
    ],
    "wheelchairAccessibility": false
  }
}
```

###	Room Type Create (Room Name Attributes)
When creating a new room type, partners have the choice to pick a predefined name, or provide a set of attributes that can be included in the room name.

This example creates a room type with a set of name attributes, 3 age categories, a single bedding configuration and non-smoking
```HTTP
POST https://services.expediapartnercentral.com/products/v1/properties/1780041/roomTypes
Accept: application/json
Content-Type: application/json
```
```JSON
{
  "partnerCode": "JS001",
  "name": {
    "attributes": {
      "typeOfRoom": "Condo",
      "bedroomDetails": "3 Bedrooms",
      "featuredAmenity": "2 Bathrooms",
      "view": "Partial Ocean View"
    }
  },
  "maxOccupants": 8,
  "occupancyByAge": [
    {
      "ageCategory": "Adult",
      "minAge": 18,
      "maxOccupants": 7
    }, {
      "ageCategory": "ChildAgeA",
      "minAge": 3,
      "maxOccupants": 5
    }, {
      "ageCategory": "Infant",
      "minAge": 0,
      "maxOccupants": 2
    }
  ],
  "bedTypes": [
    {
      "id": "1.149",
      "name": "3 double and 1 sofa bed"
    }
  ],
  "smokingPreferences": [
    {
      "id": "2.1",
      "name": "Non-Smoking"
    }
  ],
  "roomSize": {
    "squareFeet": 1023,
    "squareMeters": 95
  },
  "views": [
    "Partial Ocean View",
    "Beach View"
  ],
  "wheelchairAccessibility": false
}
```
When successful, the API will respond with what Expedia created. Please note that room types are always created as inactive, and will become active automatically when the first active rate plan is created.
```JSON
{
  "entity": {
    "resourceId": 201171339,
    "partnerCode": "JS001",
    "name": {
      "attributes": {
        "typeOfRoom": "Condo",
        "bedroomDetails": "3 Bedrooms",
        "featuredAmenity": "2 Bathrooms",
        "view": "Partial Ocean View"
      },
      "value": "Condo, 3 Bedrooms, 2 Bathrooms, Partial Ocean View"
    },
    "maxOccupants": 8,
    "occupancyByAge": [
      {
        "ageCategory": "Adult",
        "minAge": 18,
        "maxOccupants": 7
      }, {
        "ageCategory": "ChildAgeA",
        "minAge": 3,
        "maxOccupants": 5
      }, {
        "ageCategory": "Infant",
        "minAge": 0,
        "maxOccupants": 2
      }
    ],
    "bedTypes": [
      {
        "id": "1.149",
        "name": "3 double and 1 sofa bed"
      }
    ],
    "smokingPreferences": [
      {
        "id": "2.1",
        "name": "Non-Smoking"
      }
    ],
    "roomSize": {
      "squareFeet": 1023,
      "squareMeters": 95
    },
    "views": [
      "Partial Ocean View",
      "Beach View"
    ],
    "wheelchairAccessibility": false
  }
}
```
### Room Type Create (Ignored Room Name Attributes)
When using room name attributes to generate a name, Expedia has specific rules around how many attributes can be used. To abstract this complexity from our partners, the API will accept that partners specify more attributes than Expedia would actually use to generate the name. The selection logic and ranking of attributes are described in details in our API Definition section. The example below shows what would happen if a partner was to send all possible room name attributes in a create request. The Product API would respond back with the attributes it used, and the name that was generated.

Request:
```JSON
{
  "partnerCode": "JS001",
  "name": {
    "attributes": {
      "typeOfRoom": "Studio",
      "roomClass": "Basic",
      "includeBedType": true,
      "bedroomDetails": "1 Bedroom",
      "includeSmokingPref": true,
      "accessibility": true,
      "view": "Beach View",
      "featuredAmenity": "Hot Tub",
      "area": "Corner",
      "customLabel": "Blue Room"
    }
  },
  "maxOccupants": 8,
  "occupancyByAge": [
    {
      "ageCategory": "Adult",
      "minAge": 18,
      "maxOccupants": 7
    }, {
      "ageCategory": "ChildAgeA",
      "minAge": 3,
      "maxOccupants": 5
    }, {
      "ageCategory": "Infant",
      "minAge": 0,
      "maxOccupants": 2
    }
  ],
  "bedTypes": [
    {
      "id": "1.15",
      "name": "1 queen bed"
    }
  ],
  "smokingPreferences": [
    {
      "id": "2.1",
      "name": "Non-Smoking"
    }
  ],
  "roomSize": {
    "squareFeet": 1023,
    "squareMeters": 95
  },
  "views": [
    "Ocean View",
    "Beach View"
  ],
  "wheelchairAccessibility": false
}
```

The response will not include bedroom details, view, featured amenity and area as we cannot used all attributes to build a name. 
```JSON
{
  "entity": {
    "resourceId": 201171339,
    "partnerCode": "JS001",
    "name": {
      "attributes": {
        "typeOfRoom": "Studio",
        "roomClass": "Basic",
        "includeBedType": true,
        "includeSmokingPref": true,
        "accessibility": true,
        "customLabel": "Blue Room"
      },
      "value": "Basic Studio, 1 Queen Bed, Accessible, Non Smoking (Blue Room)"
    },
    "maxOccupants": 8,
    "occupancyByAge": [
      {
        "ageCategory": "Adult",
        "minAge": 18,
        "maxOccupants": 7
      }, {
        "ageCategory": "ChildAgeA",
        "minAge": 3,
        "maxOccupants": 5
      }, {
        "ageCategory": "Infant",
        "minAge": 0,
        "maxOccupants": 2
      }
    ],
    "bedTypes": [
      {
        "id": "1.15",
        "name": "1 queen bed"
      }
    ],
    "smokingPreferences": [
      {
        "id": "2.1",
        "name": "Non-Smoking"
      }
    ],
    "roomSize": {
      "squareFeet": 1023,
      "squareMeters": 95
    },
    "views": [
      "Ocean View",
      "Beach View"
    ],
    "wheelchairAccessibility": false
  }
}
```

###	Room Type Modify (Predefined Name, Code and Age Categories)
Leveraging the Create example from above, the name is modified to Executive Suite, child age category is removed, and partner code is changed.

```HTTP
PUT https://services.expediapartnercentral.com/products/v1/properties/1780041/roomTypes/201171339
Accept: application/json
Content-Type: application/json
```
```JSON
{
  "resourceId": 201171339,
  "partnerCode": "JS002",
  "name": {
    "value": "Executive Suite"
  },
  "status": "Active",
  "maxOccupants": 5,
  "occupancyByAge": [
    {
      "ageCategory": "Adult",
      "minAge": 18,
      "maxOccupants": 2
    }, {
      "ageCategory": "Infant",
      "minAge": 0,
      "maxOccupants": 2
    }
  ],
  "bedTypes": [
    {
      "id": "1.88",
      "name": "2 queen and 1 sofa bed"
    }
  ],
  "smokingPreferences": [
    {
      "id": "2.1",
      "name": "Non-Smoking"
    }
  ],
  "roomSize": {
    "squareFeet": 1023,
    "squareMeters": 95
  },
  "views": [
    "Ocean View",
    "Beach View"
  ],
  "wheelchairAccessibility": false
}
```
Response look like
```JSON
{
  "entity": {
    "resourceId": 201171339,
    "partnerCode": "JS002",
    "name": {
      "value": "Executive Suite"
    },
    "status": "Active",
    "maxOccupants": 5,
    "occupancyByAge": [
      {
        "ageCategory": "Adult",
        "minAge": 18,
        "maxOccupants": 2
      }, {
        "ageCategory": "Infant",
        "minAge": 0,
        "maxOccupants": 2
      }
    ],
    "bedTypes": [
      {
        "id": "1.88",
        "name": "2 queen and 1 sofa bed"
      }
    ],
    "smokingPreferences": [
      {
        "id": "2.1",
        "name": "Non-Smoking"
      }
    ],
    "roomSize": {
      "squareFeet": 1023,
      "squareMeters": 95
    },
    "views": [
      "Ocean View",
      "Beach View"
    ],
    "wheelchairAccessibility": false
  }
}
```
### Modify Room Type (Predefined Name to Name Attributes)
Leveraging the Create example from above, predefined name is modified to a new room name built from attributes, child age category is removed, and partner code is changed.

The name field is kept with its old value but will be overridden by the room name attributes, as name attributes always take precedence over predefined names.
```HTTP
PUT https://services.expediapartnercentral.com/products/v1/properties/1780041/roomTypes/201171339
Accept: application/json
Content-Type: application/json
```
```JSON
{
  "resourceId": 201171339,
  "partnerCode": "JS002",
  "name": {
    "attributes": {
      "typeOfRoom": "Suite",
      "roomClass": "Executive",
      "bedroomDetails": "1 Bedroom",
      "featuredAmenity": "Jetted Tub",
      "view": "City View"
    },
    "value": "Executive Suite"
  },
  "status": "Active",
  "maxOccupants": 5,
  "occupancyByAge": [
    {
      "ageCategory": "Adult",
      "minAge": 18,
      "maxOccupants": 2
    }, {
      "ageCategory": "Infant",
      "minAge": 0,
      "maxOccupants": 2
    }
  ],
  "bedTypes": [
    {
      "id": "1.88",
      "name": "2 queen and 1 sofa bed"
    }
  ],
  "smokingPreferences": [
    {
      "id": "2.1",
      "name": "Non-Smoking"
    }
  ],
  "roomSize": {
    "squareFeet": 1023,
    "squareMeters": 95
  },
  "views": [
    "City View",
    "Beach View"
  ],
  "wheelchairAccessibility": false
}
```
Response looks like
```JSON
{
  "entity": {
    "resourceId": 201171339,
    "partnerCode": "JS002",
    "name": {
      "attributes": {
        "typeOfRoom": "Suite",
        "roomClass": "Executive",
        "bedroomDetails": "1 Bedroom",
        "featuredAmenity": "Jetted Tub",
        "view": "City View"
      },
      "value": "Executive Suite, 1 Bedroom, Jetted Tub, City View"
    },
    "status": "Active",
    "maxOccupants": 5,
    "occupancyByAge": [
      {
        "ageCategory": "Adult",
        "minAge": 18,
        "maxOccupants": 2
      }, {
        "ageCategory": "Infant",
        "minAge": 0,
        "maxOccupants": 2
      }
    ],
    "bedTypes": [
      {
        "id": "1.88",
        "name": "2 queen and 1 sofa bed"
      }
    ],
    "smokingPreferences": [
      {
        "id": "2.1",
        "name": "Non-Smoking"
      }
    ],
    "roomSize": {
      "squareFeet": 1023,
      "squareMeters": 95
    },
    "views": [
      "City View",
      "Beach View"
    ],
    "wheelchairAccessibility": false
  }
}
```
##	Rate Plan Resource Examples
The rate plan resource defines the configuration of a rate that a partner would like to make available to Expedia customers. It contains the more static information about the rate, for example its name, code, cancellation and change policy, what is the base compensation, what are the additional guest amounts to charge, etc.
More dynamic information like availability and rate information per stay date is exchanged via another API, EQC AR.

### All Active Rate Plans Read
This example shows how to retrieve all active rate plans under a given room type.

Request:
```HTTP
GET https://services.expediapartnercentral.com/products/v1/properties/1780044/roomTypes/200828484/ratePlans
Content-Type: application/json
Accept: application/json
Request-ID : 307af24f-f59a-11e4-822e-005056b1298f
Authorization: Basic [your encoded username:password in Base64]
```
Response:
```JSON
{
    "entity": [
        {
            "resourceId": 205020299,
            "name": "RoomOnly22",
            "rateAcquisitionType": "SellLAR",
            "distributionRules": [
                {
                    "expediaId": "205020299A",
                    "partnerCode": "RoomOnly22",
                    "distributionModel": "HotelCollect",
                    "manageable": true,
                    "compensation": {
                        "percent": 0.1
                    }
                }
            ],
            "status": "Active",
            "type": "Standalone",
            "pricingModel": "PerDayPricing",
            "occupantsForBaseRate": 2,
            "taxInclusive": false,
            "cancelPolicy": {
                "defaultPenalties": [
                    {
                        "deadline": 0,
                        "perStayFee": "1stNightRoomAndTax",
                        "amount": 0
                    },
                    {
                        "deadline": 24,
                        "perStayFee": "None",
                        "amount": 0
                    }
                ]
            },
            "additionalGuestAmounts": [
                {
                    "dateStart": "2015-07-10",
                    "dateEnd": "2079-06-06",
                    "ageCategory": "Adult",
                    "amount": 0
                }
            ],
            "minLOSDefault": 1,
            "maxLOSDefault": 28,
            "minAdvBookDays": 0,
            "maxAdvBookDays": 500,
            "bookDateStart": "1900-01-01",
            "bookDateEnd": "2079-06-06",
            "travelDateStart": "1901-01-01",
            "travelDateEnd": "2079-06-06",
            "mobileOnly": false
        },
        {
            "resourceId": 205020302,
            "name": "RoomOnly11",
            "rateAcquisitionType": "SellLAR",
            "distributionRules": [
                {
                    "expediaId": "205020302A",
                    "partnerCode": "RoomOnly",
                    "distributionModel": "HotelCollect",
                    "manageable": true,
                    "compensation": {
                        "percent": 0.1
                    }
                }
            ],
            "status": "Active",
            "type": "Standalone",
            "pricingModel": "PerDayPricing",
            "occupantsForBaseRate": 2,
            "taxInclusive": false,
            "cancelPolicy": {
                "defaultPenalties": [
                    {
                        "deadline": 0,
                        "perStayFee": "1stNightRoomAndTax",
                        "amount": 0
                    },
                    {
                        "deadline": 24,
                        "perStayFee": "None",
                        "amount": 0
                    }
                ]
            },
            "additionalGuestAmounts": [
                {
                    "dateStart": "2015-07-10",
                    "dateEnd": "2079-06-06",
                    "ageCategory": "Adult",
                    "amount": 0
                }
            ],
            "minLOSDefault": 1,
            "maxLOSDefault": 28,
            "minAdvBookDays": 0,
            "maxAdvBookDays": 500,
            "bookDateStart": "1900-01-01",
            "bookDateEnd": "2079-06-06",
            "travelDateStart": "1901-01-01",
            "travelDateEnd": "2079-06-06",
            "mobileOnly": false
        },
        {
            "resourceId": 205833985,
            "name": "BB",
            "rateAcquisitionType": "SellLAR",
            "distributionRules": [
                {
                    "expediaId": "205833985",
                    "partnerCode": "RoomOnly",
                    "distributionModel": "ExpediaCollect",
                    "manageable": false,
                    "compensation": {
                        "percent": 0.23,
                        "minAmount": 0
                    }
                },
                {
                    "expediaId": "205833985A",
                    "partnerCode": "RoomOnly",
                    "distributionModel": "HotelCollect",
                    "manageable": true,
                    "compensation": {
                        "percent": 0.23
                    }
                }
            ],
            "status": "Active",
            "type": "Standalone",
            "pricingModel": "PerDayPricing",
            "occupantsForBaseRate": 2,
            "taxInclusive": false,
            "cancelPolicy": {
                "defaultPenalties": [
                    {
                        "deadline": 0,
                        "perStayFee": "1stNightRoomAndTax",
                        "amount": 0
                    },
                    {
                        "deadline": 24,
                        "perStayFee": "None",
                        "amount": 0
                    }
                ]
            },
            "additionalGuestAmounts": [
                {
                    "dateStart": "2015-04-08",
                    "dateEnd": "2079-06-06",
                    "ageCategory": "Adult",
                    "amount": 0
                }
            ],
            "minLOSDefault": 1,
            "maxLOSDefault": 28,
            "minAdvBookDays": 0,
            "maxAdvBookDays": 500,
            "bookDateStart": "1900-01-01",
            "bookDateEnd": "2079-06-06",
            "travelDateStart": "1901-01-01",
            "travelDateEnd": "2079-06-06",
            "mobileOnly": false
        }
    ]
}
```

### Single Rate Plan Read (PerDay Pricing, ExpediaCollect, Net Rate)
This example is for a Per Day Pricing, ExpediaCollect, Net Rate Rate Plan.

Request:
```HTTP
GET https://services.expediapartnercentral.com/products/v1/properties/1780044/roomTypes/200828484/ratePlans/204297188
Content-Type: application/json
Accept: application/json
Request-ID : 307af24f-f59a-11e4-822e-005056b1298f
Authorization: Basic [your encoded username:password in Base64]
```
Response:
```JSON
{
  "entity": {
    "resourceId": 204309700,
    "name": "Room Only",
    "distributionRules": [
      {
        "expediaId": "204309700",
        "partnerCode": "NK2",
        "distributionModel": "ExpediaCollect",
        "manageable": true,
        "compensation": {
          "percent": 0.26,
          "minAmount": 10.0,
          "exceptions": [
            {
              "dateStart": "2015-11-01",
              "dateEnd": "2015-11-02",
              "minAmount": 0,
              "percent": 0.24,
              "mon": true,
              "tue": true,
              "wed": true,
              "thu": true,
              "fri": true,
              "sat": true,
              "sun": true
            }
          ]
        }
      }
    ],
    "status": "Active",
    "type": "Standalone",
    "distributionModel": "ExpediaCollect",
    "rateAcquisitionType": "NetRate",
    "pricingModel": "PerDayPricing",
    "occupantsForBaseRate": 1,
    "taxInclusive": false,
    "cancelPolicy": {
      "defaultPenalties": [
        {
          "deadline": 0,
          "perStayFee": "None",
        "amount": 0.0
        }
      ]
    },
    "additionalGuestAmounts": [
      {
        "dateStart": "2014-12-04",
        "dateEnd": "2079-06-06",
        "ageCategory": "Adult",
        "amount": 36
      }, {
        "dateStart": "2014-12-04",
        "dateEnd": "2079-06-06",
        "ageCategory": "ChildAgeA",
        "amount": 29.7
      }, {
        "dateStart": "2014-12-04",
        "dateEnd": "2079-06-06",
        "ageCategory": "ChildAgeB",
        "amount": 29.7
      }
    ],
    "valueAddInclusions": [
      "Free Parking", "Free Breakfast", "Free Internet"
    ],
    "minLOSDefault": 1,
    "maxLOSDefault": 28,
    "minAdvBookDays": 0,
    "maxAdvBookDays": 500,
    "bookDateStart": "1900-01-01",
    "bookDateEnd": "2079-06-06",
    "travelDateStart": "1900-01-30",
    "travelDateEnd": "2079-06-06",
    "mobileOnly": false
  }
}
```

### Single Rate Plan Read (Occupancy-Based Pricing, ExpediaCollect, Sell Rate)
Request:
```HTTP
GET https://services.expediapartnercentral.com/products/v1/properties/1780044/roomTypes/200828484/ratePlans/204126855
Content-Type: application/json
Accept: application/json
Request-ID : 307af24f-f59a-11e4-822e-005056b1298f
Authorization: Basic [your encoded username:password in Base64]
```
Response:
```JSON
{
  "entity": {
    "resourceId": 204126855,
    "name": "Standard",
    "distributionRules": [
      {
        "expediaId": "204126855",
        "partnerCode": "Room Only",
        "distributionModel": "ExpediaCollect",
        "manageable": true,
        "compensation": {
          "percent": 0.25,
          "minAmount": 10.0
        }
      }
    ],
    "status": "Active",
    "type": "Standalone",
    "distributionModel": "ExpediaCollect",
    "rateAcquisitionType": "SellLAR",
    "pricingModel": "OccupancyBasedPricing",
    "taxInclusive": false,
    "cancelPolicy": {
      "defaultPenalties": [
        {
          "deadline": 0,
          "perStayFee": "50PercentCostOfStay",
         "amount": 0.0

        }, {
          "deadline": 72,
          "perStayFee": "None",
         "amount": 0.0

        }
      ]
    },
    "additionalGuestAmounts": [
      {
        "dateStart": "2014-10-23",
        "dateEnd": "2079-06-06",
        "ageCategory": "Adult",
        "amount": 30
      }, {
        "dateStart": "2014-10-23",
        "dateEnd": "2079-06-06",
        "ageCategory": "ChildAgeA",
        "amount": 30
      }
    ],
    "valueAddInclusions": [
      "Free Breakfast"
    ],
    "minLOSDefault": 2,
    "maxLOSDefault": 28,
    "minAdvBookDays": 23,
    "maxAdvBookDays": 290,
    "bookDateStart": "1900-01-01",
    "bookDateEnd": "2079-06-06",
    "travelDateStart": "1900-01-30",
    "travelDateEnd": "2079-06-06",
    "mobileOnly": false
  }
}
```

### Single Rate Plan Read (Occupancy-Based Pricing, HotelCollect)
Request:
```HTTP
GET https://services.expediapartnercentral.com/products/v1/properties/1780044/roomTypes/200828484/ratePlans/204321248
Content-Type: application/json
Accept: application/json
Request-ID : 307af24f-f59a-11e4-822e-005056b1298f
Authorization: Basic [your encoded username:password in Base64]
```
Response:
```JSON
{
  "entity": {
    "resourceId": 204321248,
    "name": "Vikings",
    "rateAcquisitionType": "SellLAR",
    "distributionRules": [
      {
        "expediaId": "204321248A",
        "partnerCode": "RoomOnly-1",
        "distributionModel": "HotelCollect",
        "manageable": true,
        "compensation": {
          "percent": 0.20,
          "exceptions": [
            {
              "dateStart": "2015-04-08",
              "dateEnd": "2015-12-31",
              "percent": 0.15,
              "mon": true,
              "tue": true,
              "wed": true,
              "thu": true,
              "fri": false,
              "sat": false,
              "sun": false
            }, {
              "dateStart": "2015-04-08",
              "dateEnd": "2015-12-31",
              "percent": 0.18,
              "mon": false,
              "tue": false,
              "wed": false,
              "thu": false,
              "fri": true,
              "sat": true,
              "sun": true
            }
          ]
        }
      }
    ],
    "status": "Active",
    "type": "Standalone",
    "pricingModel": "OccupancyBasedPricing",
    "taxInclusive": true,
    "cancelPolicy": {
      "defaultPenalties": [
        {
          "deadline": 0,
          "perStayFee": "50PercentCostOfStay",
         "amount": 0.0

        }
      ]
    },
    "valueAddInclusions": [
      "Free Parking", "Free Wireless Internet", "Breakfast Buffet"
    ],
    "minLOSDefault": 7,
    "maxLOSDefault": 14,
    "minAdvBookDays": 0,
    "maxAdvBookDays": 30,
    "bookDateStart": "2015-01-01",
    "bookDateEnd": "2016-06-06",
    "travelDateStart": "2015-01-02",
    "travelDateEnd": "2016-12-31",
    "mobileOnly": true
  }
}
```

### Single Rate Plan Read (Expedia Traveler Preference)
Request:
```HTTP
GET https://services.expediapartnercentral.com/products/v1/properties/1780044/roomTypes/200828484/ratePlans/204309700
Content-Type: application/json
Accept: application/json
Request-ID : 307af24f-f59a-11e4-822e-005056b1298f
Authorization: Basic [your encoded username:password in Base64]
```
Response:
```JSON
{
  "entity": {
    "resourceId": 204309700,
    "name": "Room Only",
    "rateAcquisitionType": "NetRate",
    "distributionRules": [
      {
        "expediaId": "204309700",
        "partnerCode": "NK2",
        "distributionModel": "ExpediaCollect",
        "manageable": true,
        "compensation": {
          "percent": 0.26,
          "minAmount": 10.0
        }
      }, {
        "expediaId": "204309700A",
        "partnerCode": "ANK2",
        "distributionModel": "HotelCollect",
        "manageable": false,
        "compensation": {
          "percent": 0.26,
          "minAmount": 10.0
        }
      }
    ],
    "status": "Active",
    "type": "Standalone",
    "pricingModel": "PerDayPricing",
    "occupantsForBaseRate": 1,
    "taxInclusive": false,
    "cancelPolicy": {
      "defaultPenalties": [
        {
          "deadline": 0,
          "perStayFee": "None",
         "amount": 0.0

        }
      ]
    },
    "additionalGuestAmounts": [
      {
        "dateStart": "2014-12-04",
        "dateEnd": "2079-06-06",
        "ageCategory": "Adult",
        "amount": 36
      }, {
        "dateStart": "2014-12-04",
        "dateEnd": "2079-06-06",
        "ageCategory": "ChildAgeA",
        "amount": 29.7
      }
    ],
    "valueAddInclusions": [
      "Free Breakfast"
    ],
    "minLOSDefault": 1,
    "maxLOSDefault": 28,
    "minAdvBookDays": 0,
    "maxAdvBookDays": 500,
    "bookDateStart": "1900-01-01",
    "bookDateEnd": "2079-06-06",
    "travelDateStart": "1900-01-30",
    "travelDateEnd": "2079-06-06",
    "mobileOnly": false
  }
}
```

### Rate Plan Create (Per-day-Pricing, ExpediaCollect)
```HTTP
POST https://services.expediapartnercentral.com/products/v1/properties/1780044/roomTypes/200835/ratePlans/
Content-Type: application/json
Accept: application/json
Content-Length: 984
Request-ID : 307af24f-f59a-11e4-822e-005056b1298f
Authorization: Basic [your encoded username:password in Base64]
```
```JSON
{
  "name": "My first test rate plan",
  "distributionRules": [
    {
      "partnerCode": "TEST1",
      "distributionModel": "ExpediaCollect"
    }
  ],
  "status": "Active",
  "type": "Standalone",
  "distributionModel": "ExpediaCollect",
  "occupantsForBaseRate": 2,
  "taxInclusive": false,
  "cancelPolicy": {
    "defaultPenalties": [
      {
        "deadline": 0,
        "perStayFee": "None",
       "amount": 0.0

      }
    ]
  },
  "additionalGuestAmounts": [
    {
      "ageCategory": "Adult",
      "amount": 40
    }, {
      "ageCategory": "ChildAgeA",
      "amount": 20
    }, {
      "ageCategory": "ChildAgeB",
      "amount": 10
    }
  ],
  "valueAddInclusions": [
    "Free Parking", "Free Breakfast", "Free Internet"
  ]
}
```

The response would look like:
```JSON
{
  "entity": {
    "resourceId": 204886798,
    "name": "My first test rate plan",
    "distributionRules": [
      {
        "expediaId": "204886798",
        "partnerCode": "TEST1",
        "distributionModel": "ExpediaCollect",
        "manageable": true,
        "compensation": {
          "percent": 0.26,
          "minAmount": 0.26,
          "exceptions": [
            {
              "from": "2015-11-01",
              "to": "2015-11-02",
              "minAmount": 0,
              "percent": 0.24,
              "mon": true,
              "tue": true,
              "wed": true,
              "thu": true,
              "fri": true,
              "sat": true,
              "sun": true
            }
          ]
        }
      }
    ],
    "status": "Active",
    "type": "Standalone",
    "distributionModel": "ExpediaCollect",
    "rateAcquisitionType": "NetRate",
    "pricingModel": "PerDayPricing",
    "occupantsForBaseRate": 2,
    "taxInclusive": false,
    "cancelPolicy": {
      "defaultPenalties": [
        {
          "deadline": 0,
          "perStayFee": "None",
        "amount": 0.0

        }
      ]
    },
    "additionalGuestAmounts": [
      {
        "dateStart": "2015-03-17",
        "dateEnd": "2079-06-06",
        "ageCategory": "ChildAgeB",
        "amount": 10
      }, {
        "dateStart": "2015-03-17",
        "dateEnd": "2079-06-06",
        "ageCategory": "ChildAgeA",
        "amount": 20
      }, {
        "dateStart": "2015-03-17",
        "dateEnd": "2079-06-06",
        "ageCategory": "Adult",
        "amount": 40
      }
    ],
    "valueAddInclusions": [
      "Free Parking", "Free Breakfast", "Free Internet"
    ],
    "minLOSDefault": 1,
    "maxLOSDefault": 28,
    "minAdvBookDays": 0,
    "maxAdvBookDays": 500,
    "bookDateStart": "2015-03-17",
    "bookDateEnd": "2079-06-06",
    "travelDateStart": "2015-03-17",
    "travelDateEnd": "2079-06-06",
    "mobileOnly": false
  }
}
```
### Rate Plan Create (Expedia Traveler Preference)
```HTTP
POST https://services.expediapartnercentral.com/products/v1/properties/1780044/roomTypes/200835/ratePlans/   HTTP/1.1
Content-Type: application/json
Accept: application/json
Content-Length: 984
Request-ID : 307af24f-f59a-11e4-822e-005056b1298f
Authorization: Basic [your encoded username:password in base64] 
```
```JSON
{
  "name": "Room Only",
  "distributionRules": [
    {
      "partnerCode": "NK2",
      "distributionModel": "ExpediaCollect"
    }, {
      "partnerCode": "ANK2",
      "distributionModel": "HotelCollect"
    }
  ],
  "occupantsForBaseRate": 1,
  "taxInclusive": false,
  "cancelPolicy": {
    "defaultPenalties": [
      {
        "deadline": 0,
        "perStayFee": "None",
       "amount": 0.0

      }
    ]
  },
  "additionalGuestAmounts": [
    {
      "dateStart": "2014-12-04",
      "dateEnd": "2079-06-06",
      "ageCategory": "Adult",
      "amount": 36
    }, {
      "dateStart": "2014-12-04",
      "dateEnd": "2079-06-06",
      "ageCategory": "ChildAgeA",
      "amount": 29.7
    }
  ],
  "valueAddInclusions": [
    "Free Breakfast"
  ]
}
```
Response would look like:
```JSON
{
  "entity": {
    "resourceId": 204309700,
    "name": "Room Only",
    "rateAcquisitionType": "NetRate",
    "distributionRules": [
      {
        "expediaId": "204309700",
        "partnerCode": "NK2",
        "distributionModel": "ExpediaCollect",
        "manageable": true,
        "compensation": {
          "percent": 0.26,
          "minAmount": 10.0
        }
      }, {
        "expediaId": "204309700A",
        "partnerCode": "ANK2",
        "distributionModel": "HotelCollect",
        "manageable": false,
        "compensation": {
          "percent": 0.26,
          "minAmount": 10.0
        }
      }
    ],
    "status": "Active",
    "type": "Standalone",
    "pricingModel": "PerDayPricing",
    "occupantsForBaseRate": 1,
    "taxInclusive": false,
    "cancelPolicy": {
      "defaultPenalties": [
        {
          "deadline": 0,
          "perStayFee": "None",
        "amount": 0.0

        }
      ]
    },
    "additionalGuestAmounts": [
      {
        "dateStart": "2014-12-04",
        "dateEnd": "2079-06-06",
        "ageCategory": "Adult",
        "amount": 36
      }, {
        "dateStart": "2014-12-04",
        "dateEnd": "2079-06-06",
        "ageCategory": "ChildAgeA",
        "amount": 29.7
      }
    ],
    "valueAddInclusions": [
      "Free Breakfast"
    ],
    "minLOSDefault": 1,
    "maxLOSDefault": 28,
    "minAdvBookDays": 0,
    "maxAdvBookDays": 500,
    "bookDateStart": "1900-01-01",
    "bookDateEnd": "2079-06-06",
    "travelDateStart": "1900-01-30",
    "travelDateEnd": "2079-06-06",
    "mobileOnly": false
  }
}
```
### Rate Plan Modify (Name, Additional Guest Amount, Value Adds)
In this example, the rate plan created in a previous example is modified to have a more meaningful name, lower additional guest amounts and free Internet.
```HTTP
PUT https://services.expediapartnercentral.com/products/v1/properties/1780044/roomTypes/200835/ratePlans/204309700 
Content-Type: application/json
Accept: application/json
Request-ID : 307af24f-f59a-11e4-822e-005056b1298f
Authorization: Basic [your encoded username:password in base64]
```
```JSON
{
  "resourceId": 204309700,
  "name": "More Meaningful Name",
  "rateAcquisitionType": "NetRate",
  "distributionRules": [
    {
      "expediaId": "204309700",
      "partnerCode": "NK2",
      "distributionModel": "ExpediaCollect",
      "manageable": true,
      "compensation": {
        "percent": 0.26,
        "minAmount": 10.0
      }
    }, {
      "expediaId": "204309700A",
      "partnerCode": "ANK2",
      "distributionModel": "HotelCollect",
      "manageable": false,
      "compensation": {
        "percent": 0.26,
        "minAmount": 10.0
      }
    }
  ],
  "status": "Active",
  "type": "Standalone",
  "pricingModel": "PerDayPricing",
  "occupantsForBaseRate": 2,
  "taxInclusive": false,
  "cancelPolicy": {
    "defaultPenalties": [
      {
        "deadline": 0,
        "perStayFee": "None",
       "amount": 0.0

      }
    ]
  },
  "additionalGuestAmounts": [
    {
      "dateStart": "2014-12-04",
      "dateEnd": "2079-06-06",
      "ageCategory": "Adult",
      "amount": 30
    }, {
      "dateStart": "2014-12-04",
      "dateEnd": "2079-06-06",
      "ageCategory": "ChildAgeA",
      "amount": 20
    }
  ],
  "valueAddInclusions": [
    "Free Breakfast", "Free Internet"
  ],
  "minLOSDefault": 1,
  "maxLOSDefault": 28,
  "minAdvBookDays": 0,
  "maxAdvBookDays": 500,
  "bookDateStart": "1900-01-01",
  "bookDateEnd": "2079-06-06",
  "travelDateStart": "1900-01-30",
  "travelDateEnd": "2079-06-06",
  "mobileOnly": false
}
```
Response would look like:
```JSON
{
  "entity": {
    "resourceId": 204309700,
    "name": "More Meaningful Name",
    "rateAcquisitionType": "NetRate",
    "distributionRules": [
      {
        "expediaId": "204309700",
        "partnerCode": "NK2",
        "distributionModel": "ExpediaCollect",
        "manageable": true,
        "compensation": {
          "percent": 0.26,
          "minAmount": 10.0
        }
      }, {
        "expediaId": "204309700A",
        "partnerCode": "ANK2",
        "distributionModel": "HotelCollect",
        "manageable": false,
        "compensation": {
          "percent": 0.26,
          "minAmount": 10.0
        }
      }
    ],
    "status": "Active",
    "type": "Standalone",
    "pricingModel": "PerDayPricing",
    "occupantsForBaseRate": 2,
    "taxInclusive": false,
    "cancelPolicy": {
      "defaultPenalties": [
        {
          "deadline": 0,
          "perStayFee": "None",
        "amount": 0.0

        }
      ]
    },
    "additionalGuestAmounts": [
      {
        "dateStart": "2014-12-04",
        "dateEnd": "2079-06-06",
        "ageCategory": "Adult",
        "amount": 30
      }, {
        "dateStart": "2014-12-04",
        "dateEnd": "2079-06-06",
        "ageCategory": "ChildAgeA",
        "amount": 20
      }
    ],
    "valueAddInclusions": [
      "Free Breakfast", "Free Internet"
    ],
    "minLOSDefault": 1,
    "maxLOSDefault": 28,
    "minAdvBookDays": 0,
    "maxAdvBookDays": 500,
    "bookDateStart": "1900-01-01",
    "bookDateEnd": "2079-06-06",
    "travelDateStart": "1900-01-30",
    "travelDateEnd": "2079-06-06",
    "mobileOnly": false
  }
}
```
### Rate Plan Delete
In this example, the rate plan created in a previous example is deleted.
```HTTP
DELETE https://services.expediapartnercentral.com/products/v1/properties/1780044/roomTypes/200835/ratePlans/204309700
Content-Type: application/json
Accept: application/json
Request-ID : 307af24f-f59a-11e4-822e-005056b1298f
Authorization: Basic [your encoded username:password in base64]
```

