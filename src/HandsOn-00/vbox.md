# Instal·lació i Configuració VirtualBox

1. Obrim Virtual Box i fem clic a **New**:

![Pas1](../HandsOn-00/figs/vbox/1.png)

2. Anomenem la màquina virtual **DebianLab_OS_GEI_VM** i seleccionem **Linux** com a sistema operatiu i **Debian (64-bit)** com a versió. Seleccioneu la RAM que voleu assignar a la màquina virtual (recomanem **4096 MB**). Finalment, *Crear un disc virtual ahora* i feu clic a **Crea**:

![Pas2](../HandsOn-00/figs/vbox/2.png)

3. Seleccioneu una ubicació per guardar el disc i també les opcions **VDI (VirtualBox Disk Image)** , **20 GB**, *Reservado dinámicamente* i feu clic a **Crear**:

![Pas3](../HandsOn-00/figs/vbox/3.PNG)

Un cop fet això, ja tenim la màquina virtual preparada.

![Pas4](../HandsOn-00/figs/vbox/4.PNG)

Ara necessitem configurar la màquina virtual perquè pugui arrancar amb la imatge ISO de Debian que ens hem descarregat. Seleccioneu la màquina virtual i feu clic a **Configuración**. Després, fes clic a **Almacenamiento** i selecciona **Unidad óptica** a l'esquerra. A la dreta, fes clic a la icona del disc al costat de l'opció *Controlador: IDE Secundario maestro*. A la finestra emergent, selecciona *Selecciona un disc òptic virtual* i selecciona la **imatge ISO de Debian que t'has descarregat**.

![Pas5](../HandsOn-00/figs/vbox/5.png)


Ara configurarem la xarxa. Ves a **Red**, selecciona **Adaptador 1**, assegurat de tenir **NAT** i fes clic *avanzadas*. 

![Pas6](../HandsOn-00/figs/vbox/6.PNG)

![Pas7](../HandsOn-00/figs/vbox/7.PNG)

Ara clicarem *Reenvío de puertos* i afegirem una nova regla. Aquesta configuració permetrà que la màquina virtual sigui accessible des de la màquina host a través de SSH. Afegiu la regla amb els parametres que es mostren a la imatge i cliqueu **Aceptar**.

![Pas8](../HandsOn-00/figs/vbox/8.PNG)

Finalment, feu clic a **Sistema**, seleccioneu **Placa base** i modifiqueu l'ordre d'arrencada perquè el primer dispositiu sigui el *Disco duro*. D'aquesta manera no us caldrà extreure la imatge *iso* un cop instal·lat el sistema operatiu.

![Pas9](../HandsOn-00/figs/vbox/boot.jpg)



