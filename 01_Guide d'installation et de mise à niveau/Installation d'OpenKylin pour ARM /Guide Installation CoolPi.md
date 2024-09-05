# Guide Installation pour CoolPi

## I. Téléchargement de l'image

Téléchargez via le lien suivant :
https://www.openkylin.top/downloads
Après le téléchargement, faites un clic droit pour décompresser et obtenir le fichier .img. Les fichiers image avec l'extension .xz peuvent être utilisés directement sans décompression.

## II. Gravure de l'image

Installation du programme de gravure : https://www.raspberrypi.com/software/
![Adresse d'installation du programme de gravure arm64](./assets/在arm设备上安装_common/arm64烧录程序安装地址.png)

Insérez la carte SD, ouvrez rpi-imager, sélectionnez une image personnalisée, puis choisissez le fichier image .img installé
![Sélection de l'image dans le programme de gravure arm64](./assets/在arm设备上安装_common/arm64烧录程序镜像选择.png)

Sélectionnez la carte SD, cliquez sur WRITE, et attendez que la création soit terminée
![Sélection de la carte SD dans le programme de gravure arm64](./assets/在arm设备上安装_common/arm64烧录程序SD卡选择.png)

## III. Démarrage d'openKylin sur Coolpi

Connectez le câble d'alimentation et le câble d'affichage de la carte de développement Coolpi, branchez le clavier et la souris, et insérez la carte SD
![Méthode de câblage de Coolpi arm64](./assets/在coolpi上安装openKylin/arm64coolpi接线方式.png)

Au démarrage de Coolpi, il peut y avoir un écran noir continu ou un démarrage soudain d'un système d'exploitation Kylin problématique (ceci est dû à une erreur de chargement de la carte SD par la carte Coolpi). Il suffit de débrancher et rebrancher le câble d'alimentation pour redémarrer.

Arrivé à l'interface de connexion, cliquez sur connexion pour accéder au bureau
![Interface de connexion arm64](./assets/在arm设备上安装_common/arm64登录界面-embedded.png)
![Bureau arm64](./assets/在arm设备上安装_common/arm64桌面-embedded.png)

## IV. Autres considérations

### 1. Système lent
Ce problème est causé par une utilisation élevée du CPU par le gestionnaire de fenêtres. Après être entré sur le bureau, allez dans le panneau de contrôle pour désactiver le mode effets spéciaux, ce qui peut efficacement atténuer les ralentissements
![Désactivation des effets spéciaux dans le panneau de contrôle arm64](./assets/在arm设备上安装_common/arm64控制面板特效关闭.png)

### 2. Modification du mot de passe
Comme le système n'a pas d'installation guidée, le système fraîchement installé n'a pas de mot de passe défini. L'utilisateur doit configurer manuellement un mot de passe après l'installation pour pouvoir l'utiliser lorsque le système demande un mot de passe
![Modification du mot de passe arm64](./assets/在arm设备上安装_common/arm64修改密码.png)