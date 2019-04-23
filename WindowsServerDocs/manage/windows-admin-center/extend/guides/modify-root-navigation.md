---
title: Modifier le comportement de navigation racine
description: Développer une extension de la solution Windows Admin Center SDK (projet Honolulu) - modifier le comportement de navigation de racine
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 08/07/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 4a5cba228aa3a0afed99c0d853c3720a5b46f650
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861730"
---
# <a name="modify-root-navigation-behavior-for-a-solution-extension"></a>Modifier le comportement de navigation de racine pour une extension de la solution

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

Dans ce guide, vous allez apprendre comment modifier le comportement de navigation racine pour votre solution avoir un comportement liste autre connexion, ainsi que comment masquer ou afficher la liste des outils.

## <a name="modifying-root-navigation-behavior"></a>Modification du comportement de navigation de racine

Ouvrez le fichier manifest.json dans {racine d’extension} \src et recherchez la propriété « rootNavigationBehavior ». Cette propriété a deux valeurs valides : « connexions » ou « path ». Le comportement de « connexions » est détaillé plus loin dans la documentation.

### <a name="setting-path-as-a-rootnavigationbehavior"></a>Chemin d’accès de paramètre comme un rootNavigationBehavior

Définissez la valeur de ```rootNavigationBehavior``` à ```path```, puis supprimez le ```requirements``` propriété et laissez le ```path``` propriété comme une chaîne vide. Vous avez terminé la configuration minimale requise pour générer une extension de la solution. Enregistrez le fichier et build gulp -> gulp servent vous seraient un outil et le côté puis charger l’extension de votre extension de Windows Admin Center locale.

Un tableau de points d’entrée manifeste valide se présente comme suit :
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

Outils intégrés avec ce type de structure ne seront pas nécessaire de connexions à charger, mais n’auront pas les fonctions de connectivité nœud soit.

### <a name="setting-connections-as-a-rootnavigationbehavior"></a>Définition des connexions en tant qu’un rootNavigationBehavior

Lorsque vous définissez la ```rootNavigationBehavior``` propriété ```connections```, vous indiquez à l’interpréteur de commandes Windows Admin Center qu’il y aura un nœud connecté (toujours un serveur d’un certain type) auquel il doit se connecter et vérifiez l’état de la connexion. Avec cela, il existe 2 étapes de vérification de la connexion. 1) Windows Admin Center tentera de rendre une tentative de connexion dans le nœud avec vos informations d’identification (pour l’établissement de la session PowerShell à distance), et (2) il exécutera le script PowerShell que vous fournissez pour déterminer si le nœud est dans un état connectable.

Une définition de solution valide avec des connexions ressemblera à ceci :

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

Lorsque le rootNavigationBehavior est définie sur « connexions », vous devez créer la définition de connexions dans le manifeste. Cela inclut la propriété « header » (doit être utilisée pour afficher dans l’en-tête de votre solution lorsqu’un utilisateur le sélectionne à partir du menu), un tableau de connectionTypes (elle permet d’indiquer les connectionTypes sont utilisés dans la solution. Plus d’informations dans la documentation du fournisseur de connexion.).

## <a name="enabling-and-disabling-the-tools-menu"></a>Activation et désactivation du menu Outils ##

Une autre propriété disponible dans la définition de la solution est la propriété « outils ». Cela détermine si le menu Outils s’affiche, ainsi que l’outil qui est chargée. Quand activé, Windows Admin Center restitue le menu Outils de gauche. Avec defaultTool, il est nécessaire que vous ajoutez un point d’entrée outil au manifeste afin de charger les ressources appropriées. La valeur de « defaultTool » doit être la propriété « name » de l’outil, car il est défini dans le manifeste.
