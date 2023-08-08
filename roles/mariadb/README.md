_Mariadb_
=========

El rol mariadb, instala mariadb-server, mariadb-client, python3 y PyMySQL. Abre el puerto 3306 en el firewall y por último habilita el servicio y lo inicia indistintamente de cuál de las dos familias (Debian o RedHat) de sistemas operativos llamen al rol. Luego configura mariadb para que escuche todas las interfaces, actualiza la clave clave root de la base de datos, elimina al usuario anónimo y la base de datos de test creada por defecto. 




Variables
--------------

En variables incluimos el archivo de contraseña para el root.

```bash
 include_vars:
   file: pass.yml
```


Añadir rol a un playbook
----------------


      roles:
         - { role: mariadb }



Handlers
--------------

Procesos que acumula llamados para realizar un solo reinicio

```
  - name: Reload firewalld
    service:
      name: firewalld
      state: reloaded
    when: ansible_os_family == 'RedHat'

  - name: Reload ufw
    service:
      name: ufw
      state: reloaded
    when: ansible_os_family == 'Debian'

  - name: Restart mariadb server
    service:
      name: mariadb
      state: restarted
    when: ansible_os_family == 'Debian'
    ```
