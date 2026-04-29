# Unidad 6

## Bitácora de proceso de aprendizaje

**Actividad 01: Un referente para pensar sistemas visuales**

<img width="807" height="857" alt="image" src="https://github.com/user-attachments/assets/682159ce-1293-49ae-b22f-ffa03ffa50bf" />


- Composición:
  - Composición all-over, sin centro ni jerarquía visual.
  - Toda la superficie tiene el mismo peso.
-Densidad:
    - Densidad alta y constante en toda la imagen.
    - No hay zonas vacías.
- Dirección del movimiento:
   - Movimiento orgánico y continuo.
   - Curvas que forman flujos y remolinos.
- Color:
   - Uso monocromático del rojo sobre fondo claro.
   - El color refuerza la lectura de la forma.
- Repetición y variación:
   - Repetición de segmentos de línea similares.
   - Variación en dirección, curvatura y longitud.

Por qué es potente

Es potente porque, usando una forma simple y un solo color, logra una imagen muy compleja y viva. Aunque está hecha con código, se percibe natural, casi como un fenómeno físico.

Hipótesis del sistema

Probablemente usa un flow field basado en ruido (como Perlin Noise), donde muchas líneas cortas siguen direcciones cambiantes dentro de un campo invisible, con reglas que controlan su longitud y separación.

<img width="791" height="1026" alt="image" src="https://github.com/user-attachments/assets/e4a6ecb8-63e0-44be-93c1-895c83d00b88" />


- Composición:
  - Composición basada en una rejilla clara.
  - Elementos separados y organizados.
- Densidad:
  - Densidad media.
  - Uso evidente del espacio negativo entre círculos.
- Color:
  - Paleta amplia y variada.
  - Colores distribuidos de forma equilibrada.
- Ritmo:
  - Ritmo generado por la repetición regular de los círculos.
  - Se rompe visualmente por el cambio de color y textura.
- Repetición y variación:
  - Repetición de la forma circular.
  - Variación en color y patrón interno.

Por qué es potente

Funciona porque combina orden y variación. La estructura es clara, pero cada círculo es diferente, lo que invita a mirar cada uno con atención.

Hipótesis del sistema

Seguramente parte de una rejilla regular con pequeñas variaciones en posición, color y patrón interno. Cada círculo sigue reglas similares, pero con parámetros cambiantes que generan diversidad dentro del mismo sistema.


**Actividad 02: Agentes autónomos y steering forces**

¿Qué es un agente autónomo?

Es una entidad con intención propia. A diferencia de una partícula que solo flota, el agente observa su entorno y decide hacia dónde moverse para cumplir un objetivo (como perseguir algo o huir).

¿Qué es una steering force?

Es una fuerza de ajuste. Es el cálculo matemático que permite a un agente girar de forma fluida. Se obtiene restando lo que el agente está haciendo de lo que querría estar haciendo (Fuerza = Velocidad Deseada - Velocidad Actual).

3. Diferencia con la gravedad o el viento

Gravedad/Viento: Son fuerzas externas e inevitables. El objeto no decide recibirlas; simplemente le ocurren.Steering Force: Es una fuerza interna. El agente la genera él mismo para "maniobrar". Es voluntad propia versus leyes físicas externas.

4. ¿Por qué son útiles para el diseño visual?
   
Porque permiten crear comportamiento y personalidad, no solo movimiento físico.Hacen que los elementos parezcan vivos.Permiten que el arte sea reactivo (que los elementos "sientan" al usuario).Generan curvas orgánicas y naturales que son imposibles de lograr con simples rebotes o caídas rectas


**Actividad 03: Flow Fields**

Construcción y Funcionamiento

¿Cómo está construido?:

Como una rejilla de celdas (2D Array). El lienzo se divide en columnas y filas según una resolución específica.

¿Qué representa cada celda?:

Un vector unitario. Este vector indica la dirección "deseada" en la que debería moverse cualquier agente que pase por esa zona.

¿Cómo consulta el agente el campo?: 

El agente toma su posición (x, y) y la divide por la resolución de la rejilla. Por ejemplo, si el agente está en x=45 y la resolución es 20, el sistema sabe que debe mirar el vector en la columna 2 (45 / 20 = 2.25, redondeado hacia abajo)

¿Cómo se convierte en movimiento?: 

El vector obtenido se convierte en la Velocidad Deseada. El agente aplica la fórmula de Reynolds: Steering = Velocidad\ Deseada - Velocidad\ Actual.

**Parámetros ImportantesResolución**

(r): Define qué tan "fino" o "grueso" es el campo. Una resolución baja (pocos píxeles por celda) crea cambios de dirección muy detallados.
  
Maxspeed: La velocidad máxima de los agentes. Determina qué tan rápido fluye el sistema.

Maxforce: La fuerza máxima de giro. Si es baja, los agentes tardan en girar y crean curvas amplias; si es alta, siguen el flujo de forma rígida y nerviosa.

Cantidad de agentes: Define la densidad visual. Muchos agentes revelan mejor la forma total del flujo (como el humo o el agua).

**Realiza al menos una modificación y analiza el efecto visual que produce.**

```js
    let xoff = 0;
    for (let i = 0; i < this.cols; i++) {
      let yoff = 0;
      for (let j = 0; j < this.rows; j++) {
        

        let angle = map(noise(xoff, yoff), 0, 1, 0, TWO_PI);
        this.field[i][j] = p5.Vector.fromAngle(angle);

        yoff += 0.01; 
      }
      xoff += 0.01;
    }
  }
```

Modificación: Cambiar el incremento de los offsets en init() (de 0.1 a 0.01 o a 0.5).

Análisis del efecto: * Si el incremento es muy pequeño (0.01), el ruido de Perlin cambia muy lento; el campo se verá muy suave, casi como una corriente de río tranquila.

Si es muy grande (0.5), el campo se vuelve caótico y ruidoso; los agentes parecerán moscas atrapadas en una tormenta, moviéndose en direcciones erráticas.



¿Qué tipo de movimiento produce?: 

Un movimiento fluido, coordinado y orgánico. No se siente como máquinas, sino como seres vivos o partículas en un medio líquido.

¿Qué sensaciones sugiere?: 

Continuidad, armonía y un "caos organizado". Da la sensación de que hay una fuerza invisible (como el viento o el magnetismo) guiando a los agentes.

¿En qué pieza musical funcionaría?:

En música Ambient o Minimalista (estilo Brian Eno o Philip Glass), donde pequeñas variaciones crean una textura rica y constante.

También en Jazz experimental, por la forma en que los agentes parecen improvisar trayectorias dentro de una estructura establecida.

**Actividad 04: Flocking**

Separación: 
Evitar chocar. El agente calcula qué tan cerca están sus vecinos inmediatos y genera una fuerza de dirección (steering) para alejarse de ellos. Garantiza que no se agrupen en un solo punto.

Alineación: 

Copiar la dirección. El agente observa la velocidad y dirección promedio de los vecinos que lo rodean y ajusta su propia trayectoria para moverse en paralelo a ellos.

Cohesión: 

Mantenerse unidos. El agente calcula el "centro de masa" (la posición promedio) de sus vecinos cercanos y genera una fuerza que lo empuja hacia ese centro, evitando quedarse rezagado o aislado.


Parámetros que controlan las reglas

En el código de Shiffman, estas reglas están controladas matemáticamente por dos tipos de parámetros principales:

Los Pesos (Multipliers): Las líneas sep.mult(1.5), ali.mult(1.0) y coh.mult(1.0). Definen qué tanta importancia le da el agente a cada regla.

El Radio de Percepción: Variables como desiredSeparation = 25 o neighborDistance = 50. Determinan qué tan "lejos" puede ver el agente para considerar a otro como su vecino.

Modificación del sistema y Comportamiento Emergente

Si alteramos los pesos, el sistema colectivo muta por completo:

Modificación 1 (Mucha Separación, poca Cohesión/Alineación): Si ponemos la Separación en 3.0 y las otras en 0.1, el comportamiento es disperso y nervioso. Se asemeja al caos de un enjambre de moscas, donde el pánico por no chocar domina la escena.

Modificación 2 (Mucha Cohesión y Alineación, poca Separación): Si ponemos Cohesión en 2.0, Alineación en 2.0 y Separación en 0.5, el grupo se vuelve extremadamente compacto, estable y fluido. Parecen una gota de mercurio moviéndose unida o un banco de peces asustado que se comprime para defenderse.

Atmósfera visual y relación musical

Atmósfera visual: El flocking produce una sensación profundamente hipnótica e inquietante a la vez, porque es vida artificial pura. Nuestro cerebro reconoce un movimiento biológico, intencional y orgánico (emergencia) naciendo de formas geométricas frías.

Relación musical: Este algoritmo brillaría en una pieza de IDM (Intelligent Dance Music), Techno Textural o música ambiental reactiva. Podrías mapear frecuencias específicas a los parámetros: por ejemplo, que un golpe de bajo (kick) fuerte aumente temporalmente la Separación (creando una explosión dispersa) y que los pads atmosféricos o sintetizadores sostenidos suban la Cohesión y Alineación, volviendo a unir la bandada en un flujo armónico.

**Actividad 05: Comparar algoritmos como herramientas de diseño**

Comparativa: Flow Fields vs. Flocking

**Movimiento:**

Flow Fields: Movimiento de "río". Las partículas se dejan llevar por una corriente invisible.

Flocking: Movimiento de "bandada". Los agentes se mueven por su relación con los demás.

**Control Visual:**

Flow Fields: Alto. Tú decides el mapa y las partículas lo obedecen.

Flocking: Bajo. Tú pones las reglas y el grupo hace lo que quiere.

**Nivel de Emergencia (Sorpresa):**

Flow Fields: Bajo. No hay sorpresas, las partículas no interactúan entre sí.

Flocking: Muy alto. Aparecen comportamientos grupales que tú no programaste directamente.

**Sensación:**

Flow Fields: Armonía, naturaleza inanimada (viento, agua), calma.

Flocking: Vida, instinto, nerviosismo, grupo social.

**Ventajas:**

Flow Fields: Es muy rápido de procesar y siempre se ve ordenado y estético.

Flocking: Se siente vivo y orgánico; es mucho más dinámico.

**Limitaciones:**

Flow Fields: Puede verse repetitivo o "muerto" si no cambias el campo.

Flocking: Consume mucha memoria si hay muchos agentes y es difícil de controlar.

**¿Cuál algoritmo usar según el tipo de canción?**

Contemplativa: Flow Fields. Porque es suave, predecible y transmite paz, como ver el movimiento del agua.

Agresiva: Flocking. Porque al subir la Separación, los agentes parecen chocar y rebotar con violencia, creando un caos visual que encaja con ritmos fuertes.

Melancólica: Flow Fields. Porque puedes hacer que las partículas caigan lentamente como lluvia o se desvanezcan con suavidad, sugiriendo tristeza.

Eufórica: Flocking. Porque cuando todos los agentes se Alinean y se unen con mucha velocidad, crean una explosión de movimiento coordinado que transmite mucha energía.

## Bitácora de aplicación 


**Actividad 06: Diseño de un instrumento visual para un tema musical**

**Concepto visual**

quiero hacer un cielo conestrella y la luna en medio es una ecena en la nmoche asi que colocaria pequeñas particulas o luciernagas/Will-o'-Wisp, quiero dar una sencacion de traquilidad y paz 

** Relacion con team musical**

la conexion es que la musica es la que determina la intensidad del momento, osea que esta hara que se sacudan mas las particulas, que la luna de un brillo mas fuerte y que salgan mas particulas

**Bocetos.**

<img width="1862" height="1027" alt="image" src="https://github.com/user-attachments/assets/412c3596-e846-457f-bba1-4d875c99e693" />
<img width="1909" height="1045" alt="image" src="https://github.com/user-attachments/assets/3395dfbd-307c-4242-ab30-723f3fa0ef0d" />
<img width="1909" height="1048" alt="image" src="https://github.com/user-attachments/assets/812c3c8e-6808-4b34-96a3-5640011558b3" />

**Moodboard o referencias.**

<img width="512" height="512" alt="image" src="https://github.com/user-attachments/assets/fe2fc656-4551-46d3-907e-1fca7042f864" />
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5334cdc9-ade4-415b-a43b-89a1c4e1fbb2" />
<img width="725" height="371" alt="image" src="https://github.com/user-attachments/assets/3efa673b-e00c-44a1-b16f-9edae58dab28" />

<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/e48665c5-e452-4ae2-9429-587256bd1d3e" />
<img width="1184" height="630" alt="image" src="https://github.com/user-attachments/assets/91d96662-590c-47a6-a0b7-dc5d8dd2fcef" />
<img width="729" height="331" alt="image" src="https://github.com/user-attachments/assets/5b6f7f93-0893-4743-9a53-2503769cf6c8" />


**Mapa de decisiones**

Fondo con rastro (Alpha): Decidí no limpiar el fondo por completo para dejar una estela suave. Esto aporta una sensación de fluidez y paso del tiempo, reforzando la paz del concepto.

Object Pooling (Piscina de objetos): Implementé este sistema para manejar miles de partículas sin caídas de FPS. La decisión técnica permite que el "enjambre" sea denso y envolvente sin romper la inmersión.

Uso de lerpColor: Los cambios de color no son instantáneos, sino graduales. Esto evita saltos visuales bruscos que romperían la atmósfera de tranquilidad.

Glow de la Luna: El uso de shadowBlur en la luna central actúa como el "corazón" del sistema, dando un punto focal claro al espectador.

**Mapa de interpretación **

Mouse (Perturbación): El movimiento del ratón ahuyenta a las partículas y a las luciérnagas. El usuario actúa como un viento que despeja el cielo.

Barra Espaciadora (Clímax): Activa una fuerza centrífuga (explosionForce). Se debe usar en los momentos de mayor explosión sonora de la canción para dispersar todo el sistema hacia los bordes.

Flechas (Mood de la Luna): Cambian el ciclo cromático de la luna para pasar de una noche gélida (azul) a una noche mística (rojo pálido).

Teclas A / D (Energía de Partículas): Permiten al intérprete cambiar la "temperatura" visual de la aurora, sincronizándola con el sentimiento de la letra o la melodía.

**Justificación del algoritmo elegido**

Flow Fields (Campos de flujo): Elegí este algoritmo porque permite un movimiento orgánico y "curvo". A diferencia de un movimiento lineal, el flow field simula las corrientes de aire invisibles de la noche.

Flocking / Boids (Enjambre): Lo apliqué a los Will-o'-Wisp (luciernagas) para que tengan un comportamiento social. No se mueven solas, sino que se acompañan, lo que genera una sensación de ecosistema vivo y no de simples puntos rebotando.

Construcción de la Luna y Glow: La luna se diseñó como el núcleo del instrumento. Para lograr el efecto de irradiación mística sin sacrificar el rendimiento, utilicé drawingContext.shadowBlur y shadowColor. Esta técnica permite que la luna no sea un círculo plano, sino una fuente de luz que "sangra" hacia el fondo, creando una atmósfera de profundidad espacial.

Sincronización Rítmica (Maquinaria Sonora): El sistema no es una animación automática; "escucha" la música en tiempo real. Utilicé un mapeo de bandas de frecuencia (FFT) y amplitud:

**Explicación de la relación audio-visual **

Frecuencias Bajas (Bass): Mapeadas al tamaño de la luna y al grosor (strokeWeight) de las partículas. El bajo es el "pulso" físico de la pieza.

Frecuencias Medias (Mid): Controlan la velocidad del viento y cuántas partículas nacen. A más intensidad en la voz o instrumentos medios, más poblado y rápido se vuelve el cielo.

Frecuencias Altas (Treble): Afectan el brillo de las estrellas y el tamaño de los fuegos fatuos. Los agudos representan los destellos y detalles finos.

** Evidencia del uso de IA**

la ia se utilizo para general una primera vercion, en esa se trabajo y depues ayudo a optimizar y corregir unos errores 

**Código fuente.**

```js
/**
 * ACTIVIDAD 06: Instrumento Visual - Nod-Krai (Enjambre Cinético Mejorado)
 */

let particles = []; // Partículas activas
let particlePool = []; // Piscina de reciclaje para optimizar memoria
let flock = [];
let stars = []; // Capa de estrellas de fondo
let flowfield;
let song, fft, amp;
let resolution = 15;
let maxParticles = 3500;

let currentMoonCol, targetMoonCol;
let moonColorIndex = 0;
let moonPalette = [];

let currentPartCol, targetPartCol;
let partColorIndex = 0;
let partPalette = [];

let lerpSpeed = 0.05; 
let explosionForce = 0; 

function preload() {
  song = loadSound('Nood Krai song.mp3'); 
}

function setup() {
  createCanvas(windowWidth, windowHeight);
  fft = new p5.FFT();
  amp = new p5.Amplitude();
  flowfield = new FlowField(resolution);

  // --- CONFIGURACIÓN DE PALETAS ---
  moonPalette = [
    color(255, 255, 250),
    color(100, 200, 255),
    color(200, 70, 80) 
  ];
  currentMoonCol = moonPalette[0];
  targetMoonCol = moonPalette[0];

  partPalette = [
    color(0, 255, 200),
    color(255, 100, 200),
    color(255, 240, 150) 
  ];
  currentPartCol = partPalette[0];
  targetPartCol = partPalette[0];

  // --- INICIALIZACIÓN DE ELEMENTOS ---
  
  // 1. Estrellas de fondo (200 fijas)
  for (let i = 0; i < 200; i++) {
    stars.push(new Star());
  }

  // 2. Pool de Partículas Aurora
  for (let i = 0; i < maxParticles; i++) {
    particlePool.push(new AuroraParticle());
  }
  
  for (let i = 0; i < 1000; i++) {
    if (particlePool.length > 0) {
      let p = particlePool.pop();
      p.reset();
      particles.push(p);
    }
  }
  
  // 3. Fuegos Fatuos (Flocking)
  for (let i = 0; i < 80; i++) {
    flock.push(new Wisp(random(width), random(height)));
  }
  
  background(0);
}

function draw() {
  fft.analyze();
  let volume = amp.getLevel();
  let bass = fft.getEnergy("bass");
  let mid = fft.getEnergy("mid");
  let treble = fft.getEnergy("treble");

  currentMoonCol = lerpColor(currentMoonCol, targetMoonCol, lerpSpeed);
  currentPartCol = lerpColor(currentPartCol, targetPartCol, lerpSpeed);

  let backgroundAlpha = map(volume, 0, 0.5, 30, 15);
  background(0, backgroundAlpha);

  // --- 1. ESTRELLAS (FONDO) ---
  for (let s of stars) {
    s.show(treble);
  }

  // --- 2. LUNA ---
  drawMoon(bass, volume);

  flowfield.update(mid);

  // --- 3. PARTÍCULAS AURORA (OBJECT POOLING) ---
  let spawnRate = floor(map(mid, 100, 255, 0, 15, true));
  for (let i = 0; i < spawnRate; i++) {
    if (particlePool.length > 0) {
      let p = particlePool.pop();
      p.reset();
      particles.push(p);
    }
  }
  
  while (particles.length > maxParticles) {
    let oldParticle = particles.shift();
    particlePool.push(oldParticle);
  }

  for (let p of particles) {
    p.avoidMouse(); 
    if (explosionForce > 0.1) p.explode(); 
    p.follow(flowfield, volume);
    p.update(volume);
    p.show(bass, mid, currentPartCol);
  }

  // --- 4. FUEGOS FATUOS ---
  for (let w of flock) {
    w.avoidMouse(); 
    if (explosionForce > 0.1) w.explode(); 
    w.applyBehaviors(flock, treble);
    w.update();
    w.show(treble, bass);
  }

  explosionForce *= 0.9; 
}

// --- CLASE ESTRELLA ---
class Star {
  constructor() {
    this.pos = createVector(random(width), random(height));
    this.size = random(0.5, 2.5);
    this.angle = random(TWO_PI); // Para el parpadeo
  }

  show(treble) {
    this.angle += 0.05;
    // El brillo base oscila y se intensifica con los agudos
    let blink = map(sin(this.angle), -1, 1, 50, 200);
    let musicBoost = map(treble, 0, 255, 0, 100);
    
    stroke(255, blink + musicBoost);
    strokeWeight(this.size);
    point(this.pos.x, this.pos.y);
  }
}

// --- RESTO DE CLASES Y FUNCIONES ---

function drawMoon(bass, vol) {
  push();
  translate(width / 2, height / 2);
  let scaleFactor = map(vol, 0, 0.6, 1.1, 1.5);
  let glowAlpha = map(vol, 0, 0.5, 20, 80); 
  let glowSize = map(bass, 0, 255, 50, 200);
  noStroke();
  drawingContext.shadowBlur = glowSize / 3;
  drawingContext.shadowColor = currentMoonCol;
  fill(red(currentMoonCol), green(currentMoonCol), blue(currentMoonCol), glowAlpha);
  ellipse(0, 0, (180 + glowSize) * scaleFactor);
  fill(currentMoonCol);
  ellipse(0, 0, 160 * scaleFactor); 
  pop();
}

class AuroraParticle {
  constructor() {
    this.pos = createVector(0, 0);
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
    this.maxSpeedBase = 2;
  }
  reset() {
    this.pos.set(random(width), random(height));
    this.vel.set(random(-1, 1), random(-1, 1));
    this.acc.set(0, 0);
  }
  explode() {
    let center = createVector(width/2, height/2);
    let dir = p5.Vector.sub(this.pos, center);
    let v = random(0.7, 1.3);
    dir.setMag(explosionForce * 2 * v);
    this.acc.add(dir);
  }
  avoidMouse() {
    let m = createVector(mouseX, mouseY);
    if (p5.Vector.dist(this.pos, m) < 150) {
      this.acc.add(p5.Vector.sub(this.pos, m).setMag(0.8));
    }
  }
  follow(field, vol) {
    let force = field.lookup(this.pos);
    force.mult(map(vol, 0, 0.5, 1, 3.5));
    this.acc.add(force);
  }
  update(vol) {
    let speedLimit = this.maxSpeedBase + map(vol, 0, 0.5, 0, 5);
    this.vel.add(this.acc);
    this.vel.limit(speedLimit);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.edges();
  }
  show(bass, mid, col) {
    let alpha = map(mid, 0, 255, 40, 200);
    stroke(red(col), green(col), blue(col), alpha);
    strokeWeight(map(bass, 0, 255, 0.5, 3.5));
    point(this.pos.x, this.pos.y);
  }
  edges() {
    if (this.pos.x > width) this.pos.x = 0; if (this.pos.x < 0) this.pos.x = width;
    if (this.pos.y > height) this.pos.y = 0; if (this.pos.y < 0) this.pos.y = height;
  }
}

class FlowField {
  constructor(r) {
    this.resolution = r;
    this.cols = floor(width / r) + 1;
    this.rows = floor(height / r) + 1;
    this.field = new Array(this.cols * this.rows);
    this.zoff = 0;
  }
  update(energy) {
    let xoff = 0;
    for (let i = 0; i < this.cols; i++) {
      let yoff = 0;
      for (let j = 0; j < this.rows; j++) {
        let angle = noise(xoff, yoff, this.zoff) * TWO_PI * 2;
        this.field[i + j * this.cols] = p5.Vector.fromAngle(angle).setMag(0.5);
        yoff += 0.1;
      }
      xoff += 0.1;
    }
    this.zoff += map(energy, 0, 255, 0.005, 0.04);
  }
  lookup(lookup) {
    let col = floor(constrain(lookup.x / this.resolution, 0, this.cols - 1));
    let row = floor(constrain(lookup.y / this.resolution, 0, this.rows - 1));
    return this.field[col + row * this.cols].copy();
  }
}

class Wisp {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D();
    this.acc = createVector(0, 0);
    this.maxSpeed = 3.0;
    this.maxForce = 0.1;
  }
  explode() {
    let center = createVector(width/2, height/2);
    let dir = p5.Vector.sub(this.pos, center);
    dir.setMag(explosionForce * random(0.8, 1.2));
    this.applyForce(dir);
  }
  avoidMouse() {
    let m = createVector(mouseX, mouseY);
    if (p5.Vector.dist(this.pos, m) < 120) {
      this.applyForce(p5.Vector.sub(this.pos, m).setMag(0.6));
    }
  }
  applyBehaviors(boids, energy) {
    this.applyForce(this.separate(boids).mult(1.5));
    this.applyForce(this.align(boids).mult(1.0));
    this.applyForce(this.cohere(boids).mult(0.5));
  }
  applyForce(f) { this.acc.add(f); }
  update() {
    this.vel.add(this.acc);
    this.vel.limit(this.maxSpeed);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.borders();
  }
  show(treble, bass) {
    let size = map(treble, 0, 255, 3, 10);
    push();
    noStroke();
    fill(255, map(bass, 0, 255, 30, 80)); 
    ellipse(this.pos.x, this.pos.y, size * 2.5);
    fill(255, map(bass, 0, 255, 180, 255));
    ellipse(this.pos.x, this.pos.y, size);
    pop();
  }
  separate(boids) {
    let steer = createVector(0, 0);
    let count = 0;
    for (let other of boids) {
      let d = p5.Vector.dist(this.pos, other.pos);
      if (d > 0 && d < 30) {
        steer.add(p5.Vector.sub(this.pos, other.pos).normalize().div(d));
        count++;
      }
    }
    if (count > 0) steer.div(count);
    if (steer.mag() > 0) steer.setMag(this.maxSpeed).sub(this.vel).limit(this.maxForce);
    return steer;
  }
  align(boids) {
    let sum = createVector(0, 0);
    let count = 0;
    for (let other of boids) {
      let d = p5.Vector.dist(this.pos, other.pos);
      if (d > 0 && d < 60) { sum.add(other.vel); count++; }
    }
    if (count > 0) return sum.div(count).setMag(this.maxSpeed).sub(this.vel).limit(this.maxForce);
    return createVector(0, 0);
  }
  cohere(boids) {
    let sum = createVector(0, 0);
    let count = 0;
    for (let other of boids) {
      let d = p5.Vector.dist(this.pos, other.pos);
      if (d > 0 && d < 60) { sum.add(other.pos); count++; }
    }
    if (count > 0) {
      let desired = p5.Vector.sub(sum.div(count), this.pos).setMag(this.maxSpeed);
      return p5.Vector.sub(desired, this.vel).limit(this.maxForce);
    }
    return createVector(0, 0);
  }
  borders() {
    if (this.pos.x < 0) this.pos.x = width; if (this.pos.y < 0) this.pos.y = height;
    if (this.pos.x > width) this.pos.x = 0; if (this.pos.y > height) this.pos.y = 0;
  }
}

function keyPressed() {
  if (keyCode === RIGHT_ARROW) {
    moonColorIndex = (moonColorIndex + 1) % moonPalette.length;
    targetMoonCol = moonPalette[moonColorIndex];
  } else if (keyCode === LEFT_ARROW) {
    moonColorIndex = (moonColorIndex - 1 + moonPalette.length) % moonPalette.length;
    targetMoonCol = moonPalette[moonColorIndex];
  }

  if (key === ' ') explosionForce = 15; 

  if (key === 'd' || key === 'D') {
    partColorIndex = (partColorIndex + 1) % partPalette.length;
    targetPartCol = partPalette[partColorIndex];
  } else if (key === 'a' || key === 'A') {
    partColorIndex = (partColorIndex - 1 + partPalette.length) % partPalette.length;
    targetPartCol = partPalette[partColorIndex];
  }
}

function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
  flowfield = new FlowField(resolution);
}

function mousePressed() {
  song.isPlaying() ? song.pause() : song.play();
}
```

**Enlace al sketch.**

[Actividad 6](https://editor.p5js.org/Tomasm12/sketches/oMVjt-_RL)

**Capturas o registros de momentos importantes de la pieza.**


<img width="1244" height="1141" alt="image" src="https://github.com/user-attachments/assets/930cbcab-955a-4f83-9598-fbd131a2e1af" />
<img width="1215" height="1125" alt="image" src="https://github.com/user-attachments/assets/0ec8db0f-15db-4e7f-8bb2-3bc0acd52ba7" />
<img width="1221" height="1122" alt="image" src="https://github.com/user-attachments/assets/d12c81f8-70d8-4a9f-8b85-71b3b4ae442a" />


## Bitácora de reflexión
