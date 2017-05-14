# http-switch
  Simple switch for http requests

```shell
npm install --save http-switch
```
Basically Http-Switch is another http handler, but it allows you to create your own server and handles only the 'request'-event. This makes it possible to use either `http`, `https`, `spdy` or any other `http`-compatible server.

## Usage

```js
const http = require('http');
const HTTPSwitch = require('http-switch');

let server = http.createServer();
let httpSwitch = new HTTPSwitch(server);

httpSwitch.for('/', (request, response) => //Handles requests matching string '/'
{
	/* Serve index.html */
});

httpSwitch.for(/^\/foo/, (request, response) => //Handles requests matching regex ^/foo
						//which includes every string starting with /foo
						//even '/foo/bar' and '/foobar'
{
	/* Serve foo.html */
});

let handler404 =
{
	handle: function(request, response)
	{
		/* Serve 404.html */
	}
};

httpSwitch.for(/^/, handler404); //Handles every request

server.listen(80);
```

Handlers are processed sequentially, so handler 3 will be called only if handler 1 and 2 don't match

### Examples
- `/`  
Handler 1
- `/foo`  
Handler 2
- `/foo/bar/abc`  
Handler 2
- `/foobar/abc`  
Handler 2
- `/barfoo`  
Handler 3
- `/abc`  
Handler 3

## API

### `new HTTPSwitch(server)`

#### arguments

- `server: object`  
Any object compatible to http.Server

#### returns

- `HTTPSwitch: object`

### `HTTPSwitch.for(path, onRequest)`

#### arguments

- `path: string or RegExp`
- `onRequest: function(request, response)`

#### returns

- `this`

### `HTTPSwitch.addHandler(path, onRequest)`
Same as `HTTPSwitch.for`

### `HTTPSwitch.switchRequest(request, response)`

#### arguments

- `request: http.IncommingMessage`
- `response: http.ServerResponse`