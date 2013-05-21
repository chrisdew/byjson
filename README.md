# byjson -- buffered Stream for reading Javascript objects from lines of JSON

`byjson` is a super-simple module providing a `JsonStream` for [node.js](http://nodejs.org/).

- supports `pipe`
- supports both UNIX and Windows line endings
- can wrap any readable stream
- can be used as a readable-writable "through-stream"
- super-simple: `stream = byjson(stream);`

## Install

    npm install byjson

or from source:

    git clone git://github.com/chrisdew/byjson.git
	cd byjson
	npm link

#Convenience API

The `byjson` module can be used as a function to quickly wrap a readable stream:

```javascript
var fs = require('fs'),
    byjson = require('byjson');

var stream = byjson(fs.createReadStream('sample.txt'));
```

The `data` event then emits Javascript objects:

```javascript
stream.on('data', function(ob) {
  console.log(ob);
});
```

#Standard API
    
You just need to add one line to wrap your readable `Stream` with a `JsonStream`.

```javascript
var fs = require('fs'),	
    byjson = require('byjson');

var stream = fs.createReadStream('sample.json');
stream = byjson.createStream(stream);

stream.on('data', function(ob) {
  console.log(ob);
});
```

#Piping

`byjson` supports `pipe` (though it strips the line endings, of course, and emits Javascript objects). When piping into a stream, the `pause()` and `resume()` 
methods are supported by the `JsonStream`, and pass on the call to the original stream.

```javascript
var stream = fs.createReadStream('sample.json');
stream = byline.createJsonStream(stream);
stream.pipe(fs.createWriteStream('nolines.json'));
```

#Simple
Unlike other modules (of which there are many), `byjson` contains no:

- monkeypatching
- dependencies
- non-standard 'line' events which break `pipe`
- limitations to only file streams
- CoffeeScript
- mostly unnecessary code
