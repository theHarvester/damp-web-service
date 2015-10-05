# L-Api

A specification for a well rounded web service.

## Overview

An l-api compliant web service provides a few key features that address the downsides of other alternatives such as REST, RPC and SOAP.

 * documentation built in
 * entities separate from actions
 * a deep and flexible way of getting only the data we need
 * minimal cruft

The structure of an l-api web service is made up of two distinct parts, entities and actions.

| Route                   | Method  | Description |
| ------                  | -----   | ----------- |
| /entities               | GET     | List the various entities available in the system |
| /entities/{type}        | GET     | Describe the entity in detail. This will include field names, related entities and other info. |
| /entities/{type}        | PUT     | Creates a new entity |
| /entities/{type}        | POST    | Queries the data starting at the entity type |
| /id/{id}                | GET     | This is the same as calling GET /entities/{type} |
| /id/{id}                | POST    | Query the required fields and related entities |
| /id/{id}                | PATCH   | Patch the data. If a field is not supplied it will not get updated |
| /id/{id}                | UPDATE  | Update the data. The entity will be updated to the data passed in |
| /id/{id}                | DELETE  | Deletes the entity |
| /actions/               | GET     | Lists the services available |
| /actions/{type}         | GET     | Gets the details of the actions available for the type |
| /actions/{type}/{action}| POST    | Performs the action |

## Entities

Entities are best compared to the RESTful web services you're used to. They should be thought of as models or data objects at the orm level. An entity always has an Id that is unique to the system and it can always describe itself.


#### Ids
The id of an entity must be unique across the system. In the following examples the convention {service_name}-{id} is used but this is just a suggestion

#### Self-description

At the very heart of entities is the ability to describe itself.
```
GET /entities/contacts
```
```
{
  "TODO": "Structure not complete",
  "fields": {
    "name": { "type":"string" },
    "email": { "type":"string" },
    "phone": { "type":"string" },
    "next_of_kin": {
      "type": "entity",
      "name": "contacts"
    }
  }
}
```

Now let's look at this request which will put the last request to use
```
POST /id/contact-12345
{
  "_fields_":["name", "email", "next_of_kin"],
  "next_of_kin":{
    "_fields_":["name", "phone"]
  }
}
```
```
{
  "id":"contact-12345",
  "name":"John Smith",
  "email":"john.smith@example.com",
  "next_of_kin":{
    "id":"contact-9876",
    "name":"Jane Smith",
    "email":"jane.smith@example.com"
  }
}
```
