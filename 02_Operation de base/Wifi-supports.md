<center>Support des cartes réseau sans fil</center>
<center>Auteur : Petit K</center>
<center>22-04-2022 23:36:00</center>



## Prise en Charges du Wifi

　　Si vous achetez un nouvel ordinateur, il est préférable d'en choisir un avec un composant sans fil, conçu pour les logiciels libres tels que Linux. Une carte réseau conçue pour les logiciels libres vous offre un meilleur support. Les appareils compatibles avec les logiciels libres fonctionnent également dès leur sortie de l'emballage.

Actuellement, [ThinkPenguin.com] (http://thinkpenguin.com/) propose une carte USB 802.11N avec le chipset AR9170, qui garantit la compatibilité avec les logiciels libres. Il existe également des cartes wifi MiniPCI pour ordinateurs portables et des adaptateurs Bluetooth USB pour la connexion de périphériques sans fil Bluetooth.

　[Passys](http://www.passys.nl/wirelessnetwork) vend des cartes PCI compatibles avec Linux (mais pas avec les logiciels libres) pour les ordinateurs de bureau.

　[Wikipedia](http://en.wikipedia.org/wiki/Comparison_of_open_source_wireless_drivers) fournit de plus amples informations sur les jeux de puces et les pilotes compatibles avec les logiciels libres.

D'autres cartes sont également compatibles avec Linux, mais pas avec les logiciels libres. Elles fonctionnent généralement, mais pas toujours.

Même si votre carte sans fil n'a pas de pilotes conçus pour Ubuntu, vous pouvez la faire fonctionner en utilisant [NDISWrapper] (http://ndiswrapper.sourceforge.net/) et des pilotes pour Microsoft Windows. Mais cela se fait au détriment de la fonctionnalité et de la fiabilité. Si vous utilisez cette méthode, votre connexion réseau sera probablement irrégulière.

　　Autres pages du wiki Ubuntu sur les réseaux sans fil.

　　1. [Page centrale pour les informations Wifi](https://help.ubuntu.com/community/WifiDocs)

　　2. [WifiDocs/WirelessTroubleShootingGuide](https://help.ubuntu.com/community/WifiDocs/WirelessTroubleShootingGuide)

　　3. [Procédures de dépannage sans fil](https://help.ubuntu.com/community/WirelessTroubleshootingProcedure)

### Cartes sans fil
Pour déterminer le type de votre carte/chipset sans fil, vérifiez d'abord s'il s'agit d'un périphérique séparé branché sur l'ordinateur. S'il s'agit d'un périphérique USB indépendant, ouvrez un terminal et saisissez la commande suivante :
<li style="background: #DCDCDC">lsusb</li>
Recherchez des termes comme "wireless" (sans fil) pour identifier le type de votre carte.
Pour les chipsets non-USB intégrés à l'ordinateur, saisissez la commande suivante :
<li style="background: #DCDCDC">lspci -v</li>
Et lisez la dernière section du résultat.

### Par fabricant
La communauté a créé des articles pour les fabricants suivants.

 | Fabricant | Type de carte réseau  |
 | ----  | -------- |
 | 3Com  | PCMCIA、PCI、PCI、Low、Profile、USB |
 | A-Link | USB |
 | Accton | PCI |
 | Adaptec | PCMCIA |
 | Advent | PCMCIA |
 | ADDON | USB |
 | Airlink101 | PCMCIA、PCI、USB |
 | Aptiva | USB |
 | Asus | PCMCIA PCI USB |
 | Atlantis Land | PCI
 | AVM | USB |
 | Belkin | PCMCIA PCI ExpressCard/34 USB |
 | Blitzz | Cardbus |
 | Broadcom | miniPCI |
 | Buffalo | PCMCIA PCI USB |
 | Cable & Wireless | Cardbus
 | Cisco | PCMCIA Cardbus |
 | Cnet | PCMCIA PCI |
 | Compaq | USB |
 | CompUSA/Realtek | PCI |
 | Conceptronic | USB
 | Dell | USB |
 | Dexlan | PCMCIA |
 | Digicom | USB |
 | Digitus | PCMCIA PCI Unknown USB |
 | D-Link | PCMCIA PCI Unknown USB |
 | Edimax | PCMCIA PCI Unknown USB |
 | eHome | PCMCIA |
 | Encore | PCI USB |
 | Gigabyte Technology | miniPCI PCI |
 | Hawking | PCMCIA PCI Cardbus USB |
 | HP | PCI |
 | Intel | miniPCI |
 | KCorp | Cardbus |
 | Level One | PCMCIA |
 | Linksys | PCMCIA PCI USB |
 | Longshine | PCMCIA |
 | Motorola | PCMCIA
 | MSI | miniPCI PCI |
 | MyEssentials | USB |
 | Netcomm | PCI USB |
 | Netcore | USB |
 | Netgear | PCMCIA PCI USB |
 | Novatech | USB |
 | Orient | USB |
 | Pheenet | USB |
 | Proxim/Orinoco | PCMCIA PCI |
 | Qualcomm Atheros | miniPCI |
 | RealTek | PCI USB |
 | RetailPlus | USB |
 | Rosewill | USB |
 | Sierra | USB |
 | Sitecom | PCMCIA PCI |
 | SMC | PCMCIA PCI USB |
 | Sonnet | PCMCIA |
 | Sweex | PCMCIA PCI USB |
 | Topcom | PCMCIA |
 | TP-Link | PCMCIA PCI USB |
 | Trendnet | PCMCIA PCI USB |
 | Trust | Unknown |
 | US Robotics | PCMCIA USB |
 | Veho | USB |
 | Westell | USB |
 | Zonet | PCMCIA PCI USB |
 | Zyxel | PCMCIA PCI USB |
 | Various | miniPCI USB |


### Par version
Veuillez consulter cette page : [WifiDocs/WirelessCardsByVersion(无线卡按版本分类)](https://help.ubuntu.com/community/WifiDocs/WirelessCardsByVersion)

### Par Carte Reseau :

1.[WifiDocs/Device](https://help.ubuntu.com/community/WifiDocs/Device)
2.[WifiDocs/Device/ADDON_ADD-GWP110](https://help.ubuntu.com/community/WifiDocs/Device/ADDON_ADD-GWP110)
3.[WifiDocs/Device/AR5006EG](https://help.ubuntu.com/community/WifiDocs/Device/AR5006EG)
4.[WifiDocs/Device/AR5007](https://help.ubuntu.com/community/WifiDocs/Device/Actiontec)
5.[WifiDocs/Device/Actiontec](https://help.ubuntu.com/community/WifiDocs/Device/Actiontec)
6.[WifiDocs/Device/Airlink101_AWLL3026](https://help.ubuntu.com/community/WifiDocs/Device/Airlink101_AWLL3026)
7.[WifiDocs/Device/AirportExtreme](https://help.ubuntu.com/community/WifiDocs/Device/AirportExtreme)
8.[WifiDocs/Device/Atheros/AR9285](https://help.ubuntu.com/community/WifiDocs/Device/Atheros/AR9285)
9.[WifiDocs/Device/BCM43224 802.11a/b/g/n (rev 01)](https://help.ubuntu.com/community/WifiDocs/Device/BCM43224%20802.11a/b/g/n%20%28rev%2001%29)
10.[WifiDocs/Device/BT_Voyager_1055](https://help.ubuntu.com/community/WifiDocs/Device/BT_Voyager_1055)
11.[WifiDocs/Device/Belkin 300 N F7D2101](https://help.ubuntu.com/community/WifiDocs/Device/Belkin%20300%20N%20F7D2101)
12.[WifiDocs/Device/Belkin F5D8053](https://help.ubuntu.com/community/WifiDocs/Device/Belkin%20F5D8053)
13.[WifiDocs/Device/Belkin_F5D7050_ver_3000_(Ralink_rt73_driver)](https://help.ubuntu.com/community/WifiDocs/Device/Belkin_F5D7050_ver_3000_%28Ralink_rt73_driver%29)
14.[WifiDocs/Device/Belkin_F5D8010](https://help.ubuntu.com/community/WifiDocs/Device/Belkin_F5D8010)
15.[WifiDocs/Device/BuffaloWLIL11GUSB](https://help.ubuntu.com/community/WifiDocs/Device/BuffaloWLIL11GUSB)
16.[WifiDocs/Device/CiscoCB21AG](https://help.ubuntu.com/community/WifiDocs/Device/CiscoCB21AG)
17.[WifiDocs/Device/CompaqW200](https://help.ubuntu.com/community/WifiDocs/Device/CompaqW200)
18.[WifiDocs/Device/D-Link_WUA-1340](https://help.ubuntu.com/community/WifiDocs/Device/D-Link_WUA-1340)
19.[WifiDocs/Device/D-Link_WUA-2340](https://help.ubuntu.com/community/WifiDocs/Device/D-Link_WUA-2340)
20.[WifiDocs/Device/DWA-111](https://help.ubuntu.com/community/WifiDocs/Device/DWA-111)
21.[WifiDocs/Device/DWA-140](https://help.ubuntu.com/community/WifiDocs/Device/DWA-140)
22.[WifiDocs/Device/DWA-552](https://help.ubuntu.com/community/WifiDocs/Device/DWA-552)
23.[WifiDocs/Device/DWL-122](https://help.ubuntu.com/community/WifiDocs/Device/DWL-122)
24.[WifiDocs/Device/DWL-520vE1](https://help.ubuntu.com/community/WifiDocs/Device/DWL-520vE1)
25.[WifiDocs/Device/DWL-G122_(Rev_B)](https://help.ubuntu.com/community/WifiDocs/Device/DWL-G122_%28Rev_B%29)
26.[WifiDocs/Device/DWL-G122_(Rev_C1)](https://help.ubuntu.com/community/WifiDocs/Device/DWL-G122_%28Rev_C1%29)
27.[WifiDocs/Device/DWL-G650+](https://help.ubuntu.com/community/WifiDocs/Device/DWL-G650%2B)
28.[WifiDocs/Device/EdimaxEW7128G](https://help.ubuntu.com/community/WifiDocs/Device/EdimaxEW7128G)
29.[WifiDocs/Device/EdimaxEW7128UG]()https://help.ubuntu.com/community/WifiDocs/Device/EdimaxEW7128UG
30.[WifiDocs/Device/EnGenius EUB9603](https://help.ubuntu.com/community/WifiDocs/Device/EnGenius%20EUB9603)
31.[WifiDocs/Device/F5D7000](https://help.ubuntu.com/community/WifiDocs/Device/F5D7000)
32.[WifiDocs/Device/F5D7010](https://help.ubuntu.com/community/WifiDocs/Device/F5D7010)
33.[WifiDocs/Device/F7D2102](https://help.ubuntu.com/community/WifiDocs/Device/F7D2102)
34.[WifiDocs/Device/Fritz!WLAN_USB_Stick](https://help.ubuntu.com/community/WifiDocs/Device/Fritz%21WLAN_USB_Stick)
35.[WifiDocs/Device/ICIDU_NI-707529_150N_ PCI-E](https://help.ubuntu.com/community/WifiDocs/Device/ICIDU_NI-707529_150N_%20PCI-E)
36.[WifiDocs/Device/IntersilPrism25Wavelan](https://help.ubuntu.com/community/WifiDocs/Device/IntersilPrism25Wavelan)
37.[WifiDocs/Device/Linksys WMP600N](https://help.ubuntu.com/community/WifiDocs/Device/Linksys%20WMP600N)
38.[WifiDocs/Device/Linksys WUSB600N](https://help.ubuntu.com/community/WifiDocs/Device/Linksys%20WUSB600N)
39.[WifiDocs/Device/LinksysWPC54GS-UK](https://help.ubuntu.com/community/WifiDocs/Device/LinksysWPC54GS-UK)
40.[WifiDocs/Device/LinksysWUSB11](https://help.ubuntu.com/community/WifiDocs/Device/LinksysWUSB11)
41.[WifiDocs/Device/LinksysWUSB54GC](https://help.ubuntu.com/community/WifiDocs/Device/LinksysWUSB54GC)
42.[WifiDocs/Device/LinksysWUSB54GP](https://help.ubuntu.com/community/WifiDocs/Device/LinksysWUSB54GP)
43.[WifiDocs/Device/Linksys_WMP54GX](https://help.ubuntu.com/community/WifiDocs/Device/Linksys_WMP54GX)
44.[WifiDocs/Device/Linksys_WUSB54GS_v1_&_v2](https://help.ubuntu.com/community/WifiDocs/Device/Linksys_WUSB54GS_v1_%26_v2)
45.[WifiDocs/Device/NEXXT NW122NXT12](https://help.ubuntu.com/community/WifiDocs/Device/NEXXT%20NW122NXT12)
46.[WifiDocs/Device/NetgearMA111](https://help.ubuntu.com/community/WifiDocs/Device/NetgearMA111)
47.[WifiDocs/Device/NetgearWG111](https://help.ubuntu.com/community/WifiDocs/Device/NetgearWG111)
48.[WifiDocs/Device/Netgear_WG311_v3](https://help.ubuntu.com/community/WifiDocs/Device/Netgear_WG311_v3)
49.[WifiDocs/Device/PENGUIN80211N](https://help.ubuntu.com/community/WifiDocs/Device/PENGUIN80211N)
50.[WifiDocs/Device/Pentagram_Hornet_USB_Lite](https://help.ubuntu.com/community/WifiDocs/Device/Pentagram_Hornet_USB_Lite)
51.[WifiDocs/Device/Proxim RangeLAN-DS](https://help.ubuntu.com/community/WifiDocs/Device/Proxim%20RangeLAN-DS)
52.[WifiDocs/Device/RT3090](https://help.ubuntu.com/community/WifiDocs/Device/RT3090)
53.[WifiDocs/Device/RTL8180L](https://help.ubuntu.com/community/WifiDocs/Device/RTL8180L)
54.[WifiDocs/Device/Ralink RT5390](https://help.ubuntu.com/community/WifiDocs/Device/Ralink%20RT5390)
55.[WifiDocs/Device/RalinkRT2860](https://help.ubuntu.com/community/WifiDocs/Device/RalinkRT2860)
56.[WifiDocs/Device/Ralink_RT5370](https://help.ubuntu.com/community/WifiDocs/Device/Ralink_RT5370)
57.[WifiDocs/Device/Realtek 8172](https://help.ubuntu.com/community/WifiDocs/Device/Realtek%208172)
58.[WifiDocs/Device/RealtekRTL8187b](https://help.ubuntu.com/community/WifiDocs/Device/RealtekRTL8187b)
59.[WifiDocs/Device/Rosewill RNX-N150UBE](https://help.ubuntu.com/community/WifiDocs/Device/Rosewill%20RNX-N150UBE)
60.[WifiDocs/Device/Rosewill RNX-N2LX](https://help.ubuntu.com/community/WifiDocs/Device/Rosewill%20RNX-N2LX)
61.[WifiDocs/Device/RosewillRNXN150UBE](https://help.ubuntu.com/community/WifiDocs/Device/RosewillRNXN150UBE)
62.[WifiDocs/Device/Sabrent 802.11n Wireless PCI](https://help.ubuntu.com/community/WifiDocs/Device/Sabrent%20802.11n%20Wireless%20PCI)
63.[WifiDocs/Device/Sabrent PCI-G802](https://help.ubuntu.com/community/WifiDocs/Device/Sabrent%20PCI-G802)
64.[WifiDocs/Device/SabrentUSB-G802](https://help.ubuntu.com/community/WifiDocs/Device/SabrentUSB-G802)
65.[WifiDocs/Device/SparkLAN WL-850R](https://help.ubuntu.com/community/WifiDocs/Device/SparkLAN%20WL-850R)
66.[WifiDocs/Device/TL-WN722N](https://help.ubuntu.com/community/WifiDocs/Device/TL-WN722N)
67.[WifiDocs/Device/TP-LINK_TL-WN781ND](https://help.ubuntu.com/community/WifiDocs/Device/TP-LINK_TL-WN781ND)
68.[WifiDocs/Device/TP-Link_TL-WN620G_(ndiswrapper)](https://help.ubuntu.com/community/WifiDocs/Device/TP-Link_TL-WN620G_%28ndiswrapper%29)
69.[WifiDocs/Device/Tenda W522U USB](https://help.ubuntu.com/community/WifiDocs/Device/Tenda%20W522U%20USB)
70.[WifiDocs/Device/Tenda_W311M](https://help.ubuntu.com/community/WifiDocs/Device/Tenda_W311M)
71.[WifiDocs/Device/Topcom_Skyracer_USB_4001g_(WLAN-USB-Stick)](https://help.ubuntu.com/community/WifiDocs/Device/Topcom_Skyracer_USB_4001g_%28WLAN-USB-Stick%29)
72.[WifiDocs/Device/WG111T](https://help.ubuntu.com/community/WifiDocs/Device/WG111T)
73.[WifiDocs/Device/WG121](https://help.ubuntu.com/community/WifiDocs/Device/WG121)
74.[WifiDocs/Device/ipn2220](https://help.ubuntu.com/community/WifiDocs/Device/ipn2220)
75.[WifiDocs/Device/wpn111](https://help.ubuntu.com/community/WifiDocs/Device/wpn111)
76.[WifiDocs/Device/xg-301](https://help.ubuntu.com/community/WifiDocs/Device/xg-301)
