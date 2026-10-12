# Optimizador de rutas de reparto
Calcular la ruta más corta para repartir en 8 puntos, y compararla contra la forma intuitiva de planearla.

## Resultado
-Ruta secuencial (ir siempre al punto más cercano)	= 69.86 km
-Ruta óptima	= 56.99 km

18.4% menos kilómetros. 12.87 km ahorrados en un solo reparto.

El óptimo está verificado contra 40,320 rutas posibles,  no es una estimación, es la ruta más corta que existe para esos puntos.

## El problema
Si se tiene que repartir en 8 lugares diferente y se visita en orden que parece lógico, lo más probable es que el recorrido sea más largo de lo necesario

La trampa es que pensar: "voy al que esté más cerca" se siente correcto en cada paso y falla en el conjunto. Al dejar los puntos lejanos para el final, obliga a un viaje largo de regreso. En esta ruta, ese último tramo mide 22.85 km: un tercio del recorrido completo en una sola línea.

La ruta óptima hace lo contrario. Sale primero hacia el grupo lejano y regresa pasando por el cercano de camino a casa. Sus primeros tramos son más largos, pero elimina el viaje final.

## Cómo funciona
Datos: Un depósito y 8 paradas reales, con coordenadas tomadas de Google Maps.

Distancias (Haversine): Calcula la distancia sobre la superficie de la esfera. No usé el teorema de Pitágoras sobre las coordenadas porque un grado de longitud no mide lo mismo en todas las latitudes: aquí son unos 105 km y en el ecuador 111.

Matriz de distancias: Las 81 distancias se calculan una sola vez y se guardan en una tabla de 9x9. El algoritmo consulta la tabla en lugar de recalcular senos y raíces en cada paso, y queda independiente de cómo se midan las distancias.

Dos algoritmos: El primero es una ruta voraz: siempre al punto más cercano. Da la línea base. El segundo recorre todas las rutas posibles con itertools y encuentra la ruta más óptima

## Cómo correrlo
Abre el notebook en Google Colab y corre las celdas de arriba a abajo. No hay nada que instalar: solo usa math e itertools, que vienen con Python.

Para usar otros puntos, cambia la lista puntos por tus propias coordenadas. Google Maps las da con clic derecho sobre el mapa.

## Limitaciones
Las distancias son en línea recta. Haversine mide de punto a punto sobre la esfera, no por calles. El manejo real es mayor, así que los dos números son optimistas. El porcentaje de mejora se sostiene porque las dos rutas se miden igual.

La búsqueda exhaustiva no escala. Con 8 paradas son 40,320 rutas y tarda menos de un segundo. Con 12 paradas serían 479 millones. Para más puntos hace falta un solver de optimización.

el programa solo toma en cuenta un solo vehículo, sin restricciones. No hay capacidad de carga, ni ventanas de horario, ni varios camiones.

## Qué sigue
-v2: capacidad del camión, múltiples vehículos, y función objetivo en pesos (combustible + horas + costo fijo por camión).
-v3: distancias reales por calle, ventanas de horario por parada, y la ruta dibujada sobre un mapa.
