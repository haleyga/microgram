# microgram
###### (in development)

## Purpose
This protocol is a small step towards defining a simple language that machines can understand and use to communicte with one another.

Standardizing a simple communications language will allow a microservice ecosystem to better automate itself.  Any service
can monitor any other service on the network, perhaps by allowing auto-registration of users/services or network discovery of other services that 
may provide needed functionality.

## About
Generally speaking, a uGram (`microgram`) compatible service is one that implements a small, strictly-scoped set of functionality (à la a microservice).
Additionally, it must conform to the uGram protocol by providing key endpoints:

* `GET <service root>/ping`: to quickly ascertain if the service is alive
* `GET <service root>/health`: to get a more complete picture of the service's health
* `GET <service root>/meta`: to get a better understanding of what the service offers, dependencies, etc.

#### Response format
`microgram` services should respond with JSON conforming to the following format:

```json
{
    "errors": [],
    "message": {}
}
```
`errors` should provide a list of any errors relevant to a user's request

`message` should contain the requested data

### /ping
This endpoint provides a quick and cheap way to ensure that the service is actually running and responding.

###### Sample response
```json
{
    "errors": [],
    "message": "pong"
}
```
or perhaps
```json
{
    "errors": [],
    "message": {
        "data": "pong"
    }
}
```

### /health
This endpoint should provide more detail about the health of the service.

###### Sample response
```json
{
    "errors": [],
    "message": {
        "peers": {
            "facebook": "intermittent",
            "twitter": "alive",
            "google": "dead"
        },
        "traffic": {
            "load": "high"
        }
    }
}
```
or perhaps
```json
{
    "errors": ["dead-connection:google"],
    "message": {
        "peers": {
            "facebook": "intermittent",
            "twitter": "alive",
        },
        "traffic": {
            "load": "high"
        }
    }
}
```

### /meta
This endpoint should provide any (safe) information that may be useful to a user of the service.

###### Sample response
```json
{
    "errors": [],
    "message": {
        "dependencies": {
            "api": ["facebook", "twitter", "google"]
        },
        "publicEndpoints": [
            "GET /",
            "GET /ping",
            "GET /healt",
            "GET /meta",
            "GET /user",
            "POST /user/new",
            "POST /register"
        ],
        "actions": {
            "facebook": ["user management"],
            "google": ["view contacts"],
            "twitter": ["post tweets"],
            "local": ["view users", "create user"]
        },
        "authentication": ["oauth", "oauth2", "username/password"],
        "discovery": {
            "registration": "POST /register",
            "listing": "www.myservicelisting.com"
        }
    }
}
```
The exact information provided is not relevant to this discussion.  This is just to demonstrate that the `/meta` endpoint is for providing the service user with additional information about the service.
Be smart, don't expose anything sensitive in a public endpoint!
