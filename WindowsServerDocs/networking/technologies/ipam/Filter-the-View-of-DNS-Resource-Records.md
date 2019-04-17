---
title: Filtrer l’affichage des enregistrements de ressource DNS
description: Cette rubrique fait partie du guide de gestion de la gestion des adresses IP (IPAM) dans Windows Server2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5b80294a-7325-476b-84eb-69f0d051e8b2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 35c0e822daa9f2c8c49ae7e6f2f40ec0411cb6fa
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="filter-the-view-of-dns-resource-records"></a>Filtrer l’affichage des enregistrements de ressource DNS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour filtrer l’affichage des enregistrements de ressource DNS dans la console client IPAM.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.  
  
### <a name="to-filter-the-view-of-dns-resource-records"></a>Pour filtrer l’affichage des enregistrements de ressource DNS  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **IPAM**. La console client IPAM s’affiche.  
  
2.  Dans le volet de navigation dans **surveiller et gérer**, cliquez sur **Zones DNS**.  Le volet de navigation se divise en un volet de navigation supérieur et un volet de navigation inférieur.  
  
3.  Dans le volet de navigation inférieur, cliquez sur **recherche directe**. Toutes les zones de recherche de DNS directe gérés par IPAM sont affichés dans les résultats de recherche du volet Affichage.  
  
4.  Cliquez sur la zone dont vous souhaitez afficher et filtrer les enregistrements.  
  
5.  Dans le volet d’informations, cliquez sur **affichage actuel**, puis cliquez sur **enregistrements de ressource**. Les enregistrements de ressources pour la zone sont affichés dans le volet d’informations.  
  
6.  Dans le volet d’informations, cliquez sur **ajouter des critères**.  
  
    ![Ajouter des critères](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_01.jpg)  
  
7.  Dans la liste déroulante, sélectionnez un critère. Par exemple, si vous souhaitez afficher un type d’enregistrement spécifique, cliquez sur **Type d’enregistrement**.  
  
    ![Sélectionnez un critère](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_02.jpg)  
  
8.  Cliquez sur **ajouter**.  
  
    ![Ajouter des critères](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_03.jpg)  
  
9. **Type d’enregistrement** est ajouté en tant que paramètre de recherche. Entrez le texte pour le type d’enregistrement que vous souhaitez rechercher. Par exemple, si vous souhaitez n’afficher que les enregistrements SRV, tapez **SRV**.  
  
    ![Spécifiez le type d’enregistrement que vous souhaitez rechercher](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_04.jpg)  
  
10. Appuyez sur ENTRÉE. Les enregistrements de ressource DNS sont filtrés en fonction des critères et rechercher une expression que vous avez spécifié.  
  
    ![Le filtre d’exécution](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_05.jpg)  
  
## <a name="see-also"></a>Voir aussi  
[Gestion d’enregistrement de ressource DNS](DNS-Resource-Record-Management.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


