---
ms.assetid: 33b80a3f-67f3-4da7-ac4a-7fd2232fbd5d
title: Serveur de fédération autonome utilisant la base de données interne Windows
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9ec4150a7d3adfaac786219d253e1d0898c18204
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876520"
---
# <a name="stand-alone-federation-server-using-wid"></a>Serveur de fédération autonome utilisant la base de données interne Windows

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un support\-serveur de fédération autonome dans Active Directory Federation Services \(AD FS\) se compose d’un seul serveur qui héberge un Service de fédération configurés pour utiliser la base de données interne Windows \(WID\). Cette topologie AD FS est pour les laboratoires de test. Nous déconseillons il pour les environnements de production, car il a une limite de serveur de fédération qu’une seule, et il ne peut pas être utilisé pour monter en puissance à d’autres serveurs.  
  
Si vous souhaitez ajouter des serveurs de fédération supplémentaires à votre laboratoire de test, vous devez reconstruire le Service de fédération à partir de zéro en déployant un des autres topologies indiquées plus loin dans cette section. Par conséquent, nous vous recommandons d’utiliser cette topologie pour un laboratoire de test ou une preuve\-de\-environnement concept dans votre réseau de test privé dans lequel un serveur de fédération unique convient, comme indiqué dans l’illustration suivante.  
  
![serveur à l’aide de WID](media/FedServerWID.gif)  
  
## <a name="test-lab-considerations"></a>Considérations de laboratoire de test  
Cette section décrit divers points de vue sur le public visé, les avantages et les limites qui sont associés à cette topologie pour les environnements de laboratoire de test.  
  
### <a name="who-should-use-this-topology"></a>Qui doit utiliser cette topologie ?  
  
-   Technologies de l’information \(informatique\) professionnels ou aux architectes informatiques qui souhaitent évaluer ou développer une preuve de concept pour cette technologie  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quels sont les avantages de l’utilisation de cette topologie ?  
  
-   Facile à configurer dans un environnement de laboratoire de test  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quelles sont les limitations de l’utilisation de cette topologie ?  
  
-   Uniquement un serveur de fédération par le Service de fédération \(n’arrive pas à monter en puissance à une batterie de serveurs\)  
  
-   Non redondantes \(n'existe qu’une seule instance de la base de données de configuration AD FS\)  
  

## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
