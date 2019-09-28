---
title: 'secedit : generaterollback'
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 3ce4bd83e6eda24c10f65bd9d450a204906ff7fd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384221"
---
# <a name="seceditgeneraterollback"></a>secedit : generaterollback



Vous permet de générer un modèle de restauration pour un modèle de configuration spécifié. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
Secedit /generaterollback /db <database file name> /cfg <configuration file name> /rbk <rollback template file name> [log <log file name>] [/quiet]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|bases|Obligatoire.</br>Spécifie le chemin d’accès et le nom de fichier d’une base de données qui contient la configuration stockée sur laquelle l’analyse doit être effectuée.</br>Si le nom de fichier spécifie une base de données qui n’a pas de modèle de sécurité (tel que représenté par le fichier `/cfg \<configuration file name>` de configuration) qui lui est associé, l’option de ligne de commande doit également être spécifiée.|
|cfg|Obligatoire.</br>Spécifie le chemin d’accès et le nom de fichier pour le modèle de sécurité qui sera importé dans la base de données à des fins d’analyse.</br>Cette option/cfg est valide uniquement lorsqu’elle est utilisée `/db \<database file name>` avec le paramètre. Si ce n’est pas spécifié, l’analyse est effectuée sur toute configuration déjà stockée dans la base de données.|
|rbk|Obligatoire.</br>Spécifie un modèle de sécurité dans lequel les informations de restauration sont écrites. Les modèles de sécurité sont créés à l’aide du composant logiciel enfichable modèles de sécurité. Vous pouvez créer des fichiers de restauration à l’aide de cette commande.|
|Sign|Facultatif.</br>Spécifie le chemin d’accès et le nom du fichier journal pour le processus.|
|Silencieux|Facultatif.</br>Supprime la sortie de l’écran et du journal. Vous pouvez toujours afficher les résultats de l’analyse en utilisant le composant logiciel enfichable Configuration et analyse de la sécurité de la console MMC (Microsoft Management Console).|

## <a name="remarks"></a>Notes

Si le chemin d’accès du fichier journal n’est pas fourni, le fichier journal pardéfaut, \*(SystemRoot \Utilisateurs UserAccount<em>\*\Mes Documents\Security\Logs DatabaseName</em>. log), est utilisé.

À partir de Windows Server 2008 `Secedit /refreshpolicy` , a été remplacé `gpupdate`par. Pour plus d’informations sur la façon d’actualiser les paramètres de sécurité, consultez [gpupdate](gpupdate.md).

La réussite de l’exécution de cette commande indique « la tâche s’est terminée avec succès ». et journalise uniquement les incompatibilités entre le modèle de sécurité et la configuration de stratégie de sécurité indiqués. Elle répertorie ces incompatibilités dans le fichier scesrv. log.

Si un modèle de restauration existant est spécifié, cette commande le remplacera. Vous pouvez créer un nouveau modèle de restauration à l’aide de cette commande. Aucun paramètre supplémentaire n’est nécessaire pour les deux conditions.

## <a name="BKMK_Examples"></a>Illustre

Après avoir créé le modèle de sécurité à l’aide du composant logiciel enfichable Configuration et analyse de la sécurité, SecTmplContoso. inf, créez le fichier de configuration de restauration pour enregistrer les paramètres d’origine. Écrivez l’action dans le fichier journal FY11.
```
Secedit /generaterollback /db C:\Security\FY11\SecDbContoso.sdb /cfg sectmplcontoso.inf /rbk sectmplcontosoRBK.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Secedit](secedit.md)
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)