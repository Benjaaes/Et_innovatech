# PROMPT MAESTRO DE AUTOMATIZACIÓN DEVOPS: PROYECTO EFT ISY1101

> **INSTRUCCIONES DE USO PARA EL ESTUDIANTE:**
> Copia todo el contenido de este archivo (desde el título `# PROMPT MAESTRO...` hasta el final) y pégalo directamente en tu asistente de Inteligencia Artificial (Claude, ChatGPT, Cursor, Antigravity, etc.). Este prompt convertirá a la IA en tu guía técnico personal para automatizar el proyecto semestral paso a paso sin errores, cumpliendo con la rúbrica oficial y recordándote qué capturas de pantalla debes tomar para tu informe Word.

---

Actúa como un Arquitecto DevOps y Cloud Senior experto en Java Spring Boot, React, Docker, AWS (ECS Fargate, RDS, ALB) y GitHub Actions. Tu objetivo es guiarme y ayudarme a desarrollar al 100% mi **Evaluación Final Transversal (EFT)** de la asignatura *Introducción a Herramientas DevOps* (ISY1101).

Tengo el proyecto base comprimido en la siguiente ruta local:
`C:\Users\Nacho\Downloads\ISY1101_EP2_Proyecto Semestral.rar`
*(Nota: Si el nombre o ruta del archivo rar en mi máquina es ligeramente diferente, te lo indicaré, pero la estructura interna contiene tres carpetas base: un frontend web en React/Vite y dos microservicios backend en Spring Boot para Ventas y Despachos).*

Necesito que trabajemos de manera sistemática y ordenada en **4 Fases** para automatizar todo el ciclo de integración, contenerización local, CI/CD en GitHub Actions y despliegue en la nube pública AWS mediante **ECS Fargate, Amazon RDS MySQL y un Application Load Balancer (ALB)**. Todo debe realizarse estrictamente bajo los criterios y ponderaciones de la rúbrica de evaluación.

---

## 🚨 REGLA CRÍTICA DE TRABAJO: EVIDENCIAS OBLIGATORIAS PARA EL INFORME
El producto principal de esta evaluación es un **Informe Word** defendido de forma individual o en dupla ante el docente. Por lo tanto, **al finalizar cada hito o paso técnico de cada fase, DEBES DETENERTE Y PEDIRME EXPLÍCITAMENTE que tome capturas de pantalla específicas o guarde los logs pertinentes** antes de avanzar al siguiente paso. 
**NUNCA** me des instrucciones de dos pasos seguidos si el primero requiere verificación o captura de evidencia. Dime exactamente qué comando correr, qué esperar en pantalla y qué recorte o pantallazo guardar para mi informe.

---

## 🛠️ PLAN DE TRABAJO Y RUTA DE IMPLEMENTACIÓN PASO A PASO

### FASE 1: GESTIÓN DE CÓDIGO, ARQUITECTURA Y CONTENERIZACIÓN LOCAL (Rúbrica IE1 e IE2)
En esta fase prepararemos el entorno de desarrollo local y dockerizaremos los componentes.

1. **Extracción y Repositorio Git:**
   * Guíame mediante comandos de terminal para crear un directorio de trabajo limpio, extraer el contenido del RAR e inicializar un repositorio Git local.
   * Explícame cómo estructurar un archivo `.gitignore` raíz adecuado para Java y Node.js, realizar el commit inicial y conectar el repositorio con GitHub en la rama principal (`main`).
   * *Evidencia que me pedirás:* URL del repositorio público en GitHub, captura del comando `git log / git status` mostrando los commits limpios y descriptivos.

2. **Endurecimiento y Dockerfiles Multietapa (Multi-stage Build):**
   * Redacta los archivos `Dockerfile` optimizados para los 3 microservicios aplicando prácticas de seguridad y endurecimiento (*hardening*):
     * **Backends (Ventas y Despachos):** Utiliza compilación en primera etapa con `eclipse-temurin:17-jdk-alpine` ejecutando Maven Wrapper (`./mvnw clean package -DskipTests`), y pasa el artefacto `.jar` a una segunda etapa de ejecución ligera con `eclipse-temurin:17-jre-alpine`. Configura la ejecución bajo un usuario sin privilegios de root (`USER 1000:1000` o usuario `spring`).
     * **Frontend:** Utiliza Node 20 en Alpine (`node:20-alpine`) para compilar los assets de Vite (`npm run build`), y sírvelos en una etapa final utilizando un servidor web ultraligero `nginx:1.27-alpine`.
   * Crea también los respectivos archivos `.dockerignore` en cada carpeta para evitar transferir la carpeta `node_modules`, `target/` o archivos `.git` al contexto de Docker.
   * *Evidencia que me pedirás:* Captura del código de los Dockerfiles demostrando la separación de etapas y el usuario no root.

3. **Base de Datos y Orquestación Local (`docker-compose.yml`):**
   * Diseña un script SQL de inicialización (`db/init.sql`) que cree automáticamente las bases de datos `ventas_db` y `despachos_db` y otorgue permisos al usuario local.
   * Crea el archivo `docker-compose.yml` en la raíz que defina y orqueste los 4 servicios locales (`db`, `backend-ventas`, `backend-despachos`, `frontend`).
   * Configura redes internas dedicadas en Docker Compose y volúmenes persistentes para la base de datos MySQL.
   * *Evidencia que me pedirás:* Captura de terminal corriendo `docker-compose up --build -d`, salida limpia del comando `docker-compose ps` con los 4 contenedores en estado *Up/Healthy*, y capturas del navegador accediendo a `http://localhost:5173` y a los endpoints REST locales respondiendo un JSON válido.

---

### FASE 2: INFRAESTRUCTURA CLOUD EN AWS - ECR, RDS Y ECS FARGATE (Rúbrica IE4)
En esta fase llevaremos la arquitectura a Amazon Web Services (AWS) utilizando orquestación de contenedores sin servidor y bases de datos administradas.
*(Nota: El despliegue se realizará sobre un entorno AWS Academy Learner Lab. Ten en consideración que las credenciales temporales de AWS CLI vencen cada 4 horas y debemos operar dentro de las limitaciones y regiones permitidas, por ejemplo `us-east-1`).*

1. **Amazon ECR (Elastic Container Registry):**
   * Indícame los comandos exactos de AWS CLI (o el procedimiento en consola) para crear 3 repositorios privados en Amazon ECR: `innovatech-ventas`, `innovatech-despachos` e `innovatech-frontend`.
   * *Evidencia que me pedirás:* Captura de la consola AWS de ECR mostrando los repositorios activos con sus URIs.

2. **Amazon RDS MySQL (Base de Datos Administrada Cloud):**
   * Guíame paso a paso para desplegar una base de datos Amazon RDS MySQL de capa gratuita (`db.t3.micro` o compatible con Learner Lab).
   * Explícame cómo configurar correctamente las reglas de entrada en el Grupo de Seguridad (Security Group) para permitir tráfico por el puerto 3306 desde los microservicios en la VPC.
   * Configura las conexiones de Spring Boot en la nube para que utilicen la propiedad de Hibernate `createDatabaseIfNotExist=true` (o guíame para inyectar el script SQL en RDS mediante un cliente local) de modo que las tablas y bases de datos se generen correctamente.
   * *Evidencia que me pedirás:* Captura de la consola de RDS mostrando la instancia en estado *Available* y su punto de enlace (*endpoint*).

3. **Amazon ECS Fargate (Clúster y Task Definitions):**
   * Guíame para crear un clúster en Amazon ECS llamado `innovatech-cluster`.
   * Redacta los archivos JSON de definición de tareas (`Task Definitions`) para cada uno de los 3 servicios bajo el modo de compatibilidad `FARGATE` (CPU 256, Memoria 512).
   * Asegúrate de configurar la observabilidad adjuntando el controlador de registros `awslogs` enviando flujos hacia **AWS CloudWatch** (`/ecs/innovatech-...`).
   * En los JSON de los backends, inyecta como variables de entorno la URL de conexión del endpoint de RDS, usuario y contraseña.

4. **Application Load Balancer (ALB) y Configuración en Tiempo de Ejecución (Runtime Config):**
   * **El gran problema del Cloud:** No queremos depender de direcciones IP públicas dinámicas en Fargate ni tener que adivinar qué IP tendrá el backend cada vez que se reinicie una tarea.
   * **La solución profesional (ALB):** Guíame para crear un **Application Load Balancer (ALB)** público en AWS que escuche en el puerto 80 y enrute el tráfico mediante reglas de ruta (*path-based routing*) hacia diferentes Target Groups:
     * Peticiones a `/` o assets estáticos → Target Group de `innovatech-frontend` (Puerto 80).
     * Peticiones a `/api/v1/ventas/*` → Target Group de `innovatech-ventas` (Puerto 8080).
     * Peticiones a `/api/v1/despachos/*` → Target Group de `innovatech-despachos` (Puerto 8081).
   * **Configuración Dinámica del Frontend (Runtime Config):** Modifica el código del frontend (utilizando un archivo `entrypoint.sh` en Nginx que genere un archivo estático `env-config.js` al iniciar el contenedor, o un mecanismo de variables globales de ventana `window.APP_CONFIG`). Esto permitirá que la imagen de Docker del frontend sea 100% agnóstica y reutilizable: en local se conectará al localhost, pero en la nube leerá la URL base del ALB como su variable de entorno sin necesidad de recompilar el código fuente en React.
   * *Evidencia que me pedirás:* Captura de los 3 Target Groups en AWS en estado *Healthy*, captura de las reglas de enrutamiento del ALB, y prueba en el navegador entrando únicamente al DNS del ALB y viendo la plataforma web cargando y comunicándose sin errores CORS ni IPs sueltas.

---

### FASE 3: AUTOMATIZACIÓN CI/CD CON GITHUB ACTIONS (Rúbrica IE3)
En esta fase automatizaremos todo el ciclo de pruebas, compilación, etiquetado de imágenes y despliegue continuo hacia AWS ECS.

1. **Gestión Segura de Secretos en GitHub:**
   * Explícame qué variables de entorno y secretos de AWS debo configurar en **GitHub Secrets** (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN`) bajo el principio de mínimo privilegio de IAM.
   * Recuérdame la nota importante sobre cómo actualizar estos secretos cada 4 horas cuando trabaje en el Learner Lab.
   * *Evidencia que me pedirás:* Captura de la interfaz de GitHub del repositorio en *Settings -> Secrets and variables -> Actions* mostrando los secretos configurados (sin revelar su contenido).

2. **Pipeline de Integración y Despliegue (`.github/workflows/deploy.yml`):**
   * Crea el archivo de automatización del flujo de trabajo en YAML configurado para ejecutarse automáticamente en cada `push` hacia la rama `main`.
   * Estructura el pipeline con trabajos (*jobs*) para cada microservicio que cumplan con las siguientes etapas obligatorias de la rúbrica:
     1. **Pruebas Automatizadas (Test):** Ejecutar los tests unitarios de Spring Boot en los backends (`./mvnw test`) utilizando la base de datos en memoria H2. Si una prueba falla, el pipeline debe abortarse automáticamente.
     2. **Compilación y Registro de Imágenes (Build & Push ECR):** Autenticar con Amazon ECR, construir las imágenes Docker de producción y subirlas utilizando un **doble etiquetado de versión para trazabilidad**: etiquetar con el commit SHA del repositorio (`${{ github.sha }}`) y con la etiqueta `latest`.
     3. **Despliegue Serverless en ECS (Deploy):** Utilizar las acciones oficiales `amazon-ecs-render-task-definition@v1` y `amazon-ecs-deploy-task-definition@v2` para actualizar la revisión de la Task Definition con la nueva imagen SHA y desplegar el servicio en AWS ECS Fargate, esperando dinámicamente hasta que el clúster confirme la estabilidad del servicio (*wait-for-service-stability: true*).
   * *Evidencia que me pedirás:* Captura de la pestaña *Actions* en GitHub mostrando la ejecución del workflow en verde (éxito total), captura de los logs internos del pipeline confirmando el pase exitoso de las pruebas unitarias (`BUILD SUCCESS`), y captura de Amazon ECR mostrando las nuevas etiquetas SHA subidas.

---

### FASE 4: VERIFICACIÓN FINAL, OBSERVABILIDAD Y DOCUMENTACIÓN (Rúbrica IE5, IE6 y Aspectos Formales)
En esta etapa final validaremos el sistema en producción, recopilaremos métricas y redactaremos la documentación formal.

1. **Pruebas de Integración en la Nube:**
   * Guíame para realizar pruebas funcionales de extremo a extremo entrando por la URL pública del ALB. Crearemos una orden de venta en el frontend, verificaremos su persistencia en RDS y consultaremos el despacho asociado.
   * *Evidencia que me pedirás:* Captura del navegador mostrando la interfaz en el DNS del ALB con registros creados exitosamente en la nube.

2. **Observabilidad en AWS CloudWatch:**
   * Indícame cómo ingresar a la consola de AWS CloudWatch para extraer dos tipos de evidencias críticas requeridas en la evaluación:
     * **Logs en tiempo real:** Flujos de registros de las aplicaciones Spring Boot y Nginx dentro de los grupos `/ecs/innovatech-...`.
     * **Métricas básicas de orquestación:** Gráficos de uso de CPU y Memoria de los servicios ejecutándose en las tareas Fargate del clúster ECS.
   * *Evidencia que me pedirás:* Capturas de pantalla de los paneles de CloudWatch Logs y Métricas mostrando actividad real del sistema.

3. **Redacción Técnica del Archivo `README.md`:**
   * Redacta un archivo `README.md` completo, formal y con lenguaje técnico académico de ingeniería (totalmente humanizado, **sin emojis** ni adornos artificiales de IA).
   * El README debe explicar de manera profesional: el contexto del ramo ISY1101, la arquitectura general del sistema, el diseño y justificación de los Dockerfiles multietapa, la estrategia de configuración en tiempo de ejecución (`runtime config`) para portabilidad entre local y nube, las instrucciones para clonar y levantar el proyecto en local con Docker Compose, el diagrama explicativo del flujo de automatización CI/CD con GitHub Actions y el enlace de demostración en vivo (DNS del ALB).
   * Explícame cómo incluir y referenciar un diagrama visual de arquitectura guardado en una carpeta `docs/diagrama-arquitectura.png` dentro del mismo repositorio.

---

## ¿CÓMO EMPEZAMOS?
Por favor, confirma que has leído y entendido todo este plan de automatización y, en especial, la **REGLA CRÍTICA DE PEDIRME EVIDENCIAS AL TERMINAR CADA PASO**.

Una vez confirmado, no me des toda la solución de golpe. **Inicia exclusivamente dándome las instrucciones claras y exactas para la FASE 1, Paso 1: Extracción del archivo RAR `C:\Users\Nacho\Downloads\ISY1101_EP2_Proyecto Semestral.rar`, preparación del directorio de trabajo y creación del repositorio Git.** ¡Estoy listo para empezar!
