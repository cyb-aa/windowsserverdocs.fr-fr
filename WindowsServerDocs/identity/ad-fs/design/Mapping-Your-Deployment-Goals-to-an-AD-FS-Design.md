---
ms.assetid: 68979914-8a1c-465a-bd37-08df30722d69
title: Mappage de vos objectifs de déploiement sur une conception AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 13d8ae8b8f3e4c8160f61284e5fb97e21b6a51b6
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191250"
---
# <a name="mapping-your-deployment-goals-to-an-ad-fs-design"></a>Mappage de vos objectifs de déploiement sur une conception AD FS


Après avoir examiné les Services de fédération Active Directory existant \(AD FS\) objectifs de déploiement et déterminé ceux liés à votre déploiement, vous pouvez mapper ces objectifs à une conception AD FS spécifique. Pour plus d’informations sur AD FS prédéfinies des objectifs de déploiement, consultez [identifier vos objectifs de déploiement AD FS](Identifying-Your-AD-FS-Deployment-Goals.md).  
  
Utilisez le tableau suivant pour déterminer quelle conception AD FS mappe à la combinaison appropriée d’AD FS, les objectifs de déploiement pour votre organisation. Cette table fait référence uniquement à ces deux conceptions AD FS principales, comme décrit dans ce guide. Toutefois, vous pouvez créer un hybride ou une conception AD FS personnalisée à l’aide de n’importe quelle combinaison des objectifs de déploiement AD FS pour répondre aux besoins de votre organisation.  
  
|Objectif de déploiement AD FS|[Conception SSO web](Web-SSO-Design.md)|[Conception SSO de web fédéré](Federated-Web-SSO-Design.md)|  
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|  
|[Fournir à vos utilisateurs Active Directory un accès à vos applications et services prenant en charge les revendications](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)|Non|Oui, dans le partenaire de compte|  
|[Fournir à vos utilisateurs Active Directory un accès aux applications et services d’autres organisations](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)|Non|Oui, facultatif dans le partenaire de compte|  
|[Fournir aux utilisateurs d’une autre organisation un accès à vos applications et services prenant en charge les revendications](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)|Oui|Oui|  

## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
  

