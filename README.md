# Calculadora PYRON

Refinería de madera de hule: biochar, créditos de carbono (CORCs, Puro.earth Biochar Edition 2025 v2) y bioestimulante. Cada versión es un solo archivo HTML, sin servidor, sin librerías y sin dependencias externas.

## Versiones

| Versión | Ruta | Para qué sirve |
|---|---|---|
| **Fase 4, vigente** | [`/fase4/`](https://gianmaperaza-afk.github.io/pyron-calculadora/fase4/) — `fase4/index.html` | Tres líneas de ingreso, costos y EBITDA, planta, resultados del modelo financiero, modo presentación, enlace compartible e impresión |
| **Fase 3, ejecutiva** | [`/`](https://gianmaperaza-afk.github.io/pyron-calculadora/) — `index.html` | Versión ejecutiva de la Fase 3, sin cambios |
| **Fase 3, completa** | [`/completa/`](https://gianmaperaza-afk.github.io/pyron-calculadora/completa/) — `completa/index.html` | Versión robusta de la Fase 3, sin cambios |

## Cambios de la Fase 4

- **Tres productos.** Biochar (USD 250 por t, EXW), CORCs (USD 150) y bioestimulante (USD 15.38 por L). Cada uno con su color en toda la página.
- **Créditos por tonelada.** Corg 0.867, H/Corg 0.46 (Ali et al., 2022), suelo a 26 °C: PF 69.70 % con la Ec. 6.4 y **1.8956 CORCs por t**. El modelo financiero redondea PF a 70 % y usa 1.91; la diferencia es de 0.5 %.
- **Eproject 320.16 kg CO₂e por t**, desglosado en seis etapas visibles en la cadena.
- **Bioestimulante sobre la madera:** 1,000 kg × 9 % × 70 % × 5 % = 3.15 L por t de madera seca. El 5 % es editable (rango 3 a 8 %).
- **Datos de campo de la plantación:** 400 árboles por ha, 1.47 m³ por árbol, 600 kg/m³ y USD 9.62 por árbol como leña. Quinta categoría de procedencia.
- **Rendimiento 30 % a 500 °C** (rango 25 a 35 %). El deslizador cubre 380 a 500 °C: dos puntos medidos y el punto del modelo.
- **Destino del biochar** con temperatura de suelo editable: Costa Sur de Guatemala 26 °C, California 20 °C, Corn Belt 11 °C, Llanuras Altas de Texas 17 °C y Florida central 24 °C. Los valores de EE. UU. son de trabajo y deben verificarse en la capa de Lembrechts et al. (2022).
- **Escenarios** pesimista, base, optimista y sin créditos. Cargan los valores de entrada y muestran los resultados oficiales fijos del modelo (VAN, TIR, inversión, exposición máxima).
- **Flujo de caja acumulado** 2027 a 2037 y **sensibilidad del VAN** con valores fijos del modelo. No se recalculan en la página.
- **Planta:** BST-50S de 20,800 t al año. 35,280 t requieren 2 unidades al 85 %.
- **Extras:** modo presentación (tecla P), enlace compartible con el estado en la URL, impresión o PDF.
- **Selector de especie:** hule (modelo completo), limón (corrida 3, 27.7 %, Corg preliminar 0.75) y canoj, macadamia, café, mangostán, palma y teca, que todavía no tienen corrida y usan el rendimiento y el Corg del hule como referencia (editables). Se quitaron eucalipto, pino, melina y mango.
- Se quitaron las equivalencias.

## Pruebas

Al abrir la página corre `correrPruebas()` y deja el resultado en la consola (tolerancia 1 %): caso de la Fase 3 (2.0564), créditos por tonelada (1.8956, referencia 1.9051 con PF 70 %), hectárea, operación de 35,280 t, planta, Eproject, bioestimulante por tonelada y VAN del FCFF del modelo (USD 12,840,410).

La auditoría previa a la implementación está en `AUDITORIA_FASE4.md`.
