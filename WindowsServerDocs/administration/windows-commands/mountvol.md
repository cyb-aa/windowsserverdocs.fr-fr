---
title: mountvol
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fea8ad4d-f04a-4aaa-a3e5-75931e867b39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 03e7cefc7c7a00338972fc365b7c25d9c795c83e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437283"
---
# <a name="mountvol"></a>mountvol



Crée, supprime ou répertorie un point de montage de volume.

Pour obtenir des exemples montrant comment utiliser cette commande, consultez [exemples](#BKMK_examples).

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

## <a name="parameters"></a>Paramètres

|     Paramètre     |                                                                                                                           Description                                                                                                                            |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [\<Drive>:]<Path> |                                                                                             Spécifie le répertoire NTFS existant dans lequel résidera le point de montage.                                                                                             |
|   \<VolumeName>   |                     Spécifie le nom de volume qui est la cible du point de montage. Le nom de volume utilise la syntaxe suivante, où *GUID* est un identificateur global unique :</br>`\\\\?\Volume\{GUID}\`</br>Les accolades sont obligatoires.                      |
|        /d         |                                                                                                    Supprime le point de montage de volume à partir du dossier spécifié.                                                                                                     |
|        /l         |                                                                                                     Indique le nom de volume monté pour le dossier spécifié.                                                                                                      |
|        /p         | Supprime le point de montage de volume à partir du répertoire spécifié, démonte le volume de base et prend le volume de base hors connexion, ce qui irrécupérable. Si d’autres processus utilisent le volume, **mountvol** ferme tous les descripteurs ouverts avant de démonter le volume. |
|        /r         |             Supprime les répertoires de point de montage de volume et les paramètres de Registre pour les volumes qui ne sont plus dans le système, en les empêchant d’être automatiquement montés et donné leur ancien volume des points lors de l’ajout au système de montage.              |
|        /n         |                                                                      Désactive le montage automatique de nouveaux volumes de base. Nouveaux volumes ne sont pas montés automatiquement lors de l’ajout au système.                                                                       |
|        /e         |                                                                                                       Active le montage automatique de nouveaux volumes de base.                                                                                                        |
|        /s         |                                                                                Monte la partition système EFI sur le lecteur spécifié. Disponible sur les ordinateurs Itanium.                                                                                |
|        /?         |                                                                                                               Affiche l'aide à l'invite de commandes.                                                                                                               |

## <a name="remarks"></a>Notes

-   **Mountvol** vous permet de lier des volumes sans nécessiter une lettre de lecteur.
-   Les volumes sont démontés à l’aide de **/p** sont répertoriés dans la liste des volumes comme « Pas monté UNTIL un VOLUME de POINT de montage est créé. » Si le volume a plus d’un montage point, utilisez **/d** pour supprimer les points de montage supplémentaires avant d’utiliser **/p**. Vous pouvez réeffectuer le volume de base montable en assignant un point de montage de volume.
-   Si vous avez besoin développer votre espace de volume sans reformater ou remplacer un disque dur, vous pouvez ajouter un chemin d’accès de montage vers un autre volume. L’avantage d’utiliser un volume avec plusieurs chemins d’accès de montage est que vous pouvez accéder à tous les volumes locaux à l’aide d’une seule lettre de lecteur (par exemple `C:`). Vous n’avez pas besoin de mémoriser la correspondance à la lettre de lecteur qui, bien que vous pouvez toujours monter les volumes locaux et affecter les lettres de lecteur.

## <a name="BKMK_examples"></a>Exemples

Pour créer un point de montage, tapez :
```
mountvol \sysmount \\?\Volume\{2eca078d-5cbc-43d3-aff8-7e8511f60d0e}\
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)