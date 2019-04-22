---
ms.assetid: 2a2f493a-9796-454a-9721-e223b799dfa7
title: Planification de l’emplacement des contrôleurs de domaine racines de forêt
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 57eafcc884a827d98c249e2da0c0af6888abc5b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823640"
---
# <a name="planning-forest-root-domain-controller-placement"></a>Planification de l’emplacement des contrôleurs de domaine racines de forêt

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Les contrôleurs de domaine racine de forêt sont nécessaires pour créer des chemins d’approbation pour les clients qui doivent accéder aux ressources dans des domaines autres que leurs propres. Placer les contrôleurs de domaine racine de forêt dans les sites des concentrateurs et aux emplacements qui hébergent des centres de données. Si les utilisateurs dans un emplacement donné ont besoin d’accéder aux ressources d’autres domaines dans le même emplacement et la disponibilité du réseau entre le centre de données et l’emplacement de l’utilisateur n’est pas fiable, vous pouvez ajouter un contrôleur de domaine de racine de forêt dans l’emplacement ou créer un raccourci d’approbation entre les deux domaines. Il est plus rentable pour créer un raccourci d’approbation entre les domaines, sauf si vous avez d’autres raisons pour placer un contrôleur de domaine de racine de forêt dans cet emplacement.  
  
Raccourcis d’approbation aide à optimiser les demandes d’authentification effectuées à partir d’utilisateurs situés dans un domaine. Pour plus d’informations sur les raccourcis d’approbation entre domaines, consultez l’article [quels cas créer un raccourci d’approbation](https://go.microsoft.com/fwlink/?LinkId=107061).  
  
Pour une feuille de calcul pour vous aider à documenter votre emplacement du contrôleur de domaine racine forêt, consultez [travail aides pour Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip , puis ouvrez « Emplacement d’un contrôleur de domaine » (DSSTOPO_4.doc).  
  
Vous devrez consulter ces informations lorsque vous créez le domaine racine de forêt. Pour plus d’informations sur le déploiement du domaine racine de forêt, consultez [déploiement d’un domaine racine de forêt Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
