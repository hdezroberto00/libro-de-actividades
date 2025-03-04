
```
Curso       : 202122
Area        : Sistemas operativos, automatización, aprovisionamiento, devops   
Descripción : Automatización de tareas con gestor de infraestructura "Ansible"
Requisitos  : 2 MV's con GNU/Linux, 1 MV con Windows
Tiempo      : 6 sesiones
```

# 1. Orquestación con Ansible

> Enlaces de interés:
> * [Automatización con Ansible. Una introducción](https://www.atareao.es/tutorial/ansible/)
> * [¿Qué es la orquestación de sistemas](https://www.redhat.com/es/topics/automation/what-is-orchestration)
> * [EN - Ansible Documentation](https://docs.ansible.com/ansible/latest/)
> * [EN - Red Hat Ansible Automation Platform](https://www.ansible.com/)

## 1.1 Entrega

* Entregar un informe tipo tutorial en formato Markdown por la plataforma GitHub.
* Entregar también los ficheros YAML que se vayan creando.
* Etiquetar las entrega (commit final) como `ansible`.

## 1.2 Crear las máquinas

> Te recuerdo que puedes hacer uso de [Vagrant](../../global/vagrant) para crear estas máquinas.

| MVs | Nombre   | Rol    | SO        | IP           |
| --- | -------- | ------ | --------- | ------------ |
| MV1 | masterXX | Master | GNU/Linux | 172.19.XX.51 |
| MV2 | slaveXXg | Slave  | GNU/linux | 172.19.XX.52 |
| MV3 | slaveXXw | Slave  | Windows10 | 172.19.XX.11 |

* Definir las máquinas "slaveXXg" y "slaveXXw" dentro del /etc/hosts de "masterXX".

> WARNING: Ansible requiere Python3.

# 2. Instalación de SSH y Ansible

# 2.1 Instalar Ansible

> Enlaces de interés: [EN - Installation Guide](https://docs.ansible.com/ansible/latest/installation_guide/index.html)

Consultar los siguientes vídeos:
* [Ansible 1 - ¿Qué es Ansible](https://youtu.be/slNIwBPeQvE)
* [Ansible 2 - Instalación](https://youtu.be/Zimn-UCbQ0A)

Ir a MV1:
* Instalar Ansible.
* Añadir al fichero de alias (`/home/nombre-alumno/.alias`) lo siguiente:
```
alias an='ansible'
alias anpb='ansible-playbook'
```

## 2.2 Acceso por SSH a las máquinas

* Instalar el servicio SSH en MV2 y MV3.
* El usuario "nombre-alumno" de la MV1 debe poder acceder por SSH sin clave a MV2. Recordar el uso de clave pública/privada de SSH. Esto lo hicimos en la actividad SSH de la unidad 1.

> Por ahora NO vamos a necesitar acceder por SSH a las máquinas Windows.

## 2.3. El inventario

El **inventario** es el conjunto de máquinas que tenemos declaradas en nuestra instalación de Ansible para informarle al sistema que son aquellas máquinas con las que vamos a trabajar.

Consultar los siguientes vídeos:
* [Ansible 3 - Inventario](https://youtu.be/VgnidinNlkQ)

Ir a la MV1:
* Crear un grupo dentro del inventario llamado "alumnoXX".
* Poner las máquinas "slaveXXg" y "slaveXXw" dentro del grupo.

# 3. Comandos ad-hoc

Los comandos ad-hoc se lanzan desde la terminal y permiten realizar tareas rápidas en un servidor. También pueden ser usados para lanzar instrucciones concretas usando módulos específicos de Ansible en un servidor.

Consultar los siguientes vídeos:
* [Ansible 4 - Comandos básicos ad-hoc](https://youtu.be/83DBL6CGNmY)
* [Ansible 5 - Comandos ad-hoc para controlar módulos](https://youtu.be/-MuOmvN1tgY)

Ir a la MV1:
* Usar Ansible para comprobar la conectividad con las máquinas del inventario.
* [Ansible - Module Index](https://docs.ansible.com/ansible/2.8/modules/modules_by_category.html)
* Usar Ansible para instalar 'neofetch' en las máquinas del inventario.
* Comprobar que el paquete está instalado.
* Volver a ejecutar el comando Ansible de instalación del mismo paquete y ver qué ocurre.

# 4. Playbook

La forma reutilizable de usar Ansible es escribiendo playbooks, que son archivos en los que declaramos las tareas que necesitamos hacer con nuestro servidor que vayamos a aprovisionar.

Consultar los siguientes vídeos:
* [Ansible 6 - Redactando un playbook](https://youtu.be/Wuv0ZPOMLf0)
* [Playbooks de Ansible - Atareado](https://atareao.es/tutorial/ansible/playbooks-de-ansible/)

## 4.1 Playbook ping

Ir a la MV1:
* Crear directorio `/home/nombre-alumno/ansibleXX.d`
* Moverse dentro de la directorio anterior.
* Crear el fichero `tarea-41-ping.yaml`.
* Dentro del fichero anterior define un playbook de Ansible con lo siguiente:
    * Aplicar a todos los hosts del inventario
    * Comprobar conectividad ping
* Comprobarlo.

## 4.2 Playbook install y uninstall

Ir a la MV1:
* Moverse al `/home/nombre-alumno/ansibleXX.d`
* Crear el fichero `tarea-42-install.yaml`.
* Dentro del fichero anterior define un playbook de Ansible con lo siguiente:
    * Aplicar al host GNU/Linux del inventario
    * Instalar el paquete `neofetch` y otros dos paquetes a elegir por el alumno.
* Comprobarlo.

Seguimos:
* Crear el fichero `tarea-43-uninstall.yaml`.
* Dentro del fichero anterior define un playbook de Ansible con lo siguiente:
    * Aplicar al host GNU/Linux del inventario
    * Desinstalar los paquetes instalados por la tarea 02.
* Comprobarlo.

# 5. Handlers

Vamos a practicar lo siguiente:
* Conectarse a una máquina remota como otro usuario mediante el uso de remote_user.
* Usar los handlers para disparar la ejecución de más tareas cuando una tarea previamente descrita en la sección tasks se ejecuta de forma satisfactoria provocando cambios.

Vídeos:
* [Ansible 7 - Conectarse como otro usuario](https://youtu.be/ggTS32i2JmA)
* [Ansible 8 - Handlers](https://youtu.be/G97sqHIG38w)

Ir a la MV1:
* Moverse al `/home/nombre-alumno/ansibleXX.d`
* Crear el fichero `tarea-51-apache-on.yaml`.
* Dentro del fichero anterior define un playbook de Ansible con lo siguiente:
    * Aplicar al host GNU/Linux del inventario
    * Definir `remote_user` con un usuario con privilegios de superusuario en las máquinas remotas.
    * Instalar Apache2
    * Iniciar el servicio Apache2. Configurar esta acción como un "Handler", de modo que se ejecuta cuando la acción anterior ha terminado bien.
* Comprobarlo.

Seguimos:
* Crear el fichero `tarea-52-apache-off.yaml`.
* Dentro del fichero anterior define un playbook de Ansible con lo siguiente:
    * Aplicar al host GNU/Linux del inventario.
    * Definir `remote_user` con un usuario con privilegios de superusuario en las máquinas remotas.
    * Para el servicio Apache2
    * Desinstalar Apache2. Configurar esta acción como un "Handler", de modo que se ejecuta cuando la acción anterior ha terminado bien.
* Comprobarlo.

# 6. Trabajar en grupo

**OJO: En este curso y para este apartado no vamos a usar la MV3x. Tendremos sólo el master en MV1a y los clientes MV2a, MV2b, MV2c, MV2d y MV2e. Para evitar los problemas de espacio en el disco duro de clase.**

## 6.1 Preparativos

* Formar un grupo con tus compañeros del mismo pasillo. Deben ser entre 4 a 6 personas.
* Supongamos que tenemos las siguientes máquinas
    * AlumnoA: MV1a, MV2a, MV3a
    * AlumnoB: MV1b, MV2b, MV3b
    * AlumnoC: MV1c, MV2c, MV3c
    * AlumnoD: MV1d, MV2d, MV3d
    * AlumnoE: MV1e, MV2e, MV3e
* Elegir a un coordinador. Por ejemplo AlumnoA. Ahora "MV1a" será el master para todas las demás.

## 6.2 Configurar SSH

* Ir a la MV1a.
* Crear usuario "supermaster". Crearle clave pública/privada.
* Ahora necesitamos configurar el SSH en las máquinas Windows.
* Permitir acceso por SSH al usuario "supermaster" a todas las máquinas MV2x y MV3x del grupo.

## 6.3 Configurar el inventario

Modificar el inventario de Ansible de la siguiente forma:
* Crear 2 grupos en el inventario: [gnulinux] y [windows]
* Poner en el grupo [gnulinux] a: MV2a, MV2b, MV2c, MV2d, MV2e.
* Poner en el grupo [windows] a: MV3a, MV3b, MV3c, MV3d, MV3e.

## 6.4 Crear playbook para GNU/Linux

Playbook:
* Crear un playbook `tarea-64-gnulinux.yaml`, y aplicarlo al grupo "gnulinux".
* Comprobarlo.

## 6.5 Crear playbook para Windows

> Enlace de interés: https://geekflare.com/es/connecting-windows-ansible-from-ubuntu/

Playbook:
* Crear un playbook `tarea-65-gnulinux.yaml`, y aplicarlo al grupo "gnulinux".
* [Crear un playbook `tarea62windows.yaml`, y aplicarlo al grupo "windows".](https://docs.ansible.com/ansible/latest/user_guide/windows_usage.html).
* Comprobarlo.

# ANEXO

Comandos de ansible:

```
ansible MV -m ping
ansible MV -m ping -i HOSTFILE
ansible MV -a 'echo "hola"'
ansible MV -m shell -a 'echo "hola"'
ansible MV -m apt -a "name=vim state=present"
ansible MV -m zypper -a "name=neofetch state=present"
ansible MV -m zypper -a "name=neofetch state=present" -b
ansible MV -m zypper -a "name=neofetch state=present" -b -k
```

https://docs.ansible.com/ansible/2.9/reference_appendices/interpreter_discovery.html

ansible-playbook FILE.yaml

## Ficheros de configuración de Ansible

```
ansible.cfg
HOME/.ansible.cfg
/etc/ansible/ansible.cfg
```

Contenido
```
[defaults]
remote_user = vagrant
```

## Clave privada con password
eval $(ssh-agent)
ssh-add

/etc/lsb-release
Muestra la versión del SO
