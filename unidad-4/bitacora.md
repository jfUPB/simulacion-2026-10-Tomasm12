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
**Actividad 05** 




## Bitácora de aplicación 



## Bitácora de reflexión

