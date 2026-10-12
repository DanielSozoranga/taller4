# Taller 4: respuestas (Partes 2 a 4)

## Parte 2

**2.2.1** Sin restricciones: 3 x 4 x 4 x 3 x 3 x 3 x 4 = 5184 combinaciones.

**2.4 Resultados**

| Medida | Resultado |
|---|---|
| Combinaciones exhaustivas sin restricciones | 5184 |
| Combinaciones válidas con R1 a R6 (/o:max) | 1046 |
| Pruebas generadas con t=2 | 22 |
| Pruebas generadas con t=3 | 79 |

**2.5.1** Filtré Metodo = Contraentrega. Hay 5 filas en t=2 y todas son de Ecuador o Colombia con monto de 500 o menos, así que cumplen R2.

**2.5.2** t=3 genera más pruebas porque debe cubrir todas las tríadas de valores (1143) y no solo los pares (230). A cambio, detecta errores que dependen de tres factores a la vez.

## Parte 3

**3.2.1** `delimiter="\t"` indica que las columnas están separadas por tabulaciones. PICT escribe sus resultados así, y sin eso `csv` buscaría comas y leería cada fila como una sola columna.

**3.2.2** `parametrize` ejecuta la función una vez por cada fila del archivo. Cada fila crea una prueba de pytest por cada función de test que la use.

**3.2.3** Si el esperado se copiara de `pasarela.py`, el test nunca fallaría: compararía el sistema consigo mismo y repetiría sus errores.

**3.3 Suite**

| Archivo | Filas | Fallos (FAILED) |
|---|---|---|
| checkout_t2.tsv (casos t2-...) | 22 | 2 |
| checkout_t3.tsv (casos t3-...) | 79 | 6 |

Total antes de corregir: 3 x (22 + 79) = 303 pruebas, 295 pasaron y 8 fallaron. Todas las fallas son de `test_total_correcto`.

**3.4.1** Todas las filas que fallan comparten el par Cupon = Bienvenida y Envio = Express.

**3.4.2** La línea responsable era `if cupon == "Bienvenida" and envio == "Express": descuento = 0.00`. Se dispara cuando se combina el cupón Bienvenida con envío Express y provoca que se pierda el descuento, así que el total sale más alto (508 en lugar de 483 para un monto de 500). Se corrigió eliminando esas dos líneas y toda la suite quedó en verde.

**3.4.3** t=2 tiene garantía porque este error depende de un solo par de valores y t=2 incluye todos los pares. Un error que dependa de tres factores a la vez podría escaparse a t=2, pero no a t=3.

**3.5 Tests nuevos**
- `test_retiro_no_supera_el_monto`: con envío Retiro, el total nunca es negativo ni supera el monto original.
- `test_billetera_en_laptop_es_rechazada`: prueba negativa escrita a mano, porque PICT solo genera combinaciones válidas.

Suite final: 339 pruebas en verde.

## Ticket de salida

1. t=3 da más seguridad para un error de tres factores, porque garantiza todas las tríadas. El costo es mayor: 79 pruebas frente a 22.
2. Si olvidara una restricción, PICT generaría combinaciones imposibles y pytest fallaría por culpa del modelo y no del sistema. Por ejemplo, sin R1 aparecería Ecuador con COP y el sistema lo rechazaría.
3. Porque, si el esperado sale del mismo sistema que se prueba, el test repite sus errores y nunca los detecta. El oráculo debe ser independiente.
