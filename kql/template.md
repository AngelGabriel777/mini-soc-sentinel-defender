
# 🔍 Consulta KQL: Dispositivos con alertas críticas

## 📝 Descripción
Identifica todos los dispositivos que han generado alertas de alta severidad en las últimas 24 horas.

## 🎯 Objetivo
- Detectar dispositivos comprometidos  
- Facilitar respuesta rápida

## 🧩 MITRE ATT&CK
- Táctica(s): Initial Access, Execution  
- Técnica(s): T1059 (Command and Scripting Interpreter)

## 🧪 Consulta KQL
```kql
SecurityAlert
| where Severity == "High"
| where TimeGenerated > ago(24h)
| summarize Count = count() by DeviceName, AlertName
| order by Count desc
