---
ms.assetid: 9f3dc104-dd69-4b03-b824-a29896780164
title: Fichier fsutil
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 236f951286e2ed2e16a329a04e912b812d020aa6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725514"
---
# <a name="fsutil-file"></a>Fichier fsutil
> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Recherche un fichier par nom d’utilisateur (si les quotas de disque sont activés), interroge des plages allouées pour un fichier, définit le nom abrégé d’un fichier, définit la longueur de données valide d’un fichier, ne définit aucune donnée pour un fichier ou crée un nouveau fichier.



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

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------------|---------------|
|CreateNew|Crée un fichier du nom et de la taille spécifiés, avec un contenu composé de zéros.|
|\<nom de fichier>|Spécifie le chemin d’accès complet au fichier, y compris le nom de fichier et l’extension, par exemple C:\documents\filename.txt.|
|\<longueur>|Spécifie la longueur des données valides du fichier.|
|findbysid|Recherche des fichiers qui appartiennent à un utilisateur spécifié sur des volumes NTFS où les quotas de disque sont activés.|
|\<nom d’utilisateur>|Spécifie le nom d’utilisateur ou le nom d’ouverture de session de l’utilisateur.|
|\<Répertoire>|Spécifie le chemin d’accès complet au répertoire, par exemple C:\Users.|
|optimizemetadata|Cela effectue un compactage immédiat des métadonnées pour un fichier donné.|
|/A|Analyser les métadonnées de fichier avant et après l’optimisation.|
|queryallocranges|Interroge les plages allouées pour un fichier sur un volume NTFS. Utile pour déterminer si un fichier a des régions éparses.|
|offset =\<décalage>|Spécifie le début de la plage qui doit être définie sur zéros.|
|longueur =\<longueur>|Spécifie la longueur de la plage (en octets).|
|queryextents|Interroge les étendues d’un fichier.|
|/R|Si <filename> est un point d’analyse, ouvrez-le au lieu de sa cible.|
|\<startingvcn>|Spécifie le premier VCN à interroger. En cas d’omission, commencez à VCN 0.|
|\<numvcns>|Nombre de VCNs à interroger. En cas d’omission ou 0, interroger jusqu’à EOF.|
|queryfileid|Interroge l’ID d’un fichier sur un volume NTFS.<p>Ce paramètre s’applique à : Windows Server 2008 R2 et Windows 7.|
|\<> du volume|Spécifie le volume en tant que nom de lecteur suivi d’un signe deux-points.|
|queryfilenamebyid|Affiche un nom de lien aléatoire pour un ID de fichier spécifié sur un volume NTFS. Étant donné qu’un fichier peut avoir plusieurs noms de liens pointant vers ce fichier, il n’est pas garanti que le lien de fichier sera fourni à la suite de la requête pour le nom de fichier.<p>Ce paramètre s’applique à : Windows Server 2008 R2 et Windows 7.|
|\<fileid>|Spécifie l’ID du fichier sur un volume NTFS.|
|queryoptimizemetadata|Interroge l’état des métadonnées d’un fichier.|
|queryvaliddata|Interroge la longueur de données valide d’un fichier.|
|/D|Affichez des informations détaillées sur les données valides.|
|seteof|Définit la EOF du fichier donné.|
|setshortname|Définit le nom abrégé (nom du fichier de longueur 8,3 caractères) d’un fichier sur un volume NTFS.|
|\<ShortName>|Spécifie le nom abrégé du fichier.|
|setvaliddata|Définit la longueur de données valide pour un fichier sur un volume NTFS.|
|\<> DATALENGTH|Spécifie la longueur du fichier en octets.|
|setzerodata|Définit une plage (spécifiée par le *décalage* et la *longueur*) du fichier à zéro, ce qui vide le fichier. Si le fichier est un fichier partiellement alloué, les unités d’allocation sous-jacentes sont désallouées.|

## <a name="remarks"></a>Notes 

-   Dans NTFS, il existe deux concepts importants de la longueur de fichier : le marqueur de fin de fichier (EOF) et la longueur de données valide (VDL). Le EOF indique la longueur réelle du fichier. Le VDL identifie la longueur des données valides sur le disque. Toutes les lectures entre VDL et EOF retournent automatiquement 0 pour conserver l’exigence de réutilisation de l’objet C2.

-   Le paramètre **setvaliddata** est uniquement disponible pour les administrateurs, car il nécessite le privilège effectuer des tâches de maintenance de volume (SeManageVolumePrivilege). Cette fonctionnalité est uniquement requise pour les scénarios de réseau multimédia avancé et de réseau de zone système. Le paramètre **setvaliddata** doit être une valeur positive supérieure à celle du VDL actuel, mais inférieure à la taille du fichier actuel.

    Il est utile pour les programmes de définir un VDL dans les cas suivants :

    -   Écriture de clusters bruts directement sur le disque via un canal matériel. Cela permet au programme d’informer le système de fichiers que cette plage contient des données valides qui peuvent être renvoyées à l’utilisateur.

    -   Création de fichiers volumineux lorsque la performance est un problème. Cela évite le temps nécessaire pour remplir le fichier avec des zéros lors de la création ou de l’extension du fichier.

## <a name="examples"></a><a name="BKMK_examples"></a>Illustre
Pour rechercher des fichiers appartenant à scottb sur le lecteur C, tapez :

```
fsutil file findbysid scottb c:\users  
```

Pour interroger les plages allouées pour un fichier sur un volume NTFS, tapez :

```
fsutil file queryallocranges offset=1024 length=64 c:\temp\sample.txt  
```

Pour optimiser les métadonnées d’un fichier, tapez :

```
fsutil file optimizemetadata C:\largefragmentedfile.txt
```

Pour interroger les étendues d’un fichier, tapez :

```
fsutil file queryextents C:\Temp\sample.txt
```

Pour définir le EOF d’un fichier, tapez :

```
fsutil file seteof C:\testfile.txt 1000
```

Pour définir le nom abrégé du fichier longfilename. txt sur le lecteur C sur Longfile. txt, tapez :

```
fsutil file setshortname c:\longfilename.txt longfile.txt  
```

Pour définir la longueur des données valides sur 4096 octets pour un fichier nommé TestFile. txt sur un volume NTFS, tapez :

```
fsutil file setvaliddata c:\testfile.txt 4096  
```

Pour définir une plage d’un fichier sur un volume NTFS sur des zéros pour la vider, tapez :

```
fsutil file setzerodata offset=100 length=150 c:\temp\sample.txt  
```

## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Fsutil](Fsutil.md)


