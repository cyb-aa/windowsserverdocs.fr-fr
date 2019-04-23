---
title: Chaînes et localisation dans Windows Admin Center
description: En savoir plus sur la préparation de vos chaînes pour la localisation dans le Kit de développement Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: fb328f86c98141a5a2a1c4fd05ec1d4c96a7bc5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845400"
---
# <a name="strings-and-localization-in-windows-admin-center"></a>Chaînes et localisation dans Windows Admin Center #

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

Allons plus approfondie dans le Kit de développement logiciel Windows Admin Center Extensions et parler de chaînes et de localisation.

Pour activer la localisation de toutes les chaînes qui sont rendus sur la couche de présentation, tirez parti du fichier strings.resjson sous /src/resources/strings - il est déjà configuré. Lorsque vous avez besoin d’ajouter une nouvelle chaîne à votre extension, ajoutez-le à ce fichier resjson comme une nouvelle entrée. La structure existante suit ce format :

``` ts
"<YourExtensionName>_<Component>_<Accessor>": "Your string value goes here.",
```

Vous pouvez utiliser n’importe quel format que pour les chaînes, mais n’oubliez pas que le processus de génération (le processus qui prend le resjson et génère la classe TypeScript utilisable) convertit un trait de soulignement (_) en points (.).

Par exemple, cette entrée :
``` ts
"HelloWorld_cim_title": "CIM Component",
```
Génère la structure d’accesseur suivante :
``` ts
MsftSme.resourcesStrings<Strings>().HelloWorld.cim.title;
```
