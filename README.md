# 👔 Simulador de Entrevistas Técnicas con IA (Líder Técnico Virtual)

Aplicación web interactiva construida con **Streamlit** y el SDK oficial **Google GenAI (`google-genai`)** que automatiza la primera etapa de selección técnica de programadores. El sistema actúa como un **Líder Técnico Virtual** exigente y profesional que conduce una entrevista técnica adaptativa de exactamente 3 preguntas, evalúa las respuestas del aspirante y emite un dictamen final de contratación en formato de reporte visual.

---

## 🎯 Escenario del Problema

Una empresa del sector tecnológico desea automatizar la primera etapa de sus filtros de selección para programadores. Esta aplicación:
1. Simula una entrevista técnica adaptativa de **exactamente 3 preguntas**.
2. Evalúa las respuestas del aspirante en tiempo real con retroalimentación técnica.
3. Emite un **dictamen final de contratación automático** en formato de reporte visual estructurado en Markdown.

---

## 📋 Especificaciones del Sistema y Checklist de Evaluación

### A. Interfaz de Usuario y Configuración Inicial
- **Selector de Rol (`st.selectbox`):** Permite elegir entre al menos 3 vacantes técnicas:
  - 🐍 *Desarrollador Python Junior* (estructuras de datos, funciones, mutabilidad, excepciones).
  - 🌐 *Desarrollador Web Frontend* (HTML5 semántico, CSS Flexbox/Grid, responsive design, JavaScript moderno/DOM).
  - 🤖 *Integrador de APIs/Bots* (arquitectura REST, métodos HTTP, códigos de estado, autenticación Bearer/OAuth, webhooks).
- **Botón "Reiniciar Entrevista":** Limpia la memoria de la sesión (`st.session_state`) sin provocar errores en pantalla, restableciendo el contador a 0 y comenzando una nueva evaluación.
- **Preparado para Streamlit Cloud:** Detección automática de la API Key mediante `st.secrets`, variables de entorno o entrada manual en la barra lateral.

### B. Lógica Conversacional y Control de Estado (`st.session_state`)
- **Límite Estricto:** La entrevista consiste en **exactamente 3 preguntas técnicas adaptativas**.
- **Dinámica Adaptativa:** Gemini formula cada nueva pregunta tomando en cuenta la respuesta anterior dada por el usuario (profundizando, retando o proponiendo escenarios prácticos).
- **Transición de Estado:** Al recibir la 3.ª respuesta, el chat se desactiva inmediatamente, deteniendo la fase de preguntas y activando la fase de evaluación. Tras la respuesta #3 **no se genera una 4.ª pregunta**.

### C. System Instructions y Protección de Rol (Anti-Jailbreak)
- **Rol Exigente:** El modelo asume el rol de un entrevistador técnico formal, riguroso y profesional.
- **Protección Anti-Jailbreak:** Si el candidato intenta desviar la conversación (pedir recetas, solicitar poemas, reescribir código ajeno o pedir ignorar instrucciones), el bot declina amablemente pero con firmeza, recuerda el rol que se está evaluando y reorienta al aspirante hacia la pregunta técnica pendiente, **sin avanzar el contador de turnos**.

### D. Generación del Reporte Final
Al completarse la 3.ª respuesta, el sistema solicita a Gemini la construcción de un reporte estructurado en Markdown que contiene:
1. **Calificación Global:** Puntuación numérica precisa de 1 a 10.
2. **Fortalezas Detectadas:** Al menos 2 aspectos técnicos positivos evidenciados.
3. **Áreas de Mejora:** Al menos 2 conceptos que el aspirante debe reforzar con recomendaciones de estudio.
4. **Dictamen Final:** Una etiqueta clara: `APROBADO` (calificación ≥ 7.0) o `NO APROBADO` (calificación < 7.0).
5. **Descarga de Reporte:** Botón para descargar el reporte completo en formato Markdown (`.md`).

---

## 🧪 Matriz de Casos de Prueba (Edge Cases)

| # | Caso de Prueba | Comportamiento Esperado y Verificado |
|---|----------------|--------------------------------------|
| **1** | **Intento de distracción / Jailbreak** | El bot se niega a desviarse del tema (recetas, poemas, evasiones) y pide al candidato responder la pregunta técnica pendiente. El contador no avanza. |
| **2** | **Límite estricto de turnos** | Tras la respuesta #3, no se genera una 4.ª pregunta. Se desactiva el chat y se muestra directamente el reporte final. |
| **3** | **Reinicio limpio de sesión** | Al hacer clic en "Reiniciar Entrevista", el contador vuelve a 0, el historial se borra y la app inicia una nueva entrevista sin errores en pantalla. |
| **4** | **Coherencia por vacante** | Las preguntas de Frontend abordan HTML/CSS/JS/DOM; las de Python Junior tratan sobre estructuras de datos y funciones; las de Integrador de APIs/Bots abordan REST, HTTP y autenticación. |

---

## 🚀 Instalación y Ejecución Local

1. **Clonar el repositorio:**
   ```bash
   git clone https://github.com/alfredoaguilera76/Simulador-de-Entrevistas-T-cnicas-con-IA.git
   cd Simulador-de-Entrevistas-T-cnicas-con-IA
   ```

2. **Instalar dependencias:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Configurar la API Key de Gemini:**
   Crea el archivo `.streamlit/secrets.toml` con tu clave de [Google AI Studio](https://aistudio.google.com/):
   ```toml
   GEMINI_API_KEY = "tu_api_key_aqui"
   ```
   *(También puedes ingresarla directamente en la barra lateral de la app o mediante la variable de entorno `GEMINI_API_KEY`).*

4. **Ejecutar la aplicación:**
   ```bash
   streamlit run app.py
   ```

---

## ☁️ Despliegue en Streamlit Community Cloud

1. Sube tu código a GitHub.
2. Ingresa a [share.streamlit.io](https://share.streamlit.io/).
3. Conecta tu repositorio de GitHub y selecciona la rama principal (`main`) y el archivo `app.py`.
4. En la configuración avanzada (**Advanced settings**), añade tu secreto:
   ```toml
   GEMINI_API_KEY = "tu_api_key_de_google_ai_studio"
   ```
5. Haz clic en **Deploy**. ¡Tu aplicación estará pública y accesible mediante su URL de Streamlit Cloud!
