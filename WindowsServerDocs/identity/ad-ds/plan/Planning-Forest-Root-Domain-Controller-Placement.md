---
ms.assetid: 2a2f493a-9796-454a-9721-e223b799dfa7
title: Planification de l’emplacement des contrôleurs de domaine racines de forêt
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: e619d5f5e9cc317f9ba1548d5ed3a32e7bd12c2c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402546"
---
# <a name="planning-forest-root-domain-controller-placement"></a>Planification de l’emplacement des contrôleurs de domaine racines de forêt

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Des contrôleurs de domaine racine de forêt sont nécessaires pour créer des chemins d’approbation pour les clients qui ont besoin d’accéder à des ressources dans des domaines autres que leur propre. Placez les contrôleurs de domaine racine de la forêt dans les emplacements Hub et les emplacements qui hébergent les centres de centres. Si les utilisateurs d’un emplacement donné doivent accéder aux ressources d’autres domaines au même emplacement et que la disponibilité du réseau entre le centre de données et l’emplacement de l’utilisateur n’est pas fiable, vous pouvez ajouter un contrôleur de domaine racine de forêt à l’emplacement ou créer un raccourci d’approbation entre les deux domaines. Il est plus rentable de créer un raccourci d’approbation entre les domaines, sauf si vous avez d’autres raisons de placer un contrôleur de domaine racine de la forêt à cet emplacement.  
  
Les raccourcis d’approbations permettent d’optimiser les demandes d’authentification effectuées par les utilisateurs situés dans l’un ou l’autre domaine. Pour plus d’informations sur les raccourcis d’approbations entre domaines, consultez l’article savoir [quand créer un raccourci d’approbation](https://go.microsoft.com/fwlink/?LinkId=107061).  
  
Pour obtenir une feuille de calcul qui vous aide à documenter le placement du contrôleur de domaine racine de la forêt, consultez [Outils d’aide pour le kit de déploiement Windows Server 2003](https://go.microsoft.com/fwlink/?LinkID=102558), Télécharger Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip et ouvrir le domaine Placement du contrôleur» (DSSTOPO_4. doc).  
  
Vous devez faire référence à ces informations lorsque vous créez le domaine racine de forêt. Pour plus d’informations sur le déploiement du domaine racine de la forêt, voir [déploiement d’un domaine racine de forêt Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
