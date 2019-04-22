---
ms.assetid: 9eab8c43-a0f2-4d19-a5a4-e1399f0d5f25
title: Déterminez votre stratégie d’application fédérée dans le partenaire de ressource
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: aca47658cc5a20f63dbd59a26ebe135dd04def92
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59811930"
---
# <a name="determine-your-federated-application-strategy-in-the-resource-partner"></a>Déterminez votre stratégie d’application fédérée dans le partenaire de ressource

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Une partie importante de la conception d’un nouveau Active Directory Federation Services \(AD FS\) infrastructure dans l’organisation partenaire ressource consiste à déterminer le jeu complet des applications et services qui seront utilisés pour participer à la fédération et les partenaires de compte seront les destinataires de ces ressources. Avant de concevoir une application fédérée et une stratégie de services, posez-vous les questions suivantes :  
  
-   Serez vous activer et de déployer une application ASP.NET ou un Windows Communication Foundation \(WCF\) service pour la fédération ?  
  
-   Les utilisateurs de votre réseau d’entreprise auront-ils besoin d’accéder à l’application fédérée ou au service via l’authentification intégrée de Windows ?  
  
-   Les utilisateurs de votre réseau de périmètre devront-ils utiliser l’application fédérée ou le service ? Dans ce cas, l’authentification intégrée de Windows sera-t-elle nécessaire ?  
  
-   Tous les serveurs Web qui hébergent des applications fédérées exécutent-ils un système d’exploitation Windows Server et les Internet Information Services \(IIS\)?  
  
-   Qui sera le destinataire des ressources fournies par l’application fédérée ou le service ?  
  
Ces questions vous aideront à planifier une conception AD FS robuste. Cela vous permettra également de créer une application fédérée et une stratégie de services rentables et efficaces en matière d’utilisation des ressources. Pour plus d’informations sur la conception de l’application fédérée et de la stratégie de services les plus appropriées pour votre organisation, consultez les rubriques suivantes de ce guide :  
  
-   [Fournir l’accès aux utilisateurs de votre Active Directory pour vos Services et Applications prenant en charge les revendications](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [Fournir l’accès aux utilisateurs de votre Active Directory pour les Applications et Services d’autres organisations](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
-   [Fournir aux utilisateurs d’une autre organisation un accès à vos Services et Applications prenant en charge les revendications](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
Pour plus d’informations sur la création d’un revendications\-application ASP.NET prenant en charge ou un service WCF, consultez [Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

