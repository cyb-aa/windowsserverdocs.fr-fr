---
title: 'secedit : analyser'
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3430cf9d-1411-48b1-b5a9-2e47701dc87f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5ae331b890d0e4201e8b621e5d84f5025934efa0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722093"
---
# <a name="seceditanalyze"></a>secedit : analyser



Vous permet d’analyser les paramètres système actuels par rapport aux paramètres de base stockés dans une base de données.

## <a name="syntax"></a>Syntaxe

```
Secedit /analyze /db <database file name> [/cfg <configuration file name>] [/overwrite] [/log <log file name>] [/quiet}]
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|db|Obligatoire.</br>Spécifie le chemin d’accès et le nom de fichier d’une base de données qui contient la configuration stockée sur laquelle l’analyse doit être effectuée.</br>Si le nom de fichier spécifie une base de données qui n’a pas de modèle de sécurité (tel que représenté par le fichier `/cfg \<configuration file name>` de configuration) qui lui est associé, l’option de ligne de commande doit également être spécifiée.|
|cfg|facultatif.</br>Spécifie le chemin d’accès et le nom de fichier pour le modèle de sécurité qui sera importé dans la base de données à des fins d’analyse.</br>Cette option/cfg est valide uniquement lorsqu’elle est utilisée `/db \<database file name>` avec le paramètre. Si ce n’est pas spécifié, l’analyse est effectuée sur toute configuration déjà stockée dans la base de données.|
|overwrite|facultatif.</br>Spécifie si le modèle de sécurité dans le paramètre/cfg doit remplacer tout modèle ou modèle composite qui est stocké dans la base de données au lieu d’ajouter les résultats au modèle stocké.</br>Cette option de ligne de commande est valide uniquement lorsque `/cfg \<configuration file name>` le paramètre est également utilisé. Si cette valeur n’est pas spécifiée, le modèle dans le paramètre/cfg est ajouté au modèle stocké.|
|log|facultatif.</br>Spécifie le chemin d’accès et le nom du fichier journal à utiliser dans le processus.|
|calme|facultatif.</br>Supprime la sortie de l’écran. Vous pouvez toujours afficher les résultats de l’analyse en utilisant le composant logiciel enfichable Configuration et analyse de la sécurité de la console MMC (Microsoft Management Console).|

## <a name="remarks"></a>Notes 

Les résultats de l’analyse sont stockés dans une zone distincte de la base de données et peuvent être affichés dans le composant logiciel enfichable Configuration et analyse de la sécurité de la console MMC.

Si le chemin d’accès du fichier journal n’est pas fourni, le fichier journal par*systemroot*défaut, (SystemRoot\*\Documents and Settings UserAccount<em>\Mes Documents\Security\Logs\*DatabaseName</em>. log), est utilisé.

Dans Windows Server 2008, `Secedit /refreshpolicy` a été remplacé par `gpupdate`. Pour plus d’informations sur la façon d’actualiser les paramètres de sécurité, consultez [gpupdate](gpupdate.md).

## <a name="examples"></a>Exemples

Effectuez l’analyse des paramètres de sécurité sur la base de données de sécurité, SecDbContoso. sdb, que vous avez créée à l’aide du composant logiciel enfichable Configuration et analyse de la sécurité. Dirigez la sortie vers le fichier SecAnalysisContosoFY11 avec une invite pour vous permettre de vérifier que la commande a été exécutée correctement.
```
Secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /log C:\Security\FY11\SecAnalysisContosoFY11.log
```
Supposons que l’analyse a révélé des insuffisances, de sorte que le modèle de sécurité SecContoso. inf a été modifié. Réexécutez la commande pour incorporer les modifications, en dirigeant la sortie vers le fichier existant SecAnalysisContosoFY11 sans invite.
```
Secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /cfg SecContoso.inf /overwrite /log C:\Security\FY11\SecAnalysisContosoFY11.xml /quiet
```

## <a name="additional-references"></a>Références supplémentaires

-   [Secedit](secedit.md)
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)