---
title: "Résoudre les problèmes de connexion des ordinateurs au serveur dans WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e45b3d89-c057-4c70-a627-86fb06dd22aa
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 9b0d11be08840ecedabab6fd4e96f5d453ea4857
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-connecting-computers-to-the-server-in-windows-server-essentials"></a>Résoudre les problèmes de connexion des ordinateurs au serveur dans WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials 
  
 Cette rubrique contient des conseils de dépannage pour les problèmes que vous pouvez rencontrer lorsque vous vous connectez un ordinateur au serveur qui exécute WindowsServerEssentials ou WindowsServerEssentials.  
  
> [!NOTE]
>  Pour les informations de dépannage plus récentes à partir de la Communauté de WindowsServerEssentials et de WindowsServerEssentials, nous vous suggérons de visiter le [Forum WindowsServerEssentials](https://social.technet.microsoft.com/Forums/windowsserver/home?forum=winserveressentials). Le Forum WindowsServerEssentials est idéale pour rechercher de l’aide ou pour poser une question.  
  
 Cette rubrique fournit des solutions pour les problèmes suivants:  
  

-   Problème 1: [problème 1](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BMRK_Package)  
  
-   Problème 2: [émettre 2](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2)  
  
-   Problème 3: [émettre 3](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2a)  
  
-   Problème 4: [émettre 4](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueNetFramework)  
  
-   Problème 5: [émettre 5](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_Time)  
  
-   Problème 6: [émettre 6](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ServiceStopped)  
  
-   Problème 7: [émettre 7](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueReconnect)  
  
-   Problème 8: [émettre 8](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_JoinWin7)  
  
-   Problème 9: [émettre 9](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueAutologon)  
  
-   Problème 10: [émettre 10](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueOldLogs)  
  
-   Problème 11: [émettre 11](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_UpgradeClientOS)  

-   Problème 1: [problème 1](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BMRK_Package)  
  
-   Problème 2: [émettre 2](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2)  
  
-   Problème 3: [émettre 3](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2a)  
  
-   Problème 4: [émettre 4](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueNetFramework)  
  
-   Problème 5: [émettre 5](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_Time)  
  
-   Problème 6: [émettre 6](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ServiceStopped)  
  
-   Problème 7: [émettre 7](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueReconnect)  
  
-   Problème 8: [émettre 8](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_JoinWin7)  
  
-   Problème 9: [émettre 9](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueAutologon)  
  
-   Problème 10: [émettre 10](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueOldLogs)  
  
-   Problème 11: [émettre 11](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_UpgradeClientOS)  

  
##  <a name="BMRK_Package"></a>Problème 1  
 **Problème**  
  
 Obtenir un Package d’installation n’a pas réussi. Essayez de nouveau d’installer le connecteur WindowsServerEssentials. Si le problème persiste, contactez votre erreur de l’administrateur réseau lors de la connexion d’un ordinateur au serveur  
  
 **Description**  
  
 Ce problème peut se produire si vous vous connectez un ordinateur à un serveur qui exécute WindowsServerEssentials, tandis que d’autres mises à jour Windows ou les installations d’applications sont en attente et l’installation du connecteur est annulée.  
  
 **Solution**  
  
 Effectuez toutes les autres mises à jour ou installations d’applications. Redémarrez les ordinateurs si vous y êtes invité.  
  
##  <a name="BKMK_ConnectorIssue2"></a>Problème 2  
 **Problème**  
  
 Ne peut pas joindre un ordinateur vers WindowsServerEssentials  
  
 **Description**  
  
 Les ordinateurs qui ont des caractères non-ASCII dans le nom d’ordinateur ne peut pas être joint à WindowsServerEssentials. Si le nom d’ordinateur comprend des caractères non-ASCII, vous recevez le message d’erreur «une erreur inattendue s’est produite.».  
  
 **Solution**  
  
 Renommez l’ordinateur client avec un nom qui contient uniquement des caractères ASCII et essayez d’ajouter l’ordinateur vers WindowsServerEssentials à nouveau.  
  
##  <a name="BKMK_ConnectorIssue2a"></a>Problème 3  
 **Problème**  
  
 Obtenir un connecteur de l’installation du logiciel est annulée erreur lors de la connexion d’un ordinateur au serveur  
  
 **Description**  
  
 Pour pouvoir connecter un ordinateur au serveur, le compte système doit disposer des autorisations Contrôle total sur les dossiers de serveur affichés sur le tableau de bord WindowsServerEssentials. Si les autorisations requises n’ont pas été accordées, vous recevez l’erreur «l’installation du logiciel connecteur est annulée» message.  
  
 **Solution**  
  
 Accordez au compte système **contrôle total** autorisations sur chaque dossier de serveur.  
  
#### <a name="to-grant-the-system-account-full-control-permissions-on-a-server-folder"></a>Pour accorder des autorisations Contrôle total sur un dossier de serveur au compte système  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Cliquez sur **stockage**, puis cliquez sur **dossiers du serveur**.  
  
3.  Cliquez sur le dossier du serveur, puis cliquez sur **ouvrir le dossier**. (Si vous ne voyez pas le **ouvrir le dossier** option, vous n’avez pas besoin de définir des autorisations sur le dossier.)  
  
4.  Dans le chemin d’accès réseau en haut de l’Explorateur Windows, cliquez sur le partage de serveur pour afficher les dossiers partagés sur le serveur. Par exemple, si le chemin d’accès est **réseau > server01 > sauvegardes de l’historique des fichiers**, cliquez sur **Server01**.  
  
5.  Cliquez sur un dossier de serveur, puis cliquez sur **propriétés**.  
  
6.  Cliquez sur le **sécurité** onglet.  
  
7.  Si **contrôle total** est ne pas autorisée pour le compte système, cliquez sur **modifier**, puis cliquez sur **système**. Sous **autorisations pour le système**, sélectionnez le **autoriser** case à cocher en regard de **contrôle total**.  
  
8.  Cliquez sur **OK** deux fois pour mettre à jour les autorisations et fermer **propriétés**.  
  
##  <a name="BKMK_ConnectorIssueNetFramework"></a>Problème 4  
 **Problème**  
  
 Obtenir de l’un à exécuter cette application, vous devez installer une des versions suivantes de .NETFramework: V4.5.50709» lors de la connexion d’un ordinateur au serveur  
  
 **Description**  
  
 Lorsque vous vous connectez un ordinateur à un serveur qui exécute WindowsServerEssentials ou WindowsServerEssentials, l’Assistant tente d’installer .NETFramework version4.5.50709 sur l’ordinateur. Toutefois, si une version antérieure de .NETFramework version4.5 est présente, la version mise à jour ne peut pas être installée et la tentative de connexion échoue avec ce message d’erreur: pour exécuter cette application, vous devez installer une des versions suivantes de .NETFramework: V4.5.50709. Contactez votre éditeur d’allocation pour obtenir des instructions sur l’obtention de la version appropriée de .NETFramework.  
  
 **Solution**  
  
 Désinstallez .NETFramework 4.5 de l’ordinateur, puis connectez l’ordinateur au serveur.  
  
#### <a name="to-uninstall-net-framework-45"></a>Pour désinstaller .NETFramework 4.5  
  
1.  Sur le **Démarrer** page de l’ordinateur client, ouvrez **le panneau de configuration**.  
  
2.  Dans le panneau de configuration, sous **programmes**, cliquez sur **désinstaller un programme**.  
  
3.  Avec le bouton droit **Microsoft .NETFramework 4.5**, puis cliquez sur **désinstaller**.  
  
4.  Après avoir désinstallé .NETFramework 4.5 avec succès, connectez l’ordinateur au serveur. La version appropriée de .NETFramework 4.5 est installée avec le logiciel du connecteur.  
  
##  <a name="BKMK_Time"></a>Problème 5  
 **Problème**  
  
 Obtenir un le serveur n’est pas disponible. Pour résoudre ce problème, contactez la personne responsable de votre réseau. Erreur lors de la connexion d’un ordinateur au serveur  
  
 **Description**  
  
 Cela peut se produire si la date et l’heure sur l’ordinateur connecté ne sont pas synchronisées avec la date et l’heure sur le serveur.  WindowsServerEssentials et WindowsServerEssentials utilisent le service de synchronisation de temps pour synchroniser la date et l’heure des ordinateurs en cours d’exécution dans un réseau de WindowsServerEssentials ou de WindowsServerEssentials. Heure synchronisée est essentielle, car le protocole d’authentification par défaut utilise l’heure du serveur dans le cadre du processus d’authentification. Par exemple, si l’horloge sur un ordinateur client n’est pas synchronisé à la date et l’heure, WindowsServerEssentials ou l’authentification WindowsServerEssentials peut tort interpréter une demande d’ouverture de session en tant qu’une tentative d’intrusion et de refuser l’accès à l’utilisateur.  
  
 Cela peut se produire si la mémoire libre s du serveur est inférieure à 5%.  
  
 Cela peut se produire si vous disposez déjà d’une connexion VPN à WindowsServerEssentials et que vous essayez de configurer le connecteur logicielles hors site à l’aide d’une adresse de domaine.  
  
 **Solution**  
  
1.  Synchroniser la date et l’heure sur l’ordinateur client avec celles sur le serveur. Connectez ensuite l’ordinateur au serveur.  
  
2.  Fermez certaines applications côté serveur, puis connectez l’ordinateur au serveur.  
  
3.  Fermer la connexion VPN, puis connectez l’ordinateur au serveur.  
  
#### <a name="to-change-the-date-and-time-on-the-client-computer"></a>Pour modifier la date et l’heure sur l’ordinateur client  
  
1.  Sur la page de démarrage de l’ordinateur client, ouvrez **le panneau de configuration**.  
  
2.  Dans le panneau de configuration, cliquez sur **horloge, langue et région**, puis cliquez sur **Date et heure**.  
  
3.  Cliquez sur **modifier la date et l’heure**, définir la date et l’heure à la date et l’heure, puis cliquez sur **OK**.  
  
4.  Cliquez sur **OK**, puis fermez le panneau de configuration.  
  
5.  Réessayez de connecter l’ordinateur client au serveur. Pour obtenir des instructions, consultez connecter des ordinateurs au serveur.  
  
 Si vous ne pouvez pas toujours vous connecter l’ordinateur client au serveur, assurez-vous que la date et l’heure sur le serveur sont correctes. Si la date et l’heure ne sont pas corrects, modifiez-les.  
  
#### <a name="to-change-the-date-and-time-on-the-server"></a>Pour modifier la date et l’heure sur le serveur  
  
1.  Ouvrez une session sur le serveur en utilisant le mot de passe que vous avez configuré pendant l’installation de WindowsServerEssentials ou WindowsServerEssentials et la configuration.  
  
    > [!NOTE]
    >  Si vous gérez le serveur à distance, vous devez vous connecter au serveur à l’aide de la connexion Bureau à distance.  
  
2.  Sur le **Démarrer** page, ouvrez **le panneau de configuration**.  
  
3.  Dans le panneau de configuration, cliquez sur **horloge, langue et région**, puis cliquez sur **Date et heure**.  
  
4.  Cliquez sur **modifier la date et l’heure**, définir la date et l’heure à la date et l’heure, puis cliquez sur **OK**.  
  
5.  Cliquez sur **OK**, puis fermez le panneau de configuration.  
  
6.  Sur l’ordinateur client, réessayez de connecter l’ordinateur client au serveur. Pour obtenir des instructions, consultez connecter des ordinateurs au serveur.  
  
##  <a name="BKMK_ServiceStopped"></a>Problème 6  
 **Problème**  
  
 Obtenir une une erreur inattendue s’est produite. Pour résoudre ce problème, contactez la personne responsable de votre réseau. Erreur lors de la connexion d’un ordinateur au serveur  
  
 **Description**  
  
 Si vous recevez ce message d’erreur, le Service Web de certificat WSS ne peut pas exécuter.  
  
 **Solution**  
  
 Démarrer le Service Web de certificat WSS.  
  
#### <a name="to-start-the-wss-certificate-web-service"></a>Pour démarrer le Service Web de certificat WSS  
  
1.  Sur du serveur **Démarrer** page, ouvrez **outils d’administration**.  
  
     Dans **outils d’administration**, avec le bouton droit **Gestionnaire des Services Internet (IIS)**, puis cliquez sur **ouvrir**.  
  
2.  Dans le volet de navigation, cliquez sur **Service Web certificat WSS**.  
  
3.  Dans le **Actions** volet, cliquez sur **Démarrer**.  
  
##  <a name="BKMK_ConnectorIssueReconnect"></a>Problème 7  
 **Problème**  
  
 Lorsque j’essaie de connecter un ordinateur au serveur après une tentative de connexion échoue, vous recevez l’avertissement Qu'un ordinateur portant ce nom est déjà connecté au serveur  
  
 **Description**  
  
 Si une tentative précédente de connecter un ordinateur au serveur a été annulée ou interrompue, l’avertissement suivant peut s’afficher lorsque vous essayez de vous connecter: un ordinateur portant ce nom est déjà connecté au serveur. Cela se produit, car un certificat a été émis lorsque vous avez tenté de se connecter au serveur la première fois.  
  
 **Solution** si vous êtes sûr qu’aucun autre ordinateur portant le même nom n’est déjà connecté au serveur, cliquez sur **suivant**, puis suivez les instructions pour terminer la **connecter mon ordinateur au serveur** Assistant.  
  
##  <a name="BKMK_JoinWin7"></a>Problème 8  
 **Problème**  
  
 Lorsque je tente de se connecter à un ordinateur client qui exécute Windows7 Édition familiale au serveur, la page web exécutant le logiciel Connecteur s’ouvre, mais l’ordinateur client ne peut pas se connecter au serveur  
  
 **Description**  
  
 Si le routeur sur votre réseau dispose de multidiffusion, les communications entre le serveur et un ordinateur client exécutant Windows7 Édition Familiale Basique ou Windows7 Édition familiale Premium ne fonctionnent pas correctement.  
  
 **Solution**  
  
 Désactiver la multidiffusion sur votre routeur. Sur certains routeurs, vous devrez peut-être également désactiver le protocole de routage RIP - 2M. Pour plus d’informations, reportez-vous à la documentation fournie par le fabricant du routeur.  
  
##  <a name="BKMK_ConnectorIssueAutologon"></a>Problème 9  
 **Problème**  
  
 Ouverture de session automatique ne fonctionne plus une fois que j’ai connecté l’ordinateur au serveur  
  
 **Description**  
  
 Si l’ouverture de session automatique est définie pour le compte d’utilisateur lorsque vous vous connectez l’ordinateur vers WindowsServerEssentials, le paramètre est remplacé lorsque le logiciel connecteur est installé sur l’ordinateur.  
  
 **Solution** pour résoudre ce problème, lorsque vous vous connectez l’ordinateur au serveur, notez le mot de passe est utilisé pour le compte d’utilisateur. Une fois le logiciel connecteur est installé, configurez l’ouverture de session automatique pour utiliser ce compte.  
  
> [!NOTE]
>  Le compte de domaine WindowsServerEssentials nécessite un mot de passe qui respecte les exigences de stratégie de mot de passe par défaut.  
  
##  <a name="BKMK_ConnectorIssueOldLogs"></a>Problème 10  
 **Problème**  
  
 Désinstallation d’une version préliminaire du logiciel connecteur ne supprime pas les journaux existants  
  
 **Description**  
  
 Une fois la mise à jour vers la version finale d’une version préliminaire (bêta ou RC) de WindowsServerEssentials, vous devez supprimer le logiciel Connecteur de chaque ordinateur qui a été connecté au serveur et puis connectez l’ordinateur pour installer la version finale du logiciel connecteur.  
  
 Toutefois, lorsque vous supprimez le logiciel du connecteur d’un ordinateur réseau, les fichiers journaux existants dans le dossier %ProgramData%\Microsoft\Windows Server\Logs\ sur cet ordinateur ne sont pas supprimés. Si vous ne supprimez pas le dossier Logs, les fichiers journaux peuvent être endommagés lorsque vous vous connectez l’ordinateur vers la version finale de WindowsServerEssentials.  
  
 **Solution** pour éviter l’endommagement des fichiers journaux, supprimez de manuellement le dossier journaux avant de reconnecter l’ordinateur client vers le serveur mis à jour.  
  
#### <a name="to-reconnect-a-computer-after-a-server-update-without-corrupting-the-log-files"></a>Pour reconnecter un ordinateur après un serveur de mise à jour sans altérer le journal des fichiers  
  
1.  Désinstaller le logiciel Connecteur de l’ordinateur client.  
  
2.  Supprimez le dossier Logs du dossier %ProgramData%\Microsoft\Windows Server\.  
  
3.  Reconnecter l’ordinateur au serveur. Qui installe la version finale du logiciel Connecteur, création d’un nouveau dossier journaux et les fichiers journaux.  
  
##  <a name="BKMK_UpgradeClientOS"></a>Problème 11  
 **Problème**  
  
 Je souhaite mettre à niveau le système d’exploitation sur un ordinateur client  
  
 **Description**  
  
 Pendant l’installation du logiciel Connecteur, les nombreuses vérifications sont effectuées sur le système d’exploitation client pour garantir que le client possède la configuration requise de connecteur. Si vous mettez à niveau le système d’exploitation client après avoir installé le logiciel Connecteur, certains des composants requis peuvent être absents et le connecteur client risque d’échouer.  
  
 **Solution**  
  
 Mettre à niveau votre système d’exploitation de client vers une version différente (par exemple, vous mettez à niveau WindowsXP vers WindowsVista ou WindowsVista vers Windows7), vous devez désinstaller le logiciel du connecteur. Utilisez **ajouter ou supprimer des programmes** dans le panneau de configuration. Une fois la mise à niveau du système d’exploitation client terminée, vous pouvez réinstaller le connecteur client en ouvrant http://&lt; *server*> /Connect dans un navigateur Web, où <*server*> est le nom du serveur WindowsServerEssentials.  
  
 Si vous avez déjà mis à niveau le client avec le logiciel Connecteur installé, utilisez **Ajout/Suppression de programmes** ou **programmes et fonctionnalités** pour désinstaller le logiciel connecteur. Puis réinstallez le logiciel du connecteur.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer WindowsServerEssentials](../manage/Manage-Windows-Server-Essentials.md)  
  
-   [Windows2012 Server Essentials dépannage des ordinateurs (TechNet Wiki)](https://social.technet.microsoft.com/wiki/contents/articles/14370.windows-2012-server-essentials-connectcomputer-troubleshooting.aspx)
