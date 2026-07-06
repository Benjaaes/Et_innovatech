# 🚀 InnovaTech - Plataforma de Ventas y Despachos (Arquitectura Serverless)

![AWS](https://img.shields.io/badge/AWS-Fargate-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=for-the-badge&logo=spring-boot&logoColor=white)
![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=github-actions&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)

Bienvenido al repositorio de **InnovaTech**, una solución empresarial basada en microservicios, desplegada en la nube utilizando contenedores y prácticas modernas de DevOps.

## 🏗️ Arquitectura de Microservicios

Este proyecto está dividido en tres componentes principales independientes, completamente dockerizados y optimizados (multietapa):

- 🟢 **Microservicio de Ventas (`back-Ventas_SpringBoot`)**: Desarrollado en Java 17 con Spring Boot. 
- 🟡 **Microservicio de Despachos (`back-Despachos_SpringBoot`)**: Desarrollado en Java 17 con Spring Boot. 
- 🔵 **Frontend Web (`front_despacho`)**: SPA desarrollada en React.js y empaquetada con Nginx. (Implementa Runtime Config).

## ☁️ Infraestructura Cloud (AWS)

La infraestructura está totalmente aprovisionada en **Amazon Web Services (AWS)** bajo un enfoque Serverless:
- **AWS ECS (Fargate)**: Orquestación y ejecución de contenedores sin necesidad de aprovisionar o administrar servidores EC2.
- **Amazon ECR**: Registro de contenedores privados para almacenar y administrar las imágenes Docker de los microservicios.
- **Amazon RDS**: Base de datos MySQL 8.x gestionada, altamente disponible y segura.
- **Application Load Balancer (ALB)**: Enrutamiento de tráfico inteligente basado en rutas (Path-based routing) apuntando a diferentes Target Groups.

## 🔄 Integración y Despliegue Continuo (CI/CD)

El pipeline de **GitHub Actions** (`deploy.yml`) automatiza completamente el ciclo de vida del software en cada push a `main`:
1. **Test**: Ejecución estricta de pruebas unitarias automáticas con base de datos H2 en memoria.
2. **Build & Push**: Construcción de las imágenes Docker (con doble etiquetado `latest` y el `SHA` del commit) y subida a Amazon ECR.
3. **Deploy Serverless**: Despliegue dinámico y transparente en AWS ECS Fargate, actualizando las revisiones de las Task Definitions.

---
*Este proyecto fue desarrollado para la Evaluación Final Transversal (EFT) aplicando las mejores prácticas de Desarrollo, Seguridad y Operaciones Cloud.*