# cd501 "Introducción a la gestión de contenedores con Docker".

Notebooks de [Jupyter](https://jupyter.org) que incluyen los apuntes del curso cd501 "Introducción a la gestión de contenedores con Docker" de [Cloudevel®](https://cloudevel.com).

## Contenido.

| Notebook | Tema |
|----------|------|
| [00](00_preliminar.ipynb) | Preliminar: entorno del curso y credenciales |
| [01](01_introduccion_a_contenedores.ipynb) | Introducción a contenedores |
| [02](02_introduccion_a_docker.ipynb) | Introducción a Docker |
| [03](03_ejecucion_basica_de_contenedores.ipynb) | Ejecución básica de contenedores |
| [04](04_dockerfile.ipynb) | El archivo Dockerfile |
| [05](05_docker_container.ipynb) | El subcomando docker container |
| [06](06_gestion_de_imagenes.ipynb) | Gestión de imágenes |
| [07](07_volumenes_en_docker.ipynb) | Volúmenes en Docker |
| [08](08_redes_en_docker.ipynb) | Redes en Docker |
| [09](09_enjambres_y_servicios.ipynb) | Enjambres (swarms) y servicios de Docker |
| [10](10_creacion_de_un_registry_privado.ipynb) | Creación de un registry privado |
| [11](11_introduccion_a_yaml.ipynb) | Introducción a YAML |
| [12](12_docker_compose.ipynb) | Docker Compose |

## Entorno de ejecución.

El entorno principal de este curso es **GitHub Codespaces**. En `.devcontainer/` hay una configuración de [Dev Container](https://containers.dev/) con *Python* 3.12, *Jupyter* (*kernels* de *Python* y de *Bash*) y *Docker*; también puede abrirse con *Visual Studio Code* y *Docker Desktop* o *Docker Engine* en un equipo propio. No hace falta instalar nada más ni usar otro equipo.

* La imagen base, la *feature* *Docker-in-Docker* (con `devcontainer-lock.json`) y los paquetes de *Python* (`.devcontainer/requirements.txt`) están fijados por versión.
* *Docker-in-Docker* ejecuta un demonio (*daemon*) de *Docker* propio dentro del contenedor de desarrollo, que corre en **modo privilegiado**. Incluye los *plugins* `docker compose` y `docker buildx`.
* Los cuadernos se ejecutan en orden: varios usan imágenes, contenedores o archivos que crearon los anteriores.

### Puertos.

| Puerto | Uso |
|--------|-----|
| 8080 | Servidor web de los cuadernos 04 y 05 |
| 8081 | Servidor web del ejemplo de *Compose* (cuaderno 12) |
| 8082 | Servicio de *Swarm* (cuaderno 09) |
| 5000 | *Registry* privado (cuaderno 10) |

En *GitHub Codespaces*, la pestaña *Ports* (Puertos) de *Visual Studio Code* redirige estos puertos para abrirlos en el navegador.

### Diferencias con una instalación nativa de *Docker*.

* **Demonio propio.** Las imágenes, los contenedores, los volúmenes y las redes pertenecen al demonio interior y se pierden al eliminar el *Codespace*. No se comparten con el *Docker* del equipo propio.
* **Rutas de los *bind mounts*.** Una ruta como `$(pwd)/webroot` se resuelve en el sistema de archivos donde corre el demonio, es decir, dentro del contenedor de desarrollo. Las rutas del directorio del repositorio funcionan; las de otro equipo, no.
* **Puertos.** `-p 8080:80` abre el puerto dentro del entorno, no directamente en el equipo del estudiante; *Codespaces* o *Visual Studio Code* lo redirigen.
* **Redes.** El demonio interior tiene su propia pila de red, separada de la del equipo anfitrión.
* ***Swarm*.** Solo se practica un enjambre de un nodo. La red de enrutamiento de entrada (*ingress routing mesh*) usa redes *overlay* con *VXLAN* (*Virtual Extensible LAN*), que pueden no estar disponibles dentro de *Docker-in-Docker*; el cuaderno 09 publica el puerto en modo `host`.
* **Almacenamiento y rendimiento.** Las capas de las imágenes se guardan en el disco del *Codespace*: las descargas y las construcciones tardan más y el espacio es limitado (se recomiendan 32 GB).
* **Instalación de *Docker*.** El cuaderno 02 explica cómo instalar *Docker Engine* en *GNU/Linux*, pero esos pasos se practican solo en un equipo propio; en el *Codespace*, *Docker* ya está instalado.

### Credenciales.

Ningún ejercicio obligatorio necesita una cuenta. Los ejercicios opcionales de *Docker Hub* del cuaderno 06 leen el usuario y un *token* de acceso personal de las variables `DOCKERHUB_USER` y `DOCKERHUB_TOKEN`:

* en *GitHub Codespaces*, como secretos de *Codespaces* del repositorio;
* en un equipo propio, en un archivo `.env` copiado de `.env.example`.

La contraseña de *MySQL* del ejemplo de *Compose* (`src/11/docker-compose.yaml`) se toma de `src/11/.env` (véase `src/11/.env.example`); el cuaderno 12 crea ese archivo con una contraseña aleatoria si no existe. Los archivos `.env` están en `.gitignore`.

### Comprobación automática de los cuadernos.

El *workflow* `.github/workflows/notebooks.yml` construye el mismo *devcontainer* en *GitHub Actions*, con *Docker-in-Docker*, y ejecuta los cuadernos en orden con `scripts/ejecutar_cuadernos.py`. Las celdas que muestran un error a propósito llevan la etiqueta `raises-exception`; las opcionales de *Docker Hub* llevan `skip-execution`, porque la *CI* (integración continua) no usa credenciales.

Para ejecutar la misma comprobación dentro del *Codespace*:

``` bash
python scripts/ejecutar_cuadernos.py --omitir scripts/cuadernos_omitidos.txt
```

## Clonación del repositorio.

``` bash
git clone https://github.com/Cloudevel/cd501.git
```

## Licencia.

Esta obra está bajo una [Licencia Creative Commons Atribución 4.0 Internacional](https://creativecommons.org/licenses/by/4.0/).

&copy; José Luis Chiquete Valdivieso. 2017-2027.
