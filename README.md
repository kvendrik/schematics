# schematics [WIP]
### An API standard for unicorns

![](http://data.whicdn.com/images/72152445/large.gif)

## What is it?
An API design standard to make it clear in one glimpse what an API can do.

## Schema's

#### What it looks like
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
        "repos": "http://kvendrik.github.io/schematicsjs/examples/schema-repos.json"
    }
}
```

#### Syntax
Some things you might find useful to know:

* `GET`
    * A details object with a `href` is optional
    * Queries can be defined with just their key e.g. `{?offset&limit}`
    * Queries are always optional
    * Optional parameters can be defined using brackets e.g. `{/username}`
    * Required parameters can be defined using brackets e.g. `/{username}`
* `POST`, `PUT` & `DELETE`
    * These all require a details object
    * This should contain a `href` String and a `params` Object
    * Params
        * An object with the properties that should go into the request body
        * As value you specify the data type the property should be
        * Currently only JavaScript data types are supported, these include: String, Number, Date, Array, Object and Boolean
        * These properties are required by default
        * If you would like to make a property optional define a details Object instead of the data type String e.g. `{ "type": "String", "optional": true }`
