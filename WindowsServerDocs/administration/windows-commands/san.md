---
title: san
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a999254507de2d90494c1acfb906d4bcf26168aa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872500"
---
# <a name="san"></a>san

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche ou définit la stratégie de réseau san de zone de stockage pour le système d’exploitation.
> [!NOTE]
> Cette commande est uniquement applicable à Windows 7 et Windows Server 2008 R2.

## <a name="syntax"></a>Syntaxe
```
san [policy={onlineAll | offlineAll | offlineShared}] [noerr]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|policy={ onlineAll &#124; offlineAll &#124; offlineShared }]|Définit la stratégie san pour le système d’exploitation actuellement démarré. La stratégie san détermine si un disque nouvellement découvert est mis en ligne ou reste hors connexion, et si elle devient en lecture/écriture ou si elle reste en lecture seule. Lorsqu’un disque est hors connexion, la disposition du disque peut être lu, mais aucun appareil de volume n’est exposés via le Plug-and-Play. Cela signifie qu’aucun système de fichiers ne peut être monté sur le disque. Lorsqu’un disque est en ligne, un ou plusieurs périphériques de volume sont installés pour le disque. Voici une explication de chaque paramètre :<br /><br />-   **onlineAll**. Spécifie que toutes les découvertes qui vient d’être des disques sont mis en ligne et en lecture/écriture. **IMPORTANT :**     Spécification **onlineAll** sur un serveur qui partage des disques peut entraîner une altération des données. Par conséquent, vous ne devez pas définir cette stratégie si les disques sont partagés entre les serveurs, sauf si le serveur fait partie d’un cluster.<br />-   **offlineAll**. Spécifie que toutes les découvertes qui vient d’être des disques, sauf le disque de démarrage sera hors connexion andread seule par défaut.<br />-   **offlineShared**. Spécifie que tout ce qui vient d’être découvert les disques qui ne se trouvent pas sur un bus partagé (par exemple, SCSI et iSCSI) sont mis en ligne et effectuées en lecture-écriture. Les disques se trouvent hors connexion seront en lecture seule par défaut.<br /><br />Pour plus d’informations, consultez [VDS_san_POLICY énumération](https://go.microsoft.com/fwlink/?LinkId=203815) (https://go.microsoft.com/fwlink/?LinkId=203815).|
|NOERR|Utilisé pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.|
## <a name="remarks"></a>Notes
-   Si la commande est spécifiée sans paramètre, la stratégie san actuelle s’affiche.
## <a name="BKMK_Examples"></a>Exemples
Pour afficher la stratégie actuelle, tapez :
```
san
```
Pour que tous les disques nouvellement découvertes, sauf le disque de démarrage, en mode hors connexion et en lecture seule par défaut, tapez :
```
san policy=offlineAll
```
## <a name="additional-references"></a>Références supplémentaires
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
