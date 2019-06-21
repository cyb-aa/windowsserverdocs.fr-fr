---
title: Créer un rôle d’utilisateur pour le contrôle d’accès
description: Cette rubrique fait partie du guide de gestion de la gestion des adresses IP (IPAM) dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ae6a42db-a104-401b-a8e6-b85c47d30b46
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3798b074a0ca7e20602da7986fe6b54e81da5495
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284097"
---
# <a name="create-a-user-role-for-access-control"></a>Créer un rôle d’utilisateur pour le contrôle d’accès

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour créer un nouveau rôle d’utilisateur de contrôle d’accès dans la console client IPAM.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
> [!NOTE]  
> Après avoir créé un rôle, vous pouvez créer une stratégie d’accès pour affecter le rôle à un utilisateur spécifique ou d’un groupe Active Directory. Pour plus d’informations, consultez [créer une stratégie d’accès](../../technologies/ipam/Create-an-Access-Policy.md).  
  
### <a name="to-create-a-role"></a>Pour créer un rôle  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **IPAM**. La console client IPAM s’affiche.  
  
2.  Dans le volet de navigation, cliquez sur **contrôle d’accès**, dans le volet de navigation inférieur, cliquez sur **rôles**.  
  
    ![Rôles de contrôle d’accès](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_01.jpg)  
  
3.  Avec le bouton droit **rôles**, puis cliquez sur **ajouter un rôle utilisateur**.  
  
    ![Ajouter le rôle d’utilisateur](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_02.jpg)  
  
4.  Le **ajouter ou modifier le rôle** boîte de dialogue s’ouvre. Dans **nom**, tapez un nom pour le rôle qui rend la fonction du rôle à effacer. Par exemple, si vous souhaitez créer un rôle qui permet aux administrateurs de gérer les enregistrements de ressource SRV de DNS, vous pouvez nommer le rôle **IPAMSrv**. Si nécessaire, faites défiler **opérations** pour localiser le type d’opérations que vous souhaitez définir pour le rôle. Pour cet exemple, faites défiler jusqu'à **opérations de gestion des enregistrements de ressource DNS**.  
  
    ![Opérations de gestion des enregistrements de ressource DNS](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_03.jpg)  
  
5.  Développez **opérations de gestion des enregistrements de ressource DNS**, puis recherchez **les opérations d’enregistrement SRV**.  
  
    ![Opérations d’enregistrement SRV](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_04.jpg)  
  
6.  Développez et sélectionnez **les opérations d’enregistrement SRV**, puis cliquez sur **OK**.  
  
    ![Sélectionnez les opérations d’enregistrement SRV](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_05.jpg)  
  
7.  Dans la console client IPAM, cliquez sur le rôle que vous venez de créer. Dans **mode Détails,** les opérations autorisées pour le rôle sont affichées.  
  
    ![Détails du nouveau rôle](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_06.jpg)  
  
## <a name="see-also"></a>Voir aussi  
[Contrôle d’accès en fonction du rôle](Role-based-Access-Control.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


