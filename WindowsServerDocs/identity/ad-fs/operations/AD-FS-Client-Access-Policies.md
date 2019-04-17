---
ms.assetid: 
title: "Stratégies de contrôle d’accès client dans ADFS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5e1e1b907e6fccbf2b9906106d3360bd9a6fd69d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="controlling-access-to-organizational-data-with-active-directory-federation-services"></a>Contrôler l’accès aux données de l’organisation avec ActiveDirectory Federation Services

Ce document fournit une vue d’ensemble du contrôle d’accès avec ADFS sur local, les scénarios hybrides et cloud.  

## <a name="ad-fs-and-conditional-access-to-on-premises-resources"></a>ADFS et l’accès conditionnel aux ressources locales 
Depuis l’introduction des Services de fédération ActiveDirectory, les stratégies d’autorisation ont été disponibles pour restreindre ou permettre aux utilisateurs d’accéder aux ressources selon les attributs de la demande et de la ressource.  Comme ADFS a été déplacé à partir d’une version à l’autre, comment ces stratégies sont implémentés a changé.  Pour plus d’informations sur les fonctionnalités de contrôle d’accès par version, voir:
- [Stratégies de contrôle d’accès dans ADFS dans Windows Server2016](Access-Control-Policies-in-AD-FS.md)
- [Contrôle d’accès dans ADFS dans Windows Server2012R2](Manage-Risk-with-Conditional-Access-Control.md)


## <a name="ad-fs-and-conditional-access-in-a-hybrid-organization"></a>ADFS et l’accès conditionnel dans une organisation hybride  

ADFS fournit le composant locaux sur de la stratégie d’accès conditionnel dans un scénario hybride. Les règles d’autorisation ADFS en fonction doivent être utilisées pour les ressources non Azure AD, comme sur local applications fédérées directement à ADFS.  Le composant cloud est fourni par [accès Azure AD conditionnel](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access).  Azure AD Connect fournit le plan de contrôle se connecter les deux.

Par exemple, lorsque vous inscrivez des périphériques avec Azure AD pour l’accès conditionnel aux ressources cloud, l’appareil Azure AD Connect écriture différée fonctionnalité rend informations d’inscription de périphérique disponibles localement pour les stratégies d’ADFS d’utiliser et de mettre en œuvre.  De cette façon, vous avez une approche cohérente pour les stratégies de contrôle d’accès pour les deux sites et de ressources de cloud.  

![Accès conditionnel](../deployment/media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  


### <a name="the-evolution-of-client-access-policies-for-office-365"></a>L’évolution des stratégies d’accès Client pour Office 365
Bon nombre d'entre vous êtes à l’aide de stratégies d’accès client avec les services ADFS pour limiter l’accès à Office 365 et d’autres services MicrosoftOnline en fonction de facteurs tels que l’emplacement du client et le type d’application client en cours d’utilisation.  
- [Stratégies d’accès client dans Windows Server2012R2 ADFS](Access-Control-Policies-W2K12.md)
- [Stratégies d’accès client dans ADFS 2.0](Access-Control-Policies-in-AD-FS-2.md)

Voici quelques exemples de ces stratégies:
- Bloquer l’accès à tous les clients extranet à Office 365
- Bloquer tout accès de clients extranet à Office 365, sauf pour les appareils de l’accès à Exchange Online pour Exchange Active Sync

Souvent la sous-jacent derrière ces stratégies consiste à réduire le risque de fuite de données en veillant à ce seulement les clients autorisés, les applications qui ne pas mettre en cache les données, ou les périphériques qui peuvent être désactivées à distance peuvent accéder aux ressources.

Bien que les stratégies documentées ci-dessus pour ADFS fonctionnent dans les scénarios spécifiques documentées, ils ont des limitations car elles dépendent des données de client qui ne sont pas toujours disponibles.  Par exemple, l’identité de l’application cliente ne sont disponible pour les services Exchange Online basé et pas pour les ressources telles que SharePoint Online, où les mêmes données peuvent être accessible via le navigateur ou un client épais telles que Word ou Excel.  ADFS est également sans se préoccuper de la ressource dans Office 365 accessible, par exemple, SharePoint Online ou Exchange Online.

Pour résoudre ces limitations et de fournir un moyen plus robuste pour utiliser des stratégies pour gérer l’accès aux données d’entreprise dans Office 365 ou d’autres ressources en fonction de Azure AD, Microsoft a introduit l’accès conditionnel de Azure AD.  Les stratégies d’accès conditionnel AD Azure peuvent être configurés pour une ressource spécifique, ou pour tout ou partie des ressources au sein d’Office 365, SaaS ou des applications personnalisées dans Azure AD.  Ces stratégies de sélecteur de vue sur l’approbation de l’appareil, d’emplacement et d’autres facteurs.

Pour en savoir plus sur l’accès conditionnel de Azure ActiveDirectory, voir [accès conditionnel dans Azure ActiveDirectory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access)

Un changement de clé l’activation de ces scénarios est [l’authentification moderne](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/), une nouvelle façon de s’authentifier auprès des utilisateurs et les périphériques qui fonctionne de la même manière sur les clients Office, Skype, Outlook et navigateurs.

## <a name="next-steps"></a>Étapes suivantes
Pour plus d’informations sur le contrôle d’accès sur le cloud et locaux, voir:

- [Accès conditionnel dans Azure ActiveDirectory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access)
- [Stratégies de contrôle d’accès dans ADFS2016](Access-Control-Policies-in-AD-FS.md)
