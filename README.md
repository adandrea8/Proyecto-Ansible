# _Taller Julio 2023 - ASLX_ 
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


## Roles

En el directorio `/roles`, creamos todos los roles que utilizaremos para nuestros playbooks:

- _**apache** :_
  - Instalar apache, abrir puertos http, https, iniciar y habilitar el servicio.
- _**mariadb :**_
  - Instalar mariadb, abrir puerto 3306 en el firewall y iniciar y habilitar el servicio.
- _**podman:**_
  - Instalar podman, iniciar y habilitar el servicio.
- _**firewall.debian:**_
    - Habilita el firewall y crea reglas para que permita el trafico saliente y deshabilita el tráfico entrante.


_Dentro de los roles existen archivos secundarios como los handlers y variables cuales nos facilitan con la optimizacion del codigo._


## files
En el directorio `/files`, creamos y cargamos todos los archivos para nuestros playbooks:

- app.properties
- Dockerfile
- todo.war
- index.j2
- virtualhost.conf
- statement_todo.sql


## PB_updates.yml

El playbook `PB_updates.yml`, actualizará todos los paquetes instalados en los servidores.

```bash
ansible-playbook PB_updates.yml
```


## PB_Web_Server.yml

El playbook `PB_Web_Server.yml`, instalara apache con proxy reverso en los servidores Rocky.

```bash
ansible-playbook PB_Web_Server.yml
```

## PB_app_web.yml

El playbook `PB_app_web.yml`, instalara podman y creara una aplicacion "todo.war" en un contenedor con podman.

```bash
ansible-playbook PB_app_web.yml
```

## PB_mariadb.yml

El playbook `PB_app_web.yml`, realizara todas las configuraciones necesarias para el funcionamiento de la aplicacion web. Entre ellas: Creacion de usuario, contraseña, base de datos, tablas y permisos.

Al usar archivos sensibles como lo son las contraseñas de root y usuario para la app web, estaran cifrados por vault.
Por eso es necesario que a la hora de ejecutar el playbook, nos pida la contraseña de los archivos. 

```bash
 ansible-playbook PB_mariadb.yml --ask-become-pass --ask-vault-pass
```


## Soporte

Por soporte, email a alexisxdandrea@hotmail.com o mensaje a [nicolasemm](https://github.com/nicolasemm)


## Autores

- [Alexis D'Andra](https://www.github.com/adandrea8)
- [Nicolas Martins](https://github.com/nicolasemm)


