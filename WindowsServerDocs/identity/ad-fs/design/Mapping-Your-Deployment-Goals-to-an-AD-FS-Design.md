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
ms.openlocfilehash: 048bce75c52895b2d9e215bdccef9cb13dc23533
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866820"
---
# <a name="mapping-your-deployment-goals-to-an-ad-fs-design"></a>Mappage de vos objectifs de déploiement sur une conception AD FS

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Après avoir examiné les Services de fédération Active Directory existant \(AD FS\) objectifs de déploiement et déterminé ceux liés à votre déploiement, vous pouvez mapper ces objectifs à une conception AD FS spécifique. Pour plus d’informations sur AD FS prédéfinies des objectifs de déploiement, consultez [identifier vos objectifs de déploiement AD FS](Identifying-Your-AD-FS-Deployment-Goals.md).  
  
Utilisez le tableau suivant pour déterminer quelle conception AD FS mappe à la combinaison appropriée d’AD FS, les objectifs de déploiement pour votre organisation. Cette table fait référence uniquement à ces deux conceptions AD FS principales, comme décrit dans ce guide. Toutefois, vous pouvez créer un hybride ou une conception AD FS personnalisée à l’aide de n’importe quelle combinaison des objectifs de déploiement AD FS pour répondre aux besoins de votre organisation.  
  
|Objectif de déploiement AD FS|[Conception SSO de Web](Web-SSO-Design.md)|[Conception SSO de Web fédéré](Federated-Web-SSO-Design.md)|  
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|  
|[Fournir l’accès aux utilisateurs de votre Active Directory pour vos Services et Applications prenant en charge les revendications](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)|Non|Oui, dans le partenaire de compte|  
|[Fournir l’accès aux utilisateurs de votre Active Directory pour les Applications et Services d’autres organisations](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)|Non|Oui, facultatif dans le partenaire de compte|  
|[Fournir aux utilisateurs d’une autre organisation un accès à vos Services et Applications prenant en charge les revendications](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)|Oui|Oui|  

## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
  

