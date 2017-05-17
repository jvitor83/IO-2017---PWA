# IO-2017-PWA

## Exemplo 1
### `manifest.json`
```
{
    "name": "Musis",
    "short_name": "Musis",
    "description": "Music searcher!",
    "icons": [
        {
            "src": "./icons/android-chrome-512x512.png",
            "sizes": "512x512",
            "type": "image/png"
        }
    ],
    "start_url": "./",
    "display": "standalone",
    "orientation": "any",
    "theme_color": "#3E4EB8",
    "background_color": "#3E4EB8"
}
```

### `index.html`
```
<html>

<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="x-ua-compatible" content="ie=edge">

    <link rel="manifest" href="manifest.json">
</head>

    <body>
        <div>
             <header>
             </header>
             <main>
             </main>
	</div>
    </body>

</html>
```

## Exemplo 2
### https://iconsflow.com/editor
### http://realfavicongenerator.net


## Exemplo 3 (AppShell)

## Exemplo 4 (Service Worker - Sample)

### `index.html`
```
    <script>
        if ('serviceWorker' in navigator) {
            navigator.serviceWorker.register('service-worker.js');
        }
    </script>
```

### `service-worker.js` - First
```
self.addEventListener('install', function(evt) {
  console.log('The service worker is being installed.');
});

self.addEventListener('fetch', function(evt) {
  console.log('HELLO WORLD from FETCH event in Service-Worker!');
});
```

### `service-worker.js` - Second
```
self.addEventListener('install', function(evt) {
  console.log('The service worker is being installed.');
});

self.addEventListener('fetch', function(evt) {
    evt.respondWith(new Response("YOU SHALL NOT PASS!"));
});
```


### `service-worker.js` - Third
```
var CACHE = 'cache-and-update';

// On install, cache some resources.
self.addEventListener('install', function(evt) {
  console.log('The service worker is being installed.');

  // Ask the service worker to keep installing until the returning promise
  // resolves.
  evt.waitUntil(precache());
});

// On fetch, use cache but update the entry with the latest contents
// from the server.
self.addEventListener('fetch', function(evt) {
  console.log('The service worker is serving the asset.');
  // You can use `respondWith()` to answer immediately, without waiting for the
  // network response to reach the service worker...
  evt.respondWith(fromCache(evt.request));
  // ...and `waitUntil()` to prevent the worker from being killed until the
  // cache is updated.
  evt.waitUntil(update(evt.request));
});

// Open a cache and use `addAll()` with an array of assets to add all of them
// to the cache. Return a promise resolving when all the assets are added.
function precache() {
  return caches.open(CACHE).then(function (cache) {
    return cache.addAll([
      './controlled.html',
      './asset'
    ]);
  });
}

// Open the cache where the assets were stored and search for the requested
// resource. Notice that in case of no matching, the promise still resolves
// but it does with `undefined` as value.
function fromCache(request) {
  return caches.open(CACHE).then(function (cache) {
    return cache.match(request).then(function (matching) {
      return matching || Promise.reject('no-match');
    });
  });
}

// Update consists in opening the cache, performing a network request and
// storing the new response data.
function update(request) {
  return caches.open(CACHE).then(function (cache) {
    return fetch(request).then(function (response) {
      return cache.put(request, response);
    });
  });
}
```
