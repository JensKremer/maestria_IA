# Ejercicio 01 — Aplicaciones de IA que uso o he usado

### Reporte de resultados

En mi mapa, el **agente basado en utilidad** fue el único que logró obtener el oro y regresar a la salida en las pruebas iniciales. Terminó en 40 pasos con un puntaje de **950**. Por otro lado, tanto el **agente de reflejo simple** como el **agente basado en modelo** llegaron al límite de 200 pasos sin conseguir el oro, con un puntaje de **-200** cada uno. El **agente basado en metas** tampoco consiguió el oro y terminó con **-210 puntos**. Finalmente, el **agente de aprendizaje**, usando los **1500 episodios** sugeridos en el ejercicio, salió casi inmediatamente sin obtener el oro, con 1 paso y un puntaje de **-1**.

El agente de reflejo simple falla principalmente porque solo responde a lo que percibe en ese momento y no guarda información de lo que ya hizo. Por eso puede terminar repitiendo movimientos o sin encontrar una forma de llegar al oro, en este caso se queda siempre girando a la derecha. En otro acomodo del mapa podría tener suerte y encontrarlo, pero sería más por cómo están colocados los obstáculos que por una estrategia propia.

En el caso del **agente basado en modelo**, mover un pit cerca de la casilla inicial hace que aparezca una brisa desde muy temprano. Esto provoca que el agente considere más casillas como peligrosas y sea más cuidadoso al explorar, incluso pudiendo quedarse sin una ruta que considere segura. Si se aleja el pit del inicio, puede explorar más libremente y obtener más información del mapa antes de encontrarse con señales de peligro.

También probé aumentar el entrenamiento del **learning agent** de **1500 a 15000 episodios**. En este caso el resultado mejoró mucho: logró **salir con el oro en 22 pasos y obtener 978 puntos**. Esto muestra que con más episodios el agente puede aprender una mejor estrategia para recorrer este mapa y encontrar una ruta más eficiente hacia el oro y de regreso.
