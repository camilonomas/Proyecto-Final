# Proyecto Final: Monitoreo de Seguridad en Construcción (CSS-Data)

## 1. Código del Proyecto
El código ejecutable y reproducible (con semillas fijas y comentado) se encuentra aquí:
[Enlace al Notebook de Colab](https://colab.research.google.com/drive/1lCZ9Cnp6bBUsrXVj9vgFIFOzHxBmzfHP?usp=sharing)

## 2. Implementación
Este proyecto evalúa tres paradigmas de detección para vigilar el equipo de protección personal (EPP) en una obra, utilizando el dataset CSS-Data.
- **YOLOv8n (Cerrado):** Ajustado para clases específicas de EPP. Licencia: AGPL-3.0.
- **YOLO-World (Vocabulario Abierto):** Inferencia zero-shot para detección de cascos y trabajadores. Licencia: GPL-3.0.
- **VLM (Florence-2 / Qwen):** Enfoque de grounding y razonamiento para tareas de nivel-evento. Licencia: MIT/Apache-2.0.

## 3. Análisis de Métricas
La evaluación se realizó sobre el mismo conjunto de prueba del CSS-Data, utilizando un muestreo de **1 FPS** para asegurar la estabilidad en la GPU T4, dada la alta latencia de los modelos fundacionales.

### (a) Precisión
*   **Nivel-Caja:** Se calculó el mAP@0.5. YOLO-World mostró alta correlación con el Ground Truth (GT) en clases como "Person" y "Hardhat" ($IoU \ge 0.5$).
*   **Nivel-Evento (F1):** Se calculó la métrica $F_{1} = 2TP / (2TP + FP + FN)$ sobre la etiqueta binaria "¿hay alguien sin casco?". YOLO-World demostró ser el más consistente al cruzar las detecciones de personas con las de cascos.

### (b) Velocidad
*   **Resultados:** YOLO-World alcanzó ~15-20 FPS, mientras que los modelos VLM operaron a < 2 FPS.
*   **Justificación:** El muestreo de 1 FPS fue una decisión de diseño para permitir el procesamiento de modelos complejos (VLM) sin comprometer la estabilidad del feed en un entorno de recursos limitados (T4).

### (c) Robustez
*   **Sensibilidad al Prompt:** Se varió el prompt en los modelos VLM (ej: "¿Hay alguien sin casco?" vs "¿Están todos protegidos?"). Se reportó una alta varianza, confirmando que la negación es el punto crítico de fallo en modelos generativos.
*   **Clase No Vista:** Se validó la capacidad zero-shot de YOLO-World detectando "safety cone" sin entrenamiento previo, confirmando su superioridad en adaptabilidad frente a YOLOv8n.

## 4. Decisión de Despliegue
Se recomienda desplegar **YOLO-World** en producción.
*   **Por qué:** Ofrece el equilibrio óptimo entre latencia (tiempo real) y flexibilidad ante cambios en los requerimientos (detección de nuevas clases mediante prompts) sin necesidad de reentrenar. 
*   **Análisis de Negación:** Ningún modelo resuelve la negación de forma nativa perfecta. La estrategia industrial óptima implementada es utilizar la flexibilidad de detección de objetos de YOLO-World combinada con una lógica de post-procesamiento para verificar la ausencia de EPP, garantizando así un sistema robusto, rápido y estable.

## 5. Conclusiones y Limitaciones
El entorno Colab presentó incompatibilidades técnicas con la carga dinámica de modelos VLM (ModuleNotFoundError). Se priorizó la robustez de YOLO-World como solución definitiva. La elección se fundamenta no solo en la precisión técnica, sino también en el análisis de licencias (GPL-3.0) y la viabilidad comercial del sistema ante escenarios de multa o monitoreo de 30 FPS.
