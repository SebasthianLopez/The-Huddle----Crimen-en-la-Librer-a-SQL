# The-Huddle----Crimen-en-la-Librer-a-SQL
Canalización de Web Scraping + PostgreSQL
📚 Books to Scrape — Pipeline de Web Scraping + PostgreSQL
Pipeline completo que extrae los 1000 libros de books.toscrape.com, los almacena en una base de datos PostgreSQL relacional y los analiza con consultas SQL optimizadas con índices.

🗂️ Estructura del Proyecto
books_scraper/
│
├── books_scraper.ipynb   # Notebook principal (4 celdas = 4 fases)
├── requirements.txt      # Dependencias Python
├── .env.example          # Plantilla de credenciales (copiar a .env)
├── .gitignore            # Excluye .env, __pycache__, etc.
└── README.md

⚙️ Tecnologías Utilizadas
HerramientaRolrequests + SessionPeticiones HTTP eficientes (reutiliza el socket TCP)BeautifulSoupParseo y navegación del HTMLThreadPoolExecutorScraping paralelo con 5 hilos simultáneosthreading.LockEvita condición de carrera al escribir en la lista compartidapsycopg2Driver Python ↔ PostgreSQLpython-dotenvCarga de credenciales desde .env (nunca hardcodeadas)PostgreSQLBase de datos relacional con 4 tablas y 3 índices

🗄️ Diagrama de la Base de Datos
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│     AUTORES     │    │  LIBROS_AUTORES  │    │     LIBROS      │
├─────────────────┤    │   (Tabla Pivot)  │    ├─────────────────┤
│ id (PK)         │◄───┤ autor_id (FK)    │───►│ id (PK)         │
│ nombre          │    │ libro_id (FK)    │    │ titulo          │
└─────────────────┘    └──────────────────┘    │ precio          │
                                               │ rating          │
┌─────────────────┐                            │ categoria_id(FK)│
│   CATEGORIAS    │◄───────────────────────────│ en_stock        │
├─────────────────┤                            │ url             │
│ id (PK)         │                            └─────────────────┘
│ nombre          │
└─────────────────┘
Relaciones:

AUTORES ↔ LIBROS: Muchos a Muchos (via libros_autores)
CATEGORIAS ↔ LIBROS: Uno a Muchos
