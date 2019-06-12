---
title: fondue
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: bcbbbf80f25f77d1feb83f358401e4d14da3d354
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439223"
---
# <a name="fondue"></a>fondue

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active les fonctionnalités facultatives de Windows en téléchargeant les fichiers requis à partir de la mise à jour de Windows ou une autre source spécifié par la stratégie de groupe. Le fichier manifeste de la fonctionnalité doit déjà être installé dans votre image de Windows. 
## <a name="syntax"></a>Syntaxe
```
fondue.exe /enable-feature:<feature_name> [/caller-name:<program_name>] [/hide-ux:{all | rebootRequest}]
```
### <a name="parameters"></a>Paramètres

|              Paramètre              |                                                                                                                                                                     Description                                                                                                                                                                     |
|-------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  Enable : <*nom_fonctionnalité*>   |                                                                               Spécifie le nom de la fonctionnalité facultative de Windows que vous souhaitez activer. Vous ne pouvez activer une fonctionnalité par ligne de commande. Pour activer plusieurs fonctionnalités, utilisez fondue.exe pour chaque fonctionnalité.                                                                                |
|    /caller-Name : <*nom_programme*>    |                                                                                 Spécifie le nom de programme ou processus lorsque vous appelez fondue.exe à partir d’un fichier de script ou lot. Vous pouvez utiliser cette option pour ajouter le nom du programme pour le rapport SQM s’il existe une erreur.                                                                                 |
| /hide-ux:{all &#124; rebootRequest} | Utilisez **tous les** pour masquer tous les messages à l’utilisateur, y compris les demandes de progression et l’autorisation pour accéder à Windows Update. Si l’autorisation est requise, l’opération échoue.<br /><br />Utilisez **rebootRequest** pour masquer uniquement les messages de l’utilisateur demandant l’autorisation de redémarrer l’ordinateur. Utilisez cette option si vous avez un script de contrôles des demandes de redémarrage. |

## <a name="BKMK_Examples"></a>Exemples
Pour activer Microsoft .NET Framework 3.5, tapez :
```
fondue.exe /enable-feature:NETFX3
```
Pour activer Microsoft .NET Framework 3.5, ajouter le nom du programme à l’état SQM et n’affiche pas de messages à l’utilisateur, type :
```
fondue.exe /enable-feature:NETFX3 /caller-name:Admin.bat /hide-ux:all
```
## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  ## <a name="see-also"></a>Voir aussi
  [Considérations relatives au déploiement de Microsoft .NET Framework 3.5](https://go.microsoft.com/fwlink/?LinkId=248869)
