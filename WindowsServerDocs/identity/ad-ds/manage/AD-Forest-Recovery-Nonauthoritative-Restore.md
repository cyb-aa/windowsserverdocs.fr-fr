---
title: Récupération de forêt AD - restauration ne faisant pas autorité
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: e4ce1d18-d346-492a-8bca-f85513aa3ac1
ms.technology: identity-adds
ms.openlocfilehash: eae4cab2bd709097fe0efd0745baeb0ec685abc7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829610"
---
# <a name="performing-a-nonauthoritative-restore-of-active-directory-domain-services"></a>Effectuez une restauration ne faisant pas autoritée des Services de domaine Active Directory 

>S'applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Pour effectuer une restauration ne faisant pas autoritée, effectuez la procédure suivante.  
  
Les procédures suivantes utilisent le Wbadmin.exe pour effectuer une restauration ne faisant pas autoritée d’Active Directory ou Services de domaine Active Directory (AD DS). Si vous utilisez une autre solution de sauvegarde ou si vous avez l’intention d’effectuer la restauration faisant autoritée de SYSVOL, plus loin dans le processus de récupération de forêt, vous pouvez effectuer une restauration faisant autorité de SYSVOL à l’aide de ces méthodes :  
  
- Si vous utilisez le Service de réplication de fichiers (FRS) pour répliquer SYSVOL, suivez les étapes de [article 290762](https://go.microsoft.com/fwlink/?LinkId=148443) dans la Base de connaissances Microsoft, à l’aide de la **BurFlags** clé de Registre de réinitialisation de réplicas FRS Définit ou si nécessaire, article 315457 [315457](https://support.microsoft.com/kb/315457)pour reconstruire l’arborescence de SYSVOL. Pour déterminer si SYSVOL répliqué par FRS, consultez [dossier SYSVOL de déterminer si un contrôleur de domaine répliqué par DFSR ou FRS](https://msdn.microsoft.com/en-us/library/windows/desktop/cc507518.aspx#determining_whether_a_domain_controller_s_sysvol_folder_is_replicated_by_dfsr_or_frs).  
- Si vous utilisez la réplication de système de fichiers distribués (DFS, Distributed File System) pour répliquer SYSVOL, consultez [effectuera une synchronisation faisant autoritée de SYSVOL de réplication DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).  

## <a name="performing-a-nonauthoritative-restore"></a>Effectuez une restauration ne faisant pas autoritée

Utilisez la procédure suivante pour effectuer une restauration ne faisant pas autoritée des services AD DS et une restauration faisant autorité de SYSVOL en même temps à l’aide de wbadmin.exe sur un contrôleur de domaine qui exécute Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008. La sauvegarde doit inclure explicitement les données d’état système ; une sauvegarde complète du serveur qui est utilisée pour la récupération de serveur complet ne fonctionneront pas. Pour plus d’informations sur la création d’une sauvegarde de l’état système, consultez [la sauvegarde des données d’état du système](AD-Forest-Recovery-Backing-up-System-State.md).  
  
### <a name="to-perform-a-nonauthoritative-restore-of-ad-ds-and-authoritative-restore-of-sysvol-using-wbadminexe"></a>Pour effectuer une restauration ne faisant pas autoritée des services AD DS faisant autorité et la restauration de SYSVOL à l’aide de wbadmin.exe  
  
- Inclure le **- authsysvol** passer dans votre commande de récupération, comme illustré dans l’exemple suivant :  

   ```  
   wbadmin start systemstaterecovery <otheroptions> -authsysvol  
   ```  

   Exemple :  

   ```  
   wbadmin start systemstaterecovery -version:11/20/2012-13:00 -authsysvol  
   ```  
  
![Restaurer](media/AD-Forest-Recovery-Nonauthoritative-Restore/nonauth.png)

## <a name="next-steps"></a>Étapes suivantes

- [Guide de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de forêt AD - procédures](AD-Forest-Recovery-Procedures.md)
