# 🔍 The Huddle — Crimen en la Librería SQL

> *"El sospechoso dejó 1000 libros. Nosotros los encontramos todos."*

Pipeline completo de **Web Scraping + PostgreSQL** que extrae los 1000 libros de [books.toscrape.com](https://books.toscrape.com/), los almacena en una base de datos relacional y los analiza con consultas SQL optimizadas.

![Python](https://img.shields.io/badge/Python-3.12-blue?logo=python&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-336791?logo=postgresql&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completado-success)

---

## 📁 Estructura del Proyecto

```
books_scraper/
│
├── books_scraper.ipynb   # Notebook principal (4 celdas = 4 fases)
├── requirements.txt      # Dependencias Python con versiones exactas
├── .env.example          # Plantilla de credenciales (copiar a .env)
├── .gitignore            # Excluye .env, __pycache__, checkpoints, etc.
└── README.md
```

---

## ⚙️ Tecnologías Utilizadas

| Herramienta | Rol |
|---|---|
| `requests` + `Session` | Peticiones HTTP eficientes (reutiliza el socket TCP) |
| `BeautifulSoup` | Parseo y navegación del HTML |
| `ThreadPoolExecutor` | Scraping paralelo con 5 hilos simultáneos |
| `threading.Lock` | Evita condición de carrera al escribir en la lista compartida |
| `psycopg2` | Driver Python ↔ PostgreSQL |
| `python-dotenv` | Carga de credenciales desde `.env` (nunca hardcodeadas) |
| PostgreSQL | Base de datos relacional con 4 tablas y 3 índices |

---

## 🗄️ Diagrama UML — Base de Datos

```
┌─────────────────────┐          ┌───────────────────────┐
│       AUTORES       │          │    LIBROS_AUTORES      │
├─────────────────────┤          │    (Tabla Puente)      │
│ PK  id    SERIAL    │◄─────────┤ FK  autor_id  INTEGER  │
│     nombre VARCHAR  │          │ FK  libro_id  INTEGER  │
└─────────────────────┘          └──────────┬────────────┘
                                            │
                                            ▼
┌─────────────────────┐          ┌───────────────────────┐
│      CATEGORIAS     │          │        LIBROS          │
├─────────────────────┤          ├───────────────────────┤
│ PK  id    SERIAL    │◄─────────┤ PK  id    SERIAL      │
│     nombre VARCHAR  │    FK    │     titulo  VARCHAR    │
└─────────────────────┘          │     precio  DECIMAL    │
                                 │     rating  INTEGER    │
                                 │ FK  categoria_id       │
                                 │     en_stock VARCHAR   │
                                 │     url      VARCHAR   │
                                 └───────────────────────┘
```

### Relaciones

| Relación | Tipo | Descripción |
|---|---|---|
| `AUTORES` ↔ `LIBROS` | Muchos a Muchos | Un autor puede tener varios libros y viceversa. Resuelta via tabla puente `libros_autores` |
| `CATEGORIAS` ↔ `LIBROS` | Uno a Muchos | Una categoría agrupa muchos libros |

---

## 🚀 Fases del Pipeline

| # | Celda | Fase | Descripción |
|---|---|---|---|
| 1 | `Celda 1` | **DDL** | Conexión a PostgreSQL y creación de las 4 tablas |
| 2 | `Celda 2` | **Scraping** | Extracción de 1000 libros con 5 hilos en paralelo |
| 3 | `Celda 3` | **DML** | Inserción de todos los datos en la base de datos |
| 4 | `Celda 4` | **SQL** | Consultas de análisis + índices de optimización |

---

## 🛠️ Cómo Ejecutar

### 1. Clonar el repositorio

```bash
git clone https://github.com/tu-usuario/books-scraper.git
cd books-scraper
```

### 2. Instalar dependencias

```bash
pip install -r requirements.txt
```

### 3. Configurar credenciales

```bash
cp .env.example .env
# Abrí .env y completá con tus datos de PostgreSQL
```

### 4. Crear la base de datos

```sql
CREATE DATABASE libros;
```

### 5. Ejecutar el notebook

```bash
jupyter notebook books_scraper.ipynb
```

Corré las celdas en orden del 1 al 4.

---

## 🔐 Seguridad

Las credenciales **nunca están hardcodeadas** en el código. Se cargan desde un archivo `.env` local que está bloqueado por `.gitignore`. El archivo `.env.example` (sin datos reales) sirve de plantilla para cualquiera que clone el repo.

---

## 👥 Equipo

| Nombre |
|---|
| Lucas Fleitas |
| Nahuel Aquino |
| Marcelo Paniagua |
| Cesar Espinola |
| Sebasthian Lopez |
