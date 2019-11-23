---
title: secedit
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 58ed57ed-08e3-403d-a363-0620b358637a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5598f830ad4cef8d45c99594da12cbcdd84e7eef
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371112"
---
# <a name="secedit"></a>secedit



Configure et analyse la sécurité du système en comparant votre configuration actuelle aux modèles de sécurité spécifiés.

## <a name="syntax"></a>Syntaxe

```
secedit 
[/analyze /db <database file name> /cfg <configuration file name> [/overwrite] /log <log file name> [/quiet]]
[/configure /db <database file name> [/cfg <configuration filename>] [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]]
[/export /db <database file name> [/mergedpolicy] /cfg <configuration file name> [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>]]
[/generaterollback /db <database file name> /cfg <configuration file name> /rbk <rollback file name> [/log <log file name>] [/quiet]]
[/import /db <database file name> /cfg <configuration file name> [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]]
[/validate <configuration file name>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|[Secedit:analyze](secedit-analyze.md)|Vous permet d’analyser les paramètres système actuels par rapport aux paramètres de base stockés dans une base de données.  Les résultats de l’analyse sont stockés dans une zone distincte de la base de données et peuvent être affichés dans le composant logiciel enfichable Configuration et analyse de la sécurité.|
|[Secedit:configure](secedit-configure.md)|Vous permet de configurer un système avec les paramètres de sécurité stockés dans une base de données.|
|[Secedit:export](secedit-export.md)|Vous permet d’exporter les paramètres de sécurité stockés dans une base de données.|
|[Secedit:generaterollback](secedit-generaterollback.md)|Vous permet de générer un modèle de restauration par rapport à un modèle de configuration.|
|[Secedit:import](secedit-import.md)|Vous permet d’importer un modèle de sécurité dans une base de données afin que les paramètres spécifiés dans le modèle puissent être appliqués à un système ou analysés par rapport à un système.|
|[Secedit:validate](secedit-validate.md)|Vous permet de valider la syntaxe d’un modèle de sécurité.|

## <a name="remarks"></a>Notes

Pour tous les noms de fichiers, le répertoire actif est utilisé si aucun chemin d’accès n’est spécifié.

Quand un modèle de sécurité est créé à l’aide du composant logiciel enfichable modèle de sécurité et que le composant logiciel enfichable Configuration et analyse de la sécurité est exécuté, les fichiers suivants sont créés :


|           Fichier           |                                                                                                                                                                                                                                                               Description                                                                                                                                                                                                                                                                |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        Scesrv. log        |                                                                                                                             **Emplacement**:%windir%\Security\Logs</br>**Créé par**: système d’exploitation</br>**Type de fichier**: texte</br>**Taux d’actualisation**: remplacé lorsque secedit/analyze,/configure,/Export ou/Import sont exécutés.</br>**Contenu**: contient les résultats de l’analyse regroupés par type de stratégie.                                                                                                                             |
| *Nom sélectionné par l’utilisateur*. sdb |                                                                                    **Emplacement**:% windir%\*compte d’utilisateur<em>\Documents\Security\Database</br></em>*créé par*<em>: exécution du composant logiciel enfichable Configuration et analyse de la sécurité</br></em>*type de fichier*<em>: propriétaire</br>*fréquence d’actualisation* des </em><em>: mise à jour chaque fois qu’un nouveau modèle de sécurité est créé.</br></em>\*de *contenu* : les stratégies de sécurité locales et les modèles de sécurité créés par l’utilisateur.                                                                                    |
| Nom du fichier. log *sélectionné par l’utilisateur* | **Emplacement**: défini par l’utilisateur, mais par défaut,% windir%\*compte d’utilisateur<em>\Documents\Security\Logs</br></em>*créé par*<em>: exécution des sous-commandes/Analyze et/configure (ou utilisation du composant logiciel enfichable Configuration et analyse de la sécurité)</br></em><em>de *type de fichier* : texte</br>*fréquence d’actualisation* des </em><em>: exécution des sous-commandes/Analyze et/configure (ou utilisation du composant logiciel enfichable Configuration et analyse de la sécurité); remplacé.</br></em>\*de *contenu* :</br>1. nom du fichier journal</br>2. date et heure</br>3. résultats de l’analyse ou de l’investigation. |
| Nom du fichier. inf *sélectionné par l’utilisateur* |                                                                                     **Emplacement**:% windir%\*compte d’utilisateur<em>\Documents\Security\Templates</br></em>*créé par*<em>: exécution du composant logiciel enfichable modèle de sécurité</br></em><em>de *type de fichier* : texte</br>*fréquence d’actualisation* </em><em>: chaque fois que le modèle de sécurité est mis à jour</br></em>\*de *contenu* : contient les informations de configuration pour le modèle pour chaque stratégie sélectionnée à l’aide du composant logiciel enfichable.                                                                                     |

> [!NOTE]
> La console MMC (Microsoft Management Console) et le composant logiciel enfichable Configuration et analyse de la sécurité ne sont pas disponibles sur Server Core.

#### <a name="additional-references"></a>Références supplémentaires

Pour obtenir des exemples d’utilisation de cette commande, consultez la section exemples dans l’un des fichiers de sous-commande.
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)