
# Vérifier les informations système

- Vérifier la version du noyau
```
uname -a
```

- Vérifier les informations de version du système
```
cat /etc/os-release
lsb_release -a
```

- Vérifier les informations du CPU
```
lscpu
```

- Vérifier les informations de mémoire
```
free
```

- Vérifier les informations du disque
```
df -h
fdisk -l
```

- Vérifier les informations en temps réel des ressources système
```
top
```

- Vérifier la vitesse de lecture/écriture du disque dur
```
iostat -t 1 3
# Une fois par seconde, 3 fois
```
