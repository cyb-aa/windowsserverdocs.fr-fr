---
title: Créer un rôle d’utilisateur pour le contrôle d’accès
description: Cette rubrique fait partie du Guide de gestion des adresses IP (IPAM) de Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: article
ms.assetid: ae6a42db-a104-401b-a8e6-b85c47d30b46
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 90ba50189b0f42f1f581032b7dc8b52b8c3fca4d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814762"
---
# <a name="create-a-user-role-for-access-control"></a>Créer un rôle d’utilisateur pour le contrôle d’accès

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour créer un nouveau rôle d’utilisateur Access Control dans la console client IPAM.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  
  
> [!NOTE]  
> Après avoir créé un rôle, vous pouvez créer une stratégie d’accès pour attribuer le rôle à un utilisateur ou à un groupe de Active Directory spécifique. Pour plus d’informations, consultez [créer une stratégie d’accès](../../technologies/ipam/Create-an-Access-Policy.md).  
  
### <a name="to-create-a-role"></a>Pour créer un rôle  
  
1.  Dans Gestionnaire de serveur, cliquez sur **IPAM**. La console client IPAM s’affiche.  
  
2.  Dans le volet de navigation, cliquez sur **contrôle d’accès**, puis dans le volet de navigation inférieur, cliquez sur **rôles**.  
  
    ![Rôles de contrôle d’accès](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_01.jpg)  
  
3.  Cliquez avec le bouton droit sur **rôles**, puis cliquez sur **Ajouter un rôle d’utilisateur**.  
  
    ![Ajouter un rôle d’utilisateur](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_02.jpg)  
  
4.  La boîte de dialogue **Ajouter ou modifier un rôle** s’ouvre. Dans **nom**, tapez un nom pour le rôle qui rend la fonction de rôle désactivée. Par exemple, si vous souhaitez créer un rôle qui permet aux administrateurs de gérer les enregistrements de ressources SRV DNS, vous pouvez nommer le rôle **IPAMSrv**. Si nécessaire, faites défiler les **opérations** pour rechercher le type d’opérations que vous souhaitez définir pour le rôle. Pour cet exemple, faites défiler jusqu’à **opérations de gestion des enregistrements de ressources DNS**.  
  
    ![Opérations de gestion des enregistrements de ressources DNS](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_03.jpg)  
  
5.  Développez **opérations de gestion des enregistrements de ressources DNS**, puis localisez les **opérations d’enregistrement SRV**.  
  
    ![Opérations d’enregistrement SRV](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_04.jpg)  
  
6.  Développez et sélectionnez **opérations d’enregistrement SRV**, puis cliquez sur **OK**.  
  
    ![Sélectionner les opérations d’enregistrement SRV](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_05.jpg)  
  
7.  Dans la console client IPAM, cliquez sur le rôle que vous venez de créer. En **mode Détails,** les opérations autorisées pour le rôle sont affichées.  
  
    ![Nouveaux détails du rôle](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_06.jpg)  
  
## <a name="see-also"></a>Voir aussi  
[Access Control basée sur les rôles](Role-based-Access-Control.md)  
[Gérer IPAM](Manage-IPAM.md)  
  


