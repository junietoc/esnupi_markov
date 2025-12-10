# 🎮 OchoBeto

## Generador de Música 8-bit usando Cadenas de Markov

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.8+-blue.svg" alt="Python">
  <img src="https://img.shields.io/badge/License-MIT-green.svg" alt="License">
  <img src="https://img.shields.io/badge/Universidad-Nacional%20de%20Colombia-red.svg" alt="UNAL">
</p>

> Proyecto final del curso **Cadenas de Markov** - Departamento de Matemáticas, Facultad de Ciencias, Universidad Nacional de Colombia.

---

## 📋 Descripción

**OchoBeto** es un sistema de composición algorítmica que utiliza cadenas de Markov para generar melodías en estilo retro 8-bit. El proyecto implementa y compara dos métodos de generación:

1. **Método Tradicional**: Cadenas de Markov aplicadas directamente sobre secuencias de notas
2. **Método Xu (2023)**: Cadenas de Markov para progresiones de acordes + interpolación de Lagrange para melodías

### 🎯 Pregunta de Investigación

> *¿Cómo automatizar la creación musical sin comprometer la originalidad y creatividad características de la producción humana?*

---

## 👥 Autores

| Nombre | GitHub |
|--------|--------|
| Inka Michelle Hernández Vásquez | - |
| Daniel Santiago López Daza | - |
| Juliana Alejandra Nieto Cárdenas | - |

**Profesor**: Freddy Rolando Hernández Romero

---

## 🏗️ Estructura del Proyecto

```
ochobeto/
├── 📁 midis/                          # Archivos MIDI de entrenamiento
│   ├── ZeldaFantasy_1_.mid
│   ├── BomberMan2_-_Stage3.mid
│   └── 3D_Worldrunner_Bonus.mid
├── 📁 generated_music/                # Melodías generadas
│   ├── *_tradicional.mid
│   └── *_xu_lagrange.mid
├── 📁 notebooks/
│   └── Generacion_de_musica_con_markov.ipynb
├── 📁 docs/
│   ├── Proyecto.pdf                   # Documento técnico
│   └── Presentacion.pdf               # Diapositivas
├── 📄 README.md
└── 📄 requirements.txt
```

---

## 🚀 Instalación

### Prerrequisitos

- Python 3.8 o superior
- pip (gestor de paquetes de Python)

### Pasos

1. **Clonar el repositorio**
```bash
git clone https://github.com/usuario/ochobeto.git
cd ochobeto
```

2. **Crear entorno virtual (recomendado)**
```bash
python -m venv venv
source venv/bin/activate  # Linux/Mac
# o
venv\Scripts\activate     # Windows
```

3. **Instalar dependencias**
```bash
pip install -r requirements.txt
```

### Dependencias principales

```txt
numpy>=1.21.0
pandas>=1.3.0
music21>=7.0.0
matplotlib>=3.4.0
seaborn>=0.11.0
scipy>=1.7.0
```

---

## 💻 Uso

### Ejecución básica

```python
from cadena_markov import CadenaDeMarkov
from utils import extraer_eventos, guardar_midi

# 1. Cargar y procesar MIDI
eventos = extraer_eventos('midis/ZeldaFantasy_1_.mid')

# 2. Crear y entrenar cadena de Markov
cadena = CadenaDeMarkov(orden=2)
secuencia = [(e['nota'], e['duracion']) for e in eventos]
cadena.entrenar(secuencia)

# 3. Generar melodía
melodia = cadena.generar_secuencia(longitud=100, temperatura=1.0)

# 4. Guardar resultado
guardar_midi(melodia, 'generated_music/mi_melodia.mid')
```

### Método Xu (Lagrange)

```python
from generador_xu import GeneradorXu

# 1. Crear generador con MIDI de referencia
gen = GeneradorXu('midis/ZeldaFantasy_1_.mid')
gen.entrenar()

# 2. Generar pieza
acordes, melodia = gen.generar_pieza(n_compases=16)

# 3. Guardar con acompañamiento de acordes
guardar_midi(melodia, 'output.mid', con_acordes=True)
```

### Parámetros importantes

| Parámetro | Descripción | Valores típicos |
|-----------|-------------|-----------------|
| `orden` | Orden de la cadena de Markov | 1, 2, 3 |
| `temperatura` | Control de aleatoriedad (menor=determinístico) | 0.5 - 2.0 |
| `chord_threshold` | Umbral para detectar acordes (segundos) | 0.05 |
| `n_compases` | Número de compases a generar | 8 - 32 |

---

## 📊 Métricas de Evaluación

El sistema implementa 4 métricas cuantitativas:

| Métrica | Qué mide | Rango ideal |
|---------|----------|-------------|
| **Cross-Entropy** | Coherencia según el modelo | 3-8 bits |
| **Perplexity** | Opciones efectivas por paso | 5-20 |
| **JS Divergence** | Similaridad al estilo original | < 0.15 |
| **Longest Copy** | Originalidad (evitar plagio) | < 15 notas |

### Resultados obtenidos

| Método | Cross-Entropy | Perplexity | JS Div. | Longest Copy |
|--------|---------------|------------|---------|--------------|
| **Tradicional** | 0.52 bits | 1.43 | 0.128 | 18 notas |
| **Xu (Lagrange)** | 26.41 bits | 89.4M | 0.276 | 5 notas |

**Interpretación**: 
- El método tradicional genera melodías coherentes pero tiende a **memorizar** el material original
- El método Xu genera melodías más **originales** pero menos coherentes

---

## 🎵 Ejemplos de Audio

Los archivos generados están disponibles en la carpeta `generated_music/`:

| Canción Original | Método Tradicional | Método Xu |
|------------------|-------------------|-----------|
| ZeldaFantasy | `ZELDA.wav` | `ZELDA_LAGRANGE.wav` |
| BomberMan2 | `BOMBERMAN.wav` | `BOMBERMAN_LAGRANGE.wav` |
| 3D Worldrunner | `WORLDRUNNER.wav` | `WORLDRUNNER_LAGRANGE.wav` |

---

## 📚 Marco Teórico

### Composición Algorítmica

> *"Algorithmic composition: the process of using some formal process to make music with minimal human intervention"* — Alpern (1995)

### Referencias Históricas

1. **ILLIAC Suite (1957)** - Hiller & Isaacson: Primera composición computacionalmente generada
2. **Analogique A (1958)** - Xenakis: Primera obra completamente automatizada con procesos estocásticos
3. **The Continuator (2010)** - Pachet: Sistema de acompañamiento en tiempo real

### Cadenas de Markov de Orden Superior

Una cadena de orden N es aquella donde el siguiente estado depende de los últimos N estados:

$$P(X_n | X_{n-1}, X_{n-2}, ..., X_0) = P(X_n | X_{n-1}, X_{n-2}, ..., X_{n-N})$$

### Fórmula de Temperatura

$$P'(x_i) = \frac{P(x_i)^{1/T}}{\sum_j P(x_j)^{1/T}}$$

Donde:
- T → 0: Comportamiento determinístico
- T = 1: Distribución original
- T → ∞: Distribución uniforme

---

## 🔬 Metodología

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   MIDI      │     │  Extracción │     │   Entrenar  │     │   Generar   │
│   Input     │ ──► │   Eventos   │ ──► │   Cadena    │ ──► │   Melodía   │
└─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘
                           │                   │                   │
                           ▼                   ▼                   ▼
                    Notas, acordes,      Matriz de          MIDI Output
                    duraciones           transición         + Evaluación
```

### Pipeline del Método Xu

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  Detectar   │     │   Markov    │     │  Lagrange   │     │   Anti-     │
│  Tonalidad  │ ──► │   Acordes   │ ──► │  Contorno   │ ──► │ Repetición  │
└─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘
       │                   │                   │                   │
       ▼                   ▼                   ▼                   ▼
   F major            I-IV-V-I           Curva suave        Memoria de
   Escala             Progresión         interpolada        4 notas
```

---

## 📈 Trabajo Futuro

- [ ] Implementar cadenas de Markov de orden variable (n-gramas adaptativos)
- [ ] Explorar Hidden Markov Models para capturar estructura latente
- [ ] Desarrollar métricas de evaluación perceptual con usuarios humanos
- [ ] Ampliar corpus con más géneros musicales
- [ ] Implementar interacción en tiempo real (estilo Continuator)
- [ ] Añadir más parámetros musicales (volumen, articulación, etc.)

---

## 📖 Bibliografía

1. Nierhaus, G. *Algorithmic composition: Paradigms of automated music generation*. Springer.
2. Tremonte de Carvalho, H. *An introduction to Markov chains in music composition and analysis*. UFRJ.
3. Shapiro, I., & Huber, M. (2021). *Markov chains for computer music generation*. Journal of Humanistic Mathematics.
4. **Xu, Y. (2023)**. *Music generator applying Markov chain and Lagrange interpolation*. CMLAI 2023.
5. Maurer, J. A. (1999). *A brief history of algorithmic composition*.
6. Pachet, F. (2010). *The Continuator: Musical interaction with style*. Journal of New Music Research.

---

## 📄 Licencia

Este proyecto está bajo la Licencia MIT. Ver el archivo `LICENSE` para más detalles.

---

## 🙏 Agradecimientos

- Profesor Freddy Rolando Hernández Romero por la guía y retroalimentación
- Universidad Nacional de Colombia, Departamento de Matemáticas
- Comunidad de música 8-bit por los archivos MIDI de referencia

---

<p align="center">
  <b>Universidad Nacional de Colombia</b><br>
  Facultad de Ciencias - Departamento de Matemáticas<br>
  Curso: Cadenas de Markov<br>
  Diciembre 2025
</p>
