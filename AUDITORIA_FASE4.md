# Auditoría Fase 4 — Calculadora PYRON

Estado: **pendiente de aprobación. No se ha tocado código.**
Archivos auditados (commit `790bb09`): `index.html` (ejecutiva, 1,025 líneas) y `completa/index.html` (completa, 1,856 líneas).

## 1. Resultado del caso de prueba de la Fase 3

Qbiochar 1 t, Corg 0.80, H/Corg 0.45, Ts 26 °C, antes de Eproject:

| Paso | Resultado |
|---|---|
| Cstored = 1 × 0.80 × 44/12 | 2.933333 tCO2e |
| PF = 88.37 − 40.59 × 0.45 | 70.1045 % |
| Closs | 0.876935 tCO2e |
| **CORCs** | **2.056399 tCO2e** |

Sigue pasando en los dos archivos. Ojo con el número: el documento de Fase 4 dice 2.0562; el valor exacto es 2.056399, que redondeado a cuatro decimales es **2.0564**. La diferencia es 0.01 %, dentro de la tolerancia de 1 %. En `correrPruebas()` voy a usar 2.0564.

## 2. Las cinco pruebas de la Fase 4, recalculadas a mano

Recalculé todo el modelo de forma independiente con los datos de las secciones A a E. Todo cierra:

| Prueba | Esperado | Recalculado | Cierra |
|---|---|---|---|
| 2. CORCs por t de biochar | 1.905 | 1.9052 | Sí |
| 3. Hectárea: biochar | ~106 t | 105.8 t | Sí |
| 3. Hectárea: CORCs | 202 | 201.6 | Sí |
| 3. Hectárea: bioestimulante | 1,111 L | 1,111.3 L | Sí |
| 3. Hectárea: ingreso | ~USD 73,800 | USD 73,799 | Sí |
| 3. Contra leña | ~19x | 19.2x | Sí |
| 4. Operación: biochar | 10,584 t | 10,584 t | Sí |
| 4. Operación: CORCs | ~20,164 | 20,165 | Sí |
| 4. Operación: ingreso | ~USD 7.4M | USD 7.38M | Sí |
| 4. Mezcla CORCs / biochar / bioestimulante | 41 / 36 / 23 % | 41 / 36 / 23 % | Sí |
| 5. Planta | 2 unidades al 85 % | 1.70 → 2 unidades al 84.8 % | Sí |
| Costos por t | 332 | 142+113+31+26+17+3 = 332 | Sí |
| Margen por t | 365, 52 % | 365.3, 52.4 % | Sí |
| EBITDA operación | USD 3.86M | USD 3.87M | Sí |

Dato útil: las 353 t/ha salen de 400 árboles × 1.47 m³ × 600 kg/m³ = 352.8 t. O sea que el volumen por árbol y la densidad no cambian; solo bajan los árboles de 500 a 400.

## 3. Tabla de auditoría

"Hoy" describe los dos archivos; cuando difieren lo indico (E = ejecutiva, C = completa).

| # | Concepto | Hoy en la calculadora | Modelo Fase 4 | ¿Coincide? | Cambio propuesto |
|---|---|---|---|---|---|
| 1 | Ecuaciones 5.1, 6.1, 6.3, 6.4, 6.5 | Implementadas y verificadas contra el PDF | Iguales | Sí | Ninguno |
| 2 | Tabla 6.1 | 34 filas, 7 a 40 °C | Igual | Sí | Ninguno |
| 3 | Temperatura de operación | E: rango 250–450 °C, óptimo fijo 350 °C. C: rango 350–700 °C, defecto 450 °C, "óptimo económico" calculado en 400 °C | 500 °C | **No** | Defecto 500 °C. Quitar el "óptimo" de 350/400 °C. Ver decisión D3 |
| 4 | Árboles por hectárea | 500 | 400 | **No** | 400 |
| 5 | m³ por árbol y densidad | 1.47 m³ y 600 kg/m³ | Implícitos en 353 t/ha | Sí | Se mantienen. Madera seca por ha pasa de 441 a 352.8 t |
| 6 | Corg | Ctot 0.867 del paper menos Cinorg 0.005 = **0.862** (Ec. 6.2, regla 6.1.6) | **0.867** directo | **No** | Ver decisión D1. Es la diferencia más delicada |
| 7 | Corg escenario bajo | No existe | 50.55 % medido por un laboratorio externo en la muestra artesanal | No | Agregarlo como `medicion_propia` y usarlo en el escenario pesimista |
| 8 | Rendimiento | E: 33 % fijo. C: curva supuesta, 29 % a 450 °C y 27 % a 500 °C | 30 % base, rango 25–35 % | **No** | 30 % a 500 °C con fuente "Modelo financiero PYRON, Fase 4". Ver D3 |
| 9 | H/Corg | E: curva supuesta, 0.58 a 350 °C. C: curva supuesta, 0.41 a 500 °C | 0.45, supuesto | **No** | 0.45 en el punto de operación |
| 10 | Corridas de laboratorio | E: no aparecen. C: tres filas "pendiente" a 400, 500 y 600 °C; la corrida 0 se quitó | Cuatro corridas con datos (0 a 3) | **No** | Cargar las cuatro como `medicion_propia`, más canoj, macadamia, café, mangostán, palma y teca con rendimiento pendiente |
| 11 | Especies | Hule, limón, eucalipto, pino, teca, melina, mango | Hule, limón y las seis pendientes | Parcial | Dejar hule y limón con datos; las demás como "pendiente de cargar". Ver D5 |
| 12 | Emisiones del proyecto | **0** tCO2e por t (los CORCs no descuentan nada) | 0.3234 tCO2e por t | **No** | 0.3234. Baja los CORCs 14 % respecto de hoy |
| 13 | Precio del biochar | USD **200** por t (el documento decía 350; en el código es 200) | USD 250 por t, EXW | **No** | 250, rango 200–300 |
| 14 | Precio del CORC | USD 150, editable | 150 base, 120 bajo, 185 alto | Sí en la base | Agregar los rangos a los escenarios |
| 15 | Bioestimulante | No existe | 3.15 L por t de madera, USD 15.38 por L | **No** | Tercera rama completa, con interruptor |
| 16 | Costos | E: no hay, muestra ingresos brutos. C: 20 % del ingreso + USD 5,000 de auditoría | USD 332 por t de biochar en seis etapas | **No** | Reemplazar por el desglose de la sección D |
| 17 | Uso alternativo | Aserrada USD 100/t, leña USD 35/t, carbón USD 60/t, todos supuestos | Leña: USD 9.62 por árbol, USD 3,846 por ha | **No** | Dejar solo leña con el dato del modelo y el contador "19x" |
| 18 | Flujo de caja | E: no hay. C: esquema de 12 meses hasta el primer pago | Calendario 2027–2037 | **No** | Ver extra 7 y decisión D6 |
| 19 | Temperatura del suelo | 26 °C fijo (E) o editable (C), redondeo al entero superior | 26 °C base y selector de destinos | Sí en la base | Agregar el selector (extra 5) |
| 20 | Nombre de la empresa | "Fortaleza" aparece en 10 líneas de E y 15 de C, incluida la etiqueta "Dato de campo Grupo Fortaleza" y los textos de ayuda | No nombrar a la patrocinadora | **No** | Reemplazar por "la plantación" / "la operación" en todo el archivo |
| 21 | Tipos de procedencia | Cinco: los cuatro de siempre más `dato_empresa` | Cuatro | **No** | Ver decisión D4 |
| 22 | Nombre del laboratorio | "ANALAB" no aparece en ningún archivo | "un laboratorio externo" | Sí | Ninguno. Solo cuidar el texto nuevo |
| 23 | "Vinagre", quetzales, "validar" | No aparecen | Prohibidos | Sí | Ninguno |
| 24 | Equivalencias (autos, árboles, vuelos) | Sección visible en E y C, con factores sin fuente | Solo si la fuente es oficial | **No** | Quitarlas. Ver extra 12 |
| 25 | Escenarios | E: no hay. C: conservador/base/optimista con porcentajes míos | Pesimista, base, optimista, sin créditos, con la tabla de rangos | **No** | Reemplazar por los cuatro botones |
| 26 | Sensibilidad | C: tornado de ±20 % uniforme sobre el ingreso | Tornado sobre el VAN con rangos bajo/alto | **No** | Ver extra 8 y hallazgo H2 |
| 27 | Planta | No existe | BST-50S, 20,800 t por unidad | No | Agregar (extra 6) |
| 28 | Pruebas | `selfTest()` con dos casos | `correrPruebas()` con cinco | No | Reescribir con las cinco pruebas |
| 29 | README | Tabla incompleta de dos versiones | Actualizado | No | Reescribirlo |
| 30 | Estilo visual | Paleta de Fase 3 | Paleta opcional de la presentación | — | Ver decisión D7 |

## 4. Hallazgos que no estaban en tu lista

**H1. El Corg del modelo usa carbono total.** En agosto verificamos contra el PDF de Puro.earth que la regla 6.1.6 no permite usar carbono total y que hay que restar el inorgánico (Ec. 6.2). Por eso la calculadora hoy usa 0.862. El modelo de Fase 4 usa 0.867. Con 0.862 la cadena da 1.892 CORCs por t en vez de 1.905: una diferencia de 0.7 %, dentro de la tolerancia de las pruebas. No cambia la historia, pero sí cambia qué número mostramos.

**H2. El orden del tornado no me sale con los rangos del documento.** La presentación pone primero el precio del bioestimulante. Con los rangos de la sección F, moviendo una variable a la vez, el efecto sobre el EBITDA anual es:

| Variable | Bajo | Alto |
|---|---|---|
| Corg (50.6 %) | −USD 1.47M | 0 |
| Precio del CORC | −0.60M | +0.71M |
| Precio del biochar | −0.53M | +0.53M |
| Rendimiento | −0.36M | +0.36M |
| Precio del bioestimulante | −0.11M | +0.11M |

El bioestimulante queda último, no primero: su rango es de ±6 % sobre el 23 % del ingreso. Para que quede primero, el modelo tiene que estar moviendo otra cosa (por ejemplo el volumen vendido, o un rango de precio más amplio). Necesito saber qué mueve el modelo antes de construir el tornado; no voy a forzar el orden.

**H3. El mini modelo financiero no llega a los números oficiales con lo que tengo.** Con la inversión por fases, un EBITDA de USD 3.86M que crece 3 % al año y tasa de 12 %, un modelo simple da un VAN cercano a USD 18M, no 12.84M. Faltan piezas que el documento no trae: impuestos, depreciación, capital de trabajo, la rampa de arranque de 2027–2029 y los costos fijos. Ver decisión D6.

**H4. La inversión de los escenarios no se deduce de la tabla de rangos.** Base USD 3.19M = 2 × 910k + 1.37M de otros. Con unidades a 850k daría 3.07M, no los 2.66M del optimista; y el pesimista de 3.83M no sale de ningún rango. Los voy a cargar como valores fijos del modelo, no calculados.

**H5. La base del bioestimulante.** 3.15 L por t de madera y 10.5 L por t de biochar solo coinciden con rendimiento de 30 %. Si el rendimiento cambia, hay que elegir una. Propongo la madera: el condensado sale del vapor de la madera, no del biochar. Con eso, al subir el rendimiento el bioestimulante no sube.

**H6. Las corridas propias no llegan a 500 °C.** Midieron 32.5–33.4 % a 380–400 °C. El 30 % a 500 °C viene del modelo. En la curva cada punto va a mostrar su origen: medido o del modelo.

**H7. Hay dos calculadoras.** El documento habla de un solo `index.html`. Ver decisión D2.

## 5. Decisiones que necesito de vos

| # | Decisión | Mi recomendación |
|---|---|---|
| D1 | ¿Corg 0.867 como el modelo, o 0.862 como exige la regla 6.1.6? | Mostrar 0.867 para que la calculadora y la presentación den el mismo 1.91, y dejar en el panel de fuentes la nota de que la certificación va a medir carbono orgánico |
| D2 | ¿Qué pasa con las dos versiones? | La Fase 4 reemplaza la ejecutiva en la raíz. La completa se queda en `/completa/` como archivo de la Fase 3, con un aviso arriba de que sus números son anteriores |
| D3 | ¿El deslizador de temperatura sigue existiendo? | Sí, pero con 500 °C como punto de operación marcado, rango 250–600 °C, y los puntos medidos (380 y 400 °C) distinguidos de los del modelo |
| D4 | ¿Qué hago con el tipo `dato_empresa`? | Pasar árboles, m³ y densidad a `supuesto` con fuente "Modelo financiero PYRON, Fase 4", para quedar en los cuatro tipos y sin nombrar a la empresa |
| D5 | ¿Quito eucalipto, pino, melina y mango? | Sí. No están en la lista de la Fase 4 y sus valores eran estimaciones mías |
| D6 | Mini modelo financiero | Mostrar los resultados oficiales como referencia fija. Si me pasás el flujo de caja anual del modelo (2027–2037), lo reproduzco en vivo |
| D7 | ¿Paleta de la presentación? | Sí: biochar verde, CORCs ocre y bioestimulante azul hacen que la calculadora y las láminas se lean como una sola cosa |

## 6. Extras

Prioridad 1 (se implementan al aprobar): modo presentación con tecla P, refinería animada en SVG, contador contra la leña, cuatro botones de escenario, selector de destino del biochar, dimensionamiento de planta.

Sobre el selector de destino: no tengo los valores oficiales de temperatura de suelo de EE. UU. Los voy a dejar editables y marcados como supuesto, con la nota de verificar en la capa de Lembrechts et al. 2022. Decime qué regiones querés (por ejemplo Valle Central de California, Medio Oeste, Florida, Texas) o las propongo yo.

Prioridad 2 y 3, para decidir:

| # | Extra | Complejidad | Comentario |
|---|---|---|---|
| 7 | Mini modelo financiero en vivo | Alta | Bloqueado por H3. Como referencia fija: baja |
| 8 | Tornado sobre el VAN | Media | Bloqueado por H2 y depende del 7 |
| 9 | Enlace compartible por hash | Baja | Solo guarda los valores de los campos. Recomendado: sirve para el QR de la presentación |
| 10 | Botón EN/ES | Media | Unas 150 cadenas de texto. Vale la pena si la van a ver compradores de EE. UU. |
| 11 | Imprimir o PDF | Baja | La completa ya tiene la base hecha |
| 12 | Equivalencias | Baja | Las quitaría: los factores actuales no tienen fuente oficial verificada |

## 7. Qué sigue

Con tu aprobación y las respuestas a D1–D7 implemento los cambios y la prioridad 1, corro las cinco pruebas, actualizo el README y te muestro el resultado. **No hago push sin tu confirmación.**
