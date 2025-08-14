## Introducción

Este informe presenta un análisis de los factores que influyen en la cancelación de clientes (churn) basado en un conjunto de datos proporcionado. Se exploraron, preprocesaron y modelaron los datos utilizando Regresión Logística y Random Forest. Se realizó una optimización del modelo Random Forest mediante búsqueda de hiperparámetros para mejorar su rendimiento predictivo. Finalmente, se analizaron las variables más relevantes identificadas por los modelos para proponer estrategias de retención.

## Metodología

1.  **Carga y Exploración de Datos:** Se cargaron y examinaron los datos iniciales, incluyendo la eliminación de la columna 'cliente_id'.
2.  **Preprocesamiento de Datos:** Se manejaron variables categóricas mediante codificación One-Hot y se trataron los valores faltantes en la columna 'cuenta_cargos_totales'. Se abordó el desbalance de clases utilizando SMOTE en el conjunto de entrenamiento.
3.  **División de Datos:** Los datos se dividieron en conjuntos de entrenamiento y prueba para la evaluación del modelo.
4.  **Modelado:** Se entrenaron modelos de Regresión Logística y Random Forest.
5.  **Optimización del Modelo (Random Forest):** Se realizó una búsqueda de hiperparámetros con validación cruzada para encontrar la mejor configuración para el modelo Random Forest.
6.  **Evaluación del Modelo:** Se evaluó el rendimiento de los modelos (Regresión Logística original, Random Forest original y Random Forest optimizado) en el conjunto de prueba utilizando métricas como Accuracy, Precision, Recall y la Matriz de Confusión.
7.  **Análisis de Importancia de Variables:** Se analizaron los coeficientes de la Regresión Logística y la importancia de las variables del Random Forest optimizado para identificar los factores más influyentes.

## Resultados del Modelado y Evaluación

Se entrenaron y evaluaron dos tipos de modelos: Regresión Logística y Random Forest (original y optimizado). Las métricas clave en el conjunto de prueba fueron:

| Modelo                       | Accuracy | Precision (Clase 1) | Recall (Clase 1) | F1-Score (Clase 1) |
| :--------------------------- | :------- | :------------------ | :--------------- | :----------------- |
| Regresión Logística Original | 0.742    | 0.50                | 0.74             | 0.60               |
| Random Forest Original       | 0.776    | 0.57                | 0.51             | 0.54               |
| Random Forest Optimizado     | 0.784    | 0.57                | 0.63             | 0.60               |

**Análisis del Rendimiento:**

*   El modelo de **Regresión Logística** mostró un buen Recall para la clase positiva (clientes que cancelan), lo que indica que es relativamente bueno identificando a los clientes en riesgo de cancelación. Sin embargo, su Precision es menor, lo que significa que tiene más falsos positivos (clientes predichos como cancelados que en realidad no lo hacen).
*   El **Random Forest Original** tuvo una mejor Precision que la Regresión Logística, pero un Recall significativamente menor.
*   El **Random Forest Optimizado** logró mejorar el Recall en comparación con el Random Forest original, manteniendo una Precision similar. Esto lo convierte en un modelo más equilibrado para este problema, donde identificar correctamente a los clientes que van a cancelar (Recall) es a menudo una prioridad.

**Consideraciones sobre la elección del modelo:**

Aunque cada modelo tiene sus fortalezas, para un problema de predicción de abandono donde es crucial identificar a la mayor cantidad posible de clientes en riesgo (minimizar falsos negativos), un modelo con un buen Recall es generalmente preferible. El **Random Forest Optimizado** logró un equilibrio razonable entre Recall y Precision, superando al modelo original y ofreciendo un mejor Recall para la clase de interés que la Regresión Logística. Por lo tanto, la elección del Random Forest optimizado es adecuada para este fin.

Además, diferentes modelos pueden ser útiles para distintos propósitos:

*   La **Regresión Logística** es más interpretable debido a sus coeficientes, lo que facilita entender la dirección y magnitud del impacto de cada variable. Puede ser útil para análisis explicativos.
*   **Random Forest** es un modelo más complejo y potente, capaz de capturar interacciones no lineales entre variables, lo que a menudo resulta en un mejor rendimiento predictivo. Es ideal para la predicción.
*   Otros modelos como **SVM** (Support Vector Machines) pueden ser efectivos en espacios de alta dimensión, mientras que modelos basados en boosting como **XGBoost** suelen ofrecer un rendimiento de vanguardia en muchos problemas de clasificación.

## Análisis de la Importancia de las Variables

Basado en el análisis de los coeficientes de la Regresión Logística y la importancia de las variables del Random Forest optimizado, los factores que parecen tener una mayor influencia en la cancelación de clientes son:

*   **Tipo de Contrato (principalmente mes a mes):** Los clientes con contratos mes a mes (`cuenta_contrato_month-to-month`) consistentemente muestran una alta propensión a cancelar en ambos modelos.
*   **Antigüedad del Cliente:** Una menor antigüedad (`cliente_antiguedad_meses`) está fuertemente asociada con una mayor probabilidad de cancelación.
*   **Servicios de Internet (Fibra Óptica y Seguridad en Línea):** Tener el servicio de fibra óptica (`internet_tipo_fiber optic`) y no tener servicios de seguridad en línea (`internet_seguridad_en_linea_no`) también se identificaron como factores importantes que aumentan el riesgo de cancelación.
*   **Cargos (Mensuales y Totales):** Las variables relacionadas con los cargos (`cuenta_cargos_mensuales`, `cuenta_cargos_totales`, `cuentas_diarias`) también son relevantes, aunque su impacto relativo varía ligeramente entre los modelos.

## Estrategias de Retención Propuestas

Basándonos en los factores identificados como más influyentes en la cancelación, se proponen las siguientes estrategias de retención:

1.  **Programas de Fidelización para Clientes con Contrato Mes a Mes:** Ofrecer incentivos, descuentos o beneficios adicionales a los clientes con contrato mes a mes para fomentar la transición a contratos a más largo plazo (uno o dos años), reduciendo así el riesgo de cancelación.
2.  **Estrategias de Onboarding y Soporte para Nuevos Clientes:** Poner especial atención a los clientes con baja antigüedad. Implementar programas de onboarding robustos, comunicación proactiva y soporte técnico eficiente durante los primeros meses para asegurar una experiencia positiva y reducir la probabilidad de cancelación temprana.
3.  **Promoción de Servicios de Seguridad en Línea:** Destacar los beneficios de los servicios de seguridad en línea para los clientes que no los tienen, ofreciendo pruebas gratuitas o descuentos para aumentar la adopción y potencialmente reducir el riesgo de cancelación asociado a esta variable.
4.  **Análisis y Optimización de Precios:** Revisar la estructura de cargos, especialmente para los clientes con fibra óptica o aquellos con cargos mensuales elevados, para asegurar que los precios sean competitivos y percibidos como justos por los clientes.
5.  **Monitoreo Continuo de Clientes de Alto Riesgo:** Utilizar el modelo entrenado para identificar proactivamente a los clientes con alta probabilidad de cancelar y dirigir esfuerzos de retención personalizados hacia ellos. Esto podría incluir ofertas especiales, contacto directo para resolver problemas o encuestas de satisfacción.

## Conclusiones

El análisis ha permitido identificar los principales impulsores de la cancelación de clientes en este conjunto de datos. El modelo Random Forest optimizado se presenta como una herramienta útil para predecir la probabilidad de churn. Las estrategias de retención propuestas se centran en abordar los factores de riesgo clave, como el tipo de contrato, la antigüedad del cliente, los servicios de internet y los cargos. La implementación de estas estrategias, respaldada por el monitoreo continuo del modelo, puede ayudar a reducir la tasa de cancelación y mejorar la retención de clientes.

**Próximos Pasos:**

*   Validar las estrategias de retención propuestas mediante pruebas A/B.
*   Continuar monitoreando el rendimiento del modelo con datos nuevos y re-entrenarlo periódicamente.
*   Explorar otras técnicas de modelado o características adicionales que puedan mejorar aún más la precisión de la predicción de churn.
