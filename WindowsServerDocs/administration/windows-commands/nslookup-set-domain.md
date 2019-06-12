---
title: nslookup set domain
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9d4d28e8-6e88-42cc-801f-94e9d8e051f4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d1af9f30dd2c44111adecb477a6469333f4f7685
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436769"
---
# <a name="nslookup-set-domain"></a>nslookup set domain

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifie le nom de domaine du système DNS (Domain Name) par défaut pour le nom spécifié.
## <a name="syntax"></a>Syntaxe
```
set domain=<DomainName>
```
## <a name="parameters"></a>Paramètres

|    Paramètre    |                                           Description                                           |
|-----------------|-------------------------------------------------------------------------------------------------|
|  <DomainName>   | Spécifie un nouveau nom pour le nom de domaine DNS par défaut. Le nom de domaine par défaut est le nom d’hôte. |
| {aide &#124; ?} |                      Affiche un résumé de **nslookup** sous-commandes.                      |

## <a name="remarks"></a>Notes
- Le nom de domaine DNS par défaut est ajouté à une demande de recherche selon l’état de la **defname** et **recherche** options. La liste de recherche du domaine DNS contient les parents du domaine DNS par défaut si elle a au moins deux composants dans son nom. Par exemple, si le domaine DNS par défaut est mfg.widgets.com, la liste de recherche est nommée mfg.widgets.com et widgets.com. Utilisez le **définir srchlist** commande pour spécifier une liste différente et la **définir tout** commande pour afficher la liste.
  ## <a name="additional-references"></a>Références supplémentaires
  [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [nslookup définie srchlist](nslookup-set-srchlist.md)
  [nslookup définie toutes les](nslookup-set-all.md)
