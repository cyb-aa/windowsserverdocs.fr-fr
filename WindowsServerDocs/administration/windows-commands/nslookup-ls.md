---
title: nslookup ls
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f15f06fe-67e7-41a9-93b5-192ab14ab380
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 632d25e29c09d7a164668128196964d082e160c3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848520"
---
# <a name="nslookup-ls"></a>nslookup ls

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche des informations pour un domaine du système DNS (Domain Name).
## <a name="syntax"></a>Syntaxe
```
ls [<Option>] <DNSDomain> [{[>] <FileName>|[>>] <FileName>}]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|<Option>|Le tableau suivant répertorie les options valides.<br /><br />--t : répertorie tous les enregistrements du type spécifié. Pour obtenir une description de <querytype>, consultez **setquerytype** dans des références supplémentaires.<br />--r : répertorie les alias des ordinateurs dans le domaine DNS. Ce paramètre est un synonyme de **- t CNAME**<br />--d: répertorie tous les enregistrements pour le domaine DNS. Ce paramètre est un synonyme de **- t ANY**<br />--h : répertorie les informations de processeur et de système d’exploitation pour le domaine DNS. Ce paramètre est un synonyme de **- t HINFO**<br />--s: répertorie des services bien connus des ordinateurs dans le domaine DNS. Ce paramètre est un synonyme de **-t WKS**.|
|<DNSDomain>|Spécifie le domaine DNS pour lesquels vous voulez des informations.|
|<FileName>|Spécifie un nom de fichier dans lequel enregistrer la sortie. Vous pouvez utiliser le signe supérieur à (>) et double supérieur (>>) caractères pour rediriger la sortie de la manière habituelle.|
|{aide &#124; ?}|Affiche un résumé de **nslookup** sous-commandes.|
## <a name="remarks"></a>Notes
-   La sortie par défaut contient les noms d’ordinateurs et leur adresse IP adresses. Lors de la sortie est dirigée vers un fichier, les marques de hachage sont imprimés pour tous les 50 enregistrements reçus à partir du serveur
## <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[nslookup définie querytype](nslookup-set-querytype.md)
