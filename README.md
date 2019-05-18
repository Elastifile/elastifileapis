# Elastifile Cloud File Service RESTful API

###### Create and run a shared file system using Elastifile Cloud File Service on Google Cloud Platform

## Table of Content
* [Prerequisites](README.md#pre)
* [API Reference V1](README.md#v1)
* [API Reference V2](README.md#v2)
* [Authentication](README.md#auth)

## [Prerequisites](#pre)
#### Setup Google Project
Setup Google Project for consuming a filesystem through Elastifile service API

For each Google cloud project you wish to use with the Elastifile service API, the following information is needed:
* Project ID and Project Number. Both can be found on GCP console Home page.
* VPC network Name
* Network project ID and network project number if using a service project attached to host project (shared vpc)

#### Enable Elastifile API
On the selected project, run the following commands while replacing ${PROJECT} with your GCP console project ID, ${NETWORK} with your VPC network Name and ${NETWORK-PROJECT} with the VPC host project ID (if using shared VPC, otherwise use $PROJECT value for both):

* Enable Google Compute API

&nbsp;&nbsp;`gcloud --project=${PROJECT} services enable compute.googleapis.com`

* Enable Google Service Networking API

&nbsp;&nbsp;`gcloud --project=${NETWORK_PROJECT} services enable servicenetworking.googleapis.com`

* Enable Elastifile Service API

&nbsp;&nbsp;`gcloud --project=${PROJECT} services enable cloud-file-service-gcp.elastifile.com`

* Allocate addresses range for service

&nbsp;&nbsp;`gcloud --project=${NETWORK_PROJECT} beta compute addresses create <ADDRESSES_NAME> --global --purpose=VPC_PEERING --prefix-length=20 --network=${NETWORK}`

* Peer VPC network with the service

&nbsp;&nbsp;`gcloud --project=${NETWORK_PROJECT} beta services vpc-peerings connect --service=cloud-file-service-gcp.elastifile.com --ranges=<ADDRESSES_NAME> --network=${NETWORK}`

* Enable custom routes propagation over peered networks

&nbsp;&nbsp;`gcloud --project=${NETWORK_PROJECT} beta compute networks peerings update cloud-file-service-network --network=${NETWORK} --import-custom-routes --export-custom-routes` 

## [API Reference V1](#v1)

This API reference is organized by resource type. Each resource type has one or more data representations and one or more methods.

Base URL: https://cloud-file-service-gcp.elastifile.com/api/v1/

##### Resource Types:
Resource|Description
--------|-----------
projects |  Register, list and manage projects
service-class | List available service-classes (SKUs)
regions | List available regions and zones
instances | Manage Elastifile instances
snapshots | Manage manual snapshots and snapshots shares
operations | Monitor long living operations
events | List and acknowledge service events

##### Path parameters

Used to identify consumer projects the API is acting on and existing resources on which the method is applied.

&nbsp;&nbsp;`/projects/12345/service-class`

&nbsp;&nbsp;`/projects/12345/service-class/general-use`

##### Query string parameters

Included at the end of URL and provides a way to pass standard optional parameters.

#### OpenAPI Specification

The Elastifile Service API uses the OpenAPI specification, which is an API description format for REST APIs. An OpenAPI file describes the entire API, including:

* Available endpoints (/project/<project>/service-class) and operations on each endpoint.
* Operation parameters (input and output for each operation).
* Authentication methods.
* Contact information, license, terms of use and other information.

#### Browse API using Swagger UI
* Navigate to https://editor.swagger.io/
* Select File -> Import from URL
 
  OpenAPI: [/efaas/openapi/openapi.json](https://github.com/Elastifile/elastifileapis/blob/v1/efaas/openapi/openapi.json)
  
  Base URL: [https://cloud-file-service-gcp.elastifile.com/api/v1/](https://cloud-file-service-gcp.elastifile.com/api/v1/)

### [API Reference V2 (Alpha)](#v2)

API Version 2 is currently available only to selected users. To access V2 of the API, please contact Elastifile support.

OpenAPI: [/efaas/openapi/openapi.v2.json](https://github.com/Elastifile/elastifileapis/blob/master/efaas/openapi/openapi.v2.json)

Base URL: [https://cloud-file-service-gcp.elastifile.com/api/v2/](https://cloud-file-service-gcp.elastifile.com/api/v2/)

## [API Authentication](#auth)

* To authenticate a user, a client application must send a JSON Web Token (JWT) in the authorization header of the HTTP request to Elastifie Cloud File Service API service endpoint. 

* The service endpoint validates the JWT in a performant way by using the JWT issuer's public keys. 

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
