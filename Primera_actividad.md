# Bitácora de proceso — Navegar la Incertidumbre

## Punto de partida

Ya tenía construido el sistema base en p5.js: una malla de nodos conectados que se mueve con ruido Perlin, propaga energía entre vecinos con una distribución normal, y ocasionalmente salta con un vuelo de Lévy. Visualmente funcionaba bien en reposo. El problema era el quinto momento del encargo: **influencia**. El mouse no parecía tener ningún efecto sobre el sistema, y esa era justo la pieza que conectaba al visitante con la idea de perturbar un equilibrio.

## Primeras hipótesis (con ChatGPT)

Antes de traer el problema aquí, ya había estado trabajando el diagnóstico con ChatGPT. Las hipótesis que salieron de esa conversación fueron, en orden: que el valor de amplitud del movimiento (`amp = 25 * (n.energy + n.memory)`) era demasiado bajo; que la memoria subía muy lento; que la energía se disipaba demasiado rápido; y, finalmente, que nunca había conectado tres funciones que ya existían en el código (`randomWalk()`, `rareEvent()`, `drawCursor()`) pero que jamás se llamaban dentro de `draw()`. Ese último hallazgo era real y verificable con solo leer el archivo, así que lo traté como el primer punto de corrección.

## Cambio de herramienta y depuración real

Al retomar el archivo completo, apliqué esa corrección de conexión de funciones y subí los parámetros de amplitud, energía y memoria como se había sugerido. Pero seguía sin ver ningún cambio al mover el mouse. En vez de seguir ajustando valores a ciegas, decidí construir una prueba de depuración directamente sobre el canvas: un overlay que mostraba en tiempo real la posición del mouse, la distancia al nodo más cercano, su energía, su memoria, y un anillo amarillo dibujando el tamaño exacto que ese nodo debería tener según la fórmula real de `drawNodes()`.

## El hallazgo

Ese overlay reveló algo que ninguna hipótesis anterior había contemplado: **el mouse sí estaba funcionando perfectamente**. La energía del nodo más cercano se mantenía siempre en `1.9500` — un valor que no era un error, sino el resultado exacto de `2 * 0.975` (el tope máximo de energía, disipado un frame después). El sistema estaba saturado al máximo cerca del cursor. El problema nunca fue que el mouse no hiciera nada; era que el efecto, aunque real y grande, estaba siendo opacado por el movimiento ambiental de fondo (el jitter de ruido Perlin, demasiado activo incluso sin interacción).

## Corrección aplicada

Con el diagnóstico correcto, la solución fue quitar el overlay de depuración y bajar la intensidad del movimiento ambiental: reduje el rango del ruido Perlin de ±16 a ±4, bajé la velocidad de evolución del campo (`fieldTime`) a la mitad, y reduje tanto la probabilidad como la magnitud de `randomWalk()`. El efecto del mouse no cambió — porque ya funcionaba — pero ahora sí es perceptible frente a un fondo más calmado.

## Repulsión y cierre conceptual

Como último detalle, añadí una fuerza de repulsión real: cada nodo calcula un vector que apunta lejos del cursor cuando está dentro de un radio de 160px, y ese vector se suma a su posición objetivo (no a su posición directa), de forma que el resorte existente lo lleva ahí con inercia. Al alejarse el mouse, la repulsión desaparece y el nodo vuelve solo a su equilibrio.

Ese último ajuste me llevó a redefinir el concepto que sostiene toda la pieza: en vez de dejarlo en un enunciado abstracto de "incertidumbre", lo ligué al **principio de Le Châtelier** — un sistema en equilibrio que, perturbado externamente, se reacomoda para alcanzar un nuevo estado de equilibrio. El visitante no controla el sistema; actúa como el reactivo externo que lo perturba.

## Uso de IA

Usé dos asistentes de IA en momentos distintos del proceso, con roles distintos:

- **ChatGPT**, en una fase inicial, para generar hipótesis rápidas sobre por qué el mouse no parecía tener efecto. Varias de esas hipótesis resultaron ser conjeturas razonables pero no verificadas contra el comportamiento real del sistema.
- **Claude**, para la depuración real (construir el overlay de prueba directamente en el canvas y confirmar con datos, no con suposiciones, dónde estaba el problema), para aplicar las correcciones de código, para añadir la repulsión, y para ayudarme a redactar tanto el informe de concepto como esta misma bitácora.

La diferencia entre ambas fases fue clave: pasar de ajustar números por conjetura a instrumentar el propio sistema para verlo pensar fue lo que realmente resolvió el problema.

## Prototipo

`sketch.js` — código fuente final del sistema.
https://editor.p5js.org/n4ndeZzz/full/pcj8TJbi-

## Autoevaluación de la rúbrica

| Criterio | Cumplo | Evidencia |
|---|---|---|
| **Encargo completo:** interpreto los cinco momentos dentro de un mismo sistema visual. | ✅ | Los 5 momentos coexisten como comportamientos simultáneos de una misma red de nodos, no como escenas separadas: posibilidad en `noise()` (líneas 168, 188, 210 de `updateNodes()`), tendencia en `n.memory +=` (línea 302), normalidad en `randomGaussian()` (línea 394, `propagateEnergy()`), excepción en el bloque `// LEVY FLIGHT` (línea 421), e influencia en `updateMouseInfluence()` (línea 471) más la repulsión en `updateNodes()` (líneas 255-267). |
| **Simulación con intención:** utilizo al menos tres conceptos de la unidad para comunicar las ideas del encargo. | ✅ | Uso cuatro: ruido Perlin (`noise()`, líneas 168/188/210), distribución normal (`randomGaussian()`, línea 394), vuelo de Lévy (línea 421) y caminata aleatoria (`randomWalk()`, línea 684). Cada uno cumple un rol conceptual distinto, no son decorativos. |
| **Interacción significativa:** la interacción modifica el comportamiento o las probabilidades del sistema, que también funciona sin intervención. | ✅ | `updateMouseInfluence()` (línea 471) cambia `n.probability` (líneas 510 y 536), una regla real de propagación, no solo un color; la repulsión (líneas 255-267) cambia el objetivo de movimiento. Sin mouse, `randomWalk()` (684), `rareEvent()` (703) y `propagateEnergy()` (378) siguen corriendo igual. |
| **Prototipo funcional:** la experiencia puede ejecutarse y recorrerse completa sin errores que impidan comprenderla. | ✅ | El archivo no tiene errores de sintaxis y todas las funciones están conectadas y se llaman una sola vez. Confirmé en vivo que el movimiento, tamaño y desplazamiento por el mouse funcionan. |
| **Proceso documentado:** la bitácora evidencia avances, decisiones, dificultades, soluciones, uso de IA y enlace al prototipo. | ✅ | Esta misma bitácora recoge el diagnóstico inicial con ChatGPT, el hallazgo real del problema mediante el overlay de depuración, las correcciones aplicadas, la redefinición conceptual hacia Le Châtelier, y la reflexión sobre el uso de IA. Falta únicamente completar el enlace al prototipo en la sección anterior. |
