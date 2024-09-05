# Peony
## Qu'est-ce que Peony

Peony est le gestionnaire de fichiers par défaut dans UKUI.

Peony utilise glib, gvfs et gio comme couches sous-jacentes, avec Qt pour la reconstruction et les améliorations. Peony peut être divisé en plusieurs parties :

   - peony-qt-core : Objets et interfaces basés sur glib, gvfs et gio
   - file-operation : Interfaces pour les opérations sur les fichiers basées sur peony-qt-core
   - peony-qt-mode : Cadre de vue basé sur peony-qt-core et file-operation
   - peony-extensions : Plugins d'extension pour Peony
   - peony-qt-ui : Architecture UI basée sur tous les composants ci-dessus et Qt.

## Comment commencer

   Depot : https://gitee.com/openkylin/peony