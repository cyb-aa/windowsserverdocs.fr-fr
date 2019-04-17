---
title: "Résoudre les problèmes de sauvegarde de l’ordinateur et les erreurs de restauration dans WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 06/25/2013
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5cc73aff-d2c0-4cf9-a23d-ef928ae5ddc9
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 37e79661442ba9f66a564b6c6c8fb57db1978454
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-computer-backup-and-restore-errors-in-windows-server-essentials"></a>Résoudre les problèmes de sauvegarde de l’ordinateur et les erreurs de restauration dans WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Utilisez ces procédures pour résoudre les problèmes de sauvegardes de l’ordinateur dans WindowsServerEssentials, y compris les problèmes de configuration de la sauvegarde, les sauvegardes incomplètes ou qui a échoué, alertes d’intégrité de sauvegarde et problèmes de fichier, dossier ou restauration complète du système.  
  
> [!NOTE]
>  Pour les informations de dépannage plus récentes à partir de la Communauté WindowsServerEssentials, visitez le [Forum WindowsServerEssentials](https://social.technet.microsoft.com/Forums//winserveressentials/threads).  
  
##  <a name="BKMK_TroubleshootBackupConfigurationIssues"></a>Résoudre les problèmes de configuration de la sauvegarde d’un ordinateur connecté  
 Utilisez ces procédures pour résoudre les problèmes de configurations de sauvegarde pour les ordinateurs qui sont sauvegardés sur votre serveur WindowsServerEssentials.  
  
### <a name="errors"></a>Erreurs  
  
-   Configuration de la sauvegarde n’a pas abouti  
  
-   Erreur de collecte d’informations pour un ordinateur  
  
-   Sauvegarde de l’ordinateur suppression erreur  
  
### <a name="resolutions"></a>Résolutions  
  
##### <a name="to-troubleshoot-errors-that-occur-while-you-configure-backups-for-a-connected-computer"></a>Pour résoudre les erreurs qui se produisent lors de la configuration des sauvegardes d’un ordinateur connecté  
  
1.  Assurez-vous que votre ordinateur est connecté au réseau par le biais d’un périphérique réseau.  
  
2.  Assurez-vous que le périphérique réseau connecté à l’ordinateur est également connecté au réseau, sous tension et qu’il fonctionne correctement.  
  
3.  Assurez-vous que Service de sauvegarde du Client Windows Server et le Service fournisseur de sauvegarde des ordinateurs de Client Windows Server sont en cours d’exécution sur le serveur.  
  
    ###### <a name="to-start-computer-backup-services-on-the-server"></a>Pour démarrer les services de sauvegarde d’ordinateur sur le serveur  
  
    1.  Sur le serveur, cliquez sur **Démarrer**, cliquez sur **outils d’administration**, puis cliquez sur **Services**.  
  
    2.  Faites défiler vers le bas et cliquez sur **Service fournisseur de sauvegarde des ordinateurs de Client Windows Server**. Si l’état du service n’est pas **démarré**, cliquez sur le service, puis cliquez sur **Démarrer**.  
  
    3.  Cliquez sur **Service sauvegarde de l’ordinateur Client de Windows Server**. Si l’état du service n’est pas **démarré**, cliquez sur le service, puis cliquez sur **Démarrer**.  
  
    4.  Fermer **Services**.  
  
4.  Assurez-vous que le Service fournisseur de sauvegarde des ordinateurs de Client Windows Server est en cours d’exécution sur l’ordinateur client.  
  
    ###### <a name="to-start-the-computer-backup-service-on-the-client-computer"></a>Pour démarrer le service de sauvegarde d’ordinateur sur l’ordinateur client  
  
    1.  Sur l’ordinateur client, cliquez sur **Démarrer**, type **Services** dans les **rechercher les programmes et fichiers** zone de texte et appuyez sur ENTRÉE.  
  
    2.  Faites défiler vers le bas et cliquez sur **Service fournisseur de sauvegarde des ordinateurs de Client Windows Server**. Si l’état du service n’est pas **démarré**, cliquez sur le service, puis cliquez sur **Démarrer**.  
  
    3.  Fermer **Services**.  
  
5.  Consultez les alertes pour déterminer s’il existe des erreurs dans la base de données de sauvegarde. S’il existe des erreurs, suivez les instructions de l’alerte pour réparer la base de données de sauvegarde.  
  
6.  Désinstaller le logiciel Connecteur WindowsServerEssentials à partir de l’ordinateur, puis réinstallez-le. Pour plus d’informations, consultez les rubriques [désinstaller le logiciel Connecteur](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13) et [installer le logiciel Connecteur](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_11).  
  
##  <a name="BKMK_TroubleshootaBackupThatDidNotCompleteProperly"></a>Résoudre les problèmes d’une sauvegarde n’a pas abouti  
 Lorsqu’une sauvegarde a échoué, aucune partie de la sauvegarde a réussi et aucune donnée n’est disponible pour pouvoir les restaurer. Toutefois, lorsqu’une sauvegarde présente un état incomplet, pas tous les éléments spécifiés dans la sauvegarde configuration ont été sauvegardés, mais certaines données peuvent être disponibles pour pouvoir les restaurer.  
  
### <a name="errors"></a>Erreurs  
  
-   La sauvegarde est incomplète  
  
-   Échec de la sauvegarde  
  
### <a name="resolutions"></a>Résolutions  
  
##### <a name="to-identify-volumes-that-were-not-backed-up-successfully"></a>Pour identifier les volumes qui ont été sauvegardés pas correctement  
  
1.  Ouvrez le tableau de bord WindowsServerEssentials, puis cliquez sur **ordinateurs et sauvegarde**.  
  
2.  Cliquez sur le nom de l’ordinateur où la sauvegarde s’est pas terminée correctement, puis cliquez sur **afficher les propriétés de l’ordinateur** dans les **tâches** volet.  
  
3.  Cliquez sur la sauvegarde qui s’est pas terminée correctement, puis cliquez sur **afficher les détails**.  
  
4.  Dans le **détails de la sauvegarde** boîte de dialogue, une coche verte s’affiche dans l’état de tous les volumes qui ont été correctement sauvegardés.  
  
##### <a name="to-troubleshoot-an-unsuccessful-backup-of-a-volume"></a>Pour résoudre les problèmes d’un échec de la sauvegarde d’un volume  
  
1.  Assurez-vous que le disque dur est connecté à l’ordinateur sous tension et qu’il fonctionne correctement.  
  
2.  Exécutez **chkdsk /f /r** pour corriger les erreurs sur le disque dur (**/f**) et récupérer des informations lisibles à partir de n’importe quel secteur défectueux (**/r**). Pour plus d’informations sur l’exécution **chkdsk**, voir [CHKDSK](https://go.microsoft.com/fwlink/?LinkId=206562).  
  
3.  Assurez-vous que l’ordinateur a été pas arrêté ou déconnecté du réseau pendant l’exécution de la sauvegarde.  
  
4.  Assurez-vous qu’il y a suffisamment d’espace libre sur chaque volume pour la sauvegarde pour s’exécuter. Sauvegarde nécessite un espace disque supplémentaire sur l’ordinateur client pour créer un cliché instantané VSS. Sur un volume qui n’est pas réservée du système, au moins 10 pour cent d’espace disque disponible doit être disponible. Sur un volume réservé au système, si la taille du volume est inférieure à 500Mo, VSS nécessite 32Mo d’espace libre créer un cliché instantané. Si le volume est égal à 500Mo ou plus, VSS a besoin de 320Mo d’espace libre.  
  
     Si l’espace libre est insuffisant sur un volume, essayez l’une de ces solutions:  
  
    -   Étendre le volume. Vous pouvez étendre un volume de base ou dynamique, sauf le volume réservé au système.  
  
        ###### <a name="to-extend-a-volume"></a>Pour étendre un volume  
  
        1.  Dans le panneau de configuration, cliquez sur **système et sécurité**.  
  
        2.  Sous **outils d’administration**, cliquez sur **créer et formater des partitions de disque dur**.  
  
        3.  Cliquez sur le volume que vous souhaitez étendre. Si **étendre le Volume** est activé, sélectionnez cette option. Si l’option n’est pas activée, vous ne pouvez pas étendre le volume.  
  
        4.  Suivez les étapes de l’Assistant extension du Volume pour étendre le volume.  
  
    -   Supprimer le contenu du volume pour libérer de l’espace.  
  
        > [!NOTE]
        >  Si vous devez libérer de l’espace sur le volume réservé au système, vous pouvez déplacer l’Image de récupération système sur un autre volume. Pour obtenir des instructions, voir [déployer une Image de récupération système](https://technet.microsoft.com/library/dd744280\(v=ws.10\).aspx).  
  
    -   Excluez le volume à partir de la sauvegarde du client. Cela uniquement si elle n’est pas important de conserver une copie de sauvegarde des données sur le volume.  
  
        > [!WARNING]
        >  Si vous excluez le volume réservé au système à partir d’une sauvegarde client, le système client n’est pas sauvegardé, et vous ne serez pas en mesure d’effectuer une restauration complète du système sur l’ordinateur.  
  
5.  Recherchez d’autres alertes sur le serveur qui peuvent indiquer il n’existe pas suffisamment d’espace disque sur le serveur pour la sauvegarde s’exécute correctement. Suivez les instructions de l’alerte pour résoudre le problème.  
  
6.  Exécutez **vssadmin** dans une invite de commandes pour résoudre les problèmes de Volume Shadow Copy Service (VSS) émet. Pour plus d’informations sur les **vssadmin**, voir [VSSADMIN](https://go.microsoft.com/fwlink/?LinkID=94332).  
  
##  <a name="BKMK_TroubleshootBackupHealthAlertIssues"></a>Résoudre les problèmes alerte d’intégrité de sauvegarde  
  
### <a name="errors"></a>Erreurs  
  
-   Le Service du fournisseur de sauvegarde WindowsServerSolutions ordinateur a cessé de fonctionner  
  
-   Le Service fournisseur de sauvegarde ordinateur WindowsServerSolutionsClient a cessé de fonctionner  
  
### <a name="resolutions"></a>Résolutions  
  
##### <a name="to-troubleshoot-a-backup-health-alert"></a>Pour résoudre une alerte d’intégrité de sauvegarde  
  
1.  Si une alerte vous indique que la base de données de sauvegarde présente des problèmes, suivez les instructions de l’alerte pour résoudre le problème.  
  
2.  Si une alerte vous indique qu’un service de sauvegarde n’est pas en cours d’exécution, essayez de démarrer le service sur le serveur ou l’ordinateur client où vous avez reçu le message d’erreur.  
  
    ###### <a name="to-start-backup-services-on-the-server"></a>Pour démarrer les services de sauvegarde sur le serveur  
  
    1.  Sur le serveur, cliquez sur **Démarrer**, puis cliquez sur **outils d’administration**, puis cliquez sur **Services**.  
  
        > [!NOTE]
        >  Si vous administrez le serveur à distance, vous devez utiliser Connexion Bureau à distance pour accéder au bureau du serveur. Pour plus d’informations sur l’utilisation de connexion Bureau à distance, consultez [se connecter à un autre ordinateur à l’aide de la connexion Bureau à distance](https://windows.microsoft.com/windows-vista/Connect-to-another-computer-using-Remote-Desktop-Connection).  
  
    2.  Faites défiler vers le bas et cliquez sur **Service fournisseur de sauvegarde des ordinateurs de Client Windows Server**. Si l’état du service n’est pas **démarré**, cliquez sur le service, puis cliquez sur **Démarrer**.  
  
    3.  Cliquez sur **Service sauvegarde de l’ordinateur Client de Windows Server**. Si l’état du service n’est pas **démarré**, cliquez sur le service, puis cliquez sur **Démarrer**.  
  
    4.  Fermer **Services**.  
  
    ###### <a name="to-start-backup-services-on-a-client-computer"></a>Pour démarrer les services de sauvegarde sur un ordinateur client  
  
    1.  Sur l’ordinateur client, cliquez sur **Démarrer**, type **Services** dans les **rechercher les programmes et fichiers** zone de texte, puis cliquez sur ENTRÉE.  
  
    2.  Avec le bouton droit **Service fournisseur de sauvegarde des ordinateurs de Client Windows Server**, puis cliquez sur **Démarrer**.  
  
    3.  Fermer le **Services**.  
  
3.  Vérifiez les journaux des événements sur l’ordinateur client ou serveur pour les problèmes relatifs à la sauvegarde des services ou des pilotes.  
  
4.  Redémarrez le serveur ou un ordinateur client où vous avez reçu le message d’erreur.  
  
5.  Vérifiez les alertes d’intégrité pour d’autres problèmes qui peuvent avoir une incidence sur la sauvegarde du client.  
  
##  <a name="BKMK_FileAndFolder"></a>Résoudre les problèmes de restauration d’un fichier ou dossier  
  
### <a name="errors"></a>Erreurs  
  
-   Restauration du fichier ou dossier n’a pas abouti.  
  
### <a name="resolutions"></a>Résolutions  
  
##### <a name="to-troubleshoot-an-unsuccessful-file-or-folder-restore"></a>Pour résoudre les problèmes d’une restauration de fichier ou dossier échoue  
  
1.  Assurez-vous que votre ordinateur est connecté au réseau par le biais d’un périphérique réseau.  
  
2.  Assurez-vous que le périphérique réseau connecté à l’ordinateur est également connecté au réseau, sous tension et qu’il fonctionne correctement.  
  
3.  Consultez les alertes pour déterminer s’il existe des erreurs dans la base de données de sauvegarde. S’il existe des erreurs, suivez les instructions de l’alerte pour réparer la base de données de sauvegarde.  
  
4.  Essayez de restaurer les fichiers ou dossiers à partir d’une autre sauvegarde.  
  
5.  Assurez-vous que le **pilote de restauration d’ordinateur Windows Server solutions** est installé et fonctionne correctement.  
  
    ###### <a name="to-check-the-status-of-the-windows-server-solution-computer-restore-driver"></a>Pour vérifier l’état du pilote de restauration d’ordinateur WindowsServerSolution  
  
    1.  Cliquez sur **Démarrer**, type **le Gestionnaire de périphériques** dans les **rechercher les programmes et fichiers** zone de texte, puis cliquez sur ENTRÉE.  
  
    2.  Dans le Gestionnaire de périphériques, cliquez sur **périphériques système**, faites défiler jusqu'à **pilote de restauration d’ordinateur WindowsServerSolutions**.  
  
    3.  Si le pilote n’est pas affiché:  
  
        1.  Ouvrez une invite de commandes avec des privilèges d’administrateur et exécutez la commande suivante:  
  
             **%ProgramFiles%\Windows Server\Bin\BackupDriverInstaller.exe?  Je**  
  
        2.  Actualiser le Gestionnaire de périphériques. Le pilote doit apparaître.  
  
    4.  Si l’icône affichée est un moniteur d’ordinateur, le pilote est correctement installé et en cours d’exécution. Fermez le Gestionnaire de périphériques.  
  
    5.  Si l’icône n’est pas un moniteur d’ordinateur  
  
        1.  Avec le bouton droit **pilote de restauration d’ordinateur WindowsServerSolutions**, puis cliquez sur **propriétés**.  
  
        2.  Cliquez sur le **pilote** onglet, puis cliquez sur **pilote mise à jour**.  
  
        3.  Cliquez sur **rechercher automatiquement un pilote mis à jour**, puis suivez les instructions pour mettre à jour le pilote.  
  
    6.  Fermez le Gestionnaire de périphériques.  
  
6.  Désinstaller le logiciel Connecteur WindowsServerEssentials à partir de l’ordinateur, puis réinstallez-le. Pour plus d’informations, consultez les rubriques [désinstaller le logiciel Connecteur](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13) et [installer le logiciel Connecteur](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_11).  
  
##  <a name="BKMK_Troubleshootfullsystemrestoreissues"></a>Résoudre les problèmes d’une restauration complète du système  
  
### <a name="errors"></a>Erreurs  
  
-   Ne peut pas ouvrir une session sur l’ordinateur client après une restauration complète du système.  
  
### <a name="resolutions"></a>Résolutions  
 Si vous modifiez le nom d’un ordinateur, et vous devez restaurer une sauvegarde qui a été enregistrée avant cette modification, après la restauration, lorsque vous tentez d’ouvrir une session sous votre compte de domaine, vous recevez cette erreur: «la base de données de sécurité sur le serveur n’a pas d’un compte d’ordinateur pour cette relation d’approbation de station de travail.» Pour gagner de l’accès réseau à l’ordinateur à nouveau, supprimer le logiciel Connecteur, supprimer l’ordinateur du domaine Windows, puis se connecter au serveur.  
  
##### <a name="to-regain-network-access-to-a-restored-computer-after-a-computer-name-change"></a>Pour récupérer l’accès réseau à un ordinateur restauré après un changement de nom d’ordinateur  
  
1.  Connectez-vous à l’ordinateur en tant qu’un administrateur local.  
  
2.  Désinstaller le logiciel connecteur. Pour plus d’informations, voir [désinstaller le logiciel Connecteur](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13).  
  
3.  Supprimer l’ordinateur du domaine. Pour plus d’informations, voir [supprimer un ordinateur d’un domaine Windows](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_8).  
  
4.  Reconnecter l’ordinateur au serveur. Pour plus d’informations, voir [connecter des ordinateurs au serveur](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Prise en charge Windows Server Essentials](Support-Windows-Server-Essentials.md)