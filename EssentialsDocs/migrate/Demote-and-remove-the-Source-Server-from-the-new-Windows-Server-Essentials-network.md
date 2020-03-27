---
title: Rétrograder et supprimer le serveur source du nouveau Network1 Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d9f18b29-8e03-439e-bdf0-1dac5e4f70c5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 160c575386feaab5353c97edc1b00b71d1ad7adf
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319017"
---
# <a name="demote-and-remove-the-source-server-from-the-new-windows-server-essentials-network1"></a>Rétrograder et supprimer le serveur source du nouveau Network1 Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Une fois que vous avez terminé l’installation de Windows Server Essentials et que vous avez terminé les tâches de l’Assistant Migration, vous devez effectuer les tâches suivantes :  
  

1.  [Désinstallez Exchange Server 2003](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_UninstallExchangeServer2003).  
  
2.  [Déconnecter les imprimantes qui sont directement connectées au serveur source](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect).  
  
3.  [Rétrograder le serveur source](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer).  
  
4.  [Déplacez le rôle de serveur DHCP du serveur source vers le routeur](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_MoveTheDHCPRole).  
  
5.  [Supprimer et réaffecter le serveur source](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  

1.  [Désinstallez Exchange Server 2003](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_UninstallExchangeServer2003).  
  
2.  [Déconnecter les imprimantes qui sont directement connectées au serveur source](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect).  
  
3.  [Rétrograder le serveur source](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer).  
  
4.  [Déplacez le rôle de serveur DHCP du serveur source vers le routeur](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_MoveTheDHCPRole).  
  
5.  [Supprimer et réaffecter le serveur source](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  

  
###  <a name="uninstall-exchange-server-2003"></a><a name="BKMK_UninstallExchangeServer2003"></a>Désinstaller Exchange Server 2003  
  
> [!IMPORTANT]
>  Si vous ajoutez des comptes d’utilisateur après avoir déplacé les boîtes aux lettres vers le serveur de destination et avant de désinstaller Exchange Server 2003 du serveur source, les boîtes aux lettres sont ajoutées sur le serveur source. Ceci est normal. Vous devez déplacer les boîtes aux lettres vers le serveur de destination pour tous les comptes d'utilisateurs ajoutés entre-temps. Répétez les instructions dans déplacer les boîtes aux lettres et les paramètres Exchange Server pour la migration vers Windows Server Essentials avant de désinstaller Exchange Server 2003.  
  
 Vous devez désinstaller Exchange Server 2003 du serveur source avant de le rétrograder. Cela supprime toutes les références de Active Directory Domain Services (AD DS) à Exchange Server sur le serveur source. Vous devez disposer de votre support Windows Small Business Server 2003 pour supprimer Exchange Server 2003.  
  
##### <a name="to-uninstall-exchange-server-2003-from-the-source-server"></a>Pour désinstaller Exchange Server 2003 du serveur source  
  
1. Ouvrez une session sur le serveur source en tant qu'administrateur.  
  
2. Cliquez sur **Démarrer**, sur **Panneau de configuration**, puis sur **Ajout/Suppression de programmes**.  
  
3. Dans la liste des programmes, sélectionnez **Windows Small Business Server 2003**, puis cliquez sur **modifier/supprimer**.  
  
4. Dans l'Assistant Installation, cliquez sur **Suivant** jusqu'à ce que la page **Sélection des composants** s'affiche.  
  
5. Dans la page Sélection des composants, développez **Exchange Server**, puis choisissez **Supprimer**.  
  
   > [!NOTE]
   > 
   >  Exchange Server vérifie qu'aucune boîte aux lettres ou dossier public ne se trouve sur le serveur. Si des données s'y trouvent encore, un message d'erreur s'affiche quand vous cliquez sur **Supprimer**. Pour éviter ce problème, assurez-vous que vous avez effectué toutes les procédures de la rubrique [déplacer les paramètres et données de SBS 2003 vers le serveur de destination](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  
   > 
   >  Exchange Server vérifie qu'aucune boîte aux lettres ou dossier public ne se trouve sur le serveur. Si des données s'y trouvent encore, un message d'erreur s'affiche quand vous cliquez sur **Supprimer**. Pour éviter ce problème, assurez-vous que vous avez effectué toutes les procédures de la rubrique [déplacer les paramètres et données de SBS 2003 vers le serveur de destination](../migrate/Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  

  
6. Cliquez sur **Suivant**.  
  
7. Lorsque vous y êtes invité, insérez le CD # 3 de Windows Small Business Server 2003, puis suivez les instructions à l’écran.  
  
###  <a name="disconnect-printers-that-are-directly-connected-to-the-source-server"></a><a name="BKMK_PhysicallyDisconnect"></a>Déconnecter les imprimantes qui sont directement connectées au serveur source  
 Avant de rétrograder le serveur source, déconnectez physiquement toutes les imprimantes qui sont connectées directement au serveur source et qui sont partagées via ce serveur source. Vérifiez qu'il ne reste aucun objet Active Directory pour les imprimantes qui étaient connectées directement au serveur source. Les imprimantes peuvent ensuite être connectées directement au serveur de destination et partagées à partir de Windows Server Essentials.  
  
###  <a name="demote-the-source-server"></a><a name="BKMK_DemoteTheSourceServer"></a>Rétrograder le serveur source  
 Avant de rétrograder le serveur source du rôle de contrôleur de domaine AD DS au rôle de serveur membre de domaine, vérifiez que les paramètres de stratégie de groupe sont appliqués à tous les ordinateurs clients, comme décrit dans la procédure suivante.  
  
> [!IMPORTANT]
>  Le serveur source et le serveur de destination doivent être connectés au réseau pendant que les modifications de la stratégie de groupe sont mises à jour sur les ordinateurs clients.  
  
##### <a name="to-force-a-group-policy-update-on-a-client-computer"></a>Pour forcer une mise à jour de la stratégie de groupe sur un ordinateur client  
  
1.  Ouvrez une session sur l'ordinateur client en tant qu'administrateur.  
  
2.  Ouvrez une fenêtre d'invite de commandes en tant qu'administrateur.  
  
3.  À l'invite de commandes, tapez **gpupdate/force**, puis appuyez sur ENTRÉE.  
  
4.  Le processus est susceptible de vous demander de fermer la session, puis de la rouvrir à nouveau pour terminer. Cliquez sur **Oui** pour confirmer.  
  
##### <a name="to-demote-the-source-server"></a>Pour rétrograder le serveur source  
  
1. Sur le serveur source, cliquez sur **Démarrer**, cliquez sur **Exécuter**, tapez **dcpromo**, puis cliquez sur **OK**.  
  
2. Cliquez deux fois sur **Suivant**.  
  
   > [!NOTE]
   >  Ne sélectionnez pas **Ce serveur est le dernier contrôleur de domaine du domaine**.  
  
3. Tapez un mot de passe pour le nouveau compte d'administrateur sur le serveur, puis cliquez sur **Suivant**.  
  
4. Dans la boîte de dialogue **Résumé** , vous êtes informé que AD DS sera supprimé de l’ordinateur et que le serveur deviendra membre du domaine. Cliquez sur **Suivant**.  
  
5. Cliquez sur **Terminer**. Le serveur source redémarre.  
  
6. Après le redémarrage du serveur source, ajoutez-le comme membre d'un groupe de travail avant de le déconnecter du réseau.  
  
   Une fois le serveur source ajouté en tant que membre d'un groupe de travail et déconnecté du réseau, vous devez le supprimer des services AD DS sur le serveur de destination.  
  
##### <a name="to-remove-the-source-server-from-active-directory"></a>Pour supprimer le serveur source d'Active Directory  
  
1.  Sur le serveur de destination, ouvrez **Utilisateurs et ordinateurs Active Directory**.  
  
2.  Dans le volet de navigation **Utilisateurs et ordinateurs Active Directory**, développez le nom du domaine, puis développez **Ordinateurs**.  
  
3.  Cliquez avec le bouton droit sur le nom du serveur source s'il existe encore dans la liste des serveurs, cliquez sur **Supprimer**, puis cliquez sur **Oui**.  
  
4.  Vérifiez que le serveur source n'est pas répertorié, puis fermez **Utilisateurs et ordinateurs Active Directory**.  
  
###  <a name="move-the-dhcp-server-role-from-the-source-server-to-the-router"></a><a name="BKMK_MoveTheDHCPRole"></a>Déplacer le rôle de serveur DHCP du serveur source vers le routeur  
  
> [!NOTE]
> 
>  Si vous avez déjà effectué cette tâche avant de commencer le processus de migration, passez à la section [Supprimer et réaffecter le serveur Source](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  
> 
>  Si vous avez déjà effectué cette tâche avant de commencer le processus de migration, passez à la section [Supprimer et réaffecter le serveur Source](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  

  
 Si votre serveur source exécute le rôle DHCP, procédez comme suit pour déplacer le rôle DHCP vers le routeur.  
  
##### <a name="to-move-the-dhcp-role-from-the-source-server-to-the-router"></a>Pour déplacer le rôle DHCP du serveur source vers le routeur  
  
1.  Désactivez le service DHCP sur le serveur source en procédant comme suit :  
  
    1.  Sur le serveur source, cliquez sur **Démarrer**, sur **Outils d'administration**, puis sur **Services**.  
  
    2.  Dans la liste des services en cours d'exécution, cliquez avec le bouton droit sur **Windows Server**, puis cliquez sur **Propriétés**.  
  
    3.  Pour **Type de démarrage**, sélectionnez **Désactivé**.  
  
    4.  Arrêtez le service.  
  
2.  Activer le rôle DHCP sur votre routeur  
  
    1.  Suivez les instructions fournies dans la documentation de votre routeur pour activer le rôle DHCP sur le routeur.  
  
    2.  Afin de garantir que les adresses IP émises par le serveur source restent les mêmes, suivez les instructions fournies dans la documentation de votre routeur pour faire en sorte que la plage DHCP sur le routeur soit identique à la plage DHCP sur le serveur source.  
  
    > [!IMPORTANT]
    >  Si vous n'avez pas configuré d'adresse IP statique ou de réservations DHCP sur le routeur du serveur de destination et si la plage DHCP n'est pas la même que sur le serveur source, il est possible que le routeur émette une nouvelle adresse IP pour le serveur de destination. Dans ce cas, réinitialisez les règles de réacheminement de port du routeur afin de mettre en place un réacheminement vers la nouvelle adresse IP du serveur de destination.  
  
###  <a name="remove-and-repurpose-the-source-server"></a><a name="BKMK_RemoveTheSourceServer"></a>Supprimer et réaffecter le serveur source  
 Désactivez le serveur source et déconnectez-le du réseau. Nous vous recommandons de ne pas reformater le serveur source avant au moins une semaine pour vous assurer que toutes les données nécessaires ont été migrées vers le serveur de destination. Après avoir vérifié que toutes les données ont été migrées, vous pouvez réinstaller si nécessaire ce serveur sur le réseau comme serveur secondaire pour d'autres tâches.  
  
> [!NOTE]
>  Une fois le serveur source rétrogradé et supprimé, redémarrez le serveur de destination.  
  
 Une fois le serveur source rétrogradé, son état n'est pas intègre. Si vous voulez réaffecter le serveur source, le moyen le plus simple est de le reformater, d'installer un système d'exploitation serveur, puis de le configurer pour l'utiliser comme serveur supplémentaire.
