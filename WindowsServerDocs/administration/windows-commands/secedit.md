---
title: secedit
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8aaa40f21c6f14dc7d686261e9980594c14a8032
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818460"
---
# <a name="secedit"></a>secedit



Configure et analyse la sécurité du système en comparant votre configuration actuelle aux modèles de sécurité spécifié.

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
|[Secedit:analyze](secedit-analyze.md)|Vous permet d’analyser les paramètres de systèmes en cours par rapport aux paramètres de ligne de base qui sont stockés dans une base de données.  Les résultats d’analyse sont stockés dans une zone séparée de la base de données et peuvent être affichés dans la Configuration de la sécurité et le composant logiciel enfichable analyse.|
|[Secedit : configurer](secedit-configure.md)|Vous permet de configurer un système avec les paramètres de sécurité stockés dans une base de données.|
|[Secedit:export](secedit-export.md)|Vous permet d’exporter les paramètres de sécurité stockées dans une base de données.|
|[Secedit:generaterollback](secedit-generaterollback.md)|Vous permet de générer un modèle de restauration par rapport à un modèle de configuration.|
|[Secedit:import](secedit-import.md)|Permet d’importer un modèle de sécurité dans une base de données afin que les paramètres spécifiés dans le modèle peuvent être appliqués à un système ou analysées par rapport à un système.|
|[Secedit:validate](secedit-validate.md)|Vous permet de valider la syntaxe d’un modèle de sécurité.|

## <a name="remarks"></a>Notes

Tous les noms de fichiers, le répertoire actif est utilisé si aucun chemin n’est spécifié.

Lorsqu’un modèle de sécurité est créé à l’aide du composant logiciel enfichable modèle de sécurité et la Configuration de sécurité et le composant logiciel enfichable analyse est exécutée, les fichiers suivants sont créés :

|Fichier|Description|
|----|-----------|
|Scesrv.log|**Emplacement**: %windir%\security\logs</br>**Créé par**: système d’exploitation</br>**Type de fichier**: texte</br>**Fréquence d’actualisation**: Remplacé lorsque secedit / analyser, / configurer, / exporter ou import sont exécutées.</br>**Contenu** : Contient les résultats de l’analyse, regroupées par type de stratégie.|
|*Nom d’utilisateur sélectionné*.sdb|**Emplacement**: %windir%\*compte d’utilisateur * \Documents\Security\Database</br>**Créé par**: exécute le composant logiciel enfichable Configuration de la sécurité et l’analyse</br>**Type de fichier**: propriétaires</br>**Fréquence d’actualisation**: Mise à jour chaque fois qu’un nouveau modèle de sécurité est créé.</br>**Contenu** : Stratégies de sécurité locales et les modèles de sécurité créés par l’utilisateur.|
|*Nom d’utilisateur sélectionné*.log|**Emplacement** : Défini par l’utilisateur mais la valeur par défaut est % windir %\*compte d’utilisateur * \Documents\Security\Logs</br>**Créé par** : Exécute le / analyze et / configurer sous-commandes (ou à l’aide du composant logiciel enfichable Configuration de la sécurité et l’analyse)</br>**Type de fichier**: texte</br>**Fréquence d’actualisation**: Exécute le / analyze et / configurer sous-commandes (ou à l’aide du composant logiciel enfichable Configuration de la sécurité et l’analyse) ; remplacé.</br>**Contenu** :</br>1.  Nom du fichier journal</br>2.  Date et heure</br>3.  Résultats d’une analyse ou une investigation.|
|*Nom d’utilisateur sélectionné*.inf|**Emplacement**: %windir%\*compte d’utilisateur * \Documents\Security\Templates</br>**Créé par**: en cours d’exécution du composant logiciel enfichable modèle de sécurité</br>**Type de fichier**: texte</br>**Fréquence d’actualisation**: chaque fois que le modèle de sécurité est mis à jour</br>**Contenu** : Contient l’ensemble des informations sur le modèle pour chaque stratégie sélectionnée à l’aide du composant logiciel enfichable.|

> [!NOTE]
> La Console MMC (Microsoft Management) et la Configuration de la sécurité et le composant logiciel enfichable analyse ne sont pas disponibles sur Server Core.

#### <a name="additional-references"></a>Références supplémentaires

Pour obtenir des exemples d’utilisation de cette commande, consultez la section exemples dans les fichiers sous-commande.
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)