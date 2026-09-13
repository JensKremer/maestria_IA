# Ejercicio 1 — Detección de objetos con YOLO

## Resultados

En este ejercicio se ejecutó la notebook de YOLO primero con las imágenes originales de Ultralytics y después con una imagen propia. En `zidane.jpg`, YOLO detectó **2 personas** y **1 corbata**. En `bus.jpg`, detectó **4 personas**, **1 autobús** y **1 señal de stop**.

Para la imagen propia utilicé una fotografía donde aparecen varios de mis perros alrededor de un plato. La misma imagen se utilizó en las dos formas de predicción solicitadas: mediante la celda `CLI` y mediante `model(...)`.

### Predicción con CLI

Se ejecutó:

```python
!yolo predict model=yolov8n.pt source='/content/perros.jpeg'
```

El resultado mostró las siguientes detecciones:

- `dog` con confianza **0.41**
- `dog` con confianza **0.40**
- `dog` con confianza **0.35**
- `dog` con confianza **0.29**
- `cat` con confianza **0.49**
- `bowl` con confianza **0.26**

En la fotografía todos los animales son perros, por lo que la detección `cat` fue incorrecta. Aun así, el modelo sí localizó a varios de los cachorros y reconoció correctamente cuatro de ellos como perros. También detectó el plato como `bowl`, aunque con una confianza baja.

### Predicción con `model(...)`

Después del entrenamiento de 3 épocas incluido en la notebook, se ejecutó:

```python
model('/content/perros.jpeg', save=True)
```

En este caso aparecieron las siguientes detecciones:

- `bear` con confianza **0.78**
- `sheep` con confianza **0.44**
- `dog` con confianza **0.38**
- `dog` con confianza **0.35**
- `cat` con confianza **0.51**
- `bowl` con confianza **0.86**

Esta segunda predicción fue diferente a la realizada con CLI. Solo dos de los perros fueron identificados correctamente como `dog`; los demás fueron confundidos con `bear`, `sheep` y `cat`. Por otro lado, el plato fue detectado como `bowl` con una confianza mucho mayor que en la primera ejecución.

## Comparación

| Elemento | CLI | `model(...)` |
|---|---|---|
| Perros correctamente detectados | 4 como `dog` | 2 como `dog` |
| Clasificaciones incorrectas | 1 como `cat` | 1 `bear`, 1 `sheep`, 1 `cat` |
| Plato | `bowl` 0.26 | `bowl` 0.86 |
| Total de cajas | 6 | 6 |

Las dos formas de predicción no coincidieron exactamente. Aunque ambas encontraron seis objetos, las clases asignadas fueron diferentes. En especial, después del entrenamiento corto de la notebook aparecieron más errores al clasificar a los perros.

Además, hubo un perro que fue ignorado en ambos resultados. Al fondo a la derecha hay un cachorro adicional, y ese cachorro no recibió ninguna caja en ninguna de las dos predicciones.

## Observaciones

Pienso que el hecho de que sean cachorros pudo influir en los resultados. Como son pequeños, están muy juntos y algunos se tapan entre sí, el modelo puede tener más dificultad para separarlos correctamente. Eso podría explicar por qué algunos fueron clasificados como `cat`, `bear` o `sheep`, y también por qué uno de los cachorros del fondo no fue detectado.

## Conclusión

YOLO detectó correctamente los objetos principales de las imágenes originales y también logró localizar varios de los animales y el plato de mi fotografía. Sin embargo, la clasificación no siempre fue totalmete correcta. En la predicción por CLI, uno de los perros fue identificado como gato, y con `model(...)` otros perros fueron confundidos con oso, oveja y gato.

También se observó que las dos formas de ejecutar la predicción sobre la misma imagen produjeron resultados distintos. En general, el ejemplo muestra que YOLO puede localizar correctamente un objeto pero asignarle una clase equivocada, especialmente cuando los objetos están juntos, parcialmente cubiertos o tienen rasgos que pueden confundirse con otras clases.
