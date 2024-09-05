


# [Cours débutant] Présentation de la bibliothèque de codecs d'images FreeImage, à l'exemple du logiciel de visualisation d'images openKylin

Le logiciel de visualisation d'images est un logiciel open source sur le système d'exploitation openKylin qui permet d'effectuer des opérations de base sur les images, telles que : zoom, rotation, affichage des détails, copie, impression, renommage, etc. Il permet également de recadrer, sauvegarder, annoter et effectuer de la reconnaissance optique de caractères (OCR) sur les images.

![Interface du logiciel de visualisation d'images](https://www.openkylin.top/upload/202302/1675302019720987.png)
Figure 1 : Interface du logiciel de visualisation d'images

En tant que logiciel de visualisation d'images, la visualisation est sa fonction de base et la plus importante. Dans la version V1.2.0 du logiciel, 10 nouveaux formats d'images (exr, psd, jfi, jif, jng, wbmp, xbm, xpm, jp2, j2k) ont été ajoutés pour la visualisation et la sauvegarde. Techniquement, ces formats sont tous implémentés via la bibliothèque FreeImage. Nous allons maintenant vous présenter en détail la bibliothèque de codecs d'images utilisée dans le logiciel de visualisation - FreeImage.

## 1. Présentation de la bibliothèque de codecs d'images du logiciel de visualisation

Le logiciel de visualisation d'images du système openKylin prend actuellement en charge 30 formats d'images : bmp, jpeg, jpg, jpe, pnm, pgm, ppm, pbm, sr, ras, dib, png, apng, gif, webp, tga, svg, ico, tiff, tif, exr, psd, jfi, jif, jng, wbmp, xbm, xpm, jp2, j2k. Pour prendre en charge ces formats d'images, le logiciel utilise les bibliothèques suivantes pour le codage et le décodage des images : la bibliothèque opencv, la bibliothèque FreeImage, la bibliothèque apng, la bibliothèque gif, etc. Parmi celles-ci, la moitié des formats d'images utilisent la bibliothèque opencv familière pour le codage et le décodage, tandis que certains formats spécifiques, comme svg, ont leurs propres bibliothèques associées. En dehors de cela, c'est la bibliothèque FreeImage qui est utilisée pour la lecture et l'écriture des images. Au cours de l'utilisation, nous avons constaté que la bibliothèque FreeImage est rapide, pratique et facile à utiliser pour les applications de haut niveau.

## 2. Présentation de la bibliothèque FreeImage

La bibliothèque FreeImage est une bibliothèque de codecs d'images open source, gratuite et multiplateforme. Elle prend en charge le traitement de formats tels que BMP, JPEG, GIF, PNG, TIFF, etc. Pour utiliser la bibliothèque FreeImage, il faut installer libfreeimage-dev et libfreeimageplus-dev. De plus, la bibliothèque FreeImage fournit de nombreuses interfaces pour obtenir des informations sur les bitmaps, et se caractérise par sa rapidité, sa flexibilité et sa facilité d'utilisation. Toutes les fonctions de la bibliothèque FreeImage commencent par FreeImage_, par exemple les fonctions de lecture et d'écriture de fichiers images sont respectivement FreeImage_Load et FreeImage_Save, et la conversion entre FreeImage et opencv est également simple. Les personnes intéressées peuvent consulter le site officiel de FreeImage pour plus de détails.

## 3. Chargement d'images avec la bibliothèque FreeImage

Dans le logiciel de visualisation d'images, tout le processus de chargement ou de manipulation d'une image utilise la matrice cv::mat pour stocker l'image. Lors de l'ouverture d'une image, le processus complet d'utilisation de la bibliothèque FreeImage pour charger l'image est le suivant :

1. Obtenir le format réel de l'image ;
2. Vérifier si l'image est prise en charge pour la lecture par FreeImage ;
3. Charger l'image avec FreeImage, obtenir un FIBITMAP ;
4. Convertir le FIBITMAP en cv::mat ;
5. Supprimer l'image chargée par libfreeimage de la mémoire pour éviter les fuites de mémoire.

### 3.1 Obtenir le format réel de l'image

Lors de la manipulation d'une image, l'extension du fichier peut être .xbm, .sr, etc., mais cela ne signifie pas que l'image est réellement au format xbm ou sr. Il faut donc d'abord utiliser une fonction de bibliothèque pour obtenir le format réel de l'image.

```cpp
// Obtenir le format du fichier en utilisant la bibliothèque, path est le chemin de l'image.
QByteArray temp_path;
temp_path.append(path.toUtf8());
FREE_IMAGE_FORMAT format = FreeImage_GetFileType(temp_path.data());
```

FreeImage_GetFileType : obtient le type de fichier à partir de l'en-tête du fichier. Paramètre : chemin de l'image. La valeur de retour de cette fonction est FREE_IMAGE_FORMAT. Comme on peut le voir dans la figure ci-dessous, la valeur de retour peut également être FIF_UNKNOWN.

![Types d'images](https://www.openkylin.top/upload/202302/1675302047853305.png)
*Figure 2 : Types d'images*

Si le format de fichier analysé par la fonction de bibliothèque est FIF_UNKNOWN, nous analysons à nouveau le format de l'image en vérifiant l'en-tête du fichier du point de vue des données du fichier, afin d'améliorer la précision de l'obtention du format du fichier.

```cpp
QFile file(path);
if (!file.open(QIODevice::ReadOnly)) {
    return FIF_UNKNOWN;
}
const QByteArray data = file.read(64);
/* Vérifier le fichier bmp */
if (data.startsWith("BM")) {
    return FIF_BMP;
}
//path est le chemin de l'image
```

### 3.2 Vérifier si la lecture est prise en charge

Après avoir obtenu le type de fichier, avant de charger l'image, nous devons encore effectuer une tâche : vérifier si ce format peut être lu par la bibliothèque FreeImage. FreeImage_FIFSupportsReading est utilisé pour vérifier si la lecture de ce type de bitmap est prise en charge.

### 3.3 Charger l'image

Supposons que nous ayons obtenu le format correct de l'image et que ce format puisse être lu par la bibliothèque FreeImage. Nous appelons alors la fonction de bibliothèque FreeImage_Load pour charger le bitmap, qui renvoie un FIBITMAP. La structure de données FIBITMAP contient les informations du bitmap et les données des pixels, c'est le cœur de FreeImage.

### 3.4 Convertir FIBITMAP en mat

Dans le logiciel de visualisation d'images, toutes les données du processus de lecture et d'écriture des images sont transmises sous forme de matrice cv::mat. L'utilisation de cv::mat est destinée à permettre l'extension future des fonctionnalités existantes du logiciel de visualisation, en particulier dans la direction de l'IA fournie par opencv.

![Structure du logiciel de visualisation d'images](https://www.openkylin.top/upload/202302/1675302060527556.png)
Figure 3 : Structure du logiciel de visualisation d'images

Par conséquent, après avoir obtenu le FIBITMAP renvoyé par FreeImage, nous devons le convertir en cv::mat. Lors de la conversion de FIBITMAP en cv::mat, il faut d'abord voir quels paramètres sont nécessaires pour construire une matrice mat d'une image.

```cpp
Mat(int rows, int cols, int type, void* data, size_t step=AUTO_STEP);
```

Donc, notre tâche suivante est d'obtenir tous les paramètres nécessaires à partir de FIBITMAP.

1. int rows, int cols
   Pour le nombre de lignes et de colonnes, FreeImage dispose de fonctions qui peuvent être appelées directement pour obtenir les lignes et les colonnes.

```cpp
FIBITMAP *dib;
int width = FreeImage_GetWidth(dib);
int height = FreeImage_GetHeight(dib);
```

2. int type
   Pour le type, il faut faire un petit traitement. La bibliothèque FreeImage fournit des méthodes pour voir la profondeur et le type de données de l'image.

```cpp
int bpp = FreeImage_GetBPP(src); // Profondeur de l'image
FREE_IMAGE_TYPE fit = FreeImage_GetImageType(src); // Renvoie le type de données du bitmap
```

Après avoir obtenu la valeur énumérée du type FreeImage, il suffit de la convertir en type de données cv::mat correspondant.

![Types de données](https://www.openkylin.top/upload/202302/1675302073529292.png)
Figure 4 : Types de données

3. void* data
   Pointeur vers les données utilisateur.
   La bibliothèque a également une fonction qui peut être appelée directement.
   FreeImage_GetBits(FIBITMAP *dib) : renvoie un pointeur vers les bits de données du bitmap.

4. size_t step
   Nombre d'octets occupés par chaque ligne
   FreeImage_GetPitch(FIBITMAP *dib) : renvoie la profondeur des bits ou la largeur de ligne (aussi appelée largeur de balayage).
   Renvoie la largeur du bitmap alignée sur la prochaine limite de 32 bits en octets.

```cpp
FIBITMAP *dib;
int step = FreeImage_GetPitch(dib);
```

### 3.5 Libération de la mémoire

```cpp
FIBITMAP *dib;
FreeImage_Unload(dib); // Supprimer l'image chargée de la mémoire pour éviter les fuites de mémoire
```

En plus de la bibliothèque FreeImage, il existe actuellement de nombreuses excellentes bibliothèques de codecs d'images. Le logiciel de visualisation d'images openKylin s'adaptera à l'avenir à davantage de bibliothèques d'images pour prendre en charge plus de formats, et utilisera les caractéristiques d'opencv pour étendre certaines fonctionnalités spéciales. Restez à l'écoute, chers amis !

![Logo openKylin](https://www.openkylin.top/upload/202302/1675302088630447.png)

La communauté openKylin (Kylin ouvert) vise à créer un écosystème de partenariat avec les entreprises à travers des méthodes open source et ouvertes, sur la base de l'open source, du volontariat, de l'égalité et de la collaboration. L'objectif est de construire ensemble une communauté de premier plan pour les systèmes d'exploitation de bureau et de promouvoir le développement prospère de la technologie open source Linux et de son écosystème matériel et logiciel. Les premiers membres du conseil d'administration de la communauté comprennent 13 entreprises industrielles et institutions du secteur, notamment Kylin Software, Puhua Basic Software, Zhongke Fangde, Kylin Information Security, Ningshu Software, Yiming Software, ZTE New Support, Yuanxin Technology, China Electronics Technology Group Corporation 32nd Research Institute, Jide System, Beijing Linzhuo, Advanced Operating System Innovation Center, etc.

Source : openKylin
Examiné par : openKylin 20 types d'images populaires
```

Cette traduction conserve la structure du texte original, y compris les titres, les listes à puces, les extraits de code et les liens vers les images. Les noms propres comme "openKylin", "FreeImage", etc. ont été laissés tels quels.