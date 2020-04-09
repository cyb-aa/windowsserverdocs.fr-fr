---
ms.assetid: 97999892-29c6-4076-be19-5e5259d8ada6
title: Déploiement de serveurs de fédération
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 800c7fc23c9b126a17e54311fc6df0d3dcf36b4c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855362"
---
# <a name="interoperating-with-ad-fs-1x"></a>Interaction avec AD FS 1.x

Pour l’interopérabilité entre Services ADFS \(AD FS\) dans Windows Server&reg; 2012 et AD FS 1. *x*, effectuez une ou plusieurs des tâches suivantes, en fonction des besoins de votre organisation :  
  
-   Planifiez l’interopérabilité entre les AD FS dans Windows Server 2012 et les versions précédentes de AD FS, et en savoir plus sur le type de revendication ID de nom. Pour plus d’informations, consultez [planification de l’interopérabilité avec AD FS 1. x](https://technet.microsoft.com/library/ff678040.aspx).  
  
-   Si vous enverrez des revendications à partir d’un AD FS service FS (Federation Service) dans Windows Server 2012 qui peut être consommé par un AD FS 1. *x* service FS (Federation Service), consultez [liste de vérification : configuration des AD FS pour envoyer des revendications à un service FS (Federation Service) AD FS 1. x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md).  
  
-   Si vous enverrez des revendications à partir d’un AD FS service FS (Federation Service) dans Windows Server 2012 qui peut être utilisé par une application hébergée par un serveur Web exécutant le AD FS 1. *x* claims\-aware Web agent, consultez [liste de vérification : configuration des AD FS pour envoyer des revendications à un agent Web prenant en charge les revendications AD FS 1. x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md).  
  
-   Si vous enverrez des revendications à partir d’un AD FS 1. *x* service FS (Federation Service) être consommées par une service FS (Federation Service) AD FS dans Windows Server 2012, consultez [liste de vérification : configuration de AD FS pour utiliser les revendications de AD FS 1. x](Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md).  
  
## <a name="differences-between-federation-service-settings"></a>Différences entre les paramètres de service FS (Federation Service)  
Bien que la plupart des AD FS 1. les paramètres *x* service FS (Federation Service) fonctionnent de la même façon que le AD FS service FS (Federation Service) dans les paramètres de Windows Server 2012, certains noms de paramètres ont changé. Le tableau suivant répertorie les noms des paramètres d’un AD FS 1. *x* service FS (Federation Service) et leurs noms équivalents pour un service FS (Federation Service) AD FS dans Windows Server 2012.  
  
|Paramètre de service FS (Federation Service) AD FS 1. x|service FS (Federation Service) AD FS équivalente dans le paramètre de Windows Server 2012  
|----------------------------------------|---------------------------------------------------------------------------------------------------------- 
|Partenaire de compte|Approbation de fournisseur de revendications  
|Partenaire de ressource|Approbation de la partie de confiance 
|Application|Approbation de la partie de confiance  
|Propriétés d'une application|Propriétés de l’approbation de partie de confiance  
|URL de l’application|Identificateur de partie de confiance et URL de point de terminaison passif de la Fédération WS\-  
|URI service FS (Federation Service)|Identificateur du service de fédération  
|URL du point de terminaison service FS (Federation Service)|URL du point de terminaison passif de la Fédération WS\-  
  
## <a name="see-also"></a>Voir aussi  
[Interopérabilité AD FS et AD FS 1. x](https://go.microsoft.com/fwlink/?LinkId=200776)  
  

