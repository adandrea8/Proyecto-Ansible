
# Taller Julio 2023 - ASLX 
Este es un repositorio donde contiene información sobre como utilizar Ansible para las tareas planteadas por el obligatorio de el taller de Linux

Ansible es una herramienta de automatización que permite gestionar la configuración y el despliegue de servidores de manera eficiente y reproducible.

### Para utilizar este repositorio

Clonar el proyecto

```bash
  git clone git@github.com:adandrea8/tallerjulio2023.git
```

Set branch

```bash
  git branch -M main
```
Traer ultimos cambios

```bash
  git pull
```

Enviar cambios

```bash
  git push -u origin main
```

Ver modificaciones

```bash
  git log
```

## Instalación

Para utilizar Ansible, primero debemos instalarlo en nuestra máquina bastion:

```bash
pip install ansible
```

El archivo `ansible.cfg` lo creamos de la siguiente manera:

```bash
ansible-config init --disabled > ansible.cfg
```


## Inventario
Creamos un archivo inventario para agregar los hosts y grupos de hosts a utilizar 

Dentro del archivo `ansible.cfg` indicamos que vamos a utilizar nuestro inventario.

```bash
inventory=./inventario
```
En inventario tenemos dos grupos y un supergrupo que incluye a ambos:

```bash
[redhat]
rocky.taller.uy	ansible_host=192.168.56.103

[debian]
ubuntuserver.taller.uy	ansible_host=192.168.56.102

[linux:children]
redhat
debian 
```

## Var

En var tenemos las variables creadas para nuestros playbooks:

## Roles

En el directorio `/roles`, creamos todos los roles que utilizaremos para nuestros playbooks:

- **apache** : 
  - Instalar apache, abrir puertos http, https, iniciar y habilitar el servicio.
- **mariadb :**
  - Instalar mariadb, abrir puerto 3306 en el firewall y iniciar y habilitar el servicio.
- **podman:**
  - Instalar podman, iniciar y habilitar el servicio.
- **firewall.debian**


## files
En el directorio `/files`, creamos y cargamos todos los archivos para nuestros playbooks:

- app.properties
- Dockerfile
- todo.war
- index.j2
- virtualhost.conf


## PB_updates.yml

El playbook `PB_updates.yml` actualizará todos los paquetes en los servidores.

```bash
ansible-playbook PB_updates.yml
```


## PB_Web_Server.yml

El playbook `PB_Web_Server.yml` instalara apache con proxy reverso en los servidores Rocky.

```bash
ansible-playbook PB_Web_Server.yml
```

## PB_app_web.yml

El playbook `PB_app_web.yml`, instalara podman y creara una aplicacion "todo.war" en un contenedor con podman.

```bash
ansible-playbook PB_app_web.yml
```


## Soporte

Por soporte, email a alexisxdandrea@hotmail.com o mensaje a [nicolasemm](https://github.com/nicolasemm)


## Autores

- [Alexis D'Andra](https://www.github.com/adandrea8)
- [Nicolas Martins](https://github.com/nicolasemm)

