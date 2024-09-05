
# <center>Utilisation des machines virtuelles KVM</center>
#### <center>Auteur : ymz316</center>
#### <center>2022-05-12 17:50:00</center>
<br>

KVM (Kernel-based Virtual Machine) est une machine virtuelle basée sur le noyau (intégrée au noyau). Elle est généralement utilisée en conjonction avec QEMU pour créer des machines virtuelles, et l'extension des images de machines virtuelles est souvent .qcow2.

Ce qu'il faut savoir avant d'installer une machine virtuelle KVM :

>1. KVM lui-même n'émule aucun périphérique matériel, il utilise les capacités de virtualisation fournies par le matériel, telles que (Intel VT-x, AMD-V, extensions de virtualisation ARM, etc.), donc KVM **nécessite que la puce supporte et active la technologie de virtualisation** (extensions VT d'Intel ou extensions AMD-V d'AMD).
>2. KVM nécessite QEMU pour compléter l'émulation des périphériques matériels tels que la carte mère, la mémoire et les E/S, et il **faut installer certains paquets de gestion** pour atteindre l'objectif de création rapide de machines virtuelles.
>3. L'installation de la machine virtuelle nécessite l'utilisation du réseau, il faut donc d'abord **configurer les informations réseau** pour fournir un support de périphérique réseau pour l'installation de la machine virtuelle.
>4. Une fois les éléments ci-dessus préparés, vous pouvez **créer une machine virtuelle**. 

### I. Prérequis pour l'installation de KVM

**1) Vérifier si la virtualisation matérielle est supportée (extensions VT d'Intel ou extensions AMD-V d'AMD)**

```
$ LC_ALL=C lscpu | grep Virtualization
Virtualization:                  VT-x
```
Ou :
```
$ grep -Eoc '(vmx|svm)' /proc/cpuinfo   # ou egrep -c '(vmx|svm)' /proc/cpuinfo
16
```

Si la sortie est un nombre supérieur à zéro (c'est-à-dire le nombre de cœurs CPU), cela indique que le CPU supporte la virtualisation matérielle ; si la sortie est 0, cela indique que le CPU ne supporte pas la virtualisation matérielle, essayez de redémarrer et d'entrer dans les paramètres du BIOS pour activer la technologie VT.

Note : Le $ ici est l'invite du terminal, il ne faut pas le copier

**2) Déterminer si le serveur peut exécuter des machines virtuelles KVM avec accélération matérielle**

```
$ kvm-ok   # Si cette commande n'est pas trouvée, installez-la avec sudo apt install cpu-checker
INFO: /dev/kvm exists
KVM acceleration can be used
```

Note : Le $ ici est l'invite du terminal, il ne faut pas le copier

### II. Installation des paquets de gestion de virtualisation KVM

**Paquets de gestion de virtualisation liés à KVM**

>**qemu-kvm** : Logiciel fournissant l'émulation matérielle pour le gestionnaire KVM. Il faut installer ce paquet pour la version 20.04, la version 22.04 semble l'avoir intégré, à vérifier.  
**libvirt** : Ensemble de logiciels pour gérer les machines virtuelles et d'autres fonctionnalités de virtualisation (comme la gestion du stockage, la gestion du réseau). Il comprend une bibliothèque API, un démon (libvirtd) et un outil en ligne de commande (virsh). Il fournit une API commune pour les fonctionnalités courantes implémentées par les hyperviseurs supportés. L'objectif principal de libvirt est de fournir un ensemble d'API unifiées et fiables pour divers outils de virtualisation, permettant à la couche supérieure de gérer différentes technologies de virtualisation d'une manière unique, il peut opérer sur des hyperviseurs tels que KVM, VMware, XEN, Hyper-v, LXC, etc. Il faut installer le paquet **libvirt-daemon-system** pour avoir les fichiers de configuration pour exécuter le démon libvirt en tant que service système.  
**libvirt-clients** : Logiciel pour gérer les plateformes de virtualisation, généralement installé automatiquement lors de l'installation de libvirt-daemon-system.  
**virt-manager** : Outil GUI basé sur libvirt (interface utilisateur graphique).  
**virtinst** : Un ensemble d'outils en ligne de commande pour créer des machines virtuelles, généralement installé automatiquement lors de l'installation de virt-manager.  
**bridge-utils** : Outil en ligne de commande pour configurer les ponts Ethernet.  
**ovmf** : Firmware UEFI pour les machines virtuelles, nécessaire pour démarrer le système de la machine virtuelle à partir de l'UEFI.

La commande d'installation est :

```
sudo apt install qemu qemu-kvm libvirt-daemon-system virt-manager bridge-utils ovmf
```

### III. Configuration réseau de la machine cliente

Après l'installation de la machine cliente, il faut configurer son interface réseau pour permettre la communication réseau entre l'hôte et les machines clientes. Comme l'installation du système Linux nécessite une communication réseau, il faut configurer à l'avance la connexion réseau de la machine cliente.

Il existe deux méthodes de connexion réseau pour les machines clientes KVM :

- **Réseau utilisateur (User Networking)** : C'est-à-dire le mode NAT. C'est une méthode simple pour permettre à la machine virtuelle d'accéder aux ressources de l'hôte, d'Internet ou du réseau local, mais elle ne permet pas d'accéder à la machine cliente depuis le réseau ou d'autres machines clientes, et les performances nécessitent des ajustements importants.
- **Pont virtuel (Virtual Bridge)** : C'est-à-dire le mode Bridge (pont). Le mode Bridge permet à la machine virtuelle de devenir un hôte avec une IP indépendante dans le réseau, permettant ainsi à la machine cliente et aux machines du sous-réseau de communiquer entre elles. Cette méthode est un peu plus complexe que le réseau utilisateur, mais une fois configurée, la communication entre la machine cliente et Internet, ainsi qu'entre la machine cliente et l'hôte, est facile.

Prenons le NAT comme exemple d'apprentissage :

Entrez `sudo virt-manager` dans le terminal ou cliquez sur "Gestionnaire de système virtuel" dans le menu de démarrage pour ouvrir l'interface graphique de virt-manager.

![](../assets/KVM/virtmanager.png)

Cliquez sur "Édition" - "Détails de la connexion", entrez dans l'interface de configuration réseau et sélectionnez le réseau virtuel. Il y a par défaut une connexion réseau NAT nommée default, le nom du périphérique est virbr0, le segment de réseau est 192.168.122.0/24.

![](../assets/KVM/net.png)

Vous pouvez cliquer sur le signe plus en bas pour créer une nouvelle connexion réseau. Ici, nous créons une connexion réseau nommée network, sélectionnez "Tout périphérique physique" pour "forward to" (cela créera un périphérique réseau virbr1), le segment de réseau est 192.168.100.0/24, configurez le DHCP, cliquez sur Terminer pour réussir la configuration. À l'étape suivante, lors de l'installation de la machine virtuelle, vous pouvez définir ce mode de connexion réseau.

![](../assets/KVM/addnet.png)

### IV. Installation de la machine virtuelle cliente

L'installation de la machine virtuelle peut se faire via l'outil graphique virt-manager ou via la commande qemu-img.

**1. Installation avec l'outil graphique virt-manager**

Entrez `sudo virt-manager` dans le terminal ou cliquez sur "Gestionnaire de système virtuel" dans le menu de démarrage pour ouvrir l'interface graphique de virt-manager.

![](../assets/KVM/virtmanager.png)

1) Cliquez sur "Fichier" - "Créer une nouvelle machine virtuelle", ou cliquez sur l'icône de création de machine virtuelle, puis sélectionnez le support d'installation local et cliquez sur Suivant.

>Note : Si nous avons déjà copié une image de disque de machine virtuelle avec un système installé depuis Internet ou ailleurs, nous pouvons choisir "Importer une image de disque existante".

![](../assets/KVM/addstep1.png)

2) Sélectionnez la source d'installation (ISO) et le format du système de la machine virtuelle (comme Red Hat Linux 8), cliquez sur Suivant

![](../assets/KVM/addstep2.png)

3) Entrez la mémoire de la machine virtuelle, le nombre de cœurs CPU (ne peut pas être supérieur au nombre de cœurs de l'hôte), cliquez sur Suivant

![](../assets/KVM/addstep3.png)

4) Sélectionnez l'image de disque (l'installation de la machine virtuelle nécessite aussi un disque virtuel), ici nous créons un disque de 40G, cliquez sur Suivant

> Note : Le chemin par défaut de l'image de disque ici est /var/lib/libirt/images/*.qcow2, et si nous avons besoin d'utiliser une image de disque déjà créée ou si nous avons besoin de personnaliser l'emplacement de stockage de l'image de disque, nous pouvons choisir "Sélectionner ou créer un stockage personnalisé"

![](../assets/KVM/addstep4.png)

5) Entrez le nom de l'instance de la machine virtuelle, sélectionnez le réseau, cliquez sur Suivant.

> Si vous ne savez pas comment choisir le réseau, vous devrez peut-être revoir les connaissances sur les différents modes de réseau de machine virtuelle (NAT, pont, only_host).

![](../assets/KVM/addstep5.png)

6) À ce stade, nous entrons dans la phase d'installation du système de la machine virtuelle

![](../assets/KVM/ossetup.png)

Si vous obtenez l'erreur "Erreur de connexion à la console graphique : Error opening Spice console, SpiceClientGtk missing", essayez de résoudre avec la commande suivante :

```
hollowman@hollowman-F117:~$ apt search SpiceClientGtk
Tri... Fait
Recherche en texte intégral... Fait  
gir1.2-spiceclientgtk-3.0/focal 0.37-2fakesync1 amd64
  Widget GTK3 pour les clients SPICE (GObject-Introspection)

hollowman@hollowman-F117:~$ sudo apt install gir1.2-spiceclientgtk-3.0 
```

**2. Installation par commande**

**1) Créer une image de disque de machine virtuelle**

```
$ qemu-img create -f qcow2 diskname.qcow2 40G
$ ll -h diskname.qcow2   # Vérifier la taille de l'image sur le système hôte, bien qu'il y ait un disque de 40G, comme il n'y a pas de données, il n'occupe en réalité que 196K
-rwxrwxrwx 1 hollowman hollowman 196K 24 nov 13:30 diskname.qcow2*
```

**2) Installer la machine virtuelle (commande virt-install)**

a. Créer une machine virtuelle à partir d'une image ISO :

```
# virt-install --name nom_instance_cliente --memory taille_mémoire --vcpus nombre_cœurs_cpu --disk path=chemin_image_disque --cdrom=chemin_support_installation --network interface_réseau --cdrom=chemin_support_installation 
# Par exemple :
$ virt-install --name rhel8 --memory 2048 --vcpus 2 --disk path=./rhel8.qcow2 --cdrom=/media/hollowman/软件/ISO/rhel-8.0-x86_64-linuxprobe.com.iso --network bridge=virbr1
```

b. Créer une machine virtuelle avec une image de disque qcow existante

```
# virt-install --name nom_instance_cliente --memory taille_mémoire --vcpus nombre_cœurs_cpu --disk path=chemin_image_disque --network interface_réseau --import
# Par exemple :
$sudo virt-install --name openEuler2109 --memory 2048 --vcpus 2 --disk path=./openEuler-21.09-x86_64.qcow2 --network bridge=virbr1 --import
```

Les explications des options de virt-install peuvent être consultées via la commande :

```
$ virt-install help # Voici un extrait

Options générales :
  -n NAME, --name NAME             Nom de l'instance cliente
  --memory MEMORY                  Taille de la mémoire de la cliente. Par exemple : --memory 1024 (en MiB)
  --vcpus VCPUS                    Nombre de cœurs CPU de la cliente (pas plus que le nombre de cœurs de l'hôte). Par exemple : --vcpus 5

Options de méthode d'installation :
  --cdrom CDROM                    Support d'installation CD-ROM
  --import                         Construire la cliente dans une image de disque existante

Options OS :
  --os-variant OS_VARIANT          Spécifier le type de système d'exploitation. Par exemple : rhl8.0, peut être consulté avec la commande 'osinfo-query'

Options de périphérique :
  --disk DISK                      Spécifier diverses options de stockage. Par exemple : --disk size=10 (créer une image de 10 GiB à l'emplacement par défaut)
  -w NETWORK, --network NETWORK    Configurer l'interface réseau de la cliente. Par exemple : --network bridge=mybr0

```

### V. Traitement des problèmes rencontrés lors de la gestion des machines virtuelles

> 1. L'image qcow2 occupe trop d'espace

Si lors de la création d'une image de disque de 40G via l'interface graphique virt-manager, on découvre que l'espace de stockage sur l'hôte est de 40G, une occupation d'espace si importante, n'est-ce pas obsessionnel ? Nous pouvons le traiter par compression via une commande.

Voici comment nous vérifions les informations de l'image de disque créée par virt-manager :

```
$ qemu-img info rhel8.qcow2   # Vérifier les informations de l'image de disque
image: rhel8.qcow2
file format: qcow2
virtual size: 40 GiB (42949672960 bytes)     # Taille du disque virtuel
disk size: 196 KiB                           # Espace réellement occupé par l'image
cluster_size: 65536
Format specific information:
    compat: 1.1
    lazy refcounts: