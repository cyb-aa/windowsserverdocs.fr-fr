---
title: Résoudre les problèmes liés à la sauvegarde de l’ordinateur et aux erreurs de restauration dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.date: 06/25/2013
ms.prod: windows-server
ms.topic: article
ms.assetid: 5cc73aff-d2c0-4cf9-a23d-ef928ae5ddc9
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 542c0a583aca56ce72ef0e2e55f2b1543558dfa8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852182"
---
# <a name="troubleshoot-computer-backup-and-restore-errors-in-windows-server-essentials"></a>Résoudre les problèmes liés à la sauvegarde de l’ordinateur et aux erreurs de restauration dans Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Ces procédures vous permettent de résoudre les problèmes liés aux sauvegardes de l'ordinateur dans Windows Server Essentials, y compris les problèmes de configuration de la sauvegarde, les sauvegardes incomplètes ou qui ont échoué, les alertes d'intégrité de sauvegarde et problèmes de restauration des fichiers, des dossiers ou du système complet.  
  
> [!NOTE]
>  Pour obtenir les informations de dépannage les plus récentes de la communauté Windows Server Essentials, visitez le [Forum Windows Server Essentials](https://social.technet.microsoft.com/Forums//winserveressentials/threads).  
  
##  <a name="troubleshoot-backup-configuration-issues-for-a-connected-computer"></a><a name="BKMK_TroubleshootBackupConfigurationIssues"></a>Résoudre les problèmes de configuration de sauvegarde pour un ordinateur connecté  
 Utilisez ces procédures pour résoudre les problèmes de configurations de sauvegarde pour les ordinateurs qui sont sauvegardés sur votre serveur Windows Server Essentials.  
  
### <a name="errors"></a>Erreurs  
  
-   Échec de configuration de la sauvegarde  
  
-   Erreur de collecte d'informations pour l'ordinateur  
  
-   Erreur d'annulation de la sauvegarde de l'ordinateur  
  
### <a name="resolutions"></a>Solutions  
  
##### <a name="to-troubleshoot-errors-that-occur-while-you-configure-backups-for-a-connected-computer"></a>Pour résoudre les erreurs se produisant pendant la configuration des sauvegardes d'un ordinateur connecté  
  
1.  Vérifiez que votre ordinateur est connecté au réseau via un périphérique réseau.  
  
2.  Le périphérique réseau doit également être connecté au réseau, être allumé et fonctionner correctement.  
  
3.  Vérifiez que le Service de sauvegarde des clients Windows Server et le Service fournisseur de sauvegarde Windows Server pour ordinateurs client s'exécutent sur le serveur.  
  
    ###### <a name="to-start-computer-backup-services-on-the-server"></a>Pour démarrer les services de sauvegarde de l'ordinateur sur le serveur  
  
    1.  Sur le serveur, cliquez sur **Démarrer**, **Outils d'administration**, puis **Services**.  
  
    2.  Faites défiler vers le bas et cliquez sur **Service fournisseur de sauvegarde Windows Server pour ordinateurs client**. Si l'état du service n'est pas **Démarré**, cliquez sur le service avec le bouton droit, puis cliquez sur **Démarrer**.  
  
    3.  Cliquez sur **Service Sauvegarde d'ordinateur client Windows Server**. Si l'état du service n'est pas **Démarré**, cliquez sur le service avec le bouton droit, puis cliquez sur **Démarrer**.  
  
    4.  Fermez **Services**.  
  
4.  Vérifiez que le Service fournisseur de sauvegarde Windows Server pour ordinateurs clients s'exécute sur l'ordinateur client.  
  
    ###### <a name="to-start-the-computer-backup-service-on-the-client-computer"></a>Pour démarrer le service de sauvegarde de l'ordinateur sur l'ordinateur client  
  
    1.  Sur l'ordinateur client, cliquez sur **Démarrer**, tapez **Services** dans les zones de texte **Rechercher les programmes et fichiers** et appuyez sur Entrée.  
  
    2.  Faites défiler vers le bas et cliquez sur **Service fournisseur de sauvegarde Windows Server pour ordinateurs client**. Si l'état du service n'est pas **Démarré**, cliquez sur le service avec le bouton droit, puis cliquez sur **Démarrer**.  
  
    3.  Fermez **Services**.  
  
5.  Consultez les alertes pour déterminer si des erreurs existent dans la base de données de sauvegarde. Le cas échéant, suivez les instructions de l'alerte pour réparer la base de données de sauvegarde.  
  
6.  Désinstallez le logiciel Connecteur Windows Server Essentials de l'ordinateur, puis réinstallez-le. Pour plus d'informations, voir les rubriques [Désinstaller le logiciel Connecteur](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13) et [Installer le logiciel Connecteur](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_11).  
  
##  <a name="troubleshoot-a-backup-that-did-not-complete-properly"></a><a name="BKMK_TroubleshootaBackupThatDidNotCompleteProperly"></a>Résoudre les problèmes liés à une sauvegarde qui n’a pas été effectuée correctement  
 Quand le statut d'une sauvegarde est Échec, aucune partie de la sauvegarde n'a abouti et aucune donnée ne peut être restaurée. Toutefois, quand l'état de la sauvegarde est Incomplet, tous les éléments spécifiés dans la sauvegarde de configuration n'ont pas été sauvegardés, mais certaines données peuvent être restaurées.  
  
### <a name="errors"></a>Erreurs  
  
-   Sauvegarde incomplète  
  
-   Échec de la sauvegarde  
  
### <a name="resolutions"></a>Solutions  
  
##### <a name="to-identify-volumes-that-were-not-backed-up-successfully"></a>Pour identifier les volumes qui n'ont pas été correctement sauvegardés  
  
1.  Ouvrez le tableau de bord Windows Server Essentials, puis cliquez sur **Ordinateurs et sauvegarde**.  
  
2.  Cliquez sur le nom de l'ordinateur pour lequel la sauvegarde ne s'est pas correctement terminée, puis cliquez sur **Afficher les propriétés de l'ordinateur** dans le volet **Tâches**.  
  
3.  Cliquez sur la sauvegarde qui ne s'exécute pas correctement, puis cliquez sur **Afficher les détails**.  
  
4.  Dans la boîte de dialogue **Détails de la sauvegarde** , une coche verte s'affiche dans l'état de tous les volumes qui ont été correctement sauvegardés.  
  
##### <a name="to-troubleshoot-an-unsuccessful-backup-of-a-volume"></a>Pour résoudre un échec de sauvegarde d'un volume  
  
1.  Vérifiez que le disque dur est connecté à l'ordinateur, qu'il est sous tension et qu'il fonctionne correctement.  
  
2.  Exécutez **chkdsk /f /r** pour corriger les erreurs sur le disque dur ( **/f**) et récupérer des informations lisibles à partir de n'importe quel secteur défectueux ( **/r**). Pour plus d’informations sur l’exécution de **chkdsk**, consultez [CHKDSK](https://go.microsoft.com/fwlink/?LinkId=206562).  
  
3.  Vérifiez que l'ordinateur n'a été pas arrêté ou déconnecté du réseau pendant l'exécution de la sauvegarde.  
  
4.  L'espace libre doit être suffisant sur chaque volume pour exécuter la sauvegarde. Celle-ci nécessite de l'espace disque supplémentaire sur l'ordinateur client pour créer un cliché instantané VSS. Sur un volume qui n'est pas réservé au système, au moins 10 pour cent d'espace disque doit être disponible. Sur un volume réservé au système, si la taille du volume est inférieure à 500 Mo, VSS nécessite 32 Mo d'espace libre pour créer un cliché instantané. Si le volume est égal à 500 Mo ou plus, VSS a besoin de 320 Mo d'espace libre.  
  
     Si l'espace libre est insuffisant sur un volume, essayez l'une de ces solutions :  
  
    -   Étendre le volume. Vous pouvez étendre un volume de base ou dynamique, sauf le volume réservé au système.  
  
        ###### <a name="to-extend-a-volume"></a>Pour étendre un volume  
  
        1.  Dans le Panneau de configuration, cliquez sur **Système et sécurité**.  
  
        2.  Sous **Outils d'administration**, cliquez sur **Créer et formater des partitions de disque dur**.  
  
        3.  Cliquez sur le volume que vous souhaitez étendre. Si **Étendre le volume** est activé, sélectionnez cette option. Si l'option n'est pas activée, vous ne pouvez pas étendre le volume.  
  
        4.  Suivez les étapes de l'Assistant Extension du volume pour étendre le volume.  
  
    -   Supprimez le contenu du volume pour libérer de l'espace.  
  
        > [!NOTE]
        >  Si vous avez besoin de libérer de l'espace sur le volume réservé au système, vous pouvez déplacer l'image de récupération du système sur un volume différent. Pour obtenir des instructions, consultez la rubrique [Déployer une image de récupération du système](https://technet.microsoft.com/library/dd744280\(v=ws.10\).aspx).  
  
    -   Excluez le volume de la sauvegarde client, uniquement si vous n'avez pas besoin de tenir à jour une copie de sauvegarde des données sur le volume.  
  
        > [!WARNING]
        >  Si vous excluez le volume réservé au système d'une sauvegarde client, le système du client n'est pas sauvegardé, et vous ne pourrez pas effectuer de restauration complète du système sur l'ordinateur.  
  
5.  Recherchez d'autres alertes sur le serveur qui peuvent indiquer que celui-ci manque d'espace disque pour que la sauvegarde s'exécute correctement. Suivez les instructions de l'alerte pour résoudre le problème.  
  
6.  Exécutez **vssadmin** dans une invite de commandes pour résoudre les problèmes du service VSS. Pour plus d’informations sur **vssadmin**, consultez [VSSADMIN](https://go.microsoft.com/fwlink/?LinkID=94332).  
  
##  <a name="troubleshoot-backup-health-alert-issues"></a><a name="BKMK_TroubleshootBackupHealthAlertIssues"></a>Résoudre les problèmes liés aux alertes d’intégrité de sauvegarde  
  
### <a name="errors"></a>Erreurs  
  
-   Interruption du Service fournisseur de sauvegarde des solutions Windows Server pour ordinateurs  
  
-   Interruption du Service fournisseur de sauvegarde des solutions Windows Server pour ordinateurs client  
  
### <a name="resolutions"></a>Solutions  
  
##### <a name="to-troubleshoot-a-backup-health-alert"></a>Pour résoudre une alerte d'intégrité de la sauvegarde  
  
1.  Si une alerte vous indique que la base de données de sauvegarde présente des problèmes, suivez les instructions de l'alerte pour résoudre ces problèmes.  
  
2.  Si une alerte vous indique qu'un service de sauvegarde ne fonctionne pas, essayez de démarrer le service sur le serveur ou l'ordinateur client sur lequel vous avez reçu le message d'erreur.  
  
    ###### <a name="to-start-backup-services-on-the-server"></a>Pour démarrer les services de sauvegarde sur le serveur  
  
    1.  Sur le serveur, cliquez sur **Démarrer**, **Outils d'administration**, puis **Services**.  
  
        > [!NOTE]
        >  Si vous administrez le serveur à distance, vous devez utiliser Connexion Bureau à distance pour accéder au Bureau du serveur. Pour plus d’informations sur l’utilisation de la connexion Bureau à distance, consultez la rubrique [Se connecter à un autre ordinateur à l’aide de la connexion Bureau à distance](https://windows.microsoft.com/windows-vista/Connect-to-another-computer-using-Remote-Desktop-Connection).  
  
    2.  Faites défiler vers le bas et cliquez sur **Service fournisseur de sauvegarde Windows Server pour ordinateurs client**. Si l'état du service n'est pas **Démarré**, cliquez sur le service avec le bouton droit, puis cliquez sur **Démarrer**.  
  
    3.  Cliquez sur **Service Sauvegarde d'ordinateur client Windows Server**. Si l'état du service n'est pas **Démarré**, cliquez sur le service avec le bouton droit, puis cliquez sur **Démarrer**.  
  
    4.  Fermez **Services**.  
  
    ###### <a name="to-start-backup-services-on-a-client-computer"></a>Pour démarrer les services de sauvegarde sur un ordinateur client  
  
    1.  Sur l'ordinateur client, cliquez sur **Démarrer**, tapez **Services** dans les zones de texte **Rechercher les programmes et fichiers** et appuyez sur ENTRÉE.  
  
    2.  Cliquez avec le bouton droit sur **Service fournisseur de sauvegarde Windows Server pour ordinateurs client**, puis cliquez sur **Démarrer**.  
  
    3.  Fermez les **Services**.  
  
3.  Vérifiez les journaux des événements sur l'ordinateur client ou le serveur pour les problèmes liés aux services de sauvegarde ou aux pilotes.  
  
4.  Redémarrez le serveur ou l'ordinateur client sur lequel vous avez reçu le message d'erreur.  
  
5.  Vérifiez les alertes d'intégrité liées à d'autres problèmes qui peuvent avoir une incidence sur la sauvegarde du client.  
  
##  <a name="troubleshoot-a-file-or-folder-restore"></a><a name="BKMK_FileAndFolder"></a>Résoudre les problèmes de restauration d’un fichier ou d’un dossier  
  
### <a name="errors"></a>Erreurs  
  
-   Échec de la restauration du fichier ou du dossier  
  
### <a name="resolutions"></a>Solutions  
  
##### <a name="to-troubleshoot-an-unsuccessful-file-or-folder-restore"></a>Pour résoudre les problèmes liés à un échec de restauration de fichier ou de dossier  
  
1.  Vérifiez que votre ordinateur est connecté au réseau via un périphérique réseau.  
  
2.  Le périphérique réseau doit également être connecté au réseau, être allumé et fonctionner correctement.  
  
3.  Consultez les alertes pour déterminer si des erreurs existent dans la base de données de sauvegarde. Le cas échéant, suivez les instructions de l'alerte pour réparer la base de données de sauvegarde.  
  
4.  Essayez de restaurer les fichiers ou les dossiers à partir d'une autre sauvegarde.  
  
5.  Vérifiez que le **Pilote de restauration d'ordinateur des solutions Windows Server** est installé et fonctionne correctement.  
  
    ###### <a name="to-check-the-status-of-the-windows-server-solution-computer-restore-driver"></a>Pour vérifier l'état du pilote de restauration d'ordinateur des solutions Windows Server  
  
    1.  Cliquez sur **Démarrer**, tapez **Gestionnaire de périphériques** dans les zones de texte **Rechercher les programmes et fichiers**, puis cliquez sur ENTRÉE.  
  
    2.  Dans le Gestionnaire de périphériques, cliquez sur **Périphériques système**, faites défiler jusqu'à **Solutions Windows Server - Pilote de restauration d'ordinateur**.  
  
    3.  Si le pilote n'est pas affiché :  
  
        1.  Ouvrez une invite de commandes avec des privilèges d'administrateur et exécutez la commande suivante :  
  
             **%ProgramFiles%\Windows Server\Bin\BackupDriverInstaller.exe ? -i**  
  
        2.  Actualisez le Gestionnaire de périphériques. Le pilote doit apparaître.  
  
    4.  Si l'icône affichée est un moniteur d'ordinateur, le pilote est correctement installé et en cours d'exécution. Fermez le Gestionnaire de périphériques.  
  
    5.  Si l'icône n'est pas un moniteur d'ordinateur :  
  
        1.  Cliquez avec le bouton droit sur **Solutions Windows Server - Pilote de restauration d'ordinateur**, puis cliquez sur **Propriétés**.  
  
        2.  Cliquez sur l'onglet **Pilote**, puis sur **Mettre à jour le pilote**.  
  
        3.  Cliquez sur **Rechercher automatiquement un pilote logiciel mis à jour**, puis suivez les instructions pour mettre à jour le pilote.  
  
    6.  Fermez le Gestionnaire de périphériques.  
  
6.  Désinstallez le logiciel Connecteur Windows Server Essentials de l'ordinateur, puis réinstallez-le. Pour plus d'informations, voir les rubriques [Désinstaller le logiciel Connecteur](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13) et [Installer le logiciel Connecteur](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_11).  
  
##  <a name="troubleshoot-a-full-system-restore"></a><a name="BKMK_Troubleshootfullsystemrestoreissues"></a>Résoudre les problèmes liés à une restauration complète du système  
  
### <a name="errors"></a>Erreurs  
  
-   Impossible d'ouvrir une session sur l'ordinateur client après une restauration complète du système.  
  
### <a name="resolutions"></a>Solutions  
 Si vous modifiez le nom d'un ordinateur, et que vous devez restaurer une sauvegarde qui a été enregistrée avant cette modification, quand vous tentez d'ouvrir une session sous votre compte de domaine après la restauration, le message d'erreur suivant s'affichera : « La base de données de sécurité du serveur n'a pas de compte d'ordinateur pour la relation d'approbation avec cette station de travail. » Pour obtenir à nouveau l'accès réseau à l'ordinateur, supprimez le logiciel Connecteur, enlevez l'ordinateur du domaine Windows, puis reconnectez-vous au serveur.  
  
##### <a name="to-regain-network-access-to-a-restored-computer-after-a-computer-name-change"></a>Pour récupérer l'accès réseau à un ordinateur restauré après une modification du nom de l'ordinateur  
  
1.  Connectez-vous à l'ordinateur en tant qu'administrateur local.  
  
2.  Désinstallez le logiciel Connecteur. Pour plus d'informations, voir [Désinstaller le logiciel Connecteur](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_13).  
  
3.  Supprimez l'ordinateur du domaine. Pour plus d'informations, voir [Supprimer un ordinateur d'un domaine Windows](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_8).  
  
4.  Reconnectez l'ordinateur au serveur. Pour plus d'informations, voir [Connexion d'ordinateurs au serveur](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Prendre en charge Windows Server Essentials](Support-Windows-Server-Essentials.md)