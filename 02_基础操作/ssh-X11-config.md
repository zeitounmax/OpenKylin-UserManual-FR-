
# Configuration du proxy X11 pour SSH

## Contexte
La fonction X11 Forwarding de SSH offre principalement une solution permettant d'exécuter certains programmes GUI du serveur sur le client local, plutôt que des programmes CLI, permettant ainsi un développement plus efficace du code correspondant.
Un exemple simple est la configuration d'un xclock sur un serveur distant et son exécution en local.

## Configuration

### Machine de développement distante
Activez le X11 Forwarding sur le serveur distant. Dans le fichier de configuration d'OpenSSH `/etc/ssh/sshd_config`, ouvrez les deux lignes suivantes :
```sh
AllowTcpForwarding yes
X11Forwarding yes
```
Ensuite, exécutez :
```sh
systemctl restart ssh.service #Debian
systemctl restart sshd.service #Autres Linux
```
Redémarrez le service sshd, la configuration du serveur distant est alors terminée.

### Configuration locale
Dans l'environnement local, modifiez le fichier de configuration d'OpenSSH-Client `/etc/ssh/ssh_config`, ouvrez les trois lignes suivantes :
```sh
ForwardAgent yes
ForwardX11 yes
ForwardX11Trusted yes
```
Ensuite, exécutez :
```sh
systemctl restart ssh.service #Debian
systemctl restart sshd.service #Autres Linux
```
Redémarrez le service sshd, la configuration est alors terminée.

## Exemple - Configuration de xclock
Comme mon serveur distant est un système Fedora, j'exécute la commande suivante pour installer xclock.
```sh
sudo dnf install -y xclock
```
Ensuite, exécutez `xclock`, vous devriez voir l'interface GUI de l'horloge, indiquant que la configuration a réussi.
![clock](../assets/ssh-X11代理配置/clock.png)

## Références
Cet article a été rédigé en se référant au site web suivant :
[https://www.jianshu.com/p/24663f3491fa](https://www.jianshu.com/p/24663f3491fa)
```
