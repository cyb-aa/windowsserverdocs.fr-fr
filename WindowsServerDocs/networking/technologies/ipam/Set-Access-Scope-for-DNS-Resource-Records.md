---
title: Définir l’étendue d’accès pour les enregistrements de ressource DNS
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
ms.assetid: a96a8752-5678-49c5-b069-d2cce8042a51
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 06c33a633975497e80863cc8d42b14a0f9ac8193
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="set-access-scope-for-dns-resource-records"></a>Définir l’étendue d’accès pour les enregistrements de ressource DNS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour définir l’étendue d’accès pour un enregistrements de ressource DNS à l’aide de la console client IPAM.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.  
  
### <a name="to-set-access-scope-for-dns-resource-records"></a>Pour définir l’étendue d’accès pour les enregistrements de ressource DNS  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **IPAM**. La console client IPAM s’affiche.  
  
2.  Dans le volet de navigation, cliquez sur **Zones DNS**.  Dans le volet de navigation inférieur, développez **recherche directe** et recherchez et sélectionnez la zone qui contient les enregistrements de ressources dont vous souhaitez modifier l’étendue d’accès.  
  
3.  Dans le volet d’informations, recherchez et sélectionnez les enregistrements de ressources dont vous souhaitez modifier l’étendue d’accès.  
  
    ![Sélectionnez les enregistrements de ressource](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_02.jpg)  
  
4.  Les enregistrements de ressource DNS sélectionnés d’avec le bouton droit, puis cliquez sur **définir l’étendue d’accès**.  
  
    ![Définir l’étendue d’accès](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_03.jpg)  
  
5.  Le **définir l’étendue d’accès** boîte de dialogue s’ouvre. Si nécessaire pour votre déploiement, cliquez pour désélectionner **hériter l’étendue d’accès du parent**. Dans **sélectionner l’étendue d’accès**, sélectionnez un élément, puis cliquez sur **OK**.  
  
    ![Sélectionnez l’étendue d’accès](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_04.jpg)  
  
## <a name="see-also"></a>Voir aussi  
[Contrôle d’accès basé sur les rôles](Role-based-Access-Control.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


