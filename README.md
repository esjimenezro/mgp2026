# Introducción a los Modelos Gráficos Probabilísticos - Otoño 2026

En este repositorio se encuentra el material para el curso de modelos gráficos probabilísticos para el periodo de Otoño 2026.

Este curso está dividido en tres módulos.

## Instalación

El entorno del curso se administra con [`uv`](https://docs.astral.sh/uv/), que resuelve
dependencias, instala Python y crea el entorno virtual en un solo paso. El curso ya **no**
usa conda.

### 1. Prerrequisitos

- **`uv`** instalado. Si no lo tienes:
  - Windows (PowerShell): `powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"`
  - macOS / Linux: `curl -LsSf https://astral.sh/uv/install.sh | sh`
  - Verifica con `uv --version` (si es una versión vieja, `uv self update`).
- **Graphviz** (el binario del sistema, no el paquete de Python) — lo usa `pgmpy` para
  dibujar redes Bayesianas:
  - Windows: `winget install Graphviz.Graphviz`
  - macOS: `brew install graphviz`
  - Ubuntu/Debian: `sudo apt install graphviz`

No necesitas instalar Python tú mismo: `uv` descarga e instala automáticamente la versión
que pide el proyecto (3.12) la primera vez que la necesita. Tampoco necesitas un
compilador de C: el curso usa [`nutpie`](https://github.com/pymc-devs/nutpie) (sampler
NUTS en Rust, con wheels precompilados) como backend de muestreo de PyMC.

### 2. Clonar el repositorio

```bash
git clone https://github.com/esjimenezro/mgp2026.git
cd mgp2026
```

### 3. Crear el entorno e instalar dependencias

```bash
uv sync
```

Esto instala Python 3.12 si no lo tienes, crea un entorno virtual en `.venv/` e instala
exactamente las versiones fijadas en `uv.lock` (PyMC 6.x con nutpie, pgmpy, arviz,
JupyterLab, etc. — la lista completa de dependencias directas está en `pyproject.toml`).

### 4. Registrar el kernel de Jupyter

```bash
uv run python -m ipykernel install --user --name mgp2026 --display-name "Python (mgp2026)"
```

Así el kernel **Python (mgp2026)** aparece en Jupyter Lab, VS Code, etc., apuntando al
`.venv` del proyecto.

### 5. Levantar Jupyter Lab

```bash
uv run jupyter lab
```

Si vas a trabajar desde VS Code: abre la carpeta del repo y, al abrir cualquier notebook,
selecciona el kernel `mgp2026` (o el intérprete `.venv\Scripts\python.exe` en Windows /
`.venv/bin/python` en macOS/Linux).

### 6. Actualizar el entorno más adelante

```bash
uv sync              # sincroniza tu .venv con lo que dice uv.lock
uv lock --upgrade    # recalcula uv.lock con las versiones más nuevas compatibles
```

### Notas técnicas

- **Sampler (`nuts_sampler`)**: al instalar `pymc[nutpie]` (ya incluido en
  `pyproject.toml`), PyMC usa `nutpie` como sampler por defecto automáticamente —no hace
  falta pasar `nuts_sampler="nutpie"` a mano en `pm.sample(...)`. Si ves un *warning* de
  PyTensor sobre que no encontró `g++`, es normal y no bloquea la ejecución: solo aplica
  a las partes del grafo que no corren por nutpie.
- **Graphviz**: el paquete de Python `graphviz` (ya en `pyproject.toml`) es solo un
  envoltorio; necesita el binario `dot` del sistema instalado por separado (ver paso 1).
  Si `pgmpy` truena al dibujar una red con un error del tipo *"failed to execute
  WindowsPath('dot')"*, revisa que el binario esté en el `PATH`.
- **Windows + OneDrive**: si tu clon del repo vive dentro de una carpeta sincronizada con
  OneDrive, `uv sync` puede fallar intermitentemente con errores de "Acceso denegado" por
  el bloqueo de archivos del sincronizador. Si te pasa, pausa temporalmente la
  sincronización de OneDrive mientras corre `uv sync` y reanúdala después.

#### Módulo 1. Introducción al modelado probabilístico.

En este módulo introduciremos los contenidos del curso y usaremos un ejemplo sencillo
para motivar el estudio de los modelos gráficos probabilísticos.

Exploraremos modelos probabilísticos básicos, dejando de lado la parte gráfica por un momento,
y en el camino recordaremos tópicos fundamentales de teoría de probabilidad, estimación de
máxima verosimilitud, y además discutiremos acerca del enfoque Bayesiano.

Todo esto lo usaremos en una aplicación básica que nos conducirá a la implementación de un
modelo de regresión que puede trabajar online.

1. Introducción y motivación al curso.

  - Presentación de la guía de aprendizaje.
  - Instalación de herramientas de trabajo.
  - Ejemplo introductorio.

2. Repaso de probabilidad.

  - Conceptos básicos de probabilidad.
  - Variables aleatorias discretas.
  - Variables aleatorias continuas.

3. Estimadores de máxima verosimilitud & máximo aposteriori.

  - Principio de máxima verosimilitud.
  - Estimadores de máxima verosimilitud.
  - Ajuste de curvas y máxima verosimilitud.
  - Máximo aposteriori.

4. Actualización Bayesiana.

  - Actualización Bayesiana discreta.
  - Actualización Bayesiana continua.

5. Regresión lineal Bayesiana.

  - Introducción a PyMC.
  - Lenguaje de modelos probabilísticos.
  - Regresión lineal Bayesiana.

#### Módulo 2. Representación, inferencia, aprendizaje y toma de decisiones en redes Bayesianas.

En este módulo aprenderemos acerca de una representación de modelos gráficos probabilísticos:
las redes Bayesianas (grafos dirigidos).

Estudiaremos tanto las propiedades teóricas de dichas representaciones así como su uso en la práctica.
Así mismo, aprenderemos cómo responder preguntas usando las estructuras gráficas y cómo aprender
sus parámetros a partir de datos.

Cerraremos el módulo extendiendo estas representaciones hacia redes de decisión, que nos
permiten modelar procesos de toma de decisión bajo incertidumbre incorporando explícitamente
preferencias (utilidad) y no solo creencias (probabilidad).

6. Redes Bayesianas.

  - Factores y operaciones con factores.
  - Redes Bayesianas y modelos probabilísticos.
  - Flujo de influencia probabilística en redes Bayesianas.

7. Inferencia en redes Bayesianas.

  - Introducción a inferencia probabilística.
  - Algoritmo de eliminación de variables.
  - Inferencia aproximada.

8. Aprendizaje en redes Bayesianas.

  - Estimación de parámetros en redes Bayesianas.

9. Redes de decisión.

  - De redes Bayesianas a redes de decisión: nodos de azar, decisión y utilidad.
  - Utilidad esperada y cálculo de la política óptima.
  - Valor de la información.
  - Caso aplicado: una decisión bajo incertidumbre con datos reales (p. ej. aprobación de
    crédito, decisión clínica o mantenimiento predictivo), integrando lo aprendido en los
    capítulos 6-8.

#### Módulo 3. Aplicaciones seleccionadas: modelos de variable latente.

En este módulo utilizaremos nuestro conocimiento en redes Bayesianas para estudiar una familia
de modelos en los que una variable no observada (latente) explica la estructura de los datos
observados.

Partiremos de un caso discreto y sencillo — el clustering — y avanzaremos hacia
datos secuenciales. Cerraremos el módulo con una síntesis que conecta explícitamente esta
familia de modelos con las redes Bayesianas estudiadas en el Módulo 2.

10. Modelos de variables latentes para clustering.

  - K-Medias.
  - Mezclas Gaussianas y algoritmo de maximización de la esperanza (EM).
  - Caso aplicado con datos reales (p. ej. Old Faithful, Palmer Penguins o segmentación
    de clientes).

11. Modelos ocultos de Markov.

  - Variables latentes en datos secuenciales.
  - Algoritmos forward-backward y Viterbi.
  - Caso aplicado con datos reales (p. ej. series de tiempo, regímenes de mercado o clima).

12. Síntesis: de Naive Bayes a modelos de variable latente. (material adicional)

  - Naive Bayes como red Bayesiana con variable de clase.
  - Conexión entre Naive Bayes y mezclas Gaussianas.
  - Cierre del curso: un mismo lenguaje gráfico para clasificación, clustering y series de tiempo.
