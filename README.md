# M13 · ¿El NPC ataca o se retira?

> **Hackatón IIA · UADE · 3.4.257.** Este README es la plantilla del grupo. En su fork, reemplacen todo lo que está entre `< >` y borren esta nota. Las bases están en `BASES.md` y su ficha en `problematicas/`.

**Integrantes:** Pedro Iturbide · Enzo Godoy
**IA declarada:** Claude
**Pista:** Neurona
**Publicación en LinkedIn:** <link>

---

## 1. Formulación y PEAS

**La pregunta.** En este encuentro, ¿el NPC gana si ataca?” Si la respuesta es SÍ, ataca. Si es NO, huye.

| PEAS | Su sistema |
|---|---|
| Medida de rendimiento | Que el NPC acierte su decisión en cada encuentro: atacar cuando tiene ventaja para ganar y retirarse cuando el enfrentamiento le es desfavorable (% de decisiones correctas = (encuentros en los que atacó y ganó + encuentros en los que se retiró y habría perdido) / total de encuentros × 10) |
| Entorno | Mapa del juego: jugador, NPCs aliados, coberturas, obstáculos y rutas de escape |
| Actuadores | Atacar, retirarse a una zona segura y pedir refuerzos |
| Sensores | Visión virtual, oído, lectura de su propia salud y detección de aliados cercanos |

**Objetivo y métrica.** Que el NPC enemigo decida en cada encuentro si atacar o retirarse de forma creíble y desafiante, evitando morir de forma absurda sin volverse cobarde, para que el combate resulte entretenido para el jugador. 
Porcentaje de decisiones correctas sobre el total de encuentros evaluados: % de decisiones correctas = (encuentros en los que atacó y ganó + encuentros en los que se retiró y habría perdido) / total de encuentros × 100

## 2. El sistema

**Cómo decide.** Neurona: Pesos: w1 = 4; w2 = -4; w3 = -0.5; Sesgo: b = 0

**Variables que usa y por qué.** Vida del NPC (%), Vida del jugador (%), Distancia (metros).

**Cómo se corre en vivo.** Cuentas a mano. 


**Dos casos de ejemplo.**

| Caso inventado | Valores | Salida del sistema |
|---|---|---|
| A | Vida del NPC 70%, vida del jugador 25%, distancia 10 m | SÍ |
| B | Vida del NPC 40%, vida del jugador 80%, distancia 20 m. | NO |

## 3. Medición

**Entrenamiento (casos 1–14)**

| | Predice SÍ | Predice NO |
|---|---|---|
| **Real SÍ** | VP =  | FN = <> |
| **Real NO** | FP = <> | VN = <> |

**Prueba (casos 15–20)**

| | Predice SÍ | Predice NO |
|---|---|---|
| **Real SÍ** | VP = <> | FN = <> |
| **Real NO** | FP = <> | VN = <> |

| Métrica | Entrenamiento | Prueba |
|---|---|---|
| Exactitud | <> | <> |
| Precisión | <> | <> |
| Sensibilidad | <> | <> |
| Especificidad | <> | <> |

**Entrenamiento contra prueba.** <¿Hay diferencia? ¿Por qué? ¿Sobreajuste o generalización?>

## 4. El error más caro

<¿Es peor el falso positivo o el falso negativo en su problema? ¿Por qué, según la ficha? ¿Qué cambiaron en el sistema para cometer menos ese error?>

## 5. ¿Comparado contra qué?

**Línea base trivial.** <Qué resultado saca un sistema que responde siempre lo mismo, y cómo se compara el suyo.>

| Checklist (Clase 6) | Respuesta |
|---|---|
| ¿Qué datos? | <> |
| ¿Qué métrica? | <> |
| ¿Comparado contra qué? | <> |
| ¿Quién lo midió? | <> |

## 6. Riesgo: el camino completo

<Datos → modelo → decisión → consecuencia aplicado a su caso. Al menos un riesgo concreto.>

## 7. Bitácora

**Uso de la IA**

| # | Qué le pedimos | Qué aceptamos | Qué rechazamos y por qué |
|---|---|---|---|
| 1 | <> | <> | <> |

**Corridas contra el evaluador**

| Corrida | Nota | Agujero que encontró | Qué cambiamos |
|---|---|---|---|
| 1 | <> | <> | <> |
