---
title: Gérer SharePoint Online dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 282f3634-6de6-4691-803c-df6c3c16660d
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 92552d3beff58edfffe119d1490fefd646479775
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311080"
---
# <a name="manage-sharepoint-online-in-windows-server-essentials"></a>Gérer SharePoint Online dans Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Vous pouvez gérer vos bibliothèques SharePoint Online et vos sites d’équipe à partir du tableau de bord, sans vous connecter à Office 365, si vous intégrez Office 365 à votre serveur Windows Server Essentials. Vous pouvez utiliser des bibliothèques SharePoint Online et des sites d’équipe avec n’importe quel plan Office 365 Business. [Découvrez comment intégrer Office 365 à votre serveur.](Manage-Office-365-in-Windows-Server-Essentials.md)  
  
 En prime, vos utilisateurs peuvent utiliser l’application my Server 2012 R2 pour accéder aux fichiers de vos bibliothèques SharePoint Online à partir de n’importe quel endroit à l’aide de leur appareil mobile ou Windows Phone. [Où puis-je me procurer l'application Mon Serveur ?](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md)  
  
 Vous n'avez pas encore essayé SharePoint ? [Opérations possibles](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/get-started-with-sharepoint-2013-HA102772778.aspx)  
  
## <a name="where-on-the-dashboard-will-i-manage-my-libraries-and-team-sites"></a>Où gérer mes bibliothèques et sites d'équipe sur le tableau de bord ?  
 Vous allez utiliser le nouvel onglet **SharePoint Online** , qui est ajouté à la zone **stockage** du tableau de bord lorsque vous intégrez Office 365 au serveur, pour gérer les ressources SharePoint Online.  

  
## <a name="what-can-i-manage-from-the-dashboard"></a>Que puis-je gérer à partir du tableau de bord ?  
  
### <a name="manage-your-online-libraries"></a>Gérer vos bibliothèques en ligne  
   
|-|-|  
| Ajouter une bibliothèque | Sous l’onglet **bibliothèques SharePoint** , utilisez **Ajouter une bibliothèque**. Vous pouvez alors effectuer tous les choix habituels :<br /><br /> -Choisissez un site d’équipe et le type de bibliothèque.<br />-Décider s’il faut utiliser le contrôle de version.<br />-Assigner des autorisations d’accès.<br /><br /> **Conseil :** Pour savoir quelles autorisations de site d’équipe votre bibliothèque héritera si vous n’affectez pas d’autorisations, utilisez **afficher les autorisations du site**. |  
| Ouvrir une bibliothèque | Pour utiliser le contenu de la bibliothèque, vous devez l’ouvrir dans Office 365. Sélectionnez simplement la bibliothèque, puis cliquez sur **Ouvrir la bibliothèque**. Ce que vous pouvez faire avec le contenu dépend des informations d’identification que vous utilisez pour vous connecter à SharePoint Online. |  
| Modifier les contrôles de version ou les autorisations d’accès | Vous pouvez utiliser **afficher les propriétés** de la bibliothèque pour afficher ou modifier les contrôles de version ou les autorisations d’accès pour la bibliothèque. |  
| Supprimer une bibliothèque | **Avertissement :** Avant de supprimer une bibliothèque SharePoint Online, veillez à enregistrer tous les fichiers que vous souhaitez conserver dans un autre emplacement. Lorsque vous supprimez une bibliothèque de SharePoint, tout est supprimé définitivement. Il est impossible de récupérer quoi que ce soit.<br /><br /> Une fois que vous avez vérifié que la bibliothèque ne stocke rien dont vous aurez besoin plus tard, sélectionnez la bibliothèque et cliquez sur **Supprimer la bibliothèque**. |  
  
### <a name="manage-your-team-sites"></a>Gérer vos sites d'équipe  
 
|-|-|  
| Gérer les sites d’équipe SharePoint | L’action **gérer les sites d’équipe** vous permet de vous connecter à Office 365 et de gérer vos sites d’équipe SharePoint Online. Ce que vous pouvez faire dans Office 365 est déterminé par le compte en ligne avec lequel vous vous connectez.<br /><br /> Lorsque vous fermez Office 365 et revenez au tableau de bord, cliquez sur **Actualiser** pour afficher les modifications. | Afficher ou modifier les autorisations du site d’équipe | Dans la mesure où une bibliothèque hérite des autorisations de son site d’équipe par défaut, il est utile d’avoir un accès facile au site d’équipe. Pour afficher ou modifier les autorisations pour un site d’équipe, sélectionnez le site d’équipe ou l’une de ses bibliothèques, puis cliquez sur **afficher les autorisations du site**.<br /><br /> **Conseil :** Vous avez besoin d’aide avec les points précis des autorisations du site d’équipe SharePoint ? Voici un lien pour [en savoir plus](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/introduction-control-user-access-with-permissions-HA102771919.aspx?CTT=5&origin=HA102771924) sur les autorisations de site d’équipe.  
  
## <a name="tips"></a>Conseils  
  
-   **Cliquez sur Actualiser pour afficher les dernières modifications apportées au portail Office 365.** Vous devez actualiser l’affichage après avoir ouvert Office 365 pour gérer SharePoint Online. Si vous laissez le tableau de bord ouvert pendant de longues périodes, cliquez sur **Actualiser** pour vous assurer de voir les dernières modifications apportées.  
  
-   **Ce que vous pouvez faire dans SharePoint Online varie selon que vous travaillez sur le tableau de bord ou dans Office 365.** Dans le tableau de bord, les modifications SharePoint Online sont effectuées à l’aide du compte administrateur pour l’intégration d’Office 365. Toutefois, lorsque vous vous connectez à Office 365 à partir du tableau de bord, les autorisations d’accès pour le compte en ligne que vous utilisez déterminent ce que vous pouvez faire.  
  
     Pour connaître le compte administrateur pour l’intégration d’Office 365, ouvrez l’onglet **office 365** dans le tableau de bord.  
  
## <a name="other-things-you-might-want-to-do"></a>Autres actions que vous souhaitez peut-être effectuer  
  
-   [Utiliser une application my Server pour travailler avec vos bibliothèques SharePoint Online à partir de n’importe où](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md)  
  
-   [En savoir plus sur l’attribution d’autorisations pour les sites d’équipe SharePoint](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/introduction-control-user-access-with-permissions-HA102771919.aspx?CTT=5&origin=HA102771924)  
  
-   [Découvrez ce que vous pouvez faire avec les fonctionnalités SharePoint](https://office.microsoft.com/office365-sharepoint-online-enterprise-help/get-started-with-sharepoint-2013-HA102772778.aspx)  
  
-   [Jetez un coup d’œil aux plans Office 365 Business qui sont disponibles](https://office.microsoft.com/business/compare-office-365-for-business-plans-FX102918419.aspx?CR_CC=200061904&WT.srch=1&WT.mc_ID=PS_bing_O365Comm_what-is-office-365-for_Text)  
  
-   [Intégrer Office 365 à votre serveur](Manage-Office-365-in-Windows-Server-Essentials.md)  
  
-   [Gérer d’autres services en ligne Microsoft à partir du tableau de bord](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)
