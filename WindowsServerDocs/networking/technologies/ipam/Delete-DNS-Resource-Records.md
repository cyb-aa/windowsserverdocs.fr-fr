---
title: Supprimer des enregistrements de ressources DNS
description: Cette rubrique fait partie du Guide de gestion des adresses IP (IPAM) de Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 366e6fd5-d563-4de3-9551-5614cbb8f2cb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fcaaf89989a44cc7a0e84e296ce16083fe021577
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405655"
---
# <a name="delete-dns-resource-records"></a>Supprimer des enregistrements de ressources DNS

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour supprimer un ou plusieurs enregistrements de ressources DNS à l’aide de la console client IPAM.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
### <a name="to-delete-dns-resource-records"></a>Pour supprimer des enregistrements de ressources DNS  
  
1.  Dans Gestionnaire de serveur, cliquez sur **IPAM**. La console client IPAM s’affiche.  
  
2.  Dans le volet de navigation, sous **surveiller et gérer**, cliquez sur **zones DNS**.  Le volet de navigation est divisé en un volet de navigation supérieur et un volet de navigation inférieur.  
  
3.  Cliquez pour développer **recherche directe** et le domaine dans lequel se trouvent les enregistrements de zone et de ressource que vous souhaitez supprimer. Cliquez sur la zone, puis dans le volet d’affichage, cliquez sur **affichage actuel**. Cliquez sur **enregistrements de ressources**.  
  
4.  Dans le volet d’informations, recherchez et sélectionnez les enregistrements de ressource que vous souhaitez supprimer.  
  
    ![Sélectionner les enregistrements de ressource à supprimer](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_01.jpg)  
  
5.  Cliquez avec le bouton droit sur les enregistrements sélectionnés, puis cliquez sur **Supprimer l’enregistrement de ressource DNS**.  
  
    ![Supprimer les enregistrements](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_02.jpg)  
  
6.  La boîte de dialogue **supprimer un enregistrement de ressource DNS** s’ouvre. Vérifiez que le serveur DNS correct est sélectionné. Si ce n’est pas le cas, cliquez sur **serveur DNS** et sélectionnez le serveur à partir duquel vous souhaitez supprimer les enregistrements de ressources. Cliquez sur **OK**. IPAM supprime les enregistrements de ressource du serveur DNS.  
  
    ![Vérifier que le serveur DNS correct est sélectionné et supprimer les enregistrements](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_03.jpg)  
  
## <a name="see-also"></a>Voir aussi  
[Gestion des enregistrements de ressources DNS](DNS-Resource-Record-Management.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


