# tallerjulio2023

## inventario
### Para la habilitacion del inventario, agregamos la siguiente linea a ansible.cnf:
inventory=./inventario

### En inventario tenemos dos grupos y un supergrupo que incluye a ambos:
[redhat]
rocky.taller.uy	ansible_host=192.168.56.103

[debian]
ubuntuserver.taller.uy	ansible_host=192.168.56.102

[linux:children]
redhat
debian

## var

### En var tenemos las variables creadas para nuestros playbooks:

##roles

### En el directorio roles, creamos todos los roles que utilizaremos para nuestros playbooks:
apache
mariadb
podman
tomcat

## files

### En el directorio files, creamos y cargamos todos los archivos para nuestros playbooks:
app.properties
index.j2
todo.war
virtualhost.conf

## PB_updates.yml

### El playbook PB_updates.yml actualizará todos los servidores:
---
- hosts: linux
  become: yes
  remote_user: sysadmin

  tasks:

  - name: Update all available packages for RedHat servers
    yum:
      name: "*"
      state: latest
    notify: Restart server
    when: ansible_os_family == "RedHat"

  - name: Update all available packages for Debian servers
    apt:
      name: "*"
      state: latest
      update_cache: yes
    notify: Restart server
    when: ansible_os_family == "Debian" 


  handlers:

  - name: Restart server
    reboot:

## PB_Web_Server.yml

### El playbook PB_Web_Server.yml instalara apache con proxy reverso en los servidores Rocky
---
- hosts: redhat
  become: yes
  remote_user: ansible

  roles:

     - role: apache

  tasks:

  - name: Crear el virtualhost con la configuracion del directorio
    file:
      path: /etc/httpd/vhosts.d
      state: directory
      owner: ansible

  - name: Copiar virtualhost de configuracion
    copy:
      src: ./files/virtualhost.conf
      dest: /etc/httpd/vhosts.d/web.conf
    notify: Restart apache


  - name: Include vhosts directory
    lineinfile:
      path: /etc/httpd/conf/httpd.conf
      line: IncludeOptional /etc/httpd/vhosts.d/*.conf  
 

  - name: Crear el directorio para index
    file:
      path: /var/www/web/html
      state: directory
      owner: ansible


  - name: Copiar archivo index
    template:
      src: ./files/index.j2
      dest: /var/www/web/html/index.html
      owner: apache
      group: apache
      mode: '644'

  handlers:

  - name: Restart apache
    service:
      name: httpd
      state: restarted

## PB_app_web.yml

### El playbook PB_app_web.yml, instalara podman y creara una aplicacion en su contenedor
---
- name: Instalar y configurar Tomcat en Podman 
  hosts: redhat
  become: yes

  roles:
    - role: podman

  tasks:

    - name: Crear directorio para el contenedor
      file:
        path: /ansible/contenedor/appweb
        state: directory

    - name: Copiar todo.war a la ruta
      copy:
        src: ./files/todo.war
        dest: /ansible/contenedor/appweb/todo.war

    - name: Copiar app.properties a la ruta
      copy:
        src: ./files/app.properties
        dest: /ansible/contenedor/appweb/app.properties
        


    - name: Crear imagen y configurar el contenedor con Podman
      podman_image:
        name: appweb
        build:
          context: /ansible/contenedor/appweb/
          dockerfile:
            FROM tomcat:9.0
            RUN mkdir /opt/config
            COPY app.properties /opt/config
            COPY todo.war /usr/local/tomcat/webapps/
            EXPOSE 8080
            CMD ["catalina.sh", "run"]
        state: present


    - name: Configurar y ejecutar
      containers.podman.podman_image:
        name: "appweb"
        path: '/opt/config'
        build:
         format: docker
        state: present
      register: results
