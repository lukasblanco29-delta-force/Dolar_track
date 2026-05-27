# 💵 DOLAR-TRACK — Analítica Cambiaria

> **Reto 3 | Economía y Finanzas**  
> Sistema End-to-End de monitoreo de TRM, gestión de inversionistas y análisis de decisión cambiaria.  
> **Grupo 4:** Juan Sebastian Salas · Alan Rossoff · Lukas Blanco

---

## 📋 Descripción del Proyecto

**DolarTrack** es un sistema de analítica cambiaria desarrollado en Python que permite registrar y analizar la Tasa Representativa del Mercado (TRM) del USD y EUR frente al COP durante todo el año 2025 (261 días hábiles), gestionar inversionistas con diferentes perfiles de riesgo, registrar operaciones de compra/venta de divisas y generar alertas automáticas de decisión de inversión.

El proyecto implementa una arquitectura **End-to-End** completa:

```
VS Code (Python) → SQLite Esquema Estrella → Tkinter (GUI) → Power BI (Dashboard 5 páginas)
```

---

## 🗂 Estructura del Proyecto

```
DolarTrack_FINAL/
├── main.py                                    # Orquestador principal
├── README.md                                  # Este archivo
│
├── Backend/                                   # Lógica de negocio y base de datos
│   ├── datos.py                               # Datos iniciales y constantes (Esquema Estrella)
│   ├── trm.py                                 # Registro y análisis de TRM
│   ├── inversionistas.py                      # Gestión de inversionistas
│   ├── operaciones.py                         # Operaciones de compra/venta
│   ├── alertas.py                             # Alertas y reportes analíticos
│   └── dolar_track.db                         # Base de datos SQLite
│
├── Frontend/                                  # Interfaz gráfica
│   ├── interfaz.py                            # GUI Tkinter completa
│   └── imagenes/
│       └── logo.png                           # Logo generado con Pillow
│
└── Dashboard/                                 # Inteligencia de Negocios
    ├── ENTREGA_FINAL_POWER_BI_GRUPO_4.pbix    # Dashboard Power BI (5 páginas)
    ├── DolarTrack_PowerBI_NUEVO.xlsx          # Base de datos Excel (261 registros/moneda)
    └── dashboard_dolartrack.html              # Dashboard web analítico
```

---

## ⚙️ Requisitos

- Python **3.10+**
- Librerías necesarias:

```bash
pip install pillow
```

> Las demás librerías (`tkinter`, `sqlite3`, `statistics`) vienen incluidas con Python.

---

## 🚀 Instalación y Ejecución

**1. Clona el repositorio:**
```bash
git clone https://github.com/tu-usuario/dolar-track.git
cd DolarTrack_FINAL
```

**2. Instala dependencias:**
```bash
pip install pillow
```

**3. Ejecuta el sistema:**
```bash
python main.py
```

La base de datos se crea automáticamente con el **Esquema Estrella** y datos iniciales en el primer arranque.

---

## 🖥 Interfaz Gráfica (Tkinter + Pillow)

La GUI cuenta con **5 módulos** accesibles desde la barra de navegación y un botón dedicado para abrir Power BI:

### 📊 Dashboard
- 4 botones CRUD rápidos: Registrar, Actualizar, Eliminar, Ver Tabla
- **Botón "📊 Abrir Power BI"** — abre el archivo .pbix automáticamente
- KPIs en tiempo real: TRM USD/EUR, promedios, volatilidad
- Señal automática de decisión (COMPRAR / VENDER / MANTENER)
- Tabla de historial reciente y últimas operaciones

### 📈 TRM
- Registro de TRM diaria (USD / EUR)
- Validación de formato de fecha y valores numéricos con `try-except`
- Señal automática al registrar cada valor
- Historial filtrable por moneda
- Actualización y eliminación de registros

### 👤 Inversionistas
- Registro con perfil de riesgo (Conservador / Moderado / Agresivo)
- Cálculo de exposición cambiaria según perfil
- Actualización de capital disponible

### 💼 Operaciones
- Registro de compras y ventas de divisas
- TRM manual o calculada automáticamente desde el promedio histórico
- Resumen de movimientos por inversionista en tiempo real

### 🔔 Alertas
- Análisis completo de señal de decisión
- Reporte de volatilidad y estadísticas
- Historial filtrable por moneda (USD / EUR)

---

## 🗄 Base de Datos — Esquema Estrella (SQLite)

### Diagrama Esquema Estrella

```
        dim_monedas (PK: id)
              │
              ▼
    ┌─── fact_trm ───┐
    │   fact_alertas  │
    └────────────────┘
              │
        dim_inversionistas (PK: id)
              │
              ▼
        fact_operaciones
```

### Tablas Dimensión

| Tabla | Descripción | Registros |
|---|---|---|
| `dim_monedas` | USD, EUR, GBP, JPY, CHF | 5 |
| `dim_inversionistas` | Perfiles y capitales | 5 |

### Tablas de Hechos

| Tabla | Descripción | Registros |
|---|---|---|
| `fact_trm` | TRM diaria histórica por moneda | 10+ |
| `fact_operaciones` | Compras y ventas de divisas | 6+ |
| `fact_alertas` | Señales automáticas generadas | 5+ |

> Todas las tablas se inicializan automáticamente con mínimo 5 registros al primer arranque.

---

## 📊 Power BI Dashboard — 5 Páginas

El archivo `ENTREGA_FINAL_POWER_BI_GRUPO_4.pbix` contiene **5 páginas** de análisis visual conectadas a una **Tabla Calendario** con jerarquía Año → Trimestre → Mes → Día:

### Página 1 — 🏠 Dashboard Principal
- 4 Tarjetas KPI: Promedio USD, TRM Máxima, TRM Mínima, Total COP Operado
- Gráfico de líneas: Evolución TRM USD vs EUR año completo 2025
- Gráfico de barras: Señal por mes (COMPRAR / VENDER / MANTENER)
- Segmentador de meses interactivo

### Página 2 — 📈 Evolución TRM
- Gráfico de líneas TRM USD — comportamiento diario 2025
- Gráfico de líneas TRM EUR — comportamiento diario 2025
- Gráfico de barras: Comparativo mensual USD vs EUR
- Segmentador de trimestres (Q1, Q2, Q3, Q4)

### Página 3 — 👤 Inversionistas
- Gráfico de barras: Capital por inversionista (COP)
- Gráfico circular: Capital expuesto por perfil de riesgo
- Gráfico de barras: USD posibles por inversionista
- Tarjeta: Total capital administrado
- Segmentador: Perfil de riesgo

### Página 4 — 💼 Operaciones
- Gráfico de barras agrupadas: Compras vs Ventas por inversionista
- Gráfico circular: Total COP — Compra vs Venta
- Gráfico de barras: Total COP por moneda (USD vs EUR)
- Tarjetas KPI: Total COP, Total USD Comprado, Total USD Vendido
- Segmentador: Tipo de operación

### Página 5 — 💡 Conclusiones
- Gráfico de líneas TRM USD — análisis anual
- Gráfico de líneas TRM EUR — análisis anual
- Segmentador de mes interactivo
- Cuadro de pregunta clave y respuesta de negocio

### Medidas DAX implementadas
- `TRM_Promedio_USD` — AVERAGE
- `TRM_Maxima_USD` — MAX
- `TRM_Minima_USD` — MIN
- `Volatilidad_USD` — DIVIDE + STDEV.P
- `TRM_Promedio_EUR` — AVERAGE
- `TRM_Maxima_EUR` — MAX
- `Total_COP_Operaciones` — SUM
- `Total_USD_Comprado` — CALCULATE + SUM
- `Total_USD_Vendido` — CALCULATE + SUM
- `Rentabilidad` — DIVIDE

### Columnas Calculadas DAX
- `Clasificacion TRM` — IF anidado (TRM ALTA / TRM MEDIA / TRM BAJA)
- `Ganancia Estimada` — Total IVA - Monto COP

---

## 📊 Análisis y Señales Automáticas

| Condición | Señal |
|---|---|
| TRM > promedio × 1.02 | 🔴 **VENDER** — TRM por encima del promedio |
| TRM < promedio × 0.98 | 🟢 **COMPRAR** — TRM por debajo del promedio |
| En rango normal | 🟡 **MANTENER** — TRM dentro del rango |

### Perfiles de riesgo

| Perfil | % Capital expuesto |
|---|---|
| Conservador | 20% |
| Moderado | 40% |
| Agresivo | 70% |

### Umbral de volatilidad
- Coeficiente de variación **> 2%** → ⚡ Alta volatilidad detectada

---

## 👥 Módulos del Backend

| Archivo | Clase | Responsabilidad |
|---|---|---|
| `trm.py` | `RegistroTRM` | CRUD de TRM, estadísticas, señales |
| `inversionistas.py` | `GestorInversionistas` | CRUD de inversionistas, exposición cambiaria |
| `operaciones.py` | `GestorOperaciones` | CRUD de operaciones, resúmenes |
| `alertas.py` | `GestorAlertas` | Registro de alertas, reportes de decisión |
| `datos.py` | — | Esquema Estrella, datos semilla, umbrales |

---

## 🔒 Seguridad y Validaciones

Todos los formularios implementan validación con `try-except` y `messagebox`:

- ✅ Campos obligatorios vacíos
- ✅ Formato de fecha `YYYY-MM-DD`
- ✅ Valores numéricos positivos
- ✅ Selección de perfil y moneda válidos
- ✅ Duplicados en registros de TRM por fecha y moneda
- ✅ Confirmación antes de eliminar registros

---

## 📌 Tecnologías Utilizadas

| Tecnología | Uso |
|---|---|
| Python 3.10+ | Lenguaje principal |
| SQLite3 | Base de datos relacional (Esquema Estrella) |
| Tkinter | Interfaz gráfica |
| Pillow | Procesamiento de imagen / logo |
| Power BI Desktop | Dashboard analítico visual (5 páginas) |
| openpyxl | Exportación de datos a Excel (261 registros/moneda) |

---

## 👨‍💻 Autores — Grupo 4

**Reto 3 — Economía "Dolar-Track"**  
Curso: Programación y Decisiones  
Universidad de La Sabana | Semestre 2026-1  
Arquitectura End-to-End: VS Code → SQLite → Tkinter → Power BI

| Estudiante | Rol |
|---|---|
| Juan Sebastian Salas Hinojosa | Analista de Datos BI |
| Alan Rossoff Vanegas | Desarrollador Frontend |
| Lukas Blanco Llanes | Arquitecto de Backend |
