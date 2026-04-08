# 🚗 Sistema de Trazabilidad Logística — Precarga de Unidades Toyota

> **Solución end-to-end** para el registro, cruce y análisis de averías en operaciones de transporte vehicular internacional.  
> Stack: `AppSheet` · `Google Sheets` · `Google Apps Script` · `Power BI (DAX)` · `Power Automate`

---

## 🧩 Contexto del problema

En operaciones de transporte de vehículos 0km, uno de los mayores conflictos entre fabricantes, transportistas y concesionarios es **la atribución de daños**: ¿el vehículo llegó dañado de planta, se dañó durante el transporte, o fue en la descarga en destino?

Sin evidencia documentada, el transportista pierde por defecto.

Este sistema fue diseñado e implementado para **eliminar esa ambigüedad** mediante el registro digital de cada unidad en cada etapa de la cadena logística.

---

## 📐 Arquitectura de la solución

```
AppSheet (app móvil)
    │
    ├── Inspección de PRECARGA  ──► Google Sheets (base de datos)
    │        └── Foto + firma digital por unidad                │
    │                                                            ▼
    └── Inspección de DESCARGA  ──────────────────► Cruce automático por VIN
                                                            │
                                                            ▼
                                                    Power BI Dashboard
                                                    (Heatmap + KPIs + Reincidencia)
```

### Flujo operativo

1. **Precarga**: El inspector registra desde la app el estado de cada vehículo al subirlo al camión — parte dañada, tipo de daño, cuadrante, foto y firma digital.
2. **Tránsito**: El registro fotográfico documenta cómo fue cargado cada camión.
3. **Descarga en destino**: Se registran las averías encontradas al bajar las unidades.
4. **Cruce automático**: Google Sheets cruza ambos registros por `ID_VIN` para determinar si el daño preexistía o se generó en tránsito.
5. **Análisis**: Power BI consume los datos y genera dashboards con mapas de calor por zona del vehículo, reincidencia por camión y distribución por tipo de daño.

---

## 📊 Modelo de datos

| Tabla | Descripción | Registros (prod.) |
|-------|-------------|:-----------------:|
| `Unidades` | Cada vehículo con VIN, modelo, equipo (camión), destino y fecha | ~2.200 |
| `Inspecciones` | Cada daño registrado: parte, tipo, cuadrante, foto, firma inspector | ~3.200 |
| `Flujos` | Catálogo de distribuidores y países destino | 8 |
| `Parte` | Catálogo de 70 partes del vehículo | 70 |
| `Tipo daño` | 14 categorías de daño | 14 |
| `Listado total Precarga` | Vista consolidada con VLOOKUP cruzando Unidades + Inspecciones | calculada |

---

## 📈 Dashboards Power BI

El dashboard principal muestra una **silueta vectorial del vehículo** con colores de intensidad proporcionales a la cantidad de daños por zona. Permite identificar de un vistazo qué parte concentra más averías.

**Métricas incluidas:**
- Daños por posición de carga (A1–B4)
- Unidades dañadas por equipo/camión
- Distribución porcentual por parte del vehículo
- Reincidencia por número de flota

---

## 🎯 Impacto medido

| Indicador | Resultado |
|-----------|:---------:|
| Reducción de averías por errores de carga | **−40%** |
| Tiempo para responder reclamo de dealer | días → horas |
| Registros en papel reemplazados | 100% digitalizado |
| Evidencia fotográfica por unidad | ✅ |

> La reducción del 40% se logró identificando, mediante análisis de reincidencia en el dashboard, que ciertos camiones y posiciones de carga concentraban sistemáticamente los daños.

---

## 📁 Estructura del repositorio

```
📦 trazabilidad-toyota/
├── 📊 data/
│   └── trazabilidad_toyota_MUESTRA.xlsx   ← modelo de datos anonimizado
├── 📸 screenshots/
│   ├── heatmap_dashboard.png
│   └── app_precarga.png
└── README.md
```

---

## 🛠️ Stack tecnológico

| Herramienta | Uso |
|-------------|-----|
| **AppSheet** | App móvil para registro de inspecciones en campo |
| **Google Sheets** | Base de datos relacional + lógica de cruce (VLOOKUP, FILTER) |
| **Google Apps Script** | Automatización de notificaciones y validaciones |
| **Power BI (DAX)** | Dashboards con heatmap vectorial, KPIs y reincidencia |
| **Power Automate** | Flujos de alerta ante umbrales de daños |

---

## 👤 Autor

**Tomás Pozo Isaurralde** — Data Analyst | BI | Process Automation  
[LinkedIn](https://www.linkedin.com/in/pozotomas) · tomaspozoisaurralde@gmail.com · Campana, Argentina
