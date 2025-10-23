# 🛡️ AWS Cloud Hardening & Monitoring (Free Tier Edition)

## 🎯 Objetivo
El objetivo de este proyecto es implementar una arquitectura segura en AWS aplicando los **principios de Zero Trust Architecture (ZTA)** según el marco **NIST SP 800-207**, utilizando exclusivamente servicios disponibles en el **Free Tier de AWS**.  
Este proyecto demuestra cómo aplicar controles de acceso estrictos, segmentación de red, cifrado y monitoreo continuo sin depender de servicios premium como GuardDuty o Security Hub.

---

## 🧩 Arquitectura general
[Usuario / Administrador]

│

▼
[IAM + MFA (PAP & PDP)]

│

▼

[VPC]
├── Subred Pública → EC2 Bastion (PEP - punto de control)

└── Subred Privada → RDS (base de datos cifrada, acceso solo interno)

│

▼

[S3 (privado y cifrado con KMS)]

│

▼

[CloudTrail + CloudWatch + SNS (PIP y monitoreo continuo)]


## 🧱 Servicios utilizados

| Servicio | Rol / Propósito |
|-----------|-----------------|
| **IAM** | Control de identidades y políticas (PAP / PDP). |
| **S3** | Almacenamiento seguro con cifrado y acceso controlado. |
| **KMS** | Cifrado de datos en reposo para S3, RDS y EBS. |
| **VPC** | Microsegmentación de red (zonas de confianza reducida). |
| **EC2 (Bastion)** | Punto de acceso seguro a recursos internos (PEP). |
| **RDS** | Base de datos privada cifrada (recurso protegido). |
| **CloudTrail** | Auditoría de acciones (PIP: información de políticas). |
| **CloudWatch** | Detección de anomalías y generación de alertas. |
| **SNS** | Notificaciones automáticas de seguridad. |

---

## ⚙️ Pasos de creación del laboratorio
(Archivo Adjunto dentro del repositorio)

---

## 🧠 Integración de componentes Zero Trust en AWS

El modelo **Zero Trust** se basa en cuatro funciones principales:
- **PAP (Policy Administration Point)** – administra las políticas de acceso.
- **PDP (Policy Decision Point)** – decide si un acceso se permite o no.
- **PEP (Policy Enforcement Point)** – aplica las decisiones del PDP.
- **PIP (Policy Information Point)** – provee información contextual al PDP.
- **PDP Feedback Loop** – monitorea la actividad y ajusta políticas según comportamiento.

### 🔹 **1. Policy Administration Point (PAP)**
**Servicio:** *AWS IAM + KMS + S3 Policies*  
Define las políticas de acceso y cifrado.  
- Las políticas IAM controlan quién puede realizar acciones (ej. `s3:GetObject`, `rds:Connect`).  
- Las claves KMS se asocian a políticas específicas para restringir cifrado/descifrado.  
- Las políticas de bucket S3 administran accesos explícitos y transporte seguro (`aws:SecureTransport`).

📘 **Ejemplo:**  
Un usuario no puede acceder a un bucket si no usa HTTPS o no tiene MFA habilitado.

---

### 🔹 **2. Policy Decision Point (PDP)**
**Servicio:** *AWS IAM + CloudTrail + CloudWatch*  
Evalúa si la solicitud de acceso cumple las políticas establecidas.  
- IAM determina si la identidad tiene permisos válidos.  
- CloudTrail registra el contexto de cada acceso (hora, IP, origen).  
- CloudWatch analiza eventos en tiempo real para detectar patrones sospechosos.

📘 **Ejemplo:**  
Cuando un usuario intenta detener una instancia EC2, IAM valida el permiso y CloudWatch detecta la acción, enviando una alerta si no corresponde a la política.

---

### 🔹 **3. Policy Enforcement Point (PEP)**
**Servicio:** *Security Groups + EC2 Bastion + VPC NACLs*  
El PEP actúa como la “frontera de control” que aplica las decisiones del PDP.  
- Los **Security Groups** y **NACLs** bloquean tráfico no autorizado.  
- El **Bastion Host (EC2)** aplica control de acceso por IP y llave SSH.  
- Todo acceso a la subred privada (RDS) debe pasar por el Bastion, asegurando “no confianza implícita”.

📘 **Ejemplo:**  
El usuario no puede conectarse directamente al RDS, solo mediante el EC2 Bastion validado.

---
