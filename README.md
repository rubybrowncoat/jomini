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
var parser = new jomini.Parser();
var streamer; /* Get a stream */
streamer.pipe(parser);
parser.on('finish', function() {
    // Access object with parser.obj
    // Ex. parser.obj.savegame_version.first    
});
```

and the result is an object with following representation

```js
{
    date: new Date(1640, 6, 1),
    player: "FRA",
    savegame_version: {
        first: 1,
        second: 9,
        third: 2,
        forth: 0
    }
}
```

## EU4 Example

Save games in EU4 have a header of `EU4txt`, which is generally discarded and
only used to validate that what is being read is an EU4 save game. In this
case, pipe the initial stream to the `Header`, which will discard the header
before passing the actual data to the parser.

```js
var jomini = require('jomini');

var header = new jomini.Header('EU4txt');
var parser = new jomini.Parser();

// Acquire data
var data = '';
header.pipe(parser);
parser.on('finish', function() {
    // Access object with parser.obj
});

header.write(data, 'utf8', function() {
    header.end();    
});
```

## Etymology

[Antoine-Henri Jomini][Jomini] is related to [Carl von Clausewitz][Clausewitz]
just like how Jomini the parser is related to Clausewitz the game engine

[Clausewitz engine]: http://en.wikipedia.org/wiki/Paradox_Development_Studio#Clausewitz_Engine
[Clausewitz]: http://en.wikipedia.org/wiki/Carl_von_Clausewitz
[Jomini]: http://en.wikipedia.org/wiki/Antoine-Henri_Jomini
