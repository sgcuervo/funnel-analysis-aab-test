# Análisis de Embudo de Ventas y Test A/A/B — App de Productos Alimenticios
El equipo de diseño y producto quiere saber dos cosas: dónde se pierden los 
usuarios en el proceso de compra, y si un cambio tipográfico mejora la 
conversión. Este análisis responde ambas preguntas con datos.

### Herramientas y tipo de proyecto
![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![Pandas](https://img.shields.io/badge/pandas-%23150458.svg?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243.svg?style=for-the-badge&logo=numpy&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?style=for-the-badge&logo=scipy&logoColor=white)
![Matplotlib](https://img.shields.io/badge/MATPLOTLIB-blue?style=for-the-badge)
![Seaborn](https://img.shields.io/badge/SEABORN-blue?style=for-the-badge)
![Análisis de Datos](https://img.shields.io/badge/AN%C3%81LISIS_DE_DATOS-blue?style=for-the-badge)
![Pruebas de Hipótesis](https://img.shields.io/badge/PRUEBAS_DE_HIP%C3%93TESIS-blue?style=for-the-badge)
![A/B Testing](https://img.shields.io/badge/A%2FB_TESTING-blue?style=for-the-badge)
![Análisis de Embudo](https://img.shields.io/badge/AN%C3%81LISIS_DE_EMBUDO-blue?style=for-the-badge)

## Preguntas clave:
1. ¿En qué etapa del embudo de ventas se pierden más usuarios?
2. ¿Qué proporción de usuarios completa todo el proceso hasta el pago?
3. ¿Los grupos de control son comparables entre sí?
4. ¿El cambio tipográfico tiene impacto estadísticamente significativo 
   en el comportamiento de los usuarios?

## Metodología
Se analizaron 243,713 eventos válidos de 7,534 usuarios únicos tras eliminar 
413 duplicados explícitos y filtrar registros anteriores al 1 de agosto de 
2019 por volumen insuficiente — la pérdida fue mínima (2,826 registros y 
17 usuarios). Se construyó el embudo con cuatro eventos secuenciales 
excluyendo el Tutorial por ser opcional. Para el test A/A/B se realizaron 
16 pruebas de hipótesis con corrección de Bonferroni (α = 0.003) para 
minimizar falsos positivos — criterio justificado dado que el cambio 
evaluado es tipográfico y de bajo riesgo.

## Insights clave:

**Embudo de ventas:**

1. **El 47% de los usuarios completa todo el proceso hasta el pago.** 
Las tasas de conversión por etapa revelan dónde está el problema real.

   | Etapa | Usuarios | Conversión a siguiente etapa |
   |---|---|---|
   | MainScreenAppear | 98% | — |
   | OffersScreenAppear | 61% | 62% |
   | CartScreenAppear | 50% | 81% |
   | PaymentScreenSuccessful | 47% | 95% |

2. **La pérdida crítica ocurre en la primera transición.** Solo el 62% 
de usuarios avanza de la pantalla principal a la pantalla de ofertas. 
Las etapas posteriores muestran conversiones altas (81% y 95%), lo que 
indica que el problema es de captación inicial, no de producto o precio.

3. **Los usuarios que avanzan tienen alta intención de compra.** Una vez 
que el usuario llega al carrito, el 95% completa el pago — el embudo 
es eficiente en sus etapas finales.

**Test A/A/B:**

4. **Los grupos de control son estadísticamente equivalentes.** Las tasas 
de conversión de los grupos 246 (48.3%) y 247 (46.1%) no muestran 
diferencia significativa (p = 0.115), confirmando que la división 
aleatoria fue correcta y cualquier diferencia con el grupo 248 será 
atribuible al cambio tipográfico.

5. **El cambio tipográfico no tiene impacto medible.** Ninguna de las 16 
pruebas arrojó p-value inferior a 0.003. Las tasas de conversión fueron 
consistentemente similares entre los tres grupos en todos los eventos 
del embudo.

## Recomendaciones:
1. **No implementar el cambio tipográfico** — no genera mejora medible 
en ninguna etapa del embudo.
2. **Priorizar el rediseño de la pantalla principal** para reducir la 
pérdida del 38% de usuarios en la primera etapa — es la oportunidad 
de mejora de mayor impacto identificada en el análisis.
3. **Analizar fuentes de tráfico** para determinar si el problema en la 
primera etapa está en la calidad de los usuarios que llegan a la app 
o en el diseño de la pantalla principal.

## Diccionario de datos

Cada entrada del dataset representa una acción de usuario o evento registrado 
en la aplicación:

- `EventName` — nombre del evento registrado
- `DeviceIDHash` — identificador único de usuario por dispositivo
- `EventTimestamp` — fecha y hora del evento
- `ExpId` — número de experimento (246 y 247 = grupos de control, 
  248 = grupo de prueba)

**Grupos experimentales:**
- `246` y `247` — grupos de control (tipografía original)
- `248` — grupo de prueba (nueva tipografía)

## Cómo reproducir el análisis

```bash
git clone https://github.com/sgcuervo/funnel-analysis-aab-test

cd funnel-analysis-aab-test

pip install -r requirements.txt

jupyter notebook analysis.ipynb
```
El dataset original está incluido en `/datasets/`.
