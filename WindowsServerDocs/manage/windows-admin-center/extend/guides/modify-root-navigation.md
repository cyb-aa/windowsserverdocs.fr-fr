---
title: Modifier le comportement de navigation racine
description: Développer une extension de solution SDK Windows Admin Center (projet Honolulu) - modifier le comportement de navigation racine
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 08/07/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 4a5cba228aa3a0afed99c0d853c3720a5b46f650
ms.sourcegitcommit: 546229d6b5fa7e16f725c6c35f4dcc272711b811
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/18/2018
ms.locfileid: "4905039"
---
# Modifier le comportement de navigation racine pour une extension de solution

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Dans ce guide, nous allez découvrir comment modifier le comportement de navigation racine pour votre solution de disposer de comportement de liste de connexion différente, mais aussi comment afficher ou masquer la liste des outils.

## Modification de comportement de navigation racine

Ouvrez manifest.json fichier dans {racine d’extension} \src et recherchez la propriété «rootNavigationBehavior». Cette propriété a deux valeurs valides: «connexion» ou «path». Le comportement de «connexion» est détaillé plus loin dans la documentation.

### Chemin d’accès de paramètre comme un rootNavigationBehavior

Définissez la valeur de ```rootNavigationBehavior``` à ```path```, puis supprimez la ```requirements``` propriété et laissez le ```path``` propriété comme une chaîne vide. Vous avez terminé la configuration minimale requise pour créer une extension de solution. Enregistrez le fichier, puis gulp build & gt; gulp servent vous un outil et côté puis chargerait l’extension dans votre extension Windows Admin Center locale.

Un tableau de points d’entrée du manifeste valide se présente comme suit:
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

Outils intégrés avec ce type de structure seront connexions non requises pour charger, mais ne possèdent des fonctionnalités de connectivité de nœud soit.

### Configuration de connexions comme un rootNavigationBehavior

Lorsque vous définissez le ```rootNavigationBehavior``` propriété ```connections```, vous indiquez l’interpréteur de commandes Windows Admin Center qu’il y aura un nœud connecté (toujours un serveur d’un type) auquel il doit se connecter, et vérifiez l’état de la connexion. Cela, il existe 2 étapes lors de la vérification de connexion. 1) Windows Admin Center tentera d’établir une tentative de connexion dans le nœud avec vos informations d’identification (pour l’établissement de la session PowerShell à distance), et 2) il s’exécutera le script PowerShell que vous fournissez afin d’identifier si le nœud est dans un état pouvant être connecté.

Une définition de solution valide avec des connexions se présente comme suit:

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

Lorsque le rootNavigationBehavior est défini sur «connexion» vous sont requis pour générer la définition de connexions dans le manifeste. Cela inclut la propriété «en-tête» (servira à afficher dans votre en-tête solution quand un utilisateur la sélectionne à partir du menu), un tableau de connectionTypes (Cela spécifiera les connectionTypes sont utilisés dans la solution. Plus d’informations dans la documentation connectionProvider.).

## Activation et désactivation du menu Outils ##

Une autre propriété disponible dans la définition de la solution est la propriété «outil». Vous pourrez ainsi déterminer si le menu Outils s’affiche, ainsi que de l’outil qui sera chargé. Lorsqu’elle est activée, Windows Admin Center rendra le menu Outils de gauche. DefaultTool, il est nécessaire que vous ajoutez un point d’entrée outil au manifeste afin de charger les ressources appropriées. La valeur de «defaultTool» doit être la propriété «name» de l’outil telle qu’elle est définie dans le manifeste.
