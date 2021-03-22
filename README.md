# REPO IS WORK IN PROGRESS

## ping-pong-websockets..

- Is a socket.io server that can handle async/await calls
- Exposes a `request` function which can be used on the client in order to make a async/await call

## How to

1. `mkdir my-server`
2. `cd my-server`
3. `npm init`
4. Add to package.json

```
"main": "server.js",
"scripts": {
  "dev": "node server.js"
},
```

5. `npm install ping-pong-websockets`
6. Create `server.js` with contents:

```
import PPW from "ping-pong-websockets/index.js";

PPW("somedir");
```

7. Add directory `somedir`
8. `npm run dev`

Your server should now be started on http://localhost:1337.

## Configuration

`PPW(path, hooks, port, config)`

### path (string)

Provide a string pointing at `somedir` (or some other directory of your liking)

### hooks (object)

Pass an object containing references to various services that you want to use in your socket calls (such as MongoDB / Redis and so on..)

### port (integer)

Provide a port of your liking. Defaults to 1337.

### config (object)

Provide configuration (https://socket.io/docs/v3/server-api/index.html) of your liking. Defaults to:

```
{
  cors: {
    origin: "*",
  },
}
```

(not that cors is required since socket.io version 4.0.0 and should be changed from "\*" in production)

## Your server

`PPW` returns an `io` instance which you can create custom socket.io functionality on

`somedir` should contain `someEvent.js`-files, these files are scanned and available as `socket.emit('someEvent')` on the client

This is the signature for any `.js` file within `somedir`

```
export default async (body, socket, io, hooks) => {
  const response = await hooks.mongo.insertSomething();
  return response;
}
```

Omitting the `async` keyword will treat the event as a regular socket io emit event.

## Your client

`index.html` is a temporary inclusion to this repo only to showcase how `request` works. The idea is simple:

```
(async () => {
  try {
    const result = await request("example-async", { somedata: "for the backend" });
    console.log(JSON.stringify(result));
  } catch ({ error }) {
    console.error(error);
  }
})();
```
