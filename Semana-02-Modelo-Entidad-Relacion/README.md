# Modelo Entidad-Relación (MER) – Aerolínea BT&Airways

Este proyecto corresponde a la actividad formativa de la **Semana 2** de la asignatura
**Modelamiento de Bases de Datos** (Duoc UC), en la que se diseña el **Modelo
Entidad-Relación (MER)** para el sistema de venta y gestión de vuelos de la aerolínea
ficticia **BT&Airways**, dando continuidad al análisis realizado en la Semana 1.

El modelo fue desarrollado en **Oracle SQL Developer Data Modeler**, e incluye tanto la
vista lógica (notación **Barker**) como la vista relacional/física (notación **Bachman /
Ingeniería de la Información**) con sus tipos de datos.

**Integrantes:** Giovanni Mena – Claudio Fuentes
**Asignatura:** Modelamiento de Bases de Datos — **Carrera:** Analista Programador
**Profesor:** Eithel Gonzales

## 📋 Descripción

- Modelo conceptual de datos (MER) para el registro de venta de vuelos de BT&Airways.
- Identificación de entidades, atributos identificadores, atributos obligatorios (`*`) y
  opcionales (`°`).
- Identificación y tipificación de las relaciones entre entidades (cardinalidad y
  obligatoriedad en notación Barker).
- Definición de tipos de dato y dominio para cada atributo (`VARCHAR2`, `DATE`, `NUMBER`,
  `INTEGER`).
- Modelamiento de una jerarquía de supertipo/subtipo: `EMPLEADO` → `ADMINISTRATIVO` /
  `PILOTO`.
- Generación del modelo relacional con claves primarias (PK) y foráneas (FK) derivadas del
  modelo lógico.

## 🧩 Entidades del modelo

| Entidad | Atributos principales | Descripción |
|---|---|---|
| **PASAJERO** | numero_documento (PK), nombre_completo, fecha_nacimiento, nacionalidad, telefono°, correo° | Persona que compra pasajes y realiza reservas. |
| **RESERVA** | numero_reserva (PK), fecha_reserva, fecha_viaje, estado, asiento° | Reserva de un pasajero para un vuelo específico; puede quedar `confirmada` o `nula`. |
| **VUELO** | numero_vuelo (PK), fecha_despegue, fecha_llegada, hora_salida, ciudad_origen, ciudad_destino | Vuelo operado por un avión entre dos ciudades. |
| **AVION** | matricula (PK), marca, modelo, capacidad | Aeronave de la flota (25 aviones, capacidad de 200 a 800 asientos). |
| **EMPLEADO** | rut (PK), nombre_completo, direccion, sueldo_base, fecha_ingreso, genero, telefono_movil, telefono_contacto°, afp | Supertipo de trabajador de la compañía (450 empleados). |
| **ADMINISTRATIVO** | rut (PK/FK), cant_horas_extras | Subtipo de `EMPLEADO`; gestiona las reservas de los pasajeros. |
| **PILOTO** | rut (PK/FK), horas_vuelo_acumuladas | Subtipo de `EMPLEADO`. |
| **EQUIPAJE** | codigo (PK), color, peso, descripcion°, tipo_embarque, fragil° | Maleta asociada a un pasajero y registrada en una reserva. |

`*` obligatorio · `°` opcional · `#` identificador (PK)

## 🔗 Relaciones principales

| Relación | Cardinalidad | Descripción |
|---|---|---|
| PASAJERO — RESERVA | 1:N | Un pasajero puede tener muchas reservas; cada reserva pertenece a un único pasajero. |
| RESERVA — VUELO | N:1 | Una reserva está asociada a un único vuelo; un vuelo puede tener muchas reservas. |
| VUELO — AVION | N:1 | Cada vuelo está asociado a un solo avión; un avión puede volar muchas veces. |
| RESERVA — ADMINISTRATIVO | N:1 | Cada reserva es gestionada por un empleado administrativo. |
| PASAJERO — EQUIPAJE | 1:N | Un pasajero puede tener varias maletas (equipaje). |
| RESERVA — EQUIPAJE | N:M | Un equipaje puede estar asociado a varias reservas y una reserva puede incluir varias maletas. |
| EMPLEADO — ADMINISTRATIVO / PILOTO | 1:1 (supertipo/subtipo) | Un empleado es administrativo o piloto, no ambos. |

## 📂 Estructura del proyecto

```
BT_AirWays2/
├── BT_AirWays2.dmd              # Archivo principal del diseño (Data Modeler)
└── BT_AirWays2/
    ├── logical/                 # Modelo lógico (MER, notación Barker)
    │   ├── Logical.xml
    │   ├── entity/               # Definición de entidades
    │   ├── relation/             # Definición de relaciones
    │   ├── inheritance/          # Jerarquía EMPLEADO -> ADMINISTRATIVO / PILOTO
    │   └── subviews/             # Vista/diagrama lógico
    ├── datatypes/                # Dominios y tipos de datos usados en el modelo
    ├── rdbms/                    # Configuración del sitio RDBMS de destino
    ├── rel/                      # Modelo relacional (PK / FK)
    ├── mapping/                  # Mapeo entre modelo lógico y relacional
    ├── pm/                       # Process Model
    ├── businessinfo/             # Información / reglas de negocio del proyecto
    └── dl_settings.xml           # Configuración general del diseño
```

## ▶️ Cómo abrir el modelo

1. Descargar e instalar **Oracle SQL Developer Data Modeler**.
2. Abrir la aplicación y seleccionar **Archivo → Abrir**.
3. Ubicar y abrir el archivo `BT_AirWays2.dmd` (mantener la subcarpeta `BT_AirWays2/`
   junto al `.dmd`, en la misma ubicación).
4. En el panel de diseño:
   - Pestaña **Logical**: modelo entidad-relación en notación Barker.
   - Pestaña **Relational_1**: modelo relacional en notación Bachman / Ingeniería de la
     Información, con tipos de datos, PK y FK.

## 🖼️ Evidencias

Las capturas del modelo (MER en notación Barker y modelo relacional en notación Bachman)
se encuentran adjuntas en el documento
`AirWay_S2_Giovanni_M_Claudio_F.docx`, entregado junto con este repositorio.

## 📄 Licencia / Uso

Actividad formativa desarrollada para la asignatura Modelamiento de Bases de Datos,
Duoc UC. Uso exclusivamente académico.

