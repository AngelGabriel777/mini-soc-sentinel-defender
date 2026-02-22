# Task 4: Análisis de Resultados en KQL con el Operador `summarize`
## Microsoft Sentinel / Log Analytics – Laboratorio Práctico

---

## Objetivo

En este laboratorio se analiza información de autenticación y seguridad utilizando el operador **`summarize` en KQL (Kusto Query Language)**.

Se aprenderá a:

- Agrupar datos
- Contar eventos
- Calcular valores distintos
- Detectar comportamientos sospechosos
- Obtener registros más recientes y más antiguos
- Generar listas y conjuntos
- Comprender la importancia del orden del pipe (`|`)

---

## Entorno de Trabajo

### Plataforma
- Microsoft Azure
- Log Analytics Workspace
- Microsoft Sentinel

### Tablas Disponibles

#### LogManagement
- `SigninLogs`
- `AADManagedIdentitySignInLogs`
- `AADNonInteractiveUserSignInLogs`
- `AuditLogs`
- `MicrosoftGraphActivityLogs`
- `Usage`

#### Microsoft Sentinel
- `SecurityAlert`
- `SecurityIncident`

> **Nota:** En este entorno se utilizan tablas nativas de Azure. La tabla principal utilizada será `SigninLogs`.

---

## Paso 1 – Contar Inicios de Sesión por Aplicación

### Objetivo
Determinar cuántos eventos de autenticación ocurrieron por aplicación en los últimos 7 días.

### Consulta

```kql
SigninLogs
| where TimeGenerated > ago(7d)
| summarize count() by AppDisplayName
```

### Explicación

- `where TimeGenerated > ago(7d)` → Filtra los últimos 7 días.
- `summarize count()` → Cuenta los eventos.
- `by AppDisplayName` → Agrupa por aplicación.

### Análisis de Seguridad

Permite identificar:

- Aplicaciones más utilizadas.
- Picos inusuales de autenticación.
- Posible actividad anómala.

---

## Paso 2 – Conteo por Tipo de Cliente y Aplicación

### Objetivo
Analizar desde qué tipo de cliente se realizan las autenticaciones.

### Consulta

```kql
SigninLogs
| where TimeGenerated > ago(7d)
| summarize cnt = count() by ClientAppUsed, AppDisplayName
```

### Explicación

- `ClientAppUsed` → Indica el tipo de cliente (Browser, Mobile, etc.).
- `cnt = count()` → Renombra la columna del conteo.

### Análisis de Seguridad

Permite detectar:

- Uso inesperado de autenticación heredada.
- Métodos de acceso sospechosos.
- Comportamientos fuera de la línea base.

---

## Paso 3 – Conteo de Direcciones IP Distintas

### Objetivo
Identificar cuántas direcciones IP diferentes realizaron autenticaciones.

### Consulta

```kql
SigninLogs
| where TimeGenerated > ago(7d)
| summarize dcount(IPAddress)
```

### Explicación

- `dcount()` → Cuenta valores distintos (aproximado).
- `IPAddress` → Dirección IP del inicio de sesión.

### Análisis de Seguridad

Un número elevado puede indicar:

- Cuenta comprometida.
- Uso compartido de credenciales.
- Intentos distribuidos de acceso.

---

## Paso 4 – Detección de Intentos con Cuenta Deshabilitada

### Objetivo
Detectar intentos de autenticación con cuentas deshabilitadas.

### Consulta

```kql
let timeframe = 30d;
let threshold = 1;
SigninLogs
| where TimeGenerated >= ago(timeframe)
| where ResultDescription has "disabled"
| summarize applicationCount = dcount(AppDisplayName) by UserPrincipalName, IPAddress
| where applicationCount >= threshold
```

### Explicación

- `let` → Define variables reutilizables.
- Filtra eventos donde la cuenta está deshabilitada.
- Cuenta aplicaciones distintas involucradas.
- Aplica un umbral mínimo.

### Análisis de Seguridad

Puede indicar:

- Ataque automatizado.
- Uso indebido de cuenta antigua.
- Error de configuración.

---

## Paso 5 – Obtener el Evento Más Reciente (`arg_max`)

### Objetivo
Obtener el inicio de sesión más reciente por usuario.

### Consulta

```kql
SigninLogs
| summarize arg_max(TimeGenerated, *) by UserPrincipalName
```

### Explicación

- `arg_max(TimeGenerated, *)` devuelve la fila completa con el tiempo más reciente para cada usuario.

### Análisis de Seguridad

Útil para:

- Respuesta ante incidentes.
- Reconstrucción de línea de tiempo.
- Verificación de última actividad.

---

## Paso 6 – Obtener el Evento Más Antiguo (`arg_min`)

### Objetivo
Identificar el primer evento registrado por usuario.

### Consulta

```kql
SigninLogs
| summarize arg_min(TimeGenerated, *) by UserPrincipalName
```

### Explicación

- Devuelve el evento más antiguo registrado para cada usuario.

### Análisis de Seguridad

Permite:

- Análisis histórico.
- Establecer comportamiento base.

---

## Paso 7 – Importancia del Orden del Pipe (`|`)

### Consulta 1

```kql
SigninLogs
| summarize arg_max(TimeGenerated, *) by UserPrincipalName
| where ResultType == 0
```

**Significado**:

1. Obtiene el último evento por usuario.
2. Luego filtra si fue exitoso.

👉 Muestra usuarios cuya **última actividad fue un inicio de sesión exitoso**.

---

### Consulta 2

```kql
SigninLogs
| where ResultType == 0
| summarize arg_max(TimeGenerated, *) by UserPrincipalName
```

**Significado**:

1. Primero filtra inicios exitosos.
2. Luego obtiene el más reciente.

👉 Muestra el **último inicio de sesión exitoso** por usuario.

### Diferencia Clave

| Consulta 1 | Consulta 2 |
|------------|------------|
| Último evento fue exitoso | Último inicio exitoso |
| Más restrictiva | Más amplia |

---

## Paso 8 – Uso de `make_list()`

### Objetivo
Generar una lista completa de aplicaciones utilizadas por usuario.

### Consulta

```kql
SigninLogs
| where TimeGenerated > ago(7d)
| summarize make_list(AppDisplayName) by UserPrincipalName
```

### Explicación

- Devuelve un arreglo JSON con todos los valores (incluyendo duplicados).

**Ejemplo de Resultado**:
```json
["Azure Portal","Azure Portal","Teams","Outlook"]
```

---

## Paso 9 – Uso de `make_set()`

### Objetivo
Generar lista única de aplicaciones por usuario.

### Consulta

```kql
SigninLogs
| where TimeGenerated > ago(7d)
| summarize make_set(AppDisplayName) by UserPrincipalName
```

### Explicación

- Elimina duplicados; devuelve solo valores distintos.

**Ejemplo de Resultado**:
```json
["Azure Portal","Teams","Outlook"]
```

---

## Paso 10 – Análisis de Alertas de Seguridad

### Consulta

```kql
SecurityAlert
| summarize count() by Severity
```

### Objetivo
Analizar la distribución de alertas según su nivel de severidad.

---

## Conceptos Aprendidos

- `summarize`
- `count()`
- `dcount()`
- `arg_max()`
- `arg_min()`
- `make_list()`
- `make_set()`
- Importancia del orden del pipe (`|`)

---

## Conclusión

El operador `summarize` permite:

- Agregación eficiente de datos.
- Análisis de comportamiento.
- Construcción de lógica de detección.
- Optimización del monitoreo en Microsoft Sentinel.

Dominar estas funciones es fundamental para:

- Analistas SOC.
- Analistas de Microsoft Sentinel.
- Preparación para la certificación SC-200.
- Actividades de Threat Hunting.

