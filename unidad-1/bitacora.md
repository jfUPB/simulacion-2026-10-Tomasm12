# Unidad 1

## Bitácora de proceso de aprendizaje


## Bitácora de aplicación 



## Bitácora de reflexión

**Actividad 01**

**La aleatoriedad en el arte generativo**

La aleatoriedad en el arte generativo es importante porque permite experimentar, salir del control total del autor y hacer que cada resultado sea distinto y único.

**Actividad 02**

**Caminatas aleatorias**


**Modifica el código del ejemplo Example 0.1: A Traditional Random Walk.**

``` javascript
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }
step() {
  const r = random(1); 
  
  if (r < 0.4) {          // 40% de probabilidad de ir a la derecha
    this.x++;
  } else if (r < 0.6) {   // 20% de ir a la izquierda
    this.x--;
  } else if (r < 0.9) {   // 30% de ir hacia abajo
    this.y++;
  } else {                // 10% de ir hacia arriba
    this.y--;
  }
}
}
```



**Antes de ejecutar el código, escribe en tu bitácora qué esperas que suceda.**

Espero que el walker no se quede mucho tiempo en su punto de origen, como lo hacía el original, y que tome una dirección más definida, en este caso hacia la derecha y hacia abajo.

**Ejecuta el código y escribe en tu bitácora qué sucedió realmente.**

El walker se quedó un momento en el centro, pero después comenzó a moverse hacia la derecha y poco a poco se fue inclinando hacia abajo.

**Ocurrió lo que esperabas? ¿Por qué crees que sí o por qué crees que no?**

Sí ocurrió lo que esperaba. Aunque pensé que el movimiento hacia la derecha sería un poco más rápido, el comportamiento general fue el esperado y mostró una dirección dominante.

**Actividad 03**

**Distribuciones de probabilidad**

**En tus propias palabras cuál es la diferencia entre una distribución uniforme y una no uniforme de números aleatorios.**
Una distribución uniforme es cuando los números aleatorios tienen la misma probabilidad de aparecer dentro de un rango y no se mantienen en una sola zona, mientras que en una distribución no uniforme algunos valores aparecen con más frecuencia, haciendo que los resultados se alejen más del punto original.

**Modifica el código de la caminata aleatoria para que utilice una distribución no uniforme, favoreciendo el movimiento hacia la derecha.**


```javascript
let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    // Empezamos en el centro de la pantalla
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    strokeWeight(2); // Un poco más grueso para que se vea mejor
    point(this.x, this.y);
  }

  step() {
    // Generamos un número aleatorio entre 0 y 1
    const r = random(1);
    
    if (r < 0.50) {
      this.x++;
    } else if (r < 0.65) {
      this.x--;
    } else if (r < 0.82) {
      this.y--;
    } else {
      this.y++;
    }

    // Opcional: Evitar que el caminante se salga de los bordes
    this.x = constrain(this.x, 0, width - 1);
    this.y = constrain(this.y, 0, height - 1);
  }
}
```

DISTRIBUCIÓN DE PROBABILIDAD:

- 0.00 a 0.50 -> Derecha (50%)

- 0.50 a 0.65 -> Izquierda (15%)

- 0.65 a 0.82 -> Arriba (17.5%)

- 0.82 a 1.00 -> Abajo (17.5%)

**Actividad 04**

**Distribución Normal**
**Crea un nuevo sketch en p5.js que represente una distribución normal.**
**Copia el código en tu bitácora.**
```javascript
function setup() {
  createCanvas(640, 360);
  background(255);
}

function draw() {
 
  let mean = width / 2;      
  let sd = 60;               
  
  //  Generar el valor x (media, desviación)
  let x = randomGaussian(mean, sd);
  
  //puntos se concentren en el centro vertical
  let y = randomGaussian(height / 2, 40);


  noStroke();
  fill(0, 10);
  circle(x, y, 8);
}
```
**Coloca en enlace a tu sketch en p5.js en tu bitácora.**

[ej 4 en p5.js](https://editor.p5js.org/Tomasm12/sketches/ZCs3BKwUR)

**Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.**

<img width="795" height="441" alt="image" src="https://github.com/user-attachments/assets/9e16480c-2239-44f1-849d-d05f822ce7ea" />

**Actividad 05**

**Distribución personalizada: Lévy flight**

**Crea un nuevo sketch en p5.js donde modifiques uno de los ejemplos anteriores y adiciones de Lévy flight.**

**Explica por qué usaste esta técnica y qué resultados esperabas obtener.**

Usé la técnica de Lévy flight porque permite modificar la caminata aleatoria tradicional introduciendo desplazamientos largos poco frecuentes. A diferencia de una caminata uniforme, donde todos los pasos tienen el mismo tamaño
El resultado esperado es que el walker vaya dibujando su recorrido de manera progresiva y, de vez en cuando, realice saltos visibles a través del panel
**Copia el código en tu bitácora.**

```javascript
let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    let r = random(1);
    let stepSize;

    // Lévy flight: pasos largos raros
    if (r < 0.01) {
      stepSize = random(20, 80); // salto grande
    } else {
      stepSize = 1; // paso pequeño
    }

    let choice = floor(random(4));

    if (choice == 0) {
      this.x += stepSize;
    } else if (choice == 1) {
      this.x -= stepSize;
    } else if (choice == 2) {
      this.y += stepSize;
    } else {
      this.y -= stepSize;
    }
  }
}
```

**Coloca en enlace a tu sketch en p5.js en tu bitácora.**

[Codigo Ej 5 en p5.js](https://editor.p5js.org/Tomasm12/sketches/dbXYaTk8r)

**Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.**

<img width="782" height="303" alt="image" src="https://github.com/user-attachments/assets/3f84c2c6-3fb3-4a2e-9e91-5f823375d092" />


**Actividad 06**

**Ruido Perlin**

**Crea un nuevo sketch en p5.js donde los visualices.**
**Explica el concepto qué resultados esberabas obtener.**

El ruido Perlin genera valores pseudoaleatorios que varían de forma continua en el tiempo. nos permite  controlar el tamaño de un objeto mostrando cómo puede producir cambios fluidos  y orgánicos.

Resultados esperados:
Se esperaba obtener una animación fluida donde el círculo cambiara de tamaño sin variaciones tan fuertes. El resultado es una forma que parece expandirse y contraerse de manera natural.

**Copia el código en tu bitácora.**

```javascript
let t = 0;

function setup() {
  createCanvas(400, 400);
  background(255);
}

function draw() {
  background(255, 20);

  let size = noise(t) * 200 + 20;

  noFill();
  stroke(0);
  strokeWeight(2);
  circle(width / 2, height / 2, size);

  t += 0.01;
}

```

**Coloca en enlace a tu sketch en p5.js en tu bitácora.**

[Ej 6](https://editor.p5js.org/Tomasm12/sketches/-m8X7buiz)

**Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.**

<img width="777" height="384" alt="image" src="https://github.com/user-attachments/assets/f03cefb3-48dc-49b0-b4a6-574de5836d25" />

**Actividad 07**

**Creación de obra generativa**

**Un texto donde expliques el concepto de obra generativa.**

Mi obra generativa consiste en un programa que crea partículas en el lienzo cada vez que hago clic con el mouse. Estas partículas se mueven de manera autónoma y siguen la posición del mouse, como si fueran peces siguiendo un punto de referencia.

- Distribución Normal: Para que los tamaños de los puntos sean variados pero naturales.

- Lévy Flight: Para que las partículas den saltos largos y no se queden amontonadas.

- Caminata Aleatoria (con Tendencia): Para que las partículas sigan al mouse.

Copia el código en tu bitácora.

```java script
let walkers = [];

function setup() {
  createCanvas(windowWidth, windowHeight);
  background(0);
}

function draw() {
  background(0, 10); // Rastro suave

  // Crear una partícula donde esté el mouse
  if (mouseIsPressed) {
    walkers.push(new Walker(mouseX, mouseY));
  }

  // Actualizar y dibujar cada partícula
  for (let w of walkers) {
    w.step();
    w.show();
  }
}

class Walker {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    // 1. DISTRIBUCIÓN NORMAL: Tamaños variados (media 5, desviación 2)
    this.size = randomGaussian(5, 2);
  }

  step() {
    // 2. LÉVY FLIGHT: Elegir el tamaño del paso
    let stepSize = random(1) < 0.05 ? random(20, 50) : random(2, 5);

    // 3. CAMINATA CON TENDENCIA: Ir hacia el mouse
    let xDir = random(-1, 1) + (mouseX > this.x ? 0.2 : -0.2);
    let yDir = random(-1, 1) + (mouseY > this.y ? 0.2 : -0.2);

    this.x += xDir * stepSize;
    this.y += yDir * stepSize;
  }

  show() {
    fill(0, 200, 255, 150);
    noStroke();
    circle(this.x, this.y, this.size);
  }
}
```

**Coloca en enlace a tu sketch en p5.js en tu bitácora.**

[EJ 7 en p5.js](https://editor.p5js.org/Tomasm12/sketches/HlD5eYCx3)

**Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.**



<img width="1163" height="1168" alt="image" src="https://github.com/user-attachments/assets/333c58a6-86d4-4fdd-aef3-be8b646f1795" />















