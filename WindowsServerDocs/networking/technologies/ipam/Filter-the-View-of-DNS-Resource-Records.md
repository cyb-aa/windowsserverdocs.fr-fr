---
title: Filtrer l’affichage des enregistrements de ressources DNS
description: Cette rubrique fait partie du guide de gestion de la gestion des adresses IP (IPAM) dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5b80294a-7325-476b-84eb-69f0d051e8b2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cc3f2b8ec6e7c5149ef6351639fbbf8f0def8be8
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283932"
---
# <a name="filter-the-view-of-dns-resource-records"></a>Filtrer l’affichage des enregistrements de ressources DNS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour filtrer l’affichage des enregistrements de ressources DNS dans la console client IPAM.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
### <a name="to-filter-the-view-of-dns-resource-records"></a>Pour filtrer l’affichage des enregistrements de ressource DNS  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **IPAM**. La console client IPAM s’affiche.  
  
2.  Dans le volet de navigation, dans **surveiller et gérer**, cliquez sur **Zones DNS**.  Le volet de navigation divise en un volet de navigation supérieur et un volet de navigation inférieur.  
  
3.  Dans le volet de navigation inférieur, cliquez sur **recherche directe**. Toutes les zones de recherche de DNS directes gérés par IPAM sont affichés dans les résultats de recherche du volet Affichage.  
  
4.  Cliquez sur la zone dont vous souhaitez afficher et filtrer les enregistrements.  
  
5.  Dans le volet d’informations, cliquez sur **affichage actuel**, puis cliquez sur **enregistrements de ressource**. Les enregistrements de ressources pour la zone figurent dans le volet d’informations.  
  
6.  Dans le volet d’informations, cliquez sur **ajouter des critères**.  
  
    ![Ajouter des critères](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_01.jpg)  
  
7.  Dans la liste déroulante, sélectionnez un critère. Par exemple, si vous souhaitez afficher un type d’enregistrement spécifique, cliquez sur **Type d’enregistrement**.  
  
    ![Sélectionnez un critère](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_02.jpg)  
  
8.  Cliquez sur **Ajouter**.  
  
    ![Ajouter des critères](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_03.jpg)  
  
9. **Type d’enregistrement** est ajouté en tant que paramètre de recherche. Entrez le texte pour le type d’enregistrement que vous souhaitez rechercher. Par exemple, si vous souhaitez afficher uniquement les enregistrements SRV, tapez **SRV**.  
  
    ![Spécifiez le type d’enregistrement que vous recherchez](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_04.jpg)  
  
10. Appuyez sur ENTRÉE. Les enregistrements de ressources DNS sont filtrés en fonction des critères et rechercher une expression que vous avez spécifié.  
  
    ![Exécuter le filtre](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_05.jpg)  
  
## <a name="see-also"></a>Voir aussi  
[Gestion des enregistrements ressource DNS](DNS-Resource-Record-Management.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


