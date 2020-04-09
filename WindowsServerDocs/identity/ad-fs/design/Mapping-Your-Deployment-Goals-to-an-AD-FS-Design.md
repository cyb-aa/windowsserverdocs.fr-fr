---
ms.assetid: 68979914-8a1c-465a-bd37-08df30722d69
title: Mappage de vos objectifs de déploiement sur une conception AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 4154f46bdb1a7b3a2c81eba6882a6a095a16ee45
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853052"
---
# <a name="mapping-your-deployment-goals-to-an-ad-fs-design"></a>Mappage de vos objectifs de déploiement sur une conception AD FS


Une fois que vous avez terminé de consulter les Services ADFS existants \(AD FS\) objectifs du déploiement et que vous avez déterminé les objectifs liés à votre déploiement, vous pouvez mapper ces objectifs à une conception de AD FS spécifique. Pour plus d’informations sur AD FS objectifs de déploiement prédéfinis, consultez [identification de vos objectifs de déploiement AD FS](Identifying-Your-AD-FS-Deployment-Goals.md).  
  
Utilisez le tableau suivant pour déterminer quelle AD FS conception correspond à la combinaison appropriée des objectifs de déploiement AD FS pour votre organisation. Ce tableau fait uniquement référence aux deux principales conceptions de AD FS, comme décrit dans ce guide. Toutefois, vous pouvez créer une conception hybride ou personnalisée AD FS à l’aide de n’importe quelle combinaison des objectifs de déploiement AD FS pour répondre aux besoins de votre organisation.  
  
|AD FS objectif de déploiement|[Conception SSO web](Web-SSO-Design.md)|[Conception SSO de web fédéré](Federated-Web-SSO-Design.md)|  
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|  
|[Fournir à vos utilisateurs Active Directory un accès à vos applications et services prenant en charge les revendications](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)|Non|Oui, dans le partenaire de compte|  
|[Fournir à vos utilisateurs Active Directory un accès aux applications et services d’autres organisations](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)|Non|Oui, facultatif dans le partenaire de compte|  
|[Fournir aux utilisateurs d’une autre organisation un accès à vos applications et services prenant en charge les revendications](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)|Oui|Oui|  

## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
  

