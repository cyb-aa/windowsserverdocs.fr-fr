---
title: fondue
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fc4467f6-ddbb-4d6d-b51e-5a50a957b8c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d75d2d9fb57f8888cfc5bf50e2f7796aefc66102
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377088"
---
# <a name="fondue"></a>fondue

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Active les fonctionnalités Windows facultatives en téléchargeant les fichiers requis à partir de Windows Update ou d’une autre source spécifiée par stratégie de groupe. Le fichier manifeste de la fonctionnalité doit déjà être installé dans votre image Windows. 
## <a name="syntax"></a>Syntaxe
```
fondue.exe /enable-feature:<feature_name> [/caller-name:<program_name>] [/hide-ux:{all | rebootRequest}]
```
### <a name="parameters"></a>Paramètres

|              Paramètre              |                                                                                                                                                                     Description                                                                                                                                                                     |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  /Enable-Feature : <*feature_name*>   |                                                                               Spécifie le nom de la fonctionnalité facultative Windows que vous souhaitez activer. Vous ne pouvez activer qu’une seule fonctionnalité par ligne de commande. Pour activer plusieurs fonctionnalités, utilisez fondue. exe pour chaque fonctionnalité.                                                                                |
|    /Caller-Name : <*nom_programme*>    |                                                                                 Spécifie le nom du programme ou du processus quand vous appelez fondue. exe à partir d’un script ou d’un fichier de commandes. Vous pouvez utiliser cette option pour ajouter le nom du programme au rapport SQM en cas d’erreur.                                                                                 |
| /Hide-UX : {All &#124; rebootRequest} | Utilisez **All** pour masquer tous les messages à l’utilisateur, y compris les demandes de progression et d’autorisation d’accès aux Windows Update. Si l’autorisation est requise, l’opération échoue.<br /><br />Utilisez **rebootRequest** pour masquer uniquement les messages utilisateur qui demandent l’autorisation de redémarrer l’ordinateur. Utilisez cette option si vous avez un script qui contrôle les demandes de redémarrage. |

## <a name="BKMK_Examples"></a>Illustre
Pour activer Microsoft .NET Framework 3,5, tapez :
```
fondue.exe /enable-feature:NETFX3
```
Pour activer Microsoft .NET Framework 3,5, ajoutez le nom du programme au rapport SQM et n’affichez pas de messages à l’utilisateur, tapez :
```
fondue.exe /enable-feature:NETFX3 /caller-name:Admin.bat /hide-ux:all
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  ## <a name="see-also"></a>Voir aussi
  [Considérations relatives au déploiement de Microsoft .NET Framework 3,5](https://go.microsoft.com/fwlink/?LinkId=248869)
