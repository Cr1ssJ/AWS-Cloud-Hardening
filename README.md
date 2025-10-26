# 🛡️ AWS Cloud Hardening – Zero Trust Architecture

**Autor:** Cristian Jiménez  
**Ocupación:** Estudiante de la Licenciatura en Ciberseguridad en la Universidad Tecnológica de Panamá (UTP)  
**Proyecto:** Implementación de arquitectura Zero Trust en AWS  
**Fecha:** Octubre 2025  

---

## 📘 Resumen General

Este proyecto implementa los principios de **Zero Trust Architecture (ZTA)** en un entorno AWS, siguiendo las recomendaciones del **NIST SP 800-207** y las mejores prácticas de seguridad en la nube.  
El objetivo es reforzar la **seguridad por capas**, **minimizar la confianza implícita**, y **proteger datos, identidades y redes** bajo un modelo de autenticación y autorización continua.

---

## 🧩 Fase 0 – Fundamentos de Zero Trust

> _“Zero Trust no es un producto, es una filosofía de seguridad.”_ – John Kindervag  

Zero Trust se basa en tres principios:
- **Nunca confiar, siempre verificar.**  
- **Aplicar privilegio mínimo.**  
- **Monitorear y validar continuamente.**

**Conceptos Clave:**
- La red siempre se considera hostil.  
- No existe confianza implícita (interna o externa).  
- Toda solicitud pasa por un **PDP (Policy Decision Point)** y un **PEP (Policy Enforcement Point)**.  
- Las decisiones de acceso se basan en identidad, dispositivo, comportamiento y contexto.  

---

## 🔐 Fase 1 – Configuración de Seguridad de Identidad (IAM)

Se establecen identidades seguras mediante **AWS IAM** y **IAM Identity Center**.  
- **Autenticación Multifactor (MFA)** obligatoria para usuarios privilegiados.  
- **Roles IAM** con privilegios mínimos (principio PoLP).  
- Generación de **credential reports**, políticas y roles documentados en `/Evidence/`.  

**Servicios:** IAM Users / Roles / Policies / MFA  
**Componentes ZT:** PAP (Policy Administration Point)

---

## 🧠 Fase 2 – Identidad y Control de Acceso (IAM)

Integración con **IAM Identity Center** como fuente de identidad central (PDP).  
- Autenticación y autorización continua.  
- Evaluación de políticas contextuales (región, dispositivo, IP).  
- Uso de **Session Manager** para acceso sin SSH a instancias.  

**Servicios:** IAM Identity Center, SSM Session Manager  
**ZT Rol:** PDP (Policy Decision Point)

---

## 🗄️ Fase 3 – Protección de Datos (S3 + SSE-S3)

Aplicación de controles de confidencialidad y disponibilidad de datos.  
- Buckets S3 con **SSE-S3 (Server-Side Encryption)**.  
- **Versioning habilitado** para protección ante borrados accidentales.  
- **Bloqueo de acceso público** en S3.  

**Servicios:** S3, SSE-S3, IAM Policies  
**ZT Rol:** PAP (administra políticas de cifrado y acceso)

---

## 🌐 Fase 4 – Segmentación de Red (VPC y Security Groups)

Diseño de una **VPC segmentada** en subredes públicas y privadas.  
- **Subred pública:** EC2 Bastion (PEP) con acceso por Session Manager.  
- **Subred privada:** RDS Multi-AZ (cifrado y aislado).  
- Tablas de rutas separadas (pública/privada).  
- Security Groups que permiten únicamente el tráfico necesario (3306/5432 desde Bastion).

**Servicios:** VPC, EC2, Security Groups, NACLs  
**ZT Rol:** PEP (Policy Enforcement Point)

---

## 🧩 Fase 5 – Base de Datos RDS Privada + KMS + IAM bajo Zero Trust

Implementación de Amazon RDS MySQL Multi-AZ con cifrado KMS.  
- RDS solo accesible desde Bastion (Zero Trust en capa de red).  
- KMS gestiona claves de cifrado para RDS.  
- Políticas IAM asocian roles de EC2 y RDS.  
- Monitoreo de consultas y logs en CloudWatch.

**Servicios:** RDS Multi-AZ, KMS, IAM, Security Groups  
**ZT Rol:** PAP + PEP combinados en la capa de datos.

---

## 📡 Fase 6 – Monitoreo Continuo (CloudWatch + CloudTrail + SNS)

Monitoreo basado en eventos y alertas de seguridad.  
- **CloudTrail** registra todos los eventos de API.  
- **CloudWatch Metric Filter** detecta intentos de inicio fallido.  
- **SNS Topic security-alerts** envía notificaciones por correo.  

**Ejemplo de Filtro CloudWatch (JSON):**
```json
{ "$.eventName": "ConsoleLogin", "$.errorMessage": "Failed authentication" }
