# Unidad 7

## Bitácora de proceso de aprendizaje

**Actividad 01: Ji Lee y la tipografía semántica**

1. Análisis de ejemplos
PARALLEL: Inclina las "L" centrales para que parezcan líneas geométricas que nunca se tocan.

EXIT: Desplaza la "X" hacia un espacio abierto, simulando a alguien saliendo por una puerta.

CLIMB: Escala las letras hacia arriba como si fueran peldaños de una escalera.

SMILE: Curva la "M" hacia abajo para que el ojo vea una boca sonriente.

2. El truco visual
La tipografía refuerza el significado porque deja de ser un texto y se vuelve un objeto. Usa el movimiento, la posición y la forma para que "veas" la acción antes de leerla.

3. Propuestas propias
GOTA: La "O" se estira hacia abajo hasta parecer una lágrima cayendo.

SALTO: La "L" flota mucho más arriba que las demás letras, simulando estar en el aire.

HUECO: La "U" se hace más grande y se hunde, creando un pozo en medio de la palabra.

4. Mi favorita
Me quedo con GOTA. Es interesante porque deforma una letra rígida (la "O") para representar algo líquido y pesado. Es el ejemplo perfecto de cómo la gravedad puede "afectar" a la lectura.

**Actividad 02: Exploración de Matter.js**

Conceptos Fundamentales de Matter.js

- Engine (El Motor): Es el "cerebro" de la simulación. Se encarga de gestionar y calcular la física de la escena paso a paso (la gravedad, las velocidades, las colisiones).

- World (El Mundo): Es el espacio o universo donde existen las cosas. Al mundo es a donde agregas todos tus objetos, paredes y restricciones para que el Motor los simule juntos.

- Bodies (Los Cuerpos): Son los objetos físicos reales (cajas, círculos, polígonos). Pueden ser estáticos (como un suelo que no se mueve por la gravedad) o dinámicos (como una pelota que cae y rebota). Tienen propiedades como masa, fricción y rebote (restitución).

- Constraint (La Restricción): Es una unión física. Sirve para conectar dos cuerpos entre sí, o un cuerpo a un punto fijo en el espacio. Imagínalo como un hilo, un resorte, una cuerda o una bisagra.

- MouseConstraint (Interacción con Ratón): Es un tipo especial de restricción que vincula el puntero del ratón con los cuerpos físicos. Es lo que te permite hacer clic sobre un objeto, arrastrarlo y lanzarlo por la pantalla.

2. Experimentos con p5.js + Matter.js
Para integrar ambas librerías, Matter.js se encarga de las matemáticas (dónde están los objetos) y p5.js se encarga de dibujar esas posiciones en la pantalla.

Experimento A: Caída de letras (Bodies y World)
Este experimento crea un suelo estático y permite que caigan rectángulos dinámicos.

```JavaScript
// Experimento 1: Gravedad y Cuerpos
const Engine = Matter.Engine;
const World = Matter.World;
const Bodies = Matter.Bodies;

let engine;
let world;
let boxes = [];
let ground;

function setup() {
  createCanvas(400, 400);
  engine = Engine.create();
  world = engine.world;
  
  // Cuerpo estático (suelo)
  ground = Bodies.rectangle(200, 390, 400, 20, { isStatic: true });
  World.add(world, ground);
}

function mousePressed() {
  // Crea un cuerpo dinámico donde se hace clic
  let newBox = Bodies.rectangle(mouseX, mouseY, 40, 40);
  boxes.push(newBox);
  World.add(world, newBox);
}

function draw() {
  background(220);
  Engine.update(engine); // Actualiza la física
  
  // Dibuja el suelo
  fill(100);
  rectMode(CENTER);
  rect(ground.position.x, ground.position.y, 400, 20);
  
  // Dibuja las cajas
  fill(255);
  for (let i = 0; i < boxes.length; i++) {
    let b = boxes[i];
    push();
    translate(b.position.x, b.position.y);
    rotate(b.angle);
    rect(0, 0, 40, 40);
    pop();
  }
}
```
<img width="362" height="356" alt="image" src="https://github.com/user-attachments/assets/174785f0-68ee-449a-9ea6-dd1352e305ee" />


Letra Colgante (Constraint y MouseConstraint)

Este experimento ata un objeto al techo y permite interactuar con él usando el ratón.

```JavaScript
// Experimento 2: Péndulo interactivo
const Engine = Matter.Engine;
const World = Matter.World;
const Bodies = Matter.Bodies;
const Constraint = Matter.Constraint;
const Mouse = Matter.Mouse;
const MouseConstraint = Matter.MouseConstraint;

let engine;
let world;
let pendulo;
let mConstraint;

function setup() {
  let canvas = createCanvas(400, 400);
  engine = Engine.create();
  world = engine.world;

  // Cuerpo del péndulo
  pendulo = Bodies.circle(200, 200, 30);
  World.add(world, pendulo);

  // Restricción (Constraint) que une el péndulo al techo
  let union = Constraint.create({
    pointA: { x: 200, y: 50 }, // Punto fijo en el aire
    bodyB: pendulo,            // Cuerpo atado
    length: 150,               // Largo de la cuerda
    stiffness: 1               // Rigidez
  });
  World.add(world, union);

  // Interacción con el ratón (MouseConstraint)
  let canvasMouse = Mouse.create(canvas.elt);
  mConstraint = MouseConstraint.create(engine, { mouse: canvasMouse });
  World.add(world, mConstraint);
}

function draw() {
  background(220);
  Engine.update(engine);

  // Dibuja la línea de la restricción
  stroke(0);
  line(200, 50, pendulo.position.x, pendulo.position.y);

  // Dibuja el cuerpo
  fill(150);
  circle(pendulo.position.x, pendulo.position.y, 60);
}
```
<img width="356" height="354" alt="image" src="https://github.com/user-attachments/assets/edd09098-fe4b-4b60-8a92-e46cdc212c13" />


Comportamiento físico para la palabra
Retomando la propuesta de la palabra GOTA, el comportamiento físico que me interesaría explorar es una alteración de la fricción, la gravedad y la rigidez mediante Constraints.

Me gustaría crear la palabra completa con cuerpos que caen normalmente, pero al impactar contra el suelo estático, en lugar de mantener su forma rígida, las letras se deformen. Esto se lograría construyendo las letras no como sólidos únicos, sino como múltiples partículas pequeñas unidas por resortes elásticos (Constraints suaves). Así, cuando la letra llegue abajo, se aplastará y rebotará con lentitud, imitando la viscosidad y el impacto de un fluido pesado chocando contra una superficie plana.

Para que puedas ver en acción inmediata los conceptos de Matter.js sin necesidad de ejecutar el código localmente, he generado un espacio de pruebas interactivo a continuación.



**Actividad 03: Exploración de audio en p5.js**

Conceptos de Audio-reactividad
Amplitud (Volumen): Es la fuerza o energía del sonido. Se usa para cambios globales (hacer algo más grande o que se mueva más rápido).

Bandas de Frecuencia (FFT): Divide el sonido en Graves, Medios y Agudos. Ideal para que diferentes partes de un diseño reaccionen a instrumentos distintos (ej. el bombo afecta la base, los platillos afectan la punta).

2. Experimentos Simples (p5.js + p5.sound)
Nota: Para estos necesitas la librería p5.sound.js en tu index.html.

Experimento A: El Pulso de la Gravedad (Amplitud)
En este experimento, el volumen del micrófono (o una canción) altera la gravedad del mundo físico.

```JavaScript
let mic, analyzer;
let engine, world;

function setup() {
  createCanvas(400, 400);
  // Configurar audio
  mic = new p5.AudioIn();
  mic.start();
  
  // Configurar física
  engine = Matter.Engine.create();
  world = engine.world;
}

function draw() {
  background(240);
  Matter.Engine.update(engine);

  let volume = mic.getLevel(); // Dato: Amplitud (0 a 1)
  
  // Comportamiento: La gravedad cambia según el volumen
  // Si gritas, las cosas "pesan" más o salen volando hacia arriba
  engine.world.gravity.y = map(volume, 0, 1, 1, -5);

  fill(0, 150, 255);
  ellipse(200, 200, 50 + volume * 500); // Visualización simple del pulso
  text("El volumen altera la gravedad", 20, 20);
}
```


<img width="362" height="340" alt="image" src="https://github.com/user-attachments/assets/8c944941-ff83-4dec-b7df-692c11d07aa5" />

Experimento B: Reacción por Frecuencia (FFT)
Aquí analizamos las frecuencias graves para generar una "fuerza puntual".

```JavaScript
let fft;

function setup() {
  createCanvas(400, 400);
  mic = new p5.AudioIn();
  mic.start();
  fft = new p5.FFT();
  fft.setInput(mic);
}

function draw() {
  background(30);
  let spectrum = fft.analyze();
  let bass = fft.getEnergy("bass"); // Dato: Energía en bajos (0 a 255)

  // Comportamiento: Si detecta un golpe de bajo (bombo), cambia el color
  // y "empuja" un objeto hacia arriba (respuesta puntual).
  if (bass > 200) {
    fill(255, 0, 0);
    rect(150, 100, 100, 100); 
    // Aquí podrías aplicar: Matter.Body.applyForce(cuerpo, ...);
  } else {
    fill(255);
    rect(175, 150, 50, 50);
  }
}
```

<img width="356" height="353" alt="image" src="https://github.com/user-attachments/assets/e8f8a9fa-8483-4360-b94d-06b70f8506ed" />


<img width="351" height="346" alt="image" src="https://github.com/user-attachments/assets/ac205139-6f51-4296-8161-91c3737952fc" />

3. Respuesta sonora para la palabra "GOTA"

Para mi palabra, la respuesta que más me serviría es una Respuesta Continua basada en Bandas de Frecuencia (Graves).

¿Por qué?

Como la idea de la GOTA es que se estire y gotee, una respuesta puntual (como un clic) sería muy brusca. Prefiero una respuesta continua donde:

Las frecuencias graves controlen la tensión de los Constraints (resortes) que forman la letra "O".

A medida que el bajo suena más fuerte y constante, el resorte se estira más y más hacia abajo (simulando que la gota se llena de líquido).

Solo cuando el sonido llega a un pico de amplitud máximo, la "gota" se desprende físicamente.



**Actividad 04: Integración inicial de palabra, física y audio**



Para esta prueba inicial, me he centrado exclusivamente en la letra **"O"** de la palabra **GOTA**, ya que es el núcleo visual donde reside la transformación semántica.

### 1. La Prueba Inicial
He construido la "O" no como un objeto sólido rígido, sino como un **cuerpo blando (soft body)**. Utilicé un anillo de partículas circulares conectadas entre sí por `Constraints` (resortes).

```javascript
// Esquema lógico de la prueba
let particles = []; // Partículas que forman la "O"
let constraints = []; // Resortes que las mantienen unidas

function setup() {
  // ... inicialización de Matter.js ...
  
  // Crear círculo de partículas
  for (let i = 0; i < 12; i++) {
    let angle = map(i, 0, 12, 0, TWO_PI);
    let x = 200 + cos(angle) * 40;
    let y = 200 + sin(angle) * 40;
    particles.push(Bodies.circle(x, y, 5));
  }
  // Conectar con Constraints para que sea elástica
}
```

2. Propiedad física manipulada: Rigidez y Longitud (Stiffness)
   
La propiedad clave aquí es la **elasticidad de los Constraints. En lugar de ser una letra dura, los resortes tienen una rigidez baja.

Manipulé la **posición del punto de anclaje superior: La parte de arriba de la "O" está fija (estática), mientras que el resto de la letra cuelga.

Esto permite que la letra se estire hacia abajo bajo la influencia de la gravedad, pasando de un círculo perfecto a una forma ovoide (gota).

4. El audio: Amplitud a Gravedad y Tensión
   
He vinculado el Volumen (Amplitud) captado por el micrófono de la siguiente manera:

Dato de audio: `mic.getLevel()`.

Efecto físico: Cuando el volumen aumenta, la **gravedad del mundo aumenta** específicamente para las partículas de la "O". 

Resultado visual: Si hay silencio, la "O" recupera un poco su forma circular. Si hay un sonido fuerte (un golpe o un grito), la letra sufre un "tirón" hacia abajo, estirándose violentamente como si la gota estuviera a punto de caer por el peso del sonido.

7. Evaluación de la prueba

Lo que funcionó:

Intención semántica: La deformación orgánica realmente transmite la sensación de un líquido viscoso. El hecho de que la parte superior esté fija y la inferior sea elástica refuerza la idea de una gota "colgando" de la palabra.

Reactividad: Es muy satisfactorio ver cómo la letra "reacciona" físicamente a la voz; se siente viva.

Lo que NO funcionó (y debo corregir):

Legibilidad: Si el sonido es demasiado fuerte, la "O" se estira tanto que deja de parecer una letra y se convierte en una línea vertical. Debo poner un límite (*clamp*) a la extensión de los resortes.

Estabilidad: A veces las partículas colisionan entre sí de forma caótica y la letra se "enreda". Necesito ajustar el *damping* (amortiguación) para que el movimiento sea más fluido y menos nervioso, más parecido al movimiento lento del agua o el aceite.

Próximo paso

Integrar las letras **G, T, A** como cuerpos rígidos estáticos para que sirvan de marco visual, permitiendo que la **O** elástica interactúe con ellas (por ejemplo, que la "O" choque con la "T" al balancearse).


## Bitácora de aplicación 


## Bitácora de reflexión
