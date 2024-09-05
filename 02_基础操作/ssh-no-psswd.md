

# <center>Connexion SSH sans mot de passe</center>
#### <center>Auteur : jianfma</center>
#### <center>2023-04-02 9:25:00</center>

1. Assurez-vous que le service SSH est installé sur le serveur. S'il n'est pas installé, installez-le.
```bash
sudo apt install openssh-server
```
2. Configurez le fichier de configuration du service SSH. Assurez-vous que le contenu suivant n'est pas commenté.
```bash
sudo vim /etc/ssh/sshd_config

Trouvez le contenu suivant et supprimez le symbole de commentaire "#"
=========================
RSAAuthentication yes
PubkeyAuthentication yes
AuthorizedKeysFile  .ssh/authorized_keys
=========================
```
3. Configurez le fichier `.ssh/authorized_keys` de l'utilisateur SSH. Si `~/.ssh/authorized_keys` n'existe pas, créez le dossier .ssh et le fichier authorized_keys. Ajoutez le contenu de `id_rsa.pub` de l'ordinateur qui doit se connecter sans mot de passe à `~/.ssh/authorized_keys`.
```bash
cat id_rsa.pub >> ~/.ssh/authorized_keys
```
4. Configurez les permissions des fichiers.
```bash
chmod 600 ~/.ssh/authorized_keys 
#Définir les permissions du répertoire .ssh
chmod 700 -R ~/.ssh
```
5. Redémarrez le service sshd.
```bash
sudo service sshd restart
```

Annexe : Le chemin de `id_rsa.pub` de l'ordinateur qui doit se connecter sans mot de passe est généralement `~/.ssh/id_rsa.pub`.
S'il n'existe pas, il faut le générer. La commande généralement utilisée pour générer une paire de clés SSH publique/privée est `ssh-keygen -t rsa -C adresse_email`. La commande `ssh-keygen` est installée par défaut sur Windows 11, Linux et macOS, et peut être utilisée directement.