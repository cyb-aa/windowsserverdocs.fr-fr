---
title: shrink
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ec87cc7c-9846-465e-a10d-4ee10db4f4e6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 09cc9ce7beebc04a0a45cd93c2b0da25c5a1a49e
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441264"
---
# <a name="shrink"></a>shrink

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La commande de réduction Diskpart réduit la taille du volume sélectionné par le montant que vous spécifiez. Cette commande rend disponible l’espace disque disponible à partir de l’espace inutilisé à la fin du volume.

## <a name="syntax"></a>Syntaxe
```
shrink [desired=<n>] [minimum=<n>] [nowait] [noerr]
shrink querymax [noerr]
```
## <a name="parameters"></a>Paramètres

|  Paramètre  |                                                                                             Description                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| desired=<n> |                                                     Spécifie la quantité souhaitée d’espace en mégaoctets (Mo) pour réduire la taille du volume par.                                                     |
| minimum=<n> |                                                           Spécifie la quantité minimale d’espace en Mo pour réduire la taille du volume.                                                           |
|  querymax   |                       Retourne la quantité maximale d’espace en Mo par lequel le volume peut être réduit. Cette valeur peut changer si les applications qui accèdent actuellement au volume.                        |
|   NOWAIT    |                                                       force la commande pour retourner immédiatement le processus de réduction est en cours d’exécution.                                                        |
|    NOERR    | Pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart. |

## <a name="remarks"></a>Notes
- Vous pouvez réduire la taille d’un volume uniquement si elle est mise en forme à l’aide du système de fichiers NTFS ou si elle n’a pas d’un système de fichiers.
- Cette commande fonctionne sur les volumes de base et sur les volumes dynamiques simples ou fractionnés.
- Si une quantité souhaitée n’est pas spécifiée, le volume sera réduit par la quantité minimale (si spécifié).
- Si une quantité minimale n’est pas spécifiée, le volume sera réduit par la quantité souhaitée (si spécifié).
- Si une quantité minimale, ni une quantité souhaitée n’est spécifiée, le volume sera réduit autant que possible.
- Si une quantité minimale est spécifiée, mais pas suffisamment d’espace libre est disponible, la commande échoue.
- Un volume doit être sélectionné pour cette opération réussisse. Utilisez le **sélectionnez volume** commande pour sélectionner un volume et déplacer le focus vers elle.
- Cette commande ne fonctionne pas sur les partitions OEM (OEM), les partitions système Extensible Firmware Interface (EFI) ou les partitions de récupération.
  ## <a name="BKMK_examples"></a>Exemples
  Pour réduire la taille du volume sélectionné par la plus grande quantité possible entre 250 et 500 Mo, tapez :
  ```
  shrink desired=500 minimum=250
  ```
  Pour afficher le nombre maximal de Mo le volume peut être réduit par, tapez :
  ```
  shrink querymax
  ```

[Resize-Partition](https://technet.microsoft.com/library/hh848680.aspx)
