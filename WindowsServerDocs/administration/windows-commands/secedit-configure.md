---
title: 'secedit : configurer'
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a92e68ca-003c-4219-8655-0e7734f5fab3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4d659429f4aa60d7c12ae0ba93e068cc0c99c093
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722078"
---
# <a name="seceditconfigure"></a>secedit : configurer



Vous permet de configurer les paramètres système actuels à l’aide des paramètres de sécurité stockés dans une base de données.

## <a name="syntax"></a>Syntaxe

```
Secedit /configure /db <database file name> [/cfg <configuration file name>] [/overwrite] [/areas SECURITYPOLICY | GROUP_MGMT | USER_RIGHTS | REGKEYS | FILESTORE | SERVICES] [/log <log file name>] [/quiet]
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|db|Obligatoire.</br>Spécifie le chemin d’accès et le nom de fichier d’une base de données qui contient la configuration stockée.</br>Si le nom de fichier spécifie une base de données qui n’a pas de modèle de sécurité (tel que représenté par le fichier `/cfg \<configuration file name>` de configuration) qui lui est associé, l’option de ligne de commande doit également être spécifiée.|
|cfg|facultatif.</br>Spécifie le chemin d’accès et le nom de fichier pour le modèle de sécurité qui sera importé dans la base de données à des fins d’analyse.</br>Cette option/cfg est valide uniquement lorsqu’elle est utilisée `/db \<database file name>` avec le paramètre. Si ce n’est pas spécifié, l’analyse est effectuée sur toute configuration déjà stockée dans la base de données.|
|overwrite|facultatif.</br>Spécifie si le modèle de sécurité dans le paramètre/cfg doit remplacer tout modèle ou modèle composite qui est stocké dans la base de données au lieu d’ajouter les résultats au modèle stocké.</br>Cette option de ligne de commande est valide uniquement lorsque `/cfg \<configuration file name>` le paramètre est également utilisé. Si cette valeur n’est pas spécifiée, le modèle dans le paramètre/cfg est ajouté au modèle stocké.|
|zones|facultatif.</br>Spécifie les zones de sécurité à appliquer au système. Si ce paramètre n’est pas spécifié, tous les paramètres de sécurité définis dans la base de données sont appliqués au système. Pour configurer plusieurs zones, séparez chaque zone par un espace. Les zones de sécurité suivantes sont prises en charge :</br>-SecurityPolicy</br>    Stratégie locale et stratégie de domaine pour le système, y compris les stratégies de compte, les stratégies d’audit, les options de sécurité, etc.</br>-Group_Mgmt</br>    Paramètres de groupe restreints pour tous les groupes spécifiés dans le modèle de sécurité.</br>-User_Rights</br>    Droits d’ouverture de session utilisateur et octroi de privilèges.</br>- RegKeys</br>    Sécurité sur les clés de Registre locales.</br>-Cache de pages</br>    Sécurité sur le stockage de fichiers local.</br>-Services</br>    Sécurité pour tous les services définis.|
|log|facultatif.</br>Spécifie le chemin d’accès et le nom du fichier journal pour le processus.|
|calme|facultatif.</br>Supprime la sortie de l’écran et du journal. Vous pouvez toujours afficher les résultats de l’analyse en utilisant le composant logiciel enfichable Configuration et analyse de la sécurité de la console MMC (Microsoft Management Console).|

## <a name="remarks"></a>Notes 

Si le chemin d’accès du fichier journal n’est pas fourni, le fichier journal par*systemroot*défaut, \*(SystemRoot \Utilisateurs UserAccount<em>\Mes Documents\Security\Logs\*DatabaseName</em>. log), est utilisé.

À partir de Windows Server 2008 `Secedit /refreshpolicy` , a été remplacé `gpupdate`par. Pour plus d’informations sur la façon d’actualiser les paramètres de sécurité, consultez [gpupdate](gpupdate.md).

## <a name="examples"></a>Exemples

Effectuez l’analyse des paramètres de sécurité sur la base de données de sécurité, SecDbContoso. sdb, que vous avez créée à l’aide du composant logiciel enfichable Configuration et analyse de la sécurité. Dirigez la sortie vers le fichier SecAnalysisContosoFY11 avec une invite pour vous permettre de vérifier que la commande a été exécutée correctement.
```
Secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /log C:\Security\FY11\SecAnalysisContosoFY11.log
```
Supposons que l’analyse a révélé des insuffisances, de sorte que le modèle de sécurité SecContoso. inf a été modifié. Réexécutez la commande pour incorporer les modifications, en dirigeant la sortie vers le fichier existant SecAnalysisContosoFY11 sans invite.
```
Secedit /configure /db C:\Security\FY11\SecDbContoso.sdb /cfg SecContoso.inf /overwrite /log C:\Security\FY11\SecAnalysisContosoFY11.xml /quiet
```

## <a name="additional-references"></a>Références supplémentaires

-   [Secedit](secedit.md)
-   [Secedit:analyze](secedit-analyze.md)
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)