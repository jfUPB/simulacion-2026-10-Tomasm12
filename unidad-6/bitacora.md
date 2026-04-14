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


## Bitácora de reflexión
