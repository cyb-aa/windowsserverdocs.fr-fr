---
ms.assetid: 97999892-29c6-4076-be19-5e5259d8ada6
title: "Déploiement de serveurs de fédération"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: f80425b6f062040c51357353038fd07ff0a79ae6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="interoperating-with-ad-fs-1x"></a>Il interagit avec AD FS 1.x

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Pour assurer l’interopérabilité entre \(ADFS\) ActiveDirectory Federation Services dans Windows Server® 2012 et ADFS 1. *x*, effectuez une ou plusieurs des tâches suivantes, selon les besoins de votre organisation:  
  
-   Planifier l’interopérabilité entre les services ADFS dans Windows Server2012 et les versions antérieures d’ADFS et en savoir que plus sur l’ID de nom de type de revendication. Pour plus d’informations, voir [planification de l’interopérabilité avec ADFS 1.x](https://technet.microsoft.com/library/ff678040.aspx).  
  
-   Si vous allez envoyer des revendications à partir d’un Service de fédération ADFS dans Windows Server2012 qui peut être consommée par un ADFS 1. *x* Service de fédération, consultez [liste de vérification: Configuring ADFS to Send Claims to an ADFS 1.x Federation Service](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Federation-Service.md).  
  
-   Si vous allez envoyer des revendications à partir d’un Service de fédération ADFS dans Windows Server2012 pouvant être consommée par une application qui est hébergée par un serveur Web exécutant les services ADFS 1. *x* agent Web prenant en claims\, voir [liste de vérification: Configuring ADFS to Send Claims to an ADFS 1.x Claims-Aware Web Agent](Checklist--Configuring-AD-FS-to-Send-Claims-to-an-AD-FS-1.x-Claims-Aware-Web-Agent.md).  
  
-   Si vous allez envoyer des revendications à partir d’un ADFS 1. *x* Service de fédération pour être consommée par un Service de fédération ADFS dans Windows Server2012, voir [liste de vérification: configuration d’ADFS pour consommer les revendications à partir d’ADFS 1.x](Checklist--Configuring-AD-FS--to-Consume-Claims-from-AD-FS-1.x.md).  
  
## <a name="differences-between-federation-service-settings"></a>Différences entre les paramètres du Service de fédération  
Bien que la plupart des ADFS 1. *x* travail paramètres de Service de fédération de la même manière que le Service de fédération ADFS dans les paramètres de Windows Server2012, certains noms de paramètre ont été modifiés. Le tableau suivant répertorie les noms des paramètres pour un ADFS 1. *x* Service de fédération et leurs noms équivalents pour un Service de fédération ADFS dans Windows Server2012.  
  
|Paramètre de Service de fédération ADFS 1.x|Service de fédération ADFS équivalents dans le paramètre de Windows Server2012  
|----------------------------------------|---------------------------------------------------------------------------------------------------------- 
|Partenaire de compte|Approbation de fournisseur de revendications  
|Partenaire de ressource|Approbation de partie de confiance 
|Application|Approbation de partie de confiance  
|Propriétés de l’application|Propriétés d’approbation de partie de confiance  
|URL de l’application|Identificateur de partie de confiance et les URL de point de terminaison WS-Federation passif  
|URI du Service de fédération|Identificateur du Service de fédération  
|URL de point de terminaison de Service de fédération|URL de point de terminaison passifs WS-Federation  
  
## <a name="see-also"></a>Voir aussi  
[ADFS et l’interopérabilité de ADFS 1.x](https://go.microsoft.com/fwlink/?LinkId=200776)  
  

