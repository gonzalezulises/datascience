# DataScience Base Environment

Ambiente base para experimentación con IA y ciencia de datos, configurado con las mejores herramientas y librerías actuales.

## Especificaciones del Sistema

| Componente | Especificación |
|------------|----------------|
| **CPU** | Intel i7-10875H (8 núcleos/16 hilos) |
| **RAM** | 16 GB |
| **GPU** | NVIDIA RTX 2070 Super (8GB VRAM) |
| **CUDA** | 13.0 (Driver 580.126.09) |
| **OS** | Ubuntu 24.04.3 LTS |
| **Python** | 3.12.x |
| **Package Manager** | uv 0.9.18 |

## Stack Instalado

### Deep Learning
- **PyTorch 2.5.1+cu121** - Framework de deep learning con soporte CUDA
- **torchvision, torchaudio** - Extensiones para visión y audio
- **transformers 5.2.0** - Modelos preentrenados (Hugging Face)
- **accelerate** - Entrenamiento distribuido
- **peft** - Fine-tuning eficiente (LoRA, QLoRA)
- **bitsandbytes** - Cuantización de modelos

### Machine Learning Clásico
- **scikit-learn** - ML tradicional
- **xgboost** - Gradient boosting
- **lightgbm** - Gradient boosting optimizado
- **catboost** - Gradient boosting con categorías

### Procesamiento de Datos
- **pandas** - DataFrames clásicos
- **polars** - DataFrames rápidos (Rust-based)
- **numpy** - Computación numérica
- **datasets** - Datasets de Hugging Face

### Visualización
- **matplotlib** - Gráficos base
- **seaborn** - Visualización estadística
- **plotly** - Gráficos interactivos

### Notebooks
- **jupyterlab** - IDE de notebooks
- **ipykernel** - Kernel para Jupyter
- **ipywidgets** - Widgets interactivos

### Web Scraping
- **beautifulsoup4, lxml** - Parsing HTML
- **requests, httpx** - HTTP clients
- **playwright, selenium** - Browser automation

### Bases de Datos
- **sqlalchemy** - ORM
- **psycopg2-binary** - PostgreSQL
- **redis** - Cache/Queue

## Inicio Rápido

### Clonar y Configurar

```bash
git clone https://github.com/YOUR_USERNAME/datascience.git
cd datascience

# Crear ambiente virtual
uv venv

# Activar ambiente
source .venv/bin/activate

# Instalar dependencias
uv sync
```

### Verificar GPU

```bash
source .venv/bin/activate
python -c "import torch; print(f'CUDA: {torch.cuda.is_available()}'); print(f'GPU: {torch.cuda.get_device_name(0)}')"
```

### Iniciar JupyterLab

```bash
source .venv/bin/activate
jupyter lab
```

### Usar Positron IDE

```bash
positron ~/projects/datascience
```

El kernel "DataScience (Python 3.12)" ya está registrado.

## Crear Nuevo Proyecto

### Opción 1: Proyecto independiente

```bash
cd ~/projects
uv init mi-proyecto
cd mi-proyecto
uv venv
source .venv/bin/activate

# Añadir dependencias base
uv add numpy pandas matplotlib scikit-learn

# Añadir PyTorch con CUDA (requiere configurar índice)
# Ver pyproject.toml de este repo como referencia
```

### Opción 2: Copiar configuración de este proyecto

```bash
cd ~/projects
mkdir mi-proyecto && cd mi-proyecto

# Copiar configuración
cp ../datascience/pyproject.toml .
cp ../datascience/.gitignore .

# Editar nombre en pyproject.toml
sed -i 's/name = "datascience"/name = "mi-proyecto"/' pyproject.toml

# Crear ambiente e instalar
uv venv
uv sync
source .venv/bin/activate
```

### Opción 3: Proyecto minimalista

```bash
cd ~/projects
uv init proyecto-ligero
cd proyecto-ligero
uv venv
source .venv/bin/activate

# Solo lo esencial
uv add pandas polars matplotlib jupyterlab ipykernel
```

## Configuración de PyTorch con CUDA

Para usar PyTorch con CUDA en nuevos proyectos, añadir al `pyproject.toml`:

```toml
[[tool.uv.index]]
name = "pytorch-cu121"
url = "https://download.pytorch.org/whl/cu121"
explicit = true

[tool.uv.sources]
torch = { index = "pytorch-cu121" }
torchvision = { index = "pytorch-cu121" }
torchaudio = { index = "pytorch-cu121" }
```

Luego instalar:

```bash
uv add torch torchvision torchaudio
```

## Estructura del Proyecto

```
datascience/
├── .venv/              # Ambiente virtual (ignorado en git)
├── .gitignore          # Archivos ignorados
├── pyproject.toml      # Configuración del proyecto y dependencias
├── uv.lock             # Lock file de dependencias
├── README.md           # Este archivo
├── CLAUDE.md           # Contrato para Claude Code
├── notebooks/          # Jupyter notebooks
├── src/                # Código fuente
├── data/               # Datos (ignorado en git)
└── models/             # Modelos entrenados (ignorado en git)
```

## Comandos Útiles

```bash
# Activar ambiente
source .venv/bin/activate

# Añadir dependencia
uv add nombre-paquete

# Actualizar dependencias
uv sync --upgrade

# Ver dependencias instaladas
uv pip list

# Ejecutar script
python script.py

# Iniciar Jupyter
jupyter lab

# Registrar kernel para Jupyter
python -m ipykernel install --user --name=mi-kernel --display-name="Mi Kernel"
```

## Limitaciones del Hardware

- **RAM 16GB**: Modelos >7B parámetros requieren cuantización
- **VRAM 8GB**: Usar técnicas como QLoRA para LLMs grandes
- **Recomendación**: Para modelos grandes, usar `bitsandbytes` con 4-bit quantization

## IDE Recomendado

**Positron** - IDE especializado en ciencia de datos
- Variable Explorer
- Data Frame Explorer
- Multi-session console
- Soporte nativo para notebooks

Instalación: Ya instalado en `/usr/bin/positron`

## Licencia

MIT
