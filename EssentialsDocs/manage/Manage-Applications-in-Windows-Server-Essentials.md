---
title: Gérer des applications dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ae89c46a-0afd-4858-9150-ec97650f45a4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 7701cbc11ec691b4d4aadd5668dce6438eb9a076
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311413"
---
# <a name="manage-applications-in-windows-server-essentials"></a>Gérer des applications dans Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Le tableau de bord du serveur dans Windows Server Essentials et Windows Server 2012 R2 avec le rôle expérience Windows Server Essentials installé permet d’effectuer des tâches d’administration courantes. Pour effectuer ces tâches, reportez-vous aux sections suivantes :  
  
-   [Tâches de gestion d’applications dans le tableau de bord](Manage-Applications-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Installer ou supprimer des compléments à l’aide du tableau de bord](Manage-Applications-in-Windows-Server-Essentials.md#BKMK_2)  
  
##  <a name="application-management-tasks-in-the-dashboard"></a><a name="BKMK_1"></a>Tâches de gestion d’applications dans le tableau de bord  
 La page de gestion **Applications** du tableau de bord fournit :  
  
- la liste des compléments installés, qui indique :  
  
  -   le nom du complément ou du service en ligne ;  
  
  -   l'état de mise à jour du complément ;  
  
  -   l'état de l'abonnement du complément ;  
  
  -   le nom de la société ou de l'éditeur qui propose ce complément ;  
  
- un volet des tâches qui inclut un ensemble de tâches permettant de gérer un complément sélectionné ;  
  
- la liste des compléments pouvant être téléchargés et installés à partir de Microsoft Pinpoint.  
  
  Le tableau ci-dessous décrit les différentes tâches de gestion de complément disponibles dans le tableau de bord du serveur. Certaines de ces tâches sont spécifiques au complément et s'affichent uniquement quand vous sélectionnez un complément dans la liste.  
  
|Nom de la tâche|Description|  
|---------------|-----------------|  
|Supprimer le complément|Supprime le complément sélectionné du serveur et de tous les autres ordinateurs du réseau.|  
|Installer le complément dans les ordinateurs du réseau|Vous permet de planifier l'installation du complément sélectionné sur tous les autres ordinateurs du réseau.|  
|Obtenir de l'aide sur le complément|Ouvre votre navigateur Internet et affiche un site web à partir duquel vous pouvez rechercher des solutions à des problèmes et en savoir plus sur un complément sélectionné.|  
|Mettre à jour le complément|Favorise le téléchargement et l'installation de mises à jour des compléments installés sur votre serveur et les ordinateurs du réseau.|  
|Renouveler l'abonnement au complément|Ouvre votre navigateur Internet et affiche un site web à partir duquel vous pouvez renouveler votre abonnement au complément.|  
|Consulter la déclaration de confidentialité du complément|Ouvre votre navigateur Internet et affiche un site web dans lequel vous pouvez consulter la déclaration de confidentialité.|  
|Comment installer ou supprimer des compléments ?|Ouvre votre navigateur Internet et affiche une page web contenant la rubrique d'aide sur ce sujet.|  
  
##  <a name="install-or-remove-add-ins-using-the-dashboard"></a><a name="BKMK_2"></a>Installer ou supprimer des compléments à l’aide du tableau de bord  
 Un complément est une application logicielle qui fournit des fonctions et des fonctionnalités supplémentaires pour votre serveur. Un nombre croissant de compléments sont disponibles auprès de Microsoft et d'autres éditeurs de logiciels indépendants.  
  
 Pour pouvoir tirer parti des fonctionnalités étendues fournies par un complément, vous devez installer au préalable le complément sur le serveur.  
  
#### <a name="to-install-an-add-in-from-microsoft-pinpoint"></a>Pour installer un complément à partir de Microsoft Pinpoint  
  
1.  Dans le tableau de bord du serveur, cliquez sur **applications**, puis sur l’onglet **Microsoft Pinpoint** .  La liste des compléments disponibles s’affiche.  
  
2.  Cliquez sur le complément que vous souhaitez installer. Une page d'informations sur le complément s'affiche.  
  
3.  Dans la page d'informations du complément, cliquez sur Télécharger et suivez les instructions qui s'affichent pour télécharger et installer le complément.  
  
4.  Suivez les instructions de l'Assistant pour installer le complément.  
  
5.  Une fois l'installation terminée, redémarrez le tableau de bord, ouvrez la page **Applications** du tableau de bord du serveur et vérifiez que le complément figure dans la liste.  
  
#### <a name="to-install-an-add-in-from-another-provider"></a>Pour installer un complément à partir d'un autre fournisseur  
  
1.  Ouvrez l'Explorateur Windows et accédez à l'emplacement du fichier d'installation du complément.  
  
2.  Double-cliquez sur le fichier pour exécuter l'Assistant d'installation.  
  
3.  Suivez les instructions de l'Assistant pour installer le complément.  
  
4.  Une fois l'installation terminée, redémarrez le tableau de bord, ouvrez la page **Applications**, puis vérifiez que le complément figure dans la liste.  
  
#### <a name="to-remove-an-add-in"></a>Pour supprimer un complément  
  
1.  Ouvrez le tableau de bord du serveur.  
  
2.  Cliquez sur l'onglet **Applications**.  
  
3.  Sous l'onglet **Compléments**, sélectionnez le complément à supprimer, puis cliquez sur **Supprimer le complément**.  
  
4.  Dans la fenêtre **Supprimer le complément**, cliquez sur **Supprimer**.  
  
    > [!NOTE]
    >  Vous devrez peut-être redémarrer le tableau de bord pour supprimer complètement le complément.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Présentation du tableau de bord](Overview-of-the-Dashboard-in-Windows-Server-Essentials.md)  
  
-   [Gérer Windows Server Essentials](Manage-Windows-Server-Essentials.md)
