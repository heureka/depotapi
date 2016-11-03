# DepotAPI
DepotAPI is tool for managing shop and shipper depots.

## Supported methods
- getShippers (public)
- getShipper (public)
- getShipperDepots (public)
- insertShipperDepots (shipper)
- deleteShipperDepot (shipper)
- getShopDepots (shop)
- insertShopDepots (shop)
- deleteShopDepot (shop)

## Authenticate
Authenticate is needed for all nonpublic methods. DepotAPI is used as an authenticate method based on HMAC. First you need to do is 
Heureka login and api key. Then send your username, password and api key to login request and you get back 4 hours valid
token. This token has to be sent in header for communication in secured methods. Last step of security is hash. All data 
which you send to Heureka must be extended about hash.

### Authenticate URL
https://api.heureka.cz/depot-api/v1/authenticate/login

### Request definition
    'depotApiKey' => 'string'
    'fermiId'     => 'string',
    'password'    => 'string',
    
### Hash formula
    hash_hmac('sha_256', serialized_array_data, your_api_key)
    
### Example
#### Request
    POST /depot-api/v1/authenticate/login HTTP/1.1
    Host: api.20.heureka.cz.dev.czech
    Content-Type: application/json
    
    {
    	"depotApiKey":"your_unique_api_key",
    	"fermiId":"example@heureka.cz",
    	"password":"your_password"
    }
    
#### Response
    {
        "code": 200,
        "message": "ok",
        "token": "random_generated_token"
    }

## GetShippers (public)
This method is used for getting a list of all shippers

### URL
    https://api.heureka.cz/depot-api/v1/delivery-places/getShippers

### Example
#### Request
    POST /depot-api/v1/delivery-places/getShippers HTTP/1.1
    Host: api.heureka.cz
    
#### Response
    {
      "code": 200,
      "message": "ok",
      "shippers": [
        [
            "shipperId": 1,
            "name": "HeurekaPoint",
            "availableDepots": 50
        ],
        [
            "shipperId": 2,
            "name": "Geis",
            "availableDepots": 20
        ]
      ]
    }

## GetShipper (public)
This method is used for obtaining detail information about specific shipper.

### URL
    https://api.heureka.cz/depot-api/v1/delivery-places/getShipper
### Request definition
    "shipperId" => "int"
### Example
#### Request
    POST /depot-api/v1/delivery-places/getShipper HTTP/1.1
    Host: api.heureka.cz
    Content-Type: application/json
    
    {
        "shipperId": 1
    }
#### Response
    {
      "code": 200,
      "message": "ok",
      "shipper": [
        "shipperId": 1,
        "name": "HeurekaPoint",
        "logo": "url_address_of_logo",
        "urlHomepage": "url_of_shipper",
        "streetHeadquarter": "street",
        "houseNumberHeadquarter": 666,
        "cityHeadquarter": "city",
        "zipCodeHeadquarter": 46000,
        "phone": "+420777777777",
        "streetPlaceOfBussiness": "street",
        "houseNumberPlaceOfBusiness": 666,
        "cityPlaceOfBussiness": "city",
        "zipCodePlaceOfBusiness": 46000,
        "identificationNumber": 02387727,
        "availableDepots": 50,
      ]
    }
    
## GetShipperDepots (public)
This method returns detail information about shipper depots. Maximum count of visited depots in one request is 100. 
Default range is 0 to 100.


### URL
    https://api.heureka.cz/depot-api/v1/delivery-places/getShipperDepots

### Request definition
    'shipperId' => 'int',
    'from'      => 'int|null',
    'to'        => 'int|null',

### Example
#### Request
    POST /depot-api/v1/delivery-places/getShipperDepots HTTP/1.1
    Host: api.heureka.cz
    Content-Type: application/json
    
    {
        "shipperId": 1,
        "from": 0,
        "to": 50
    }

#### Response
    {
      "code": 200,
      "message": "ok",
      "depots": [
        [
            "depotId": 1,
            "shipperId": 1,
            "street": "street_of_depot",
            "houseNumber": 666,
            "city": "city_of_depot",
            "zipCode": 46000,
            "gpsLat": 1.0000,
            "gpsLong": 1.0000,
            "phone": "+420777777777"
        ],
        [
            "depotId": 2,
            "shipperId": 1,
            "street": "street_of_depot",
            "houseNumber": 666,
            "city": "city_of_depot",
            "zipCode": 46000,
            "gpsLat": 1.0000,
            "gpsLong": 1.0000,
            "phone": "+420777777777"
        ],
      ]
    }
    
## InsertShipperDepots (shipper)
This method is used for inserting and updating shipperDepots. If depot with specific depotId exists, then edit the existing 
depot.

### URL
    https://api.heureka.cz/depot-api/v1/delivery-places/insertShipperDepots
 
### Request definition
    'depots' => [
        [
            'depotId'     => 'int',
            'shipperId'   => 'int',
            'street'      => 'string',
            'houseNumber' => 'string',
            'city'        => 'string',
            'zipCode'     => 'string',
            'gpsLat'      => 'float',
            'gpsLong'     => 'float',
            'phone'       => 'string',
        ],
    ],
    'hash'   => 'string',

### Example
#### Request
    POST /depot-api/v1/delivery-places/insertShipperDepots HTTP/1.1
    Host: api.heureka.cz
    Content-Type: application/json
    token: N005NlspOSoOxNX73XUxuhs-t_U
    
    {
    	"depots": [
            {
                "depotId": 1,
                "shipperId": 1,
                "street": "street_of_depot",
                "houseNumber": 666,
                "city": "city_of_depot",
                "zipCode": 46000,
                "gpsLat": 1.0000,
                "gpsLong": 1.0000,
                "phone": "+420777777777"
            },
            {
                "depotId": 2,
                "shipperId": 1,
                "street": "street_of_depot",
                "houseNumber": 666,
                "city": "city_of_depot",
                "zipCode": 46000,
                "gpsLat": 1.0000,
                "gpsLong": 1.0000,
                "phone": "+420777777777"
            }
         ],
        "hash": "your_generated_hash"
    }

#### Response
    "code": 200,
    "message": "ok",
    "description": "Added or edited 2 depots."
    
## DeleteShipperDepot (shipper)
This method is used for deleting existing shipper depots based on his id.
 
### URL
    https://api.heureka.cz/depot-api/v1/delivery-places/deleteShipperDepot

### Request definition
    'depotId'   => 'int',
    'shipperId' => 'int',
    'hash'      => 'string',
    
### Example
#### Request
    POST /depot-api/v1/delivery-places/deleteShipperDepot HTTP/1.1
    Host: api.heureka.cz
    Content-Type: application/json
    token: N005NlspOSoOxNX73XUxuhs-t_U
    
    {
    	"depotId": 1,
    	"shipperId": 1,
        "hash": "your_generated_hash"
    }

#### Response
    "code": 200,
    "message": "ok",
    "description": "Depot was successfully deleted"

## GetShopDepots (shop)
This method is used for obtaining a list of all your depots

### URL
    https://api.heureka.cz/depot-api/v1/delivery-places/getShopDepots

### Request definition
    'shopId' => 'int',
    'hash'   => 'string',

### Example

#### Request
    POST /depot-api/v1/delivery-places/getShopDepots HTTP/1.1
    Host: api.20.heureka.cz.dev.czech
    Content-Type: application/json
    token: N005NlspOSoOxNX73XUxuhs-t_U
    
    {
    	"shopId": 1,
        "hash": "your_generated_hash"
    }

#### Response
    "code": 200,
    "message": "ok",
    "depots": [
        {
            "depotId": 1,
            "shopId": 1,
            "name": "depot_name",
            "street": "street_of_depot",
            "houseNumber": 666,
            "city": "city_of_depot",
            "zipCode": 46000,
            "gpsLat": 1.0000,
            "gpsLong": 1.0000,
            "phone": "+420777777777"
        },
        {
            "depotId": 1,
            "shopId": 1,
            "name": "depot_name",
            "street": "street_of_depot",
            "houseNumber": 666,
            "city": "city_of_depot",
            "zipCode": 46000,
            "gpsLat": 1.0000,
            "gpsLong": 1.0000,
            "phone": "+420777777777"
        }
    ]

## InsertShopDepots (shop)
This method is used for inserting new or updating existing shop depots. If you want to insert new depot, set depotId value as 
null. 

### URL
    https://api.heureka.cz/depot-api/v1/delivery-places/insertShopDepots

### Request definition
    'depots' => [
        [
            'depotId'     => 'int|null',
            'shopId'      => 'int',
            'name'        => 'string',
            'street'      => 'string',
            'houseNumber' => 'string',
            'city'        => 'string',
            'zipCode'     => 'string',
            'gpsLat'      => 'float',
            'gpsLong'     => 'float',
            'phone'       => 'string',
        ],
    ],
    'hash'   => 'string',

### Example

#### Request
    POST /depot-api/v1/delivery-places/InsertShopDepots HTTP/1.1
    Host: api.20.heureka.cz.dev.czech
    Content-Type: application/json
    token: N005NlspOSoOxNX73XUxuhs-t_U
    
    {
        "depots": [
            {
                "depotId": 1,
                "shopId": 1,
                "name": "depot_name",
                "street": "street_of_depot",
                "houseNumber": "666",
                "city": "city_of_depot",
                "zipCode": "46000",
                "gpsLat": 1.0000,
                "gpsLong": 1.0000,
                "phone": "+420773595388"
            }, 
            {
                "depotId": null,
                "shopId": 1,
                "name": "depot_name",
                "street": "street_of_depot",
                "houseNumber": "666",
                "city": "city_of_depot",
                "zipCode": "46000",
                "gpsLat": 1.0000,
                "gpsLong": 1.0000,
                "phone": "+420773595388"
            }
        ],
        "hash": "your_generated_hash"
    }
    
#### Response
    "code": 200,
    "message": "ok",
    "depots": [
        "updated": [
            1
        ],
        "unchanged": [],
        "inserted": [
            {
                "depotId": 2,
                "shopId": 1,
                "name": "depot_name",
                "street": "street_of_depot",
                "houseNumber": "666",
                "city": "city_of_depot",
                "zipCode": "46000",
                "gpsLat": 1.0000,
                "gpsLong": 1.0000,
                "phone": "+420773595388"
            }
        ],
        "notInserted": [],
    ]

## DeleteShopDepots (shop)
This function is used for deleting depots based on their ids

### URL
    https://api.heureka.cz/depot-api/v1/delivery-places/deleteShopDepots

### Request definition
    'depots' => [
        [
            'depotId' => 'int',
            'shopId'  => 'int',
        ],
    ],
    'hash'   => 'string',
    
### Example

#### Request
    POST /depot-api/v1/delivery-places/deleteShopDepots HTTP/1.1
    Host: api.20.heureka.cz.dev.czech
    Content-Type: application/json
    token: N005NlspOSoOxNX73XUxuhs-t_U
    
    {
    "depots": [
        {
            "depotId": 1,
            "shopId": 1
        }, 
        {
            "depotId": 2,
            "shopId": 1
        }
    ],
    "hash": "your_generated_hash"
    }    

#### Response
    "code": 200,
    "message": "ok",

    
    




