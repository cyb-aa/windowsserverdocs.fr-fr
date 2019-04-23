---
ms.assetid: 9f3dc104-dd69-4b03-b824-a29896780164
title: fichier de fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: ffaf02f74f20f4eb94b94d8f0ffc51f26a62390e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828120"
---
# <a name="fsutil-file"></a>fichier de fsutil
>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Recherche un fichier par nom d’utilisateur (si les Quotas de disque sont activés), interroge les plages allouées d’un fichier, définit un nom court, définit la longueur de données valide, données égales à zéro pour un fichier ou crée un nouveau fichier.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
fsutil file [createnew] <filename> <length>
fsutil file [findbysid] <username> <directory>
fsutil file [optimizemetadata] [/A] <filename>
fsutil file [queryallocranges] offset=<offset> length=<length> <filename>
fsutil file [queryextents] [/R] <filename> [<startingvcn> [<numvcns>]]
fsutil file [queryfileid] <filename>
fsutil file [queryfilenamebyid] <volume> <fileid>
fsutil file [queryoptimizemetadata] <filename>
fsutil file [queryvaliddata] [/R] [/D] <filename>
fsutil file [seteof] <filename> <length>
fsutil file [setshortname] <filename> <shortname>
fsutil file [setvaliddata] <filename> <datalength>
fsutil file [setzerodata] offset=<offset> length=<length> <filename>

```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------------|---------------|
|CreateNew|Crée un fichier du nom spécifié et la taille, avec du contenu qui se compose de zéros.|
|\<filename>|Spécifie le chemin d’accès complet au fichier, y compris le nom de fichier et l’extension, par exemple C:\documents\filename.txt.|
|\<length>|Spécifie la longueur de données valide.|
|findbysid|Recherche les fichiers qui appartiennent à un utilisateur spécifié sur les volumes NTFS où les Quotas de disque sont activés.|
|\<username>|Spécifie le nom du utilisateur nom d’utilisateur ou d’ouverture de session.|
|\<directory>|Spécifie le chemin d’accès complet au répertoire, par exemple C:\users.|
|optimizemetadata|Cela effectue un compactage immédiat des métadonnées pour un fichier donné.|
|/A|Analyser les métadonnées d’un fichier avant et après l’optimisation.|
|queryallocranges|Interroge les plages allouées d’un fichier sur un volume NTFS. Utile pour déterminer si un fichier comporte des zones éparses.|
|offset=\<offset>|Spécifie le début de la plage qui doit être défini en zéros.|
|length=\<length>|Spécifie la longueur de la plage (en octets).|
|queryextents|Extensions de requêtes pour un fichier.|
|/R|Si <filename> est une nouvelle analyse point, ouvrez il plutôt que sa cible.|
|\<startingvcn>|Spécifie le premier VCN à interroger. Si omis, commencez à VCN 0.|
|\<numvcns>|Nombre de VCNs à interroger. Si omis ou 0, la requête jusqu'à ce que EOF.|
|queryfileid|Interroge l’ID d’un fichier sur un volume NTFS.<br /><br />Ce paramètre s’applique à :  Windows Server 2008 R2 et Windows 7.|
|\<volume>|Spécifie le volume en tant que nom du lecteur suivie du signe deux-points.|
|queryfilenamebyid|Affiche un nom de lien aléatoire pour un ID de fichier spécifié sur un volume NTFS. Dans la mesure où un fichier peut avoir plusieurs noms de lien qui pointe vers ce fichier, il le lien de fichier est fourni à la suite de la requête pour le nom de fichier n’est pas garanti.<br /><br />Ce paramètre s’applique à :  Windows Server 2008 R2 et Windows 7.|
|\<fileid>|Spécifie l’ID du fichier sur un volume NTFS.|
|queryoptimizemetadata|Interroge l’état de métadonnées d’un fichier.|
|queryvaliddata|Interroge la longueur de données valides pour un fichier.|
|/D|Afficher les informations détaillées des données valides.|
|seteof|Définit le EOF du fichier donné.|
|Nom_court|Définit le nom court (nom de fichier de longueur en caractères au format 8.3) pour un fichier sur un volume NTFS.|
|\<shortname>|Spécifie le nom court.|
|setvaliddata|Définit la longueur de données valides pour un fichier sur un volume NTFS.|
|\<datalength>|Spécifie la longueur du fichier en octets.|
|setzerodata|Définit une plage (spécifié par *décalage* et *longueur*) du fichier en zéros, qui vide le fichier. Si le fichier est un fichier partiellement alloué, les unités d’allocation sous-jacentes sont rendues non valides.|

## <a name="remarks"></a>Notes

-   Dans NTFS, il existe deux concepts importants de la longueur du fichier : le marqueur de fin de fichier (EOF) et la longueur de données valides (VDL). Le EOF indique la longueur réelle du fichier. La longueur de données valides identifie la longueur des données valides sur le disque. N’importe quel lit entre la longueur de données valides et EOF retour automatiquement la nécessité de réutilisation de 0 pour conserver l’objet C2.

-   Le **setvaliddata** paramètre est uniquement disponible pour les administrateurs, car il nécessite le privilège de tâches (SeManageVolumePrivilege) de maintenance de volume effectuer. Cette fonctionnalité est requise uniquement pour avancée multimédia et système de réseaux. Le **setvaliddata** paramètre doit être une valeur positive est supérieure à la longueur de données valides, mais inférieure à la taille du fichier.

    Il est utile pour les programmes définir une longueur de données valides lorsque :

    -   Écriture de clusters bruts directement sur le disque via un canal de matériel. Cela permet au programme informer le système de fichiers, cette plage contient des données valides qui peuvent être retournées à l’utilisateur.

    -   Création des fichiers volumineux lorsque les performances sont un problème. Cela évite le temps que nécessaire pour remplir le fichier avec des zéros lorsque le fichier est créé ou étendu.

## <a name="BKMK_examples"></a>Exemples
Pour rechercher les fichiers appartenant à scottb sur le lecteur C, tapez :

```
fsutil file findbysid scottb c:\users  
```

Pour interroger les plages allouées d’un fichier sur un volume NTFS, tapez :

```
fsutil file queryallocranges offset=1024 length=64 c:\temp\sample.txt  
```

Pour optimiser les métadonnées d’un fichier, tapez :

```
fsutil file optimizemetadata C:\largefragmentedfile.txt
```

Pour interroger les étendues pour un fichier, tapez :

```
fsutil file queryextents C:\Temp\sample.txt
```

Pour définir le EOF pour un fichier, tapez :

```
fsutil file seteof C:\testfile.txt 1000
```

Pour définir le nom court pour le fichier Longfilename.txt sur le lecteur C à Longfile.txt, tapez :

```
fsutil file setshortname c:\longfilename.txt longfile.txt  
```

Pour définir la longueur de données valide à 4 096 octets pour un fichier nommé Testfile.txt sur un volume NTFS, tapez :

```
fsutil file setvaliddata c:\testfile.txt 4096  
```

Pour définir une plage d’un fichier sur un volume NTFS aux zéros vider, tapez :

```
fsutil file setzerodata offset=100 length=150 c:\temp\sample.txt  
```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


