---
ms.assetid: 68979914-8a1c-465a-bd37-08df30722d69
title: "Mappage de vos objectifs de déploiement à une conception ADFS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 048bce75c52895b2d9e215bdccef9cb13dc23533
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="mapping-your-deployment-goals-to-an-ad-fs-design"></a>Mappage de vos objectifs de déploiement à une conception ADFS

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Après avoir examiné les objectifs de déploiement Active Directory Federation Services \(AD FS\) existantes et vous déterminez quels objectifs sont liés à votre déploiement, vous pouvez mapper ces objectifs à une conception AD FS spécifique. Pour plus d’informations sur les services AD FS prédéfinies des objectifs de déploiement, consultez [identifier vos objectifs de déploiement AD FS](Identifying-Your-AD-FS-Deployment-Goals.md).  
  
Utilisez le tableau suivant pour déterminer quelle conception AD FS correspond à la combinaison appropriée d’AD FS objectifs de déploiement pour votre organisation. Ce tableau fait uniquement référence aux deux conceptions d’AD FS principales, comme décrit dans ce guide. Toutefois, vous pouvez créer un hybride ou une conception AD FS personnalisée à l’aide de n’importe quelle combinaison des objectifs de déploiement AD FS pour répondre aux besoins de votre organisation.  
  
|Objectif de déploiement AD FS|[Conception SSO de Web](Web-SSO-Design.md)|[Conception SSO de Web fédéré](Federated-Web-SSO-Design.md)|  
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|  
|[Fournir vos utilisateurs Active Directory un accès à vos Services et Applications prenant en charge les revendications](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)|N°|Oui, dans le partenaire de compte|  
|[Fournir un accès à vos utilisateurs Active Directory pour les Applications et Services d’autres organisations](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)|N°|Oui, facultatif dans le partenaire de compte|  
|[Fournir aux utilisateurs d’une autre organisation d’accéder à vos Services et Applications prenant en charge les revendications](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)|Oui|Oui|  

## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
  

