# schematics [WIP]
### A front-end friendly REST API standard experiment

![](http://data.whicdn.com/images/72152445/large.gif)

## What is it?
An API design standard to make it clear in one glimpse what an API can do and how to use it. The most important piece of this are the [Schema's](#schemas). Schema's are basically small JSON blobs you add to your reponse to help people see in one glimpse what your API can do and how they can use it.

## Schema's

#### What it looks like
Some examples:

`GET https://api.yourdomain.com`<br>
The root contains the initial schema.
```json
{
    "$schematics": {
        "users": "https://api.github.com/users{/username}{?limit,offset}",
        "emojis": "https://api.github.com/emojis",
        "events": {
            "href": "https://api.github.com/events{?limit,offset}",
            "post": {
                "type": "object",
                "properties": {
                    "id": {
                        "type": "number",
                        "required": true
                    },
                    "date": {
                        "type": "string",
                        "pattern": "\\d{4}-\\d{2}-\\d{2}T\\d{2}:\\d{2}:\\d{2}(.[^Z]+)?Z"
                    }
                }
            },
            "get": null
        },
        "repos": "https://api.github.com/repos{/orgName}"
    }
}
```

`GET https://api.yourdomain.com/repos/babel`<br>
When you request one of the endpoints in the initial schema the API tells you where you can go from there by
providing you with a new schema object.
```json
{
    "data": [
        {
            "id": 24560307,
            "name": "babel",
            "fullName": "babel/babel",
            "private": false,
            "htmlUrl": "https://github.com/babel/babel",
            "description": "Babel is a compiler for writing next generation JavaScript."
        }
    ],
    "$schematics": {
        "issues": "https://api.github.com/repos/babel/{repoName}/issues{/id}",
        "tags": "https://api.github.com/repos/babel/{repoName}/tags"
    }
}
```

### Creating your own
Some things you might find useful to know:

* URLs
    * URL templates are [`RFC6570`](https://help.apiary.io/api_101/uri-templates/)
    * Queries can be defined with just their key e.g. `{?offset&limit}`
    * Queries are always optional
    * Optional parameters can be defined using brackets e.g. `{/username}`
    * Required parameters can be defined using brackets e.g. `/{username}`
* Collections
    * When only a string is specified the request type is `GET`
    * Can have a collection object with different request types
        * `POST`, `PUT` & `DELETE`
            * These optionally have an object with the properties that should/can go into the request body
            * This object is a [JSON schema](http://json-schema.org/)
