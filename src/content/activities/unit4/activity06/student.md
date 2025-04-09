#### Figuras de Lissajous

[MIRA EL RESULTADO ACA](https://editor.p5js.org/jugabriel77/full/_cvocOewR)



``` js
var radius = 26;  // Radio de los círculos
var gutter = 6;   // Espacio entre las figuras

// Variables auto-calculadas
var rows, cols;    // Número de filas y columnas
var size;          // Tamaño del cuadrado que contiene cada figura
var tx, ty;        // Desplazamientos para centrar la tabla en la pantalla
var angle;         // Ángulo usado en cada iteración de dibujo
var back;          // Lienzo fuera de la pantalla para trazar puntos
var maxRatio;      // Determina el color de cada figura de Lissajous

function setup() {
  // Establece el lienzo para ocupar toda la pantalla
  createCanvas(windowWidth, windowHeight);

  // El bucle de dibujo tiene un bucle anidado.
  // En cada iteración, para el par (i,j),
  // se traza la figura de Lissajous para esa combinación de velocidades.
  // Esta figura se contiene en un cuadrado
  // cuyo lado se calcula aquí.
  // La figura se dibuja en el centro de dicho cuadrado,
  // con el radio 'radius' utilizando el "gutter" como espacio de relleno
  size = 2 * (radius + gutter);

  // Se calcula cuántas filas y columnas caben en la pantalla
  rows = floor(windowHeight / size) - 1;
  cols = floor(windowWidth / size) - 1;

  // Desplazamientos para centrar la tabla en la pantalla
  ty = (windowHeight - (rows + 1) * size) / 2;
  tx = (windowWidth - (cols + 1) * size) / 2;
  angle = 0;

  // El lienzo fuera de la pantalla se ajusta exactamente al tamaño de la tabla de figuras
  back = createGraphics((cols + 1) * size, (rows + 1) * size);
  back.background(0);  // Fondo del lienzo fuera de la pantalla
  back.colorMode(HSB);  // Se usa el modo de color HSB para el color
  maxRatio = max(cols, rows);  // La relación máxima posible de la velocidad de las coordenadas
}

function draw() {
  // Se traslada el origen usando tx, ty calculados en setup
  translate(tx, ty);

  // Se renderiza el lienzo fuera de la pantalla
  image(back, 0, 0);

  for (var i = 0; i <= cols; i++) {
    for (var j = 0; j <= rows; j++) {
      // En cada iteración se considera un cuadrado imaginario,
      // con el lado 'size'
      var cx = size * i + size / 2;  // Centro del cuadrado en el eje x
      var cy = size * j + size / 2;  // Centro del cuadrado en el eje y
      var x, y;

      // i==0 y j==0 es el primer espacio vacío, así que se continua
      if (i == 0 && j == 0) continue;

      if (i == 0 || j == 0) {
        // Esto sucede para la fila de encabezado y la columna de encabezado

        // Para la fila de encabezado, j=0, por lo que el factor es i
        // Para la columna de encabezado, i=0, por lo que el factor es j
        var factor = max(i, j);

        // Ahora trazamos el punto polar (radio, factor*ángulo)
        // El coeficiente 'factor' provoca que el mismo ángulo dé
        // diferentes velocidades a través de la fila/columna
        x = radius * cos(angle * factor - HALF_PI);
        y = radius * sin(angle * factor - HALF_PI);

        // Dibujamos el círculo en el lienzo principal
        strokeWeight(1);
        noFill();
        ellipse(cx, cy, 2 * radius, 2 * radius);

        // Dibujamos las líneas perpendiculares
        // Para la columna de encabezado, estas son líneas horizontales,
        // para la fila de encabezado, son líneas verticales
        stroke(255, 75);
        var endX = j == 0 ? cx + x : (cols + 0.9) * size;
        var endY = i == 0 ? cy + y : (rows + 0.9) * size;
        line(cx + x, cy + y, endX, endY);
      } else {
        // Este es el caso para una intersección
        // Tomamos la coordenada x de la fila de encabezado,
        // la coordenada y de la columna de encabezado y trazamos el punto
        x = radius * cos(angle * i - HALF_PI);
        y = radius * sin(angle * j - HALF_PI);

        // Este punto también se traza en el lienzo fuera de la pantalla
        // sin borrar los puntos anteriores. El trazo forma
        // la figura de Lissajous.
        // El color se determina por la relación de "velocidades"
        // de las coordenadas x e y
        ratio = max(i, j) / min(i, j);
        h = map(ratio, 1, maxRatio, 0, 255);
        back.stroke(h, 255, 255);
        back.strokeWeight(2);
        back.point(cx + x, cy + y);
      }

      // Finalmente, dibujamos el punto global calculado con respecto a cx y cy
      strokeWeight(8);
      stroke(255);
      point(cx + x, cy + y);
    }
  }

  // Se incrementa el ángulo para la siguiente iteración
  angle += 0.01;
}
```

![descarga (4)](https://github.com/user-attachments/assets/8fbdd031-5636-4379-887c-ed5fed25df26)


![descarga (3)](https://github.com/user-attachments/assets/eb407198-1f7d-49eb-af57-e7f14c591990)


