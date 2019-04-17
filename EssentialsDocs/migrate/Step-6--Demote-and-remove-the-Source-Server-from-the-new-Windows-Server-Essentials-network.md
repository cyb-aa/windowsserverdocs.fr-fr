---
title: "Étape6: Rétrograder et supprimer le serveur Source du nouveau réseau WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 86244c66-2c5e-488d-adb8-112e1ca3e2e1
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 24a1f2da2333c7e6854e9efd9d996391d0fcb3b9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="step-6-demote-and-remove-the-source-server-from-the-new-windows-server-essentials-network"></a>Étape6: Rétrograder et supprimer le serveur Source du nouveau réseau WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Après avoir terminé l’installation de WindowsServerEssentials et terminer la migration, vous devez effectuer les tâches suivantes:  
  
1.  [Supprimer les Services de certificats ActiveDirectory](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_ADCS)  
  
2.  [Déconnecter les imprimantes qui sont directement connectés au serveur Source](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect)  
  
3.  [Rétrograder le serveur Source](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer)  
  
4.  [Supprimer et réaffecter le serveur Source](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer)  
  
##  <a name="BKMK_ADCS"></a>Supprimer les Services de certificats ActiveDirectory  
 La procédure est légèrement différente si vous avez plusieurs services de rôle Services de certificats ActiveDirectory (ADCS) installés sur un seul serveur. Vous pouvez utiliser la procédure suivante pour désinstaller un service de rôle ADCS et conserver les autres services de rôle ADCS.  
  
 Pour effectuer cette procédure, vous devez ouvrir une session avec les mêmes autorisations que l’utilisateur qui a installé l’autorité de certification (CA). Si vous désinstallez une autorité de certification d’entreprise, l’appartenance à des administrateurs de l’entreprise ou son équivalent est le minimum requis pour effectuer cette procédure.  
  
#### <a name="to-remove-ad-cs"></a>Pour supprimer les services ADCS  
  
1.  Ouvrez une session sur le serveur Source en tant qu’administrateur de domaine.  
  
2.  Cliquez sur **Démarrer**, cliquez sur **outils d’administration**, puis cliquez sur **le Gestionnaire de serveur**.  
  
3.  Cliquez sur **continuer** dans les **contrôle de compte d’utilisateur** boîte de dialogue.  
  
4.  Dans le **résumé des rôles**, cliquez sur **supprimer des rôles**.  
  
5.  Dans l’Assistant Suppression de rôles, cliquez sur **suivant**.  
  
6.  Désactivez le **ActiveDirectory Certificate Services** case à cocher, puis cliquez sur **suivant**.  
  
7.  Sur le **confirmer les Options de suppression** page, passez en revue les informations, puis cliquez sur **supprimer**.  
  
    > [!NOTE]
    >  Si Internet Information Services (IIS) est en cours d’exécution, vous êtes invité à arrêter le service avant de continuer. Cliquez sur **OK**.  
  
    > [!NOTE]
    >  Tout d’abord, vous devrez peut-être supprimer **inscription de l’autorité de Certification via le Web**, s’il est installé.  
  
8.  Une fois l’Assistant Suppression de rôles, redémarrez le serveur pour terminer le processus de désinstallation.  
  
    > [!IMPORTANT]
    >  Redémarrez le serveur, même si vous n’êtes pas invité à le faire.  
  
##  <a name="BKMK_PhysicallyDisconnect"></a>Déconnecter les imprimantes qui sont directement connectés au serveur Source  
 Avant de rétrograder le serveur Source, déconnectez physiquement toutes les imprimantes qui sont directement connectés au serveur Source et sont partagées via le serveur Source. Ne garantir qu’aucun objet ActiveDirectory pour les imprimantes qui étaient connectées directement au serveur Source. Les imprimantes peuvent être directement connectés au serveur de Destination et partagés à partir de WindowsServerEssentials.  
  
##  <a name="BKMK_DemoteTheSourceServer"></a>Rétrograder le serveur Source  
 Avant de rétrograder le serveur Source du rôle du contrôleur de domaine ADDS au rôle d’un serveur membre de domaine, vérifiez que les paramètres de stratégie de groupe sont appliqués à tous les ordinateurs clients, comme décrit dans la procédure suivante.  
  
> [!IMPORTANT]
>  Le serveur Source et le serveur de Destination doivent être connectés au réseau alors que les modifications de stratégie de groupe sont mis à jour sur les ordinateurs clients.  
  
#### <a name="to-force-a-group-policy-update-on-a-client-computer"></a>Pour forcer une mise à jour de stratégie de groupe sur un ordinateur client  
  
1.  Connectez-vous à l’ordinateur client en tant qu’administrateur.  
  
2.  Ouvrez une fenêtre d’invite de commandes en tant qu’administrateur.  
  
3.  À l’invite de commandes, tapez **gpupdate /force**, puis appuyez sur ENTRÉE.  
  
4.  Le processus peut nécessiter vous déconnecter et reconnecter pour terminer. Cliquez sur **Oui** pour confirmer.  
  
 Si vous effectuez une migration à partir de WindowsServerEssentials ou des versions précédentes, pour rétrograder le serveur, voir [supprimer des Services de domaine ActiveDirectory](https://technet.microsoft.com/library/hh472163.aspx). Une fois que vous ajoutez le serveur Source en tant que membre d’un groupe de travail et déconnectez du réseau, vous devez le supprimer à partir des services ADDS sur le serveur de Destination.  
  
 Si vous effectuez une migration à partir de WindowsServerEssentials, utilisez le Gestionnaire de serveur pour supprimer le rôle Services de domaine ActiveDirectory, rétrogradant ainsi le contrôleur de domaine sur le serveur Source à l’aide de la procédure suivante:  
  
#### <a name="to-remove-the-source-server-from-active-directory"></a>Pour supprimer le serveur Source à partir d’ActiveDirectory  
  
1.  Sur le serveur de Destination, ouvrez **ActiveDirectory Users and Computers**.  
  
2.  Dans le **ActiveDirectory Users and Computers** volet de navigation, développez le nom de domaine, puis puis **ordinateurs**.  
  
3.  Si le serveur Source existe toujours dans la liste des serveurs, cliquez sur le nom du serveur Source, cliquez sur **supprimer**, puis cliquez sur **Oui**.  
  
4.  Vérifiez que le serveur Source n’est pas répertorié, puis fermez **ActiveDirectory Users and Computers**.  
  
##  <a name="BKMK_RemoveTheSourceServer"></a>Supprimer et réaffecter le serveur Source  
 Désactiver le serveur Source et déconnectez-le du réseau. Nous recommandons que vous ne pas reformater le serveur Source au moins une semaine pour vous assurer que toutes les données nécessaires migrés vers le serveur de Destination. Après avoir vérifié que toutes les données a migré, vous pouvez réinstaller ce serveur sur le réseau comme serveur secondaire pour d’autres tâches, si nécessaire.  
  
> [!NOTE]
>  Après avoir rétrogradé et supprimer le serveur Source, redémarrez le serveur de Destination.  
  
 Après avoir rétrogradé le serveur Source, il n’est pas dans un état sain. Si vous voulez réaffecter le serveur Source, le moyen le plus simple consiste à remettre, installer un système d’exploitation et puis de le configurer pour l’utiliser comme serveur supplémentaire.  
  
## <a name="next-steps"></a>Étapes suivantes  
 Vous avez rétrogradé et supprimé le serveur Source du nouveau réseau WindowsServerEssentials. Passez maintenant à [étape7: effectuer les tâches de post-migration pour la migration de WindowsServerEssentials](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md).  
  

Pour afficher toutes les étapes, voir [migrer vers WindowsServerEssentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

