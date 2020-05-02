---
title: nslookup set domain
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9d4d28e8-6e88-42cc-801f-94e9d8e051f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f6ee52cd5ad35dcfc6da1cd3885f66124338d62f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723616"
---
# <a name="nslookup-set-domain"></a>nslookup set domain

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

remplace le nom de domaine DNS (Domain Name System) par défaut par le nom spécifié.
## <a name="syntax"></a>Syntaxe
```
set domain=<DomainName>
```
### <a name="parameters"></a>Paramètres

|    Paramètre    |                                           Description                                           |
|-----------------|-------------------------------------------------------------------------------------------------|
|  <DomainName>   | Spécifie un nouveau nom pour le nom de domaine DNS par défaut. Le nom de domaine par défaut est le nom d’hôte. |
| {Help &#124; ?} |                      Affiche un bref résumé des sous-commandes **nslookup** .                      |

## <a name="remarks"></a>Notes 
- Le nom de domaine DNS par défaut est ajouté à une demande de recherche en fonction de l’état des **defname** et des options de **recherche** . La liste de recherche de domaine DNS contient les parents du domaine DNS par défaut s’il a au moins deux composants dans son nom. Par exemple, si le domaine DNS par défaut est mfg.widgets.com, la liste de recherche est nommée à la fois mfg.widgets.com et widgets.com. Utilisez la commande **Set srchlist** pour spécifier une autre liste et la commande **Set All** pour afficher la liste.
  ## <a name="additional-references"></a>Références supplémentaires
  - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [nslookup Set srchlist](nslookup-set-srchlist.md)
  [nslookup Set All](nslookup-set-all.md)
