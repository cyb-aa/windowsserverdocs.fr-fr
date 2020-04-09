---
title: nslookup ls
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f15f06fe-67e7-41a9-93b5-192ab14ab380
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dbaef500410d6427beb008109abcc2c339b8d65c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838732"
---
# <a name="nslookup-ls"></a>nslookup ls

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Répertorie des informations pour un domaine DNS (Domain Name System).
## <a name="syntax"></a>Syntaxe
```
ls [<Option>] <DNSDomain> [{[>] <FileName>|[>>] <FileName>}]
```
### <a name="parameters"></a>Paramètres

|    Paramètre    |                                                                                                                                                                                                                                                                                                               Description                                                                                                                                                                                                                                                                                                                |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <Option>     | Le tableau suivant répertorie les options valides.<p>--t : répertorie tous les enregistrements du type spécifié. Pour obtenir une description de <querytype>, consultez **SetQueryType** dans Références supplémentaires.<br />--a : répertorie les alias des ordinateurs dans le domaine DNS. Ce paramètre est un synonyme de **-t CNAME**<br />--d : répertorie tous les enregistrements du domaine DNS. Ce paramètre est un synonyme de **-t any**<br />--h : répertorie les informations du processeur et du système d’exploitation pour le domaine DNS. Ce paramètre est un synonyme de **-t HINFO**<br />--s : répertorie les services connus des ordinateurs dans le domaine DNS. Ce paramètre est un synonyme de **-t WKS**. |
|   <DNSDomain>   |                                                                                                                                                                                                                                                                                         Spécifie le domaine DNS pour lequel vous souhaitez obtenir des informations.                                                                                                                                                                                                                                                                                         |
|   <FileName>    |                                                                                                                                                                                                                                 Spécifie un nom de fichier dans lequel enregistrer la sortie. Vous pouvez utiliser les caractères supérieur à (>) et double supérieur à (> >) pour rediriger la sortie de manière habituelle.                                                                                                                                                                                                                                  |
| {Help &#124; ?} |                                                                                                                                                                                                                                                                                          Affiche un bref résumé des sous-commandes **nslookup** .                                                                                                                                                                                                                                                                                           |

## <a name="remarks"></a>Notes
- La sortie par défaut contient les noms des ordinateurs et leurs adresses IP. Lorsque la sortie est dirigée vers un fichier, les marques de hachage sont imprimées pour chaque enregistrement 50 reçu du serveur
  ## <a name="additional-references"></a>Références supplémentaires
  - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [nslookup Set QueryType](nslookup-set-querytype.md)
