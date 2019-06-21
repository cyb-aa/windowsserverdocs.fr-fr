---
title: Définir l’étendue d’accès pour des enregistrements de ressource DNS
description: Cette rubrique fait partie du guide de gestion de la gestion des adresses IP (IPAM) dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a96a8752-5678-49c5-b069-d2cce8042a51
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c79e1f63b9bcb43520a57defca8228b76db68a31
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283871"
---
# <a name="set-access-scope-for-dns-resource-records"></a>Définir l’étendue d’accès pour des enregistrements de ressource DNS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour définir l’étendue d’accès pour les enregistrements de ressource a DNS à l’aide de la console client IPAM.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
### <a name="to-set-access-scope-for-dns-resource-records"></a>Pour définir l’étendue d’accès pour les enregistrements de ressource DNS  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **IPAM**. La console client IPAM s’affiche.  
  
2.  Dans le volet de navigation, cliquez sur **Zones DNS**.  Dans le volet de navigation inférieur, développez **recherche directe** et recherchez et sélectionnez la zone qui contient les enregistrements de ressources dont vous souhaitez modifier l’étendue d’accès.  
  
3.  Dans le volet d’informations, recherchez et sélectionnez les enregistrements de ressources dont vous souhaitez modifier l’étendue d’accès.  
  
    ![Sélectionnez les enregistrements de ressources](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_02.jpg)  
  
4.  Cliquez sur les enregistrements de ressources DNS sélectionnés, puis cliquez sur **définir l’étendue d’accès**.  
  
    ![Définir l’étendue d’accès](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_03.jpg)  
  
5.  Le **définir l’étendue d’accès** boîte de dialogue s’ouvre. Si nécessaire pour votre déploiement, cliquez pour désélectionner **hériter l’étendue d’accès du parent**. Dans **sélectionner l’étendue d’accès**, sélectionnez un élément, puis cliquez sur **OK**.  
  
    ![Sélectionnez l’étendue d’accès](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_04.jpg)  
  
## <a name="see-also"></a>Voir aussi  
[Contrôle d’accès en fonction du rôle](Role-based-Access-Control.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


