---
title: Chaînes et localisation dans le centre d’administration Windows
description: En savoir plus sur la préparation de vos chaînes pour la localisation dans le kit de développement logiciel (SDK) du centre d’administration Windows (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 61289ae175ca8b906386cff9e36f5023ea28d051
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385241"
---
# <a name="strings-and-localization-in-windows-admin-center"></a>Chaînes et localisation dans le centre d’administration Windows #

>S'applique à : Windows Admin Center, Windows Admin Center Preview

Voyons plus en détail le kit de développement logiciel (SDK) extensions du centre d’administration Windows et parle des chaînes et de la localisation.

Pour activer la localisation de toutes les chaînes qui sont rendues sur la couche de présentation, tirez parti du fichier Strings. resjson sous/SRC/Resources/Strings-il est déjà configuré. Lorsque vous devez ajouter une nouvelle chaîne à votre extension, ajoutez-la à ce fichier resjson en tant que nouvelle entrée. La structure existante suit ce format :

``` ts
"<YourExtensionName>_<Component>_<Accessor>": "Your string value goes here.",
```

Vous pouvez utiliser n’importe quel format pour les chaînes, mais sachez que le processus de génération (le processus qui prend le resjson et renvoie la classe de type _ machine utilisable) convertit le trait de soulignement (_) en points (.).

Par exemple, l’entrée suivante :
``` ts
"HelloWorld_cim_title": "CIM Component",
```
Génère la structure d’accesseur suivante :
``` ts
MsftSme.resourcesStrings<Strings>().HelloWorld.cim.title;
```

## <a name="add-other-languages-for-localization"></a>Ajouter d’autres langues pour la localisation ## 

Pour la localisation dans d’autres langues, vous devez créer un fichier Strings. resjson pour chaque langue. Ces fichiers doivent être placés dans ```\loc\output\{!ExtensionName}\{!LanguageFolder}\strings.resjson```. Les langues disponibles avec les dossiers correspondants sont les suivantes :

| Langue      | Dossier      |
| ------------- |-------------|
| Čeština | cs-CZ |
| Deutsch | de-DE |
| Anglais | fr-FR |
| Español | es-ES |
| Français | fr-FR | 
| Magyar | hu-HU | 
| Italiano | it-IT |
| 日本語 | ja-JP | 
| 한국어 | ko-KR | 
| Nederlands | nl-NL |
| Polski | pl-PL |
| Português (Brasil) | pt-BR |
| Português (Portugal) | pt-PT |
| Русский | ru-RU |
| Svenska | sv-SE |
| Türkçe    | tr-TR |
| 中文(简体) | zh-CN |
| 中文(繁體) | zh-TW |
> [!NOTE]
> Si les besoins de votre structure de fichiers sont différents dans la région/la sortie, vous devez ajuster le localeOffset pour la tâche Gulp « Generate-resjson-JSON-Localized » qui se trouve dans le gulpfile. js. Ce décalage correspond à la profondeur du dossier Loc où il doit commencer à rechercher les fichiers Strings. resjson.

Chaque fichier Strings. resjson sera mis en forme de la même façon que précédemment mentionné en haut de ce guide. 

Par exemple, pour inclure une localisation pour Español, incluez cette ```\loc\output\HelloWorld\es-ES\strings.resjson```entrée dans : 
```json
"HelloWorld_cim_title": "CIM Componente",
```
À chaque fois que vous avez ajouté des chaînes localisées, la génération de Gulp doit être exécutée à nouveau pour qu’elles apparaissent. Exécutez :
``` cmd
gulp generate 
```

Pour confirmer que vos chaînes ont été générées, ```\src\app\assets\strings\{!LanguageFolder}\strings.resjson```accédez à. L’entrée que vous venez d’ajouter s’affichera dans ce fichier.
Maintenant, si vous basculez l’option langue dans le centre d’administration Windows, vous pourrez voir les chaînes localisées dans votre extension. 
