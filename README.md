# Comercial Nova — Portal WordPress en AWS

Proyecto final del curso Virtualización de Servicios Tecnológicos, Universidad Peruana Unión (Filial Juliaca).

## 1. Integrante y rol

Rosa María de los Ángeles Torres Apaza — Diseño de arquitectura, implementación en AWS, documentación y pruebas (trabajo individual).

## 2. Problema planteado y alcance

Comercial Nova requiere un portal web corporativo y de contenidos en WordPress, desplegado en AWS, con enfoque en disponibilidad, seguridad, escalabilidad y control de costos. El alcance de esta entrega cubre: despliegue funcional de WordPress en alta disponibilidad, base de datos gestionada, almacenamiento persistente, monitoreo básico y análisis de costos.

## 3. Arquitectura propuesta

Arquitectura de dos capas sobre una VPC segmentada en subredes públicas y privadas:

- Capa de aplicación: 2 instancias EC2 (Amazon Linux + Apache + PHP) detrás de un Application Load Balancer, en subredes públicas de distintas zonas de disponibilidad.
- Capa de datos: Amazon RDS (MySQL) Multi-AZ en subred privada, sin acceso público.
- Almacenamiento: Amazon S3 para medios y/o backups, con versionado habilitado.
- Monitoreo: Amazon CloudWatch con dashboard y alarma de CPU.

Diagrama: ver `arquitectura/diagrama.png`. Detalle de decisiones: ver `arquitectura/justificacion_arquitectura.md`.

## 4. Servicios cloud utilizados y por qué

| Servicio | Uso | Justificación |
|---|---|---|
| Amazon EC2 | Servidores web WordPress (x2) | Redundancia y capacidad de escalar horizontalmente |
| Application Load Balancer | Distribución de tráfico | Alta disponibilidad, health checks automáticos |
| Amazon RDS (MySQL) | Base de datos de WordPress | Gestión administrada, backups automáticos, Multi-AZ |
| Amazon S3 | Medios / backups | Almacenamiento durable y económico, versionado |
| Amazon CloudWatch | Monitoreo y alertas | Visibilidad operacional sin infraestructura adicional |
| IAM | Control de acceso | Principio de mínimo privilegio |
| VPC | Segmentación de red | Aislar capa de datos de acceso público |

## 5. Pasos para desplegar / reproducir

1. Crear VPC con 2 subredes públicas y 2 privadas en distintas AZ.
2. Crear Security Groups: SG-ALB (80/443 abierto), SG-EC2 (80 solo desde SG-ALB, 22 restringido), SG-RDS (3306 solo desde SG-EC2).
3. Crear instancia RDS MySQL en subred privada, sin acceso público, con backups automáticos activados.
4. Lanzar 2 instancias EC2 en subredes públicas con el script de user-data (ver `infraestructura/scripts/`).
5. Verificar acceso individual a cada EC2 y completar la instalación de WordPress.
6. Crear bucket S3, configurar acceso privado y versionado.
7. Crear Target Group + Application Load Balancer apuntando a las 2 EC2.
8. (Opcional) Crear Launch Template + Auto Scaling Group con política de CPU.
9. Configurar dashboard y alarma en CloudWatch.
10. Documentar estimación de costos y optimizaciones.

## 6. URL / evidencia de acceso al WordPress desplegado

- URL (DNS del ALB): `[pendiente]`
- Ver evidencia completa en `wordpress/evidencia_publicacion_contenido.md`

## 7. Evidencias de funcionamiento

Ver `evidencias/pruebas_funcionamiento.md` y capturas en `evidencias/capturas_servicios/`.

## 8. Estrategia de seguridad, monitoreo y costos

- **Seguridad:** ver `seguridad/matriz_accesos.md`
- **Monitoreo:** ver `monitoreo/dashboard_metricas.png` y `monitoreo/alertas_configuradas.md`
- **Costos:** ver `costos/estimacion_costos.xlsx` y `costos/optimizacion_costos.md`

## 9. Evidencias específicas de AWS

Inventario completo de recursos y capturas por servicio (VPC, EC2, S3, RDS, IAM, CloudWatch) en `aws/inventario_recursos_aws.md` y `aws/evidencias_ec2_s3_rds_cloudwatch.md`.

## 10. Limitaciones conocidas y mejoras futuras

- [Ej. AWS Academy no permite crear roles IAM personalizados; se usó el LabRole predeterminado]
- [Ej. Auto Scaling limitado por cuota del laboratorio; se documentó como restricción]
- Mejoras futuras: CDN con CloudFront, cifrado adicional con KMS, WAF para protección de aplicación, pipeline de despliegue automatizado con IaC.

## 11. Lecciones aprendidas

[Completar al finalizar: qué funcionó, qué se cambiaría, principales dificultades técnicas.]
