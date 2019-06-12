---
title: L’activation de la bannière de découverte d’extension
description: L’activation de la bannière de découverte d’extension
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: fafc16d6acae3c5a7a58a379d2f88998b8e98c3d
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811879"
---
# <a name="enabling-the-extension-discovery-banner"></a>L’activation de la bannière de découverte d’extension

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

Une nouvelle fonctionnalité disponible dans Windows Admin Center aperçu 1903 est la bannière de découverte d’extension. Cette fonctionnalité permet à une extension déclarer le fabricant de matériel de serveur et il prend en charge les modèles, et lorsqu’un utilisateur se connecte à un serveur ou cluster pour lequel une extension est disponible, une bannière de notification s’affichera pour facilement installer l’extension. Les développeurs d’extensions sera en mesure d’obtenir plus de visibilité pour leurs extensions et les utilisateurs seront en mesure de découvrir facilement des fonctionnalités de gestion pour leurs serveurs.

![Bannière de découverte d’extension](../../media/extend-guides-extension-discovery-banner/extension-discovery-banner.png)

## <a name="how-the-extension-discovery-banner-works"></a>Fonctionne de la bannière de découverte d’extension

Une fois Windows Admin Center est lancé, il se connecter aux flux d’extension inscrites et extraire les métadonnées pour les packages d’extension disponibles. Lorsqu’un utilisateur se connecte à un serveur ou un cluster dans Windows Admin Center, nous lirons le fabricant de matériel de serveur et le modèle à afficher dans l’outil vue d’ensemble. Si nous trouvons une extension qui déclare qu’il prend en charge le fabricant du serveur et/ou les modèles, nous affichons une bannière pour informer l’utilisateur. En cliquant sur le lien « Set » prendra l’utilisateur pour le Gestionnaire d’extensions où ils peuvent installer l’extension.

## <a name="how-to-implement-the-extension-discovery-banner"></a>Comment implémenter la bannière de découverte d’extension

Les métadonnées de « tags » dans le fichier .nuspec sont utilisée pour déclarer le fabricant du matériel et/ou votre prend en charge de l’extension des modèles. Balises sont séparées par des espaces, et vous pouvez ajouter un fabricant ou balise de modèle ou les deux pour déclarer la prise en charge fabricant et/ou les modèles. Le format de balise est ``"[value type]_[value condition]"`` où [type de valeur] est « Fabricant » ou « Model » (non respect de la casse) et [valeur condition] est un [expression régulière Javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions) définissant le fabricant ou chaîne de modèle et [type de valeur] et [ condition de valeur] séparées par un trait de soulignement. Cette chaîne est ensuite encodée à l’aide d’URI de codage et ajoutées à la chaîne de métadonnées .nuspec « tags ».

### <a name="example"></a>Exemple

Supposons que j’ai développé une extension qui prend en charge des serveurs à partir d’une société nommée Contoso Inc., avec le modèle de nom R3xx et R4xx.

1. La balise pour le fabricant serait ``"Manufacturer_/Contoso Inc./"``. L’étiquette pour les modèles peut être ``"Model_/^R[34][0-9]{2}$/"``. Selon la rigueur avec laquelle vous souhaitez définir la condition de correspondance, il y a différentes façons de définir votre expression régulière. Vous pouvez également diviser les balises de fabricant ou du modèle en plusieurs balises, par exemple, la balise de modèle peut également être ``"Model_/R3../ Model_/R4../"``.
2. Vous pouvez tester l’expression régulière avec la Console de DevTools de votre navigateur web. Dans Edge ou Chrome, appuyer sur F12 pour ouvrir la fenêtre DevTools et dans l’onglet de la Console, tapez la suivante, puis appuyez sur ENTRÉE :

   ```javascript
   var regex = /^R[34][0-9]{2}$/
   ```

   Si vous tapez et exécutez la commande suivante, il retourne 'true'.

   ```javascript
   regex.test('R300')
   ```

   Et si vous exécutez la commande suivante, elle retourne « false ».

   ```javascript
   regex.test('R500')
   ```

3. Une fois que vous avez vérifié l’expression régulière, vous pouvez l’encoder dans la DevTools Console également, à l’aide de la méthode Javascript suivante :

   ```javascript
   encodeURI(/^R[34][0-9]{2}$/)
   ```

   Le format final de la chaîne de balise à ajouter à votre fichier .nuspec serait :

   ```
   <tags>Manufacturer_/Contoso%20Inc./ Model_/%5ER%5B34%5D%5B0-9%5D%7B2%7D$/</tags>
   ```

> [!Tip]
> Nous sommes conscients qu’un fabricant de matériel peut-être avoir un très large éventail de noms de modèle dont certaines peuvent être prises en charge alors que certains ne le sont pas. N’oubliez pas que cette fonctionnalité est destinée à faciliter la **découverte** de votre extension, mais il ne devra pas être un inventaire parfaitement à jour de tous vos modèles. Vous pouvez définir votre expression régulière pour être une expression plus simple qui correspond à un sous-ensemble de vos modèles. Un utilisateur ne peut pas voir la bannière de découverte s’ils se connectent tout d’abord à un modèle de serveur qui ne correspond pas à la condition, mais tôt ou tard, elles se connectent à un autre serveur qui effectue et sera détecter et installer l’extension. Vous pouvez également envisager la définition d’une expression régulière simple qui seulement correspond à votre nom de fabricant. Dans certains cas, votre extension ne peut pas réellement en charge un modèle spécifique, mais vous pouvez utiliser la [fonctionnalité d’affichage de l’outil dynamique](./dynamic-tool-display.md) pour définir un script PowerShell personnalisé pour vérifier la prise en charge du modèle et afficher uniquement votre extension, le cas échéant, ou Fournit une fonctionnalité limitée dans votre extension pour les modèles qui ne prennent pas en charge toutes les fonctionnalités.