# Elastifile Cloud File Service RESTful API

## Create and run a shared file system using Elastifile Cloud File Service on Google Cloud Platform

### Prerequisites
Setup Google Project for consuming a filesystem through Elastifile service API

Per Google cloud project you wish to use the Elastifile service API with, the following information is needed:
* Project id and project numeric id, both can be found on cloud console home page or by using:
* VPC network name
* Network project id and network project numeric id if using a service project attached to host project (shared vpc)


On the selected project run, replacing ${PROJECT} with your cloud console project id, ${NETWORK} with your VPC network name and ${NETWORK-PROJECT} with the VPC host project id (if using shared VPC otherwise use $PROJECT value for both):

* Enable Google Compute API

`gcloud --project=${PROJECT} services enable compute.googleapis.com`

* Enable Google ServiceNetworking API

`gcloud --project=${NETWORK_PROJECT} services enable servicenetworking.googleapis.com`

* Enable Elastifile service API

`gcloud --project=${PROJECT} services enable cloud-file-service-gcp.elastifile.com`

* Allocate addresses range for service

`gcloud --project=${NETWORK_PROJECT} beta compute addresses create <ADDRESSES_NAME> --global --purpose=VPC_PEERING --prefix-length=20 --network=${NETWORK}`

* Peer VPC network with the service

`gcloud --project=${NETWORK_PROJECT} beta services vpc-peerings connect --service=cloud-file-service-gcp.elastifile.com --ranges=<ADDRESSES_NAME> --network=${NETWORK}`

* Enable custom routes propagation over peered networks

`gcloud --project=${NETWORK_PROJECT} beta compute networks peerings update cloud-file-service-network --network=${NETWORK} --import-custom-routes --export-custom-routes` 

### API Reference V1

This API reference is organized by resource type. Each resource type has one or more data representations and one or more methods.

Base URL: https://cloud-file-service-gcp.elastifile.com/api/v1/

##### Resource Types:
projects -  Register, list and manage projects

service-class - List available service-classes (SKU’s)

regions - List available regions and zones

instances - Manage Elastifile instances
 
snapshots - Manage manual snapshots and snapshots shares

operations - Monitor long living operations

events - List and acknowledge service events

##### Path parameters

Used to identify consumer projects the API is been acting on and existing resource the method is been applied on

/projects/12345/service-class

/projects/12345/service-class/general-use

##### Query string parameters

Included at the end of URL and provides a way to pass standard optional parameters

#### OpenAPI Specification

Elastifile service API is using the OpenAPI specification, which is an API description format for REST APIs. An OpenAPI file allows to describe the entire API, including:

* Available endpoints (/project/<project>/service-class) and operations on each endpoint
* Operation parameters Input and output for each operation
* Authentication methods
* Contact information, license, terms of use and other information.

#### Browse API using Swagger UI
* Navigate to https://editor.swagger.io/
* Select File -> Import from URL
 
  OpenAPI: https://github.com/Elastifile/elastifileapis/blob/v1/efaas/openapi/openapi.json


### API Reference V2 (Alpha)

The API version 2 is currently available only to selected users. To access version 2 of the API please contact elastifile support.

Base URL: https://cloud-file-service-gcp.elastifile.com/api/v2/

OpenAPI: https://github.com/Elastifile/elastifileapis/blob/v1/efaas/openapi/openapi.v2.json


### API Authentication

* To authenticate a user, a client application must send a JSON Web Token (JWT) in the authorization header of the HTTP request to Elastifie Cloud File Service API service endpoint. 

* The service endpoint validates a JWT in a performant way by using the JWT's issuer's public keys. 

* The service endpoint caches the public keys for five minutes. In addition, service endpoint caches validated JWTs for five minutes or until JWT expiry, whichever happens first.

An authenticated call should look like this:
~~~~
curl -H "Authorization: Bearer ${TOKEN}" \
-H "accept: application/json" \
-H "Content-Type: application/json" \
"https://cloud-file-service-gcp.elastifile.com/api/v1/regions"
~~~~

Python example code to generate the JWT token:

https://github.com/Elastifile/elastifileapis/tree/v1/efaas/jwt
