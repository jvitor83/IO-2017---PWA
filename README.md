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
<!doctype html>
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

### https://getmdl.io/templates/index.html

### `index.html`
```
    <link rel="stylesheet" href="./vendors/mdl/material.min.css">
    <script src="./vendors/mdl/material.min.js"></script>
    <link rel="stylesheet" href="./vendors/mdl/material.icons.css">
```

```
<div class="mdl-layout mdl-js-layout mdl-layout--fixed-header">

	<header class="mdl-layout__header">
	    <div class="mdl-layout__header-row">
		<span class="mdl-layout-title">Musis</span>
	    </div>
	</header>
	<main class="mdl-layout__content">
	    <div class="page-content">

	    </div>
	</main>

</div>
```

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
var CACHE_FILES = [
    './index.html',
    './manifest.json',
    './vendors/mdl/material.min.js',
    './vendors/mdl/material.min.css',
    './vendors/mdl/material.icons.css',
    './vendors/mdl/material.icons.woff2'
];
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
    return cache.addAll(CACHE_FILES);
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

## Exemplo 5 (Service Worker)

`sw-precache`

`sw-precache --config=sw-precache-config.js`

```
module.exports = {
  "staticFileGlobs": [
    "**/*.css",
    "**/*.html",
    "**/*.png",
    "**/*.ico",
    "**/*.svg",
    "**/*.woff2",
    "**/*.js",
    "**/*.json"
  ]
}
```


## Exemplo 6 (My App)

### `index.html` (dentro do header no final)
```
<div class="toolbar">
    <form onsubmit="searchArtist();return false">
	<div class="mdl-grid">

	    <div class="mdl-cell mdl-cell--3-col-phone mdl-cell--1-col-phone">
		<!-- Textfield with Floating Label -->
		<div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label">
		    <input class="mdl-textfield__input" autofocus type="text" id="artist">
		    <label class="mdl-textfield__label" for="artist">Artist/Music</label>
		</div>
	    </div>
	    <div class="mdl-cell mdl-cell--1-col-phone mdl-cell--1-col-phone">
		<!-- Colored FAB button with ripple -->
		<button class="mdl-button mdl-js-button mdl-button--fab mdl-js-ripple-effect mdl-button--colored">
		    <i class="material-icons">search</i>
		</button>
	    </div>

	</div>
    </form>

</div>
```

### `index.html` (dentro da div page-content)
```
<div id="searchArtistResult" class="mdl-grid">

</div>

<script id="artist-card" type="text/template">
<div class="mdl-card mdl-cell mdl-shadow--6dp artist-card">
    <div class="mdl-card__title artist-card-title">
	<span class="mdl-card__title-text artist-card-title-content">{{name}}</span>
    </div>
    <div class="mdl-card__supporting-text artist-card-text">
	<a href="{{url}}" target="blank"><span>{{track_name}}</span></a>
    </div>
    <div class="mdl-card__supporting-text artist-card-img-container">
	<img src="{{image}}" class="artist-card-img" />
    </div>
</div>
</script>
```

### `index.html` (dentro do body - no final)
```
    <script>
        function searchArtist() {
            var artistInputValue = document.getElementById('artist').value;
            if (!artistInputValue) {
                alert('Artist not informed!');
                return;
            }
            var searchArtistResult = document.getElementById('searchArtistResult');
            searchArtistResult.innerHTML = '';
            var url = 'https://api.musixmatch.com/ws/1.1/track.search?apikey=a2092c783aa8162ca77beab498103c8c&page_size=100&page=1&format=jsonp&callback=callback&q=' + artistInputValue;
            fetchJsonp(url).then(function (response) {
                return response.json();
            }).then(function (data) {
                data.message.body.track_list.forEach(function (data) {
                    var artistCard = generateArtistCard(data.track);
                    searchArtistResult.insertAdjacentHTML('beforeend', artistCard);
                });
            });
        }

        function generateArtistCard(artist) {
            var template = document.getElementById('artist-card').innerHTML;
            template = template.replace('{{name}}', artist.artist_name);
            template = template.replace('{{track_name}}', artist.track_name);
            template = template.replace('{{url}}', artist.track_share_url);
            template = template.replace('{{image}}', artist.album_coverart_100x100);
            return template;
        }
    </script>
```

### `index.html` (dentro do head - no final)
```
<style>
        .toolbar {
            padding: 0px 16px 0px 16px;
            background-color: white;
            color: black;
            height: 90px;
        }

        .artist-card {
            background-color: white;
            min-height: 130px;
        }

        .artist-card-title-content {
            color: white;
        }

        .artist-card-title {
            background-color: #3E4EB8;
        }

        .artist-card-text {
            color: black;
            font-size: 14pt;
            z-index: 99;
            width: 60%;
        }

        .artist-card-img {
            float: right;
        }

        .artist-card-img-container {
            position: absolute;
        }
    </style>
```


## Exemplo 7 (Service Worker - RUNTIME)

### `sw-precache-config.js`
```
  "runtimeCaching": [{
    "urlPattern": /^https:\/\/api.musixmatch.com\//,
    "handler": "networkFirst"
  }]
```

`sw-precache --config=sw-precache-config.js`
