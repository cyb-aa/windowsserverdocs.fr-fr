---
title: "Gérer des Applications dans WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ae89c46a-0afd-4858-9150-ec97650f45a4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e98d661ac71697bc0e38b6a25fe2f9d2b0b7254f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="manage-applications-in-windows-server-essentials"></a>Gérer des Applications dans WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials
 
 Le serveur du tableau de bord dans WindowsServerEssentials et Windows Server2012R2 avec le rôle expérience WindowsServerEssentials installé permettent d’effectuer des tâches d’administration courantes. Pour effectuer ces tâches, consultez les rubriques suivantes:  
  
-   [Tâches de gestion d’applications dans le tableau de bord](Manage-Applications-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Installer ou supprimer des compléments à l’aide du tableau de bord](Manage-Applications-in-Windows-Server-Essentials.md#BKMK_2)  
  
##  <a name="BKMK_1"></a>Tâches de gestion d’applications dans le tableau de bord  
 Le **Applications** page de gestion du tableau de bord fournit:  
  
-   Liste des compléments installés, qui affiche:  
  
    -   Le nom du service en ligne ou du complément  
  
    -   L’état de mise à jour du complément;  
  
    -   L’état d’abonnement du complément;  
  
    -   Le nom de la société ou d’un éditeur qui est mise à disposition du complément;  
  
-   Un volet des tâches qui inclut un ensemble de tâches permettant de gérer un complément sélectionné  
  
-   Une liste des modules complémentaires qui sont disponibles pour télécharger et installer à partir de MicrosoftPinpoint  
  
 Le tableau suivant décrit les différentes tâches de gestion de complément qui sont disponibles dans le tableau de bord du serveur. Certaines tâches sont spécifiques du complément, afin qu’ils soient visibles uniquement lorsque vous sélectionnez un complément dans la liste.  
  
|Nom de la tâche|Description|  
|---------------|-----------------|  
|Supprimer le complément|Supprime le complément sélectionné à partir du serveur et de tous les autres ordinateurs du réseau.|  
|Installer le complément sur les ordinateurs du réseau|Vous permet de planifier l’installation du complément sélectionné sur tous les autres ordinateurs du réseau.|  
|Obtenir de l’aide avec le complément|Ouvre votre navigateur Internet pour un site Web à partir de laquelle vous pouvez rechercher des solutions aux problèmes et en savoir plus sur un complément sélectionné.|  
|Mise à jour le complément|Vous permet de télécharger et installer les mises à jour pour les compléments installés sur votre serveur et les ordinateurs du réseau.|  
|Renouveler l’abonnement du complément;|Ouvre votre navigateur Internet pour un site Web à partir de laquelle vous pouvez renouveler votre abonnement du complément.|  
|Lire la déclaration de confidentialité pour le complément|Ouvre votre navigateur Internet pour un site Web à partir de laquelle vous pouvez afficher la déclaration de confidentialité.|  
|Comment installer ou supprimer des compléments?|Ouvre votre navigateur Internet et affiche une page web qui affiche la rubrique d’aide sujet.|  
  
##  <a name="BKMK_2"></a>Installer ou supprimer des compléments à l’aide du tableau de bord  
 Un complément est une application logicielle qui fournit les fonctionnalités supplémentaires pour votre serveur. Un nombre croissant de compléments est disponible à partir de Microsoft et d’autres éditeurs de logiciels indépendants (ISV).  
  
 Avant que vous pouvez tirer parti des fonctionnalités étendues qui un complément fournit, vous devez d’abord installer le complément sur le serveur.  
  
#### <a name="to-install-an-add-in-from-microsoft-pinpoint"></a>Pour installer un complément à partir de MicrosoftPinpoint  
  
1.  Dans le tableau de bord du serveur, cliquez sur **Applications**, puis cliquez sur le **MicrosoftPinpoint** onglet.  Une liste des compléments disponibles s’affichent.  
  
2.  Cliquez sur le complément que vous souhaitez installer. La page informations du complément s’affiche.  
  
3.  Sur la page informations du complément, cliquez sur Télécharger et suivez les instructions pour télécharger et installer le complément.  
  
4.  Suivez les instructions de l’Assistant pour installer le complément.  
  
5.  Lorsque l’installation est terminée, redémarrez le tableau de bord, ouvrez le **Applications** page du tableau de bord, serveur et vérifiez que le complément s’affiche dans la liste.  
  
#### <a name="to-install-an-add-in-from-another-provider"></a>Pour installer un complément à partir d’un autre fournisseur  
  
1.  Ouvrez l’Explorateur Windows et accédez à l’emplacement du fichier d’installation du complément.  
  
2.  Double-cliquez sur le fichier pour exécuter l’Assistant installation.  
  
3.  Suivez les instructions de l’Assistant pour installer le complément.  
  
4.  Lorsque l’installation est terminée, redémarrez le tableau de bord, ouvrez le **Applications** page, puis vérifiez que le complément s’affiche dans la liste.  
  
#### <a name="to-remove-an-add-in"></a>Pour supprimer un complément  
  
1.  Ouvrez le tableau de bord du serveur.  
  
2.  Cliquez sur le **Applications** onglet.  
  
3.  Sur le **compléments**, sélectionnez le complément que vous souhaitez supprimer, puis cliquez sur **supprimer le complément**.  
  
4.  Dans le **supprimer complément** fenêtre, cliquez sur **supprimer**.  
  
    > [!NOTE]
    >  Vous devrez peut-être redémarrer le tableau de bord pour supprimer complètement le complément.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Vue d’ensemble du tableau de bord](Overview-of-the-Dashboard-in-Windows-Server-Essentials.md)  
  
-   [Gérer WindowsServerEssentials](Manage-Windows-Server-Essentials.md)
