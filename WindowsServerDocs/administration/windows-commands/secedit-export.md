---
title: 'secedit : export'
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 49a8b241-aa8c-45b7-844d-67a29fab708e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ea0181bcdcae8d3869327985a0db0601ded6505d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834972"
---
# <a name="seceditexport"></a>secedit : export



Exporte les paramètres de sécurité stockés dans une base de données configurée avec des modèles de sécurité. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
Secedit /export /db <database file name> [/mergedpolicy] /cfg <configuration file name> [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|bases|Obligatoire.</br>Spécifie le chemin d’accès et le nom de fichier d’une base de données qui contient la configuration stockée sur laquelle l’analyse doit être effectuée.</br>Si le nom de fichier spécifie une base de données qui n’a pas de modèle de sécurité (tel que représenté par le fichier de configuration) qui lui est associé, l’option de ligne de commande `/cfg \<configuration file name>` doit également être spécifiée.|
|mergedpolicy|Ce paramètre est facultatif.</br>Fusionne et exporte les paramètres de sécurité de la stratégie locale et du domaine.|
|cfg|Obligatoire.</br>Spécifie le chemin d’accès et le nom de fichier pour le modèle de sécurité qui sera importé dans la base de données à des fins d’analyse.</br>Cette option/cfg est valide uniquement lorsqu’elle est utilisée avec le paramètre `/db \<database file name>`. Si ce n’est pas spécifié, l’analyse est effectuée sur toute configuration déjà stockée dans la base de données.|
|zones|Ce paramètre est facultatif.</br>Spécifie les zones de sécurité à appliquer au système. Si ce paramètre n’est pas spécifié, tous les paramètres de sécurité définis dans la base de données sont appliqués au système. Pour configurer plusieurs zones, séparez chaque zone par un espace. Les zones de sécurité suivantes sont prises en charge :</br>-SecurityPolicy</br>    Stratégie locale et stratégie de domaine pour le système, y compris les stratégies de compte, les stratégies d’audit, les options de sécurité, etc.</br>-Group_Mgmt</br>    Paramètres de groupe restreints pour tous les groupes spécifiés dans le modèle de sécurité.</br>-User_Rights</br>    Droits d’ouverture de session utilisateur et octroi de privilèges.</br>- RegKeys</br>    Sécurité sur les clés de Registre locales.</br>-Cache de pages</br>    Sécurité sur le stockage de fichiers local.</br>-Services</br>    Sécurité pour tous les services définis.|
|log|Ce paramètre est facultatif.</br>Spécifie le chemin d’accès et le nom du fichier journal pour le processus.|
|silencieux|Ce paramètre est facultatif.</br>Supprime la sortie de l’écran et du journal. Vous pouvez toujours afficher les résultats de l’analyse en utilisant le composant logiciel enfichable Configuration et analyse de la sécurité de la console MMC (Microsoft Management Console).|

## <a name="remarks"></a>Notes

Vous pouvez utiliser cette commande pour sauvegarder vos stratégies de sécurité sur un ordinateur local en plus d’importer les paramètres sur un autre ordinateur.

Si le chemin d’accès du fichier journal n’est pas fourni, le fichier journal par défaut, (*systemroot*\Documents and settings\*UserAccount<em>\Mes Documents\Security\Logs\*DatabaseName</em>. log), est utilisé.

Dans Windows Server 2008, `Secedit /refreshpolicy` a été remplacé par `gpupdate`. Pour plus d’informations sur la façon d’actualiser les paramètres de sécurité, consultez [gpupdate](gpupdate.md).

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

Exportez la base de données de sécurité et les stratégies de sécurité de domaine dans un fichier INF, puis importez ce fichier dans une autre base de données afin de répliquer les paramètres de stratégie de sécurité sur un autre ordinateur.
```
Secedit /export /db C:\Security\FY11\SecDbContoso.sdb /mergedpolicy /cfg SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log /quiet
```
Importez ce fichier dans une autre base de données sur un autre ordinateur.
```
Secedit /import /db C:\Security\FY12\SecDbContoso.sdb /cfg SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY12.log /quiet
```

## <a name="additional-references"></a>Références supplémentaires

-   [Secedit:import](secedit-import.md)
-   [Secedit](secedit.md)
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)