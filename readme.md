

# scratch3-api

> ### Now Supporting [Turbowarp](https://turbowarp.org)!

A remake of [trumank's scratch-api](https://www.npmjs.com/package/scratch-api) optimized for scratch 3.0. It not only adds to the api, but is also composed mostly of promises to prevent [callback hell](http://callbackhell.com/). So, instead of

```js
const Scratch = require("scratch-api");

Scratch.UserSession.create("<username>", "<password>", function (err, user) {
  user.cloudSession("<project>", function (err, cloud) {
    cloud.on("set", function (name, value) {
      console.log(`${name} was set to ${value}`);
    });
  });
});
```

you can do

```js
const Scratch = require("scratch3-api");

async function main() {
  let session = await Scratch.UserSession.create("<username>", "<password>");
  let cloud = await session.cloudSession("<project>");
  cloud.on("set", function (name, value) {
    console.log(`${name} was set to ${value}`);
  });
}

main();
```

which is a lot easier to read. If, for some reason, you _like_ callback hell, you can just use `.then()`.

```js
const Scratch = require("scratch3-api");

Scratch.UserSession.create("<username>", "<password>").then(function (session) {
  session.cloudSession("<project>").then(function (cloud) {
    cloud.on("set", function (name, value) {
      console.log(`${name} was set to ${value}`);
    });
  });
});
```

## Installation

> Requires Node.js >= 10

Install with npm:

```sh
npm install scratch3-api
```

Or yarn:

```sh
yarn add scratch3-api
```

Or by cloning this repository:

```sh
git clone https://github.com/ErrorGamer2000/scratch3-api.git
```

## Adding to your script

### CommonJS
```js
const Scratch = require("scratch3-api");
// or
const { UserSession, CloudSession, Projects, Rest } = require("scratch3-api")
```


### ESM
```js
import Scratch from "scratch3-api";
// or
import { UserSession, CloudSession, Projects, Rest } from "scratch3-api";
```


## API Documentation

- [`UserSession`](#usersession-api)
- [`CloudSession`](#cloudsession-api)
- [`Projects`](#projects-api)
- [`Rest`](#rest-api)
- [`Other APIs`](#other-apis)
  - [`Project`](#project-api)
  - [`Sprite`](#sprite-api)
- [Credits](#contributors)

## UserSession API

The `UserSession` api handles the login and verification that makes the package to work. (Stored in the `session` variable for examples)
 
### Methods
---
#### Static Methods

- `create([username], [password])`
  - Creates and loads a new UserSession with the given username and password.
  - `username` - The Scratch account username (not case sensitive). Optional. If not provided user will be prompted.
  - `password` - The Scratch account password. Optional. If not provided user will be prompted.
  - `returns`: `Promise.<UserSession>`
   <!--- For some reason this is needed to keep code from overlapping last bullet --->
<p />
  
  ```js
  let session = await Scratch.UserSession.create("<username", "<password>");
  
  //Could also be done with
  let session = new Scratch.UserSession();
  await session.load("<username", "<password>");
  ```
- `constructor()`
  - Creates a blank, unloaded `UserSession`.
  - `returns`: `UserSession`
  <p />
  
  ```js
  let session = new Scratch.UserSession();
  ```
  
#### Instance Methods

- `load([username], [password])`
  - Loads the `UserSession` instance with the given name and password.
  - `username` - The Scratch account username (not case sensitive). Optional. If not provided user will be prompted.
  - `password` - The Scratch account password. Optional. If not provided user will be prompted.
  - `returns`: `Promise.<undefined>`
  <p />
  
  ```js
  await session.load("<username", "<password>");
  ```
- `prompt()`
  > ⚠ Deprecated! This feature will be removed soon. Please use `<UserSession>.load` without parameters instead.
  - Prompts the user for their username and password then loads the `UserSession`.
  - `returns`: `Promise.<undefined>`
  <p />
  
  ```js
  await session.prompt();
  ```
- `verify()`
  - Validates the `UserSession`'s login.
  - `returns`: `Promise.<Boolean>`
  <p />
  
  ```js
  const valid = await session.verify();
  ```
- `comment(options)`
  - Validates the `UserSession`'s login.
  - `options`
    - `project`, `user`, or `studio` - The project, user, or studio to comment on. User must be a username and the others must be ids.
    - `parent` - The comment id to reply to. Optional.
    - `replyto` - The user id to address (@username ...). Optional.
    - `content` - The text of the comment to post.
  - `returns`: `Promise.<undefined>`
  <p />
  
  ```js
  await session.comment({
    project: 517845853,
    content: "Commented from Node.js with scratch3-api!"
  });
  ```
- `cloudSession(proj, [turbowarp = false])`
	- Create a new [`CloudSession`](#cloudsession-api) for the specified project with the current `UserSession` and connects it.
	- `proj` - The id of the project to connect to. Can be a string or number.
	- `turbowarp` - Whether or not to connect to the [turbowarp](https://turbowarp.org) cloud servers instead of the scratch cloud servers.
	- `returns`: `Promise.<CloudSession>`
	<p />
	
  ```js
	let cloud = await session.cloudSession(60917032);
  ```

### Instance Properties

- `get` `projects` - Returns a new instance of the [`Projects`](#usersession-projects-api) api.
- `loaded` - A boolean describing whether or not the session has been loaded with a username and password.
- `valid` - A boolean describing whether or not the session is valid.
- `username` - The username that the session was loaded with. Will not be defined if the `load()` method has not been called.
- `password` - The password that the session was loaded with. Will not be defined if the `load()` method has not been called.
- `id` - The user's id number.
- `sessionId` - A string containing the scratch session id that the user is currently logged in with.
- `token` - The session's scratch token.

## CloudSession API

Extends: [`EventEmitter`](https://nodejs.org/api/events.html#events_class_eventemitter)

### Methods
---
#### Static Methods

- `create(user, proj, [turbowarp = false])`
	- `user` - The `UserSession` to create the `CloudSession` with. If an invalid `UserSession` is provided, thing may break.
	- `proj` - The id of the project to connect to. Can be a string or number.
	- `turbowarp` - Whether or not to connect to the [turbowarp](https://turbowarp.org) cloud servers instead of the scratch cloud servers.
	- `returns`: `Promise.<CloudSession>`
	<p />
	
  ```js
  let cloud = Scratch.CloudSession.create(session, 60917032);
  ```
- `constructor(user, proj, [turbowarp = false])`


---

## Contributors

- [
