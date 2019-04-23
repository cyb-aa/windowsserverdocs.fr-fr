---
title: secedit:configure
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a92e68ca-003c-4219-8655-0e7734f5fab3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8285c22c3c64b4f056124d8a1bb02297c7aea3c8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853390"
---
# <a name="seceditconfigure"></a>secedit:configure



Vous permet de configurer les paramètres système actuels à l’aide des paramètres de sécurité stockées dans une base de données. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
Secedit /configure /db <database file name> [/cfg <configuration file name>] [/overwrite] [/areas SECURITYPOLICY | GROUP_MGMT | USER_RIGHTS | REGKEYS | FILESTORE | SERVICES] [/log <log file name>] [/quiet]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|db|Obligatoire.</br>Spécifie le chemin d’accès et le nom d’une base de données qui contient la configuration stockée.</br>Si le nom de fichier spécifie une base de données qui n’a pas un modèle de sécurité (comme représenté par le fichier de configuration) associé, le `/cfg \<configuration file name>` option de ligne de commande doit également être spécifiée.|
|cfg|Facultatif.</br>Spécifie le chemin d’accès et le nom du modèle de sécurité qui sera importé dans la base de données pour l’analyse.</br>Cette option /cfg est uniquement valide lorsqu’il est utilisé avec le `/db \<database file name>` paramètre. S’il n’est pas spécifié, l’analyse est exécutée sur n’importe quelle configuration déjà stockée dans la base de données.|
|overwrite|Facultatif.</br>Spécifie si le modèle de sécurité dans le paramètre /cfg doit remplacer n’importe quel modèle ou un modèle composite qui est stocké dans la base de données au lieu d’ajouter les résultats dans le modèle stocké.</br>Cette option de ligne de commande est valide uniquement lorsque le `/cfg \<configuration file name>` paramètre est également utilisé. S’il n’est pas spécifié, le modèle dans le paramètre /cfg est ajouté au modèle stocké.|
|Zones|Facultatif.</br>Spécifie les zones de sécurité à appliquer au système. Si ce paramètre n’est pas spécifié, tous les paramètres de sécurité définis dans la base de données sont appliquées au système. Pour configurer plusieurs zones, séparez chaque zone par un espace. Les zones de sécurité suivantes sont prises en charge :</br>-   SecurityPolicy</br>    Stratégie locale et stratégie de domaine pour le système, y compris les stratégies de compte, d’audit stratégies, options de sécurité et ainsi de suite.</br>-   Group_Mgmt</br>    Paramètres de groupe restreint pour les groupes spécifiés dans le modèle de sécurité.</br>-   User_Rights</br>    Droits d’ouverture de session utilisateur et octroi de privilèges.</br>-   RegKeys</br>    Sécurité des clés de Registre local.</br>-   FileStore</br>    Sécurité sur le stockage de fichiers local.</br>-Services</br>    Sécurité pour tous les services définis.|
|journal|Facultatif.</br>Spécifie le chemin d’accès et le nom du fichier journal pour le processus.|
|quiet|Facultatif.</br>Supprime la sortie écran et journal. Vous pouvez toujours afficher les résultats analyse à l’aide du composant logiciel enfichable Configuration de la sécurité et l’analyse à la Console MMC (Microsoft Management).|

## <a name="remarks"></a>Notes

Si le chemin d’accès du fichier journal n’est pas fourni, le fichier journal par défaut, (*systemroot*\Users \*UserAccount*\My Documents\Security\Logs\*DatabaseName*.log) est utilisé.

À partir de Windows Server 2008, `Secedit /refreshpolicy` a été remplacé par `gpupdate`. Pour plus d’informations sur l’actualisation des paramètres de sécurité, consultez [Gpupdate](gpupdate.md).

## <a name="BKMK_Examples"></a>Exemples

Effectuer l’analyse pour les paramètres de sécurité sur la base de données de sécurité, SecDbContoso.sdb, que vous avez créé à l’aide de la Configuration de la sécurité et le composant logiciel enfichable analyse. Diriger la sortie vers le fichier SecAnalysisContosoFY11 avec l’invite afin de vérifier la commande a été exécuté correctement.
```
Secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /log C:\Security\FY11\SecAnalysisContosoFY11.log
```
Supposons que l’analyse a révélé certaines insuffisances donc le modèle de sécurité, SecContoso.inf, a été modifié. Exécutez la commande à nouveau pour incorporer les modifications, en redirigeant la sortie vers le fichier existant SecAnalysisContosoFY11 sans afficher d’invite.
```
Secedit /configure /db C:\Security\FY11\SecDbContoso.sdb /cfg SecContoso.inf /overwrite /log C:\Security\FY11\SecAnalysisContosoFY11.xml /quiet
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Secedit](secedit.md)
-   [Secedit:analyze](secedit-analyze.md)
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)