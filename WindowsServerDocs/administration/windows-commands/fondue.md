---
title: fondue
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fc4467f6-ddbb-4d6d-b51e-5a50a957b8c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87af579d25e52543fe03159c40688f1e7540dcc9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844592"
---
# <a name="fondue"></a>fondue

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active les fonctionnalités Windows facultatives en téléchargeant les fichiers requis à partir de Windows Update ou d’une autre source spécifiée par stratégie de groupe. Le fichier manifeste de la fonctionnalité doit déjà être installé dans votre image Windows. 
## <a name="syntax"></a>Syntaxe
```
fondue.exe /enable-feature:<feature_name> [/caller-name:<program_name>] [/hide-ux:{all | rebootRequest}]
```
#### <a name="parameters"></a>Paramètres

|              Paramètre              |                                                                                                                                                                     Description                                                                                                                                                                     |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  /Enable-Feature : <*feature_name*>   |                                                                               Spécifie le nom de la fonctionnalité facultative Windows que vous souhaitez activer. Vous ne pouvez activer qu’une seule fonctionnalité par ligne de commande. Pour activer plusieurs fonctionnalités, utilisez fondue. exe pour chaque fonctionnalité.                                                                                |
|    /Caller-Name : <*program_name*>    |                                                                                 Spécifie le nom du programme ou du processus quand vous appelez fondue. exe à partir d’un script ou d’un fichier de commandes. Vous pouvez utiliser cette option pour ajouter le nom du programme au rapport SQM en cas d’erreur.                                                                                 |
| /Hide-UX : {All &#124; rebootRequest} | Utilisez **All** pour masquer tous les messages à l’utilisateur, y compris les demandes de progression et d’autorisation d’accès aux Windows Update. Si l’autorisation est requise, l’opération échoue.<p>Utilisez **rebootRequest** pour masquer uniquement les messages utilisateur qui demandent l’autorisation de redémarrer l’ordinateur. Utilisez cette option si vous avez un script qui contrôle les demandes de redémarrage. |

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre
Pour activer Microsoft .NET Framework 3,5, tapez :
```
fondue.exe /enable-feature:NETFX3
```
Pour activer Microsoft .NET Framework 3,5, ajoutez le nom du programme au rapport SQM et n’affichez pas de messages à l’utilisateur, tapez :
```
fondue.exe /enable-feature:NETFX3 /caller-name:Admin.bat /hide-ux:all
```
## <a name="additional-references"></a>Références supplémentaires
- - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  ## <a name="see-also"></a>Voir aussi
  [Considérations relatives au déploiement de Microsoft .NET Framework 3,5](https://go.microsoft.com/fwlink/?LinkId=248869)
