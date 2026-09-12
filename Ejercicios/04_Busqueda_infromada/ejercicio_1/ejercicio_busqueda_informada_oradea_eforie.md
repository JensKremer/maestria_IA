# Ejercicio 1 — Comparación de Greedy y A* en el mapa de Rumania

## 1. Instancia elegida

Para este ejercicio se eligió la siguiente ruta:

- Origen: `Oradea`
- Destino: `Eforie`

---

## 2. Heurística

Al ejecutar:

```text
python 02_heuristics.py --from-city Oradea --to Eforie
```

se obtuvieron, entre otros, los siguientes valores relevantes para las rutas encontradas:

| Ciudad | h(n) |
|---|---:|
| Oradea | 513 |
| Sibiu | 391 |
| Fagaras | 301 |
| Rimnicu Vilcea | 349 |
| Pitesti | 253 |
| Bucharest | 166 |
| Urziceni | 120 |
| Hirsova | 64 |
| Eforie | 0 |

---

## 3. Subgrafo relevante

En el siguiente diagrama aparecen las ciudades de los caminos encontrados por Greedy y A*. Entre paréntesis se muestra el valor de `h(n)` de cada ciudad.

```text
Oradea (513)
    |
   151
    |
Sibiu (391)
   /       \
 99         80
 /           \
Fagaras     Rimnicu Vilcea
 (301)          (349)
   |              |
  211             97
   |              |
   |           Pitesti (253)
   |              |
   |             101
   |              |
   +-------- Bucharest (166)
                  |
                  85
                  |
             Urziceni (120)
                  |
                  98
                  |
              Hirsova (64)
                  |
                  86
                  |
              Eforie (0)
```

Greedy tomó la rama por Fagaras, mientras que A* terminó utilizando la rama por Rimnicu Vilcea y Pitesti.

---

## 4. Resultados

| Algoritmo | Status | Path | Depth | Cost | Expanded | Generated | Frontier máx. |
|---|---|---|---:|---:|---:|---:|---:|
| Greedy best-first | success | Oradea → Sibiu → Fagaras → Bucharest → Urziceni → Hirsova → Eforie | 6 | 730 km | 6 | 18 | 7 |
| A* | success | Oradea → Sibiu → Rimnicu Vilcea → Pitesti → Bucharest → Urziceni → Hirsova → Eforie | 7 | 698 km | 11 | 32 | 6 |

Aunque Greedy encontró un camino con una carretera menos, su recorrido total fue 32 km más largo que el encontrado por A*.

---

## 5. Análisis

En esta prueba Greedy y A* no encontraron el mismo camino. Greedy llegó a Eforie recorriendo 730 km, mientras que A* encontró una ruta de 698 km.

La diferencia se entiende mejor observando qué toma en cuenta cada algoritmo. Greedy se guía solamente por qué ciudad parece estar más cerca del destino `h(n)`. A* utiliza `g(n) + h(n)`, por lo que además de la estimación al destino también considera todo lo que ya se ha recorrido.

Un punto donde esto se puede observar es después de llegar a Sibiu. Desde ahí, Fagaras tiene `h = 301` y Rimnicu Vilcea tiene `h = 349`. Para Greedy, Fagaras parece ser la mejor opción porque su valor heurístico es menor, así que continúa por esa rama y llega a Bucharest pasando por Fagaras.

A* también puede considerar primero Fagaras. Desde Sibiu, llegar a Fagaras da aproximadamente:

```text
g = 250
h = 301
f = 551
```

Mientras que para Rimnicu Vilcea:

```text
g = 231
h = 349
f = 580
```

Sin embargo, A* no descarta la otra alternativa. Después de continuar por Fagaras, llegar a Bucharest por ese camino produce:

```text
g = 461
h = 166
f = 627
```

En ese momento todavía existe la alternativa de Rimnicu Vilcea con `f = 580`, así que A* regresa a esa opción y la sigue explorando. Finalmente encuentra el camino por Rimnicu Vilcea y Pitesti, que termina siendo más barato.

Esto también muestra por qué Greedy puede encontrar un camino más caro aunque la heurística sea admisible. Que `h(n)` no sobreestime la distancia restante ayuda a estimar qué tan cerca está una ciudad del objetivo, pero Greedy ignora por completo el costo que ya lleva acumulado. Por eso puede tomar decisiones que parecen buenas localmente, pero que al final producen una ruta más costosa.

En el camino encontrado por A*, los valores de `f` fueron:

```text
Oradea            f = 513
Sibiu             f = 542
Rimnicu Vilcea    f = 580
Pitesti           f = 581
Bucharest         f = 595
Urziceni          f = 634
Hirsova           f = 676
Eforie            f = 698
```

Se puede ver que `f` no disminuye conforme se avanza por la ruta.

También se observa que A* hizo más trabajo que Greedy en esta instancia. Greedy expandió 6 nodos y A* expandió 11. Greedy llegó más rápido a una solución porque siguió la ciudad que parecía acercarlo más al destino, pero A* exploró más alternativas para encontrar una ruta de menor costo.

