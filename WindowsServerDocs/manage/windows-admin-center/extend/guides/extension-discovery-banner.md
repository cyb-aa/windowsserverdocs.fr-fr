---
title: L’activation de la bannière de la découverte d’extension
description: L’activation de la bannière de la découverte d’extension
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 03/08/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 389acba6d1fe6f65320bd780c9fde6469b387e0b
ms.sourcegitcommit: e558dda2774345e9ad17ff04b759f68c59d88139
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2019
ms.locfileid: "9262921"
---
# L’activation de la bannière de la découverte d’extension #

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Une nouvelle fonctionnalité disponible dans Windows Admin Center Preview 1903 est la bannière de la découverte d’extension. Cette fonctionnalité permet à une extension déclarer le fabricant de matériel de serveur et elle prend en charge les modèles, et lorsqu’un utilisateur se connecte à un serveur ou cluster pour laquelle une extension est disponible, une bannière de notification s’affiche pour installer facilement l’extension. Les développeurs d’extensions sera en mesure d’obtenir plus de visibilité pour leurs extensions et les utilisateurs seront en mesure de découvrir facilement plusieurs fonctionnalités de gestion pour leurs serveurs.

![Bannière de la découverte d’extension](../../media/extend-guides-extension-discovery-banner/extension-discovery-banner.png)

## Comment fonctionne la bannière de la découverte d’extension ##

Lorsque Windows Admin Center est lancée, elle se connectera les flux d’extension enregistrés et extraire les métadonnées pour les packages d’extension disponible. Ensuite, lorsqu’un utilisateur se connecte à un serveur ou cluster dans Windows Admin Center, nous lisons le fabricant de matériel de serveur et le modèle à afficher dans l’outil de vue d’ensemble. Si nous trouvons une extension qui déclare qu’il prend en charge fabricant et/ou le modèle du serveur actuel, nous affichons une bannière pour informer l’utilisateur. En cliquant sur le lien «Set» dirigera l’utilisateur pour le Gestionnaire d’extensions où ils peuvent installer l’extension.

## L’implémentation de la bannière de la découverte d’extension ##

Les métadonnées de «balises» dans le fichier .nuspec sont utilisée pour déclarer le fabricant de matériel et/ou modèles votre prend en charge de l’extension. Balises sont délimitées par des espaces, et vous pouvez ajouter soit un fabricant ou balise de modèle ou les deux pour déclarer la prise en charge fabricant et/ou des modèles. Le format de balise est ``"[value type]_[value condition]"`` où [type de valeur] est «Fabricant» ou «Modèle» (casse) et [condition de valeur] est une [expression régulière Javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions) définissant le fabricant ou la chaîne de modèle et le [type de valeur] et [value condition] sont séparés par un trait de soulignement. Cette chaîne est ensuite encodée à l’aide des URI d’encodage et ajouté à la chaîne de métadonnées .nuspec «balises».

### Exemple ###

Supposons que j’ai développé une extension qui prend en charge des serveurs à partir d’une société appelée Contoso Inc., avec le modèle de nom R3xx et R4xx.

1. La balise du fabricant serait ``"Manufacturer_/Contoso Inc./"``. La balise pour les modèles pourrait être ``"Model_/^R[34][0-9]{2}$/"``. En fonction de vos besoins définir la condition de correspondance, il y aura différentes façons de définir votre expression régulière. Vous pouvez également séparer les balises fabricant ou le modèle dans plusieurs balises, par exemple, la balise de modèle peut également être ``"Model_/R3../ Model_/R4../"``.
2. Vous pouvez tester l’expression régulière avec la Console d’outils de votre navigateur web. Dans Edge ou Chrome, d’appuyer sur F12 pour ouvrir la fenêtre d’outils et dans l’onglet de la Console, tapez l’entrée de positionnement et suivante:

```javascript
var regex = /^R[34][0-9]{2}$/
```

Ensuite, si vous tapez et exécutez la commande suivante, elle retourne «true».

```javascript
regex.test('R300')
```

Et si vous exécutez la commande suivante, elle retourne «false».

```javascript
regex.test('R500')
```

3. Une fois que vous avez vérifié l’expression régulière, vous pouvez l’encoder dans la Console outils également, à l’aide de la méthode Javascript suivante:

```javascript
encodeURI(/^R[34][0-9]{2}$/)
```

Le format final de la chaîne de balise à ajouter à votre fichier .nuspec serait:

```
<tags>Manufacturer_/Contoso%20Inc./ Model_/%5ER%5B34%5D%5B0-9%5D%7B2%7D$/</tags>
```

> [!Tip]
> Nous comprenons que le fabricant de matériel peut être un très large éventail de noms de modèle dont certaines peuvent être pris en charge pendant que certains ne sont pas. N’oubliez pas que cette fonctionnalité est conçue pour faciliter la **découverte** de votre extension, mais il ne doit pas être un inventaire de tous vos modèles parfaitement à jour. Vous pouvez définir votre expression régulière pour être une expression plus simple qui correspond à un sous-ensemble de vos modèles. Un utilisateur ne voyiez pas la bannière de découverte si leur première connexion à un modèle de serveur qui ne correspond pas à la condition, mais plus tôt ou tard ils se connectent à un autre serveur qui est et découvrir et installera l’extension. Vous pouvez également envisager de définition d’une expression régulière simple qui seulement correspond à votre nom du fabricant. Dans certains cas, votre extension peut en réalité ne pas prendre en charge un modèle spécifique, mais vous pouvez utiliser l' [outil dynamique Afficher la fonctionnalité](./dynamic-tool-display.md) pour définir un script PowerShell personnalisé pour vérifier la prise en charge de modèle et de ne visualiser que votre extension, le cas échéant, ou fournir limitée fonctionnalité dans votre extension pour les modèles qui ne prennent pas en charge toutes les fonctionnalités.