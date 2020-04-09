---
title: 'Étape 6 : Rétrograder et supprimer le serveur source du nouveau réseau Windows Server Essentials'
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 86244c66-2c5e-488d-adb8-112e1ca3e2e1
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b1b7836e976ef35aec66e206ec8b1de0e2150d62
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852332"
---
# <a name="step-6-demote-and-remove-the-source-server-from-the-new-windows-server-essentials-network"></a>Étape 6 : Rétrograder et supprimer le serveur source du nouveau réseau Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Une fois que vous avez terminé l’installation de Windows Server Essentials et que vous avez terminé la migration, vous devez effectuer les tâches suivantes :  
  
1.  [Supprimer les services de certificats Active Directory](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_ADCS)  
  
2.  [Déconnecter les imprimantes qui sont directement connectées au serveur source](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect)  
  
3.  [Rétrograder le serveur source](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer)  
  
4.  [Supprimer et réaffecter le serveur source](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer)  
  
##  <a name="remove-active-directory-certificate-services"></a><a name="BKMK_ADCS"></a>Supprimer les services de certificats Active Directory  
 La procédure est légèrement différente si vous avez plusieurs services de rôle AD CS (Active Directory Certificate Services) installés sur un même serveur. Vous pouvez utiliser la procédure suivante pour désinstaller un service de rôle AD CS (Active Directory Certificate Services) et conserver les autres services de rôle AD CS.  
  
 Pour effectuer cette procédure, vous devez ouvrir une session avec les mêmes autorisations que celles de l'utilisateur qui a installé l'autorité de certification (CA). Si vous désinstallez une autorité de certification d'entreprise, l'appartenance au groupe Administrateurs de l'entreprise ou son équivalent est le minimum requis pour effectuer cette procédure.  
  
#### <a name="to-remove-ad-cs"></a>Pour supprimer des services AD CS  
  
1.  Connectez-vous au serveur source en tant qu'administrateur de domaine.  
  
2.  Cliquez sur **Démarrer**, sur **Outils d’administration**, puis sur **Gestionnaire de serveur**.  
  
3.  Cliquez sur **Continuer** dans la boîte de dialogue **Contrôle de compte d'utilisateur**.  
  
4.  Dans la section **Résumé des rôles**, cliquez sur **Supprimer les rôles**.  
  
5.  Dans l'Assistant Suppression de rôles, cliquez sur **Suivant**.  
  
6.  Décochez la case **Services de certificats Active Directory**, puis cliquez sur **Suivant**.  
  
7.  Dans la page **Confirmer les options de suppression**, passez en revue les informations, puis cliquez sur **Supprimer**.  
  
    > [!NOTE]
    >  Si Internet Information Services (IIS) est en cours d'exécution, vous êtes invité à arrêter le service avant de continuer. Cliquez sur **OK**.  
  
    > [!NOTE]
    >  Vous devez d'abord supprimer **Inscription de l'autorité de certification via le web**, si elle est installée.  
  
8.  Quand l'Assistant Suppression de rôles a terminé, redémarrez le serveur pour terminer le processus de désinstallation.  
  
    > [!IMPORTANT]
    >  Redémarrez le serveur, même si vous n'êtes pas invité à le faire.  
  
##  <a name="disconnect-printers-that-are-directly-connected-to-the-source-server"></a><a name="BKMK_PhysicallyDisconnect"></a>Déconnecter les imprimantes qui sont directement connectées au serveur source  
 Avant de rétrograder le serveur source, déconnectez physiquement toutes les imprimantes qui sont connectées directement au serveur source et qui sont partagées via ce serveur source. Vérifiez qu'il ne reste aucun objet Active Directory pour les imprimantes qui étaient connectées directement au serveur source. Les imprimantes peuvent ensuite être connectées directement au serveur de destination et partagées à partir de Windows Server Essentials.  
  
##  <a name="demote-the-source-server"></a><a name="BKMK_DemoteTheSourceServer"></a>Rétrograder le serveur source  
 Avant de rétrograder le serveur source du rôle de contrôleur de domaine AD DS au rôle de serveur membre de domaine, vérifiez que les paramètres de stratégie de groupe sont appliqués à tous les ordinateurs clients, comme décrit dans la procédure suivante.  
  
> [!IMPORTANT]
>  Le serveur source et le serveur de destination doivent être connectés au réseau pendant que les modifications de la stratégie de groupe sont mises à jour sur les ordinateurs clients.  
  
#### <a name="to-force-a-group-policy-update-on-a-client-computer"></a>Pour forcer une mise à jour de la stratégie de groupe sur un ordinateur client  
  
1. Connectez-vous à l'ordinateur client en tant qu'administrateur.  
  
2. Ouvrez une fenêtre d'invite de commandes en tant qu'administrateur.  
  
3. À l'invite de commandes, tapez **gpupdate/force**, puis appuyez sur ENTRÉE.  
  
4. Le processus est susceptible de vous demander de fermer la session, puis de la rouvrir à nouveau pour terminer. Cliquez sur **Oui** pour confirmer.  
  
   Si vous effectuez une migration à partir de Windows Server Essentials ou de ses versions antérieures, pour rétrograder le serveur, consultez [supprimer des Active Directory Domain Services](https://technet.microsoft.com/library/hh472163.aspx). Une fois le serveur source ajouté en tant que membre d'un groupe de travail et déconnecté du réseau, vous devez le supprimer des services AD DS sur le serveur de destination.  
  
   Si vous effectuez une migration à partir de Windows Server Essentials, utilisez Gestionnaire de serveur pour supprimer le rôle Active Directory Domain Services, ce qui rétrograde le contrôleur de domaine sur le serveur source à l’aide de la procédure suivante :  
  
#### <a name="to-remove-the-source-server-from-active-directory"></a>Pour supprimer le serveur source d'Active Directory  
  
1.  Sur le serveur de destination, ouvrez **Utilisateurs et ordinateurs Active Directory**.  
  
2.  Dans le volet de navigation **Utilisateurs et ordinateurs Active Directory**, développez le nom du domaine, puis développez **Ordinateurs**.  
  
3.  Si le serveur source existe toujours dans la liste des serveurs, cliquez avec le bouton droit sur le nom du serveur source, cliquez sur **supprimer**, puis sur **Oui**.  
  
4.  Vérifiez que le serveur source n'est pas répertorié, puis fermez **Utilisateurs et ordinateurs Active Directory**.  
  
##  <a name="remove-and-repurpose-the-source-server"></a><a name="BKMK_RemoveTheSourceServer"></a>Supprimer et réaffecter le serveur source  
 Désactivez le serveur source et déconnectez-le du réseau. Nous vous recommandons de ne pas reformater le serveur source avant au moins une semaine pour vous assurer que toutes les données nécessaires ont été migrées vers le serveur de destination. Après avoir vérifié que toutes les données ont été migrées, vous pouvez réinstaller si nécessaire ce serveur sur le réseau comme serveur secondaire pour d'autres tâches.  
  
> [!NOTE]
>  Une fois le serveur source rétrogradé et supprimé, redémarrez le serveur de destination.  
  
 Une fois le serveur source rétrogradé, son état n'est pas intègre. Si vous voulez réaffecter le serveur source, le moyen le plus simple est de le reformater, d'installer un système d'exploitation serveur, puis de le configurer pour l'utiliser comme serveur supplémentaire.  
  
## <a name="next-steps"></a>Étapes suivantes :  
 Vous avez rétrogradé et supprimé le serveur source du nouveau réseau Windows Server Essentials. Passez maintenant à [l’étape 7 : effectuer des tâches de postconnexion pour la migration de Windows Server Essentials](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md).  
  

Pour afficher toutes les étapes, consultez [migrer vers Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

