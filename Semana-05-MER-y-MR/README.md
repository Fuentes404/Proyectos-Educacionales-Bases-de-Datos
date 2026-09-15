# Transformando el MER en Modelo Relacional — Retail Solari S.A.

Este proyecto corresponde a la actividad sumativa **"Transformando el MER en Modelo Relacional"** de la asignatura
**Modelamiento de Bases de Datos**. Consiste en tomar un Modelo Entidad-Relación (MER) conceptual, incompleto y sin
normalizar, y transformarlo en un **Modelo Entidad-Relación Extendido (MER-E) Normalizado**, para luego derivar el
**Modelo Relacional (MR) Normalizado** y su correspondiente **script DDL** en Oracle SQL.

## 📋 Descripción

- Análisis del caso de negocio de **Solari S.A.**, empresa retail con presencia en Chile.
- Identificación de entidades, supertipos y subtipos, con sus atributos identificadores, obligatorios y opcionales.
- Identificación de las relaciones entre entidades y sus cardinalidades.
- Normalización de los datos aplicando las tres primeras formas normales (1FN, 2FN, 3FN).
- Transformación del MER-E Normalizado en **Modelo Relacional (MR)**, generando tablas, claves primarias (PK),
  claves foráneas (FK) y restricciones.
- Generación del **script DDL** para la creación física de las tablas en Oracle Database.
- Modelado realizado con **Oracle SQL Developer Data Modeler**.

## 🏢 Contexto del caso: Retail Solari S.A.

Solari S.A. es una empresa dedicada a la comercialización de productos de consumo esencial. Con seis años de
presencia en Chile, ofrece una amplia variedad de productos —agrupados en distintas marcas, modelos y
categorías— adquiridos a múltiples proveedores y distribuidos a través de sucursales ubicadas en distintas
comunas del país.

Anteriormente se intentó construir una base de datos relacional para la operación de la empresa, pero el proyecto
quedó inconcluso: solo existe un **modelo conceptual incompleto, sin normalizar y con omisiones lógicas**, que es
el punto de partida de este trabajo.

### Reglas de negocio

- La empresa tiene sucursales en 50 de las 346 comunas del país; una comuna puede tener más de una sucursal.
- Cada sucursal transfiere su identificación a los productos que vende.
- Existen del orden de 9.000 productos distintos a la venta.
- Los proveedores pueden ser **empresas** o **personas naturales**, por lo que se utiliza un discriminador
  (subtipos) para registrar el tipo de proveedor. Algunas empresas proveedoras tienen sitio web.
- Cada modelo de producto pertenece a una única marca; existen del orden de 1.000 marcas distintas y cientos de
  modelos por marca, por lo que el identificador de `MODELO` es compuesto (id_modelo + id_marca).
- Existen categorías de productos que no vencen, y no todas las categorías tienen productos en bodega.
- Cada venta genera una boleta que registra cantidad de productos, fecha de venta y monto total.

## 🧩 Modelos y notaciones utilizadas

| Modelo | Notación | Herramienta |
|--------|----------|-------------|
| Modelo Entidad-Relación Extendido (MER-E) Normalizado | **Barker** (crow's foot, `#` identificador, `*` obligatorio, `o` opcional) | Oracle SQL Developer Data Modeler |
| Modelo lógico con tipos de dato | **Barker** extendido (atributos + tipos de dato, `P`/`PF`/`F`) | Oracle SQL Developer Data Modeler |
| Modelo Relacional (MR) Normalizado | **Bachman / Ingeniería de la Información** (tablas físicas, PK y FK nombradas explícitamente) | Oracle SQL Developer Data Modeler |
| Script de creación de tablas | DDL Oracle SQL | Oracle Database 11g |

## 🗺️ Modelo Entidad-Relación Extendido (MER-E) Normalizado — Notación Barker

Vista conceptual con entidades, atributos identificadores (`#`), obligatorios (`*`) y opcionales (`o`), y las
relaciones representadas con simbología *crow's foot* (uno, muchos, opcional/obligatorio) propia de la notación
Barker.

<img width="1904" height="731" alt="mer-e-notacion-barker" src="https://github.com/user-attachments/assets/c03147ed-a4cf-4ef4-b5b4-1b637f7483fe" />


*Entidades principales:* `PRODUCTO`, `PRODUCTO_PROVEEDOR`, `PROVEEDOR` (con subtipos `PROVEEDOR_PERSONA` y
`PROVEEDOR_EMPRESA`), `CATEGORIA`, `MODELO`, `MARCA`, `SUCURSAL`, `COMUNA`, `REGION`, `CLIENTE`, `BOLETA_VENTA` y
`DETALLE_BOLETA` (entidad asociativa que resuelve la relación muchos a muchos entre `PRODUCTO` y `BOLETA_VENTA`).

## 🧱 Modelo lógico Barker con tipos de dato

Vista intermedia que conserva la notación Barker (marcas `P` para clave primaria, `PF` para clave primaria/foránea
y `F` para clave foránea) pero ya incorpora los **tipos de dato** definidos para cada atributo tras el análisis de
dominio, como paso previo a la generación del modelo físico.

<img width="1902" height="733" alt="modelo-logico-barker-tipos-datos" src="https://github.com/user-attachments/assets/e956d9b5-7c1b-44b3-872a-3aa8fe04d6f5" />

## 🔗 Modelo Relacional (MR) Normalizado — Notación Bachman / Ingeniería de la Información

Modelo físico resultante de aplicar las reglas de transformación de MER-E a MR: cada entidad se convierte en
tabla, se definen las claves primarias (`PK`) y foráneas (`FK`) de forma explícita y nombrada, y se representan
las relaciones entre tablas mediante la notación de Bachman / Ingeniería de la Información.

<img width="1903" height="721" alt="modelo-relacional-mr-bachman" src="https://github.com/user-attachments/assets/dce5f9f6-877a-4b0e-8563-62dee640aa2e" />

*Tablas generadas:* `BOLETA_VENTA`, `CATEGORIA`, `CLIENTE`, `COMUNA`, `DETALLE_BOLETA`, `MARCA`, `MODELO`,
`PRODUCTO`, `PRODUCTO_PROVEEDOR`, `PROVEEDOR`, `PROVEEDOR_EMPRESA`, `PROVEEDOR_PERSONA`, `REGION`, `SUCURSAL`
(14 tablas en total).

## 🛠️ Script DDL (extracto)

El script completo se genera con Oracle SQL Developer Data Modeler y se encuentra documentado en el archivo Word
del proyecto. Ejemplo de la creación de una tabla y su clave primaria:

```sql
CREATE TABLE PRODUCTO
(
  id_producto             NUMBER (6)      NOT NULL ,
  nombre                  VARCHAR2 (50)   NOT NULL ,
  descripcion             VARCHAR2 (50) ,
  fecha_vencimiento       DATE ,
  monto_venta             NUMBER (10,2)   NOT NULL ,
  MODELO_id_modelo        NUMBER (5)      NOT NULL ,
  MODELO_MARCA_id_marca   NUMBER (5)      NOT NULL ,
  CATEGORIA_id_categoria  NUMBER (3)      NOT NULL ,
  SUCURSAL_id_sucursal    NUMBER (3)      NOT NULL
);

ALTER TABLE PRODUCTO
  ADD CONSTRAINT PRODUCTO_PK PRIMARY KEY ( id_producto );

ALTER TABLE PRODUCTO
  ADD CONSTRAINT PRODUCTO_MODELO_FK FOREIGN KEY ( MODELO_id_modelo, MODELO_MARCA_id_marca )
  REFERENCES MODELO ( id_modelo, MARCA_id_marca );
```

> Resumen generado por Data Modeler: **14 CREATE TABLE**, **28 ALTER TABLE** (PK/FK/restricciones).

## 📂 Estructura del proyecto

```
proyecto/
├── README.md
├── images/
│   ├── mer-e-notacion-barker.png              # MER-E Normalizado (Barker)
│   ├── modelo-logico-barker-tipos-datos.png    # Modelo lógico Barker con tipos de dato
│   └── modelo-relacional-mr-bachman.png        # Modelo Relacional (MR) Normalizado (Bachman)
├── PRY2204_Exp2_S5_Claudio_Fuentes.docx        # Documento de respuesta con evidencias y contexto del caso
└── Modelo_Base/                                # Carpeta generada por Oracle Data Modeler (.dmd + recursos)
    └── Modelo_Base.dmd
```

## ▶️ Flujo de trabajo

1. Se analiza el MER conceptual incompleto entregado como punto de partida (entidades `PRODUCTO`, `PROVEEDOR`,
   `SUCURSAL`, `CLIENTE`, `COMUNA` y la entidad asociativa `BOLETA_VENTA`).
2. Se normalizan los datos aplicando 1FN, 2FN y 3FN, resolviendo dependencias parciales y transitivas.
3. Se construye el **MER-E Normalizado** en notación Barker con Oracle SQL Developer Data Modeler, incorporando
   supertipos/subtipos (`PROVEEDOR_PERSONA` / `PROVEEDOR_EMPRESA`), claves compuestas (`MODELO`) y entidades
   asociativas (`DETALLE_BOLETA`, `PRODUCTO_PROVEEDOR`).
4. Se aplican las reglas de transformación de MER-E a **Modelo Relacional (MR)**, generando tablas, PK, FK y
   restricciones de integridad.
5. Se genera el **script DDL** en Oracle SQL a partir del Modelo Relacional.
6. Se exportan las capturas del modelo, se guarda el diseño como archivo `.dmd` con su subcarpeta de recursos, y
   se comprime todo en un `.zip`/`.rar` junto con el documento Word.
7. El documento Word (con las capturas Barker y Bachman) se sube sin comprimir al repositorio de GitHub, y el
   enlace del repositorio se entrega junto con el documento en el aula virtual.

## 🧰 Herramientas utilizadas

- **Oracle SQL Developer Data Modeler** — modelado MER-E, MR y generación de DDL.
- **Oracle Database 11g** — motor de base de datos objetivo del script DDL.
- **GitHub** — repositorio de entrega del documento de evidencias.

## ✍️ Autor

Claudio Fuentes — Actividad Semana 5, asignatura Modelamiento de Bases de Datos.
