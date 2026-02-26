# Unidad 3

## Bitácora de proceso de aprendizaje

*Actividad 01*

Por un lado, la IA y el arte generativo son impresionantes. La rapidez con la que producen imágenes complejas y visualmente fuertes es algo realmente fascinante. Aunque puedan hacer obsoletos ciertos aspectos tradicionales como el error humano o el proceso largo de aprendizaje, también abren la puerta a algo nuevo y diferente. No creo que eso le quite lo bello.

Lo que más me impactó fue cuando Hodgin duda del valor de su propio esfuerzo al ver que una IA puede lograr algo similar en segundos. Eso me hizo pensar en cómo hoy importa más el resultado que el proceso. Lo que se evalúa y se juzga es el producto final, no el camino para llegar ahí.

No considero que la IA sea buena o mala. Más bien es un nuevo tipo de arte que responde a un tiempo donde se prioriza la rapidez y la producción. La pregunta que me queda es si, al enfocarnos solo en el resultado, estamos perdiendo algo importante del proceso creativo y de lo que significa realmente crear.

*Actividad 02*

En esta actividad entendí que multiplicar la aceleración por cero al final de update() no es un detalle menor. Se hace porque la aceleración solo debe representar las fuerzas que actúan en ese frame. Si no se reinicia, las fuerzas se acumulan y el objeto seguiría acelerando aunque ya no exista ninguna causa. Se limpia al final porque primero se usa para actualizar velocidad y posición, y luego el sistema queda listo para el siguiente frame.

También comprendí qué pasaba cuando dejábamos de asumir masa = 1. Si dentro de applyForce dividía directamente force.div(10), estaba modificando el vector original. Como p5.Vector se pasa por referencia, esa fuerza quedaba alterada para todos los objetos que la usaran. Lo correcto es crear una copia del vector, dividir esa copia por la masa y luego sumarla a la aceleración.

Aprendí que pequeños detalles como reiniciar un vector o entender el paso por referencia cambian completamente el comportamiento del sistema.

*Actividad 03*

*Fricción*

<img width="672" height="159" alt="image" src="https://github.com/user-attachments/assets/a3c280e0-9c45-4370-940a-209b6331dccd" />


```javascript

let mover;

function setup() {
  createCanvas(640, 240);
  // Creamos el objeto en la izquierda, al centro, con masa 2
  mover = new Mover(50, height / 2, 2);
  
  // Le damos un "impulso" inicial (velocidad de salida)
  mover.velocity = createVector(10, 0);
}

function draw() {
  // Usamos un fondo con transparencia para ver el rastro del deslizamiento
  background(255, 5); 

  // 1. Calculamos la fricción
  // La fricción es opuesta a la velocidad: F = -1 * mu * v_normalizada
  let friction = mover.velocity.copy();
  friction.normalize();
  friction.mult(-1);
  
  let mu = 0.05; // Qué tan "rugoso" es el suelo
  friction.mult(mu);

  // 2. Aplicamos la fuerza
  mover.applyForce(friction);

  // 3. Actualizamos y mostramos
  mover.update();
  mover.checkEdges();
  mover.show();
  
  // Dibujamos una línea decorativa que represente el "suelo"
  stroke(200);
  line(0, height/2 + 20, width, height/2 + 20);
}

// --- CLASE MOVER (Estilo exacto de la Actividad 02) ---

class Mover {
  constructor(x, y, m) {
    this.mass = m;
    this.radius = m * 8;
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
  }

  applyForce(force) {
    // A = F / M
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    // Limpiamos la aceleración en cada frame
    this.acceleration.mult(0);
  }

  show() {
    stroke(0);
    strokeWeight(2);
    fill(127, 127);
    circle(this.position.x, this.position.y, this.radius * 2);
  }

  checkEdges() {
    if (this.position.x > width - this.radius) {
      this.position.x = width - this.radius;
      this.velocity.x *= -0.9;
    } else if (this.position.x < this.radius) {
      this.position.x = this.radius;
      this.velocity.x *= -0.9;
    }
  }
}
```
*Resistencia al aire y fluidos*

```javascript
let gotas = [];

function setup() {
  createCanvas(640, 400);
  // Creamos 50 gotas de lluvia con masas pequeñas
  for (let i = 0; i < 50; i++) {
    gotas[i] = new Mover(random(width), random(-height), random(1, 3));
  }
}

function draw() {
  background(230, 230, 250); // Fondo color grisáceo/lluvia

  for (let g of gotas) {
    // 1. Gravedad (Atrae la gota hacia abajo)
    let gravity = createVector(0, 0.2 * g.mass);
    g.applyForce(gravity);

    // 2. Resistencia del aire (Drag)
    // Se aplica en todo el canvas. Es lo que evita que la lluvia caiga demasiado rápido.
    let drag = g.velocity.copy();
    let speed = g.velocity.mag();
    
    let c = 0.1; // Coeficiente de resistencia del aire
    let dragMagnitude = c * speed * speed; // v al cuadrado
    
    drag.normalize();
    drag.mult(-1);
    drag.mult(dragMagnitude);
    
    g.applyForce(drag);

    g.update();
    g.checkEdges();
    g.show();
  }
}

// --- TU CLASE MOVER ---
class Mover {
  constructor(x, y, mass) {
    this.mass = mass;
    // Las gotas son estiradas, no círculos perfectos
    this.w = mass * 2; 
    this.h = mass * 10; 
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  show() {
    stroke(100, 150, 255);
    strokeWeight(2);
    // Dibujamos una línea en lugar de un círculo para que parezca lluvia
    line(this.position.x, this.position.y, this.position.x, this.position.y + this.h);
  }

  checkEdges() {
    // Si la gota sale por abajo, vuelve a empezar arriba
    if (this.position.y > height) {
      this.position.y = random(-100, -10);
      this.position.x = random(width);
      this.velocity.mult(0); // Reinicia velocidad al reaparecer
    }
  }
}
```
<img width="638" height="399" alt="image" src="https://github.com/user-attachments/assets/9292817b-729a-40bb-bced-5c07a080e378" />

*Atracción gravitacional.*


```javascript
let central; // El "Sol" o Planeta central
let moons = []; // El arreglo de lunas
let G = 5; 

function setup() {
  createCanvas(windowWidth, windowHeight);
  
  // 1. Creamos el cuerpo central (masa grande, en el centro)
  central = new Mover(width / 2, height / 2, 20);
  
  // 2. Creamos 5 lunas con posiciones y velocidades iniciales
  for (let i = 0; i < 5; i++) {
    let x = random(width);
    let y = random(height);
    let m = random(2, 5);
    moons[i] = new Mover(x, y, m);
    
    // Les damos un empujón inicial para que entren en órbita
    moons[i].velocity = createVector(random(-2, 2), random(-2, 2));
  }
}

function draw() {
  // El cuarto parámetro (20) crea el efecto de estela/rastro
  background(0, 0, 20, 20); 

  // Dibujamos el centro
  fill(255, 200, 0);
  noStroke();
  circle(central.position.x, central.position.y, central.mass * 2);

  for (let moon of moons) {
    // La gravedad del centro atrae a la luna
    let force = central.calculateAttraction(moon);
    moon.applyForce(force);

    moon.update();
    moon.show();
  }
}

class Mover {
  constructor(x, y, mass) {
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.mass = mass;
  }

  // Lógica de Newton: F = G * (m1 * m2) / r^2
  calculateAttraction(mover) {
    let force = p5.Vector.sub(this.position, mover.position);
    let distance = constrain(force.mag(), 5, 50); 
    force.normalize();
    let strength = (G * this.mass * mover.mass) / (distance * distance);
    force.mult(strength);
    return force;
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  show() {
    stroke(255);
    fill(100, 150, 255);
    circle(this.position.x, this.position.y, this.mass * 2);
  }
}

```
<img width="1053" height="980" alt="image" src="https://github.com/user-attachments/assets/200ee07f-845e-4b8e-a032-de5e83dd9bce" />




## Bitácora de aplicación 



## Bitácora de reflexión
