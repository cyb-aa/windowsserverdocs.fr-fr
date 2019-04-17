---
ms.assetid: 2a2f493a-9796-454a-9721-e223b799dfa7
title: "Planification d’emplacement de contrôleur de domaine de racine de forêt"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 374ce31c61c666302e2b00a8f365c289cb8799f8
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="planning-forest-root-domain-controller-placement"></a>Planification d’emplacement de contrôleur de domaine de racine de forêt

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Les contrôleurs de domaine racine de forêt sont nécessaires pour créer des chemins d’approbation pour les clients qui doivent accéder aux ressources dans des domaines autres que leurs propres. Placez les contrôleurs de domaine racine de forêt dans les emplacements de hub et aux emplacements qui hébergent des centres de données. Si les utilisateurs dans un emplacement donné ont besoin d’accéder aux ressources d’autres domaines dans le même emplacement et la disponibilité du réseau entre le centre de données et l’emplacement de l’utilisateur n’est pas fiable, vous pouvez ajouter un contrôleur de domaine de racine de forêt à l’emplacement ou créer un raccourci d’approbation entre les deux domaines. Il est plus rentable pour créer un raccourci d’approbation entre les domaines, sauf si vous disposez d’autres raisons pour placer un contrôleur de domaine de racine de forêt dans cet emplacement.  
  
Raccourcis d’approbation aide à optimiser les demandes d’authentification effectuées à partir des utilisateurs situés dans un domaine. Pour plus d’informations sur les raccourcis d’approbation entre domaines, voir quels cas créer un raccourci d’approbation ([https://go.microsoft.com/fwlink/?LinkId=107061](https://go.microsoft.com/fwlink/?LinkId=107061)).  
  
Pour une feuille de calcul pour vous aider à documenter votre emplacement des contrôleurs de domaine racine forêt, voir tâche identificateurs d’applet pour Windows Server2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), télécharger Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip, et Ouvrez (DSSTOPO_4.doc) "Placement du contrôleur de domaine".  
  
Vous devrez faire référence à ces informations lorsque vous créez le domaine racine de forêt. Pour plus d’informations sur le déploiement du domaine racine de forêt, voir [déploiement d’un domaine racine de forêt Windows Server2008](https://technet.microsoft.com/library/cc731174.aspx).  
  


