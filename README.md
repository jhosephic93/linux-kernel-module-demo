# Que son los modulos en Linux?

- Los modulos son la forma en como el kernel linux puede agregar funcionalidad y entre esas funcionalidades estan los drivers de hardwares.
- Un módulo del kernel es un fragmento de código o binarios que pueden ser cargado/deshabilitados y eliminados del kernel según las necesidades de este.
- El kernel de Linux es modular porque permite insertar y eliminar código bajo demanda con el fin de añadir o quitar una funcionalidad.
- Los módulos del kernel son archivos terminados con la extensión **.ko** que se almacenan en la ubicación **/lib/modules/versión_del_kernel.**

```console
$ uname -r  # Para ver el kernel quei estamos usando.
```

- Algunas de las funcionalidades que podemos añadir al núcleo mediante los módulos del kernel son las siguientes:

	1. Usar los drivers privativos de nuestra tarjeta gráfica.
		 - Ejem: Instalación de drivers como nvidia gpu para el cual se deshabilita el driver que viene por defecto en Linux en EC2 con GPU.
	2. Registrar las temperaturas de componentes de nuestro ordenador.
	3. Que los ventiladores de nuestro ordenador sean gestionados por el software 	creado por el fabricante de nuestro ordenador.
	4. Hacer funcionar nuestro tarjeta de red wifi.
	5. etc.

### Comandos Basicos

1. Install Linux Modules Extra on Raspberry Pi.
```console
$ sudo apt install linux-modules-extra-raspi
```
2. Location of Modules Linux Kernel.
```console
$ cd /lib/modules/$(uname -r)/
```
3. Lista  los módulos del kernel cargados actualmente.
```console
$ sudo lsmod
```
4. Agregamos un modulo en caso de no existir.
```console
$ sudo modprobe <nombre-del-módulo>
```
5. Borrar modulo
```console
$ sudo rmmod <nombre-del-módulo>
```
6. Mostrar información sobre un módulo.
```console
$ modinfo <nombre-del-módulo>
```
7. Listar las opciones que se establecen para un módulo cargado
```console
$ systool -v -m <nombre-del-módulo>
```
8. Links:
- [Para mayor informacion sobre Gestionar Modulos de Kernel Linux](https://geekland.eu/gestionar-modulos-del-kernel-linux/)
- [Para mayor informacion sobre Como cargar modulos del del kernel en Linux](https://www.zeppelinux.es/como-cargar-modulos-del-kernel-en-linux/)


## CREATE A VIRTUAL NETWORK INTERFACE

modprobe - Add or remove modules from the Linux Kernel

### OPCION01

1. Agregamos el modulo vcan en caso de no existir.
```console
$ sudo modprobe vcan
```
2. Crear una interface de red virtual.
```console
$ sudo ip link add dev vcan0 type vcan
```
3. Verificar la creacion del module en el kernel
```console
$ sudo lsmod | grep vcan
$ ifconfig -a
```
4. Ubicacion de los dispositivos virtuales de red.
```console
$ cd /sys/devices/virtual/net
```
5. Borrar modulo
```console
$ sudo rmmod vcan
```

### OPCION02

1. Agregamos el modulo dummy en caso de no existir.
```console
$ sudo modprobe dummy
```
2. Crear una interface de red virtual.
```console
$ sudo ip link add dev dummy00 type dummy
```
3. Verificar la creacion del module en el kernel
```console
$ sudo lsmod | grep dummy
$ ifconfig -a
```
4. Ubicacion de los dispositivos virtuales de red.
```console
$ cd /sys/devices/virtual/net
```
5. Borrar modulo
```console
$ sudo rmmod dummy
```

## DEMO - Carpeta linux-kernel-linux-demo

Install dependencies:
```
sudo apt install kmod gcc make
```

Compilation

```
make all
```

Test
```
make test
```

Insertar module en Linux Kernel
```
sudo insmod lkm_example.ko
```

Listar module en Linux Kernel, para verificar que se haya insertado dicho modulo

```
sudo lsmod | grep example
```

Run docker as priveled in order to grant access to all devices on host

```
$ docker run -ti --privileged --cap-add=ALL -v /dev:/dev ubuntu:18.04 bash
# cat /proc/1/status |grep Cap
# apt update && apt install kmod -y
# lsmod |grep lkm_example
# rmmod lkm_example
```


More info 
- https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities 
- https://phoenixnap.com/kb/docker-privileged