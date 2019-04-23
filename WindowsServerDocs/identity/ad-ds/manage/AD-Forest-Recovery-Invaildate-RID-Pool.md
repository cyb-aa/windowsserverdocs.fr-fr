---
title: Récupération de forêt AD - invalidant le Pool RID
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 2f5f84df-bd85-4ca4-bdd3-835bd1d45c11
ms.technology: identity-adds
ms.openlocfilehash: 46115991e48da301a8a739009bac27415ebe73df
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842510"
---
# <a name="ad-forest-recovery---invalidating-the-current-rid-pool"></a>Récupération de forêt AD - invalidant le pool RID actuel  

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Utilisez la procédure suivante pour nous Windows PowerShell à invalider le pool RID actuel sur un contrôleur de domaine. Windows PowerShell est activée par défaut sur Windows Server 2012 et Windows Server 2008 R2, mais pas Windows Server 2008 dans lequel il doit être installé à l’aide de **ajouter des fonctionnalités**. Il peut être [téléchargé](https://www.microsoft.com/download/details.aspx?id=20020) pour s’exécuter sur Windows Server 2003.  

Pour vérifier que la commande terminée, recherchez les ID d’événement 16654 (la source est Directory-Services-SAM) dans le journal système dans l’Observateur d’événements dans Windows Server 2012. Les versions antérieures de Windows ne consigne pas cet événement.  
  
> [!NOTE]
> Une fois que vous invalidez le pool RID, vous recevrez une erreur lorsque vous tentez tout d’abord créer le principal de sécurité (utilisateur, ordinateur ou groupe). La tentative de création d’un objet déclenche une demande d’un nouveau pool RID. Nouvelle tentative de l’opération réussit, car le nouveau pool RID sera alloué.  
  
## <a name="to-invalidate-the-current-rid-pool"></a>Pour invalider le pool RID actuel  
  
- Ouvrez une session Windows PowerShell avec élévation de privilèges, exécutez la commande suivante et appuyez sur ENTRÉE :  

   ```powershell
   $Domain = New-Object System.DirectoryServices.DirectoryEntry  
   $DomainSid = $Domain.objectSid  
   $RootDSE = New-Object System.DirectoryServices.DirectoryEntry("LDAP://RootDSE")  
   $RootDSE.UsePropertyCache = $false  
   $RootDSE.Put("invalidateRidPool", $DomainSid.Value)  
   $RootDSE.SetInfo()  
   ```  

## <a name="next-steps"></a>Étapes suivantes

- [Guide de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de forêt AD - procédures](AD-Forest-Recovery-Procedures.md)
