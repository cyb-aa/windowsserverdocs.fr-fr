---
title: Résolution des problèmes d’AD FS - détection en boucle
description: Ce document décrit comment résoudre les problèmes de détection de boucle
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cc8eeb11e44da3b8f26b1ab94143c189bca9ed38
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830910"
---
# <a name="ad-fs-troubleshooting---loop-detection"></a>Résolution des problèmes d’AD FS - détection en boucle 
 
Bouclage dans AD FS se produit lorsqu’une partie de confiance rejette un jeton de sécurité valide en permanence et redirige vers AD FS.

## <a name="loop-detection-cookie"></a>Cookie de détection de boucle
Pour éviter ce problème, AD FS a implémenté ce que l'on appelle un cookie de détection de boucle. Par défaut, AD FS écrit un cookie pour les clients passifs web nommés **MSISLoopDetectionCookie**. Ce cookie conserve une valeur d’horodatage et un nombre de jetons émis de valeur.  Cela permet à ADFS d’effectuer le suivi de la fréquence et le nombre de fois où un client a visité le Service de fédération au sein d’un intervalle de temps spécifique.

Si un client passif visite le Service de fédération pour un jeton de cinq (5) fois pendant 20 secondes, AD FS génère l’erreur suivante :

**MSIS7042 : La même session de navigateur du client a apporté '{0}'requêtes dans le dernier'{1}' secondes. Pour plus d’informations, contactez votre administrateur.**

Entrer dans des boucles infinies est souvent provoquée par une application de tiers qui ne consomme pas correctement le jeton émis par AD FS de partie de confiance anormal, et de l’application est renvoyer le client passif à AD FS, à plusieurs reprises, pour un nouveau jeton.  AD FS est va émettre le client passif un nouveau jeton chaque fois, tant qu’ils ne dépassent pas 5 demandes au sein de 20 secondes. 

## <a name="adjusting-the-loop-detection-cookie"></a>Ajuster le cookie de détection de boucle
Vous pouvez utiliser PowerShell pour modifier le nombre de jetons émis de valeur et la valeur de l’intervalle de temps.

```powershell
Set-AdfsProperties -LoopDetectionMaximumTokensIssuedInterval 5  -LoopDetectionTimeIntervalInSeconds 20
```
La valeur minimale pour **LoopDetectionMaximumTokensIssuedInterval** est 1.

La valeur minimale pour **LoopDetectionTimeIntervalInSeconds** est 5.

Vous pouvez également désactiver la détection des boucles lorsque vous effectuez un test de performances.

```powershell
Set-AdfsProperties -EnableLoopDetection $false
```

>[!IMPORTANT]
>Il est déconseillé de désactiver la détection des boucles de façon permanente car cela empêche les utilisateurs d’entrer dans les États de boucle infinie.


## <a name="next-steps"></a>Étapes suivantes

- [Résolution des problèmes de AD FS](ad-fs-tshoot-overview.md)



