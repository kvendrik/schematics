# schematics [WIP]
### A front-end friendly REST API standard experiment

![](http://data.whicdn.com/images/72152445/large.gif)

## What is it?
An API design standard to make it clear in one glimpse what an API can do and how to use it. The most important piece of this are the [Schema's](#schemas). Schema's are basically small JSON blobs you add to your reponse to help people see in one glimpse what your API can do and how they can use it.

## Schema's

#### What it looks like
Some examples:

`GET api.yourdomain.com`
The root contains the initial schema.
```json
{
    "$schema": {
        "users": "https://api.github.com/users{/username}{?limit,offset}",
        "emojis": "https://api.github.com/emojis",
        "events": {
            "post": {
                "href": "https://api.github.com/events",
                "params": {
                    "name": "String",
                    "date": { "type": "String", "optional": true }
                }
            },
            "get": "https://api.github.com/events"
        },
        "repos": "https://api.github.com/repos{/orgName}"
    }
}
```

`GET api.yourdomain.com/repos/babel`
When you request one of the endpoints in the initial schema the API tells you where you can go from there by
providing you with a new schema object.
```
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
    "$schema": {
        "issues": "https://api.github.com/repos/babel/{repoName}/issues{/id}",
        "tags": "https://api.github.com/repos/babel/{repoName}/tags"
    }
}
```

### Creating your own
Some things you might find useful to know:

* `GET`
    * A details object with a `href` is optional
    * Queries can be defined with just their key e.g. `{?offset&limit}`
    * Queries are always optional
    * Optional parameters can be defined using brackets e.g. `{/username}`
    * Required parameters can be defined using brackets e.g. `/{username}`
* `POST`, `PUT` & `DELETE`
    * These all require a details object
    * This should contain a `href` String and optionally a `params` object
    * Params
        * An object with the properties that should go into the request body
        * As key you specify the name of the parameter
        * As value you specify the data type the property should be
        * These properties are required by default
        * If you would like to make a property optional define a details object instead of the data type string e.g. `{ "type": "String", "optional": true }`
