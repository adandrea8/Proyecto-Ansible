_Apache_
=========

El rol apache, instala apache, abre los puertos en el firewall (lo habilita si es necesario), habilita el servicio de apache y lo levanta, indistintamente de cuál de las dos familias (Debian o RedHat) de sistemas operativos llamen al rol. 


Añadir rol a un playbook
----------------


      roles:
         - { role: apache }



Handlers
--------------

Procesos que acumula llamados para realizar un solo reinicio

```

- name: Reload firewalld
  service:
    name: firewalld
    state: reloaded
  when: ansible_os_family == 'RedHat'

- name: Restart apache
  service:
    name: httpd
    state: restarted
  when: ansible_os_family == 'RedHat'

- name: Reload ufw
  service:
    name: ufw
    state: reloaded
  when: ansible_os_family == 'Debian'

- name: Restart apache
  service:
    name: apache2
    state: restarted
  when: ansible_os_family == 'Debian'
    ```
