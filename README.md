# M13 · ¿El NPC ataca o se retira?

> **Hackatón IIA · UADE · 3.4.257.** Este README es la plantilla del grupo. En su fork, reemplacen todo lo que está entre `< >` y borren esta nota. Las bases están en `BASES.md` y su ficha en `problematicas/`.

**Integrantes:** Pedro Iturbide · Enzo Godoy
**IA declarada:** Claude
**Pista:** Neurona
**Publicación en LinkedIn:** https://lnkd.in/p/g2DiTJ3D

---

## 1. Formulación y PEAS

**La pregunta.** En este encuentro, ¿al NPC le conviene atacar? Clase positiva (SÍ) = el NPC ganaría si ataca → ataca. Clase negativa (NO) = el NPC perdería si ataca → se retira. En la tabla real, 1 = ganaría, 0 = perdería.

| PEAS | Su sistema |
|---|---|
| Medida de rendimiento | Que el NPC acierte su decisión en cada encuentro: atacar cuando tiene ventaja para ganar y retirarse cuando el enfrentamiento le es desfavorable (% de decisiones correctas = (encuentros en los que atacó y ganó + encuentros en los que se retiró y habría perdido) / total de encuentros × 100) |
| Entorno | Mapa del juego: jugador, NPCs aliados, coberturas, obstáculos y rutas de escape |
| Actuadores | Atacar, retirarse a una zona segura y pedir refuerzos |
| Sensores | Visión virtual, oído, lectura de su propia salud y detección de aliados cercanos |

**Objetivo y métrica.** Que el NPC enemigo decida en cada encuentro si atacar o retirarse de forma creíble y desafiante, evitando morir de forma absurda sin volverse cobarde, para que el combate resulte entretenido para el jugador. 
Porcentaje de decisiones correctas sobre el total de encuentros evaluados: % de decisiones correctas = (encuentros en los que atacó y ganó + encuentros en los que se retiró y habría perdido) / total de encuentros × 100

## 2. El sistema

**Cómo decide.** Neurona: Pesos: w1 = 4; w2 = -4; w3 = -0.5; Sesgo: b = 0

**Variables que usa y por qué.** Vida del NPC (%), Vida del jugador (%), Distancia (metros). Elegimos estas variables ya que cumplen con las características de que sean relevantes, medibles, no redundantes y no tiene atajos indeseados

**Cómo se corre en vivo.** Cuentas a mano. 
Caso 1 (75, 18, 19): x₁ = 0.75, x₂ = 0.18, x₃ = 0.6333

4·0.75 = 3.0000; −4·0.18 = −0.7200; −0.5·0.6333 = −0.3167
z = 3.0000 − 0.7200 − 0.3167 = 1.9633
e^(−1.9633) = 0.1404 → y = 1/1.1404 = 0.877 → Ataca

Caso 2 (50, 68, 6): x₁ = 0.50, x₂ = 0.68, x₃ = 0.2

4·0.50 = 2.0000; −4·0.68 = −2.7200; −0.5·0.2 = −0.1000
z = 2.0000 − 2.7200 − 0.1000 = −0.8200
e^(0.8200) = 2.2705 → y = 1/3.2705 = 0.306 → Huye

Caso 3 (59, 20, 1): x₁ = 0.59, x₂ = 0.20, x₃ = 0.0333

4·0.59 = 2.3600; −4·0.20 = −0.8000; −0.5·0.0333 = −0.0167
z = 2.3600 − 0.8000 − 0.0167 = 1.5433
e^(−1.5433) = 0.2137 → y = 1/1.2137 = 0.824 → Ataca

Caso 4 (85, 29, 5): x₁ = 0.85, x₂ = 0.29, x₃ = 0.1667

4·0.85 = 3.4000; −4·0.29 = −1.1600; −0.5·0.1667 = −0.0833
z = 3.4000 − 1.1600 − 0.0833 = 2.1567
e^(−2.1567) = 0.1157 → y = 1/1.1157 = 0.896 → Ataca

Caso 5 (51, 69, 1): x₁ = 0.51, x₂ = 0.69, x₃ = 0.0333

4·0.51 = 2.0400; −4·0.69 = −2.7600; −0.5·0.0333 = −0.0167
z = 2.0400 − 2.7600 − 0.0167 = −0.7367
e^(0.7367) = 2.0894 → y = 1/3.0894 = 0.324 → Huye

Caso 6 (13, 15, 13): x₁ = 0.13, x₂ = 0.15, x₃ = 0.4333

4·0.13 = 0.5200; −4·0.15 = −0.6000; −0.5·0.4333 = −0.2167
z = 0.5200 − 0.6000 − 0.2167 = −0.2967
e^(0.2967) = 1.3457 → y = 1/2.3457 = 0.426 → Huye

Caso 7 (64, 45, 28): x₁ = 0.64, x₂ = 0.45, x₃ = 0.9333

4·0.64 = 2.5600; −4·0.45 = −1.8000; −0.5·0.9333 = −0.4667
z = 2.5600 − 1.8000 − 0.4667 = 0.2933
e^(−0.2933) = 0.7458 → y = 1/1.7458 = 0.573 → Ataca

Caso 8 (86, 8, 14): x₁ = 0.86, x₂ = 0.08, x₃ = 0.4667

4·0.86 = 3.4400; −4·0.08 = −0.3200; −0.5·0.4667 = −0.2333
z = 3.4400 − 0.3200 − 0.2333 = 2.8867
e^(−2.8867) = 0.0558 → y = 1/1.0558 = 0.947 → Ataca

Caso 9 (52, 76, 18): x₁ = 0.52, x₂ = 0.76, x₃ = 0.6

4·0.52 = 2.0800; −4·0.76 = −3.0400; −0.5·0.6 = −0.3000
z = 2.0800 − 3.0400 − 0.3000 = −1.2600
e^(1.2600) = 3.5254 → y = 1/4.5254 = 0.221 → Huye

Caso 10 (43, 77, 26): x₁ = 0.43, x₂ = 0.77, x₃ = 0.8667

4·0.43 = 1.7200; −4·0.77 = −3.0800; −0.5·0.8667 = −0.4333
z = 1.7200 − 3.0800 − 0.4333 = −1.7933
e^(1.7933) = 6.0089 → y = 1/7.0089 = 0.143 → Huye

Caso 11 (45, 98, 30): x₁ = 0.45, x₂ = 0.98, x₃ = 1.0

4·0.45 = 1.8000; −4·0.98 = −3.9200; −0.5·1 = −0.5000
z = 1.8000 − 3.9200 − 0.5000 = −2.6200
e^(2.6200) = 13.7357 → y = 1/14.7357 = 0.068 → Huye

Caso 12 (99, 90, 14): x₁ = 0.99, x₂ = 0.90, x₃ = 0.4667

4·0.99 = 3.9600; −4·0.90 = −3.6000; −0.5·0.4667 = −0.2333
z = 3.9600 − 3.6000 − 0.2333 = 0.1267
e^(−0.1267) = 0.8810 → y = 1/1.8810 = 0.532 → Ataca

Caso 13 (15, 33, 25): x₁ = 0.15, x₂ = 0.33, x₃ = 0.8333

4·0.15 = 0.6000; −4·0.33 = −1.3200; −0.5·0.8333 = −0.4167
z = 0.6000 − 1.3200 − 0.4167 = −1.1367
e^(1.1367) = 3.1163 → y = 1/4.1163 = 0.243 → Huye

Caso 14 (32, 59, 7): x₁ = 0.32, x₂ = 0.59, x₃ = 0.2333

4·0.32 = 1.2800; −4·0.59 = −2.3600; −0.5·0.2333 = −0.1167
z = 1.2800 − 2.3600 − 0.1167 = −1.1967
e^(1.1967) = 3.3095 → y = 1/4.3095 = 0.232 → Huye

Total de aciertos: 78.57% (11/14)

**Dos casos de ejemplo.**

| Caso inventado | Valores | Salida del sistema |
|---|---|---|
| A | Vida del NPC 70%, vida del jugador 25%, distancia 10 m | SÍ |
| B | Vida del NPC 40%, vida del jugador 80%, distancia 20 m. | NO |

## 3. Medición

**Entrenamiento (casos 1–14)**

| | Predice SÍ | Predice NO |
|---|---|---|
| **Real SÍ** | VP = 4 | FN = 1 |
| **Real NO** | FP = 2 | VN = 7 |

**Prueba (casos 15–20)**

| | Predice SÍ | Predice NO |
|---|---|---|
| **Real SÍ** | VP = 3 | FN = 1 |
| **Real NO** | FP = 0 | VN = 2 |

| Métrica | Entrenamiento | Prueba |
|---|---|---|
| Exactitud | 78.57% | 83.33% |
| Precisión | 80% | 100% |
| Sensibilidad | 77.78% | 75% |
| Especificidad | 66.67% | 100% |

**Entrenamiento contra prueba.** Si, hay diferencia, porque en los datos de prueba fue mas certera, aunque hubieron menos casos. Creemos que esto es generalización ya que le fue mejor con los casos que no vio y supo como resolverlos adecuadamente.

## 4. El error más caro

El error mas caro es el Falso Positivo, debido a que el NPC muere de forma absurda y es lo que su objetivo dice que hay que evitar. El cambio en el sistema que haríamos en un futuro para cometer menos este error es actualizar los pesos con descenso de gradiente, multiplicar el error por un factor mayor cuando el NPC atacó y debía huir. El falso negativo hace que el jugador vea un enemigo cobarde y el combate pierda desafío. Es un error molesto, pero recuperable: el NPC puede volver o pedir refuerzos. En cambio, el falso positivo hace que el NPC muera de forma absurda, algo que rompe la credibilidad de la IA de forma más visible, por lo que comprendemos que este error se paga mas caro.

## 5. ¿Comparado contra qué?

**Línea base trivial.** Un sistema que siempre responde lo mismo tiene menor exactidud que nuestro sistema. Para ser especificos, un sistema que siempre responde que si tiene una exactitud del 35.71% y un sistema que siempre contesta que no tiene una exactitud del 64.29%. El nuestro tiene una exactitud de 78.57%.

| Checklist (Clase 6) | Respuesta |
|---|---|
| ¿Qué datos? | Se utilizaron 14 casos de entrenamiento, con variables como: "vida NPC", "Vida Jugador" y "Distancia". Etiqueta real: Si el NPC ganaba al atacar. |
| ¿Qué métrica? | Exactitud |
| ¿Comparado contra qué? | Contra un sistema que siempre huye y que siempre ataca. |
| ¿Quién lo midió? | Nosotros lo medimos, comparando la salida de la neurona con la columna real. Los pesos los propuso Claude y las cuentas se hicieron a mano, sin ver las etiquetas reales. |

## 6. Riesgo: el camino completo

Camino completo.
Datos: 14 encuentros de entrenamiento y 6 de prueba, con tres variables (vida del NPC, vida del jugador, distancia). 
Modelo: una neurona sigmoide con pesos propuestos por Claude con criterio del dominio (w₁ = 4, w₂ = -4, w₃ = -0.5, b = 0), no entrenados con los datos. Entradas normalizadas (vida/100, distancia/30).
Decisión: si z ≥ 0, el NPC ataca; si no, se retira.
Consecuencia: en entrenamiento acierta 78.57% (11 de 14). Cuando falla, o el NPC muere de forma absurda (falso positivo: casos 7 y 12) o se retira cuando podía ganar (falso negativo: caso 6).

Riesgo:
1. Falso positivo cerca del umbral. Los casos 7 y 12 tenían z = 0.29 y 0.13, casi en el límite, y ahí la neurona atacó y se equivocó. En el juego, el jugador vería al NPC lanzarse a un combate perdido con vida alta de ambos, justo lo que el objetivo dice que hay que evitar (“morir de forma absurda”). Mitigación: subir el umbral (b = -0.5) o hacer que el NPC pida refuerzos cuando z es chico.

## 7. Bitácora

**Uso de la IA**

| # | Qué le pedimos | Qué aceptamos | Qué rechazamos y por qué |
|---|---|---|---|
| 1 | Crear una neurona sobre el comportamiento de un NPC (“¿ataca o se retira?”) con su PEAS, objetivo y métrica, siguiendo el documento de la materia | La estructura general del trabajo y la observación de que la pregunta SÍ/NO dejaba afuera un caso (atacar y perder) | Que hiciera todo de una sola vez (neurona, PEAS, matriz, etc.). Queríamos avanzar por partes y decidir nosotros cada paso |
| 2 | Empezar solo por el PEAS, el objetivo y la métrica, sin crear nada sin consultarnos | Empezar solo por el PEAS, el objetivo y la métrica, sin crear nada sin consultarnos | No rechazamos |
| 3 | Crear dos casos de ejemplo | Un caso en el que el NPC ataca y otro en el que se retira | No rechazamos |
| 4 | Pesos y sesgo para una neurona, dados los datos de entrenamiento. Nos dio 2 conjuntos de datos, uno utilizando la lógica y otros completamente aleatorios | La estructura de la neurona sigmoide y la normalización | Rechazamos pesos aleatorios, porque nos servía mas que la IA utilice la lógica para plantear los pesos y sesgo |
| 5 | Todas las cuentas a mano de los 14 casos | Las cuentas de los casos en donde los pesos y el sesgo fueron propuestos por lógica | Las cuentas donde los pesos y el sesgo fueron propuestos aleatoriamente |
| 6 | Resolver el set de prueba (casos 15 a 20) | Resolverlo con los mismos pesos | No rechazamos |
| 7 | Cambiar los umbrales a z = -2 y 2 y ver los errores, para evaluar el error más caro | Usarlo como análisis para elegir el error más caro | No adoptaron los umbrales ±2 como modelo final |
| 8 | Qué cambios reducen el falso positivo | Se propuso adoptar b = -0.5 como modelo v2, con ajuste manual | No rechazamos |
| 9 | Ayuda con la corrida 1 (5,5/10)| Las correcciones | no rechazamos |

**Corridas contra el evaluador**

| Corrida | Nota | Agujero que encontró | Qué cambiamos |
|---|---|---|---|
| 1 | 5,5 | ¿Cuánto suma VP+FN en tu matriz de entrenamiento y cuántos casos entre el 1 y el 14 tienen resultado real SÍ? ¿Cuál es el modelo real: los pesos de la sección 2 o los que cambiaron con descenso de gradiente? ¿Qué le pasa al jugador cuando el NPC se retira en un combate que podía ganar? | Conteo correcto de los resultados de entrenamiento, corrigiendo los porcentajes de las métricas. Corrección del punto 4, donde afirmabamos que habiamos hecho el cambio de pesos y nos contradeciamos con lo que nuestro sistema decia. El jugador percibe un NPC cobarde y el combate pierde desafío. Es menos grave que el falso positivo porque es recuperable.|
