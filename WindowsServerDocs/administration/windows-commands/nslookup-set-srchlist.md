---
title: nslookup set srchlist
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 39b28e7d43df2427caae46d323cd30f03b6b484c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436574"
---
# <a name="nslookup-set-srchlist"></a>nslookup set srchlist

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifie la liste recherche et le nom des domaines du système DNS (Domain Name) par défaut.

## <a name="syntax"></a>Syntaxe
```
Set srchlist=<DomainName>[/...]
```
## <a name="parameters"></a>Paramètres

|    Paramètre    |                                                                                        Description                                                                                        |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  <DomainName>   | Spécifie les nouveaux noms pour la liste de recherche et de domaine DNS par défaut. La valeur de nom de domaine par défaut est basée sur le nom d’hôte. Vous pouvez spécifier un maximum de six noms séparés par des barres obliques (/). |
| {aide &#124; ?} |                                                                   Affiche un résumé de **nslookup** sous-commandes.                                                                   |

## <a name="remarks"></a>Notes
- Le **définir srchlist**option substitue à la liste par défaut DNS domaine recherche et le nom de la **définit le domaine** commande. Utilisez le **définir tout** commande pour afficher la liste.
  ## <a name="BKMK_examples"></a>Exemples
  L’exemple suivant définit le domaine DNS mfg.widgets.com et la liste de recherche pour les noms de trois :
  ```
  set srchlist=mfg.widgets.com/mrp2.widgets.com/widgets.com
  ```
  ## <a name="additional-references"></a>Références supplémentaires
  [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [nslookup définie domaine](nslookup-set-domain.md)
  [nslookup définie toutes les](nslookup-set-all.md)
