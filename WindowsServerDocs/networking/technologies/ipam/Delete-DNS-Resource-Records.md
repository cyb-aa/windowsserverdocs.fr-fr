---
title: Supprimer des enregistrements de ressources DNS
description: Cette rubrique fait partie du guide de gestion de la gestion des adresses IP (IPAM) dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 366e6fd5-d563-4de3-9551-5614cbb8f2cb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ecbe5dcc452aa39a9afca7e1c8d5fe70d8d4528d
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283978"
---
# <a name="delete-dns-resource-records"></a>Supprimer des enregistrements de ressources DNS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour supprimer un ou plusieurs enregistrements de ressource DNS à l’aide de la console client IPAM.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
### <a name="to-delete-dns-resource-records"></a>Pour supprimer des enregistrements de ressource DNS  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **IPAM**. La console client IPAM s’affiche.  
  
2.  Dans le volet de navigation, dans **surveiller et gérer**, cliquez sur **Zones DNS**.  Le volet de navigation divise en un volet de navigation supérieur et un volet de navigation inférieur.  
  
3.  Cliquez pour développer **recherche directe** et le domaine où se trouvent les enregistrements de ressources et de zone que vous souhaitez supprimer. Cliquez sur la zone, puis dans le volet d’informations, cliquez sur **affichage actuel**. Cliquez sur **enregistrements de ressource**.  
  
4.  Dans le volet d’informations, recherchez et sélectionnez les enregistrements de ressources que vous souhaitez supprimer.  
  
    ![Sélectionnez les enregistrements de ressource à supprimer](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_01.jpg)  
  
5.  Cliquez sur les enregistrements sélectionnés, puis cliquez sur **enregistrement de ressource DNS supprimer**.  
  
    ![Supprimer les enregistrements](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_02.jpg)  
  
6.  Le **supprimer un enregistrement de ressource DNS** boîte de dialogue s’ouvre. Vérifiez que le serveur DNS correct est sélectionné. Si elle n’est pas le cas, cliquez sur **serveur DNS** et sélectionnez le serveur à partir de laquelle vous souhaitez supprimer les enregistrements de ressources. Cliquez sur **OK**. IPAM supprime les enregistrements de ressources du serveur DNS.  
  
    ![Vérifiez que le serveur DNS correct est sélectionné et supprimer des enregistrements](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_03.jpg)  
  
## <a name="see-also"></a>Voir aussi  
[Gestion des enregistrements ressource DNS](DNS-Resource-Record-Management.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


