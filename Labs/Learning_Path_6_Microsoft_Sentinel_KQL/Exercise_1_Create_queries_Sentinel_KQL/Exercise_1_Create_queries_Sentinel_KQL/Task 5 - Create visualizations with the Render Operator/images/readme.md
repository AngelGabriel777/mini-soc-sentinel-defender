# Taller 5 – Creación de Visualizaciones en KQL con el Operador Render

## Objetivo
Generar visualizaciones gráficas utilizando el operador `render` en Microsoft Sentinel para analizar eventos de seguridad disponibles en el entorno.

---

## 5.1 Visualización de Inicios de Sesión por Usuario (Gráfico de Barras)

### Consulta

```kql
SigninLogs
| where TimeGenerated > ago(7d)
| summarize TotalInicios = count() by UserPrincipalName
| order by TotalInicios desc
| render barchart
```

### Descripción

Esta consulta:

* Filtra los eventos de inicio de sesión de los últimos 7 días.
* Cuenta la cantidad de inicios de sesión por usuario.
* Genera un gráfico de barras para visualizar qué cuentas presentan mayor actividad.

### Análisis

Permite identificar:

* Usuarios con alta frecuencia de autenticación.
* Posibles cuentas con actividad inusual.
* Concentración de accesos en determinados usuarios.

---

## 5.2 Visualización Temporal de Inicios de Sesión (Serie de Tiempo)

### Consulta

```kql
SigninLogs
| where TimeGenerated > ago(7d)
| summarize TotalInicios = count() by bin(TimeGenerated, 1h)
| render timechart
```

### Descripción

Esta consulta:

* Agrupa los eventos en intervalos de 1 hora.
* Cuenta el número de inicios de sesión por intervalo.
* Genera una gráfica de línea mostrando la evolución temporal.

### Análisis

Permite:

* Detectar picos de actividad.
* Identificar horarios de mayor autenticación.
* Observar comportamientos anómalos en el tiempo.

---

## 5.3 Visualización de Incidentes por Severidad (Nivel Avanzado)

### Consulta

```kql
SecurityIncident
| summarize TotalIncidentes = count() by Severity
| render piechart
```

### Descripción

Agrupa los incidentes registrados según su nivel de severidad y los muestra en un gráfico circular.

### Nota

Si no se muestran resultados, significa que no existen incidentes generados en el entorno.

---

## Resultados Esperados

* Ejecución correcta de las consultas con el operador `render`.
* Obtención de gráficos de barras, líneas y circulares según los datos disponibles.
* Interpretación de los patrones visualizados para identificar comportamientos relevantes en materia de seguridad.

---

## Conclusión

Este taller permitió aplicar el operador `render` en KQL para transformar datos tabulares en representaciones gráficas que facilitan el análisis de eventos de autenticación e incidentes de seguridad. El uso de visualizaciones adecuadas mejora la capacidad de detección de anomalías y la comprensión del panorama de seguridad en Microsoft Sentinel.
