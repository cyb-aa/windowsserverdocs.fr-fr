---
title: "Récupération de la forêt ActiveDirectory - restauration ne faisant pas autorité"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: e4ce1d18-d346-492a-8bca-f85513aa3ac1
ms.technology: identity-adfs
ms.openlocfilehash: 684672491a0574b12e9b117a8e3c9c1f0936f357
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="performing-a-nonauthoritative-restore-of-active-directory-domain-services"></a>Effectuer une restauration ne faisant pas autoritée des Services de domaine ActiveDirectory 

>S’applique à: Windows Server2016, Windows Server2012 et 2012R2, Windows Server2008 et 2008R2
 
 Pour effectuer une restauration ne faisant pas autoritée, effectuez la procédure suivante.  
  
 Les procédures suivantes utilisent le Wbadmin.exe pour effectuer une restauration ne faisant pas autoritée d’ActiveDirectory ou des Services de domaine ActiveDirectory (ADDS). Si vous utilisez une autre solution de sauvegarde ou si vous prévoyez d’effectuer la restauration faisant autoritée de SYSVOL, plus loin dans le processus de récupération de forêt, vous pouvez effectuer une restauration faisant autorité de SYSVOL à l’aide de ces méthodes:  
  
-   Si vous utilisez le Service de réplication de fichiers (FRS) pour répliquer SYSVOL, suivez les étapes de [article 290762](https://go.microsoft.com/fwlink/?LinkId=148443) dans la Base de connaissances Microsoft, à l’aide de la **BurFlags** clé de Registre réinitialiser des jeux de réplicas FRS, ou si nécessaire, l’article 315457 [315457](https://support.microsoft.com/kb/315457)pour reconstruire l’arborescence de SYSVOL. Pour déterminer si SYSVOL répliqué par FRS, voir [dossier SYSVOL de déterminer si un contrôleur de domaine est répliqué par DFSR ou FRS](https://msdn.microsoft.com/en-us/library/windows/desktop/cc507518.aspx#determining_whether_a_domain_controller_s_sysvol_folder_is_replicated_by_dfsr_or_frs).  
  
-   Si vous utilisez la réplication du système de fichiers distribués (DFS) pour répliquer SYSVOL, consultez [effectuer une synchronisation faisant autoritée de SYSVOL répliqué DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).  
  
 
## <a name="performing-a-nonauthoritative-restore"></a>Effectuer une restauration ne faisant pas autoritée  
 Utilisez la procédure suivante pour effectuer une restauration ne faisant pas autoritée des services ADDS et une restauration faisant autorité de SYSVOL en même temps à l’aide de wbadmin.exe sur un contrôleur de domaine qui exécute Windows Server2012, Windows Server2008R2 ou Windows Server2008. La sauvegarde doit inclure explicitement les données d’état système; une sauvegarde complète du serveur qui est utilisée pour la récupération complète du serveur ne fonctionne pas. Pour plus d’informations sur la création d’une sauvegarde de l’état système, voir [sauvegarde les données d’état système](AD-Forest-Recovery-Backing-up-System-State.md).  
  
### <a name="to-perform-a-nonauthoritative-restore-of-ad-ds-and-authoritative-restore-of-sysvol-using-wbadminexe"></a>Pour effectuer une restauration ne faisant pas autoritée des services ADDS et restauration faisant autorité de SYSVOL à l’aide de wbadmin.exe  
  
-   Inclure les **- authsysvol** basculer dans votre commande de récupération, comme illustré dans l’exemple suivant:  
  
    ```  
    wbadmin start systemstaterecovery <otheroptions> -authsysvol  
    ```  
  
     Par exemple:  
  
    ```  
    wbadmin start systemstaterecovery -version:11/20/2012-13:00 -authsysvol  
    ```  
  
 ![Restauration](media/AD-Forest-Recovery-Nonauthoritative-Restore/nonauth.png)

## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de forêt ActiveDirectory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt ActiveDirectory - procédures](AD-Forest-Recovery-Procedures.md)
