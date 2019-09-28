---
title: Activation de la bannière de détection des extensions
description: Activation de la bannière de détection des extensions
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: fb549d84f565feeea348d2f50a9188218e7638d1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357078"
---
# <a name="enabling-the-extension-discovery-banner"></a>Activation de la bannière de détection des extensions

>S'applique à : Windows Admin Center, Windows Admin Center Preview

Une nouvelle fonctionnalité disponible dans la version préliminaire du centre d’administration Windows 1903 est la bannière de découverte des extensions. Cette fonctionnalité permet à une extension de déclarer le fabricant du matériel du serveur et les modèles qu’elle prend en charge, et lorsqu’un utilisateur se connecte à un serveur ou à un cluster pour lequel une extension est disponible, une bannière de notification s’affiche pour installer facilement l’extension. Les développeurs d’extensions pourront obtenir plus de visibilité sur leurs extensions et les utilisateurs pourront facilement découvrir de nouvelles fonctionnalités de gestion pour leurs serveurs.

![Bannière détection des extensions](../../media/extend-guides-extension-discovery-banner/extension-discovery-banner.png)

## <a name="how-the-extension-discovery-banner-works"></a>Fonctionnement de la bannière de découverte des extensions

Lorsque le centre d’administration Windows est lancé, il se connecte aux flux d’extension inscrits et récupère les métadonnées pour les packages d’extension disponibles. Ensuite, lorsqu’un utilisateur se connecte à un serveur ou à un cluster dans le centre d’administration Windows, nous allons lire le fabricant et le modèle de matériel de serveur à afficher dans l’outil vue d’ensemble. Si nous trouvons une extension qui déclare qu’elle prend en charge le fabricant et/ou le modèle du serveur actuel, nous affichons une bannière pour informer l’utilisateur. Cliquez sur le lien « configurer maintenant » pour que l’utilisateur puisse installer l’extension sur le gestionnaire d’extensions.

## <a name="how-to-implement-the-extension-discovery-banner"></a>Comment implémenter la bannière de détection des extensions

Les métadonnées « Tags » dans le fichier. NuSpec sont utilisées pour déclarer le fabricant de matériel et/ou les modèles pris en charge par votre extension. Les balises sont délimitées par des espaces et vous pouvez ajouter une balise de fabricant ou de modèle, ou les deux pour déclarer le fabricant et/ou les modèles pris en charge. Le format de balise est ``"[value type]_[value condition]"``, où [valeur type] est « fabricant » ou « modèle » (respect de la casse), et [condition de valeur] est une [expression régulière JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions) définissant la chaîne du fabricant ou du modèle, et [type de valeur] et [condition de valeur] sont séparés par un trait de soulignement. Cette chaîne est ensuite encodée à l’aide de l’encodage URI et ajoutée à la chaîne de métadonnées. NuSpec "tags".

### <a name="example"></a>Exemple

Supposons que j’ai développé une extension qui prend en charge les serveurs d’une société nommée Contoso Inc., avec le nom de modèle R3xx et R4xx.

1. La balise du fabricant serait ``"Manufacturer_/Contoso Inc./"``. La balise pour les modèles peut être ``"Model_/^R[34][0-9]{2}$/"``. Selon la manière dont vous souhaitez définir la condition de correspondance, il y aura différentes façons de définir votre expression régulière. Vous pouvez également séparer les balises du fabricant ou du modèle en plusieurs balises. par exemple, la balise Model peut également être ``"Model_/R3../ Model_/R4../"``.
2. Vous pouvez tester l’expression régulière avec la console DevTools de votre navigateur Web. Dans Edge ou chrome, appuyez sur F12 pour ouvrir la fenêtre DevTools, puis dans l’onglet Console, tapez la commande suivante et appuyez sur entrée :

   ```javascript
   var regex = /^R[34][0-9]{2}$/
   ```

   Ensuite, si vous tapez et exécutez la commande suivante, elle retourne la valeur « true ».

   ```javascript
   regex.test('R300')
   ```

   Et si vous exécutez la commande suivante, elle retourne la valeur « false ».

   ```javascript
   regex.test('R500')
   ```

3. Une fois que vous avez vérifié l’expression régulière, vous pouvez également l’encoder dans la console DevTools, à l’aide de la méthode JavaScript suivante :

   ```javascript
   encodeURI(/^R[34][0-9]{2}$/)
   ```

   Le format final de la chaîne de balise à ajouter à votre fichier. NuSpec serait :

   ```
   <tags>Manufacturer_/Contoso%20Inc./ Model_/%5ER%5B34%5D%5B0-9%5D%7B2%7D$/</tags>
   ```

> [!Tip]
> Nous savons qu’un fabricant de matériel peut avoir un très large éventail de noms de modèles dont certains peuvent être pris en charge alors que d’autres ne le sont pas. N’oubliez pas que cette fonctionnalité est destinée à faciliter la **découverte** de votre extension, mais qu’elle ne doit pas être un inventaire parfaitement à jour de tous vos modèles. Vous pouvez définir votre expression régulière de sorte qu’elle soit une expression plus simple qui corresponde à un sous-ensemble de vos modèles. Il se peut qu’un utilisateur ne puisse pas voir la bannière de découverte s’il se connecte pour la première fois à un modèle de serveur qui ne correspond pas à la condition, mais tôt ou tard, il se connecte à un autre serveur qui procède à la détection et à l’installation de l’extension. Vous pouvez également envisager de définir une expression régulière simple qui correspond uniquement au nom de votre fabricant. Dans certains cas, votre extension peut ne pas prendre en charge un modèle spécifique, mais vous pouvez utiliser la fonctionnalité d’affichage de l' [outil dynamique](./dynamic-tool-display.md) pour définir un script PowerShell personnalisé pour vérifier la prise en charge du modèle et afficher uniquement votre extension le cas échéant, ou fournir une limite fonctionnalités de votre extension pour les modèles qui ne prennent pas en charge toutes les fonctionnalités.