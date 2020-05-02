---
title: mountvol
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fea8ad4d-f04a-4aaa-a3e5-75931e867b39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca3e4320499161194f87ceaca4f693ff997f3ba2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723922"
---
# <a name="mountvol"></a>mountvol



Crée, supprime ou répertorie un point de montage de volume.

## <a name="syntax"></a>Syntaxe

```
mountvol [<Drive>:]<Path VolumeName>
mountvol [<Drive>:]<Path> /d
mountvol [<Drive>:]<Path> /l
mountvol [<Drive>:]<Path> /p
mountvol /r
mountvol [/n | /e]
mountvol <Drive>: /s
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[\<Lecteur> :]<Path>|Spécifie le répertoire NTFS existant dans lequel se trouve le point de montage.|
|\<Nom_volume>|Spécifie le nom du volume qui est la cible du point de montage. Le nom du volume utilise la syntaxe suivante, où *GUID* est un identificateur global unique :</br>`\\\\?\Volume\{GUID}\`</br>Les crochets {} sont requis.|
|/d|Supprime le point de montage de volume du dossier spécifié.|
|/l|Répertorie le nom du volume monté pour le dossier spécifié.|
|/p|Supprime le point de montage de volume du répertoire spécifié, démonte le volume de base et met le volume de base hors connexion, ce qui le rend non montable. Si d’autres processus utilisent le volume, **mountvol** ferme tous les descripteurs ouverts avant de démonter le volume.|
|/r|Supprime les répertoires de points de montage de volume et les paramètres de Registre pour les volumes qui ne se trouvent plus dans le système, ce qui les empêche d’être montés automatiquement et de recevoir leurs anciens points de montage de volume lorsqu’ils sont rajoutés au système.|
|/n|Désactive le montage automatique des nouveaux volumes de base. Les nouveaux volumes ne sont pas montés automatiquement lorsqu’ils sont ajoutés au système.|
|/e|Réactive le montage automatique des nouveaux volumes de base.|
|/s|Monte la partition système EFI sur le lecteur spécifié.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 

-   **Mountvol** vous permet de lier des volumes sans nécessiter de lettre de lecteur.
-   Les volumes démontés à l’aide de **/p** sont répertoriés dans la liste des volumes comme non montés jusqu’à ce qu’un point de montage de volume soit créé. Si le volume a plusieurs points de montage, utilisez **/d** pour supprimer les points de montage supplémentaires avant d’utiliser **/p**. Vous pouvez rendre le volume de base montable à nouveau en affectant un point de montage de volume.
-   Si vous devez étendre votre espace de volume sans reformater ou remplacer un disque dur, vous pouvez ajouter un chemin d’accès de montage à un autre volume. L’avantage de l’utilisation d’un seul volume avec plusieurs chemins de montage est que vous pouvez accéder à tous les volumes locaux à l’aide `C:`d’une seule lettre de lecteur (par exemple,). Vous n’avez pas besoin de vous souvenir du volume qui correspond à la lettre de lecteur, même si vous pouvez toujours monter des volumes locaux et leur attribuer des lettres de lecteur.

## <a name="examples"></a>Exemples

Pour créer un point de montage, tapez :
```
mountvol \sysmount \\?\Volume\{2eca078d-5cbc-43d3-aff8-7e8511f60d0e}\
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
