---
title: Afficher les enregistrements de ressources DNS pour une zone DNS
description: Cette rubrique fait partie du Guide de gestion des adresses IP (IPAM) de Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: article
ms.assetid: 375feefc-949e-47c3-9e61-35b79e021966
ms.author: lizross
author: eross-msft
ms.openlocfilehash: dfd772f89d913cd75b8c31ef4e5236f3cc11263c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854842"
---
# <a name="view-dns-resource-records-for-a-dns-zone"></a>Afficher les enregistrements de ressources DNS pour une zone DNS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour afficher les enregistrements de ressources DNS pour une zone DNS dans la console client IPAM.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
### <a name="to-view-dns-resource-records-for-a-zone"></a>Pour afficher les enregistrements de ressources DNS pour une zone  
  
1.  Dans Gestionnaire de serveur, cliquez sur **IPAM**. La console client IPAM s’affiche.  
  
2.  Dans le volet de navigation, sous **surveiller et gérer**, cliquez sur **zones DNS**.  Le volet de navigation est divisé en un volet de navigation supérieur et un volet de navigation inférieur.  
  
3.  Dans le volet de navigation inférieur, cliquez sur **recherche directe**, puis développez la liste domaine et zone pour rechercher et sélectionner la zone que vous souhaitez afficher. Par exemple, si vous avez une zone nommée Dublin, cliquez sur **Dublin**.  
  
    ![Sélectionnez la zone que vous souhaitez afficher](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01a.jpg)  

  
4.  Dans le volet d’affichage, la vue par défaut est celle des serveurs DNS de la zone. Pour modifier l’affichage, cliquez sur **affichage actuel**, puis sur **enregistrements de ressources**.  
  
    ![Changer l’affichage en enregistrements de ressources](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_Zone_RR_02.jpg)  
  
5.  Les enregistrements de ressources DNS pour la zone s’affichent. Pour filtrer les enregistrements, tapez le texte que vous souhaitez rechercher dans **filtre**.  
  
    ![Taper du texte pour filtrer les enregistrements](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01c.jpg)  
  
6.  Pour filtrer les enregistrements de ressources par type d’enregistrement, étendue d’accès ou d’autres critères, cliquez sur **Ajouter des critères**, puis effectuez des sélections dans la liste critères et cliquez sur **Ajouter**.  
  
    ![Utiliser des critères pour filtrer les enregistrements](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01d.jpg)  
  
## <a name="see-also"></a>Voir aussi  
[Gestion des zones DNS](DNS-Zone-Management.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


