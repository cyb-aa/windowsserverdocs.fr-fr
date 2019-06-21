---
title: Afficher les enregistrements de ressources DNS pour une zone DNS
description: Cette rubrique fait partie du guide de gestion de la gestion des adresses IP (IPAM) dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 375feefc-949e-47c3-9e61-35b79e021966
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 44db34199257367e98279ccbcbc2d5041ee9884c
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283804"
---
# <a name="view-dns-resource-records-for-a-dns-zone"></a>Afficher les enregistrements de ressources DNS pour une zone DNS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour afficher les enregistrements de ressource DNS pour une zone DNS dans la console client IPAM.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
### <a name="to-view-dns-resource-records-for-a-zone"></a>Pour afficher les enregistrements de ressource DNS pour une zone  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **IPAM**. La console client IPAM s’affiche.  
  
2.  Dans le volet de navigation, dans **surveiller et gérer**, cliquez sur **Zones DNS**.  Le volet de navigation divise en un volet de navigation supérieur et un volet de navigation inférieur.  
  
3.  Dans le volet de navigation inférieur, cliquez sur **recherche directe**, puis développez la liste de domaine et fuseau horaire pour rechercher et sélectionner la zone que vous souhaitez afficher. Par exemple, si vous avez une zone nommée dublin, cliquez sur **dublin**.  
  
    ![Sélectionnez la zone à afficher](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01a.jpg)  

  
4.  Dans le volet d’informations, la vue par défaut est des serveurs DNS pour la zone. Pour modifier la vue, cliquez sur **affichage actuel**, puis cliquez sur **enregistrements de ressource**.  
  
    ![Modifiez l’affichage des enregistrements de ressource](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_Zone_RR_02.jpg)  
  
5.  Les enregistrements de ressources DNS pour la zone sont affichés. Pour filtrer les enregistrements, tapez le texte à rechercher dans **filtre**.  
  
    ![Tapez le texte à filtrer les enregistrements](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01c.jpg)  
  
6.  Pour filtrer les enregistrements de ressource par type d’enregistrement, l’étendue d’accès ou d’autres critères, cliquez sur **ajouter des critères**et effectuer des sélections parmi la liste des critères, puis cliquez sur **ajouter**.  
  
    ![Utiliser des critères pour filtrer les enregistrements](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01d.jpg)  
  
## <a name="see-also"></a>Voir aussi  
[Gestion des zones DNS](DNS-Zone-Management.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


