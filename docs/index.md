# MAD Protocol Specification

A specification for a well rounded web service.

MAD Protocol stands for Model Action Documentation Protocol.

## Overview

An MAD compliant web service provides a few key features that address the downsides of other alternatives such as REST, RPC and SOAP.

 * documentation built in
 * data separate from actions
 * a deep and flexible way of getting only the data we need
 * minimal cruft but still self contained

The structure of an MAD web service is made up of three distinct parts which are models, entities and actions. All of which have have documentation built in.

## Models

The model is has two key responsibilities.

1. Entity creation
2. Entity querying

### Model Routes

| Route                   | Method  | Description |
| ------                  | -----   | ----------- |
| /models               | GET     | List the various models available in the system |
| /models/{type}        | GET     | Describe the model in detail. This will include field names, related models and other info. |
| /models/{type}        | PUT     | Creates a new entity from a model |
| /models/{type}        | POST    | Queries the data starting at the entity type |

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
      "type": "model",
      "name": "contacts"
    }
  }
}
```

## Entities

Where the model is the factory that finds and creates entities, the entity is simply an instance of that model.

Each of the entities in a MAD web service must have a globally unique Id in the system. With this we have the ability to reference any entity with a single string.

The entity is responsible for the following actions

1. Read - TODO
2. Update - TODO
3. Patch - TODO
4. Delete - TODO
5. Describe - TODO

### Entity Routes

| Route                   | Method  | Description |
| ------                  | -----   | ----------- |
| /entity/{id}                | GET     | This is the same as calling GET /models/{type} |
| /entity/{id}                | POST    | Query the required fields and related entities |
| /entity/{id}                | PATCH   | Patch the data. If a field is not supplied it will not get updated |
| /entity/{id}                | UPDATE  | Update the data. The entity will be updated to the data passed in |
| /entity/{id}                | DELETE  | Deletes the entity |

#### Ids
The Id of an entity must be unique across the system. In the following examples the convention {service_name}-{id} is used but this is just a suggestion.


```
POST /entity/contact-12345
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

## Actions

### Action Routes

| Route                   | Method  | Description |
| ------                  | -----   | ----------- |
| /actions/               | GET     | Lists the services available |
| /actions/{type}         | GET     | Gets the details of the actions available for the type |
| /actions/{type}/{action}| POST    | Performs the action |
