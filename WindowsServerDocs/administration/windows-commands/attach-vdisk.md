---
title: attach vdisk
description: Rubrique de commandes de Windows pour **attacher vdisk** -attache (parfois appelées montages ou surfaces) un disque dur virtuel (VHD) afin qu’il apparaisse sur l’ordinateur hôte comme un lecteur de disque dur local.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 882ab875-0c14-4eb3-98ef-fd0e8fa40d9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e715b36f8d9d8b416311567c2455920243dfc7a2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885610"
---
# <a name="attach-vdisk"></a>attach vdisk

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

attache (parfois appelées montages ou surfaces) un disque dur virtuel (VHD) afin qu’il apparaisse sur l’ordinateur hôte comme un lecteur de disque dur local. Si le VHD a déjà une partition de disque et un volume de système de fichiers lorsque vous l'attachez, une lettre de lecteur est assignée au volume contenu dans le disque dur virtuel.
> [!NOTE]
> Cette commande est uniquement applicable à Windows 7 et Windows Server 2008 R2.

## <a name="syntax"></a>Syntaxe
```
attach vdisk [readonly] { [sd=<SDDL>] | [usefilesd] } [noerr]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|en lecture seule|attache le disque dur virtuel en lecture seule. Toute écriture opération retourne une erreur.|
|sd=<SDDL string>|Définit le filtre de l’utilisateur sur le disque dur virtuel. La chaîne de filtre doit être au format de définition du langage SDDL (Security Descriptor). Par défaut le filtre utilisateur autorise l’accès comme sur un disque physique.<br /><br />Les chaînes SDDL peuvent être complexes, mais dans sa forme la plus simple, un descripteur de sécurité qui protège l’accès est appelé une liste de contrôle d’accès discrétionnaire (DACL). Il est au format : D: < dacl_flags >< string_ace1 >< string_ace2 >... < string_acen ><br /><br />Les indicateurs DACL courantes sont :<br /><br />-   **Un** autoriser l’accès<br />-   **D** refuser l’accès<br /><br />Droits communs sont :<br /><br />-   **GA** tous les accès<br />-   **GR** un accès en lecture<br />-   **GW** un accès en écriture<br /><br />Comptes d’utilisateurs courants sont :<br /><br />-   **BA** intégré Administrateurs<br />-   **Australie** utilisateurs authentifiés<br />-   **CO** créateur propriétaire<br />-   **WD** -tout le monde<br /><br />Exemples :<br /><br />**D:P :(A;; GR ; ; Australie** donne accès en lecture à tous les utilisateurs authentifiés<br /><br />**D:P :(A;; DISPONIBILITÉ GÉNÉRALE ; ; WD** donne tout le monde complet via le système|
|usefilesd|Spécifie que le descripteur de sécurité sur le fichier .vhd doit être utilisé sur le disque dur virtuel. Si le **Usefilesd** paramètre n’est pas spécifié, le disque dur virtuel n’aura pas un descripteur de sécurité explicite, sauf si elle est spécifiée avec la **Sd** paramètre.|
|NOERR|Utilisé pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.|
## <a name="remarks"></a>Notes
-   Un disque dur virtuel doit être sélectionné et détaché pour cette opération réussisse. Utilisez le **sélectionnez vdisk** commande pour sélectionner un disque dur virtuel et de déplacer le focus vers elle.
## <a name="BKMK_Examples"></a>Exemples
Pour attacher le disque dur virtuel sélectionné comme étant en lecture seule, tapez :
```
attach vdisk readonly
```
## <a name="additional-references"></a>Références supplémentaires
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
-   [compact vdisk](compact-vdisk.md)

-   [detail vdisk](detail-vdisk.md)
-   [Détacher vdisk](detach-vdisk.md)
-   [Développez vdisk](expand-vdisk.md)
-   [Fusion vdisk](merge-vdisk.md)
-   [select vdisk](select-vdisk.md)
-   [list_1](list_1.md)
