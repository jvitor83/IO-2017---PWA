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
            "src": "./icons/android-chrome-256x256.png",
            "sizes": "256x256",
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

> Musis

> #3e4eb8

> ./icons/


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
    evt.respondWith(new Response("YOU SHALL NOT PASS!"));
});
```

## Exemplo 6 (Service Worker - Sample)

### `service-worker.js` - Second
```
var CACHE_FILES = [
  './',
  './index.html',
  './manifest.json',
  './vendors/pwacompat/pwacompat.min.js',
  './vendors/mdl/material.min.js',
  './vendors/mdl/material.min.css',
  './vendors/mdl/material.icons.css',
  './vendors/mdl/material.icons.woff2',
  './icons/android-chrome-192x192.png',
  './icons/android-chrome-256x256.png',
  './icons/apple-touch-icon.png',
  './icons/browserconfig.xml',
  './icons/favicon-16x16.png',
  './icons/favicon-32x32.png',
  './icons/favicon.ico',
  './icons/mstile-150x150.png',
  './icons/safari-pinned-tab.svg'
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
  );
});
```

### MOSTRAR O CACHE NO DEV TOOLS



## Exemplo 5 (My App)

### COPIAR FETCH
### COPIAR ES6-PROMISE

### Adicionar no head (apos material)

```
    <script src="./vendors/es6-promise/es6-promise.js"></script>
    <script src="./vendors/fetch/fetch.js"></script>
```

### Adicionar no `service-worker.js`

```
  './vendors/es6-promise/es6-promise.js',
  './vendors/fetch/fetch.js',
```

### `index.html` (dentro do header no final)
```
<div class="toolbar">
    <form id="form">
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
    	var form = document.getElementById("form");

        form.addEventListener("submit", searchArtist, true);

        function searchArtist(event) {
            event.preventDefault();
	    
            var artistInputValue = document.getElementById('artist').value;
            if (!artistInputValue) {
                alert('Artist not informed!');
                return;
            }
            var searchArtistResult = document.getElementById('searchArtistResult');
            searchArtistResult.innerHTML = '';
            var url = 'https://api.musixmatch.com/ws/1.1/track.search?apikey=a2092c783aa8162ca77beab498103c8c&page_size=100&page=1&format=jsonp&callback=callback&q=' + artistInputValue;
            fetch(url)
                .then(function (response) { return response.text(); })
                .then(function (responseText) {
                    const match = responseText.match(/\((.*)\);/m);
                    if (!match[1]) throw new Error('invalid JSONP response');
                    const json = JSON.parse(match[1]);
                    return json;
                })
                .then(function (data) {
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



## Exemplo 6 (Service Worker)

`sw-precache`

### `sw-precache-config.js`

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
  ],
  "runtimeCaching": [{
    "urlPattern": /^https:\/\/api.musixmatch.com\//,
    "handler": "networkFirst"
  },{
    "urlPattern": /^http:\/\/s.mxmcdn.net\//,
    "handler": "networkFirst"
  }]
}
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


## Exemplo 9 (Push Notification)



### `index.html`
```
<!DOCTYPE html>
<html>
<head></head>
<body>
    <button onclick="inscreverUsuario()">Inscrever Usuario</button>
    <button onclick="desinscreverUsuario()">Desinscrever Usuario</button>
    <div id="inscricaoDoUsuario"></div>
    
    
    
</body>
</html>
```

### `index.html`

```
<script>
        /* AppKey */
        function urlB64ToUint8Array(base64String) {
            const padding = '='.repeat((4 - base64String.length % 4) % 4);
            const base64 = (base64String + padding)
                .replace(/\-/g, '+')
                .replace(/_/g, '/');
            const rawData = window.atob(base64);
            const outputArray = new Uint8Array(rawData.length);
            for (let i = 0; i < rawData.length; ++i) {
                outputArray[i] = rawData.charCodeAt(i);
            }
            return outputArray;
        }
        const applicationServerKey = urlB64ToUint8Array('-----------------------------------------');
	
        
</script>
```

#### https://web-push-codelab.appspot.com

#### COLOCA A CHAVE NO CODIGO

```
var swRegistration = null;
if ('serviceWorker' in navigator && 'PushManager' in window) {
    navigator.serviceWorker.register('sw.js').then(function (swReg) {
	swRegistration = swReg;
    });
}
```

```
function inscreverUsuario() {
	swRegistration.pushManager.subscribe({
	    userVisibleOnly: true,
	    applicationServerKey: applicationServerKey
	})
	.then(function (subscription) {
	    gravarInscricaoNoServidor(subscription);
	});
}
function gravarInscricaoNoServidor(subscription) {
        var usrSub = document.getElementById('inscricaoDoUsuario');
        usrSub.innerHTML = JSON.stringify(subscription);
}
```

```
function desinscreverUsuario() {
    swRegistration.pushManager.getSubscription()
	.then(function (inscricao) {
	    if (inscricao) {
		return inscricao.unsubscribe();
	    }
	})
	.then(function (sub) {
	    removerInscricaoNoServidor(sub);
	});
}
function removerInscricaoNoServidor(sub) {
    var usrSub = document.getElementById('inscricaoDoUsuario');
    if (sub) {
	usrSub.innerHTML = JSON.stringify(sub);
    }else{
	usrSub.innerHTML = '';
    }
}
```

#### `sw.js`
```
self.addEventListener("push", function (event) {
    console.log('[Service Worker] Push Received.');

    var data = event.data.json();
    console.log(`[Service Worker] Push had this data:`, data);

    var title = data.title;
    var options = data.options;
    const notificationPromise = self.registration.showNotification(title, options);
    event.waitUntil(notificationPromise);
});
```

```
self.addEventListener('notificationclick', function(event) {
  console.log('[Service Worker] Notification click Received.');

  event.notification.close();

  event.waitUntil(
    clients.openWindow('https://developers.google.com/web/')
  );
});
```

#### MOSTRAR FUNCIONANDO

```
{
"title": "Google IO 2017 Extended",
"options": {
  "body": "O que você está achando do Google IO 2017 Extended?",
  "icon": "https://cdn1.tnwcdn.com/wp-content/blogs.dir/1/files/2017/01/Google-IO.jpg",
  "actions": [
    { "action": "yes", "title": "Ótimo", "icon": "https://image.spreadshirtmedia.com/image-server/v1/compositions/1006808757/views/1,width=300,height=300,appearanceId=1,backgroundColor=E8E8E8,version=1485256808/heart-emoticon-men-s-t-shirt.jpg" },
    { "action": "no", "title": "Muito Bom", "icon": "http://pix.iemoji.com/images/emoji/apple/ios-9/256/thumbs-up-sign.png" }
  ]
}
}
```
