# Ejercicio 1 — Comparación de BFS, UCS, DFS, DLS e IDS en el mapa de Rumania

## 1. Instancia elegida

Para este ejercicio se eligió la siguiente pareja de ciudades:

- **Origen:** Zerind
- **Destino:** Craiova

La misma pareja se utilizó para ejecutar los cinco algoritmos de búsqueda no informada: **BFS, UCS, DFS, DLS e IDS**.

---

## 2. Subgrafo relevante

El siguiente diagrama muestra las ciudades que aparecen en los caminos obtenidos y las conexiones relevantes entre ellas.

```text
Zerind
  |
 75
  |
Arad
  |
140
  |
Sibiu --------99-------- Fagaras
  |                         |
 80                        211
  |                         |
Rimnicu Vilcea --97-- Pitesti --101-- Bucharest
  |                     |
146                   138
  |                     |
  +-------- Craiova -----+
```

---

## 3. Resultados

| Algoritmo | Status | Path | Depth | Cost | Expanded | Generated | Frontier máx. |
|---|---|---|---:|---:|---:|---:|---:|
| BFS | success | Zerind → Arad → Sibiu → Rimnicu Vilcea → Craiova | 4 | 441 km | 7 | 17 | 3 |
| UCS | success | Zerind → Arad → Sibiu → Rimnicu Vilcea → Craiova | 4 | 441 km | 10 | 26 | 4 |
| DFS | success | Zerind → Arad → Sibiu → Fagaras → Bucharest → Pitesti → Craiova | 6 | 764 km | 7 | 20 | 6 |
| DLS (`limit=2`) | cutoff | — | — | — | 3 | 8 | 5 |
| DLS (`limit=4`) | success | Zerind → Arad → Sibiu → Rimnicu Vilcea → Craiova | 4 | 441 km | 6 | 12 | 7 |
| IDS | success | Zerind → Arad → Sibiu → Rimnicu Vilcea → Craiova | 4 | 441 km | 16 | 42 | 7 |

---

## 4. Análisis

En este caso, BFS y UCS encontraron el mismo camino:

`Zerind → Arad → Sibiu → Rimnicu Vilcea → Craiova`

Aunque llegaron al mismo resultado, cada algoritmo lo hace de una forma diferente. BFS busca primero los caminos con menor profundidad, es decir, los que tienen menos carreteras. En este caso encontró una ruta de 4 carreteras. UCS, en cambio, toma en cuenta el costo acumulado de cada camino y busca la opción con menor distancia total. Para esta ruta el costo fue de 441 km, así que en este ejemplo ambos algoritmos terminaron encontrando la misma solución.

Con DFS el resultado fue diferente. Como este algoritmo sigue una rama lo más profundo posible antes de regresar y probar otra opción, encontró primero la siguiente ruta:

`Zerind → Arad → Sibiu → Fagaras → Bucharest → Pitesti → Craiova`

La ruta sí llega al destino, pero necesita 6 carreteras y recorre 764 km. Esto muestra que DFS puede encontrar una solución rápidamente dependiendo del orden en el que explora los nodos, pero no necesariamente será la ruta más corta o la de menor costo.

Para DLS probé varios límites. Con `--limit 2` el resultado fue `cutoff`, ya que no era posible llegar a Craiova con esa profundidad. Después probé `--limit 3` y el resultado siguió siendo `cutoff`. Finalmente, con `--limit 4` sí encontró la solución:

`Zerind → Arad → Sibiu → Rimnicu Vilcea → Craiova`

Esto tiene sentido porque la solución encontrada por BFS tiene una profundidad de 4, por lo que un límite menor no permite llegar al destino.

IDS también encontró el mismo camino de profundidad 4 que BFS. La diferencia es que IDS repite la búsqueda varias veces aumentando el límite poco a poco hasta encontrar la solución. Por esta razón terminó expandiendo 16 nodos y generando 42, que es bastante más que BFS. A cambio, puede encontrar una solución con la misma profundidad que BFS sin tener que mantener una frontera tan grande.

También me llamó la atención que UCS expandió 10 nodos mientras que BFS expandió solamente 7, aunque ambos terminaron con el mismo camino. Esto ocurre porque UCS decide qué nodo explorar basándose en el costo acumulado, por lo que puede revisar otras rutas antes de confirmar cuál es la de menor costo.


### Conclusión

Con este ejercicio pude ver de forma más clara que cada algoritmo puede comportarse de manera diferente aunque todos estén trabajando sobre el mismo mapa. BFS, UCS e IDS encontraron la misma ruta de Zerind a Craiova, con una profundidad de 4 y un costo de 441 km, pero cada uno llegó a ella utilizando un criterio diferente.

DFS también encontró una solución, pero la ruta fue más larga, tanto en número de carreteras como en kilómetros. En el caso de DLS, los límites 2 y 3 no fueron suficientes y dieron como resultado `cutoff`, mientras que con un límite de 4 ya fue posible encontrar la solución. Esto coincide con la profundidad encontrada por BFS e IDS.

En general, el ejercicio me ayudó a entender que encontrar una solución no significa necesariamente encontrar la mejor, y que la forma en que cada algoritmo decide qué nodo explorar cambia bastante el resultado y la cantidad de trabajo que realiza.