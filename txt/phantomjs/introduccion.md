# PhantomJS

## 1. Qué es PhantomJS

PhantomJS es un navegador sin interfaz gráfica que sólo puede ser utilizado con una terminal, con la consola de windows o usando una API de Javascript.

## 2. Para qué puede servir

* Para lanzar Test Unitarios con Mocha y muchos otros.

* Hacer capturas de pantalla de nuestra web.

* Manejar el DOM con librerías JQuery.

* Simular el comportamiento del usuario.

* etc...

## 3. Instalación

  Solo hay que ir a la página oficial de PhantomJS y desde allí descargar la versión para tu Sistema Operativo.

## 4. Ejecución

  Para ejecutar un script PhantomJS debemos poner en consola:

  ```bash
  phantomjs nombre_archivo
  ```

## 5. Ejemplos

### 5.1. Hola mundo

```javascript
"use strict";
console.log('Hello, world!');
phantom.exit();
```

### 5.2 Testear un servidor

```javascript
"use strict";
var page = require('webpage').create(),
    server = 'http://posttestserver.com/post.php?dump',
    data = 'universe=expanding&answer=42';

page.open(server, 'post', data, function (status) {
    if (status !== 'success') {
        console.log('Unable to post!');
    } else {
        console.log(page.content);
    }
    phantom.exit();
});
```

### 5.3 Escribir en un fichero

```javascript
"use strict";
var fs = require('fs'),
    system = require('system');

if (system.args.length < 3) {
    console.log("Usage: echoToFile.js DESTINATION_FILE <arguments to echo...>");
    phantom.exit(1);
} else {
    var content = '',
        //f = null,
        i;
    for ( i= 2; i < system.args.length; ++i ) {
        content += system.args[i] + (i === system.args.length-1 ? '' : ' ');
    }

    try {
        fs.write(system.args[1], content, 'w');
    } catch(e) {
        console.log(e);
    }

    phantom.exit();
}

```

### 5.4 Cambiar el contenido de la página web

```javascript
"use strict";
var page = require('webpage').create();
page.viewportSize = { width: 400, height : 400 };
page.content = '<html><body><canvas id="surface"></canvas></body></html>';
page.evaluate(function() {
    var el = document.getElementById('surface'),
        context = el.getContext('2d'),
        width = window.innerWidth,
        height = window.innerHeight,
        cx = width / 2,
        cy = height / 2,
        radius = width  / 2.3,
        imageData,
        pixels,
        hue, sat, value,
        i = 0, x, y, rx, ry, d,
        f, g, p, u, v, w, rgb;

    el.width = width;
    el.height = height;
    imageData = context.createImageData(width, height);
    pixels = imageData.data;

    for (y = 0; y < height; y = y + 1) {
        for (x = 0; x < width; x = x + 1, i = i + 4) {
            rx = x - cx;
            ry = y - cy;
            d = rx * rx + ry * ry;
            if (d < radius * radius) {
                hue = 6 * (Math.atan2(ry, rx) + Math.PI) / (2 * Math.PI);
                sat = Math.sqrt(d) / radius;
                g = Math.floor(hue);
                f = hue - g;
                u = 255 * (1 - sat);
                v = 255 * (1 - sat * f);
                w = 255 * (1 - sat * (1 - f));
                pixels[i] = [255, v, u, u, w, 255, 255][g];
                pixels[i + 1] = [w, 255, 255, v, u, u, w][g];
                pixels[i + 2] = [u, u, w, 255, 255, v, u][g];
                pixels[i + 3] = 255;
            }
        }
    }

    context.putImageData(imageData, 0, 0);
    document.body.style.backgroundColor = 'white';
    document.body.style.margin = '0px';
});

page.render('colorwheel.png');

phantom.exit();

```

### 5.5 Cargar una url

```javascript
"use strict";
var page = require("webpage").create(),
    url = "http://www.google.es"

console.log("###Load '" + url + "'");
page.viewportSize = { width: 600, height: 600 };
  page.open(url, function(){
    page.render('google.png');
    phantom.exit();
});
```
