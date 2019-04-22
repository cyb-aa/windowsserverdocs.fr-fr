---
title: secedit:generaterollback
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 385a6799-51a7-4fe3-bd73-10c7998b6680
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d7a55ddc3caea1002ab51ce4f992b36673ea312b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825640"
---
# <a name="seceditgeneraterollback"></a>secedit:generaterollback



Vous permet de générer un modèle de restauration pour un modèle de configuration spécifié. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
Secedit /generaterollback /db <database file name> /cfg <configuration file name> /rbk <rollback template file name> [log <log file name>] [/quiet]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|db|Obligatoire.</br>Spécifie le chemin d’accès et le nom d’une base de données qui contient la configuration stockée par rapport à laquelle l’analyse sera effectuée.</br>Si le nom de fichier spécifie une base de données qui n’a pas un modèle de sécurité (comme représenté par le fichier de configuration) associé, le `/cfg \<configuration file name>` option de ligne de commande doit également être spécifiée.|
|cfg|Obligatoire.</br>Spécifie le chemin d’accès et le nom du modèle de sécurité qui sera importé dans la base de données pour l’analyse.</br>Cette option /cfg est uniquement valide lorsqu’il est utilisé avec le `/db \<database file name>` paramètre. S’il n’est pas spécifié, l’analyse est exécutée sur n’importe quelle configuration déjà stockée dans la base de données.|
|rbk|Obligatoire.</br>Spécifie un modèle de sécurité dans lequel les informations de restauration sont écrit. Modèles de sécurité sont créés à l’aide du composant logiciel enfichable modèles de sécurité. Fichiers de restauration peuvent être créés avec cette commande.|
|journal|Facultatif.</br>Spécifie le chemin d’accès et le nom du fichier journal pour le processus.|
|quiet|Facultatif.</br>Supprime la sortie écran et journal. Vous pouvez toujours afficher les résultats analyse à l’aide du composant logiciel enfichable Configuration de la sécurité et l’analyse à la Console MMC (Microsoft Management).|

## <a name="remarks"></a>Notes

Si le chemin d’accès du fichier journal n’est pas fourni, le fichier journal par défaut, (*systemroot*\Users \*UserAccount*\My Documents\Security\Logs\*DatabaseName*.log) est utilisé.

À partir de Windows Server 2008, `Secedit /refreshpolicy` a été remplacé par `gpupdate`. Pour plus d’informations sur l’actualisation des paramètres de sécurité, consultez [Gpupdate](gpupdate.md).

L’état de l’exécution réussie de cette commande « la tâche est terminée avec succès. ? journaux et uniquement les incompatibilités entre le modèle de sécurité et la configuration de stratégie de sécurité. Il répertorie ces incompatibilités dans le scesrv.log.

Si un modèle de restauration existant est spécifié, cette commande remplace. Vous pouvez créer un nouveau modèle de restauration avec cette commande. Aucuns paramètres supplémentaires ne sont nécessaires pour des conditions.

## <a name="BKMK_Examples"></a>Exemples

Après avoir créé le modèle de sécurité à l’aide de la Configuration et la sécurité enfichable analyse, SecTmplContoso.inf, créez le fichier de configuration de restauration pour enregistrer les paramètres d’origine. Écrire l’action dans le fichier journal FY11.
```
Secedit /generaterollback /db C:\Security\FY11\SecDbContoso.sdb /cfg sectmplcontoso.inf /rbk sectmplcontosoRBK.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Secedit](secedit.md)
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)