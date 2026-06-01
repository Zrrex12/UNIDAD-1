# Automatización de Infraestructura Digital – Unidad I
## Entornos de Desarrollo en la Automatización de Redes

---

**Institución:** Universidad Tecnologicas del Norte de Guanajuato 
**Carrera:** Ing. Redes Inteligentes y Ciberseguridad  
**Grupo:** GIRI6091  
**Unidad:** Unidad I – Entornos de desarrollo en la automatización de redes  
**Nombre:** Rafael Robles González
**Núm. Control:** 1223100473  
**Fecha:** Junio 2025  

---

## Índice

1. [Introducción](#introducción)
2. [Desarrollo](#desarrollo)
   - [Descripción de Herramientas](#descripción-de-las-herramientas)
     - [Docker Engine](#docker-engine)
     - [Docker Compose](#docker-compose)
     - [Docker Swagger](#docker-swagger)
   - [Procedimiento de Instalación](#procedimiento-de-instalación)
     - [VSCode y Plugins](#vscode-y-plugins)
     - [Docker](#docker)
     - [Git](#git)
   - [Evidencia de Pruebas de Verificación](#evidencia-de-pruebas-de-verificación)
3. [Conclusión](#conclusión)
4. [Bibliografía](#bibliografía)

---

## Introducción

El presente reporte documenta el proceso de configuración de un entorno de desarrollo orientado a la automatización de infraestructura de redes. En el contexto actual de las tecnologías de la información, la automatización representa un pilar fundamental para la gestión eficiente de redes y sistemas, permitiendo reducir errores humanos, optimizar tiempos de despliegue y garantizar la reproducibilidad de entornos de trabajo.

Para llevar a cabo esta práctica se instalaron y configuraron herramientas clave ampliamente utilizadas en la industria: Docker Engine, Docker Compose, Docker Swagger, Visual Studio Code con sus extensiones respectivas, y Git como sistema de control de versiones. Estas herramientas, en conjunto, conforman un ecosistema robusto que facilita el desarrollo, prueba y despliegue de aplicaciones y servicios de red en contenedores.

Docker permite empaquetar aplicaciones junto con todas sus dependencias en unidades portables llamadas contenedores, lo que elimina el clásico problema de "funciona en mi máquina". Docker Compose extiende esta capacidad al permitir la orquestación de múltiples contenedores mediante archivos de configuración declarativos en formato YAML. Swagger, por su parte, es una herramienta esencial para el diseño y documentación de APIs REST, integrándose con facilidad en entornos Docker.

A lo largo de este reporte se describe de manera detallada cada una de las herramientas utilizadas, el procedimiento técnico de instalación paso a paso, y las pruebas de verificación realizadas para comprobar el correcto funcionamiento del entorno. Asimismo, se incluye una conclusión con los principales hallazgos y aprendizajes obtenidos durante el desarrollo de la unidad.

---

## Desarrollo

### Descripción de las Herramientas

#### Docker Engine

Docker Engine es el motor de contenedores de código abierto que permite crear, ejecutar y gestionar contenedores en un sistema operativo anfitrión. Utiliza tecnologías del kernel de Linux como `namespaces` y `cgroups` para aislar procesos, garantizando que cada contenedor opere de forma independiente. Docker Engine se compone de tres elementos principales:

- **Docker Daemon (`dockerd`):** Proceso en segundo plano que gestiona objetos Docker como imágenes, contenedores, redes y volúmenes.
- **Docker CLI:** Interfaz de línea de comandos que permite al usuario interactuar con el daemon mediante comandos como `docker run`, `docker build` o `docker ps`.
- **Docker API REST:** Interfaz programática que permite a otras herramientas comunicarse con el daemon.

En el contexto de automatización de redes, Docker Engine es fundamental porque permite desplegar herramientas de red (como Ansible, Netmiko o NAPALM) en contenedores aislados y reproducibles.

#### Docker Compose

Docker Compose es una herramienta que permite definir y ejecutar aplicaciones multi-contenedor a través de un archivo de configuración YAML (`docker-compose.yml`). En lugar de ejecutar múltiples comandos `docker run` de forma manual, Compose permite describir todos los servicios, redes y volúmenes de una aplicación en un solo archivo y levantarlos con un único comando.

Comandos básicos de Docker Compose:

```shell
# Iniciar contenedores definidos en un archivo docker-compose.yml
docker-compose up

# Iniciar contenedores en segundo plano (modo detached)
docker-compose up -d

# Detener y eliminar contenedores
docker-compose down

# Ver logs de servicios en Docker Compose
docker-compose logs servicio

# Listar contenedores activos del proyecto
docker-compose ps
```

Docker Compose es especialmente útil en la automatización de infraestructuras, ya que permite replicar entornos completos (con base de datos, backend, frontend y herramientas de red) de manera consistente en cualquier equipo o servidor.

#### Docker Swagger

Swagger (actualmente conocido como OpenAPI) es una especificación y conjunto de herramientas para describir, producir, consumir y visualizar servicios web RESTful. En entornos Docker, Swagger UI puede desplegarse como un contenedor independiente que expone una interfaz gráfica para interactuar con APIs documentadas.

Su integración en proyectos de automatización de redes permite:

- Documentar endpoints de APIs REST de gestión de red.
- Probar peticiones HTTP directamente desde el navegador.
- Generar clientes y servidores automáticamente a partir de la especificación.

Ejemplo de despliegue de Swagger UI con Docker:

```shell
docker run -p 8080:8080 swaggerapi/swagger-ui
```

---

### Procedimiento de Instalación

#### VSCode y Plugins

Visual Studio Code es el editor de código elegido por su flexibilidad y amplio ecosistema de extensiones.

**Instalación en Linux (distribución basada en Arch/Manjaro):**

```shell
# Instalar VSCode desde el gestor de paquetes (AUR)
yay -S visual-studio-code-bin

# Verificar instalación
code --version
```

**Extensiones instaladas:**

| Extensión | Descripción |
|---|---|
| Docker (Microsoft) | Administración de contenedores desde VSCode |
| YAML (Red Hat) | Soporte y validación de archivos YAML |
| GitLens | Visualización avanzada de historial Git |
| Remote - Containers | Desarrollo dentro de contenedores Docker |
| Python | Soporte para scripts de automatización |

**Instalación de extensiones por CLI:**

```shell
code --install-extension ms-azuretools.vscode-docker
code --install-extension redhat.vscode-yaml
code --install-extension eamodio.gitlens
```

#### Docker

**Instalación de Docker Engine en Linux Mint:**

```shell
# Actualizar repositorios del sistema
sudo apt update && sudo apt upgrade -y

# Instalar Docker
sudo apt install -y docker.io

# Habilitar e iniciar el servicio de Docker
sudo systemctl enable docker
sudo systemctl start docker

# Agregar el usuario actual al grupo docker (evitar usar sudo)
sudo usermod -aG docker $USER

# Cerrar sesión y volver a iniciar para aplicar cambios de grupo
# Verificar instalación
docker --version
```

**Salida esperada:**
```
Docker version 26.x.x, build xxxxxxx
```
<img src="imagenes/docker-version.png" alt="Docker versión" width="600">

**Instalación de Docker Compose:**

```shell
# Instalar Docker Compose (plugin incluido en Docker Desktop o standalone)
sudo apt install -y docker-compose

# Verificar instalación
docker-compose --version
```

**Salida esperada:**
```
Docker Compose version v2.x.x
```

#### Git

Git es el sistema de control de versiones distribuido utilizado para gestionar el código fuente del proyecto.

**Instalación en Linux Mint:**

```shell
# Instalar Git
sudo apt install -y git

# Verificar instalación
git --version
```

**Salida esperada:**
```
git version 2.x.x
```
<img src="imagenes/git-version.png" alt="Git versión" width="600">

**Configuración inicial de Git:**

```shell
# Configurar nombre de usuario
git config --global user.name "Tu Nombre"

# Configurar correo electrónico
git config --global user.email "tu@correo.com"

# Verificar configuración
git config --list
```

**Clonar el repositorio del proyecto:**

```shell
git clone https://github.com/edomenzain/automatizacion-redes
cd automatizacion-redes
```

---

### Evidencia de Pruebas de Verificación

#### 1. Verificación de Docker con imagen `hello-world`

Para confirmar que Docker Engine se instaló correctamente, se ejecuta la imagen oficial de prueba `hello-world`:

```shell
docker run hello-world
```

**Salida esperada en terminal:**

```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
...
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.
```
<img src="imagenes/docker-hello-world.png" alt="Docker hello-world" width="600">

#### 2. Verificación de Docker Compose con archivo `.yml`

Se crea un archivo `docker-compose.yml` de prueba para verificar el funcionamiento de Docker Compose:

```yaml
version: '3.8'

services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    container_name: nginx-prueba
```

**Ejecución del archivo YML:**

```shell
# Levantar los servicios definidos en el archivo
docker-compose up -d

# Verificar que los contenedores estén corriendo
docker-compose ps

# Ver los logs del servicio web
docker-compose logs web
```

**Salida esperada de `docker-compose ps`:**

```
NAME            IMAGE          COMMAND                  SERVICE   CREATED         STATUS         PORTS
nginx-prueba    nginx:alpine   "/docker-entrypoint.…"   web       X seconds ago   Up X seconds   0.0.0.0:8080->80/tcp
```

**Verificación en navegador:** Acceder a `http://localhost:8080` debe mostrar la página de bienvenida de Nginx.

<img src="imagenes/nginx-localhost.png" alt="Nginx en navegador" width="600">

**Detener y limpiar contenedores de prueba:**

```shell
docker-compose down
```

---

## Lista de Verificación del Entorno de Desarrollo

| Herramienta | Instalada | Verificada | Versión |
|---|---|---|---|
| Docker Engine | ✅ | ✅ | 26.x.x |
| Docker Compose | ✅ | ✅ | 2.x.x |
| Git | ✅ | ✅ | 2.x.x |
| Visual Studio Code | ✅ | ✅ | 1.x.x |
| Extensión Docker (VSCode) | ✅ | ✅ | - |
| Extensión YAML (VSCode) | ✅ | ✅ | - |
| Imagen hello-world ejecutada | ✅ | ✅ | - |
| Archivo .YML ejecutado con Compose | ✅ | ✅ | - |

---

## Conclusión


A lo largo de esta primera unidad de Automatización de Infraestructura Digital, se llevó a cabo la instalación y configuración de un entorno de desarrollo completo orientado a la automatización de redes. El proceso permitió comprender de manera práctica cómo herramientas como Docker Engine, Docker Compose y Git se integran para formar un flujo de trabajo eficiente y reproducible.

Uno de los hallazgos más importantes fue la facilidad con la que Docker permite aislar entornos de ejecución mediante contenedores, eliminando los conflictos de dependencias que son comunes en instalaciones directas sobre el sistema operativo. La ejecución de la imagen `hello-world` confirmó el correcto funcionamiento del motor de Docker, mientras que la ejecución del archivo `.yml` mediante Docker Compose demostró la capacidad de orquestar múltiples servicios desde un único archivo declarativo.

Asimismo, la integración de Visual Studio Code con las extensiones de Docker y YAML proporcionó una experiencia de desarrollo más fluida, permitiendo gestionar contenedores y validar archivos de configuración directamente desde el editor. Git, por su parte, resultó ser una herramienta indispensable para el versionamiento del código y la colaboración en proyectos de automatización.

En conclusión, el entorno de desarrollo configurado en esta unidad constituye una base sólida sobre la cual se pueden construir proyectos de automatización de infraestructura de red más complejos, aplicando principios de Infrastructure as Code (IaC) y DevOps que son ampliamente demandados en la industria actual.

---

## Anexo – Recursos de la Comunidad de Colaboración

| Recurso | URL |
|---|---|
| Repositorio base del proyecto | https://github.com/edomenzain/automatizacion-redes |
| Docker Hub – Imágenes oficiales | https://hub.docker.com/ |
| Documentación oficial de Docker | https://docs.docker.com/ |
| Documentación de Docker Compose | https://docs.docker.com/compose/ |
| Swagger UI en Docker Hub | https://hub.docker.com/r/swaggerapi/swagger-ui |
| Documentación de Git | https://git-scm.com/doc |
| Extensiones de VSCode para Docker | https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker |
| Stack Overflow – Comunidad Docker | https://stackoverflow.com/questions/tagged/docker |

---

## Bibliografía

Bell, P. (2014). *Introducing GitHub: A non-technical guide*. O'Reilly Media.

Docker Inc. (2024). *Docker documentation*. https://docs.docker.com/

Gift, N., Behrman, K., Deza, A., & Gheorghiu, G. (2019). *Python for DevOps: Learn ruthlessly effective automation*. O'Reilly Media.

Hillar, G. C. (2016). *Building RESTful Python web services*. Packt Publishing.

Jackson, C., Borsetti, N., Khatri, H., & Quevedo, J. (2020). *Cisco Certified DevNet Associate DEVASC 200-901 official cert guide*. Cisco Press.

Lenz, M. (2018). *Python continuous integration and delivery: A concise guide with examples*. Apress.

Merkel, D. (2014). Docker: Lightweight Linux containers for consistent development and deployment. *Linux Journal*, *2014*(239), 2.

SmartBear Software. (2024). *Swagger documentation*. https://swagger.io/docs/

Tsitoara, M. (2019). *Beginning Git and GitHub: A comprehensive guide to version control, project management, and teamwork for the new developer*. Apress.