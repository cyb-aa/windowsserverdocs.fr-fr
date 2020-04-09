---
title: Récupération de la forêt Active Directory-restauration ne faisant pas autorité
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: e4ce1d18-d346-492a-8bca-f85513aa3ac1
ms.technology: identity-adds
ms.openlocfilehash: 4fe4905bba944c86d168eaa46ae699ad25ba0194
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823952"
---
# <a name="performing-a-nonauthoritative-restore-of-active-directory-domain-services"></a>Exécution d’une restauration ne faisant pas autorité de Active Directory Domain Services 

>S’applique à : Windows Server 2016, Windows Server 2012 et 2012 R2, Windows Server 2008 et 2008 R2

Pour effectuer une restauration ne faisant pas autorité, effectuez la procédure suivante.  
  
Les procédures suivantes utilisent le fichier Wbadmin. exe pour effectuer une restauration ne faisant pas autorité de Active Directory ou Active Directory Domain Services (AD DS). Si vous utilisez une autre solution de sauvegarde ou si vous envisagez de terminer la restauration faisant autorité de SYSVOL plus tard dans le processus de récupération de forêt, vous pouvez effectuer une restauration faisant autorité de SYSVOL à l’aide des méthodes alternatives suivantes :  
  
- Si vous utilisez le service de réplication de fichiers (FRS) pour répliquer SYSVOL, suivez les étapes de l' [article 290762](https://go.microsoft.com/fwlink/?LinkId=148443) de la base de connaissances Microsoft, à l’aide de la clé de Registre **BurFlags** pour réinitialiser les jeux de réplicas FRS ou, si nécessaire, de l’article 315457 [315457](https://support.microsoft.com/kb/315457)pour reconstruire l’arborescence SYSVOL. Pour déterminer si SYSVOL est répliqué par FRS, consultez [déterminer si le dossier Sysvol d’un contrôleur de domaine est répliqué par DFSR ou FRS](https://msdn.microsoft.com/library/windows/desktop/cc507518.aspx#determining_whether_a_domain_controller_s_sysvol_folder_is_replicated_by_dfsr_or_frs).  
- Si vous utilisez la réplication système de fichiers DFS (DFS) pour répliquer SYSVOL, consultez [effectuer une synchronisation faisant autorité d’un SYSVOL répliqué par DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md).  

## <a name="performing-a-nonauthoritative-restore"></a>Exécution d’une restauration ne faisant pas autorité

Utilisez la procédure suivante pour effectuer une restauration ne faisant pas autorité de AD DS et une restauration faisant autorité de SYSVOL en même temps à l’aide de Wbadmin. exe sur un contrôleur de périphérique qui exécute Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008. La sauvegarde doit inclure explicitement les données d’État du système ; une sauvegarde complète du serveur qui est utilisée pour la récupération complète du serveur ne fonctionnera pas. Pour plus d’informations sur la création d’une sauvegarde de l’état du système, consultez [sauvegarde des données sur l’état du système](AD-Forest-Recovery-Backing-up-System-State.md).  
  
### <a name="to-perform-a-nonauthoritative-restore-of-ad-ds-and-authoritative-restore-of-sysvol-using-wbadminexe"></a>Pour effectuer une restauration ne faisant pas autorité de AD DS et de la restauration faisant autorité de SYSVOL à l’aide de Wbadmin. exe  
  
- Incluez le commutateur **-authsysvol** dans votre commande de récupération, comme illustré dans l’exemple suivant :  

   ```  
   wbadmin start systemstaterecovery <otheroptions> -authsysvol  
   ```  

   Par exemple :  

   ```  
   wbadmin start systemstaterecovery -version:11/20/2012-13:00 -authsysvol  
   ```  
  
![Restaurer](media/AD-Forest-Recovery-Nonauthoritative-Restore/nonauth.png)

## <a name="next-steps"></a>Étapes suivantes

- [Guide de récupération de la forêt Active Directory](AD-Forest-Recovery-Guide.md)
- [Récupération de la forêt Active Directory : procédures](AD-Forest-Recovery-Procedures.md)
