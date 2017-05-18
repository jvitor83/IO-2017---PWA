# IO-2017-PWA

## Testando 0

### lighthouse


[chrome://flags/#enable-add-to-shelf](chrome://flags/#enable-add-to-shelf)

[chrome://flags/#bypass-app-banner-engagement-checks](chrome://flags/#bypass-app-banner-engagement-checks)

https://chrome.google.com/webstore/detail/lighthouse/blipmdconlkpinefehnmjammfjpmpbjk




https://pwa.rocks/


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
    </body>

</html>
```

## Exemplo 2
### https://iconsflow.com/editor
### http://realfavicongenerator.net

### COPIAR PWACOMPAT

### `index.html` - (adicionar no final do head)
```
<script src="./vendors/pwacompat/pwacompat.min.js"></script>
```


## Exemplo 3 (AppShell)

### INICIA LIVE-SERVER
`live-server`


### COPIAR MDL

### `index.html` - (final da tag head)
```
    <link rel="stylesheet" href="./vendors/mdl/material.min.css">
    <script src="./vendors/mdl/material.min.js"></script>
    <link rel="stylesheet" href="./vendors/mdl/material.icons.css">
```

### `index.html` - (adicionar dentro da body)
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

### https://getmdl.io/templates/index.html


## Exemplo 4 (Service Worker - Sample)

### `index.html` - (dentro do body - no final)
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

## Exemplo 5 (Service Worker - Sample)

### `service-worker.js` - Second
```
self.addEventListener('install', function(evt) {
  console.log('The service worker is being installed.');
});

self.addEventListener('fetch', function(evt) {
    evt.respondWith(new Response("YOU SHALL NOT PASS!"));
});
```

## Exemplo 6 (Service Worker - Sample)

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
var CACHE = 'v1';

self.addEventListener('install', function (event) {
  event.waitUntil(
    caches.open(CACHE).then(function (cache) {
      return cache.addAll(CACHE_FILES);
    })
  );
});

self.addEventListener('fetch', function (event) {
  event.respondWith(
    caches.match(event.request)
      .catch(function () {
        return fetch(event.request);
      })
      .then(function (response) {
        caches.open(CACHE).then(function (cache) {
          cache.put(event.request, response);
        });
        return response.clone();
      })
  );
});
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

### COPIAR FETCH
### COPIAR FETCHJSONP
### COPIAR ES6-PROMISE

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

## Exemplo 8 (Finalizacao)

### Git
```shell
git init
git add .
git commit -m "First commit"
git remote add origin https://github.com/jvitor83/musis.git
git push origin master
```


### https://github.com/jvitor83/musis/settings
> Enable gh pages

### https://jvitor83.github.io/musis/index.html
