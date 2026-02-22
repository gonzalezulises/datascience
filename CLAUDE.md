# CLAUDE.md - Contrato de Proyecto DataScience

## Contexto del Proyecto

Este es el ambiente base para experimentación con IA y ciencia de datos del usuario. Fue configurado el 2026-02-21 con las siguientes características:

### Hardware
- **CPU**: Intel i7-10875H (8 núcleos/16 hilos, hasta 5.1GHz)
- **RAM**: 16 GB
- **GPU**: NVIDIA RTX 2070 Super Mobile (8GB VRAM)
- **CUDA**: 13.0 con driver 580.126.09
- **Disco**: 1.8 TB NVMe

### Software Base
- **OS**: Ubuntu 24.04.3 LTS (kernel 6.17)
- **Python**: 3.12.x
- **Package Manager**: uv (NUNCA usar pip directamente)
- **IDE**: Positron (instalado en /usr/bin/positron)

---

## Convenciones Obligatorias

### Package Manager
- **DEBE** usar `uv` para gestión de paquetes, NUNCA pip
- Comandos: `uv add`, `uv sync`, `uv pip list`, `uv venv`

### Ambientes Virtuales
- Cada proyecto DEBE tener su propio `.venv/`
- Activar con: `source .venv/bin/activate`
- Los ambientes NO se commitean a git

### PyTorch con CUDA
- Usar índice explícito `pytorch-cu121` (ver pyproject.toml)
- Verificar GPU: `python -c "import torch; print(torch.cuda.is_available())"`

### Estructura de Proyectos
```
proyecto/
├── .venv/              # Ambiente (gitignore)
├── .gitignore
├── pyproject.toml      # Dependencias
├── uv.lock
├── CLAUDE.md           # Contexto para Claude
├── README.md
├── notebooks/
├── src/
├── data/               # Gitignore
└── models/             # Gitignore
```

---

## Estado Actual del Proyecto

### Dependencias Instaladas
- **Deep Learning**: torch 2.5.1+cu121, transformers 5.2.0, accelerate, peft, bitsandbytes
- **ML**: scikit-learn, xgboost, lightgbm, catboost
- **Datos**: pandas, polars, numpy, datasets
- **Viz**: matplotlib, seaborn, plotly
- **Notebooks**: jupyterlab, ipykernel

### Kernels Registrados
- `datascience` → "DataScience (Python 3.12)"

### Configuración Especial
- PyTorch usa índice CUDA 12.1 (compatible con RTX 2070 Super)
- Python limitado a >=3.12,<3.14 para compatibilidad

---

## Instrucciones para Nuevos Proyectos

### Crear proyecto nuevo (método recomendado)

```bash
cd ~/projects
uv init nombre-proyecto
cd nombre-proyecto

# Copiar configuración CUDA de datascience
cat >> pyproject.toml << 'EOF'

[[tool.uv.index]]
name = "pytorch-cu121"
url = "https://download.pytorch.org/whl/cu121"
explicit = true

[tool.uv.sources]
torch = { index = "pytorch-cu121" }
torchvision = { index = "pytorch-cu121" }
torchaudio = { index = "pytorch-cu121" }
EOF

# Limitar Python
sed -i 's/requires-python = ">=3.12"/requires-python = ">=3.12,<3.14"/' pyproject.toml

# Crear ambiente
uv venv
source .venv/bin/activate

# Instalar dependencias según necesidad
uv add torch torchvision  # Deep learning
uv add pandas polars      # Datos
uv add jupyterlab ipykernel  # Notebooks

# Registrar kernel
python -m ipykernel install --user --name=nombre-proyecto
```

### Clonar este proyecto como base

```bash
cd ~/projects
cp -r datascience nuevo-proyecto
cd nuevo-proyecto
rm -rf .venv .git uv.lock

# Cambiar nombre
sed -i 's/name = "datascience"/name = "nuevo-proyecto"/' pyproject.toml

# Reinicializar
git init
uv venv
uv sync
source .venv/bin/activate
```

---

## Limitaciones a Recordar

1. **RAM 16GB**: Modelos LLM >7B requieren cuantización (4-bit con bitsandbytes)
2. **VRAM 8GB**: Usar gradient checkpointing y batch sizes pequeños
3. **Técnicas recomendadas para LLMs grandes**:
   - QLoRA (4-bit) con peft
   - `load_in_4bit=True` en transformers
   - `torch.cuda.empty_cache()` entre operaciones

---

## Tareas Pendientes / Roadmap

- [ ] Configurar pre-commit hooks
- [ ] Añadir estructura de notebooks de ejemplo
- [ ] Configurar MLflow para tracking de experimentos
- [ ] Crear templates de proyectos específicos (NLP, CV, tabular)

---

## Comandos Frecuentes

```bash
# Activar ambiente
cd ~/projects/datascience && source .venv/bin/activate

# Verificar GPU
python -c "import torch; print(torch.cuda.get_device_name(0))"

# Iniciar JupyterLab
jupyter lab

# Abrir en Positron
positron .

# Ver uso de GPU
nvidia-smi

# Añadir paquete
uv add nombre-paquete

# Actualizar todo
uv sync --upgrade
```

---

## Historial de Sesiones

### 2026-02-21 - Configuración Inicial
- Sistema actualizado (135 paquetes)
- Instalado stack completo de ciencia de datos
- Configurado PyTorch con CUDA 12.1
- Instalado Positron IDE
- Creado repositorio GitHub
- GPU verificada: RTX 2070 Super funcionando correctamente
