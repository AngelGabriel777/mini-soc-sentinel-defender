
# 🏗 Arquitectura del Sistema – Mini SOC

## 📝 Descripción
Mini SOC que centraliza la seguridad de endpoints, identidades, correo y nube mediante Microsoft Sentinel y Microsoft Defender XDR. Permite detección de amenazas, hunting, automatización y respuesta ante incidentes.

## 🧩 Componentes Principales
- Microsoft Sentinel (centralización de logs y detecciones)
- Defender XDR (detección avanzada y automatización)
- Defender for Endpoint (protección de dispositivos)
- Defender for Identity (protección de identidades)
- Defender for Office 365 (protección de correo)
- Cloud Apps (protección de aplicaciones en la nube)
- Logic Apps (automatización de playbooks)
- Azure Storage (almacenamiento de logs y datos históricos)

## 🔁 Flujo de Datos
1. Endpoint → Defender for Endpoint  
2. Identidad → Defender for Identity  
3. Correo → Defender for Office 365  
4. Aplicaciones → Defender for Cloud Apps  
5. Todos los logs y alertas centralizados en Microsoft Sentinel  

## 📐 Diagrama Conceptual
![Diagrama de Arquitectura](../../diagrams/ejemplo_diagrama.png)

## 📐 Diagrama Lógico
- Workspaces de Sentinel configurados por tipo de log  
- Conectores activos para Defender XDR, Office 365 y Endpoint  
- Reglas de detección y alertas aplicadas  

## 🔒 Consideraciones de Seguridad
- Principio de menor privilegio
- Zero Trust
- Auditoría periódica de logs y alertas

## 📁 Archivos Relacionados
- `/diagrams/ejemplo_diagrama.png`
