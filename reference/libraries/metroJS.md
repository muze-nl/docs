# MetroJS: HTTPS Client with middleware

MetroJS is a javascript library containing an HTTPS client with support for middleware. It's imilar to ExpressJS, but for the client.

MetroJS acts as a proxy between your code and the Fetch API. This proxying arrangement allows for middleware, other software which can act upon the requests. For example middleware might be used to handle authentication, or to process the retrieved data into a consistent format

**Example:**
```javascript
import * as metro from '@muze-nl/metro'

const client = metro.client({
  url: 'https://github.com/'
}).with((req,next) => {
  req = req.with({
    headers: {
      'Content-Type':'application/json',
      'Accept':'application/json'
    }
  })
  if (typeof req.body == 'object') {
    req = req.with({
      body: JSON.stringify(req.body)
    })
  }
  let res = await next(req)
  let body = await res.json()
  return res.with({ body })
})
```
## Table of Contents
1. [Introduction](#introduction)
2. [Quickstart](docs/quickstart.md)
3. [Usage](#usage)
4. [Middleware](#middleware)
5. [Documentation](docs/) - See also [metro.muze.nl](https://metro.muze.nl/)
6. [Contributions](CONTRIBUTING.md)
7. [License](#license)

<a name="introduction"></a>
## Introduction

MetroJS is an HTTPS client with support for middleware. Similar to [ExpressJS](https://expressjs.com/), but for the client.

You add middleware with the `with()` function, as shown above.

The signature for a middleware function is:

```javascript
async (request, next) => {
   // alter request
   let response = await next(request)
   // alter response
   return response
}
```

However, both request and response are immutable. You can not change them. You can 
however create a copy with some values different, using the `with()` function.

Both metro.request() and metro.response() are compatible with the normal Request 
and Response objects, used by the Fetch API. Any code that works with those, will work
with the request and response objects in MetroJS.

<a name="usage"></a>
## Usage

```bash
npm install @muze-nl/metro
```

In the browser, using a cdn:
```html
<script src="https://cdn.jsdelivr.net/npm/@muze-nl/metro@0.6.4/dist/browser.js"></script>
<script>
  async function main() {
    const client = metro.client('https://example.com/')
    const result = await client.get('folder/page.html')
  }
  main()
</script>
```

Using ES6 modules, in the browser or Node:
```javascript
import * as metro from '@muze-nl/metro'

async function main() {
  const client = metro.client('https://example.com/')
  const result = await client.get('folder/page.html')
}
```

<a name="middleware"></a>
## Using middleware
A middleware is a function with `(request, next)` as parameters, returning a `response`.
Both request and response adhere to the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
[Request](https://developer.mozilla.org/en-US/docs/Web/API/Request) and 
[Response](https://developer.mozilla.org/en-US/docs/Web/API/Response) standard.

`next` is a function that takes a `request` and returns a `Promise<Response>`. This function is defined by MetroJS
and automatically passed to your middleware function. The idea is that your middleware function can change the request
and pass it on to the next middleware or the actual fetch() call, then intercept the response and change that and return it:

```javascript
async function myMiddleware(req,next) {
  req = req.with('?foo=bar')
  let res = await next(req)
  if (res.ok) {
    res = res.with({headers:{'X-Foo':'bar'}})
  }
  return res
}
```

Both request and response have a `with` function. This allows you to create a new request or response, from 
the existing one, with one or more options added or changed. The original request or response is not changed.

