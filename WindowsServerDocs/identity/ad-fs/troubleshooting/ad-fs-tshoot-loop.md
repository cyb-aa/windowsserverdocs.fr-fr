---
title: AD FS la détection des boucles
description: Ce document décrit comment résoudre les problèmes de détection de boucle
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2f8842dc53756cc4f65b6d6794a8c4952e111c00
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385344"
---
# <a name="ad-fs-troubleshooting---loop-detection"></a>AD FS la détection des boucles 
 
Le bouclage dans AD FS se produit lorsqu’une partie de confiance rejette en permanence un jeton de sécurité valide et redirige vers AD FS.

## <a name="loop-detection-cookie"></a>Cookie de détection de boucle
Pour éviter ce problème, AD FS a implémenté ce qui est appelé un cookie de détection de boucle. Par défaut, AD FS écrit un cookie sur des clients Web passifs nommés **MSISLoopDetectionCookie**. Ce cookie contient une valeur d’horodatage et un nombre de jetons émis.  Cela permet à AD FS d’effectuer le suivi de la fréquence et du nombre de fois où un client a visité le service FS (Federation Service) dans un intervalle de temps spécifique.

Si un client passif parcourt le service FS (Federation Service) pour un jeton cinq fois dans un délai de 20 secondes, AD FS génère l’erreur suivante :

@NO__T 0MSIS7042 : La même session de navigateur client a effectué des demandes « {0} » au cours des « {1} » dernières secondes. Pour plus d’informations, contactez votre administrateur. **

La saisie dans des boucles infinies est souvent due à une application de partie de confiance incorrecte qui ne consomme pas correctement le jeton émis par AD FS, et l’application renvoie le client passif à AD FS, de façon répétée, pour un nouveau jeton.  AD FS est le client passif un nouveau jeton à chaque fois, à condition qu’ils ne dépassent pas 5 demandes dans un délai de 20 secondes. 

## <a name="adjusting-the-loop-detection-cookie"></a>Ajustement du cookie de détection de boucle
Vous pouvez utiliser PowerShell pour modifier le nombre de jetons émis et la valeur TimeSpan.

```powershell
Set-AdfsProperties -LoopDetectionMaximumTokensIssuedInterval 5  -LoopDetectionTimeIntervalInSeconds 20
```
La valeur minimale pour **LoopDetectionMaximumTokensIssuedInterval** est 1.

La valeur minimale pour **LoopDetectionTimeIntervalInSeconds** est 5.

Vous pouvez également désactiver la détection de boucle lorsque vous effectuez des tests de performances.

```powershell
Set-AdfsProperties -EnableLoopDetection $false
```

>[!IMPORTANT]
>Il n’est pas recommandé de désactiver définitivement la détection de boucle, car cela empêche les utilisateurs d’entrer dans des États de boucle infinis.


## <a name="next-steps"></a>Étapes suivantes

- [Résolution des problèmes AD FS](ad-fs-tshoot-overview.md)



