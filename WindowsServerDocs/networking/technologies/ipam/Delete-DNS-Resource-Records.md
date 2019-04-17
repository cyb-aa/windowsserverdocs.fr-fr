---
title: Supprimer les enregistrements de ressource DNS
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
ms.assetid: 366e6fd5-d563-4de3-9551-5614cbb8f2cb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f9ebfeca1da9e36cd00272113f2e86c33174074b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="delete-dns-resource-records"></a>Supprimer les enregistrements de ressource DNS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour supprimer un ou plusieurs enregistrements de ressource DNS à l’aide de la console client IPAM.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.  
  
### <a name="to-delete-dns-resource-records"></a>Pour supprimer les enregistrements de ressource DNS  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **IPAM**. La console client IPAM s’affiche.  
  
2.  Dans le volet de navigation dans **surveiller et gérer**, cliquez sur **Zones DNS**.  Le volet de navigation se divise en un volet de navigation supérieur et un volet de navigation inférieur.  
  
3.  Cliquez pour développer **recherche directe** et le domaine où se trouvent les enregistrements de ressources et de zone que vous souhaitez supprimer. Cliquez sur la zone, puis dans le volet d’informations, cliquez sur **affichage actuel**. Cliquez sur **enregistrements de ressource**.  
  
4.  Dans le volet d’informations, recherchez et sélectionnez les enregistrements de ressources que vous souhaitez supprimer.  
  
    ![Sélectionnez les enregistrements de ressource à supprimer](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_01.jpg)  
  
5.  Cliquez sur les enregistrements sélectionnés, puis cliquez sur **enregistrement de ressource DNS supprimer**.  
  
    ![Supprimer les enregistrements](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_02.jpg)  
  
6.  Le **supprimer un enregistrement de ressource DNS** boîte de dialogue s’ouvre. Vérifiez que le serveur DNS correct est sélectionné. S’il n’est pas le cas, cliquez sur **serveur DNS** et sélectionnez le serveur à partir de laquelle vous souhaitez supprimer les enregistrements de ressource. Cliquez sur **OK**. IPAM supprime les enregistrements de ressources du serveur DNS.  
  
    ![Vérifiez que le serveur DNS correct est sélectionné et supprimer des enregistrements](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_03.jpg)  
  
## <a name="see-also"></a>Voir aussi  
[Gestion d’enregistrement de ressource DNS](DNS-Resource-Record-Management.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


