# ZephTTP2
ZephTTP2 is an interface built on top of `node:http2` with the sole intent of making everything easy to use while creating as barebones of a framework as possible, staying as faithful to the original syntax of `node:http2` as possible.
## Installation.
You can install ZephTTP by opening a command prompt in your project's directory and typing `npm install zephttp2`. Import the module by using the following import in your project:
```js
import zephttp2 from 'zephttp2';
```
## Initializing the interface.
Create the interface by calling the `createServer` method. For a secure server define an options object with certificate and key information.
```js
import fs from 'node:fs';
import zephttp2 from 'zephttp2';

// Creates an interface to be used with `node:http2`.
const server = zephttp2.createServer();

// Creates a secure interface to be used with `node:http2`.
const server = zephttp2.createServer({
        cert: fs.readFileSync('cert.pem'),
        key: fs.readFileSync('key.pem')
});
```
## Creating server routes on the stack.
Register a route to the stack by calling the `on` method.
```js
import zephttp2 from 'zephttp2';

// Creates an interface to be used with `node:http`.
const server = zephttp2.createServer();

// Handles GET requests on '/' from the subdomains '' and 'www'.
server.on('/', {
        allowedMethods: ['get'],
        allowedSubdomains: ['', 'www']
}, function (request, response) {
        // Sends the header information to the client.
        response.writeHead(200, {
                'content-type': 'text/html'
        });
        
        // Sends the response to the client.
        response.end('<!DOCTYPE html><html lang="en"><head></head><body><p>Hello, world!</p></body></html>');
});
```
## Exposing the server to the internet.
Make the server public by calling the `listen` method.
```js
import zephttp2 from 'zephttp2';

// Creates an interface to be used with `node:http`.
const server = zephttp2.createServer({ ... });

// Handles GET requests on '/' from the subdomains '' and 'www'.
server.on('/', {
        allowedMethods: ['get'],
        allowedSubdomains: ['', 'www']
}, function (request, response) {
        // Sends the header information to the client.
        response.writeHead(200, {
                'content-type': 'text/html'
        });
        
        // Sends the response to the client.
        response.end('<!DOCTYPE html><html lang="en"><head></head><body><p>Hello, world!</p></body></html>');
});

// Exposes the HTTP/2 server to the internet.
server.listen(443, '0.0.0.0', function () {
        const { address, port } = server.address();
        console.log('[zephttp2] Secure HTTP/2 server listening on ${address}:${port}.');
});
```
## Handling errors.
Handle errors by using the server object returns when creating the interface.
```js
import zephttp2 from 'zephttp2';

// Creates an interface to be used with `node:http`.
const server = zephttp2.createServer();

// Handles errors caused by the server.
server.server.on('error', function (error) {
        console.error(error);
});

// Handles errors caused by the client.
server.server.on('clientError', function (error) {
        console.error(error);
});
```