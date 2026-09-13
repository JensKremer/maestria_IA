# Ejercicio 1 — Más capas en el perceptrón multicapa (Iris)

## Análisis comparativo

En este ejercicio se probaron dos redes para clasificar el conjunto Iris. Una fue programada manualmente con NumPy y la otra con Keras/TensorFlow. Primero se ejecutó la arquitectura original `4 × 3 × 3` y después se agregaron dos capas para obtener una red `4 × 3 × 3 × 3 × 3`. En todos los casos se mantuvieron la activación sigmoide, el error MSE, una tasa de aprendizaje de `0.03` y `500` épocas.

| Implementación | Topología | Error/Loss inicial | Error/Loss final |
|---|---|---:|---:|
| NumPy | `4 × 3 × 3` | 0.820746 | **0.062228** |
| NumPy | `4 × 3 × 3 × 3 × 3` | 0.729241 | **0.281692** |
| Keras | `4 × 3 × 3` | 0.301911 | **0.177921** |
| Keras | `4 × 3 × 3 × 3 × 3` | 0.243237 | **0.221888** |

En mis resultados, agregar dos capas no mejoró el error final. En NumPy, la red original terminó con un error de `0.062228`, mientras que la red profunda terminó en `0.281692`. En Keras pasó algo parecido: la red original terminó con un loss de `0.177921` y la profunda con `0.221888`. Por lo tanto, en estas pruebas la red más sencilla obtuvo mejores resultados después de las 500 épocas.

Las curvas tampoco se comportaron exactamente igual. En NumPy, la red profunda pasó muchas épocas casi sin mejorar y después tuvo una caída más fuerte del error. En Keras, el loss bajó al principio y después se fue estabilizando. Esto puede deberse a que las dos implementaciones no entrenan exactamente de la misma forma. Por ejemplo, en NumPy los pesos se actualizan muestra por muestra, mientras que Keras realiza el entrenamiento internamente por lotes. También pueden influir la inicialización aleatoria de los pesos y el orden en el que se usan los datos.

Considero que sí tiene sentido que la red con más capas no haya funcionado mejor en Iris. Al usar varias capas con sigmoide, el cambio que llega a las primeras capas durante el backpropagation puede hacerse cada vez más pequeño. Esto puede hacer que la red tarde más en aprender o que se quede estancada por varias épocas, algo que se puede observar principalmente en la curva de NumPy. Además, Iris es un conjunto de datos pequeño y el problema no parece necesitar una red tan profunda. En este caso, agregar más capas hizo al modelo más complejo, pero no produjo una mejora en el resultado final.
