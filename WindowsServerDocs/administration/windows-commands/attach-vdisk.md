---
title: attach vdisk
description: La rubrique relative aux commandes Windows pour **Attach vdisk**, qui attache (ou Monte) un disque dur virtuel (VHD) afin qu’il apparaisse sur l’ordinateur hôte comme un lecteur de disque dur local.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 882ab875-0c14-4eb3-98ef-fd0e8fa40d9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3a903ed231e34ac902ce10b5342f27e772ac89f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851262"
---
# <a name="attach-vdisk"></a>attach vdisk

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Joint (parfois appelé montage ou surfaces) un disque dur virtuel (VHD) afin qu’il apparaisse sur l’ordinateur hôte en tant que lecteur de disque dur local. Si le VHD a déjà une partition de disque et un volume de système de fichiers lorsque vous l'attachez, une lettre de lecteur est assignée au volume contenu dans le disque dur virtuel.

> [!NOTE]
> Cette commande s’applique uniquement à Windows 7 et Windows Server 2008 R2.

## <a name="syntax"></a>Syntaxe

```
attach vdisk [readonly] { [sd=<SDDL>] | [usefilesd] } [noerr]
```

#### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| readonly | Attache le disque dur virtuel en lecture seule. Toute opération d’écriture retourne une erreur. |
| `sd=<SDDL string>` | Définit le filtre utilisateur sur le disque dur virtuel. La chaîne de filtrage doit être au format SDDL (Security Descriptor Definition Language). Par défaut, le filtre utilisateur autorise l’accès comme sur un disque physique. Les chaînes SDDL peuvent être complexes, mais dans sa forme la plus simple, un descripteur de sécurité qui protège l’accès est connu sous le nom de liste de contrôle d’accès discrétionnaire (DACL, Discretionary Access Control List). Il se présente sous la forme : `D:<dacl_flags><string_ace1><string_ace2>`... `<string_acen>`<p>Les indicateurs DACL courants sont les suivants :<p>-  **.** Autoriser l’accès<p>- **D**. Refuser l’accès<p>Les droits courants sont les suivants :<p>- **GA**. Tout accès<p>- **GR**. Accès en lecture<p>- **GW**. Accès en écriture<p>Les comptes d’utilisateur courants sont les suivants :<p>- **BA**. Administrateurs intégrés<p>- **au**. Utilisateurs authentifiés<p>- **co**. Créateur propriétaire<p>- **WD**. Tout le monde<p>Exemples :<p>**D :P: (A ;; GR ;;;** La valeur au fournit un accès en lecture à tous les utilisateurs authentifiés.<p>**D :P: (A ;; GA ;;; WD** offre un accès complet à tous. |
| usefilesd | Spécifie que le descripteur de sécurité du fichier. vhd doit être utilisé sur le disque dur virtuel. Si le paramètre **Usefilesd** n’est pas spécifié, le disque dur virtuel n’aura pas de descripteur de sécurité explicite, sauf s’il est spécifié avec le paramètre **SD** . |
| noerr | Utilisé uniquement pour les scripts. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |

## <a name="remarks"></a>Notes

- Pour que cette opération aboutisse, vous devez sélectionner et détacher un disque dur virtuel. Utilisez la commande **Select vdisk** pour sélectionner un disque dur virtuel et lui déplacer le focus.

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

Pour attacher le disque dur virtuel sélectionné en lecture seule, tapez :

```
attach vdisk readonly
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [Compact vdisk](compact-vdisk.md)

- [détailler vdisk](detail-vdisk.md)

- [détacher vdisk](detach-vdisk.md)

- [développer vdisk](expand-vdisk.md)

- [Merge vdisk](merge-vdisk.md)

- [sélectionner vdisk](select-vdisk.md)

- [tarifs](list_1.md)
