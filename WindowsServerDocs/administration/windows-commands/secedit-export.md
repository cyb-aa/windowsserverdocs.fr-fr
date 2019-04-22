---
title: Secedit:export
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 49a8b241-aa8c-45b7-844d-67a29fab708e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f9d6268777d0791dbc0cdca2d4318399378698b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813490"
---
# <a name="seceditexport"></a>Secedit:export



Exporte les paramètres de sécurité stockées dans une base de données configurée avec des modèles de sécurité. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
Secedit /export /db <database file name> [/mergedpolicy] /cfg <configuration file name> [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]

```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|db|Obligatoire.</br>Spécifie le chemin d’accès et le nom d’une base de données qui contient la configuration stockée par rapport à laquelle l’analyse sera effectuée.</br>Si le nom de fichier spécifie une base de données qui n’a pas un modèle de sécurité (comme représenté par le fichier de configuration) associé, le `/cfg \<configuration file name>` option de ligne de commande doit également être spécifiée.|
|mergedpolicy|Facultatif.</br>Fusionne et exporte les paramètres de sécurité de stratégie locale et de domaine.|
|cfg|Obligatoire.</br>Spécifie le chemin d’accès et le nom du modèle de sécurité qui sera importé dans la base de données pour l’analyse.</br>Cette option /cfg est uniquement valide lorsqu’il est utilisé avec le `/db \<database file name>` paramètre. S’il n’est pas spécifié, l’analyse est exécutée sur n’importe quelle configuration déjà stockée dans la base de données.|
|Zones|Facultatif.</br>Spécifie les zones de sécurité à appliquer au système. Si ce paramètre n’est pas spécifié, tous les paramètres de sécurité définis dans la base de données sont appliquées au système. Pour configurer plusieurs zones, séparez chaque zone par un espace. Les zones de sécurité suivantes sont prises en charge :</br>-   SecurityPolicy</br>    Stratégie locale et stratégie de domaine pour le système, y compris les stratégies de compte, d’audit stratégies, options de sécurité et ainsi de suite.</br>-   Group_Mgmt</br>    Paramètres de groupe restreint pour les groupes spécifiés dans le modèle de sécurité.</br>-   User_Rights</br>    Droits d’ouverture de session utilisateur et octroi de privilèges.</br>-   RegKeys</br>    Sécurité des clés de Registre local.</br>-   FileStore</br>    Sécurité sur le stockage de fichiers local.</br>-Services</br>    Sécurité pour tous les services définis.|
|journal|Facultatif.</br>Spécifie le chemin d’accès et le nom du fichier journal pour le processus.|
|quiet|Facultatif.</br>Supprime la sortie écran et journal. Vous pouvez toujours afficher les résultats analyse à l’aide du composant logiciel enfichable Configuration de la sécurité et l’analyse à la Console MMC (Microsoft Management).|

## <a name="remarks"></a>Notes

Vous pouvez utiliser cette commande pour sauvegarder vos stratégies de sécurité sur un ordinateur local en plus de l’importation des paramètres vers un autre ordinateur.

Si le chemin d’accès du fichier journal n’est pas fourni, le fichier journal par défaut, (*systemroot*\Documents and Settings\*UserAccount*\My Documents\Security\Logs\*DatabaseName*. ouvrir une session) est utilisée.

Dans Windows Server 2008, `Secedit /refreshpolicy` a été remplacé par `gpupdate`. Pour plus d’informations sur l’actualisation des paramètres de sécurité, consultez [Gpupdate](gpupdate.md).

## <a name="BKMK_Examples"></a>Exemples

Exporter la base de données de sécurité et les stratégies de sécurité de domaine vers un fichier inf, puis importer ce fichier à une autre base de données afin de répliquer les paramètres de stratégie de sécurité sur un autre ordinateur.
```
Secedit /export /db C:\Security\FY11\SecDbContoso.sdb /mergedpolicy /cfg SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log /quiet
```
Importez ce fichier dans une autre base de données sur un autre ordinateur.
```
Secedit /import /db C:\Security\FY12\SecDbContoso.sdb /cfg SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY12.log /quiet
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Secedit:import](secedit-import.md)
-   [Secedit](secedit.md)
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)