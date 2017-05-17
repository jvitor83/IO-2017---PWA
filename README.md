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
