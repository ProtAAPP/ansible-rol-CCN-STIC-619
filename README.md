# ansible-rol-CCN-STIC-619

Este proyecto tiene como objetivo traducir los scripts que acompañan a la guía CN-STIC-619 a Ansible. No se trata de que desde Ansible se copie el script correspondiente y se ejecute, sino de traducir la funcionalidad a la estructura de Ansible, definiendo el estado final en el que ha de quedar el sistema, comprobando si se cumple o no, y realizar las acciones correctivas correspondientes.

## Cómo contribuir

Descarga este repositorio, crea una rama con los cambios a introducir y pide un PR (Pull Request) a, al menos, uno de los colaboradores para que verifique los cambios antes de integrarlos en el repositorio.

Recuerda que este rol de Ansible pretende seguir fielmente lo indicado en la guía y en el ENS, por lo que no propongas cambios que estén fuera de este marco. Tampoco se han de introducir cambios personalizados a un organismo en concreto. Para esto último se puede realizar un fork del repositorio e introducir en él esos cambios ad-hoc.

Los playbooks de Ansible deben estar preparados para ser lanzados periódicamente desde AWX, por ejemplo, al contrario que los scripts que están pensados para ser lanzados una única vez. Ten en cuenta esto al crear las tareas de los playbooks.

[Ansible-lint](https://ansible-lint.readthedocs.io/) es una herramienta que ayuda a que los playbooks creados sigan patrones y buenas prácticas de diseño. Úsala para verificar que los playbooks que creas sigan estas buenas prácticas.

## Requisitos

Es necesario que las máquinas sobre las que se ejecute el rol tengan python3 y el módulo pexpect instalados.

## Cómo usar el rol

1. Descarga el código del repositorio dentro de una carpeta dentro del directorio roles donde tengas la instalación de Ansible.
2. Crea un playbook que incluya el rol.
3. Ejecuta el playbook contra el inventario a bastionar indicando la etiqueta en función del nivel de categoría ENS:
   1. ens_basica
   2. ens_media
   3. ens_alta

Hay que tener en cuenta que el rol necesita de variables que han de ser definidas por quien vaya a ejecutar el playbook:
* grub_root_pass. La contraseña de ususario root a configurar en grub. Se recomienda que esa contraseña se almacene encriptada haciendo uso de Ansible Vault.
* root_pass (opcional). La contraseña del usuario root de cada máquina. El rol forzará el cambio de contraseña de root si no se cumplen los parámetros de seguridad indicados por la guía.
* reboot_test_command (opcional). Comando a ejecutar para corroborar que la máquina (o un servicio de la máquina) se ha reiniciado correctamente.

También el rol permite que se sobrescriban otras variables para adaptarse a las peculiaridades de cada máquina o grupo de máquinas:
* banner. El banner que mostrar en las máquinas bastionadas.
* usuarios_innecesarios. Lista de usuarios que no han de existir en la máquina.
* otros_usuarios_innecesarios. Lista de usuarios a eliminar junto con el contenido de su home y correo.
* grupos_innecesarios. Lista de grupos que no han de existir en la máquina.
* shell_usuarios_especificos. Lista de usuarios específicos con UID a los que asignar una shell en concreto diferente de bash.
* demonios_y_procesos_innecesarios. Demonios y procesos que no son necesarios en la máquina o grupos de máquinas.


A la hora de ejecutar el rol habrá que indicar el nivel de categoría del ENS que han de cumplir los hosts donde se aplicará el bastionado. Ejemplo:

```bash
ansible-playbook my_playbook_con_rol_CCN-STIC-619.yml -i hosts_categoria_media_ENS --tag ens_media
```
Habrá que añadir otras etiquetas en la ejecución del rol en los siguientes casos:
* `gnome`. En el caso de que el sistema de escritorio esté instalado.
* `portatiles`. En el caso de que se trate de un equipo portátil.

Ejemplo:
```bash
ansible-playbook my_playbook_con_rol_CCN-STIC-619.yml -i hosts_categoria_media_ENS --tag ens_media,gnome,portatiles
```
