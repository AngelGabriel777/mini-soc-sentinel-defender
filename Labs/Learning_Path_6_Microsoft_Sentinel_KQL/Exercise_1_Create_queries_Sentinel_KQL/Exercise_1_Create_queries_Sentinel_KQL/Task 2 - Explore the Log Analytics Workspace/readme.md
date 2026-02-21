# TALLER 2  
# Exploración del Espacio de Trabajo de Análisis de Registros en Microsoft Sentinel (Interfaz 2026)

---

## 1. Objetivo

Explorar el espacio de trabajo de análisis de registros en Microsoft Sentinel, utilizando el editor en modo KQL para ejecutar consultas sobre datos reales del workspace y analizar los resultados obtenidos.

---

## 2. Entorno de Trabajo

- Plataforma: Microsoft Azure Portal  
- Servicio: Microsoft Sentinel  
- Workspace: defenderWorkspace  
- Área utilizada: Registros (Log Analytics)  
- Tabla utilizada: SigninLogs  

---

## 3. Procedimiento

---

### 3.1 Acceso al Servicio Microsoft Sentinel

1. En la barra de búsqueda del portal Azure escribir:

   Microsoft Sentinel

2. Seleccionar el servicio desde los resultados.
3. Elegir el espacio de trabajo:

   law-sentinel-lab
![Acceso al área de registros](images/img1.jpg)

---

### 3.2 Acceso al Área de Registros

1. En el menú de navegación izquierdo seleccionar:

   Registros

2. Cerrar la ventana emergente de Log Analytics si aparece.
3. Cerrar el Centro de consultas para trabajar directamente en el editor.


![Acceso al área de registros](images/img2.jpg)

---

### 3.3 Configuración del Editor en Modo KQL

1. Verificar que el editor esté configurado en:

   Modo KQL

2. Confirmar que el intervalo de tiempo esté establecido en:

   Últimas 24 horas

3. Confirmar que el filtro de resultados esté configurado en:

   Mostrar: 1000 resultados

![Acceso al área de registros](images/img3.jpg)
![Acceso al área de registros](images/img4.jpg)

---

### 3.4 Exploración del Esquema

1. En el panel izquierdo revisar la sección de tablas.
2. Identificar la tabla:

   SigninLogs

3. Expandir la tabla para observar las columnas disponibles.

Esta tabla contiene registros de intentos de inicio de sesión dentro del entorno.

---

### 3.5 Ejecución de Consulta

En el editor de consultas ingresar la siguiente instrucción:

```kql
SigninLogs
| take 10
