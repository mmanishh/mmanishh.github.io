---
title: How to Fetch in NodeJS
date: 2022-11-21
image: /assets/images/2022/nodejs-fetch.svg
---

Fetch API was introduced in 2015 and launched as a modern successor to `XMLHttpRequest` and it has become the default method to make an asynchronous HTTP request in web applications. Out of the many advantages over `XMLHttpRequest`, it offers the promise of making your code cleaner.

The Fetch API is provided as a high-level function, and in its most basic version, it takes a URL and produces a promise that resolves to the response:

```javascript
fetch("http://example.com/api/endpoint")
  .then((response) => {
    // Do something with response
  })
  .catch(function (err) {
    console.log("Unable to fetch -", err);
  });
```

However Fetch API has been there since 2015, it hasn`t been included in NodeJS core API due to some [limitations](https://news.ycombinator.com/item?id=30162332). But fetch api is being introduced in [v17](https://github.com/nodejs/node/pull/41749). So, you can use fetch API in nodejs from v17.

If you were about to use Fetch API in NodeJS runtime on the version below 17 just like in the browser then you might have encountered this problem.

```sh
ReferenceError: fetch is not defined
```

But by installing other third-party libs/alternatives you can use fetch API in NodeJS versions which do not support Fetch API.

## Alternatives

## a. Axios

[Axios](https://axios-http.com/)  is a Promise-based HTTP client for both browsers and NodeJS. Axios provides a simple-to-use library in a small package with a very extensible interface.

### Features

- Make XMLHttpRequests from the browser
- Make HTTP requests from node.js
- Supports the Promise API
- Intercept request and response
- Transform request and response data
- Cancel requests
- Automatic transforms for JSON data
- Client side support for protecting against XSRF

### Installation

Using npm:

```sh
npm install axios
```

### Usage

Using axios as simple as using Fetch API

```javascript
const axios = require('axios');

// Make a request for a user with a given ID
axios.get('/user?ID=12345')
  .then(function (response) {
    // handle success
    console.log(response);
  })
  .catch(function (error) {
    // handle error
    console.log(error);
  })
  .then(function () {
    // always executed
  });
```

## b. Node-Fetch

[Node Fetch](https://github.com/node-fetch/node-fetch)  is a lightweight module that brings the Fetch API to Node.js. It's built to be consistent with the Browser-based Fetch API `window.fetch`

### Features

- Stay consistent with `window.fetch` API.
- Make conscious trade-off when following [WHATWG](https://whatwg.org/) fetch spec and stream spec - implementation details, document known differences.
- Use native promise and async functions.
- Use native Node streams for body, on both request and response.
- Decode content encoding (gzip/deflate/brotli) properly, and convert string output (such as res.text() and res.json()) to UTF-8 automatically.
- Useful extensions such as redirect limit, response size limit, explicit errors for troubleshooting.

### Installation

Using npm (requires Node.js 12.0.0):

```sh
npm install node-fetch
```

### Usage

```javascript
import fetch from 'node-fetch';

const response = await fetch('https://api.github.com/users/github');
const data = await response.json();

console.log(data);
```

## c. Superagent

[Superagent](https://github.com/visionmedia/superagent) is a small progressive client-side HTTP request library, and Node.js module with the same API, supporting many high-level HTTP client features.

### Installation

Using npm:

```sh
npm install superagent
```

### Usage

```javascript
const superagent = require('superagent');

// callback
superagent
  .post('/api/pet')
  .send({ name: 'Manny', species: 'cat' }) // sends a JSON post body
  .set('X-API-Key', 'foobar')
  .set('accept', 'json')
  .end((err, res) => {
    // Calling the end function will send the request
  });

// promise with then/catch
superagent.post('/api/pet').then(console.log).catch(console.error);

// promise with async/await
(async () => {
  try {
    const res = await superagent.post('/api/pet');
    console.log(res);
  } catch (err) {
    console.error(err);
  }
})();
```

## d. Undici-fetch

[Undici Fetch](https://github.com/Ethan-Arrowood/undici-fetch) is another library based on [undici](https://github.com/nodejs/undici) which is NodeJS`s other HTTP library. It's believed to be [faster](https://github.com/Ethan-Arrowood/undici-fetch/blob/main/benchmarks.md) than other existing libraries. The reason for introducing a replacement for NodeJs core HTTP stack is explained here in the [blog](https://nodejs.medium.com/introducing-undici-4-1e321243e007)

### Installation

Using npm:

```sh
npm i undici-fetch
```

### Usage

```javascript
// import and initialize all-at-once
const fetch = require('undici-fetch')()

// Promise Chain
fetch('https://example.com')
  .then(res => res.json())
  .then(json => console.log(json))

// Async/Await
const res = await fetch('https://example.com')
const json = await res.json()
```

## Availability of Fetch in NodeJS

Fetch API is finally available to NodeJS from `v17.6`. You can now run Fetch-related code by adding the --experimental-fetch parameter with the node command:

```sh
node app.js --experimental-fetch
```

### Benefits of Fetch API in NodeJS

Thare are a lot of benefits of Fetch API being inbuilt into Node Module. Some of them include the following:

 1. No Extra Third Party Fetch Package
 2. Cross Platform Familiarity (Includes Browser and NodeJS Runtime)
 3. Faster Implementation (Since its based on undici)

## Conclusion

We discussed the availability of Fetch API in browser since 2015 replacing `XMLHttpRequest` and being unavailable in NodeJS runtime. Also, we listed the alternatives for Fetch API in NodeJS with other third party libs. However, Fetch API was introduced in `v17` So if you are using Node version `>=17` , you can start using Fetch API through built-in node module.





