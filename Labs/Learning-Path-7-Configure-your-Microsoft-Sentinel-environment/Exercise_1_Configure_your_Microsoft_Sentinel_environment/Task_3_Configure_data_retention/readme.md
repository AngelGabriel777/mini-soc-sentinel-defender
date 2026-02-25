
## Task 3 – Configure Data Retention

---

## Objetivo

Configurar el período de retención de datos del Log Analytics Workspace para cumplir con requerimientos de almacenamiento, cumplimiento normativo y análisis forense.

---

## Prerrequisito

- Microsoft Sentinel habilitado.
- Log Analytics Workspace:
  - **Nombre:** defenderWorkspace
  - **Resource Group:** Defender-RG
  - **Estado:** Active

---

## Paso 1 – Regresar al Inicio del Portal

1. En el menú superior del portal de Azure (breadcrumb), seleccionar **Home**.

---

##  Paso 2 – Acceder a Log Analytics Workspaces

1. En la barra de búsqueda del portal de Azure, escribir:


2. Seleccionar **Log Analytics workspaces** en la sección *Services*.

---

##  Paso 3 – Seleccionar el Workspace

1. En la lista de workspaces disponibles, seleccionar:

**defenderWorkspace**

---

## Paso 4 – Acceder a Configuración de Retención

1. En el panel izquierdo, expandir la sección **Settings**.
2. Seleccionar **Usage and estimated costs**.
3. Dentro de esta sección, seleccionar **Data retention**.

---

##  Paso 5 – Modificar el Período de Retención

1. Cambiar el período de retención de datos a:

2. Seleccionar **OK** para aplicar los cambios.

---

##  Resultado Esperado

El Log Analytics Workspace debe reflejar la nueva configuración:

| Configuración        | Valor |
|---------------------|--------|
| Workspace           | defenderWorkspace |
| Data Retention      | 180 days |
| Estado              | Updated |

---

## Resultado Final

El Log Analytics Workspace queda configurado con un período de retención de 180 días, permitiendo:

- Mayor capacidad de investigación histórica.
- Cumplimiento de requisitos de auditoría.
- Balance entre retención y costos operativos.

---
