# Bitácora — Errantes / Umbral (deseo vs. miedo)
---

## 1. Ficha breve

### Tensión e intención
Exploro la tensión entre **el deseo y el miedo**. Espero que se manifieste como una atracción que nunca se resuelve ni en cercanía ni en huida: el sistema se acerca a lo que lo atrae, lo cruza, se asusta, se aleja, y vuelve a sentir el tirón.

### Tipos y cantidades
- **Errantes** — 240. Quien desea/teme.
- **Umbral** — entre 5 y 8, aleatorio por corrida. Aquello que se desea/teme.

### Reglas (matriz)

| fuerza que ejerce ↓ / recibe → | Errantes | Umbral |
|---|---|---|
| **Errantes** | repulsión débil, corto alcance | *(relación central — ver abajo)* |
| **Umbral** | indiferencia total (0) | repulsión moderada, alcance medio |

La celda que falta es la relación central, **Errantes → Umbral**: no monótona (atrae lejos, repele cerca). No entra en una matriz de un solo número porque su forma también es parte del diseño.

### Parámetros y justificación

| Parámetro | Valor | Justificación |
|---|---|---|
| Errantes | 240 | población grande = el impulso pesa mucho frente al objeto |
| Umbral | 5–8, aleatorio | el objeto de deseo/miedo es escaso |
| Núcleo de repulsión | 10px, universal | evita solapamiento total en cualquier relación |
| Errantes ↔ Errantes | alcance 40, fuerza −0.3 | textura de enjambre, relación de fondo |
| Umbral ↔ Umbral | alcance 160, fuerza −0.6 | mantiene los focos distribuidos, no colapsan en uno |
| Errantes → Umbral, atracción | alcance 260, pico +1.2 en ~120px | el deseo se siente de lejos y crece gradual |
| Errantes → Umbral, miedo | desde 46px hasta −2.2 | el miedo aparece de golpe, no gradual |
| Umbral → Errantes | 0 | el objeto no responde ni se entera |
| Fricción | 0.97 | baja: sostiene el ciclo, no lo deja resolverse |
| Velocidad máxima | 4 | tope de seguridad, no objetivo de diseño |

### Invariantes y variables
- **Invariantes** (entre corridas): la forma de la curva Errantes→Umbral, la indiferencia de Umbral, la proporción poblacional, los rangos de fricción/velocidad.
- **Variables** (entre corridas): semilla, posiciones y velocidades iniciales, número exacto de Umbral y sus posiciones.

---

## 2. Registro de pruebas

### Selección de la tensión
Comparé tres tensiones propuestas — deseo/miedo, pertenencia/individualidad, apego/autonomía — y elegí deseo/miedo porque:

> `era la que podría verse de manera muy evidente en materia`

### Pruebas

**A. Fricción alta.**
Cambiar `FRICTION` de `0.97` a `0.85` en `sketch.js` y reiniciar.
Predicción: el enjambre debería *asentarse* cerca del Umbral en vez de cruzarlo una y otra vez — el conflicto se "resolvería" en reposo.

**B. Deseo dominante vs. deseo débil.**
Probar `EU_PEAK_STRENGTH = 0.3` y luego `EU_PEAK_STRENGTH = 3.0`.
Pregunta: ¿en qué punto el miedo deja de poder frenar el acercamiento, o el deseo se vuelve casi imperceptible?

**C. Identidad entre corridas.**
Reiniciar 4–5 veces seguidas.

### Hallazgos
`es tal vez mas notable haciendolo mas pesado, por lo que al aumentar la fricción es mas evidente el funcionamiento`

---

## 3. Autoevaluación

| Criterio | Peso | Valoración | Aporte |
|---|---|---|---|
| La intención es clara y perceptible en el comportamiento | 20% | 75% | 15.0 |
| Tipos, cantidades, matriz y parámetros están justificados desde la intención | 25% | 88% | 22.0 |
| Comprendo y puedo modificar el funcionamiento técnico del sistema | 20% | 50% | 10.0 |
| El sistema produce variaciones con identidad reconocible | 15% | 80% | 12.0 |
| Experimenté, comparé, seleccioné y descarté con criterios claros | 10% | 100% | 10.0 |
| Puedo distinguir y sustentar lo diseñado y lo emergente | 10% | 100% | 10.0 |
| **Total** | **100%** | | **79.0 + 4** |

```
aporte = valoración x peso ÷ 100
nota propuesta = puntaje total ÷ 20
```

**Nota propuesta:** pendiente — faltan los tres valores marcados arriba.

---

¹ Confirmar viéndolo correr un par de minutos — verificado matemáticamente (curva de fuerza), no visualmente.
² Prueba: explicar sin mirar el código por qué Umbral casi no se mueve.
³ Se completa después de correr las pruebas A/B/C.
⁴ Prueba: señalar 2–3 comportamientos que aparecen al correrlo que nadie diseñó a propósito.
