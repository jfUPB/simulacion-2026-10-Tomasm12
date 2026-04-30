# Unidad 4

## Bitácora de proceso de aprendizaje

**Actividad 01** 

me pareció chévere cómo usando algo de física logra que las formas se vean casi como si estuvieran vivas. Aunque todo viene de fórmulas y movimientos repetitivos, al final se siente muy natural y fluido.

También me gustó cómo usa el sonido para que todo tenga más armonía. La mezcla entre lo visual y el audio hace que la experiencia se sienta más completa y con ritmo.

**Actividad 02** 

**Qué está pasando en la simulación? ¿Cuál es la interacción?**

En la simulación vemos una línea con dos círculos en los extremos que está girando.
Esto pasa porque la variable angle aumenta en cada frame, entonces la figura rota continuamente.

La interacción ocurre cuando el usuario presiona una tecla. Cuando eso pasa, el ángulo aumenta un poco más y la figura gira más.

**Nota que en cada frame se está trasladando el origen del sistema de coordenadas al centro de la pantalla. ¿Por qué crees que se hace esto?**

Normalmente el punto (0,0) está en la esquina superior izquierda.

Con translate(width / 2, height / 2) se mueve ese origen al centro del canvas.

Se hace esto para que la figura gire desde el centro de la pantalla y no desde la esquina.

**Cuál es la relación entre el sistema de coordenadas y la función rotate().**

La función rotate() no mueve el dibujo, mueve el papel.

Imagina que pones el alfiler en el centro, giras la hoja un poco, y luego dibujas la línea.

Como la hoja ya está inclinada, lo que dibujes saldrá inclinado.


**Observa que al dibujar los elementos gráficos parece que se está dibujando en la posición (0, 0) del sistema de coordenadas. ¿Por qué crees que se hace esto? y ¿Por qué aunque en cada frame se hace lo mismo, los elementos gráficos rotan?**

Se dibuja usando (0,0) porque ese punto se convirtió en el centro de la pantalla después de usar translate(width/2, height/2). Así es más fácil hacer que las figuras giren alrededor del centro.

Aunque en cada frame se dibuja lo mismo, los elementos rotan porque antes se usa rotate(angle). Esta función gira el sistema de coordenadas, entonces cuando se dibujan las figuras ya están giradas.


**Identifica el marco motion 101. ¿Qué es lo que se está haciendo en este marco?**

En este caso el objeto (mover) tiene:

- posición

- velocidad

En cada frame el programa:

1. Actualiza la posición (update()).

2. Revisa si toca los bordes (checkEdges()).

3. Dibuja el objeto en pantalla (show()).



**¿Qué hace la función heading()?**

heading() calcula el ángulo del vector de velocidad.

Ese ángulo indica hacia dónde se está moviendo el objeto.
Luego ese ángulo se usa en rotate(angle) para que el rectángulo apunte en la misma dirección en la que se mueve.

**¿Qué hace la función push() y pop()? Realiza algunos experimentos para entender su funcionamiento.**

push() guarda el estado actual del sistema de coordenadas.

pop() lo restaura.

Esto se usa para que las transformaciones como translate() y rotate() solo afecten a ese objeto y no a todo lo demás que se dibuje después.


**¿Qué hace rectMode(CENTER)? Realiza algunos experimentos para entender su funcionamiento.**

Por defecto, p5.js dibuja los rectángulos desde la esquina superior izquierda.

rectMode(CENTER): Cambia esto para que las coordenadas que le das (0, 0) sean el centro exacto del rectángulo.

**¿Cuál es la relación entre el ángulo de rotación y el vector de velocidad? Trata de dibujar en un papel el vector de velocidad y cómo se relaciona con el ángulo de rotación y la operación de traslación y rotación.**

1. El Vector de Velocidad: Si dibujas una flecha desde el centro del objeto hacia donde se mueve, ese es su vector de velocidad.

2. El Ángulo: heading() mide el ángulo entre esa flecha y la horizontal (el eje X).

3. La Transformación:

- Primero, trasladas el origen a position.x, position.y (donde está el objeto ahora).

- Luego, rotas el papel con el ángulo que te dio la velocidad.

- Finalmente, dibujas el rectángulo en (0,0).

**Actividad 03**

```java script
let x, y; // Posición
let vx = 0; // Velocidad en X

function setup() {
  createCanvas(600, 200);
  x = width / 2;
  y = height / 2;
}

function draw() {
  background(220);

  // 1. INPUT: Si presionas flechas, cambia la velocidad
  if (keyIsDown(LEFT_ARROW))  vx = -5;
  if (keyIsDown(RIGHT_ARROW)) vx = 5;
  
  // 2. MOVIMIENTO: La posición cambia según la velocidad
  x += vx;

  // 3. DIBUJO CON ROTACIÓN
  push();
  translate(x, y); // Mover el origen al vehículo
  
  // Si vx es positivo mira a la derecha (0°), si es negativo a la izquierda (180°)
  let angulo = (vx < 0) ? PI : 0; 
  rotate(angulo);

  // Dibujamos el triángulo centrado
  triangle(15, 0, -15, -10, -15, 10); 
  pop();

  // Frenado simple para que no se escape
  vx *= 0.95; 
}
```


Primero creé variables para la posición (x, y) y la velocidad (vx) del vehículo. En cada frame del programa la posición cambia según la velocidad, lo que hace que el vehículo se mueva por la pantalla.

Después dibujé el vehículo usando un triángulo. Para colocarlo en la pantalla utilicé translate(x, y), que mueve el sistema de coordenadas a la posición del vehículo.

También usé rotate() para que el triángulo apunte hacia la dirección en la que se está moviendo. Si el vehículo se mueve a la derecha el triángulo mira a la derecha, y si se mueve a la izquierda rota 180 grados para mirar hacia ese lado.

Finalmente programé las flechas del teclado para controlar el movimiento: la flecha izquierda mueve el vehículo a la izquierda y la flecha derecha lo mueve a la derecha.

**Actividad 04** 

**¿Qué modificación hay que hacer al motion 101 cuando se quiere agregar fuerzas acumulativas?**

Cuando usamos fuerzas, cada fuerza modifica la aceleración.
Pero en cada frame del programa pueden actuar muchas fuerzas (gravedad, viento, atracción, etc.).

Por eso se hace esta modificación al final del update():

this.acceleration.mult(0);
¿Por qué es necesario?

Porque las fuerzas solo deben aplicarse durante un frame.

Si no reiniciamos la aceleración:

- Las fuerzas seguirían acumulándose

- El objeto aceleraría cada vez más sin control

Entonces el proceso correcto es:

1. Aplicar fuerzas → applyForce()

2. Actualizar movimiento

3. Resetear aceleración

**dentifica dónde está el Attractor en la simulación. Cambia el color de este.**

El Attractor se crea en el setup() con attractor = new Attractor(); y se dibuja en el draw() con attractor.display();.

Flujo correcto:
fuerzas → aceleración → velocidad → posición → reset aceleración


Para cambiar su color, busca en la función show() de la clase Attractor y modifica el fill():


```JavaScript
// Dentro de la clase Attractor
show() {
  stroke(0);
  // Cambia el color aquí. Ejemplo: un azul vibrante
  fill(0, 150, 255, 200); 
  circle(this.position.x, this.position.y, this.mass * 2);
}
```

**Observa que el Attractor tiene dos atributos this.dragging y this.rollover. Estos atributos no se modifican en el código, pero permitirían mover el attractor con el mouse y cambiar su color cuando el mouse está sobre él. ¿Cómo podrías modificar el código para que esto funcione?**

Para que estas variables funcionen, necesitamos usar funciones de p5.js que detecten la posición y el estado del mouse. Aquí te explico cómo implementarlas:

Para el rollover (Cambiar color al pasar el mouse):
En el show() del Attractor, calculamos la distancia entre el mouse y el centro del círculo:

```JavaScript

let d = dist(mouseX, mouseY, this.position.x, this.position.y);
if (d < this.mass) {
  this.rollover = true;
  fill(255, 0, 0); // Rojo si el mouse está encima
} else {
  this.rollover = false;
  fill(127); // Gris normal
}
```
Para el dragging (Arrastrar con el mouse):
Necesitas usar tres funciones clave de p5.js fuera de la clase:

- mousePressed(): Si el mouse está sobre el objeto al hacer clic, activamos dragging = true.

- mouseReleased(): Al soltar el clic, dragging = false.

- handleDrag() (dentro del Attractor): Si dragging es true, actualiza la posición del attractor con mouseX y mouseY.

**Actividad 05** 

**¿Cuál es la relación entre r y theta con las posiciones x y y? Puedes repasar entonces la definición de coordenadas polares y cómo se convierten a coordenadas cartesianas.**

La relación es que r y theta se usan para calcular las posiciones x y y cuando se convierten coordenadas polares a coordenadas cartesianas.

r es la distancia desde el centro.

theta (θ) es el ángulo.

Para obtener la posición en la pantalla se usan estas fórmulas:

x = r · cos(θ)
y = r · sin(θ)

Es decir, r determina qué tan lejos está el punto del centro y theta determina hacia qué dirección (ángulo) se mueve el punto.


**Modifica la función draw():**

¿Qué ocurre?

La línea no se dibuja correctamente o aparece un error.

¿Por qué?

Porque x y y no están definidas en el código. La posición ahora está guardada en el vector v, por lo que se deberían usar v.x y v.y.


**Ahora realiza esta modificación:**

```
 function draw() {
  background(255);
  // Translate the origin point to the center of the screen
  translate(width / 2, height / 2);
  let v = p5.Vector.fromAngle(theta,r);
  fill(127);
  stroke(0);
  strokeWeight(2);
  line(0, 0, v.x, v.y);
  circle(v.x, v.y, 48);
  theta += 0.02;
}
```

¿Qué ocurre?

El punto gira alrededor del centro formando un círculo.

¿Por qué?

Porque p5.Vector.fromAngle(theta, r) usa theta como ángulo y r como la distancia desde el centro.
Como theta aumenta (theta += 0.02), el ángulo cambia y el punto se mueve en círculo alrededor del origen.



**Actividad 06** 

La función sinusoide permite simular movimientos repetitivos u oscilaciones, como ondas, sonido o movimiento de péndulos. En la simulación, el valor de sin() hace que el objeto se mueva de un lado a otro de forma suave.

Los parámetros cambian el comportamiento de la onda:

- Amplitud: controla qué tan lejos se mueve el objeto desde el centro. Si aumenta, el movimiento es más grande.

- Periodo / frecuencia: controla qué tan rápido se repite el movimiento.

- Velocidad angular: determina qué tan rápido cambia el ángulo de la función.

- Fase: desplaza la onda hacia la izquierda o derecha.

Al modificar estos valores en la simulación se puede ver cómo cambia el movimiento del punto, lo que ayuda a entender cómo funcionan las ondas y movimientos periódicos en programación y simulaciones.


**Actividad 07** 

```java script
class Oscillator {
  constructor() {
    this.angle = createVector();
    this.angleVelocity = createVector();
    this.amplitude = createVector(random(20, width / 2), random(20, height / 2));
    
    // Para Unidad 1: Offset para el Perlin Noise
    this.off = random(1000); 
  }

  update() {
    // 1. UNIDAD 3: Fuerzas (Aceleración basada en la distancia al mouse)
    let mouse = createVector(mouseX - width/2, mouseY - height/2);
    let dist = mouse.mag();
    // Creamos una aceleración que depende de qué tan lejos esté el mouse
    let acceleration = map(dist, 0, width, 0.001, 0.01);
    
    // 2. UNIDAD 1: Aleatoriedad (Perlin Noise para suavizar el cambio)
    // En lugar de random puro, la velocidad evoluciona orgánicamente
    this.angleVelocity.x = map(noise(this.off), 0, 1, -acceleration, acceleration);
    this.angleVelocity.y = map(noise(this.off + 100), 0, 1, -acceleration, acceleration);
    
    this.angle.add(this.angleVelocity);
    this.off += 0.01; // Avanzamos en el tiempo del ruido
  }

  show() {
    let x = sin(this.angle.x) * this.amplitude.x;
    let y = sin(this.angle.y) * this.amplitude.y;

    push();
    translate(width / 2, height / 2);
    stroke(0, 50); // Un poco de transparencia para ver mejor el grupo
    line(0, 0, x, y);
    fill(random(255), random(255), random(255));
    circle(x, y, 16); 
    pop();
  }
}
```

Primero agregué aleatoriedad usando Perlin Noise (noise()) en lugar de random(). Para esto utilicé una variable off que funciona como un desplazamiento en el tiempo del ruido. Esto permite que la velocidad del ángulo cambie de forma suave y continua, generando un movimiento más orgánico.

También incorporé un concepto de la Unidad 3: fuerzas. La aceleración del oscilador ahora depende de la distancia entre el mouse y el centro de la pantalla. Para calcular esto utilicé un vector hacia la posición del mouse y su magnitud (mag()), y luego usé map() para convertir esa distancia en un valor de aceleración.

Cuando el mouse está más lejos del centro, la aceleración aumenta y el movimiento del oscilador cambia más rápido. Cuando está más cerca, el movimiento es más suave.

Finalmente, en la función show() añadí colores aleatorios para los círculos, lo que hace la visualización más dinámica y permite ver mejor el comportamiento del movimiento generado por la combinación de ruido y fuerzas.

**Actividad 08** 

```javascript
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let startAngle = 0; // Nueva variable para que la ola avance
let angleVelocity = 0.2;
let amplitude = 100;

function setup() {
  createCanvas(640, 240);
}

function draw() {
  background(255); // Limpiamos la pantalla en cada frame

  stroke(0);
  strokeWeight(2);
  fill(127, 127);

  // El ángulo comienza desde startAngle en cada frame
  let angle = startAngle; 

  for (let x = 0; x <= width; x += 24) {
    // 1) Calculamos la posición y
    let y = amplitude * sin(angle);
    
    // 2) Dibujamos el círculo
    circle(x, y + height / 2, 48);
    
    // 3) Incrementamos el ángulo para el siguiente círculo del mismo frame
    angle += angleVelocity;
  }

  // Aumentamos el ángulo de inicio para que la ola se mueva hacia la izquierda
  startAngle += 0.05; 
}
```

background(255) al draw(): Si no limpiamos el fondo, los círculos se enciman y verás una mancha gris en lugar de una ola.

startAngle: Creamos esta variable para controlar el inicio de la fase de la onda.

startAngle += 0.05: Al final del draw(), aumentamos un poco este valor. Esto desplaza toda la función seno en el siguiente frame, creando la ilusión óptica de que la ola se desplaza.



**Actividad 09** 

```javascript
let bob1, bob2;
let spring1, spring2;

function setup() {
  createCanvas(640, 480); // Un poco más alto para ver ambos resortes
  
  // Inicializamos el primer sistema (fijo al techo)
  spring1 = new Spring(width / 2, 10, 100);
  bob1 = new Bob(width / 2, 110);
  
  // Inicializamos el segundo sistema (se colgará de bob1)
  spring2 = new Spring(bob1.position.x, bob1.position.y, 100);
  bob2 = new Bob(width / 2, 210);
}

function draw() {
  background(255);

  let gravity = createVector(0, 2);

  // --- Lógica para Bob 1 ---
  bob1.applyForce(gravity);
  spring1.connect(bob1);
  spring1.constrainLength(bob1, 30, 200);
  bob1.update();
  bob1.handleDrag(mouseX, mouseY);

  // --- Lógica para Bob 2 (CONECTADO EN SERIE) ---
  // Actualizamos el anclaje de spring2 para que siga a bob1
  spring2.anchor.set(bob1.position.x, bob1.position.y);
  
  bob2.applyForce(gravity);
  spring2.connect(bob2);
  spring2.constrainLength(bob2, 30, 200);
  bob2.update();
  bob2.handleDrag(mouseX, mouseY);

  // --- Dibujo ---
  spring1.showLine(bob1);
  spring1.show();
  bob1.show();

  spring2.showLine(bob2);
  spring2.show(); // Este anclaje ahora se ve sobre bob1
  bob2.show();
}

function mousePressed() {
  bob1.handleClick(mouseX, mouseY);
  bob2.handleClick(mouseX, mouseY);
}

function mouseReleased() {
  bob1.stopDragging();
  bob2.stopDragging();
}
```

```javascript
class Spring {
  constructor(x, y, length) {
    this.anchor = createVector(x, y);
    this.restLength = length;
    this.k = 0.2;
  }

  connect(bob) {
    let force = p5.Vector.sub(bob.position, this.anchor);
    let currentLength = force.mag();
    let stretch = currentLength - this.restLength;
    force.setMag(-1 * this.k * stretch);
    bob.applyForce(force);
  }

  constrainLength(bob, minlen, maxlen) {
    let direction = p5.Vector.sub(bob.position, this.anchor);
    let length = direction.mag();
    if (length < minlen) {
      direction.setMag(minlen);
      bob.position = p5.Vector.add(this.anchor, direction);
      bob.velocity.mult(0);
    } else if (length > maxlen) {
      direction.setMag(maxlen);
      bob.position = p5.Vector.add(this.anchor, direction);
      bob.velocity.mult(0);
    }
  }

  show() {
    fill(127);
    circle(this.anchor.x, this.anchor.y, 10);
  }

  showLine(bob) {
    stroke(0);
    line(bob.position.x, bob.position.y, this.anchor.x, this.anchor.y);
  }
}

```

En esta simulación modifiqué el sistema original que tenía un solo resorte, para crear un sistema de dos resortes conectados en serie.

Primero está spring1, que está fijo al techo del canvas. De este resorte cuelga bob1, que es la primera masa. Luego agregué spring2, pero en lugar de estar fijo al techo, su punto de anclaje depende de la posición de bob1. De este segundo resorte cuelga bob2, la segunda masa.

Para que el sistema funcione correctamente, en cada frame actualizo el punto de anclaje del segundo resorte usando la posición de bob1:

```javascript
spring2.anchor.set(bob1.position.x, bob1.position.y);
```

De esta manera, el segundo resorte siempre sigue a la primera masa y el sistema se comporta como una cadena de resortes.

El resultado es que cuando se mueve o se arrastra una de las masas con el mouse, la fuerza se transmite a través de los resortes y ambas masas reaccionan, generando un movimiento más realista.

**Actividad 10** 

```javascript
let bob1, bob2;
let spring1, spring2;

function setup() {
  createCanvas(640, 480); // Un poco más alto para ver ambos resortes
  
  // Inicializamos el primer sistema (fijo al techo)
  spring1 = new Spring(width / 2, 10, 100);
  bob1 = new Bob(width / 2, 110);
  
  // Inicializamos el segundo sistema (se colgará de bob1)
  spring2 = new Spring(bob1.position.x, bob1.position.y, 100);
  bob2 = new Bob(width / 2, 210);
}

function draw() {
  background(255);

  let gravity = createVector(0, 2);

  // --- Lógica para Bob 1 ---
  bob1.applyForce(gravity);
  spring1.connect(bob1);
  spring1.constrainLength(bob1, 30, 200);
  bob1.update();
  bob1.handleDrag(mouseX, mouseY);

  // --- Lógica para Bob 2 (CONECTADO EN SERIE) ---
  // Actualizamos el anclaje de spring2 para que siga a bob1
  spring2.anchor.set(bob1.position.x, bob1.position.y);
  
  bob2.applyForce(gravity);
  spring2.connect(bob2);
  spring2.constrainLength(bob2, 30, 200);
  bob2.update();
  bob2.handleDrag(mouseX, mouseY);

  // --- Dibujo ---
  spring1.showLine(bob1);
  spring1.show();
  bob1.show();

  spring2.showLine(bob2);
  spring2.show(); // Este anclaje ahora se ve sobre bob1
  bob2.show();
}

function mousePressed() {
  bob1.handleClick(mouseX, mouseY);
  bob2.handleClick(mouseX, mouseY);
}

function mouseReleased() {
  bob1.stopDragging();
  bob2.stopDragging();
}
```



Creé un sistema de péndulo doble conectando el inicio del segundo péndulo al final del primero.

¿Cómo funciona?

Péndulo 1: Está sujeto a un punto fijo en el techo (su pivote).

Péndulo 2: Su punto de apoyo (pivote) ya no es fijo; ahora es la "bola" del primer péndulo.

El truco del código:
Al igual que con los resortes, la clave es actualizar la posición del segundo en cada frame del draw(). Le decimos al segundo péndulo: "Tu techo es la posición actual de la bola del primero".

```JavaScript

// La conexión que crea el sistema en serie:
p2.pivot.set(p1.bob.x, p1.bob.y);
```



## Bitácora de aplicación 

**Actividad 11** 

**Describe el concepto de tu obra generativa.**

Mi obra generativa está formada por olas de puntos de colores que se mueven de manera continua. Las olas tienen un movimiento suave que cambia con el tiempo.

El mouse permite mover y sacudir las olas como si las pudieras tocar, alterando su forma. Al presionar un botón, los puntos se dispersan como una explosión, pero después el sistema hace que vuelvan a ordenarse y continúen funcionando como una ola.
  
```JavaScript

let waveLayers = [];
let numLayers = 25; 
let pointsPerLine = 60; 
let startAngle = 0;

class QuantumPoint {
  constructor(x, y, layer) {
    this.anchorPos = createVector(x, y);
    this.pos = createVector(x, y);
    this.vel = createVector();
    this.acc = createVector();
    this.layer = layer;
    this.noiseOff = random(1000); 
  }

  applyForce(f) {
    this.acc.add(f);
  }

  update() {
    // UNIDAD 3: Resorte mucho más suave para un retorno lento y elegante
    let restoring = p5.Vector.sub(this.anchorPos, this.pos);
    restoring.mult(0.03); // Antes era 0.08. Ahora es más "flojo"
    this.applyForce(restoring);

    // UNIDAD 2: Cinemática
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0); 
    this.vel.mult(0.92); // Fricción más alta para que se deslicen como en agua
  }

  interact() {
    let d = dist(mouseX, mouseY, this.pos.x, this.pos.y);
    
    if (d < 100) { 
      let repel = p5.Vector.sub(this.pos, createVector(mouseX, mouseY));
      repel.setMag(map(d, 0, 100, 1.0, 0)); // Repulsión más suave
      this.applyForce(repel);

      let mouseVel = createVector(mouseX - pmouseX, mouseY - pmouseY);
      mouseVel.mult(0.03); // Arrastre más delicado
      this.applyForce(mouseVel);
    }
  }

  showPoint() {
    // UNIDAD 1: Variación aleatoria de tamaño mucho más notoria (respiración)
    // Aceleramos un poco el tiempo del ruido para que el cambio de tamaño sea evidente
    let rNoise = noise(this.noiseOff, frameCount * 0.04); 
    // Mapeamos a un rango más extremo: desde puntos minúsculos hasta esferas grandes
    let baseRadius = map(rNoise, 0, 1, 1, 12); 
    
    let kineticRadius = baseRadius + this.vel.mag() * 0.2; 
    
    let hue = (this.pos.x * 0.1 + frameCount * 0.5 + this.layer * 5) % 360;
    
    fill(hue, 80, 100, 0.5); 
    noStroke();
    
    circle(this.pos.x, this.pos.y, kineticRadius);
  }
}

function setup() {
  createCanvas(800, 500); 
  colorMode(HSB, 360, 100, 100, 1); 
  
  for (let l = 0; l < numLayers; l++) {
    let currentLine = [];
    for (let i = 0; i < pointsPerLine; i++) {
      let x = map(i, 0, pointsPerLine - 1, 50, width - 50);
      let y = map(l, 0, numLayers - 1, 80, height - 80);
      currentLine.push(new QuantumPoint(x, y, l));
    }
    waveLayers.push(currentLine);
  }
}

function draw() {
  background(10, 0.4); 
  blendMode(ADD);

  for (let l = 0; l < waveLayers.length; l++) {
    let points = waveLayers[l];
    
    for (let i = 0; i < points.length; i++) {
      let p = points[i];
      
      let baseAngle = startAngle + (i * 0.1) + (l * 0.05);
      p.anchorPos.y = map(l, 0, numLayers - 1, 80, height - 80) + sin(baseAngle) * 30;
      
      let noiseVal = noise(p.noiseOff, frameCount * 0.005);
      p.anchorPos.y += map(noiseVal, 0, 1, -15, 15);
      
      p.interact(); 
      p.update();   
      p.showPoint();
      
      p.noiseOff += 0.02; // Aceleramos un poco el offset para que cambien de tamaño fluidamente
    }
  }

  blendMode(BLEND); 
  startAngle += 0.015; 
}

function keyPressed() {
  if (key === 'E' || key === 'e') {
    for (let currentLine of waveLayers) {
      for (let p of currentLine) {
        // UNIDAD 1: Explosión suave y etérea
        let explosion = p5.Vector.random2D();
        
        if (random(1) < 0.9) {
          explosion.mult(random(3, 10)); // Impulso muy leve
        } else {
          explosion.mult(random(15, 30)); // El salto Lévy también es más suave
        }
        
        p.applyForce(explosion);
      }
    }
  }
}
```
[Ej 11 unidad 4](https://editor.p5js.org/Tomasm12/sketches/-ZxXFZg5D)

<img width="712" height="444" alt="image" src="https://github.com/user-attachments/assets/fb74306a-7833-4e93-879b-ae8f2499439e" />

<img width="720" height="441" alt="image" src="https://github.com/user-attachments/assets/6fe7eb2e-0c42-4c31-a52e-54b7d87da35e" />


## Bitácora de reflexión






