locomotive-http-socketio-server
===============================

Integration of locomotive (express) and socket.io

## Installation

    npm install locomotive-http-socketio-server
    
## Usage

Change your server.js file to use httpSocketioServer. The socket.io instance will be available off app.sio.

```
var locomotive = require('locomotive')
  , bootable = require('bootable');
locomotive.boot.httpSocketioServer = require('locomotive-http-socketio-server');

// Create a new application and initialize it with *required* support for
// controllers and views.  Move (or remove) these lines at your own peril.
var app = new locomotive.Application();
app.phase(locomotive.boot.controllers(__dirname + '/app/controllers'));
app.phase(locomotive.boot.views());

// Add phases to configure environments, run initializers, draw routes, and
// start an HTTP server.  Additional phases can be inserted as neeeded, which
// is particularly useful if your application handles upgrades from HTTP to
// other protocols such as WebSocket.
app.phase(require('bootable-environment')(__dirname + '/config/environments'));
app.phase(bootable.initializers(__dirname + '/config/initializers'));
app.phase(locomotive.boot.routes(__dirname + '/config/routes'));
app.phase(locomotive.boot.httpSocketioServer(3000, '0.0.0.0'));

// Handle the socket.io events that are specific to your application
app.phase(require('__dirname' + '/lib/socket'))

// Boot the application.  The phases registered above will be executed
// sequentially, resulting in a fully initialized server that is listening
// for requests.
app.boot(function(err) {
  if (err) {
    console.error(err.message);
    console.error(err.stack);
    return process.exit(-1);
  }
});
```

Then, in lib/socket.js:

```
module.exports = function(){
  // listen to the connect event
  this.sio.sockets.on('connection', function(socket){
    
  });
}
```
