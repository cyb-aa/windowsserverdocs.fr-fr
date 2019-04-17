---
title: "Rétrograder et supprimer le serveur Source à partir de la nouvelle network1 de WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d9f18b29-8e03-439e-bdf0-1dac5e4f70c5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 1545189732194ad5c0aba401f834b0102799e016
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="demote-and-remove-the-source-server-from-the-new-windows-server-essentials-network1"></a>Rétrograder et supprimer le serveur Source à partir de la nouvelle network1 de WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Après avoir terminé l’installation de WindowsServerEssentials et que vous effectuez les tâches de l’Assistant Migration, vous devez effectuer les tâches suivantes:  
  

1.  [Désinstaller Exchange Server2003](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_UninstallExchangeServer2003).  
  
2.  [Déconnecter les imprimantes qui sont directement connectés au serveur Source](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect).  
  
3.  [Rétrograder le serveur Source](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer).  
  
4.  [Déplacer le rôle de serveur DHCP à partir du serveur Source vers le routeur](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_MoveTheDHCPRole).  
  
5.  [Supprimer et réaffecter le serveur Source](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  

1.  [Désinstaller Exchange Server2003](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_UninstallExchangeServer2003).  
  
2.  [Déconnecter les imprimantes qui sont directement connectés au serveur Source](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect).  
  
3.  [Rétrograder le serveur Source](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer).  
  
4.  [Déplacer le rôle de serveur DHCP à partir du serveur Source vers le routeur](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_MoveTheDHCPRole).  
  
5.  [Supprimer et réaffecter le serveur Source](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  

  
###  <a name="BKMK_UninstallExchangeServer2003"></a>Désinstaller Exchange Server2003  
  
> [!IMPORTANT]
>  Si vous ajoutez des comptes d’utilisateur après avoir déplacé des boîtes aux lettres vers le serveur de Destination et avant de désinstaller Exchange Server2003 à partir du serveur Source, les boîtes aux lettres sont ajoutés sur le serveur Source. Ceci est normal. Vous devez déplacer les boîtes aux lettres vers le serveur de Destination pour tous les comptes d’utilisateur qui sont ajoutés pendant ce temps. Répétez les instructions dans les boîtes aux lettres de déplacer le serveur Exchange et les paramètres pour la migration de WindowsServerEssentials avant de désinstaller Exchange Server2003.  
  
 Vous devez désinstaller Exchange Server2003 à partir du serveur Source avant de le rétrograder. Cette opération supprime toutes les références dans les Services de domaine ActiveDirectory (ADDS) pour Exchange Server sur le serveur Source. Vous devez disposer de votre support de WindowsSmallBusinessServer2003 pour supprimer Exchange Server2003.  
  
##### <a name="to-uninstall-exchange-server-2003-from-the-source-server"></a>Pour désinstaller Exchange Server2003 à partir du serveur Source  
  
1.  Ouvrez une session sur le serveur Source en tant qu’administrateur  
  
2.  Cliquez sur **Démarrer**, cliquez sur **le panneau de configuration**, puis cliquez sur **ajouter ou supprimer des programmes**.  
  
3.  Dans la liste des programmes, sélectionnez **WindowsSmallBusinessServer2003**, puis cliquez sur **modifier ou supprimer**.  
  
4.  Dans l’Assistant installation, cliquez sur **suivant** jusqu'à ce que le **sélection des composants** page s’affiche.  
  
5.  Sur la page Sélection des composants, développez **Exchange Server**, puis choisissez **supprimer**.  
  
    > [!NOTE]

    >  Exchange Server vérifie pour vous assurer qu’il n’y a aucune boîtes aux lettres ou dossiers publics sur le serveur. S’il reste des données, un message d’erreur s’affiche lorsque vous cliquez sur **supprimer**. Pour éviter ce problème, assurez-vous que vous avez effectué toutes les procédures décrites dans la rubrique [déplacer SBS2003paramètres et données vers le serveur de Destination](Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  

    >  Exchange Server vérifie pour vous assurer qu’il n’y a aucune boîtes aux lettres ou dossiers publics sur le serveur. S’il reste des données, un message d’erreur s’affiche lorsque vous cliquez sur **supprimer**. Pour éviter ce problème, assurez-vous que vous avez effectué toutes les procédures décrites dans la rubrique [déplacer SBS2003paramètres et données vers le serveur de Destination](../migrate/Move-Windows-SBS-2003-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  

  
6.  Cliquez sur **suivant**.  
  
7.  Lorsque vous y êtes invité, insérez le CD n°3WindowsSmallBusinessServer2003 et suivez les instructions à l’écran.  
  
###  <a name="BKMK_PhysicallyDisconnect"></a>Déconnecter les imprimantes qui sont directement connectés au serveur Source  
 Avant de rétrograder le serveur Source, déconnectez physiquement toutes les imprimantes qui sont directement connectés au serveur Source et sont partagées via le serveur Source. Ne garantir qu’aucun objet ActiveDirectory pour les imprimantes qui étaient connectées directement au serveur Source. Les imprimantes peuvent être directement connectés au serveur de Destination et partagés à partir de WindowsServerEssentials.  
  
###  <a name="BKMK_DemoteTheSourceServer"></a>Rétrograder le serveur Source  
 Avant de rétrograder le serveur Source du rôle du contrôleur de domaine ADDS au rôle d’un serveur membre de domaine, vérifiez que les paramètres de stratégie de groupe sont appliqués à tous les ordinateurs clients, comme décrit dans la procédure suivante.  
  
> [!IMPORTANT]
>  Le serveur Source et le serveur de Destination doivent être connectés au réseau alors que les modifications de stratégie de groupe sont mis à jour sur les ordinateurs clients.  
  
##### <a name="to-force-a-group-policy-update-on-a-client-computer"></a>Pour forcer une mise à jour de stratégie de groupe sur un ordinateur client  
  
1.  Ouvrez une session en tant qu’administrateur l’ordinateur client.  
  
2.  Ouvrez une fenêtre d’invite de commandes en tant qu’administrateur.  
  
3.  À l’invite de commandes, tapez **gpupdate /force**, puis appuyez sur ENTRÉE.  
  
4.  Le processus peut nécessiter vous déconnecter et reconnecter pour terminer. Cliquez sur **Oui** pour confirmer.  
  
##### <a name="to-demote-the-source-server"></a>Pour rétrograder le serveur Source  
  
1.  Sur le serveur Source, cliquez sur **Démarrer**, cliquez sur **exécuter**, type **dcpromo**, puis cliquez sur **OK**.  
  
2.  Cliquez sur **suivant** deux fois.  
  
    > [!NOTE]
    >  Ne sélectionnez pas **ce serveur est le dernier contrôleur de domaine dans le domaine**.  
  
3.  Tapez un mot de passe pour le nouveau compte d’administrateur sur le serveur, puis cliquez sur **suivant**.  
  
4.  Dans le **Résumé** boîte de dialogue, vous êtes informé que les services ADDS seront supprimés de l’ordinateur et que le serveur va devenir un membre du domaine. Cliquez sur **suivant**.  
  
5.  Cliquez sur **Terminer**. Le serveur Source redémarre.  
  
6.  Après le redémarrage du serveur Source, ajoutez le serveur Source en tant que membre d’un groupe de travail avant de le déconnecter du réseau.  
  
 Une fois que vous ajoutez le serveur Source en tant que membre d’un groupe de travail et déconnectez du réseau, vous devez le supprimer à partir des services ADDS sur le serveur de Destination.  
  
##### <a name="to-remove-the-source-server-from-active-directory"></a>Pour supprimer le serveur Source à partir d’ActiveDirectory  
  
1.  Sur le serveur de Destination, ouvrez **ActiveDirectory Users and Computers**.  
  
2.  Dans le **ActiveDirectory Users and Computers** volet de navigation, développez le nom de domaine, puis puis **ordinateurs**.  
  
3.  Le nom du serveur Source s’il existe toujours dans la liste des serveurs, cliquez avec le bouton droit **supprimer**, puis cliquez sur **Oui**.  
  
4.  Vérifiez que le serveur Source n’est pas répertorié, puis fermez **ActiveDirectory Users and Computers**.  
  
###  <a name="BKMK_MoveTheDHCPRole"></a>Déplacer le rôle de serveur DHCP à partir du serveur Source vers le routeur  
  
> [!NOTE]

>  Si vous avez déjà effectué cette tâche avant de commencer le processus de migration, passez à la section [supprimer et réaffecter le serveur Source](Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  

>  Si vous avez déjà effectué cette tâche avant de commencer le processus de migration, passez à la section [supprimer et réaffecter le serveur Source](../migrate/Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer).  

  
 Si votre serveur Source exécute le rôle DHCP, procédez comme suit pour déplacer le rôle DHCP vers le routeur.  
  
##### <a name="to-move-the-dhcp-role-from-the-source-server-to-the-router"></a>Pour déplacer le rôle DHCP du serveur Source vers le routeur  
  
1.  Désactiver le service DHCP sur le serveur Source, comme suit:  
  
    1.  Sur le serveur Source, cliquez sur **Démarrer**, cliquez sur **outils d’administration**, puis cliquez sur **Services**.  
  
    2.  Dans la liste des services en cours d’exécution, cliquez sur le **Windows Server**, puis cliquez sur **propriétés**.  
  
    3.  Pour **type de démarrage**, sélectionnez **désactivé**.  
  
    4.  Arrêtez le service.  
  
2.  Activer le rôle DHCP sur votre routeur  
  
    1.  Suivez les instructions de la documentation de votre routeur pour activer le rôle DHCP sur le routeur.  
  
    2.  Pour vous assurer que les adresses IP émises par le serveur Source restent les mêmes, suivez les instructions de la documentation de votre routeur pour configurer la plage DHCP sur le routeur soit identique à la plage DHCP sur le serveur Source.  
  
    > [!IMPORTANT]
    >  Si vous n’avez pas configuré un statique IP ou réservations DHCP sur le routeur pour le serveur de Destination, et la plage DHCP n’est pas le même que le serveur Source, il est possible que le routeur émette une nouvelle adresse IP pour le serveur de Destination. Dans ce cas, réinitialisez le règles du routeur pour transférer vers la nouvelle adresse IP du serveur de Destination de réacheminement de port.  
  
###  <a name="BKMK_RemoveTheSourceServer"></a>Supprimer et réaffecter le serveur Source  
 Désactiver le serveur Source et déconnectez-le du réseau. Nous recommandons que vous ne pas reformater le serveur Source au moins une semaine pour vous assurer que toutes les données nécessaires migrés vers le serveur de Destination. Après avoir vérifié que toutes les données a migré, vous pouvez réinstaller ce serveur sur le réseau comme serveur secondaire pour d’autres tâches, si nécessaire.  
  
> [!NOTE]
>  Après avoir rétrogradé et supprimer le serveur Source, redémarrez le serveur de Destination.  
  
 Après avoir rétrogradé le serveur Source, il n’est pas dans un état sain. Si vous voulez réaffecter le serveur Source, le moyen le plus simple consiste à remettre, installer un système d’exploitation et puis de le configurer pour l’utiliser comme serveur supplémentaire.
