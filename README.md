currency-codes
==============

JSON list of currency codes, as defined by ISO 4217.

## Testing

This file is pretty much the lifeblood of the API I'm working on. At the time of this writing, it will be used to validate currencies when queried by the DuckDuckGo's Answers module I'm building. You should be able to guess how important the file's quality is.

To enforce said quality, the file has to be deemed valid by a Schema tested using the [jassi.js test suite](https://github.com/iclanzan/jassi). It needs to be more than proper JSON. Conforming to the spec ensures the API won't break when it finds a weird thingie.

## What's in a currency?

Apparently, money. Other than that, each unit follows the following structure:

```
"currency code": {
  "name": "name for that currency",
  "code": "the currency code (again)",
  "country": "which country or countries the currency is available in; empty in the case of materials",
  "num": "the currency number (more on that later)",
  "e": "the number of decimal points necessary to show it correctly"
}
```

Nothing special, as you can see. This structure is actually imposed by ISO 4217, which we are following here. Now, a big part of this is making sure each of these things is correct. I could say that "EUR" is the currency code for Mars' "EUphoRia" currency, which would be a lie [citation needed]. Right now, I can't really make sure a currency is correct, other than manual verification against the list of active currencies.

## The schema

So, how does the file pass validation? It needs to conform to the schema, that is as follows:

```
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "title": "ISO 4217 Currency Codes",
    "description": "A currency following the convention defined by ISO 4217.",
    "type": "object",
    "properties": {
        "name": { "type": "string" },
        "code": { "type": "string" },
        "country": {
            "type": "array",
            "items": {
                "type": "string"
            },
            "minItems": 1,
            "uniqueItems": true
        },
        "num":{ "type": "string" },
        "e": {
          "description": "The number of decimal points the currency uses to represent values correctly.",
          "type": "integer",
          "minimum": 0
        }
    },
    "required": ["name", "code"]
}
```
