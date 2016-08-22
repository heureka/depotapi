# DepotAPI
##Authenticate
-https://api.heureka.cz/depot-api/v1/authenticate/login

Content-type: application/json
###Body: 
{
"depotApiKey" => string,
"fermiId" => string,
"password" => string
}

###Return
{
"token" => string (with 4 hours expiration)
}

Token must be added to header for the next communication with private api functions like:
header: 
{
  Content-Type: application/json,
  token: "xxxxx",
}

##GetShipper (public)
- https://api.heureka.cz/depot-api/v1/delivery-places/GetShipper
- function for get shipper with specific id

Content-type: application/json

###Body:
{
  "shipperId" => int
}

###return

####OK
{
  "code" => 200,
  "message" => "OK",
  "shipper": {
    "shipperId" => int,
    "name" => string,
    "logoUrl" => string ,
    "urlHomepage" => string ,
    "headquarter" => string,
    "phone" => string,
    "placeOfBusiness" => string,
    "identificationNumber" => string,
    "availableDepots" => int
  }
}

####NONEXIST Shipper
{
  "code" => 404,
}




