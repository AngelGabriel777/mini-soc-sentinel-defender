
# 🔌 Integración: Microsoft Sentinel + Defender XDR

## 📝 Descripción General
Esta integración centraliza los logs de Defender XDR en Microsoft Sentinel para correlacionar alertas y generar detecciones automáticas.

## 🎯 Objetivo
- Permitir monitoreo y respuesta ante amenazas desde Sentinel
- Automatizar alertas y playbooks

## 🧩 Servicios Involucrados
- Microsoft Sentinel  
- Microsoft Defender XDR  

## 🔧 Configuración Técnica
### Requisitos Previos
- Permisos de administrador en Azure y Microsoft 365
- Licencias activas de Defender XDR y Sentinel
- Acceso al tenant

### Pasos de Configuración
1. Configurar el workspace en Microsoft Sentinel  
2. Activar conector de Defender XDR  
3. Validar ingestión de logs

### Parámetros Utilizados
| Parámetro | Valor | Descripción |
|----------|--------|--------------|
| Workspace | SOC-Workspace | Sentinel workspace principal |
| Conector | Defender XDR | Fuente de logs y alertas |

## 📊 Datos Ingeridos
- Tipos de logs: SecurityEvent, Alerts, DeviceEvents  
- Frecuencia: cada 5 minutos  
- Tamaño aproximado: variable según endpoints

## 🔒 Seguridad
- Usar cuentas con permisos mínimos  
- No exponer logs sensibles sin necesidad  

## 🧪 Validación
- Revisar logs en Sentinel  
- Ejecutar queries de prueba para alertas

## 📁 Archivos Relacionados
- `/kql/queries/sentinel_defender_queries.md`
