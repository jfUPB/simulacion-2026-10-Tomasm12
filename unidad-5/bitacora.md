# Unidad 5


## Bitácora de proceso de aprendizaje

**Actividad 01: Anatomía de una partícula (Example 4.2: An Array of Particles)**

**Capa de comportamiento:**

**¿Qué propiedades definen el estado físico y el estado vital de la partícula?**


El estado físico se gestiona mediante vectores: position para la ubicación en el lienzo, velocity para el cambio de posición y acceleration para alterar esa velocidad (simulando fuerzas como la gravedad). El estado vital se rige por la variable lifespan, un valor numérico que decrece constantemente y determina tanto la transparencia visual del objeto como su derecho a existir en la memoria del programa.


**¿Qué condición determina la "muerte" de una partícula y cómo ocurre?**

La muerte ocurre cuando lifespan es menor a 0.0. Este proceso es un híbrido: visualmente es gradual, ya que el valor de vida se mapea directamente al canal alpha (transparencia) del dibujo; pero lógicamente es instantánea, pues el sistema usa la función isDead() para borrar el objeto del ArrayList en el momento exacto en que cumple la condición, evitando que el procesador siga calculando partículas invisibles.

**¿Cómo se aplica el patrón Motion 101 en la actualización de cada frame?**

Se implementa mediante la jerarquía de integración de Euler: primero, la aceleración se suma a la velocidad y, posteriormente, esa velocidad se suma a la posición para generar el movimiento *(P=P+V)* . En este modelo de partículas, se añade un paso extra de "envejecimiento" al final del método update(), donde se resta una constante al lifespan para que el movimiento y la desaparición ocurran simultáneamente.


**Capa de estructura:**


**¿Quién crea las partículas? ¿En qué momento?**

Las partículas son instanciadas por el sketch principal dentro de la función draw(). En este ejemplo, se crea una nueva instancia de la clase Particle en cada frame (usando particles.push()), lo que genera un flujo constante de nuevos objetos en la posición del mouse o en un punto fijo.

**¿Quién decide cuándo eliminar una partícula del array?**

El sketch principal es el encargado de la ejecución, pero la lógica reside en la partícula. Dentro del bucle de animación, el programa interroga a cada objeto mediante su método isDead(). Si la partícula responde que su lifespan se ha agotado, el programa principal ejecuta la instrucción splice() para borrarla definitivamente.

**¿Por qué se recorre el array en orden inverso para eliminar? ¿Qué pasaría si no se hiciera así?**

Se hace para evitar errores de índice. Cuando eliminas un elemento de un ArrayList o arreglo, los elementos siguientes se desplazan una posición hacia la izquierda. Si recorres el array hacia adelante (0->N), al eliminar el elemento en el índice 2, el que estaba en el 3 pasa al 2, y el bucle salta al 3 en la siguiente vuelta, saltándose un objeto sin analizar. Recorrer en reversa (N->0) garantiza que el desplazamiento no afecte a los elementos que aún no han sido procesados.

**Si no eliminaras nunca las partículas, ¿Qué pasaría con la memoria y el rendimiento? Haz el experimento: comenta la línea que elimina y observa el frame rate.**

Ocurriría una "fuga de memoria" lógica. Aunque las partículas sean invisibles (transparencia 0), el procesador seguiría calculando su física y el arreglo crecería infinitamente. Al hacer el experimento de comentar la eliminación, notarás que el frame rate cae drásticamente después de unos segundos, ya que el sistema se satura intentando procesar miles de vectores innecesarios en cada cuadro, lo cual es ineficiente incluso en computadoras con hardware potente.


**Capa de visualización:**

**¿Qué elementos visuales usa para representar una partícula?**

En el código estándar de este ejemplo, la partícula se representa mediante un círculo simple (circle() o ellipse()). Este dibujo utiliza un borde y un relleno en tonos grises, cuya ubicación exacta en el lienzo está determinada por los componentes x y y del vector position.

**¿Cómo se conecta el “tiempo de vida” con la apariencia visual?**

La conexión es directa y proporcional: el valor de la variable lifespan (que inicia en 255) se pasa como el cuarto argumento (canal alpha) en las funciones stroke() y fill(). Esto provoca que la partícula pierda opacidad en cada frame, creando un efecto de desvanecimiento que comunica visualmente que el objeto está "muriendo" antes de desaparecer del sistema.

**Si quisieras cambiar la representación visual (por ejemplo, usar líneas en vez de círculos), ¿Qué cambiarías y qué NO cambiarías?**

Cambiarías exclusivamente el bloque de código dentro del método show(), reemplazando la función de dibujo por un line() que use la posición actual y la velocidad para orientarse. No cambiarías nada de la lógica de movimiento, la integración de vectores, ni el sistema de gestión de memoria, ya que la física y el ciclo de vida son independientes de la geometría que se renderice en pantalla.

**Actividad 02: Del array al sistema: la abstracción del emisor (Example 4.4: A System of Systems)**

**Comparación con Example 4.2:**

**¿Qué responsabilidades que antes estaban en draw() ahora están dentro de la clase Emitter?**

Toda la gestión de memoria y el ciclo de vida. El bucle for inverso encargado de actualizar, dibujar y eliminar las partículas con el splice(), así como la línea de código que genera nuevas partículas (push(new Particle())), desaparecen del draw() y se encapsulan dentro de los métodos del emisor.

**¿Cuál es la ventaja de encapsular la lógica de emisión en una clase separada?**

La modularidad y el orden. Al encapsular, conviertes un bloque de código complejo en una "caja negra" reutilizable. La mayor ventaja es que puedes tener múltiples fuentes de partículas funcionando al mismo tiempo con físicas y orígenes distintos, sin que sus variables choquen entre sí y manteniendo el código del programa principal muy limpio.

**En este ejemplo hay un array de emitters. ¿Quién crea los emitters? ¿Quién crea las partículas dentro de cada emitter?**

El sketch principal es quien crea los emitters (usualmente cuando haces clic con el mouse, empujando un nuevo sistema al array principal). Por su parte, cada emitter es el único responsable de crear a sus propias partículas, instanciándolas internamente en cada frame según su propia posición de origen.

**Dibuja un diagrama que muestre la jerarquía: sketch → [emitters] → [partículas]. ¿Cuántos niveles de “colección” hay?**

<img width="1054" height="816" alt="image" src="https://github.com/user-attachments/assets/39cf79db-8845-4926-8fee-b8fda1dbefaa" />

**Actividad 03: Heterogeneidad: herencia y polimorfismo (Example 4.5)**

**¿Qué tienen en común las subclases de partículas? ¿Qué tienen de diferente?**

En común, comparten toda la capa de comportamiento físico y vital: los vectores de posición, velocidad y aceleración, así como la lógica de desgaste del lifespan y el cálculo de fuerzas. Todo esto lo heredan intacto de la clase padre Particle. Lo que tienen de diferente es únicamente la capa de visualización: la subclase Confetti sobreescribe el método show() para renderizar un cuadrado que rota en función de su posición en X, ignorando el círculo gris de la clase original.


**¿Por qué es importante que el Emitter no necesite saber qué tipo específico de partícula está gestionando? Explica esto con tus propias palabras.**

Esto es el núcleo del polimorfismo. El Emitter solo tiene una lista y a todos sus elementos les da la misma orden general: p.run(). Si el emisor tuviera que identificar cada objeto (ej. "si es confeti haz esto, si es círculo haz aquello"), el código se volvería inmanejable y lento a medida que crece el proyecto. Al no importar el tipo específico, el sistema delega la responsabilidad: el emisor solo se encarga de organizar el ciclo general, y cada objeto internamente ya sabe cómo debe comportarse y dibujarse.

**Si mañana quisieras agregar un tercer tipo de partícula, ¿Qué tendrías que crear y qué NO tendrías que modificar?**

Tendrías que crear una nueva clase usando la palabra clave extends (por ejemplo, class Spark extends Particle) y redactar su propio método show(). Después, solo añadirías una condición extra en el método addParticle() del Emitter para que a veces instancie esta nueva clase. NO tendrías que modificar en absoluto la clase base Particle, ni el bucle de iteración inversa del emisor, ni las funciones setup() o draw() del sketch principal.

**Compara con Example 4.2: ¿Cambió la lógica del Emitter? ¿Cambió la lógica de muerte? ¿Qué capa del sistema se modificó y cuáles permanecieron intactas?**

La lógica de iteración (el bucle for inverso) y la lógica de muerte (evaluar isDead() y aplicar el splice()) permanecen idénticas. Lo que se modificó fue la capa de estructura (al diversificar los objetos con herencia) y la capa de visualización (al tener representaciones gráficas simultáneas). La capa de comportamiento (la integración de los vectores usando Motion 101) permaneció totalmente intacta.


**Actividad 04**

** En Example 4.6, ¿Dónde se define la gravedad? ¿Quién la aplica a las partículas? ¿Es una fuerza global o local?**

Respuesta: La gravedad se define en la función draw() (fuera de las clases). El Emitter es el encargado de repartirla, ya que tiene una función applyForce que le "pasa" ese empujón a cada una de sus partículas. Es una fuerza global porque es el mismo vector para todas: a todas las empuja igual hacia abajo, sin importar en qué parte de la pantalla estén.

**En Example 4.7, ¿Qué diferencia hay entre la gravedad y la fuerza del repeller? ¿Dónde “vive” cada una?**

Respuesta: La gravedad es una fuerza "tonta" y constante (siempre apunta igual), mientras que el repeller es "inteligente" porque su fuerza cambia según la distancia. La gravedad "vive" como una variable suelta en el código principal, pero la fuerza del repeller "vive" dentro de su propia clase (Repeller), que es la que sabe calcular qué tanto debe empujar a cada partícula según lo cerca que esté.

**La fuerza del repeller depende de la distancia entre la partícula y el repeller. ¿Qué principio físico se está modelando?**

Respuesta: Se está usando la idea de la gravedad de Newton o la fuerza de los imanes. Es el principio de que la fuerza se vuelve mucho más débil a medida que te alejas (lo que llaman la ley del cuadrado inverso). Básicamente, si estás muy cerca el empujón es fuertísimo, pero si te alejas un poco, la fuerza cae rapidísimo.

**¿Cambió la clase Particle entre Example 4.6 y 4.7? ¿Qué implica esto sobre la separación entre comportamiento de la partícula y fuerzas externas?**

Respuesta: No, la clase Particle casi no cambió. Esto es genial porque significa que la partícula es "ciega": ella no sabe quién la empuja (si es el viento, un imán o la gravedad), solo sabe recibir una fuerza y moverse. Esto permite que el comportamiento (moverse) esté separado de las fuerzas (quién la empuja), haciendo que el código sea mucho más fácil de organizar.

<img width="664" height="401" alt="image" src="https://github.com/user-attachments/assets/58e67ef9-62db-40db-8fa2-6dfa92596d05" />

modificacion de codigo 

```js

  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    
    // Guardamos el estado del lienzo
    push(); 
    // Movemos el "centro" del dibujo a la posición de la partícula
    translate(this.position.x, this.position.y); 
    // Hacemos que rote un poco (usando su lifespan para que el giro cambie)
    rotate(this.lifespan * 0.1); 
    
    rectMode(CENTER);
    square(0, 0, 8); // Dibujamos el cuadrado en el nuevo centro (0,0)
    pop(); 
  }
```

1. ¿Qué líneas de código tocaste?

Toqué exclusivamente las líneas dentro de la función show() de la clase Particle (aproximadamente de la línea 66 a la 72 del código original).

2. ¿Qué clases/funciones modificaste?

Solo modifiqué la clase Particle y su función show(). No se tocó nada más.

3. ¿Qué partes del programa NO necesitaste modificar?

La clase Emitter: Sigue creando y borrando partículas igual que antes.

La clase Repeller: Sigue calculando la fuerza de repulsión sin importarle cómo se ven las partículas.

La función update(): La física (gravedad, velocidad, posición) sigue funcionando igual.

El draw() y setup(): El flujo principal del programa no cambió.

4. ¿Por qué fue posible hacer este cambio sin afectar las demás capas?

Fue posible gracias a la modularidad. El programa está dividido en "especialistas": a la física solo le importan los números (vectores), y al dibujo solo le importa dónde están esos números para poner un color o una forma. Como el Emitter y el Repeller solo hablan con la "capa de física", puedes cambiar la "capa visual" por completo y el sistema ni se entera; simplemente sigue moviendo "puntos" en el espacio.







## Bitácora de aplicación 

El ciclo que quiero crear representa el origen del universo. Inicia con una esfera blanca que vibra o se mueve y comienza a expandirse hasta explotar. A partir de esta explosión se generan partículas de distintos colores y tamaños que se dispersan por toda la pantalla, simulando estrellas, planetas y la formación del universo. Finalmente, se genera un agujero negro que consume todo lo existente, poniendo fin a este ciclo.

<img width="415" height="723" alt="image" src="https://github.com/user-attachments/assets/41fff355-e69d-4421-8b3e-4c3165922933" />


La emisión representa la creación constante del universo: primero ocurre una gran explosión inicial y luego siguen naciendo partículas, mostrando que el universo está en expansión y nunca es completamente estático.

Las fuerzas simbolizan las leyes que ordenan el caos. Al inicio, las partículas se mueven libremente, pero la interacción del usuario introduce perturbaciones. Al final, la gravedad del agujero negro se vuelve dominante e inevitable.

La condición de muerte . Las partículas desaparecen con el tiempo o son absorbidas por el agujero negro, no como destrucción total, sino como transformación de la materia.

La visualización usa el contraste entre colores vivos y el vacío oscuro para mostrar la vida, diversidad y energía del universo frente a su colapso final.

La interacción del usuario es el motor del ciclo: activa el nacimiento del universo, influye en su comportamiento y finalmente decide cuándo y dónde ocurre su destrucción. es el creador y el destructor de todo.

## Bitácora de reflexión


[Link Apply](https://editor.p5js.org/Tomasm12/sketches/vV1TPqRDf)

<img width="689" height="505" alt="image" src="https://github.com/user-attachments/assets/f48c73c9-9a0f-40c5-a3a7-8ea8ae4776d4" />
<img width="673" height="517" alt="image" src="https://github.com/user-attachments/assets/7808d28e-3183-4ea2-90cc-1355a7e8aba7" />

<img width="706" height="515" alt="image" src="https://github.com/user-attachments/assets/9d59e6e0-1193-4d4e-9b17-1ad94cac5fa1" />

<img width="697" height="515" alt="image" src="https://github.com/user-attachments/assets/4b8e3acc-89f7-4b6c-b99c-5a85a18d8b68" />
