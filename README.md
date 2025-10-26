# Proyecto Zero Trust Architecture en AWS (Free Tier)

**Descripción del Proyecto**: El objetivo de este proyecto fue diseñar e implementar una arquitectura de seguridad de Zero Trust en AWS, empleando principalmente servicios dentro del nivel gratuito (Free Tier). La solución demuestra cómo aplicar controles de acceso estrictos, segmentación de red, cifrado de datos y monitoreo continuo sin depender de servicios premium (no se usaron, por ejemplo, GuardDuty ni Security Hub). De este modo, se crea un entorno en la nube seguro y trazable, alineado con las guías de Zero Trust del NIST SP 800-207, y se evidencia que es posible mejorar la postura de seguridad usando recursos estándar de AWS Free Tier.

La arquitectura implementa un modelo Zero Trust con múltiples capas defensivas en AWS. Todos los accesos comienzan en un plano de identidad robusto: un AWS IAM Identity Center con MFA, que actúa como punto de administración y decisión de políticas (PAP/PDP) para autenticar y autorizar a cada usuario antes de permitirle el acceso. Los recursos en la nube se aíslan en una VPC con subredes separadas: una subred pública que aloja un Bastion Host (servidor de salto seguro como Policy Enforcement Point, PEP) y una subred privada donde reside la base de datos (Amazon RDS) sin exposición pública. Los datos sensibles se almacenan cifrados (por ejemplo, objetos en Amazon S3 protegidos con claves de AWS KMS) y se aplican políticas que requieren conexiones seguras (HTTPS) y cifrado de datos para cualquier acceso. Además, se habilitó un monitoreo contínuo de la actividad mediante AWS CloudTrail y Amazon CloudWatch, complementado con notificaciones automáticas vía Amazon SNS, logrando visibilidad total de eventos de seguridad y respuesta inmediata ante incidentes. En conjunto, la arquitectura resultante integra la administración de políticas (IAM Identity Center, IAM y KMS), el monitoreo permanente (CloudWatch, CloudTrail) y mecanismos de alerta (SNS), cumpliendo los lineamientos del marco NIST SP 800-207.

## Fases del proyecto
- **Fase 0: Configuración Inicial de Identidades Seguras**
- **Fase 1: Auditoría de Identidades y Control de Acceso con Mínimo
Privilegio (IAM)**
- **Fase 2: Protección de Datos en Almacenamiento (Bucket S3 Seguro con
Cifrado)**
- **Fase 3: Segmentación de Red (VPC Privada, Subredes y Security Groups)**
- **Fase 4: Implementación de Bastion Host Seguro y Acceso a Base de
Datos Privada**
- **Fase 5: Monitoreo Continuo y Alertas Automatizadas (CloudTrail,
CloudWatch, SNS)**

## Servicios AWS Utilizados

- **AWS IAM Identity Center (AWS SSO)**
- **AWS Identity and Access Management (IAM)**
- **AWS Key Management Service (KMS)**
- **Amazon Virtual Private Cloud (VPC)**
- **Amazon Elastic Compute Cloud (EC2)**
- **AWS Systems Manager (Session Manager)**
- **Amazon Relational Database Service (RDS)**
- **Amazon Simple Storage Service (S3)**
- **AWS CloudTrail**
- **Amazon CloudWatch**
- **Amazon Simple Notification Service (SNS)**

## Evidencias dentro de este Repositorio

- Capturas de configuración
- Políticas y scripts
- Pruebas de seguridad
- Informe técnico

## Requisitos para Replicar el Proyecto

Cuenta de AWS Free Tier: Una cuenta de AWS con elegibilidad para el nivel gratuito. Es recomendable usar la región us-east-1 (N. Virginia) u otra región donde los servicios mencionados estén disponibles dentro de Free Tier. Asegúrese de que la cuenta esté configurada con las credenciales adecuadas y sin usos previos que consuman los límites gratuitos necesarios (por ejemplo, horas de EC2/RDS disponibles).

- **MFA y AWS Identity Center**: Habilitar MFA en la cuenta (especialmente en la raíz) y, de ser posible, utilizar AWS IAM Identity Center para gestionar usuarios y roles temporales. (Alternativamente, se podría usar un usuario IAM convencional con MFA para simplificar, aunque la arquitectura óptima utiliza Identity Center para evitar usuarios IAM estáticos.) Es necesario contar con una aplicación de autenticación MFA (como Google Authenticator, Authy, etc.) para las pruebas de inicio de sesión con MFA.

- **Conocimientos básicos de AWS**: Familiaridad con la consola de AWS y los servicios mencionados. La implementación se puede realizar íntegramente a través de la consola web y AWS CloudShell/CLI siguiendo las fases descritas. No se requieren herramientas de pago ni suscripciones adicionales.

- **Límites del Free Tier**: Todos los recursos se eligieron para permanecer dentro de las franquicias gratuitas (instancias t3.micro de EC2/RDS, 20 GB de almacenamiento, cierto volumen de logs, etc.). Aun así, se debe monitorear el uso para no exceder los límites mensuales del Free Tier y evitar costos inesperados.


**Inspiración**: Proyecto desarrollado como iniciativa personal, siguiendo las recomendaciones del marco NIST SP 800-207 (Zero Trust Architecture) y las mejores prácticas de seguridad en la nube de AWS y el Curso Designing & Deploying a Zero Trust Architecture de EC-Council.


