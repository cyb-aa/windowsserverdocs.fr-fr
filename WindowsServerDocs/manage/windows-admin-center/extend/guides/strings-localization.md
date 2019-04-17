---
title: Chaînes et localisation dans Windows Admin Center
description: En savoir plus sur l’obtention prête pour la localisation de vos chaînes dans le SDK Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f0671160bd790d80e35f6d6c1586a07969939070
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296731"
---
# Chaînes et localisation dans Windows Admin Center #

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Passons plus détaillées dans le SDK Windows Admin Center Extensions et parler de chaînes et de localisation.

Pour permettre la localisation de toutes les chaînes qui sont restitués sur la couche de présentation, tirer parti du fichier strings.resjson sous /src/resources/strings - il est déjà configuré. Lorsque vous avez besoin ajouter une nouvelle chaîne à votre extension, l’ajouter à ce fichier resjson comme une nouvelle entrée. La structure existante suit ce format:

``` ts
"<YourExtensionName>_<Component>_<Accessor>": "Your string value goes here.",
```

Vous pouvez utiliser n’importe quel format qui que vous convient pour les chaînes, mais n’oubliez pas que le processus de génération (le processus qui prend le resjson et génère la classe TypeScript utilisable) convertit le trait de soulignement (_) à des points (.).

Par exemple, cette entrée:
``` ts
"HelloWorld_cim_title": "CIM Component",
```
Génère la structure d’accesseur suivante:
``` ts
MsftSme.resourcesStrings<Strings>().HelloWorld.cim.title;
```

## Ajouter d’autres langues pour la localisation ## 

Pour la localisation pour d’autres langages, un fichier strings.resjson doit être créé pour chaque langue. Ces fichiers doivent être des endroits dans ```\loc\output\{!ExtensionName}\{!LanguageFolder}\strings.resjson```. Les langues disponibles avec les dossiers correspondants sont:

| Language      | Folder      |
| ------------- |-------------|
| Čeština | cs-CZ |
| Allemand | de-DE |
| Anglais | en-US |
| Español | es-ES |
| Français | fr-FR | 
| Magyar | hu-HU | 
| Italiano | it-IT |
| 日本語 | ja-JP | 
| 한국어 | ko-KR | 
| Néerlandais | nl-NL |
| Polski | pl-PL |
| Portuguêse (Brasil) | pt-BR |
| Portuguêse (Portugal) | pt-PT |
| РУССКИЙ | ru-RU |
| Svenska | sv-SE |
| Türkçe    | tr-TR |
| 中文(简体) | zh-CN |
| 中文(繁體) | zh-TW |
> [!NOTE]
> Si vos besoins de structure de fichier sont différents à l’intérieur de l’emplacement/sortie, vous devez ajuster le localeOffset pour la tâche gulp 'générer resjson-json-localisée' qui se trouve dans le gulpfile.js. Ce décalage est jusqu'à quel niveau dans le dossier d’emplacement, elle doit démarrer recherche de fichiers strings.resjson.

Chaque fichier strings.resjson sera formaté de la même manière, comme indiquée précédemment dans la partie supérieure de ce guide. 

Par exemple, pour inclure une localisation pour Español inclure cette entrée dans ```\loc\output\HelloWorld\es-ES\strings.resjson```: 
```json
"HelloWorld_cim_title": "CIM Componente",
```
À tout moment que vous avez ajouté les chaînes localisées, gulp générer doit être exécuté à nouveau afin de les faire apparaître. Exécutez la commande suivante:
``` cmd
gulp generate 
```

Pour vérifier que vos chaînes ont été générés atteindre ```\src\app\assets\strings\{!LanguageFolder}\strings.resjson```. Votre entrée qui vient d’être ajoutée s’affichent dans ce fichier.
Maintenant si vous passez l’option de langue dans Windows Admin Center, vous serez en mesure de voir les chaînes localisées dans votre extension. 
