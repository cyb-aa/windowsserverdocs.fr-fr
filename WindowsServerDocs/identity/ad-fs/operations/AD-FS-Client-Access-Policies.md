---
ms.assetid: ''
title: Stratégies de contrôle d’accès client dans AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cc91cd9a446c8ca30471b65374ca99a7bd49d369
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882370"
---
# <a name="controlling-access-to-organizational-data-with-active-directory-federation-services"></a>Contrôler l’accès aux données de l’organisation avec Active Directory Federation Services

Ce document fournit une vue d’ensemble du contrôle d’accès avec AD FS entre en local, les scénarios hybrides et cloud.  

## <a name="ad-fs-and-conditional-access-to-on-premises-resources"></a>AD FS et l’accès conditionnel aux ressources locales 
Depuis l’introduction de Active Directory Federation Services, les stratégies d’autorisation ont été disponibles pour bloquer ou autoriser l’accès des utilisateurs aux ressources en fonction des attributs de la demande et de la ressource.  Comme AD FS a été déplacé à partir d’une version à l’autre, comment ces stratégies sont implémentées a changé.  Pour plus d’informations sur les fonctionnalités de contrôle d’accès par version, consultez :
- [Stratégies de contrôle d’accès dans AD FS dans Windows Server 2016](Access-Control-Policies-in-AD-FS.md)
- [Contrôle d’accès dans AD FS dans Windows Server 2012 R2](Manage-Risk-with-Conditional-Access-Control.md)


## <a name="ad-fs-and-conditional-access-in-a-hybrid-organization"></a>AD FS et l’accès conditionnel dans une organisation hybride  

AD FS fournit le composant de site on de la stratégie d’accès conditionnel dans un scénario hybride. Les règles d’autorisation AD FS en fonction doivent être utilisées pour les ressources non Azure AD, comme sur site applications fédérées directement dans AD FS.  Le composant de cloud est fourni par [accès conditionnel Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access).  Azure AD Connect fournit le plan de contrôle entre les deux.

Par exemple, lorsque vous inscrivez des appareils avec Azure AD pour l’accès conditionnel aux ressources du cloud, l’appareil Azure AD Connect réécrit fonctionnalité rend des informations d’inscription de périphérique disponibles en local pour consommer et d’appliquer des stratégies AD FS.  De cette façon, vous avez une approche cohérente pour les stratégies de contrôle d’accès pour les deux en local et de ressources de cloud.  

![Accès conditionnel](../deployment/media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  


### <a name="the-evolution-of-client-access-policies-for-office-365"></a>L’évolution des stratégies d’accès Client pour Office 365
Beaucoup d'entre vous sont à l’aide de stratégies d’accès client avec AD FS pour limiter l’accès à Office 365 et d’autres services Microsoft Online services en fonction de facteurs tels que l’emplacement du client et le type d’application cliente qui est utilisée.  
- [Stratégies d’accès client dans Windows Server 2012 R2 AD FS](Access-Control-Policies-W2K12.md)
- [Stratégies d’accès client dans AD FS 2.0](Access-Control-Policies-in-AD-FS-2.md)

Voici quelques exemples de ces stratégies :
- Bloquer tout accès extranet client à Office 365
- Bloquer tout accès de clients extranet à Office 365, à l’exception des appareils d’accéder à Exchange Online pour Exchange Active Sync

Est souvent le besoin sous-jacent derrière ces stratégies pour atténuer les risques de fuite de données en veillant à ce seulement les clients autorisés, les applications qui ne pas mettre en cache les données, ou les appareils qui peuvent être désactivées à distance peuvent accéder aux ressources.

Tandis que les stratégies documentées ci-dessus pour AD FS fonctionnent dans les scénarios spécifiques documentées, ils présentent des limitations, car ils dépendent des données client qui ne sont pas toujours disponibles.  Par exemple, l’identité de l’application cliente a uniquement été disponible pour les services Exchange Online en fonction et pas pour les ressources telles que SharePoint Online, où les mêmes données peuvent être accessible via le navigateur ou un client lourd telles que Word ou Excel.  Également les services AD FS n’a pas connaissance de la ressource dans Office 365 accédée, telles que SharePoint Online ou Exchange Online.

Pour résoudre ces limitations et de fournir un moyen plus robuste d’utiliser les stratégies pour gérer l’accès aux données d’entreprise dans Office 365 ou d’autres ressources d’Azure AD en fonction, Microsoft a introduit l’accès conditionnel Azure AD.  Les stratégies d’accès conditionnel d’AD Azure peuvent être configurés pour une ressource spécifique, ou pour les ressources de tout ou partie d’Office 365, SaaS ou des applications personnalisées dans Azure AD.  Ces stratégies de tableau croisé dynamique sur l’approbation de l’appareil, l’emplacement et d’autres facteurs.

Pour en savoir plus sur l’accès conditionnel Azure AD, consultez [l’accès conditionnel dans Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access)

Un changement de clé que l’activation de ces scénarios est [l’authentification moderne](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/), une nouvelle façon d’authentifier les utilisateurs et appareils qui fonctionne de la même façon sur les clients Office, Skype, Outlook et les navigateurs.

## <a name="next-steps"></a>Étapes suivantes
Pour plus d’informations sur le contrôle d’accès dans le cloud et en local, consultez :

- [Accès conditionnel dans Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access)
- [Stratégies de contrôle d’accès dans AD FS 2016](Access-Control-Policies-in-AD-FS.md)
