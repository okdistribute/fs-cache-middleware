# fs-cache-stream

A streaming cache for any filesystem 

```
npm install fs-cache-stream
```

Similar to [express-asset-file-cache-middleware](https://www.npmjs.com/package/express-asset-file-cache-middleware), but doesn't require express and supports streaming payloads.

## Usage


```js
const fileCacheMiddleware = require('./cache')
```

function getAsset (url, params) {
  return {
    contentType: 'image/png',
    contentLength, // if known
    stream: streamingFetch('https://get/that/asset.png')
  }
}

var cache = fileCacheMiddleware(getAsset, { 
  maxSize: 10 * 1024 * 1024 * 1024 
  cacheDir: '/tmp'
})

http.createServer(funcrion (req, res) {
  // Pass extra parameters on the fly with the request
  var params = { token: 'my-token' }
  cache(req, res, params, (err) => {
    if (err) {
      logger.error(err)
      res.statusCode = 500
      res.end(err.message)
    }
  })
}
