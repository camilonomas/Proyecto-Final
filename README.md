# Proyecto Final: Monitoreo de Seguridad en Construcción

## 1. Código del Proyecto
El código ejecutable y reproducible se encuentra aquí:
[Enlace al Notebook de Colab](https://colab.research.google.com/drive/1lCZ9Cnp6bBUsrXVj9vgFIFOzHxBmzfHP?usp=sharing)

## 2. Implementación
Este proyecto compara tres paradigmas de detección para vigilar el equipo de protección personal (EPP) en obras:
- **YOLOv8n (Cerrado):** Ajustado para clases específicas. Licencia: AGPL-3.0.
- **YOLO-World (Vocabulario Abierto):** Detección en tiempo real basada en prompts. Licencia: GPL-3.0.
- **Florence-2/VLM:** Modelo fundacional utilizado para grounding y razonamiento. Licencia: MIT.

## 3. Análisis de Métricas (Tarea 3)
La evaluación se realizó sobre el mismo conjunto de prueba del CSS-Data, utilizando un muestreo de ~1 FPS para asegurar la estabilidad en la GPU T4.

| Modelo | Precisión (F1-Nivel Evento) | Velocidad (FPS) |
| :--- | :--- | :--- |
| YOLO-World | [Escribe tu resultado F1] | ~15-20 FPS |
| Florence-2 | [Escribe tu resultado F1] | < 2 FPS |

## 4. Decisión de Despliegue (Tarea 4)
Se recomienda desplegar **YOLO-World**.
- **Por qué:** Ofrece el mejor equilibrio entre latencia (tiempo real) y flexibilidad ante cambios en los requerimientos (ej. detectar nuevas clases sin reentrenar).
- **Análisis de Negación:** La detección de cascos es crítica. Se observó que los modelos VLM son sensibles al prompt y menos consistentes, mientras que YOLO-World permite una detección más fiable mediante la lógica de cruce de clases de texto.

## 5. Conclusiones
El entorno Colab presentó retos de compatibilidad con la carga dinámica de módulos VLM. Se priorizó la robustez de YOLO-World como solución definitiva para producción por su licencia y capacidad de inferencia estable.
