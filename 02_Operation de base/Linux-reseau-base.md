# <center>Sous Linux, les commandes réseau de base que vous devez absolument maîtriser !</center>
#### <center>Auteur : Petit K</center>
#### <center>2022-04-22 23:36:00</center>

Que vous soyez un administrateur système Linux ambitieux ou un passionné de Linux, vous devez absolument connaître ces commandes réseau Linux fondamentales et importantes !

Au cours de l'apprentissage de Linux, tout le monde prête une grande attention à l'utilisation de la ligne de commande, et vous avez probablement lu de nombreux livres pour l'étudier. Dans l'article d'aujourd'hui, je (Note : l'auteur est Abhishek Prakash) vais vous résumer un ensemble de commandes réseau qui m'ont aidé à obtenir d'excellentes notes dans mon cours d'ingénierie des réseaux informatiques. N'hésitez pas à sortir vos antisèches et à les noter rapidement, en espérant que cela vous sera utile aussi.

### Connectivité réseau
Ping : Envoie un message de demande d'écho ICMP à l'hôte, continuellement jusqu'à ce que vous appuyiez sur Ctrl+C. Ping indique qu'un paquet est envoyé de votre machine via ICMP, puis reçoit une réponse au niveau IP. Ping peut vérifier si vous êtes connecté à un autre hôte.

Telnet host : Interagit avec l'hôte sur le port spécifié. Le port par défaut de telnet est 23. D'autres ports couramment utilisés sont le port d'écho 7, le port SMTP 25 pour l'envoi de courrier, et le port 79 pour les requêtes utilisateur. Utilisez Ctrl+] pour quitter telnet.

### ARP
ARP est le protocole de résolution d'adresse, utilisé pour convertir les adresses IPv4 32 bits (adresses IP) en adresses MAC 48 bits (adresses Ethernet). L'utilisateur root peut ajouter/supprimer des entrées ARP. Les entrées ARP sont toutes mises en cache dans le noyau et sont généralement automatiquement supprimées après 20 minutes. Cependant, l'utilisateur root peut créer des entrées ARP permanentes.

arp -a : Affiche la table ARP
arp -s[pub] : Ajoute une entrée
arp -a -d : Supprime toutes les entrées

### Routage
netstat -r : Affiche la table de routage. La table de routage est stockée dans le noyau, et IP l'utilise pour envoyer des paquets vers l'extérieur du réseau.

routed : Démon BSD qui effectue un routage dynamique. Implémente le protocole de routage RIP. Ne peut être utilisé qu'avec les privilèges root.

gated : gated est un autre démon de routage qui implémente RIP. Il utilise simultanément OSPF/EGP/RIP. Ne peut être utilisé qu'avec les privilèges root.

traceroute : Peut être utilisé pour tracer les informations de routage traversées par les paquets IP.

netstat -rnf inet : Peut afficher la table de routage IPv4.

sysctl net.inet.ip.forwarding=1 : Permet la transmission continue des paquets (transforme un hôte en routeur).

route : La commande route est utilisée pour configurer des routes statiques dans la table de routage. Toutes les informations de PC à IP/SubNet doivent passer par l'IP de passerelle spécifiée. Cette commande peut également être utilisée pour définir la route par défaut.

route add|delete [-net|-host] : Ajoute/supprime des routes statiques (par exemple : route add 192.168.20.0/24 192.168.30.4).

route flush : Supprime toutes les routes.

route add -net 0.0.0.0 192.168.10.2 : Ajoute une route par défaut.

### Fichiers importants
/etc/hosts : Adresses IP et noms
/etc/networks : Adresses IP et noms de réseaux
/etc/protocols : Numéros de protocoles et noms de protocoles
/etc/services : Noms des services tcp/udp correspondant aux numéros de ports

### Outils et analyse des performances réseau
ifconfig[up] : Active l'interface
ifconfig[down|delete] : Désactive l'interface
tcpdump -i -vvv : Outil pour capturer et analyser les paquets de données
netstat -w [seconds] -l [interface] : Affiche les paramètres réseau et les données

### Autres
nslookup : Convertit l'IP en nom ou le nom en IP en interrogeant le serveur DNS. Par exemple, nslookup ubuntukylin.com donnera l'adresse IP de ubuntukylin.com.

ftp : Transfère des fichiers entre l'hôte local et l'hôte distant.

rlogin : Connexion à distance à l'hôte.