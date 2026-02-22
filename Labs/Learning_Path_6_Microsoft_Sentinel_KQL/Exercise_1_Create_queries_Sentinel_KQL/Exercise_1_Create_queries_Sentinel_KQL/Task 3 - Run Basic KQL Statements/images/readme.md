# Tarea 3 – Ejecución de Sentencias KQL Básicas  
## Microsoft Sentinel – Workspace: law-sentinel-lab

---

## Descripción

En esta tarea se ejecutan sentencias básicas del lenguaje **Kusto Query Language (KQL)** dentro de Microsoft Sentinel, con el objetivo de comprender el uso de operadores fundamentales para la exploración y análisis de registros.

Debido a que el entorno no contiene la tabla `SecurityEvent_CL` del laboratorio oficial, se utilizará la tabla **SigninLogs**, disponible en el workspace `law-sentinel-lab`.

---

## Objetivos

- Ejecutar consultas básicas en KQL.
- Utilizar el operador `search`.
- Aplicar filtros con `where`.
- Filtrar múltiples valores con `in`.
- Declarar variables con `let`.
- Crear listas dinámicas con `datatable`.
- Generar tablas resumidas con `summarize`.

---

## Entorno

- Plataforma: Microsoft Azure Portal  
- Servicio: Microsoft Sentinel  
- Workspace: `law-sentinel-lab`  
- Área: Logs / Records  
- Tabla principal utilizada: `SigninLogs`  
- Intervalo de tiempo: Últimas 24 horas (salvo que la consulta lo modifique)

---

## Consideraciones Importantes

- Antes de ejecutar cada consulta, borrar la anterior o abrir una nueva pestaña (+).
- Ejecutar cada bloque de código de manera individual.
- Verificar que el editor esté en **Modo KQL**.
- Confirmar el intervalo de tiempo antes de ejecutar consultas.

---

#  1. Uso del operador `search`

```kql
search "Microsoft"
```

![Acceso al área de registros](images/img1.jpg)

**Descripción:**
Busca la palabra "Microsoft" en todas las columnas de todas las tablas disponibles.

**Nota:**
El operador `search` sin especificar tabla es menos eficiente que el filtrado específico.

---

#  2. Búsqueda en tablas específicas

```kql
search in (SigninLogs, AuditLogs) "Microsoft"
```

**Descripción:**
Limita la búsqueda únicamente a las tablas `SigninLogs` y `AuditLogs`.

---

#  3. Filtro por tiempo con `where`

```kql
SigninLogs
| where TimeGenerated > ago(7d)
```

![Acceso al área de registros](images/img2.jpg)

**Descripción:**
Filtra registros generados en los últimos 7 días.

---

#  4. Filtro con condición adicional

```kql
SigninLogs
| where TimeGenerated > ago(7d)
| where ResultType == 0
```

![Acceso al área de registros](images/img3.jpg)


**Descripción:**
Filtra inicios de sesión exitosos (`ResultType == 0`) en los últimos 7 días.

---

#  5. Uso de múltiples condiciones `where`

```kql
SigninLogs
| where TimeGenerated > ago(7d)
| where ResultType == 0
| where UserPrincipalName contains "@"
```

**Descripción:**
Aplica múltiples filtros:

* Últimos 7 días
* Inicios exitosos
* Usuarios que contengan dominio

---

#  6. Uso del operador `in`

```kql
SigninLogs
| where TimeGenerated > ago(7d)
| where ResultType in (0, 50053)
```

**Descripción:**
Filtra múltiples códigos de resultado en una sola condición.

---

#  7. Uso de `let` para declarar variables

```kql
let timeOffset = 10m;
let discardResult = 50053;
SigninLogs
| where TimeGenerated > ago(timeOffset * 6)
| where ResultType != discardResult
```

**Descripción:**
Declara variables reutilizables dentro de la consulta.

![Acceso al área de registros](images/img6.jpg)

---

#  8. Lista dinámica con `datatable`

```kql
let suspiciousApps = datatable(app:string)
[
  "Microsoft Teams",
  "Azure Portal"
];
SigninLogs
| where TimeGenerated > ago(7d)
| where AppDisplayName in (suspiciousApps)
```

**Descripción:**
Crea una lista dinámica y filtra registros que coincidan con los valores definidos.

---

#  9. Tabla dinámica con `summarize`

```kql
let LowActivityUsers =
    SigninLogs
    | summarize total = count() by UserPrincipalName
    | where total < 5;
LowActivityUsers
```

**Descripción:**

1. Resume el número de eventos por usuario.
2. Filtra usuarios con baja actividad.
3. Devuelve una tabla dinámica con los resultados.

![Acceso al área de registros](images/img5.jpg)

---

##  Resultados Esperados

* Ejecución correcta de consultas KQL.
* Visualización de datos filtrados.
* Comprensión del uso de operadores básicos.
* Manipulación de datos mediante variables y tablas dinámicas.

---

##  Conclusión

Durante esta tarea se aplicaron sentencias básicas en KQL dentro del workspace `law-sentinel-lab` en Microsoft Sentinel.

Se utilizaron operadores fundamentales como `search`, `where`, `in` y `let`, así como estructuras dinámicas con `datatable` y `summarize`, demostrando comprensión práctica del lenguaje KQL para análisis de registros en entornos de seguridad.





