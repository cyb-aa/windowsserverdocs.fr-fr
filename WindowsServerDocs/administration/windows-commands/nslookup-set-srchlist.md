---
title: nslookup set srchlist
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8486266d-22ac-4ce5-aad6-1cd0c08110a2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bbeb09501474ade670147a6021abd2bb25291d71
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838282"
---
# <a name="nslookup-set-srchlist"></a>nslookup set srchlist

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifie la liste de recherche et le nom de domaine DNS (Domain Name System) par défaut.

## <a name="syntax"></a>Syntaxe
```
Set srchlist=<DomainName>[/...]
```
### <a name="parameters"></a>Paramètres

|    Paramètre    |                                                                                        Description                                                                                        |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <DomainName>   | Spécifie de nouveaux noms pour la liste de recherche et le domaine DNS par défaut. La valeur du nom de domaine par défaut est basée sur le nom d’hôte. Vous pouvez spécifier un maximum de six noms séparés par des barres obliques (/). |
| {Help &#124; ?} |                                                                   Affiche un bref résumé des sous-commandes **nslookup** .                                                                   |

## <a name="remarks"></a>Notes
- La commande **Set srchlist**remplace la liste de recherche et le nom de domaine DNS par défaut de la commande **Set Domain** . Utilisez la commande **Set All** pour afficher la liste.
  ## <a name="examples"></a><a name=BKMK_examples></a>Illustre
  L’exemple suivant définit le domaine DNS sur mfg.widgets.com et la liste de recherche sur les trois noms :
  ```
  set srchlist=mfg.widgets.com/mrp2.widgets.com/widgets.com
  ```
  ## <a name="additional-references"></a>Références supplémentaires
  - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [nslookup Set Domain](nslookup-set-domain.md)
  [nslookup Set All](nslookup-set-all.md)
