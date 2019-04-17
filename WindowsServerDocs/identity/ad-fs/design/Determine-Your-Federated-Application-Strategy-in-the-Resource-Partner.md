---
ms.assetid: 9eab8c43-a0f2-4d19-a5a4-e1399f0d5f25
title: "Déterminer votre stratégie d’Application fédérée dans le partenaire de ressource"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: aca47658cc5a20f63dbd59a26ebe135dd04def92
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="determine-your-federated-application-strategy-in-the-resource-partner"></a>Déterminer votre stratégie d’Application fédérée dans le partenaire de ressource

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Une partie importante de la conception d’une nouvelle infrastructure de Services de fédération ActiveDirectory \(ADFS\) dans l’organisation partenaire de ressource consiste à déterminer le jeu complet des applications et services qui sera utilisé pour participer à la fédération et les partenaires de compte seront les destinataires de ces ressources. Avant de concevoir une application fédérée et une stratégie de services, posez-vous les questions suivantes:  
  
-   L’activation et déploiement d’une application ASP.NET ou un service WindowsCommunicationFoundation \(WCF\) pour la fédération?  
  
-   Les utilisateurs sur votre réseau d’entreprise nécessitent un accès à l’application fédérée ou le service via l’authentification intégrée de Windows?  
  
-   Sera l’application fédérée ou le service utilisé par les utilisateurs dans votre réseau de périmètre? Dans ce cas, l’authentification intégrée Windows sera requise?  
  
-   Tous les serveurs Web sont que les applications hôtes fédérées exécutent-ils un système d’exploitation Windows Server et des \(IIS\) Internet Information Services?  
  
-   Qui sera fournir des ressources pour l’application fédérée ou le service?  
  
Ces questions vous aidera à planifier une conception ADFS solide. Il sera également vous aider à créer une application fédérée et la stratégie de services rentable et la ressource efficace. Pour plus d’informations sur la conception la plus appropriée application fédérée et stratégie de services de votre organisation, consultez les rubriques suivantes de ce guide:  
  
-   [Fournir vos utilisateurs Active Directory un accès à vos Services et Applications prenant en charge les revendications](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [Fournir un accès à vos utilisateurs Active Directory pour les Applications et Services d’autres organisations](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
-   [Fournir aux utilisateurs d’une autre organisation d’accéder à vos Services et Applications prenant en charge les revendications](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
Pour plus d’informations sur la création d’une application prenant en charge claims\ ASP.NET ou un service WCF, voir [SDK de WindowsIdentityFoundation](https://go.microsoft.com/fwlink/?LinkId=122266).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

