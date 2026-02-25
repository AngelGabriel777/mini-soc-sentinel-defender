## Task 2 – Deploy Microsoft Sentinel to a Workspace

---

## 📌 Objetivo

Implementar **Microsoft Sentinel** sobre el Log Analytics Workspace creado en el Task 1, con el fin de habilitar capacidades SIEM y SOAR en el entorno.

---

## 🏗 Prerrequisito

- Log Analytics Workspace previamente creado:
  - **Nombre:** defenderWorkspace  
  - **Resource Group:** Defender-RG  
  - **Estado:** Created  

---

## 🏠 Paso 1 – Regresar al Inicio del Portal

1. Una vez finalizada la implementación del workspace, seleccionar **Home** desde el menú de navegación superior del portal de Azure.

---

## 🔎 Paso 2 – Acceder a Microsoft Sentinel

1. En la sección **Azure services**, ubicar **Microsoft Sentinel**.  
2. Seleccionarlo para ingresar al servicio.

> También puede buscar “Microsoft Sentinel” en la barra superior de búsqueda.

---

## ➕ Paso 3 – Agregar Sentinel al Workspace

1. Seleccionar **+ Create**.  
2. Se mostrará la lista de Log Analytics Workspaces disponibles.  
3. Seleccionar el workspace:

   **defenderWorkspace**

4. Verificar la siguiente información:
   - Ubicación: westus  
   - Resource Group: defender-rg  
   - Suscripción: Azure subscription 1  

5. Hacer clic en **Add**.

![Habilitación de Microsoft Sentinel](images/img1.jpg)

---

## ⏳ Paso 4 – Esperar la Implementación

1. Esperar unos minutos mientras se completa el proceso de habilitación.  
2. Una vez finalizado, el workspace se abrirá dentro del panel de Microsoft Sentinel.

---

## ✔ Resultado Esperado

Microsoft Sentinel debe quedar correctamente habilitado sobre el workspace:

| Configuración | Valor |
|--------------|--------|
| Workspace | defenderWorkspace |
| Estado | Enabled / Active |
| Servicio | Microsoft Sentinel |

![Panel principal de Microsoft Sentinel](images/img2.jpg)

---

## 🎯 Resultado Final

Microsoft Sentinel queda desplegado correctamente sobre el Log Analytics Workspace, permitiendo iniciar la configuración de:

- Conectores de datos  
- Reglas analíticas  
- Watchlists  
- Inteligencia de amenazas  
- Automatización (SOAR)  

---
