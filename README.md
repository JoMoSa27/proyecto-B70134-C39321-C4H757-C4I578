# *Battle royale 2D multijugador en línea*

Juego de acción multijugador en línea, en tiempo real, con vista cenital en dos dimensiones. De 2 a 16 jugadores compiten simultáneamente en un mapa cerrado que se reduce por fases. Gana el último superviviente.

**Entrega 1 — Selección y aprobación del juego**

---

## 1. Descripción general

Los jugadores se conectan desde el navegador a una misma partida. Todos aparecen en el mapa con el mismo armamento inicial y deben desplazarse hacia **armerías**: puntos fijos y conocidos del escenario donde reaparecen armas de forma periódica. La zona jugable se contrae en fases sucesivas, forzando el encuentro entre los supervivientes.

El juego parte del género *battle royale* en dos dimensiones, pero introduce modificaciones orientadas a **eliminar el azar como factor de resultado**, que es la característica que más lo distingue de sus referentes (ver reglas 6, 7 y 8).

---

## 2. Reglas del juego

Las reglas se describen con el nivel de precisión necesario para ser implementadas sin ambigüedad. Los valores numéricos indicados son iniciales y quedarán sujetos a ajuste durante el desarrollo.

### Regla 1 — Movimiento continuo

Mientras el jugador mantiene pulsada una tecla de dirección (W, A, S, D), su personaje se desplaza en esa dirección a velocidad constante. El servidor recalcula la posición en cada tick de simulación y verifica que el desplazamiento no atraviese obstáculos ni los límites del mapa; si la trayectoria colisiona, el jugador se detiene en el punto de contacto.

**Efecto sobre el estado:** modifica la posición del jugador.

### Regla 2 — Apuntado y disparo

El jugador apunta con el puntero del ratón y dispara al pulsar el botón principal. El disparo se produce únicamente si el arma equipada tiene munición disponible y si ha transcurrido su intervalo de cadencia desde el disparo anterior. Al dispararse, se genera un **proyectil** con posición inicial en el jugador, dirección hacia el punto apuntado y velocidad propia del arma. El proyectil avanza en cada tick hasta impactar o agotar su alcance máximo.

**Efecto sobre el estado:** reduce en 1 la munición del arma, crea una entidad proyectil, reinicia el temporizador de cadencia y actualiza el ángulo de apuntado del jugador.

### Regla 3 — Impacto y eliminación

Cuando un proyectil colisiona con un jugador distinto de quien lo disparó, se resta el daño del arma a la vida del objetivo y el proyectil se elimina. Si un proyectil colisiona con un obstáculo, se elimina sin causar daño. Cuando la vida de un jugador llega a 0 o menos, queda eliminado, deja de ser controlable y pasa a modo espectador; su posición de eliminación se registra para la tabla de resultados.

**Efecto sobre el estado:** modifica la vida del objetivo, destruye la entidad proyectil, y en caso de eliminación cambia el estado del jugador, reduce el contador de supervivientes e incrementa las eliminaciones del atacante.

### Regla 4 — Reducción de la zona segura

Cada 90 segundos, la zona segura entra en una nueva fase: su radio se reduce y su centro se desplaza hacia una nueva posición. Un jugador que se encuentre fuera del radio de la zona segura pierde vida de forma continua, con una intensidad que aumenta en cada fase (2, 4, 7 y 12 puntos de vida por segundo en las fases 1 a 4 respectivamente). El daño de zona puede eliminar a un jugador según la Regla 3.

**Efecto sobre el estado:** modifica el centro, el radio y la fase de la zona; reduce la vida de los jugadores situados fuera de ella.

### Regla 5 — Condición de fin

La partida termina cuando queda un único jugador con vida, que se declara ganador. Si el último par de jugadores es eliminado en el mismo tick de simulación, la partida termina en **empate**. Al finalizar se muestra una tabla de posiciones ordenada de forma inversa al orden de eliminación, indicando para cada participante su puesto, número de eliminaciones y tiempo de supervivencia.

**Efecto sobre el estado:** cambia el estado de la partida a finalizada y genera la tabla de resultados.

---

### Reglas de identidad propia

Las siguientes reglas constituyen la adaptación del equipo respecto al género de origen. Las tres comparten un propósito común: **trasladar el resultado de la partida desde el azar hacia la decisión y la habilidad del jugador.**

### Regla 6 — Armerías fijas *(identidad propia)*

**En el género de origen**, las armas aparecen en posiciones y con tipos aleatorios, de modo que un jugador afortunado puede obtener ventaja decisiva sobre otro sin que medie ninguna decisión.

**En este juego se elimina esa aleatoriedad.** Todos los jugadores comienzan con el mismo arma básica. Las demás armas solo pueden obtenerse en **armerías**: 6 posiciones fijas del mapa, idénticas en cada partida, visibles permanentemente en el minimapa e identificadas por el arma concreta que proveen. Un jugador que se sitúa sobre una armería disponible recoge su arma de forma automática. Tras ser recogida, la armería queda inactiva durante 45 segundos, transcurridos los cuales vuelve a estar disponible.

**Efecto sobre el estado:** modifica el arma equipada y la munición del jugador; marca la armería como inactiva e inicia su temporizador de reaparición.

**Consecuencia de diseño:** el acceso a mejor armamento deja de depender de la suerte y pasa a depender del conocimiento del mapa, de la elección de ruta y de la capacidad de disputar posiciones que todos los jugadores saben valiosas.

### Regla 7 — Marca del cazador *(identidad propia)*

El jugador que acumule más eliminaciones en la partida recibe la **marca del cazador**: su posición se muestra de forma permanente en el minimapa de todos los demás jugadores. La marca se reasigna automáticamente en cuanto otro jugador iguale o supere ese número de eliminaciones, y no se aplica mientras ningún jugador haya conseguido eliminaciones.

**Efecto sobre el estado:** asigna el identificador del jugador marcado en el estado de la partida, lo que modifica la información de posiciones transmitida al resto de clientes.

**Consecuencia de diseño:** introduce un mecanismo de equilibrio basado en el mérito y no en la compensación aleatoria. Genera además una decisión estratégica real: eliminar rivales otorga ventaja material pero incrementa la exposición, de modo que el jugador debe valorar cuándo conviene combatir y cuándo evitar el enfrentamiento.

### Regla 8 — Zona anunciada *(identidad propia)*

El centro y el radio de la **siguiente** fase de la zona se muestran en el minimapa 20 segundos antes de que la contracción comience.

**Efecto sobre el estado:** expone en el estado de partida el centro y radio de la fase siguiente durante la ventana de anuncio.

**Consecuencia de diseño:** el reposicionamiento deja de ser una reacción a un suceso imprevisto y se convierte en una decisión anticipada. Los jugadores pueden optar por desplazarse pronto en busca de posición ventajosa, o permanecer para disputar una armería asumiendo el riesgo. Se elimina el componente de sorpresa que en el género de origen puede eliminar a un jugador por causas ajenas a su desempeño.

---

## 3. Justificación de complejidad

### 3.1 Mecánica de progreso: tiempo real

El juego funciona **en tiempo real**: todos los jugadores actúan simultáneamente y de forma continua, sin alternancia de turnos.

El servidor mantiene la autoridad exclusiva sobre el estado y ejecuta un bucle de simulación a **30 ticks por segundo**, en el que procesa las entradas recibidas, actualiza posiciones, avanza proyectiles, resuelve colisiones, aplica daño de zona y evalúa la condición de fin. Los clientes envían intenciones y reciben instantáneas del estado **20 veces por segundo**.

Para que los controles respondan de forma inmediata pese a la latencia de red, el cliente aplica **predicción** de su propio movimiento y **reconciliación** con las correcciones del servidor: numera cada entrada enviada, la aplica localmente sin esperar respuesta, y al recibir la corrección adopta la posición autoritativa y reaplica las entradas aún no confirmadas.

### 3.2 Estado que debe sincronizarse

| Ámbito | Información | Frecuencia de cambio |
|---|---|---|
| **Por jugador** | Identificador, posición (x, y), ángulo de apuntado, vida, arma equipada, munición, estado (vivo / eliminado), número de eliminaciones | Cada tick |
| **Proyectiles** | Identificador, propietario, posición, dirección, velocidad, daño, distancia recorrida | Cada tick |
| **Armerías** | Posición (estática), arma que proveen, disponibilidad, temporizador de reaparición | Al ser recogidas |
| **Zona** | Centro y radio actuales, fase, tiempo restante de fase, centro y radio de la fase siguiente durante la ventana de anuncio | Por fase y por segundo |
| **Partida** | Estado (espera / en curso / finalizada), número de supervivientes, portador de la marca del cazador, cronómetro, tabla de resultados | Según evento |

**Restricción de escalabilidad y seguridad.** Transmitir el estado completo a 16 clientes 20 veces por segundo es inviable en ancho de banda, y además permitiría a cualquier jugador leer las posiciones de sus rivales inspeccionando el tráfico del navegador. Por ello el servidor implementa **gestión de interés**: divide el mapa en una rejilla espacial y transmite a cada cliente únicamente las entidades situadas dentro de su radio de percepción. Las entidades fuera de ese área se omiten del envío, no se marcan como ocultas.

Las dos excepciones son deliberadas y forman parte del diseño: el portador de la marca del cazador (Regla 7) y la información de zona (Regla 8) se transmiten a todos los clientes.

### 3.3 Jugadores simultáneos

**De 2 a 16 jugadores** en una misma partida.

El sistema es funcional desde 2 participantes: con menos de 4 jugadores, la duración de cada fase de zona se reduce de 90 a 45 segundos para que la partida mantenga un ritmo adecuado. El límite superior de 16 responde a la capacidad de ancho de banda del servidor y se validará mediante medición durante el desarrollo.

### 3.4 Cumplimiento del criterio de mérito sobre azar

El juego contiene un único elemento de aleatoriedad: **la posición inicial de aparición de los jugadores**. Todo lo demás es determinista.

- Las armas están en posiciones fijas y conocidas por todos (Regla 6).
- El armamento inicial es idéntico para todos los participantes (Regla 6).
- La contracción de la zona se anuncia con antelación (Regla 8).
- No existe aleatoriedad en el daño, la dispersión ni la resolución de impactos: el proyectil es una entidad simulada cuya trayectoria depende exclusivamente de la puntería del jugador.

El resultado depende por tanto de la puntería, la elección de ruta, el conocimiento del mapa, la gestión del posicionamiento frente a la zona y la decisión sobre cuándo combatir o evitar el combate.

---

## 4. Equipo y roles

| Integrante | Rol | Responsabilidad principal |
|---|---|---|
| Alejandro Quesada | Servidor | Servidor, protocolo de mensajes, sincronización de estado, y manejo de salas y reconexión. |
| Josué Morera | Cliente | Lógica del juego del lado del cliente, modelo de estado e interacción en el tablero. |
| Jean Aguilar | Diseño | Wireframes e identidad visual, documentación (`design.md`, `README.md`), y verificación de que las reglas funcionen y de que nada se rompa entre entregas. |
| Adrián Rodríguez | Frontend | Estructura semántica del HTML, diseño responsive y cumplimiento de validadores y linters. Lidera las entregas de HTML y CSS. |


## 5. Tecnologías previstas

- **Lenguaje:** TypeScript (cliente y servidor)
- **Servidor:** Node.js con WebSockets
- **Cliente:** Canvas 2D en navegador
- **Estructura:** monorepo con paquete compartido de lógica de simulación entre cliente y servidor
- **Control de versiones e integración:** Git, GitHub, GitHub Actions

---

## 6. Estado del proyecto

En fase de propuesta. Pendiente de aprobación de la Entrega 1.

---

## 7. Originalidad

El proyecto parte del género *battle royale* en dos dimensiones e introduce las adaptaciones descritas en las reglas 6, 7 y 8. Todo el arte, mapa, armas, interfaz y nomenclatura son originales. No se reutilizará código fuente de implementaciones existentes del género.
