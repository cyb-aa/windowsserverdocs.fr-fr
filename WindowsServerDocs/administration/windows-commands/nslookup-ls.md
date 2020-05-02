---
title: nslookup ls
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f15f06fe-67e7-41a9-93b5-192ab14ab380
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8357208406c0fca5d68da419baa2092d94fa9ec9
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723705"
---
# <a name="nslookup-ls"></a>nslookup ls

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Répertorie des informations pour un domaine DNS (Domain Name System).
## <a name="syntax"></a>Syntaxe
```
ls [<Option>] <DNSDomain> [{[>] <FileName>|[>>] <FileName>}]
```
### <a name="parameters"></a>Paramètres

|    Paramètre    |                                                                                                                                                                                                                                                                                                               Description                                                                                                                                                                                                                                                                                                                |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <Option>     | Le tableau suivant répertorie les options valides.<p>--t : répertorie tous les enregistrements du type spécifié. Pour obtenir une description <querytype>de, consultez **SetQueryType** dans Références supplémentaires.<br />--a : répertorie les alias des ordinateurs dans le domaine DNS. Ce paramètre est un synonyme de **-t CNAME**<br />--d : répertorie tous les enregistrements du domaine DNS. Ce paramètre est un synonyme de **-t any**<br />--h : répertorie les informations du processeur et du système d’exploitation pour le domaine DNS. Ce paramètre est un synonyme de **-t HINFO**<br />--s : répertorie les services connus des ordinateurs dans le domaine DNS. Ce paramètre est un synonyme de **-t WKS**. |
|   <DNSDomain>   |                                                                                                                                                                                                                                                                                         Spécifie le domaine DNS pour lequel vous souhaitez obtenir des informations.                                                                                                                                                                                                                                                                                         |
|   <FileName>    |                                                                                                                                                                                                                                 Spécifie un nom de fichier dans lequel enregistrer la sortie. Vous pouvez utiliser les caractères supérieur à (>) et double supérieur à (>>) pour rediriger la sortie de manière habituelle.                                                                                                                                                                                                                                  |
| {Help &#124; ?} |                                                                                                                                                                                                                                                                                          Affiche un bref résumé des sous-commandes **nslookup** .                                                                                                                                                                                                                                                                                           |

## <a name="remarks"></a>Notes 
- La sortie par défaut contient les noms des ordinateurs et leurs adresses IP. Lorsque la sortie est dirigée vers un fichier, les marques de hachage sont imprimées pour chaque enregistrement 50 reçu du serveur
  ## <a name="additional-references"></a>Références supplémentaires
  - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [nslookup Set QueryType](nslookup-set-querytype.md)
