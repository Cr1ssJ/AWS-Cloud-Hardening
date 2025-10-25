# 🛡️ AWS Cloud Hardening – Zero Trust Architecture

**Autor:** Cristian Jiménez  
**Carrera:** Estudiante de la Licenciatura en Ciberseguridad – Universidad Tecnológica de Panamá  
**Fecha:** Octubre 2025  

---

## 🎯 Objetivo General
Diseñar y desplegar un entorno AWS seguro basado en los principios de **Zero Trust Architecture (ZTA)**, conforme a las recomendaciones de **NIST SP 800-207**, aplicando buenas prácticas de endurecimiento de infraestructura, monitoreo continuo y respuesta ante incidentes.

---

## 🧩 Fases del Proyecto

### 🧱 **Fase 1 – Configuración de Identidad (IAM + MFA + Identity Center)**
- Creación de usuarios, grupos y roles IAM.
- Habilitación de MFA en cuentas privilegiadas.
- Configuración de políticas `iam-policy-sample.json`.
- Integración con AWS Identity Center.

📁 **Evidencias:**
`iam-credential-report.csv`, `iam-users.json`, `iam-policies.json`, `iam-roles.json`, `identity-center-user.png`, `mfa-setup.png`, `mfa-setup-completed.png`.

---

### ☁️ **Fase 2 – Almacenamiento Seguro (S3 Hardening)**
- Creación del bucket `zt-cloudtrail-logs`.
- Configuración de **versioning**, **encryption (AES-256)** y **bucket policy** restrictiva.
- Pruebas de acceso y validación de políticas.

📁 **Configuraciones:**
`s3-policy.json`, `s3-encryption.json`, `s3-versioning.json`.

📁 **Evidencias:**
`permissions.png`, capturas de políticas aplicadas.

---

### 🖥️ **Fase 3 – Infraestructura Segura (VPC + EC2 + RDS)**
- Creación de VPC Zero Trust `ZT-VPC`.
- Subnets pública y privada con tablas de ruteo personalizadas.
- Despliegue de **EC2 (Amazon Linux 2023)** y **RDS (MySQL)**.
- Asociación de roles SSM (`AmazonSSMManagedInstanceCore`) para control sin SSH.
- Pruebas de conexión y registro de logs.

📁 **Evidencias:**
Capturas de `Route Tables`, `Subnets`, `RDS instance`, `Session Manager`, `diagram-phase3.png`.

---

### 🧠 **Fase 4 – Gestión de Logs (CloudWatch + RDS Integration)**
- Exportación de logs RDS a CloudWatch.
- Creación de log groups `/aws/rds/zt-db-instance/error`.
- Validación de métricas y retención.

📁 **Evidencias:**
Capturas de `Log groups`, `RDS Logs`, `CloudWatch configuration`.

---

### 🔍 **Fase 5 – Monitoreo Continuo (CloudTrail + CloudWatch + SNS)**
- Activación de **CloudTrail** para toda la cuenta.
- Envío de logs a S3 (`zt-cloudtrail-logs`).
- Creación de **metric filter** para detectar intentos de inicio fallido:
  ```bash
  { ($.eventName = "ConsoleLogin") && ($.errorMessage = "Failed authentication") }
- Configuración de alarma Security Alert vinculada a SNS Topic (security-alerts).
- Prueba de simulación con credenciales erróneas.

### 📁 Evidencias
Capturas sugeridas:
- `Security Alert (OK / ALARM)` → `security-alert.png`
- `Notificación por correo (SNS)` → `sns-notification.png`

---

## 🔐 Fase 6 – Integración Zero Trust (PAP, PDP, PEP, PIP)

### 🎯 Objetivo
Alinear la arquitectura a **NIST SP 800-207**, mapeando servicios AWS a los componentes del modelo Zero Trust.

| Componente | AWS Servicio              | Función                                   |
|------------|---------------------------|-------------------------------------------|
| **PAP**    | IAM, S3 Policy, KMS       | Administra políticas y cifrado            |
| **PDP**    | IAM, CloudWatch           | Evalúa decisiones de acceso               |
| **PEP**    | EC2 Bastion, SG, NACL     | Aplica políticas y bloqueos               |
| **PIP**    | CloudTrail, CloudWatch Logs | Provee contexto (IP, región, tiempo)    |
| **Feedback** | SNS, IAM                | Ajusta políticas adaptativamente          |

**Evidencias (Fase 6):**
- Diagrama Zero Trust final → `zero-trust-diagram.png`
- Documento teórico / explicación de integración → `docs/zero-trust-notes.md` (opcional)

---

## 🧾 Fase 7 – Evidencias y Reporte Final

### 🎯 Objetivo
Consolidar evidencias técnicas y elaborar la documentación final del proyecto.

**Exportación de logs y artefactos:**
- `cloudtrail-events.json` (Event history o consulta)
- `cloudwatch-alarms.json` (estado/config de alarmas)
- `sns-alert.png` (captura del correo recibido)
- `iam-credential-report.csv` (ya incluido)
- Diagramas finales (`architecture-final.png`, `zero-trust-diagram.png`)


  
