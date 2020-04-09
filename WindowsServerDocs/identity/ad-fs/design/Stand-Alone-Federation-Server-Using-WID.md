---
ms.assetid: 33b80a3f-67f3-4da7-ac4a-7fd2232fbd5d
title: Serveur de fédération autonome utilisant la base de données interne Windows
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7253691cff4cc83032e4a345682ca1e43bafddc4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858772"
---
# <a name="stand-alone-federation-server-using-wid"></a>Serveur de fédération autonome utilisant la base de données interne Windows

Un serveur de Fédération autonome\-dans Services ADFS \(AD FS\) se compose d’un serveur unique qui héberge une service FS (Federation Service) configurée pour utiliser la base de données interne Windows \(le schéma WID.\) Cette AD FS topologie est destinée aux laboratoires de test. Nous ne la recommandons pas pour les environnements de production, car elle n’a qu’une limite d’un seul serveur de Fédération et ne peut pas être utilisée pour augmenter la taille des serveurs.  
  
Si vous souhaitez ajouter des serveurs de Fédération supplémentaires à votre laboratoire de test, vous devez recréer le service FS (Federation Service) à partir de zéro en déployant les autres topologies mentionnées plus loin dans cette section. Par conséquent, nous vous recommandons d’utiliser cette topologie pour un laboratoire de test ou une preuve\-de l’environnement de\-concept dans votre réseau de test privé, dans lequel un seul serveur de Fédération est approprié, comme le montre l’illustration suivante.  
  
![serveur utilisant WID](media/FedServerWID.gif)  
  
## <a name="test-lab-considerations"></a>Considérations relatives au laboratoire de test  
Cette section décrit les différentes considérations à prendre en compte concernant le public concerné, les avantages et les limitations associés à cette topologie pour les environnements de laboratoire de test.  
  
### <a name="who-should-use-this-topology"></a>Qui doit utiliser cette topologie ?  
  
-   Technologies de l’information \(informatique\) des professionnels de l’informatique ou des architectes informatiques qui souhaitent évaluer ou développer une preuve de concept pour cette technologie  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quels sont les avantages de l’utilisation de cette topologie ?  
  
-   Facile à configurer dans un environnement de laboratoire de test  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quelles sont les limitations de l’utilisation de cette topologie ?  
  
-   Un seul serveur de Fédération par service FS (Federation Service) \(aucune possibilité de mise à l’échelle vers une batterie de serveurs\)  
  
-   Non redondant \(il existe une seule instance de la base de données de configuration AD FS\)  
  

## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
