# Unidad 2

## Bitácora de proceso de aprendizaje
**Actividad 01**
La actividad que escogí fue Pikaworm. Me llamó la atención por lo extraña que se veía y por la sensación de que los Pikaworms se iban acercando a uno, como si fueran a salirse de la pantalla.

**Actividad 02**

**¿Cómo funciona la suma dos vectores en p5.js?**

la suma de vectores se hace usando métodos del objeto p5.Vector, como position.add(velocity). Esta operación suma cada componente del vector velocidad a la posición (x con x y y con y), lo que permite actualizar la posición y generar movimiento.

**¿Por qué esta línea position = position + velocity; no funciona?**

No funciona porque position y velocity son vectores, es decir, objetos. En JavaScript el operador + solo sirve para sumar valores simples como números, no objetos. Por eso, para sumar vectores, es necesario usar los métodos que ofrece p5.Vector.

**Actividad 03**

¿Qué tuviste que hacer para hacer la conversión propuesta?

- Reemplazar x e y por un solo vector (this.pos) para representar la posición.

- Usar funciones de p5.Vector en lugar de operaciones con números sueltos.

- Crear el movimiento como un vector (velocity) en vez de decidir pasos en direcciones fijas.

- Actualizar la posición con suma vectorial usando this.pos.add(velocity).

- Pensar el movimiento como dirección + magnitud, no como “sumar o restar 1” a x o y.

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
    // Iniciamos en el centro con un vector
    this.pos = createVector(width / 2, height / 2);
  }

  show() {
    stroke(0, 100); // Un poco de transparencia para ver el rastro
    strokeWeight(2);
    point(this.pos.x, this.pos.y);
  }

  step() {
    // 1. Creamos un vector aleatorio de longitud 1
    let velocity = p5.Vector.random2D();
    
    // 2. Sumamos ese vector a la posición actual
    this.pos.add(velocity);
  }
}
```

**Actividad 04**

**¿Qué resultado esperas obtener en el programa anterior?**

Espero que el vector position cambie después de llamar a la función, porque dentro de playingVector() se modifican sus valores x e y.

**¿Qué resultado obtuviste?**

En la consola se muestra primero el vector (6, 9) y luego el vector (20, 30). Esto indica que el vector sí fue modificado por la función.

**¿Qué tipo de paso se está realizando en el código?**

En este código se está realizando un paso por referencia

**¿Qué aprendiste?**

los vectores en p5.js se comportan como objetos y, cuando se pasan a una función, cualquier cambio que se haga dentro de ella modifica el vector original. Esto es importante para evitar errores cuando no se quiere cambiar el valor original.

**Actividad 05**

¿Para qué sirve el método mag()? Nota que hay otro método llamado magSq(). ¿Cuál es la diferencia entre ambos? ¿Cuál es más eficiente?

El método mag() sirve para calcular la magnitud o longitud real de un vector. magSq() calcula la magnitud al cuadrado, sin hacer la raíz cuadrada.
La diferencia es que mag() da el valor exacto y magSq() se usa para comparaciones. magSq() es más eficiente porque evita el cálculo de la raíz cuadrada.

¿Para qué sirve el método normalize()?

Sirve para convertir un vector en un vector unitario, es decir, con magnitud 1, manteniendo la misma dirección.

Te encuentras con un periodista en la calle y te pregunta ¿Para qué sirve el método dot()? ¿Qué le responderías en un frase?

El método dot() sirve para saber qué tan alineados o relacionados están dos vectores.

La versión de instancia se llama desde un vector (v1.dot(v2)), mientras que la versión estática se llama desde la clase (p5.Vector.dot(v1, v2)). Ambas realizan el mismo cálculo, solo cambia la forma de usarlas.

El método dot() tiene una versión estática y una de instancia. ¿Cuál es la diferencia entre ambas?

Ahora el mismo periodista curioso de antes te pregunta si le puedes dar una intuición geométrica acerca del producto cruz. Entonces te pregunta ¿Cuál es la interpretación geométrica del producto cruz de dos vectores? Tu respuesta debe incluir qué pasa con la orientación y la magnitud del vector resultante.

¿Para que te puede servir el método dist()?

El método dist() sirve para calcular la distancia entre dos puntos, representados como vectores.

¿Para qué sirven los métodos normalize() y limit()?

normalize() se usa para mantener solo la dirección del vector, sin importar su magnitud.
limit() se usa para limitar la magnitud máxima de un vector, por ejemplo para controlar la velocidad.

**Actividad 06**

El código que genera el resultado que te pedí.

```javascript
let u = 0;
let velocito = 0.01;
let palla = 1;

function setup() {
  createCanvas(400, 400);
}

function draw() {
  background(200);

  let v0 = createVector(50, 50);
  let v1 = createVector(300, 0);
  let v2 = createVector(0, 300);

  // Animación del lerp
  u += velocito * palla;
  if (u >= 1 || u <= 0) palla *= -1;

  // Vector interpolado
  let movingPoint = p5.Vector.lerp(v1, v2, u);

  // Colores base
  let redColor = color(255, 0, 0);
  let blueColor = color(0, 0, 255);

  // Color del vector morado cambia según u
  let movingColor = lerpColor(redColor, blueColor, u);

  // Flechas fijas
  drawArrow(v0, v1, redColor);
  drawArrow(v0, v2, blueColor);

  // Flecha que cambia de color
  drawArrow(v0, movingPoint, movingColor);

  // Vector verde
  drawArrow(p5.Vector.add(v0, v1), p5.Vector.sub(v2, v1), "green");
}

function drawArrow(base, vec, myColor) {
  push();
  stroke(myColor);
  strokeWeight(4);
  fill(myColor);
  translate(base.x, base.y);
  line(0, 0, vec.x, vec.y);
  rotate(vec.heading());
  let arrowSize = 12;
  translate(vec.mag() - arrowSize, 0);
  triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
  pop();
}

```

¿Cómo funciona lerp() y lerpColor().
¿Cómo se dibuja una flecha usando drawArrow()?
- lerp() interpola linealmente entre dos vectores usando un valor entre 0 y 1, generando posiciones intermedias.
  
- lerpColor() hace lo mismo, pero con colores, creando una transición suave entre dos colores.
  
En el código, ambos usan el mismo valor (u), por lo que el movimiento y el color están sincronizados.

La flecha se dibuja moviendo el sistema de coordenadas al punto base, trazando una línea según el vector, rotando según su dirección y agregando un triángulo al final para representar la punta.


**Actividad 07**

Cuál es el concepto del marco motion 101 y cómo se interpreta geométricamente.

El marco Motion 101 es una forma básica de representar movimiento usando vectores. Un objeto tiene un vector de posición, que indica dónde está, y un vector de velocidad, que indica cómo cambia su posición con el tiempo.
Geométricamente, el movimiento ocurre al sumar la velocidad a la posición en cada instante, lo que produce un desplazamiento continuo en una dirección.

¿Cómo se aplica motion 101 en el ejemplo?

el objeto (mover) tiene una posición y una velocidad representadas como vectores.
En cada cuadro de la animación, el método update() suma la velocidad a la posición, lo que hace que el objeto se mueva.
Luego, checkEdges() controla qué pasa cuando el objeto llega a los bordes del canvas, y show() dibuja el objeto en su nueva posición.
Así, el ejemplo aplica directamente el marco Motion 101: actualizar la posición con la velocidad y luego dibujar el resultado.


**Actividad 08**

¿Qué observaste cuando usas cada una de las aceleraciones propuestas?

 - Aceleración constante:
El objeto se mueve cada vez más rápido en una misma dirección. El movimiento es regular y predecible.

 - Aceleración aleatoria:
El objeto cambia de dirección y velocidad todo el tiempo. El movimiento es irregular y caótico.

 - Aceleración hacia el mouse:
El objeto se mueve siguiendo el mouse. El movimiento parece intencional, como si persiguiera un objetivo.


## Bitácora de aplicación 

**Actividad 09**


Describe el concepto de tu obra generativa. Explica el concepto de tu obra generativa, qué regla aplicaste para la aceleración y por qué, si fue una decisión de diseño, o qué te evoca, si fue una exploración artística.

quiero generar particulas de colores que persiguen al mause y usando los conceptos de motion 101, quiero que estas paraezcan de dorma aletoria y cunaod llegue al mouse giren y orbiten alrededor de este como si fueran atomos.

El código de la aplicación.

```javascript


let particles = [];

function setup() {
  createCanvas(windowWidth, windowHeight);
  // Usamos HSB para que cada partícula tenga un tono (hue) distinto
  colorMode(HSB, 360, 100, 100, 100);
  
  for (let i = 0; i < 60; i++) {
    particles.push(new Particle());
  }
  background(0);
}

function draw() {
  // Fondo negro con transparencia para el rastro
  background(0, 15); 

  for (let p of particles) {
    p.update();
    p.display();
  }
}

class Particle {
  constructor() {
    this.pos = createVector(random(width), random(height));
    this.vel = p5.Vector.random2D();
    this.acc = createVector(0, 0);
    this.maxSpeed = 5;
    
    // ASIGNACIÓN DE COLOR: Cada partícula nace con su propio tono aleatorio
    this.color = random(360); 
  }

  update() {
    let mouse = createVector(mouseX, mouseY);
    let force = p5.Vector.sub(mouse, this.pos);
    force.setMag(0.2); 
    this.acc = force;

    this.vel.add(this.acc);
    this.vel.limit(this.maxSpeed);
    this.pos.add(this.vel);
  }

  display() {
    // Usamos el color único de esta partícula
    stroke(this.color, 80, 100, 70); 
    strokeWeight(2);
    
    push();
    translate(this.pos.x, this.pos.y);
    // La línea apunta hacia donde se mueve (velocidad)
    line(0, 0, this.vel.x * 4, this.vel.y * 4);
    pop();
  }
}

function mousePressed() {
  for (let p of particles) {
    p.pos = createVector(random(width), random(height));
    // Al hacer click, también les damos un nuevo color aleatorio
    p.color = random(360);
  }
}
```
Un enlace al proyecto en el editor de p5.js.
[Actividad 9  Codigo](https://editor.p5js.org/Tomasm12/sketches/V4bS40WkZ)

Selecciona capturas de pantalla representativas de tu pieza de arte generativa.

<img width="492" height="967" alt="image" src="https://github.com/user-attachments/assets/71ecea06-3d97-457a-be2d-cc84461feb59" />


<img width="330" height="394" alt="image" src="https://github.com/user-attachments/assets/225815db-7069-48e7-a952-1ed6d2139c52" />


## Bitácora de reflexión

