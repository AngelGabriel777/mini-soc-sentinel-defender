# TALLER 3  
# Ejecución de Sentencias KQL Básicas en Microsoft Sentinel (Interfaz 2026)

---

## 1. Objetivo

Ejecutar sentencias básicas del lenguaje Kusto Query Language (KQL) dentro del workspace **law-sentinel-lab**, aplicando operadores fundamentales como `search`, `where`, `in` y `let` para filtrar, analizar y estructurar datos almacenados en la tabla **SigninLogs**.

---

## 2. Entorno de Trabajo

- Plataforma: Microsoft Azure Portal  
- Servicio: Microsoft Sentinel  
- Workspace: law-sentinel-lab  
- Área utilizada: Records (Log Analytics)  
- Tabla principal utilizada: SigninLogs  
- Intervalo de tiempo configurado: Últimas 24 horas  

---

## 3. Consideraciones Previas

- Antes de ejecutar cada consulta, borrar la anterior o abrir una nueva pestaña de consulta seleccionando el botón **+** (máximo 25 pestañas).
- Verificar que el editor esté configurado en **Modo KQL**.
- Confirmar que el intervalo de tiempo esté establecido en **Últimas 24 horas**, salvo cuando la consulta filtre explícitamente por `TimeGenerated`.

---

## 4. Desarrollo de Consultas

---

### 4.1 Uso del operador `search`

El operador `search` permite buscar un término en todas las columnas disponibles.

```kql
search "Microsoft"
```

**Descripción técnica:**
Busca la palabra *Microsoft* en todas las tablas y columnas disponibles dentro del workspace.
Este operador es menos eficiente que filtrar por tabla específica.

---

### 4.2 Uso de `search` en tabla específica

```kql
search in (SigninLogs) "Microsoft"
```

**Descripción técnica:**
Limita la búsqueda únicamente a la tabla **SigninLogs**, optimizando el rendimiento.

---

### 4.3 Uso del operador `where` (filtro por tiempo)

```kql
SigninLogs
| where TimeGenerated > ago(7d)
```

**Descripción técnica:**
Filtra los registros generados en los últimos 7 días utilizando la columna `TimeGenerated`.

---

### 4.4 Uso de `where` con condición adicional

```kql
SigninLogs
| where TimeGenerated > ago(7d) and ResultType == 0
```

**Descripción técnica:**
Filtra los inicios de sesión exitosos (`ResultType == 0`) ocurridos en los últimos 7 días.

---

### 4.5 Uso de múltiples cláusulas `where`

```kql
SigninLogs
| where TimeGenerated > ago(7d)
| where ResultType == 0
| where AppDisplayName =~ "Microsoft"
```

**Descripción técnica:**
Aplica filtros progresivos:

* Últimos 7 días
* Inicios de sesión exitosos
* Aplicaciones cuyo nombre contenga "Microsoft"

---

### 4.6 Uso del operador `in`

```kql
SigninLogs
| where TimeGenerated > ago(7d)
| where ResultType in (0, 50053)
```

**Descripción técnica:**
Filtra múltiples valores en una sola condición.
Permite evaluar más de un código de resultado simultáneamente.

---

### 4.7 Uso de `let` para declarar variables

```kql
let timeOffset = 10m;
SigninLogs
| where TimeGenerated > ago(timeOffset * 6)
```

**Descripción técnica:**
Declara una variable (`timeOffset`) para reutilizarla dentro de la consulta, facilitando la modificación de parámetros.

---

### 4.8 Uso de `let` con lista dinámica

```kql
let suspiciousApps = datatable(app:string)
[
  "Microsoft Teams",
  "Azure Portal"
];
SigninLogs
| where AppDisplayName in (suspiciousApps)
```

**Descripción técnica:**
Define una lista dinámica de aplicaciones y filtra registros que coincidan con dichos valores.

---

### 4.9 Uso de `let` con tabla dinámica

```kql
let LowActivityUsers =
    SigninLogs
    | summarize cnt = count() by UserPrincipalName
    | where cnt < 5;
LowActivityUsers
```

**Descripción técnica:**

1. Resume la cantidad de eventos por usuario.
2. Filtra usuarios con menos de 5 eventos.
3. Devuelve una tabla dinámica con los resultados.

---

## 5. Resultados Esperados

* Visualización de datos filtrados correctamente.
* Aplicación adecuada de operadores básicos de KQL.
* Comprensión del filtrado por tiempo, condiciones y listas.
* Uso correcto de variables y estructuras dinámicas.

---

## 6. Conclusión

Durante el desarrollo del Taller 3 se ejecutaron sentencias básicas en lenguaje KQL dentro del workspace **law-sentinel-lab** en Microsoft Sentinel.

Se aplicaron operadores fundamentales como `search`, `where`, `in` y `let`, permitiendo filtrar, organizar y analizar registros de inicio de sesión almacenados en la tabla **SigninLogs**.

La práctica fortaleció la comprensión del funcionamiento del motor de consultas KQL y su aplicación en escenarios reales de análisis de registros de seguridad.
