---
ms.assetid: 97999892-29c6-4076-be19-5e5259d8ada6
title: Déploiement de serveurs de fédération
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: f2aaca5ffc846c41af82c276750c564db38b5020
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359509"
---
# <a name="interoperating-with-ad-fs-1x"></a>Interaction avec AD FS 1.x

Pour l’interopérabilité entre Services ADFS \(AD FS @ no__t-1 dans Windows Server® 2012 et AD FS 1. *x*, effectuez une ou plusieurs des tâches suivantes, en fonction des besoins de votre organisation :  
  
-   Planifiez l’interopérabilité entre les AD FS dans Windows Server 2012 et les versions précédentes de AD FS, et en savoir plus sur le type de revendication ID de nom. Pour plus d’informations, consultez [planification de l’interopérabilité avec AD FS 1. x](https://technet.microsoft.com/library/ff678040.aspx).  
  
-   Si vous enverrez des revendications à partir d’un AD FS service FS (Federation Service) dans Windows Server 2012 qui peut être consommé par un AD FS 1. *x* service FS (Federation Service), consultez [Checklist : Configuration de AD FS pour envoyer des revendications à un AD FS 1. x service FS (Federation Service) @ no__t-0.  
  
-   Si vous enverrez des revendications à partir d’un AD FS service FS (Federation Service) dans Windows Server 2012 qui peut être utilisé par une application hébergée par un serveur Web exécutant le AD FS 1. *x* claims @ no__t-1aware Web agent, voir [Checklist : Configuration de AD FS pour envoyer des revendications à un AD FS 1. x agent Web prenant en charge les revendications @ no__t-0.  
  
-   Si vous enverrez des revendications à partir d’un AD FS 1. *x* service FS (Federation Service) être consommées par une service FS (Federation Service) AD FS dans Windows Server 2012, consultez [Checklist : Configuration de AD FS pour consommer des revendications à partir de AD FS 1. x @ no__t-0.  
  
## <a name="differences-between-federation-service-settings"></a>Différences entre les paramètres de service FS (Federation Service)  
Bien que la plupart des AD FS 1. les paramètres *x* service FS (Federation Service) fonctionnent de la même façon que le AD FS service FS (Federation Service) dans les paramètres de Windows Server 2012, certains noms de paramètres ont changé. Le tableau suivant répertorie les noms des paramètres d’un AD FS 1. *x* service FS (Federation Service) et leurs noms équivalents pour un service FS (Federation Service) AD FS dans Windows Server 2012.  
  
|Paramètre de service FS (Federation Service) AD FS 1. x|service FS (Federation Service) AD FS équivalente dans le paramètre de Windows Server 2012  
|----------------------------------------|---------------------------------------------------------------------------------------------------------- 
|Partenaire de compte|Approbation de fournisseur de revendications  
|Partenaire de ressource|Approbation de partie de confiance 
|Application|Approbation de partie de confiance  
|Propriétés de l’application|Propriétés de l’approbation de partie de confiance  
|URL de l’application|Identificateur de partie de confiance et URL de point de terminaison passif WS @ no__t-0Federation  
|URI service FS (Federation Service)|Identificateur du service de fédération  
|URL du point de terminaison service FS (Federation Service)|URL du point de terminaison passif WS @ no__t-0Federation  
  
## <a name="see-also"></a>Voir aussi  
[Interopérabilité AD FS et AD FS 1. x](https://go.microsoft.com/fwlink/?LinkId=200776)  
  

