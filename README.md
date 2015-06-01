# Jomini [![travis][travis-image]][travis-url]

[travis-image]: https://img.shields.io/travis/nickbabcock/jomini.svg?style=flat
[travis-url]: https://travis-ci.org/nickbabcock/jomini

Jomini is a javascript library that is able to parse data files generated by
the [Clausewitz engine][] into an object. This can be best explained by an
example.

Given the following:

```
date=1640.7.1
player="FRA"
savegame_version=
{
    first=1
    second=9
    third=2
    forth=0
}
```

Parse it with the following

```js
var jomini = require('jomini');
var str = /* get your data */;
jomini.parse(str);
```

and the result is an object with following representation

```js
{
    date: new Date(Date.UTC(1640, 6, 1)),
    player: "FRA",
    savegame_version: {
        first: 1,
        second: 9,
        third: 2,
        forth: 0
    }
}
```

## Install

```
npm install --save jomini
```

## `toArray`

Given

```
army=
{
    name="1st army"
    unit={
        name="1st unit"
    }
}
army=
{
    name="2nd army"
    unit={
        name="1st unit"
    }
    unit={
        name="2nd unit"
    }
}
```

The resulting parsed structure will be:

```js
army: [{
    name: "1st army",
    unit: {
        name: "1st unit"
    }
}, {
    name: "2nd army",
    unit: [{
        name: "1st unit"
    }, {
        name: "2nd unit"
    }]
}]
```

Notice that the first army has an object for `unit` whereas the second army
has an array of `unit`. This is where domain knowledge of the document being
parsed is useful. When you're navigating the object structure you have two
options for successful access. Whenever you are accessing an object that could
be an array, do a type check and branch to the appropriate action, or use
`toArray` to make sure that the property on all objects defined by a path
are arrays. The solution to the example would be `jemini.toArray(obj,
'army.unit')`.

## Etymology

[Antoine-Henri Jomini][Jomini] is related to [Carl von Clausewitz][Clausewitz]
just like how Jomini the parser is related to Clausewitz the game engine

## Changelog

`v0.2.1 June 1st 2015`

Remove extraneous packaged files that (#1)

`v0.2.0 Feb 7th 2015`

A near complete rewrite with [Jison](http://zaach.github.io/jison/) that
ditches the handwritten stream based approach taken previously for an all in
one parser generator. While I believe this sacrifices performance, it makes up
with it in accuracy. I couldn't test the two implementations because parser
generator one was the only one that could parse all the files given.

`v0.1.3 Feb 2nd 2015`

- Added `toDate` to the API
- Implemented parsing of dates that contain hour information

`v0.1.2 Feb 1st 2015`

- Implemented simplified `parse` API
- Implemented `toArray` to force properties to be an array
- Dates are now stored as dates in object and not ISO 8601 strings

`v0.1.1 Jan 31st 2015`

- Removed dependency on lodash

[Clausewitz engine]: http://en.wikipedia.org/wiki/Paradox_Development_Studio#Clausewitz_Engine
[Clausewitz]: http://en.wikipedia.org/wiki/Carl_von_Clausewitz
[Jomini]: http://en.wikipedia.org/wiki/Antoine-Henri_Jomini
