---
title: Modifier le comportement de navigation racine
description: Développer une extension de solution Kit de développement logiciel (SDK) du centre d’administration Windows (projet Honolulu)-modifier le comportement de navigation racine
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 08/07/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 78c94f3ea13f54ac31f9de9dd60873b93eba2c17
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385278"
---
# <a name="modify-root-navigation-behavior-for-a-solution-extension"></a>Modifier le comportement de navigation racine pour une extension de solution

>S'applique à : Windows Admin Center, Windows Admin Center Preview

Dans ce guide, nous allons apprendre à modifier le comportement de navigation racine de votre solution pour avoir un comportement de liste de connexions différent, ainsi que pour masquer ou afficher la liste d’outils.

## <a name="modifying-root-navigation-behavior"></a>Modification du comportement de navigation racine

Ouvrez le fichier manifest. JSON dans {extension root} \SRC et recherchez la propriété « rootNavigationBehavior ». Cette propriété a deux valeurs valides : « Connections » ou « Path ». Le comportement des « connexions » est détaillé plus loin dans la documentation.

### <a name="setting-path-as-a-rootnavigationbehavior"></a>Définition du chemin d’accès en tant que rootNavigationBehavior

Définissez la valeur de ```rootNavigationBehavior``` sur ```path```, puis supprimez la propriété ```requirements``` et laissez la propriété ```path``` sous forme de chaîne vide. Vous avez terminé la configuration minimale requise pour générer une extension de solution. Enregistrez le fichier et Gulp Build-> Gulp servir comme vous le feriez pour un outil, puis chargez l’extension dans votre extension du centre d’administration Windows local.

Un tableau entryPoints de manifestes valide se présente comme suit :
```
    "entryPoints": [
        {
          "entryPointType": "solution",
          "name": "main",
          "urlName": "testsln",
          "displayName": "resources:strings:displayName",
          "description": "resources:strings:description",
          "icon": "sme-icon:icon-win-powerShell",
          "path": "",
          "rootNavigationBehavior": "path"
        }
    ],
```

Les outils générés avec ce type de structure n’ont pas besoin de connexions à charger, mais elles ne disposent pas non plus de la fonctionnalité de connectivité de nœud.

### <a name="setting-connections-as-a-rootnavigationbehavior"></a>Définition des connexions en tant que rootNavigationBehavior

Lorsque vous affectez à la propriété ```rootNavigationBehavior``` la valeur ```connections```, vous indiquez à l’interpréteur de commandes du centre d’administration Windows qu’il y aura un nœud connecté (toujours un serveur d’un certain type) auquel il doit se connecter, et vérifier l’état de la connexion. Dans ce cas, il existe 2 étapes dans la vérification de la connexion. 1) le centre d’administration Windows tentera d’effectuer une tentative de connexion au nœud avec vos informations d’identification (pour l’établissement de la session PowerShell distante) et 2) il exécutera le script PowerShell que vous fournissez pour identifier si le nœud est dans un État connectable.

Une définition de solution valide avec des connexions se présente comme suit :

``` json
        {
          "entryPointType": "solution",
          "name": "example",
          "urlName": "solutionexample",
          "displayName": "resources:strings:displayName",
          "description": "resources:strings:description",
          "icon": "sme-icon:icon-win-powerShell",
          "rootNavigationBehavior": "connections",
          "connections": {
            "header": "resources:strings:connectionsListHeader",
            "connectionTypes": [
                "msft.sme.connection-type.example"
                ]
            },
            "tools": {
                "enabled": false,
                "defaultTool": "solution"
            }
        },
```

Lorsque rootNavigationBehavior est défini sur « Connections », vous devez générer la définition des connexions dans le manifeste. Cela comprend la propriété « Header » (qui sera utilisée pour afficher dans l’en-tête de votre solution lorsqu’un utilisateur le sélectionne dans le menu), un tableau connectionTypes (spécifie les connectionTypes qui sont utilisés dans la solution. En savoir plus sur cela dans la documentation d’connectionProvider.).

## <a name="enabling-and-disabling-the-tools-menu"></a>Activation et désactivation du menu outils ##

La propriété « Tools » est une autre propriété disponible dans la définition de la solution. Cela permet de déterminer si le menu outils est affiché, ainsi que l’outil qui sera chargé. Lorsque cette option est activée, le centre d’administration Windows affiche le menu outils de gauche. Avec defaultTool, il est nécessaire d’ajouter un point d’entrée d’outil au manifeste afin de charger les ressources appropriées. La valeur de « defaultTool » doit être la propriété « Name » de l’outil, car il est défini dans le manifeste.
