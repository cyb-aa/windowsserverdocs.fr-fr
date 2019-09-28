---
title: système
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d57c2df1-eb82-4b81-b8cd-e30564c6a929
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 970a5d6403804db7585bf3895dbb61eead286c37
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384381"
---
# <a name="san"></a>système

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Affiche ou définit la stratégie de réseau de zone de stockage (San) pour le système d’exploitation.
> [!NOTE]
> Cette commande s’applique uniquement à Windows 7 et Windows Server 2008 R2.

## <a name="syntax"></a>Syntaxe
```
san [policy={onlineAll | offlineAll | offlineShared}] [noerr]
```
### <a name="parameters"></a>Paramètres

|                          Paramètre                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|--------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Policy = {onlineAll &#124; offlineAll &#124; offlineShared}] | Définit la stratégie San pour le système d’exploitation actuellement démarré. La stratégie San détermine si un disque nouvellement découvert est mis en ligne ou reste hors connexion, et s’il est en lecture/écriture ou reste en lecture seule. Lorsqu’un disque est hors connexion, la disposition du disque peut être lue, mais aucun périphérique de volume n’est exposé via Plug-and-Play. Cela signifie qu’aucun système de fichiers ne peut être monté sur le disque. Lorsqu’un disque est en ligne, un ou plusieurs périphériques de volume sont installés pour le disque. Voici une explication de chaque paramètre :<br /><br />-   **onlineAll**. Spécifie que tous les disques récemment découverts seront mis en ligne et en lecture/écriture. **IMPORTANT :**     La spécification de **onlineAll** sur un serveur qui partage des disques peut entraîner une altération des données. Par conséquent, vous ne devez pas définir cette stratégie si les disques sont partagés entre les serveurs, sauf si le serveur fait partie d’un cluster.<br />-   **offlineAll**. Spécifie que tous les disques récemment découverts, à l’exception du disque de démarrage, sont hors connexion Andread uniquement par défaut.<br />-   **offlineShared**. Spécifie que tous les disques récemment découverts qui ne résident pas sur un bus partagé (par exemple, SCSI et iSCSI) sont mis en ligne et en lecture-écriture. Les disques qui restent hors connexion sont en lecture seule par défaut.<br /><br />Pour plus d’informations, consultez [VDS_san_POLICY, énumération](https://go.microsoft.com/fwlink/?LinkId=203815) (<https://go.microsoft.com/fwlink/?LinkId=203815>). |
|                            noerr                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            Utilisé uniquement pour les scripts. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

## <a name="remarks"></a>Notes
- Si la commande est fournie sans paramètres, la stratégie San actuelle est affichée.
  ## <a name="BKMK_Examples"></a>Illustre
  Pour afficher la stratégie actuelle, tapez :
  ```
  san
  ```
  Pour mettre tous les disques récemment découverts, à l’exception du disque de démarrage, en mode hors connexion et en lecture seule par défaut, tapez :
  ```
  san policy=offlineAll
  ```
  ## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
