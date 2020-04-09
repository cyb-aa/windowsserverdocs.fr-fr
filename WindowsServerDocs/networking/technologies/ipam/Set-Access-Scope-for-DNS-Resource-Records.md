---
title: Définir l’étendue d’accès pour des enregistrements de ressource DNS
description: Cette rubrique fait partie du Guide de gestion des adresses IP (IPAM) de Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: article
ms.assetid: a96a8752-5678-49c5-b069-d2cce8042a51
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 613796c933498d104db4895733c9a9b1957cb952
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860612"
---
# <a name="set-access-scope-for-dns-resource-records"></a>Définir l’étendue d’accès pour des enregistrements de ressource DNS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour définir l’étendue d’accès d’un enregistrement de ressource DNS à l’aide de la console client IPAM.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
### <a name="to-set-access-scope-for-dns-resource-records"></a>Pour définir l’étendue d’accès pour les enregistrements de ressources DNS  
  
1.  Dans Gestionnaire de serveur, cliquez sur **IPAM**. La console client IPAM s’affiche.  
  
2.  Dans le volet de navigation, cliquez sur **zones DNS**.  Dans le volet de navigation inférieur, développez **recherche directe** et recherchez et sélectionnez la zone qui contient les enregistrements de ressource dont vous souhaitez modifier l’étendue d’accès.  
  
3.  Dans le volet d’informations, recherchez et sélectionnez les enregistrements de ressource dont vous souhaitez modifier l’étendue d’accès.  
  
    ![Sélectionner les enregistrements de ressource](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_02.jpg)  
  
4.  Cliquez avec le bouton droit sur les enregistrements de ressources DNS sélectionnés, puis cliquez sur **définir l’étendue d’accès**.  
  
    ![Définir l’étendue d’accès](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_03.jpg)  
  
5.  La boîte de dialogue **définir l’étendue d’accès** s’ouvre. Si nécessaire pour votre déploiement, cliquez pour désélectionner **hériter l’étendue d’accès du parent**. Dans **Sélectionner l’étendue d’accès**, sélectionnez un élément, puis cliquez sur **OK**.  
  
    ![Sélectionner l’étendue d’accès](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_04.jpg)  
  
## <a name="see-also"></a>Voir aussi  
[Access Control basée sur les rôles](Role-based-Access-Control.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


