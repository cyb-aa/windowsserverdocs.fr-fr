---
title: Afficher les enregistrements de ressource DNS pour une Zone DNS
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
ms.assetid: 375feefc-949e-47c3-9e61-35b79e021966
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 786c1ee8fd673bd17465ab9586dd1e0bcfd7971c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="view-dns-resource-records-for-a-dns-zone"></a>Afficher les enregistrements de ressource DNS pour une Zone DNS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour afficher les enregistrements de ressource DNS pour une zone DNS dans la console client IPAM.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.  
  
### <a name="to-view-dns-resource-records-for-a-zone"></a>Pour afficher les enregistrements de ressource DNS pour une zone  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **IPAM**. La console client IPAM s’affiche.  
  
2.  Dans le volet de navigation dans **surveiller et gérer**, cliquez sur **Zones DNS**.  Le volet de navigation se divise en un volet de navigation supérieur et un volet de navigation inférieur.  
  
3.  Dans le volet de navigation inférieur, cliquez sur **recherche directe**, puis développez la liste de domaine et le fuseau horaire pour rechercher et sélectionner la zone que vous voulez afficher. Par exemple, si vous disposez d’une zone intitulée dublin, cliquez sur **dublin**.  
  
    ![Sélectionnez la zone que vous voulez afficher](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01a.jpg)  

  
4.  Dans le volet d’informations, l’affichage par défaut est des serveurs DNS de la zone. Pour modifier l’affichage, cliquez sur **affichage actuel**, puis cliquez sur **enregistrements de ressource**.  
  
    ![Modifier l’affichage des enregistrements de ressources](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_Zone_RR_02.jpg)  
  
5.  Les enregistrements de ressource DNS pour la zone sont affichés. Pour filtrer les enregistrements, tapez le texte que vous souhaitez trouver dans **filtre**.  
  
    ![Tapez le texte à filtrer les enregistrements](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01c.jpg)  
  
6.  Pour filtrer les enregistrements de ressources par type d’enregistrement, de l’étendue d’accès ou d’autres critères, cliquez sur **ajouter des critères**et effectuer des sélections à partir de la liste des critères, puis cliquez sur **ajouter**.  
  
    ![Utiliser des critères pour filtrer les enregistrements](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01d.jpg)  
  
## <a name="see-also"></a>Voir aussi  
[Gestion des zones DNS](DNS-Zone-Management.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


