# Descenso de Gradiente / Encuentra el fondo del mar

Un juego para aprender los conceptos detrás del descenso de gradiente explorando las profundidades en busca de un tesoro.

Desarrollado para la exposición I AM AI por IMAGINARY.

## Configuración

Al abrir la aplicación se carga un archivo de configuración. Admite las siguientes claves:

- **defaultLanguage** (cadena, predeterminado: `"es"`): Idioma predeterminado a utilizar. Puede ser reemplazado por la cadena de consulta `lang`.
- **languages** (matriz de cadenas, predeterminado: todos los idiomas presentes en `tr.json`): Selección y orden de códigos de idioma para i18n.
- **useGamepads** (booleano, predeterminado: `verdadero`): Habilita el uso de mandos.
- **useScreenControls** (booleano, predeterminado: `verdadero`): Muestra los controles en pantalla.
- **useKeyboardControls** (booleano, predeterminado: `verdadero`): Controla el juego mediante teclado (jugador 1: <kbd>←</kbd><kbd>↓</kbd><kbd>→</kbd> o respectivamente <kbd>←</kbd><kbd>Espacio</kbd><kbd>→</kbd>; jugador 2: <kbd>A</kbd><kbd>S</kbd><kbd>D</kbd>).
- **botType** (`"ninguno"`, `"aleatorio"`, `"descenso-de-gradiente"`, `"intersección-tangente"` o `nulo`, predeterminado: `nulo`): Establece el tipo de bot a utilizar. Si es `nulo`, permite que el usuario elija.
- **botTypeLabels** (`"dificultad"` o `"estrategia"`, predeterminado: `"dificultad"`): Define el tipo de etiquetas para describir el bot (`"dificultad"`: nivel de dificultad; `"estrategia"`: nombre de la estrategia del bot).
- **showBotTypeTips** (booleano, predeterminado: `falso`): Muestra descripciones extendidas para los tipos de bot. Las sugerencias son opcionales y pueden configurarse en los archivos de traducción con las claves `bot-types.dificultad.<tipo>-tip` o `bot-types.estrategia.<tipo>-tip`.
- **maxPlayers** (entero, predeterminado: 2): Número máximo de jugadores (entre 1 y 4).
- **maxTime** (entero o cadena `"Infinito"`, predeterminado: `"Infinito"`): Tiempo máximo en segundos hasta que finalice el juego.
- **maxProbes** (entero o cadena `"Infinito"`, predeterminado: `"Infinito"`): Número máximo de sondas hasta que finalice el juego.
- **showSeaFloor** (booleano, predeterminado: `falso`): Hace visible el fondo marino desde el inicio.
- **maxDepthTilt** (flotante ≥ 0, predeterminado: `4`): Inclina la generación del fondo marino hacia zonas superficiales [0,1) o profundas (1,∞).
- **map** (matriz o `nulo`, predeterminado: `nulo`): Si es `nulo`, genera automáticamente un mapa. De lo contrario, usa el mapa proporcionado. Consulta [más abajo](#configurar-un-mapa-del-fondo-marino) para detalles.
- **continuousGame** (booleano, predeterminado: `falso`): Omite la pantalla de título y el límite de tiempo, reinicia automáticamente.
- **showDemo** (booleano, predeterminado: `falso`): Alterna entre la pantalla de título y el modo demo.
- **demoDuration** (entero, predeterminado: `18000`): Duración del modo demo en milisegundos.
- **fullScreenButton** (booleano, predeterminado: `verdadero`): Muestra un botón para alternar el modo pantalla completa.
- **languageButton** (booleano, predeterminado: `verdadero`): Muestra un botón para alternar entre los idiomas definidos en `languages`.
- **debugControls** (booleano, predeterminado: `falso`): Muestra datos de depuración para los controles.
- **debugLog** (booleano, predeterminado: `falso`): Muestra mensajes de depuración en la consola.

Por defecto se usa el archivo de configuración `config.json`. Sin embargo, este nombre puede modificarse mediante la variable de consulta `config`, por ejemplo: `index.html?config=config.local.json`.

## Control remoto del juego

El juego añade un objeto `game` al ámbito global.

### Configurar un mapa del fondo marino

Un mapa del fondo marino es simplemente una matriz de números entre 0 (cerca de la superficie) y 1 (lejos de la superficie). La matriz se usa para generar una polilínea desde el lado izquierdo de la pantalla hasta el derecho, con nodos equidistantes según los elementos de la matriz.

Los mapas pueden establecerse mediante `game.setMap(map)`. Por ejemplo, un mapa en forma de V muy simple sería:

```
game.setMap([0, 1, 0]);
```

Un mapa similar a una parábola podría definirse así:

```
const parabola = t => 1 - Math.pow(2 * t - 1, 2);
const crearMapa = (distancia, longitud) => Array.from(
  { length: longitud },
  (_, i) => distancia(i / (longitud - 1))
);
game.setMap(crearMapa(parabola, 100));
```

Al pasar `nulo` como mapa, se vuelve a generar automáticamente uno nuevo cada ronda.

Al iniciar una nueva ronda, el mapa actual se muestra en la consola del navegador. Esto permite almacenarlo y reutilizarlo posteriormente.

Nota: El mapa solo se aplica a nuevas rondas del juego, no a la actual.

### Establecer el modo de juego

El juego consta de los modos `titulo`, `numjugadores` y `jugar`. Puedes cambiar de modo o reingresar al actual mediante `game.setMode(modo)`. Por ejemplo, para cargar un mapa y jugar una nueva ronda:

```
const mapa = ...;
game.setMap(mapa);
game.setMode('jugar');
```

### Mostrar el fondo marino

Si el juego está en modo `jugar`, el fondo marino de la ronda actual puede mostrarse mediante `game.showSeaFloor(animar)`. Si `animar` es `verdadero`, el fondo se revelará gradualmente. De lo contrario, aparecerá inmediatamente. Este método no tiene efecto en otros modos.

## Compilación

Esta aplicación web se construye usando varios lenguajes compilables:

- Las páginas HTML se generan desde plantillas **pug**.
- La hoja de estilos CSS se precompila desde archivos **sass**.
- Los scripts JS se transcompilan desde archivos **es6** (ES2015).

Para aplicar modificaciones es necesaria la recompilación. Debes instalar:

- **node** y **npm**
- **gulp** (instalar globalmente)

Luego ejecuta en la línea de comandos para instalar dependencias:

```
cd src
npm install
```

Tras instalar las dependencias, compila según necesidad:

- **sass (hojas de estilo)**
    ```
    gulp styles
    ```
- **scripts (ES6)**
    ```
    gulp scripts
    ```
- **dependencias (ES6)**
    ```
    gulp dependencies
    ```
- **pug (páginas HTML)**
    ```
    gulp html
    ```
- **todo**
    ```
    npm run build
    ```
    o respectivamente
    ```
    gulp build
    ```
- **vigilar cambios y recompilar automáticamente**
    ```
    gulp watch
    ```

Nota: `gulp html` debe ejecutarse después de `gulp styles`, `gulp scripts` y `gulp dependencies` porque los archivos HTML deben actualizarse para apuntar a los artefactos compilados. `gulp build` ejecuta todas las tareas en orden.

### Servir con recarga automática

```
cd src
npx reload -d .. -w ../index.html -p [puerto libre]
```

### Internacionalización

Para añadir una traducción:

- Determina el código de idioma (ej: `eo` para esperanto).
- Copia un archivo de `tr` (ej: `tr/en.json` → `tr/eo.json`).
- Traduce todos los textos en el nuevo archivo. Conserva los espacios.
- Añade el código del idioma y su autónimo a `tr.json`.
- Incluye tu nombre en la lista de colaboradores.

## Créditos

Desarrollado por Christian Stussak y Eric Londaits, basado en un concepto de Aaron Montag, IMAGINARY gGmbH.

### Traducciones

- Holandés: Jarne Renders
- Inglés: Christian Stussak, Eric Londaits
- Francés: Barbara Knapiak, Daniel Ramos
- Alemán: Christian Stussak, Andreas Daniel Matt
- Español: Daniel Ramos

## Licencia

Copyright (c) 2020 IMAGINARY gGmbH. Licenciado bajo MIT License (ver LICENSE)
