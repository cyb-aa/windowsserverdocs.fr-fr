---
title: Stratégies de Access Control client dans AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f9939662c22e9500235bae014b7fb9064afd911b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858112"
---
# <a name="controlling-access-to-organizational-data-with-active-directory-federation-services"></a>Contrôle de l’accès aux données organisationnelles avec Services ADFS

Ce document fournit une vue d’ensemble du contrôle d’accès avec AD FS dans les scénarios locaux, hybrides et dans le Cloud.  

## <a name="ad-fs-and-conditional-access-to-on-premises-resources"></a>AD FS et accès conditionnel aux ressources locales 
Depuis l’introduction de Services ADFS, des stratégies d’autorisation sont disponibles pour restreindre ou autoriser l’accès des utilisateurs aux ressources en fonction des attributs de la demande et de la ressource.  Comme AD FS a été déplacée de la version à la version, la façon dont ces stratégies sont implémentées a changé.  Pour plus d’informations sur les fonctionnalités de contrôle d’accès par version, consultez :
- [Stratégies de Access Control dans AD FS dans Windows Server 2016](Access-Control-Policies-in-AD-FS.md)
- [Contrôle d’accès dans AD FS dans Windows Server 2012 R2](Manage-Risk-with-Conditional-Access-Control.md)


## <a name="ad-fs-and-conditional-access-in-a-hybrid-organization"></a>AD FS et accès conditionnel dans une organisation hybride  

AD FS fournit le composant local de la stratégie d’accès conditionnel dans un scénario hybride. Les règles d’autorisation basées sur AD FS doivent être utilisées pour les ressources non Azure AD, telles que les applications locales fédérées directement à AD FS.  Le composant Cloud est fourni par [Azure ad accès conditionnel](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access).  Azure AD Connect fournit le plan de contrôle reliant les deux.

Par exemple, lorsque vous inscrivez des appareils avec Azure AD pour l’accès conditionnel aux ressources de Cloud, la fonctionnalité de réécriture de l’appareil Azure AD Connect rend les informations d’inscription de l’appareil disponibles localement pour que les stratégies de AD FS utilisent et s’appliquent.  De cette façon, vous disposez d’une approche cohérente pour accéder aux stratégies de contrôle des ressources locales et du Cloud.  

![accès conditionnel](../deployment/media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  


### <a name="the-evolution-of-client-access-policies-for-office-365"></a>L’évolution des stratégies d’accès client pour Office 365
Un grand nombre d’entre vous utilisent des stratégies d’accès client avec AD FS pour limiter l’accès à Office 365 et à d’autres services en ligne Microsoft en fonction de facteurs tels que l’emplacement du client et le type d’application cliente utilisé.  
- [Stratégies d’accès client dans Windows Server 2012 R2 AD FS](Access-Control-Policies-W2K12.md)
- [Stratégies d’accès client dans AD FS 2,0](Access-Control-Policies-in-AD-FS-2.md)

Voici quelques exemples de ces stratégies :
- Bloquer l’accès client extranet à Office 365
- Bloquer tout accès client extranet à Office 365, à l’exception des appareils qui accèdent à Exchange Online pour Exchange Active Sync

Souvent, le besoin sous-jacent de ces stratégies est de réduire le risque de fuite de données en garantissant que seuls les clients autorisés, les applications qui ne cachent pas de données ou les appareils qui peuvent être désactivés à distance peuvent accéder aux ressources.

Tandis que les stratégies documentées ci-dessus pour AD FS fonctionner dans les scénarios spécifiques documentés, elles ont des limites, car elles dépendent de données client qui ne sont pas disponibles de manière cohérente.  Par exemple, l’identité de l’application cliente n’est disponible que pour les services Exchange Online et non pour les ressources telles que SharePoint Online, où les mêmes données sont accessibles via le navigateur ou un « client épais » tel que Word ou Excel.  De plus, AD FS n’a pas connaissance de la ressource dans Office 365 qui fait l’objet d’un accès, comme SharePoint Online ou Exchange Online.

Pour répondre à ces limitations et fournir un moyen plus robuste d’utiliser des stratégies pour gérer l’accès aux données d’entreprise dans Office 365 ou d’autres ressources basées sur Azure AD, Microsoft a introduit Azure AD accès conditionnel.  Azure AD stratégies d’accès conditionnel peuvent être configurées pour une ressource spécifique, ou pour toutes les ressources dans Office 365, SaaS ou les applications personnalisées dans Azure AD.  Ces stratégies pivotent sur l’approbation de l’appareil, l’emplacement et d’autres facteurs.

Pour en savoir plus sur Azure AD l’accès conditionnel, consultez [accès conditionnel dans Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access)

L’une des modifications clés qui activent ces scénarios est [l’authentification moderne](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/), une nouvelle façon d’authentifier les utilisateurs et les appareils qui fonctionnent de la même façon sur les clients Office, Skype, Outlook et les navigateurs.

## <a name="next-steps"></a>Étapes suivantes
Pour plus d’informations sur le contrôle de l’accès dans le Cloud et sur site, consultez :

- [Accès conditionnel dans Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access)
- [Stratégies de Access Control dans AD FS 2016](Access-Control-Policies-in-AD-FS.md)
