---
title: attach vdisk
description: Rubrique relative aux commandes Windows pour **Attach vdisk** -attaches (parfois appelées montages ou surfaces) un disque dur virtuel (VHD) pour qu’il apparaisse sur l’ordinateur hôte en tant que lecteur de disque dur local.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d29eacfc8575ec50859733612a3d58b166d9402d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382640"
---
# <a name="attach-vdisk"></a>attach vdisk

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

joint (parfois appelé montage ou surfaces) un disque dur virtuel (VHD) afin qu’il apparaisse sur l’ordinateur hôte en tant que lecteur de disque dur local. Si le VHD a déjà une partition de disque et un volume de système de fichiers lorsque vous l'attachez, une lettre de lecteur est assignée au volume contenu dans le disque dur virtuel.
> [!NOTE]
> Cette commande s’applique uniquement à Windows 7 et Windows Server 2008 R2.

## <a name="syntax"></a>Syntaxe
```
attach vdisk [readonly] { [sd=<SDDL>] | [usefilesd] } [noerr]
```
### <a name="parameters"></a>Paramètres

|    Paramètre     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     seulement     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                             attache le disque dur virtuel en lecture seule. Toute opération d’écriture retourne une erreur.                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| SD = <SDDL string> | Définit le filtre utilisateur sur le disque dur virtuel. La chaîne de filtrage doit être au format SDDL (Security Descriptor Definition Language). Par défaut, le filtre utilisateur autorise l’accès comme sur un disque physique.<br /><br />Les chaînes SDDL peuvent être complexes, mais dans sa forme la plus simple, un descripteur de sécurité qui protège l’accès est connu sous le nom de liste de contrôle d’accès discrétionnaire (DACL, Discretionary Access Control List). Il se présente sous la forme : D : < dacl_flags > < string_ace1 > < string_ace2... < string_acen ><br /><br />Les indicateurs DACL courants sont les suivants :<br /><br />-   **A** autoriser l’accès<br />-   **D** refuser l’accès<br /><br />Les droits courants sont les suivants :<br /><br />-   **GA** tous les accès<br />-   **GR** accès en lecture<br />accès en écriture -   **GW**<br /><br />Les comptes d’utilisateur courants sont les suivants :<br /><br />administrateurs intégrés -   **BA**<br />utilisateurs**authentifiés** par -   <br />-   **co** Creator propriétaire<br />-   **WD** -tout le monde<br /><br />Exemples :<br /><br />**D :P: (A ;; GR ;;;** La valeur au fournit un accès en lecture à tous les utilisateurs authentifiés<br /><br />**D :P: (A ;; GA ;;; WD** offre à tout le monde un acces complet |
|    usefilesd     |                                                                                                                                                                                                                                                                                                                                                                                          Spécifie que le descripteur de sécurité du fichier. vhd doit être utilisé sur le disque dur virtuel. Si le paramètre **Usefilesd** n’est pas spécifié, le disque dur virtuel n’aura pas de descripteur de sécurité explicite, sauf s’il est spécifié avec le paramètre **SD** .                                                                                                                                                                                                                                                                                                                                                                                          |
|      noerr       |                                                                                                                                                                                                                                                                                                                                                                                                           Utilisé uniquement pour les scripts. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur.                                                                                                                                                                                                                                                                                                                                                                                                           |

## <a name="remarks"></a>Notes
- Pour que cette opération aboutisse, vous devez sélectionner et détacher un disque dur virtuel. Utilisez la commande **Select vdisk** pour sélectionner un disque dur virtuel et lui déplacer le focus.
  ## <a name="BKMK_Examples"></a>Illustre
  Pour attacher le disque dur virtuel sélectionné en lecture seule, tapez :
  ```
  attach vdisk readonly
  ```
  ## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
- [Compact vdisk](compact-vdisk.md)

- [détailler vdisk](detail-vdisk.md)
- [Détacher vdisk](detach-vdisk.md)
- [développer vdisk](expand-vdisk.md)
- [Merge vdisk](merge-vdisk.md)
- [sélectionner vdisk](select-vdisk.md)
- [list_1](list_1.md)
