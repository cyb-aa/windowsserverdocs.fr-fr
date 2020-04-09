---
title: shrink
description: Rubrique relative aux commandes Windows pour DiskPart Shrink, qui réduit la taille du volume sélectionné par la quantité que vous spécifiez.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ec87cc7c-9846-465e-a10d-4ee10db4f4e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2afdaf4ac27ef0c4378d6ae34d959dc81e63bc18
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834202"
---
# <a name="shrink"></a>shrink

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La commande DiskPart Shrink réduit la taille du volume sélectionné en spécifiant la quantité spécifiée. Cette commande permet d’libérer de l’espace disque disponible à partir de l’espace inutilisé à la fin du volume.

## <a name="syntax"></a>Syntaxe
```
shrink [desired=<n>] [minimum=<n>] [nowait] [noerr]
shrink querymax [noerr]
```
### <a name="parameters"></a>Paramètres

|  Paramètre  |                                                                                             Description                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| desired =<n> |                                                     Spécifie la quantité d’espace souhaitée en mégaoctets (Mo) pour réduire la taille du volume.                                                     |
| minimum =<n> |                                                           Spécifie la quantité minimale d’espace en Mo pour réduire la taille du volume.                                                           |
|  QueryMax   |                       Retourne la quantité maximale d’espace en Mo en fonction de laquelle le volume peut être réduit. Cette valeur peut changer si les applications accèdent actuellement au volume.                        |
|   nowait    |                                                       force la commande à retourner immédiatement pendant que le processus de réduction est toujours en cours.                                                        |
|    noerr    | À des fins de script uniquement. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |

## <a name="remarks"></a>Notes
- Vous pouvez réduire la taille d’un volume uniquement s’il est formaté à l’aide du système de fichiers NTFS ou s’il ne dispose pas d’un système de fichiers.
- Cette commande fonctionne sur les volumes de base et sur les volumes dynamiques simples ou fractionnés.
- Si une quantité souhaitée n’est pas spécifiée, le volume est réduit d’une quantité minimale (si elle est spécifiée).
- Si une quantité minimale n’est pas spécifiée, le volume est réduit selon la quantité souhaitée (si elle est spécifiée).
- Si ni une quantité minimale, ni une quantité souhaitée n’est spécifiée, le volume est réduit de la manière la plus possible.
- Si une quantité minimale est spécifiée mais que l’espace libre disponible est insuffisant, la commande échoue.
- Vous devez sélectionner un volume pour que cette opération aboutisse. Utilisez la commande **Sélectionner un volume** pour sélectionner un volume et lui déplacer le focus.
- Cette commande ne fonctionne pas sur les partitions OEM (Original Equipment Manufacturer), les partitions système Extensible Firmware Interface (EFI) ou les partitions de récupération.
  ## <a name="examples"></a><a name=BKMK_examples></a>Illustre
  Pour réduire la taille du volume sélectionné par la plus grande quantité possible comprise entre 250 et 500 mégaoctets, tapez :
  ```
  shrink desired=500 minimum=250
  ```
  Pour afficher le nombre maximal de Mo que le volume peut réduire, tapez :
  ```
  shrink querymax
  ```

[Resize-Partition](https://technet.microsoft.com/library/hh848680.aspx)
