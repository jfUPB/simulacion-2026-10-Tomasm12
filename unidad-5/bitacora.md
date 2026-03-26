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



**Capa de visualización:**








## Bitácora de aplicación 


## Bitácora de reflexión
