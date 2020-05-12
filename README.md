## DevOps Avanzado 
## CaaS Despliegue y a automatizacion en 30min.
 
Aqui encontraras un Servidor de Rancher con un cluster ya creado de k3s

## Requirementos

Debes instalar estos cliente para que todo sea exitoso


- [Git](https://gitforwindows.org/) 
- [VirtualBox](https://www.virtualbox.org)
- [Ansible](https://www.ansible.com/) Solo Linux 
- [Vagrant](https://www.vagrantup.com)


## Hardware recomendado :

- Sistema operativo windows o Linux
- 4 Threads o mas
- 8GB RAM
- 200GB espacio en disco.


## Deploy

0. Ejecuta `git clone https://github.com/mvaldivia90/rancher.git` 
0. Ejecuta en tu terminal `vagrant plugin install vagrant-disksize` (esto sirve para modificar el tamaño de los box)
0. Ejecuta `vagrant up`
	-Si ejecutas de Windows y tienes problemas en el despliege indicando bad interpreter debes instalar lo siguiente:
	- [dos2unix](https://sourceforge.net/projects/dos2unix/)
0. Espera a que los ambientes esten prepadors aprox 5 a 10 min.
0. Ejecuta un vagrant status para saber el estado de las VM, Resultado esperado Running.


## Acceso y actividades
 

Se han realizado modificaciones a los OS con el proposito de que la prueba de concepto sea totalmente funcional
- se extendio el disco del box rancherOS a 50GB
- Se crea particion de 26GB tipo linux
- Se  crea FS /storagenfs 
- Se habilitan servicio de red en rancherOS ( puede ser revisado ejecutando sudo ros service list)

0. Cuando provisionamiento termine debes entrar a la siguiente URL  [](http://172.22.101.101).

La contraseña por defecto es admin, pero esta puede ser actualizada en el archivo config.yaml, en este archivo se encuentra definido la cantidad de nodos y uso de recursos de nuestro cluster kubernetes.

0. Crear un proyecto 
0. Deploy apps NFS-provisioner y asignar el filesystem /storagenfs como host path
   Si el NFS queda en estado Pending de forma permanente se debe validar lo siguiente tanto en el server como los nodos desplegados:
   - Servicios de Red activos en rancherOS 
        sudo ros service enable kernel-extras
	sudo ros service enable kernel-headers
	sudo ros service enable kernel-headers-system-docker
	sudo ros service enable volume-nfs
	sudo ros service enable volume-cifs
   - Verificar si la particion /dev/sda2 esta montada en /storagenfs
   - Verificar Owner y permisos (rancher:rancher 775)
   - si no te permite montar el FS debes eliminar la particion 2 y ejeuctar en tu terminar fuera del server-01 vagrant reload.

0. Prueba de volumenes PVC 
   - Seleccionar el proyecto creado 
   - Seleccionar Workloads
   - Seleccionar Volumenes
   - Seleccionar Add Volume
   - Asignar nombre de volumen PVC
   - Seleccionar new persistent volume
   - Asignar el storage class creado 
   - Dar una capacidad de prueba en esta caso 1 GB
   - Para que el disco este en estado correcto debe mostrarse como BOUND




## Eliminacion de maquinas virtuales

Para eliminar todas las VM dejes ejecutar. `vagrant destroy -f`
