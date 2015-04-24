[![Fortune.js](https://fortunejs.github.io/fortune-website/assets/fortune_logo.svg)](http://fortunejs.com)

[![Build Status](https://img.shields.io/travis/fortunejs/fortune/rewrite.svg?style=flat-square)](https://travis-ci.org/fortunejs/fortune)
[![npm Version](https://img.shields.io/npm/v/fortune.svg?style=flat-square)](https://www.npmjs.com/package/fortune)
[![License](https://img.shields.io/npm/l/fortune.svg?style=flat-square)](https://www.npmjs.com/package/fortune)
[![Piggu](https://img.shields.io/badge/pigs-flying-fca889.svg?style=flat-square)](http://fortunejs.com)

**Fortune** is a framework for [data-driven applications](https://groups.drupal.org/node/143074). It provides [workflows](https://en.wikipedia.org/wiki/Workflow) for manipulating data.

[View the website](http://fortunejs.com) for documentation. Get it from `npm`:

```sh
$ npm install fortune --save
```


## Motivation

The goal is to provide an integrated system for automating data manipulation given a set of models that conform to [some limitations](https://github.com/fortunejs/fortune/blob/rewrite/lib/index.js#L113). Fortune provides generic features of web applications, or *skins around databases* if you're a hater. It is intended to be used standalone or composed within Node.js web frameworks (Koa, Express, Hapi, etc).


## Key Concepts

- The **dispatcher** controls the flow of a request through middleware functions that mutate state.
- There are two components that are interchangeable: the **adapter** and the **serializer**. Adapters interact with data storage, and serializers parse requests and render responses.
- Networking is optional, and may be handled by serializers.


## Example

Here is how to get started:

```js
import fortune from 'fortune'
import http from 'http'

const app = fortune.create()
const listener = fortune.net.requestListener.bind(app)
const server = http.createServer(listener)
```

This sets up an instance of Fortune with default options, as well as a request listener which is used to start an HTTP server instance.

```js
app.defineType('user', {
  name: { type: String },
  groups: { link: 'group', isArray: true, inverse: 'members' }
})

app.defineType('group', {
  name: { type: String },
  members: { link: 'user', isArray: true, inverse: 'group' }
})
```

Defining record types. There is a many-to-many relationship between `user` and `group` on the `groups` and `members` fields respectively.

```js
app.initialize().then(() => server.listen(1337))
```

Finally we need to call `initialize` before we do anything with the instance, since the setup is not declarative. This yields a HTTP API that conforms to the full [JSON API](http://jsonapi.org) specification, and a custom implementation of [Micro API](http://micro-api.org) specification. By default, it is backed by an in-memory database, NeDB.


## License

Fortune is licensed under the [MIT license](https://raw.githubusercontent.com/fortunejs/fortune/rewrite/LICENSE).
