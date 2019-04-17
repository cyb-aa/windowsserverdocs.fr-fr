---
title: "Récupération de la forêt ActiveDirectory - invalidant le Pool RID"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 2f5f84df-bd85-4ca4-bdd3-835bd1d45c11
ms.technology: identity-adfs
ms.openlocfilehash: cb024356ae5f872e93448d73ea54b271fe3fae4d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---invalidating-the-current-rid-pool"></a>Récupération de la forêt ActiveDirectory - invalidant le pool RID actuel  

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2

 Utilisez la procédure suivante pour nous Windows PowerShell à invalider le pool RID actuel sur un contrôleur de domaine. Windows PowerShell est activée par défaut sur Windows Server2012 et Windows Server2008R2, mais pas Windows Server2008dans lequel il doit être installé à l’aide de **ajouter des fonctionnalités**. Il peut être [téléchargé](https://www.microsoft.com/download/details.aspx?id=20020) pour s’exécuter sur Windows Server2003.  
  
 Pour vérifier la commande terminée, recherchez les ID d’événement 16654 (la source est active-Services-SAM) dans le journal système dans l’Observateur d’événements de Windows Server2012. Les versions antérieures de Windows ne vous connectez pas cet événement.  
  
> [!NOTE]
>  Une fois que vous invalidez le pool RID, vous recevrez une erreur lorsque vous essayez tout d’abord créer l’entité de sécurité (utilisateur, ordinateur ou groupe). La tentative de création d’un objet déclenche une demande d’un nouveau pool RID. Nouvelle tentative de l’opération réussit, car le nouveau pool RID sera alloué.  
  
## <a name="to-invalidate-the-current-rid-pool"></a>À invalider le pool RID actuel  
  
1.  Ouvrez une session Windows PowerShell avec élévation de privilèges, exécutez la commande suivante et appuyez sur ENTRÉE:  
  
    ```  
    $Domain = New-Object System.DirectoryServices.DirectoryEntry  
    $DomainSid = $Domain.objectSid  
    $RootDSE = New-Object System.DirectoryServices.DirectoryEntry("LDAP://RootDSE")  
    $RootDSE.UsePropertyCache = $false  
    $RootDSE.Put("invalidateRidPool", $DomainSid.Value)  
    $RootDSE.SetInfo()  
    ```  
  
## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de forêt ActiveDirectory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt ActiveDirectory - procédures](AD-Forest-Recovery-Procedures.md)
