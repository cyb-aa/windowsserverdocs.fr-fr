---
title: Secedit:import
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1dd59d4c-9d48-444a-871b-b957eb682597
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 305b915a0d7e8ab152b072ff131854f56b9b0386
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441537"
---
# <a name="seceditimport"></a>Secedit:import



Importe les paramètres de sécurité stockées dans un fichier inf précédemment exporté à partir de la base de données configurée avec des modèles de sécurité. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
Secedit /import /db <database file name> /cfg <configuration file name> [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|db|Obligatoire.</br>Spécifie le chemin d’accès et le nom d’une base de données qui contient la configuration stockée dans laquelle l’importation sera effectuée.</br>Si le nom de fichier spécifie une base de données qui n’a pas un modèle de sécurité (comme représenté par le fichier de configuration) associé, le `/cfg \<configuration file name>` option de ligne de commande doit également être spécifiée.|
|overwrite|Facultatif.</br>Spécifie si le modèle de sécurité dans le paramètre /cfg doit remplacer n’importe quel modèle ou un modèle composite qui est stocké dans la base de données au lieu d’ajouter les résultats dans le modèle stocké.</br>Cette option de ligne de commande est valide uniquement lorsque le `/cfg \<configuration file name>` paramètre est également utilisé. S’il n’est pas spécifié, le modèle dans le paramètre /cfg est ajouté au modèle stocké.|
|cfg|Obligatoire.</br>Spécifie le chemin d’accès et le nom du modèle de sécurité qui sera importé dans la base de données pour l’analyse.</br>Cette option /cfg est uniquement valide lorsqu’il est utilisé avec le `/db \<database file name>` paramètre. S’il n’est pas spécifié, l’analyse est exécutée sur n’importe quelle configuration déjà stockée dans la base de données.|
|overwrite|Facultatif.</br>Spécifie si le modèle de sécurité dans le paramètre /cfg doit remplacer n’importe quel modèle ou un modèle composite qui est stocké dans la base de données au lieu d’ajouter les résultats dans le modèle stocké.</br>Cette option de ligne de commande est valide uniquement lorsque le `/cfg \<configuration file name>` paramètre est également utilisé. S’il n’est pas spécifié, le modèle dans le paramètre /cfg est ajouté au modèle stocké.|
|Zones|Facultatif.</br>Spécifie les zones de sécurité à appliquer au système. Si ce paramètre n’est pas spécifié, tous les paramètres de sécurité définis dans la base de données sont appliquées au système. Pour configurer plusieurs zones, séparez chaque zone par un espace. Les zones de sécurité suivantes sont prises en charge :</br>-   SecurityPolicy</br>    Stratégie locale et stratégie de domaine pour le système, y compris les stratégies de compte, d’audit stratégies, options de sécurité et ainsi de suite.</br>-   Group_Mgmt</br>    Paramètres de groupe restreint pour les groupes spécifiés dans le modèle de sécurité.</br>-   User_Rights</br>    Droits d’ouverture de session utilisateur et octroi de privilèges.</br>-   RegKeys</br>    Sécurité des clés de Registre local.</br>-   FileStore</br>    Sécurité sur le stockage de fichiers local.</br>-Services</br>    Sécurité pour tous les services définis.|
|journal|Facultatif.</br>Spécifie le chemin d’accès et le nom du fichier journal pour le processus.|
|quiet|Facultatif.</br>Supprime la sortie écran et journal. Vous pouvez toujours afficher les résultats analyse à l’aide du composant logiciel enfichable Configuration de la sécurité et l’analyse à la Console MMC (Microsoft Management).|

## <a name="remarks"></a>Notes

Avant d’importer un fichier .inf vers un autre ordinateur, exécutez le /generaterollback secedit commande sur la base de données laquelle l’importation sera effectuée et secedit / valider sur le fichier d’importation pour vérifier son intégrité.

Si le chemin d’accès du fichier journal n’est pas fourni, le fichier journal par défaut, (*systemroot*\Documents and Settings\*UserAccount<em>\My Documents\Security\Logs\*DatabaseName</em>. ouvrir une session) est utilisée.

Dans Windows Server 2008, `Secedit /refreshpolicy` a été remplacé par `gpupdate`. Pour plus d’informations sur l’actualisation des paramètres de sécurité, consultez [Gpupdate](gpupdate.md).

## <a name="BKMK_Examples"></a>Exemples

Exporter la base de données de sécurité et les stratégies de sécurité de domaine vers un fichier inf, puis importer ce fichier à une autre base de données afin de répliquer les paramètres de stratégie de sécurité sur un autre ordinateur.
```
Secedit /export /db C:\Security\FY11\SecDbContoso.sdb /mergedpolicy /cfg NetworkShare\Policies\SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log /quiet
```
Importez simplement la partie de stratégies de sécurité du fichier dans une autre base de données sur un autre ordinateur.
```
Secedit /import /db C:\Security\FY12\SecDbContoso.sdb /cfg NetworkShare\Policies\SecContoso.inf /areas securitypolicy /log C:\Security\FY11\SecAnalysisContosoFY12.log /quiet
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Secedit:export](secedit-export.md)
-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Secedit:validate](secedit-validate.md)
-   [Secedit](secedit.md)
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)