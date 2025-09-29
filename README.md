# Practica2
# Vehicle Sales Management System (Prolog)

**ST0244 – Programming Languages · Practice II (EAFIT)**
Lecturer: Alexander Narváez Berrío
Value: 12% of the final grade
Date: March 2025

---

## 📌 Descripción

Sistema en **Prolog** para consultar y analizar el inventario de una concesionaria, con **filtros por atributos clave**, generación de **reportes estructurados** usando `findall/3` y `bagof/3`, y **restricciones** para asegurar que el valor total no exceda un límite dado **priorizando vehículos más económicos**.

---

## 🎯 Objetivos cubiertos

* Catálogo de vehículos con: **brand**, **reference**, **type**, **price**, **year**.
* Filtros por **tipo** y **presupuesto** (`meet_budget/2`).
* Listados y agrupaciones por **marca** y por **tipo+año** usando `findall/3` y `bagof/3`.
* Reporte `generate_report/4` que respeta un **límite global** y **prioriza precios bajos** (estrategia **greedy** en orden ascendente de precio).

---

## 🗂️ Estructura del repositorio

```
.
├── src/
│   └── vehicles.pl           % Implementación principal (módulo `vehicles`)
├── README.md                 % Este documento
└── media/
    └── screenshots/          % (Opcional) Capturas de consultas en SWI‑Prolog
```

> Si prefieres una raíz más simple, puedes colocar `vehicles.pl` en la raíz del repo.

---

## 🧰 Requisitos

* **SWI‑Prolog** (recomendado ≥ 8.x)
  Descarga: [https://www.swi-prolog.org/Download.html](https://www.swi-prolog.org/Download.html)

---

## ▶️ Cómo ejecutar

1. Clona el repositorio y abre **SWI‑Prolog** en la carpeta del proyecto.
2. Carga el archivo:

   ```prolog
   ?- [src/vehicles].   % o [vehicles]. si está en la raíz
   ```
3. Ejecuta los **casos de prueba** o consultas ejemplo (ver abajo).

> Tip: para recargar cambios usa `make/0` en SWI‑Prolog.

---

## 📚 Predicados disponibles

### Hechos (catálogo)

```prolog
vehicle(Brand, Reference, Type, PriceUSD, Year).
% Type ∈ {sedan, suv, pickup, sport}
```

### Filtros y listados

```prolog
meet_budget(+Reference, +BudgetMax).
% Verdadero si la referencia tiene Price =< BudgetMax.

vehicles_by_brand(+Brand, -Refs).
% Lista (findall/3) de referencias por marca.

brand_grouped_by_type_year(+Brand, -Groups).
% Agrupa con bagof/3 por (Type,Year) y devuelve Groups como
% una lista de términos Type-Year-Refs (sin duplicados, ordenada).
```

### Reporte principal

```prolog
generate_report(+Brand, +Type, +Budget, -Result).
% Result = result(List, Total)
% - Filtra por Brand/Type y por Price =< Budget (por vehículo).
% - Ordena por precio ascendente.
% - Selecciona greedy sin exceder min(Budget, inventory_limit).
% - List es [ref(Reference, Price, Year), ...].
```

### Parámetro de negocio

```prolog
inventory_limit(1000000).
% Límite global del inventario (ajústalo si el enunciado fija otro valor).
```

### Casos de prueba (helpers)

```prolog
case1_toyota_suv_under_30k(-L).
case2_ford_grouped(-G).
case3_total_sedan_under_500k(-Result).
```

---

## 🧪 Casos de prueba (según el enunciado)

### Caso 1 – Toyota SUV < $30,000

```prolog
?- case1_toyota_suv_under_30k(L).
L = [rav4].
```

> Basado en el catálogo provisto (`rav4` cuesta 28,000 USD).

### Caso 2 – Ford agrupado por tipo y año (bagof/3)

```prolog
?- case2_ford_grouped(G).
G = [
  pickup-2021-[ranger],
  sedan-2020-[fusion],
  sedan-2021-[focus],
  sport-2023-[mustang],
  suv-2022-[escape],
  suv-2023-[bronco]
].
```

### Caso 3 – Total para sedanes sin exceder $500,000

```prolog
?- case3_total_sedan_under_500k(result(List, Total)).
List = [
  ref(onix,    18000, 2022),
  ref(sentra,  20000, 2022),
  ref(focus,   21000, 2021),
  ref(corolla, 22000, 2022),
  ref(civic,   23000, 2022),
  ref(fusion,  26000, 2020),
  ref(camry,   28000, 2023),
  ref(accord,  29000, 2023),
  ref(series3, 43000, 2022)
],
Total = 230000.
```

> Se priorizan los más baratos (orden ascendente) y el total queda ≤ 500,000.

---

## 🔎 Ejemplos adicionales

```prolog
?- meet_budget(rav4, 30000).
true.

?- vehicles_by_brand(toyota, L).
L = [corolla, camry, rav4, hilux].

?- generate_report(bmw, suv, 120000, R).
R = result([ref(x3,47000,2021), ref(x5,60000,2021)], 107000).
```

---

## 🧠 Decisiones de diseño

* **Greedy por precio ascendente**: cumple la política de “si se excede el límite, priorizar más baratos”.
* **`bagof/3` + `setof/3`**: se agrupa por `(Type, Year)` y se devuelven grupos ordenados y sin duplicados.
* **Módulo `vehicles`**: expone solo lo necesario y deja claro el namespace.

---

## 🛠️ Cómo extender

* **Agregar más vehículos**: añade nuevos hechos `vehicle/5` manteniendo tipos válidos.
* **Cambiar el límite**: ajusta `inventory_limit/1`.
* **Nuevos reportes**: crea predicados que llamen a `findall/3`/`bagof/3`/`generate_report/4` con otros criterios (p. ej., por rango de años, por tope de precio acumulado diferente, etc.).

---

## 📸 Entrega sugerida

Incluye en el repo una carpeta `media/screenshots/` con capturas de:

1. Carga del archivo (`[src/vehicles].`).
2. Ejecución de los tres casos de prueba.
3. 1–2 consultas adicionales (p. ej. `vehicles_by_brand/2` y `generate_report/4`).

Además, sube un **video (≤ 5 min)** demostrando:

* Catálogo y predicados claves (30–60s).
* Ejecución de casos y explicación del límite/greedy (2–3 min).
* Cierre y conclusiones (30–40s).

---

## 👥 Autores

* **Estudiante 1** – *código, pruebas, video*
* **Estudiante 2** – *documentación, pruebas, video*

> Reemplaza con nombres completos y correos institucionales.

---

## 📄 Licencia

Este proyecto se entrega con fines académicos para la práctica ST0244. Puedes usar **MIT** si deseas publicar el repositorio abiertamente.
