---
title: attach vdisk
description: Rubrique de référence pour la commande Attach vdisk, qui attache (ou Monte) un disque dur virtuel (VHD) afin qu’il apparaisse sur l’ordinateur hôte en tant que lecteur de disque dur local.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 882ab875-0c14-4eb3-98ef-fd0e8fa40d9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91f988d1f84869874dbd0d6a25dce43ef5138066
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718906"
---
# <a name="attach-vdisk"></a>attach vdisk

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Joint (parfois appelé montage ou surfaces) un disque dur virtuel (VHD) afin qu’il apparaisse sur l’ordinateur hôte en tant que lecteur de disque dur local. Si le VHD a déjà une partition de disque et un volume de système de fichiers lorsque vous l'attachez, une lettre de lecteur est assignée au volume contenu dans le disque dur virtuel.

> [!IMPORTANT]
> Vous devez choisir et détacher un disque dur virtuel pour que cette opération aboutisse. Utilisez la commande **Select vdisk** pour sélectionner un disque dur virtuel et lui déplacer le focus.

## <a name="syntax"></a>Syntaxe

```
attach vdisk [readonly] { [sd=<SDDL>] | [usefilesd] } [noerr]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| readonly | Attache le disque dur virtuel en lecture seule. Toute opération d’écriture retourne une erreur. |
| `sd=<SDDL string>` | Définit le filtre utilisateur sur le disque dur virtuel. La chaîne de filtrage doit être au format SDDL (Security Descriptor Definition Language). Par défaut, le filtre utilisateur autorise l’accès comme sur un disque physique. Les chaînes SDDL peuvent être complexes, mais dans sa forme la plus simple, un descripteur de sécurité qui protège l’accès est connu sous le nom de liste de contrôle d’accès discrétionnaire (DACL, Discretionary Access Control List). Elle utilise la forme suivante `D:<dacl_flags><string_ace1><string_ace2>`:...`<string_acen>`<p>Les indicateurs DACL courants sont les suivants :<ul><li>**.** Autoriser l’accès</li><li>**D**. Accès refusé</li></ul>Les droits courants sont les suivants :<ul><li>**GA**. Tout accès</li><li>**GR**. Accès en lecture</li><li> **GW**. Accès en écriture</li></ul>Les comptes d’utilisateur courants sont les suivants :<ul><li>**BA**. Administrateurs intégrés</li><li>**AU**Au. Utilisateurs authentifiés</li><li>**Co**. Créateur propriétaire</li><li>**WD**. Tout le monde</li></ul>Exemples :<ul><li>**D :P: (A ;; GR ;;; **Au. Octroie l’accès en lecture à tous les utilisateurs authentifiés.</li><li>**D :P: (A ;; GA ;;; WD**. Donne à tous les utilisateurs un accès complet.</li></ul> |
| usefilesd | Spécifie que le descripteur de sécurité du fichier. vhd doit être utilisé sur le disque dur virtuel. Si le paramètre **Usefilesd** n’est pas spécifié, le disque dur virtuel n’aura pas de descripteur de sécurité explicite, sauf s’il est spécifié avec le paramètre **SD** . |
| noerr | Utilisé uniquement pour les scripts. Lorsqu’une erreur se produit, DiskPart continue à traiter les commandes comme si l’erreur ne s’était pas produite. Sans ce paramètre, une erreur provoque la fermeture de DiskPart avec un code d’erreur. |

## <a name="examples"></a>Exemples

Pour attacher le disque dur virtuel sélectionné en lecture seule, tapez :

```
attach vdisk readonly
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [sélectionner vdisk](select-vdisk.md)

- [Compact vdisk](compact-vdisk.md)

- [détailler vdisk](detail-vdisk.md)

- [detach vdisk](detach-vdisk.md)

- [développer vdisk](expand-vdisk.md)

- [Merge vdisk](merge-vdisk.md)

- [list](list_1.md)
