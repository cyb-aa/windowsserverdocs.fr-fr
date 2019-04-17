---
ms.assetid: 33b80a3f-67f3-4da7-ac4a-7fd2232fbd5d
title: "Serveur de fédération autonome à l’aide de WID"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9ec4150a7d3adfaac786219d253e1d0898c18204
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="stand-alone-federation-server-using-wid"></a>Serveur de fédération autonome à l’aide de WID

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Un serveur de fédération autonome dans ActiveDirectory Federation Services \(ADFS\) se compose d’un serveur qui héberge un Service de fédération configurés pour utiliser la base de données interne Windows \(WID\). Cette topologie ADFS est pour les laboratoires de test. Nous déconseillons il pour les environnements de production, car il a une limite d’uniquement un serveur de fédération, et il ne peut pas servir à évoluer vers d’autres serveurs.  
  
Si vous souhaitez ajouter des serveurs de fédération supplémentaires à votre laboratoire de test, vous devez reconstruire le Service de fédération à partir de zéro en déployant un des autres topologies mentionnés plus loin dans cette section. Par conséquent, nous vous recommandons d’utiliser cette topologie pour un laboratoire de test ou un environnement proof\-maximum\-concept dans votre réseau de test privé dans lequel un seul serveur de fédération est adéquat, comme illustré dans l’illustration suivante.  
  
![Serveur à l’aide de WID](media/FedServerWID.gif)  
  
## <a name="test-lab-considerations"></a>Considérations en matière de laboratoire de test  
Cette section décrit des considérations sur le public, les avantages et les limites qui sont associés à cette topologie pour les environnements de laboratoire de test.  
  
### <a name="who-should-use-this-topology"></a>Qui doit utiliser cette topologie?  
  
-   \(IT\) professionnels de l’informatique ou les architectes informatiques want évaluer ou développer une preuve de concept de cette technologie  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quels sont les avantages de l’utilisation de cette topologie?  
  
-   Facile à configurer dans un environnement de laboratoire de test  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quelles sont les limites de l’utilisation de cette topologie?  
  
-   Uniquement un serveur de fédération par le Service de fédération \ (aucune fonctionnalité d’évoluer vers un farm\)  
  
-   Non redondantes \ (qu’une seule instance de l’exists\ de base de données de configuration ADFS)  
  

## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
