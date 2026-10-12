# Taller Práctico 4: PICT + pytest (PagoYa)

**Integrantes:** Daniel Sozoranga

## Resumen del modelo
Se modeló la pasarela de pagos PagoYa con 7 parámetros (Dispositivo, Pais, Metodo, Moneda, Envio, Cupon y Monto) en `checkout.pict`. Se aplicaron las reglas R1 a R6: moneda según el país, Contraentrega solo en Ecuador o Colombia hasta 500, Billetera solo en Celular o Tablet, Express no disponible para el país Otro, cupón Bienvenida desde 50 y monto 2000 solo con Tarjeta o Transferencia.

## Resultados
| Medida | Valor |
|---|---|
| Combinaciones exhaustivas sin restricciones | 5184 (3 x 4 x 4 x 3 x 3 x 3 x 4) |
| Combinaciones válidas (/o:max) | 1046 |
| Pruebas con t=2 | 22 |
| Pruebas con t=3 | 79 |
| Fallos antes de corregir (t=2 / t=3) | 2 / 6 |

Después de corregir `pasarela.py`, la suite completa pasa: 339 pruebas en verde.

## El error que encontramos
Todas las filas que fallaban combinaban el cupón Bienvenida con el envío Express. En `pasarela.py`, una línea ponía el descuento en 0 para esa combinación, por lo que el cliente pagaba de más (por ejemplo, 508 en lugar de 483). Se corrigió eliminando esa excepción.

## Cómo ejecutar
    pict checkout.pict /o:2 > checkout_t2.tsv
    pict checkout.pict /o:3 > checkout_t3.tsv
    pytest

En Windows PowerShell 5, guardar con `> archivo.tsv` genera UTF-16 y pytest no lo puede leer. Usar cmd o Git Bash.
