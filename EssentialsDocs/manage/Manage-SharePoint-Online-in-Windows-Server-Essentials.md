---
title: Gérer SharePoint Online dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 282f3634-6de6-4691-803c-df6c3c16660d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b9b12c138e6166684b4b9e87b794444febd3c247
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830380"
---
# <a name="manage-sharepoint-online-in-windows-server-essentials"></a>Gérer SharePoint Online dans Windows Server Essentials

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Vous pouvez gérer vos bibliothèques SharePoint Online et les sites d’équipe à partir du tableau de bord, sans vous connecter à Office 365, si vous intégrez Office 365 à votre serveur Windows Server Essentials. Vous obtenez des bibliothèques SharePoint Online et les sites d’équipe avec un plan d’entreprise Office 365. [Découvrez comment intégrer Office 365 à votre serveur.](Manage-Office-365-in-Windows-Server-Essentials.md)  
  
 En guise de bonus, vos utilisateurs seront en mesure d’utiliser l’application My Server 2012 R2 pour accéder aux fichiers dans vos bibliothèques SharePoint Online à partir de n’importe où à l’aide de leur appareil mobile ou un téléphone de Windows. [Où puis-je me procurer l'application Mon Serveur ?](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md)  
  
 Vous n'avez pas encore essayé SharePoint ? [Opérations possibles](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/get-started-with-sharepoint-2013-HA102772778.aspx)  
  
## <a name="where-on-the-dashboard-will-i-manage-my-libraries-and-team-sites"></a>Où gérer mes bibliothèques et sites d'équipe sur le tableau de bord ?  
 Vous devez utiliser le nouvel **SharePoint Online** onglet, qui est ajouté à la **stockage** zone du tableau de bord lorsque vous intégrez Office 365 avec le serveur, pour gérer les ressources SharePoint Online.  

  
## <a name="what-can-i-manage-from-the-dashboard"></a>Que puis-je gérer à partir du tableau de bord ?  
  
### <a name="manage-your-online-libraries"></a>Gérer vos bibliothèques en ligne  
   
|-|-|  
| Ajouter une bibliothèque | Sur le **bibliothèques SharePoint** onglet, utilisez **ajouter une bibliothèque**. Vous pouvez alors effectuer tous les choix habituels :<br /><br /> -Choisir un site d’équipe et le type de bibliothèque.<br />-Déterminer s’il faut utiliser le contrôle de version.<br />-Attribuer les autorisations d’accès.<br /><br /> **Conseil :** Pour connaître les autorisations de site d’équipe héritera votre bibliothèque si vous n’affectez pas des autorisations, utilisez **afficher les autorisations de site**. |  
| Ouvrez une bibliothèque | Pour travailler avec le contenu de la bibliothèque, vous aurez besoin pour l’ouvrir dans Office 365. Sélectionnez simplement la bibliothèque, puis cliquez sur **Ouvrir la bibliothèque**. Ce que vous pouvez faire avec le contenu varie selon les informations d’identification qui vous permet de vous connecter à SharePoint Online. |  
| Modifier les contrôles de version ou les autorisations d’accès | Vous pouvez utiliser **afficher les propriétés de la bibliothèque** pour afficher ou modifier les contrôles de version ou les autorisations d’accès de la bibliothèque. |  
| Supprimer une bibliothèque | **Avertissement :** Avant de supprimer une bibliothèque SharePoint Online, veillez à enregistrer dans un autre emplacement tous les fichiers que vous souhaitez conserver. Lorsque vous supprimez une bibliothèque à partir de SharePoint, tout est définitivement supprimé. Il est impossible de récupérer quoi que ce soit.<br /><br /> Une fois que vous avez vérifié que la bibliothèque ne stocke rien dont vous aurez besoin plus tard, sélectionnez-la, puis cliquez sur **supprimer la bibliothèque**. |  
  
### <a name="manage-your-team-sites"></a>Gérer vos sites d'équipe  
 
|-|-|  
| Gérer les sites d’équipe SharePoint | Le **gérer les sites d’équipe** action vous permet de vous connecter à Office 365 et de gérer vos sites d’équipe SharePoint Online. Ce que vous pouvez faire dans Office 365 sera déterminé par le compte en ligne que vous vous connectez à l’aide.<br /><br /> Lorsque vous fermez Office 365 et que vous retournez au tableau de bord, cliquez sur **Actualiser** pour afficher les modifications. | Afficher ou modifier les autorisations de site d’équipe | Dans la mesure où une bibliothèque hérite des autorisations de son site d’équipe par défaut, il est utile de disposer d’un accès facile au site d’équipe. Pour afficher ou modifier les autorisations pour un site d’équipe, sélectionnez le site d’équipe ou une de ses bibliothèques, puis cliquez sur **afficher les autorisations de site**.<br /><br /> **Conseil :** Vous avez besoin d'aide sur les détails des autorisations de site d'équipe SharePoint ? Voici un lien pour [en savoir plus](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/introduction-control-user-access-with-permissions-HA102771919.aspx?CTT=5&origin=HA102771924) sur les autorisations de site d’équipe.  
  
## <a name="tips"></a>Conseils  
  
-   **Cliquez sur Actualiser pour afficher les dernières modifications apportées dans le portail Office 365.** Vous devez actualiser l’affichage après avoir ouvert Office 365 pour gérer SharePoint Online. Si vous laissez le tableau de bord ouvert pendant de longues périodes, cliquez sur **Actualiser** pour vous assurer de voir les dernières modifications apportées.  
  
-   **Ce que vous pouvez faire dans SharePoint Online dépend de si vous travaillez sur le tableau de bord ou dans Office 365.** Le tableau de bord SharePoint Online sont apportées à l’aide du compte d’administrateur pour l’intégration d’Office 365. Mais lorsque vous vous connectez à Office 365 à partir du tableau de bord, les autorisations d’accès pour le compte en ligne que vous utilisez déterminent ce que vous pouvez effectuer.  
  
     Pour trouver le compte d’administrateur pour l’intégration d’Office 365, ouvrez le **Office 365** onglet du tableau de bord.  
  
## <a name="other-things-you-might-want-to-do"></a>Autres actions que vous souhaitez peut-être effectuer  
  
-   [Utiliser une application My Server pour travailler avec vos bibliothèques SharePoint Online depuis n’importe où](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md)  
  
-   [En savoir plus sur l’attribution d’autorisations pour les sites d’équipe SharePoint](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/introduction-control-user-access-with-permissions-HA102771919.aspx?CTT=5&origin=HA102771924)  
  
-   [Découvrez ce que vous pouvez faire avec les fonctionnalités de SharePoint](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/get-started-with-sharepoint-2013-HA102772778.aspx)  
  
-   [Examinons les offres commerciales d’Office 365 sont disponibles](https://office.microsoft.com/business/compare-office-365-for-business-plans-FX102918419.aspx?CR_CC=200061904&WT.srch=1&WT.mc_ID=PS_bing_O365Comm_what-is-office-365-for_Text)  
  
-   [Intégrer Office 365 à votre serveur](Manage-Office-365-in-Windows-Server-Essentials.md)  
  
-   [Gérer d’autres services Microsoft online services à partir du tableau de bord](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)
