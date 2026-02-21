# TALLER 2  
# Exploración del Espacio de Trabajo de Análisis de Registros en Microsoft Sentinel (Interfaz 2026)

---

## 1. Objetivo

Explorar el espacio de trabajo de análisis de registros en Microsoft Sentinel, familiarizándose con la interfaz del editor KQL, el esquema de tablas disponibles y la ejecución de consultas básicas sobre datos de ejemplo.

---

## 2. Entorno de Trabajo

- Plataforma: Microsoft Azure Portal  
- Servicio: Microsoft Sentinel  
- Workspace: defenderWorkspace  
- Área utilizada: Registros (Log Analytics)

---

## 3. Procedimiento

---

### 3.1 Acceso al Servicio Microsoft Sentinel

1. En la barra de búsqueda del portal Azure escribir:

   Microsoft Sentinel

2. Seleccionar el servicio desde los resultados.
3. En la página principal, elegir el espacio de trabajo:

   defenderWorkspace

---

### 3.2 Acceso al Área de Registros

1. En el menú de navegación izquierdo expandir la sección:

   General

2. Seleccionar:

   Registros

3. Cerrar la ventana emergente de video de Log Analytics si aparece.
4. Cerrar el Centro de consultas para trabajar directamente en el editor.

---

### 3.3 Configuración del Editor en Modo KQL

1. Ubicar el selector de modo en la parte superior del editor.
2. Cambiar de:

   Modo simple → Modo KQL

Esto permite escribir consultas manualmente utilizando el lenguaje Kusto Query Language.

---

### 3.4 Exploración del Esquema y Panel de Filtros

1. En el panel izquierdo observar:

   - Esquema (Schema)
   - Tablas disponibles
   - Panel de filtros

2. Expandir diferentes tablas para visualizar:

   - Columnas
   - Tipos de datos
   - Herramientas adicionales disponibles

Esta exploración permite comprender la estructura de los datos almacenados en el workspace.

---

### 3.5 Ejecución de Consulta en Tabla Personalizada

En el editor de consultas, introducir la siguiente consulta:

```kql
SecurityEvent_CL
