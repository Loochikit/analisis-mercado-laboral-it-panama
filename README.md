# Análisis del Mercado Laboral IT en Panamá 🇵🇦
**Universidad Tecnológica de Panamá (UTP)**  
**Facultad de Ingeniería de Sistemas Computacionales (FISC)**  
**Curso:** Gestión de la Información — Segundo Parcial (Semestre I, 2026)  
**Grupo 4**

---

## 📌 1. Introducción del Proyecto

Este proyecto consiste en el desarrollo de un **Pipeline de Datos de Ingesta Inteligente**, un **Modelo de Aprendizaje Automático (Machine Learning)** y un **Dashboard Interactivo** para analizar la dinámica actual del mercado laboral en tecnologías de la información (IT) en la República de Panamá. 

El sistema realiza web scraping automatizado de portales de empleo, estructura los datos no estructurados de las ofertas de trabajo utilizando un Modelo de Lenguaje de Gran Escala (LLM - Google Gemini) para extraer información clave, aplica agrupamiento no supervisado (K-Means) para identificar perfiles profesionales de TI, modela el crecimiento de la demanda tecnológica con regresión lineal y expone todo a través de una interfaz interactiva de Streamlit dirigida a estudiantes, egresados y tomadores de decisiones académicas en la UTP.

---

## 🎯 2. Justificación del Problema en Panamá

El sector tecnológico en Panamá se encuentra en constante evolución, impulsado por el hub logístico y financiero del país. Sin embargo, existe una brecha notable entre las competencias que las empresas buscan y el contenido de los programas de estudio universitarios. 

### Desafíos Clave:
* **Falta de Datos en Tiempo Real:** Los estudios de empleabilidad tradicionales se publican con años de desfase, imposibilitando la toma de decisiones curriculares ágiles.
* **Opacidad Salarial:** Existe desinformación sobre la remuneración justa para los distintos roles tecnológicos en Panamá.
* **Evolución Tecnológica Acelerada:** Las tecnologías cambian a un ritmo que las estructuras académicas tradicionales no logran seguir.

**Este proyecto justifica su relevancia** al proveer a la UTP una herramienta basada en ciencia de datos que monitoriza las demandas del mercado directamente de portales activos como *Konzerta* y *Computrabajo*. Esto ayuda a identificar habilidades emergentes antes de que queden obsoletas, optimizando el perfil de egreso de los estudiantes de la FISC.

---

## 🛠️ 3. Tecnologías Utilizadas

El desarrollo del proyecto está cimentado en el ecosistema científico de Python:

* **Extracción y Web Scraping:** `requests`, `BeautifulSoup4` (Parsing DOM).
* **Procesamiento de Lenguaje Natural (NLP/LLM):** `google-generativeai` (Gemini API para extracción estructurada JSON).
* **Análisis de Datos:** `pandas`, `numpy`.
* **Base de Datos:** `sqlite3` (Motor relacional local de archivo).
* **Machine Learning:** `scikit-learn` (Modelos de K-Means, TF-IDF Vectorizer, PCA y LinearRegression).
* **Visualización e Interfaz:** `streamlit` (Estructura de la aplicación web) y `plotly` (Gráficos interactivos vectoriales).

---

## 📁 4. Arquitectura del Proyecto

El repositorio está organizado siguiendo las mejores prácticas de ingeniería de software para Ciencia de Datos:

```text
parcial_n2_grupo4/
├── data/
│   ├── raw/                        # Documentos crudos extraídos de scraping
│   └── processed/                  # Base de datos SQLite y archivos de exportación limpios
├── src/
│   ├── __init__.py
│   ├── pipeline.py                 # Código de Ingesta, Scraping y Llamadas al LLM
│   ├── modelo.py                   # Modelado K-Means y Regresión Temporal (Tendencias)
│   └── app.py                      # Dashboard de Streamlit con visualizaciones Plotly
├── requirements.txt                # Librerías y dependencias necesarias
└── README.md                       # Documentación del proyecto (este archivo)
```

---

## 🚀 5. Guía de Instalación Paso a Paso

Siga estos pasos para configurar y ejecutar el proyecto en un entorno local (Windows / macOS / Linux).

### Paso 1: Clonar el Repositorio
```bash
git clone https://github.com/tu-usuario/parcial_n2_grupo4.git
cd parcial_n2_grupo4
```

### Paso 2: Crear y Activar un Entorno Virtual
En la terminal de su sistema:
* **Windows (PowerShell):**
  ```powershell
  python -m venv venv
  .\venv\Scripts\Activate.ps1
  ```
* **macOS / Linux:**
  ```bash
  python3 -m venv venv
  source venv/bin/activate
  ```

### Paso 3: Instalar las Dependencias
Instale todas las librerías necesarias con el gestor de paquetes de Python `pip`:
```bash
pip install -r requirements.txt
```

### Paso 4: Configuración del Archivo de Entorno (`.env`)
Para utilizar la extracción inteligente con IA, cree un archivo llamado `.env` en la raíz del proyecto (`parcial_n2_grupo4/`) y añada su clave de API de Google Gemini:
```env
GEMINI_API_KEY="TU_GEMINI_API_KEY_AQUI"
```
*(Nota: Si no se configura la API Key, el pipeline activará automáticamente un sistema de fallback heurístico local para procesar los datos, por lo que la aplicación seguirá funcionando al 100%).*

---

## ⚙️ 6. Cómo Funcionan los Componentes

### A. El Pipeline de Datos ([pipeline.py](file:///c:/Users/bryan/Downloads/Super_Sandbox/parcial_n2_grupo4/src/pipeline.py))
El script realiza tres operaciones críticas:
1. **Scraping Base:** Emplea `requests` y `BeautifulSoup` para extraer las vacantes bajo el término "tecnologia".
2. **Generación / Simulación:** Cuenta con un generador robusto de datos simulados realistas de Panamá que actúa de inmediato si el scraping real falla o se ve bloqueado por cortafuegos (ej: Cloudflare). Esto permite tener más de 150 ofertas realistas (empresas panameñas, salarios en USD, etc.) distribuidas temporalmente sobre 6 meses.
3. **Procesamiento de LLM:** Utiliza la API de Gemini para analizar la oferta cruda y extraer un objeto estructurado con las habilidades técnicas específicas, los salarios máximos/mínimos y la experiencia requerida, guardándolos en la base de datos relacional SQLite (`data/processed/laboral_it.db`) y exportándolos a un archivo CSV consolidado.

### B. El Modelo de Machine Learning ([modelo.py](file:///c:/Users/bryan/Downloads/Super_Sandbox/parcial_n2_grupo4/src/modelo.py))
El script de modelado aplica dos técnicas:
1. **K-Means Clustering:** Convierte las habilidades a una matriz numérica usando TF-IDF. Agrupa las ofertas en 4 clusters correspondientes a perfiles profesionales (e.g. *Frontend / UI Web*, *Data & Analytics*, *DevOps*, *Backend / Core Systems*). Aplica **PCA (Análisis de Componentes Principales)** para reducir las dimensiones a 2 ($x$, $y$) para poder graficarlas.
2. **Regresión Lineal Temporal (Habilidades Emergentes):** Agrupa las vacantes en periodos quincenales, calcula la frecuencia porcentual de cada tecnología y ajusta una regresión lineal. La **pendiente ($m$)** determina la tasa de crecimiento quincenal de la habilidad, prediciendo cuáles tendrán mayor tracción a futuro.

### C. El Dashboard de Streamlit ([app.py](file:///c:/Users/bryan/Downloads/Super_Sandbox/parcial_n2_grupo4/src/app.py))
Es la interfaz interactiva.
1. **Filtros Dinámicos:** Permite a los usuarios filtrar por palabra clave, experiencia, salarios, portales de empleo e incluso habilidades específicas.
2. **Visualizaciones de Plotly:** Renderiza gráficos interactivos de barras para el Top 10 habilidades, diagramas de caja para ver la distribución salarial, un plano de dispersión 2D para ver los clusters de K-Means, y gráficos de crecimiento predictivo.
3. **Insights con LLM:** Integra un módulo que envía las estadísticas actuales a Gemini para generar un análisis estratégico automatizado dirigido a las autoridades académicas de la UTP.

---

## 📊 7. Ejecución del Proyecto

### Iniciar el Pipeline y los Modelos (Opcional)
Para regenerar la extracción de datos y el entrenamiento de los modelos, ejecute en su terminal:
```bash
python src/pipeline.py
python src/modelo.py
```
*(Nota: Dado que la base de datos `laboral_it.db` y el CSV `vacantes_limpias.csv` ya están pre-poblados y guardados en el repositorio, **no es estrictamente necesario correr este paso la primera vez**; puede ir directamente al paso de lanzar el Dashboard).*

### Lanzar el Dashboard de Streamlit
Para levantar el servidor local de visualización interactiva:
```bash
streamlit run src/app.py
```
Una ventana de su navegador predeterminado se abrirá automáticamente en la dirección: `http://localhost:8501`.

---

## 🛠️ 8. Solución de Problemas Comunes (Troubleshooting)

### A. Pantalla "Welcome to Streamlit! Email:" en la terminal
Al ejecutar por primera vez `streamlit run src/app.py`, Streamlit te solicitará un correo electrónico para noticias.
* **Solución**: Simplemente presiona **Enter** en tu teclado (dejando el campo vacío) para continuar. El servidor local se iniciará de inmediato.

### B. Error al activar el entorno virtual en PowerShell (Windows)
Si al ejecutar `.\venv\Scripts\Activate.ps1` recibes un mensaje indicando que *"la ejecución de scripts está deshabilitada en este sistema"*:
* **Solución 1**: Utiliza el **Símbolo del Sistema (CMD)** estándar de Windows en lugar de PowerShell y ejecuta:
  ```cmd
  venv\Scripts\activate
  ```
* **Solución 2**: Si deseas seguir usando PowerShell, abre una ventana de PowerShell como Administrador y ejecuta la siguiente política de permisos, luego vuelve a intentar la activación:
  ```powershell
  Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process
  ```

