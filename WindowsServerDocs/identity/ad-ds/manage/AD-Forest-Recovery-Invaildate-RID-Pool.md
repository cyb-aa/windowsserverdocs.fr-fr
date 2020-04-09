---
title: Récupération de la forêt Active Directory-invalidation du pool RID
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 2f5f84df-bd85-4ca4-bdd3-835bd1d45c11
ms.technology: identity-adds
ms.openlocfilehash: 9e693f6f30fb721897eaaac89b3d146c57e0e63f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823922"
---
# <a name="ad-forest-recovery---invalidating-the-current-rid-pool"></a>Récupération de la forêt Active Directory-invalidation du pool RID actuel  

>S’applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Utilisez la procédure suivante pour utiliser Windows PowerShell afin d’invalider le pool RID actuel sur un contrôleur de domaine. Windows PowerShell est activé par défaut sur Windows Server 2012 et Windows Server 2008 R2, mais pas sur Windows Server 2008 sur lequel il doit être installé à l’aide de l’option **Ajouter des fonctionnalités**. Il peut être [téléchargé](https://www.microsoft.com/download/details.aspx?id=20020) pour s’exécuter sur Windows Server 2003.  

Pour vérifier que la commande s’est correctement déroulée, recherchez l’ID d’événement 16654 (la source est Directory-Services-SAM) dans le journal système dans observateur d’événements dans Windows Server 2012. Les versions antérieures de Windows ne consignent pas cet événement.  
  
> [!NOTE]
> Une fois que vous avez invalidé le pool RID, vous recevez une erreur lorsque vous tentez de créer pour la première fois un principal de sécurité (utilisateur, ordinateur ou groupe). La tentative de création d’un objet déclenche une demande de nouveau pool RID. La nouvelle tentative de l’opération a échoué, car le nouveau pool RID sera alloué.  
  
## <a name="to-invalidate-the-current-rid-pool"></a>Pour invalider le pool RID actuel  
  
- Ouvrez une session Windows PowerShell avec élévation de privilèges, exécutez la commande suivante et appuyez sur entrée :  

   ```powershell
   $Domain = New-Object System.DirectoryServices.DirectoryEntry  
   $DomainSid = $Domain.objectSid  
   $RootDSE = New-Object System.DirectoryServices.DirectoryEntry("LDAP://RootDSE")  
   $RootDSE.UsePropertyCache = $false  
   $RootDSE.Put("invalidateRidPool", $DomainSid.Value)  
   $RootDSE.SetInfo()  
   ```  

## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt Active Directory : procédures](AD-Forest-Recovery-Procedures.md)
