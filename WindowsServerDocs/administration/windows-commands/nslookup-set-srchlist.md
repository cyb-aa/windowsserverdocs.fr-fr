---
title: nslookup set srchlist
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8486266d-22ac-4ce5-aad6-1cd0c08110a2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fb93a9f7cf969161536e88bec929b7e6ba0f0e5d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372769"
---
# <a name="nslookup-set-srchlist"></a>nslookup set srchlist

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

modifie la liste de recherche et le nom de domaine DNS (Domain Name System) par défaut.

## <a name="syntax"></a>Syntaxe
```
Set srchlist=<DomainName>[/...]
```
## <a name="parameters"></a>Paramètres

|    Paramètre    |                                                                                        Description                                                                                        |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <DomainName>   | Spécifie de nouveaux noms pour la liste de recherche et le domaine DNS par défaut. La valeur du nom de domaine par défaut est basée sur le nom d’hôte. Vous pouvez spécifier un maximum de six noms séparés par des barres obliques (/). |
| {Help &#124; ?} |                                                                   Affiche un bref résumé des sous-commandes **nslookup** .                                                                   |

## <a name="remarks"></a>Notes
- La commande **Set srchlist**remplace la liste de recherche et le nom de domaine DNS par défaut de la commande **Set Domain** . Utilisez la commande **Set All** pour afficher la liste.
  ## <a name="BKMK_examples"></a>Illustre
  L’exemple suivant définit le domaine DNS sur mfg.widgets.com et la liste de recherche sur les trois noms :
  ```
  set srchlist=mfg.widgets.com/mrp2.widgets.com/widgets.com
  ```
  ## <a name="additional-references"></a>Références supplémentaires
  [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [nslookup Set Domain](nslookup-set-domain.md)
  [nslookup Set All](nslookup-set-all.md)
