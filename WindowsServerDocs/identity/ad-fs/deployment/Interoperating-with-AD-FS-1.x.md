---
ms.assetid: 97999892-29c6-4076-be19-5e5259d8ada6
title: Déploiement de serveurs de fédération
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0eb9265513d5ca18da1150d3be6752d364b7cd1a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192080"
---
# <a name="interoperating-with-ad-fs-1x"></a>Interaction avec AD FS 1.x

Pour l’interopérabilité entre Active Directory Federation Services \(AD FS\) dans Windows Server® 2012 et AD FS 1. *x*, effectuez l’une ou plusieurs des tâches suivantes, selon les besoins de votre organisation :  
  
-   Planifiez l’interopérabilité entre AD FS dans Windows Server 2012 et les versions précédentes d’AD FS et en savoir que plus sur l’ID de nom de type de revendication. Pour plus d’informations, consultez [planification de l’interopérabilité avec AD FS 1.x](https://technet.microsoft.com/library/ff678040.aspx).  
  
-   Si vous allez envoyer des revendications à partir d’un Service de fédération AD FS dans Windows Server 2012 qui peuvent être consommées par an AD FS 1. *x* Service de fédération, consultez [liste de vérification : Configurer AD FS pour envoyer des revendications à un Service de fédération AD FS 1.x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md).  
  
-   Si vous allez envoyer des revendications à partir d’un Service de fédération AD FS dans Windows Server 2012 qui peuvent être utilisés par une application qui est hébergée par un serveur Web exécutant les services AD FS 1. *x* revendications\-agent Web prenant en charge, consultez [liste de vérification : Configurer AD FS pour envoyer des revendications à un Agent de Web prenant en charge les revendications AD FS 1.x](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md).  
  
-   Si vous allez envoyer des revendications à partir de l’an AD FS 1. *x* Service de fédération à être consommés par un Service de fédération AD FS dans Windows Server 2012, consultez [liste de vérification : Configurer AD FS pour utiliser les revendications à partir d’AD FS 1.x](Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md).  
  
## <a name="differences-between-federation-service-settings"></a>Différences entre les paramètres de Service de fédération  
Bien que la plupart des AD FS 1. *x* fonctionnement des paramètres de Service de fédération dans la même façon que le Service de fédération AD FS dans les paramètres de Windows Server 2012, certains noms de paramètre ont été modifiés. Le tableau suivant répertorie les noms des paramètres pour un AD FS 1. *x* Service de fédération et leurs noms équivalents pour un Service de fédération AD FS dans Windows Server 2012.  
  
|Paramètre de Service de fédération AD FS 1.x|Service de fédération AD FS équivalents dans le paramètre de Windows Server 2012  
|----------------------------------------|---------------------------------------------------------------------------------------------------------- 
|Partenaire de compte|Approbation de fournisseur de revendications  
|Partenaire de ressource|Approbation de partie de confiance 
|Application|Approbation de partie de confiance  
|Propriétés de l’application|Propriétés d’approbation de partie confiance  
|URL de l’application|Partie de confiance identificateur du tiers et WS\-URL de point de terminaison passif de fédération  
|URI du Service de fédération|Identificateur du service de fédération  
|URL de point de terminaison de Service de fédération|WS\-URL de point de terminaison passif de fédération  
  
## <a name="see-also"></a>Voir aussi  
[AD FS et AD FS 1.x interopérabilité](https://go.microsoft.com/fwlink/?LinkId=200776)  
  

