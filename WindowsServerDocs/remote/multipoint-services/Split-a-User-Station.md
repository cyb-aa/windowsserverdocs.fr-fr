---
title: Diviser une station utilisateur
description: Découvrez comment fractionner un affichage dans MultiPoint services pour que deux utilisateurs puissent utiliser la même station
ms.date: 07/08/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: f0d1fc9c-f5ea-45bc-a8da-623c5d081cdf
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: db828da06045bb884db138458f875af1d4d5b99d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853892"
---
# <a name="split-a-user-station"></a>Diviser une station utilisateur
Tout moniteur de station MultiPoint services dont la résolution est supérieure à 1024x768 peut être divisé en deux stations à l’aide de la tâche de **fractionnement** de la station sous l’onglet **stations** . Le bureau qui est présent sur le moniteur au moment où le fractionnement se produit est déplacé vers la moitié gauche du moniteur, et une nouvelle station est créée à la moitié droite du même moniteur. Pour achever sa création, la nouvelle station doit être mappée à un clavier, une souris et un concentrateur USB. Une fois une station divisée, un utilisateur peut se connecter à la station de gauche tandis qu’un autre utilisateur se connecte à la station de droite.  
  
Les avantages de l’utilisation d’une station à écran partagé peuvent inclure :  
  
-   Réduction des coûts et de la place nécessaire, puisque le système MultiPoint Services admet plus de stagiaires  
  
-   Autoriser deux élèves à collaborer ensemble, côte à côte sur un projet  
  
-   Possibilité pour l’enseignant d’expliquer une procédure sur une station tandis que le stagiaire suit la démonstration sur l’autre station  
   
> [!NOTE]  
> Quand vous divisez une station, la session active sur la station est suspendue. L’utilisateur doit se reconnecter à la station après la division pour reprendre son travail.  
  
**Pour diviser une station :**  
  
1.  Dans le gestionnaire MultiPoint, en mode station, cliquez sur l’onglet **stations** .  
  
2.  Dans la colonne **Station**, cliquez sur le nom de la station que vous voulez diviser.  
  
3.  Sous **Tâches de stations**, cliquez sur **Diviser la station**.  
  
**Pour replacer une station partagée sur une seule station :**  
  
1.  Dans le gestionnaire MultiPoint, en mode station, cliquez sur l’onglet **stations** .  
  
2.  Dans la colonne **Station**, cliquez sur le nom de la station dont vous voulez terminer la division.  
  
3.  Sous **Tâches de stations**, cliquez sur **Annuler la division de la station**.  
  
## <a name="see-also"></a>Voir aussi  
[Gérer les stations d’utilisateur](Manage-User-Stations.md)