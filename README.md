# Practica2
# Vehicle Sales Management System (Prolog) – ST0244

**Curso:** ST0244 – Programming Languages · Practice II (EAFIT)
**Docente:** Alexander Narváez Berrío
**Valor:** 12% de la nota final
**Fecha:** Marzo de 2025
**Autor:** Jose David Acevedo

---

## Descripción del proyecto

Este proyecto implementa en Prolog un sistema para consultar y analizar el inventario de vehículos de una concesionaria. El modelo de datos se basa en hechos del tipo `vehicle(Brand, Reference, Type, Price, Year)` y se complementa con predicados para filtrar por presupuesto y tipo, listar por marca, agrupar por (tipo, año) y generar reportes que respetan un límite total de inventario priorizando vehículos de menor precio.

---

## Objetivos del enunciado abordados

1. Gestionar un catálogo de vehículos filtrando por atributos clave (marca, tipo, precio, año).
2. Generar listados/agrupaciones estructuradas utilizando `findall/3` y `bagof/3`.
3. Aplicar restricciones para garantizar consistencia en los resultados (p. ej., presupuesto máximo y límite global del reporte).

---

## Contenido del proyecto

* **Catálogo**: hechos `vehicle/5` con al menos 10 vehículos de distintas marcas y tipos válidos (Sedan, SUV, Pickup, Sport).
* **Filtros básicos**:

  * `meet_budget(Reference, BudgetMax)`: verdadero si el precio de la referencia no supera el presupuesto.
  * `vehicles_by_brand(Brand, Refs)`: lista de referencias por marca usando `findall/3`.
* **Agrupación por marca**:

  * `brand_grouped_by_type_year(Brand, Groups)`: agrupación de referencias por (tipo, año) usando `bagof/3`.
* **Reporte con límite**:

  * `generate_report(Brand, Type, Budget, Result)`: devuelve `Result = result(List, Total)` con vehículos de la marca y tipo que no superan el presupuesto, ordenados por precio ascendente y seleccionados sin exceder el límite global del inventario.

---

## Casos de prueba solicitados (resumen)

1. Listar todas las referencias **Toyota** de tipo **SUV** con precio menor a **$30,000**.
2. Mostrar los vehículos de la marca **Ford** usando `bagof/3`, **agrupados por tipo y año**.
3. Calcular el **valor total** de un inventario filtrado por tipo **Sedan** sin superar **$500,000**.

---

## Integrante

* Jose David Acevedo

---

## Nota sobre ejecución

El código puede ejecutarse en **SWI‑Prolog** (escritorio) o en **SWISH** (IDE web) realizando consultas sobre los predicados anteriores. Las capturas de pantalla de consultas y resultados deben incluirse en la entrega según se solicita en el enunciado.

---

## 📄 Licencia

Este proyecto se entrega con fines académicos para la práctica ST0244. Puedes usar **MIT** si deseas publicar el repositorio abiertamente.
