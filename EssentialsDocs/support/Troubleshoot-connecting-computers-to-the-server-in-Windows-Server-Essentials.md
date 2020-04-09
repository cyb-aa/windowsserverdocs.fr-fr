---
title: Résolution des problèmes de connexion d'ordinateurs au serveur dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: e45b3d89-c057-4c70-a627-86fb06dd22aa
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 2d98d4cc561a3c29ce73455f38f787709149d056
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852192"
---
# <a name="troubleshoot-connecting-computers-to-the-server-in-windows-server-essentials"></a>Résolution des problèmes de connexion d'ordinateurs au serveur dans Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials 
  
 Cette rubrique contient des conseils de dépannage pour les problèmes que vous pouvez rencontrer lorsque vous connectez un ordinateur au serveur qui exécute Windows Server Essentials ou Windows Server Essentials.  
  
> [!NOTE]
>  Pour obtenir les informations les plus récentes sur la résolution des problèmes de la communauté Windows Server Essentials et Windows Server Essentials, nous vous suggérons de visiter le [Forum Windows Server Essentials](https://social.technet.microsoft.com/Forums/windowsserver/home?forum=winserveressentials). Le forum Windows Server Essentials est le lieu idéal pour rechercher de l'aide ou pour poser une question.  
  
 Cette rubrique fournit des solutions aux problèmes suivants :  
  

-   Problème 1 : [problème 1](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BMRK_Package)  
  
-   Problème 2 : [problème 2](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2)  
  
-   Problème 3 : [problème 3](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2a)  
  
-   Problème 4 : [numéro 4](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueNetFramework)  
  
-   Problème 5 : [problème 5](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_Time)  
  
-   Problème 6 : [problème 6](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ServiceStopped)  
  
-   Problème 7 : [problème 7](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueReconnect)  
  
-   Problème 8 : [problème 8](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_JoinWin7)  
  
-   Problème 9 : [problème 9](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueAutologon)  
  
-   Problème 10 : [problème 10](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueOldLogs)  
  
-   Problème 11 : [problème 11](Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_UpgradeClientOS)  

-   Problème 1 : [problème 1](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BMRK_Package)  
  
-   Problème 2 : [problème 2](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2)  
  
-   Problème 3 : [problème 3](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssue2a)  
  
-   Problème 4 : [numéro 4](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueNetFramework)  
  
-   Problème 5 : [problème 5](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_Time)  
  
-   Problème 6 : [problème 6](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ServiceStopped)  
  
-   Problème 7 : [problème 7](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueReconnect)  
  
-   Problème 8 : [problème 8](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_JoinWin7)  
  
-   Problème 9 : [problème 9](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueAutologon)  
  
-   Problème 10 : [problème 10](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_ConnectorIssueOldLogs)  
  
-   Problème 11 : [problème 11](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md#BKMK_UpgradeClientOS)  

  
##  <a name="issue-1"></a><a name="BMRK_Package"></a>Problème 1  
 **Problème**  
  
 L’installation du package a échoué. Réessayez d’installer le connecteur Windows Server Essentials. Si le problème persiste, contactez votre administrateur réseau lors de la connexion d’un ordinateur au serveur.  
  
 **Description**  
  
 Ce problème peut se produire si vous connectez un ordinateur à un serveur qui exécute Windows Server Essentials alors que d’autres mises à jour Windows ou installations d’applications sont en attente et que l’installation du connecteur est annulée.  
  
 **Solution**  
  
 Effectuez toutes les autres mises à jour ou installations d’applications. Redémarrez les ordinateurs si vous y êtes invité.  
  
##  <a name="issue-2"></a><a name="BKMK_ConnectorIssue2"></a>Problème 2  
 **Problème**  
  
 Impossible de joindre un ordinateur à Windows Server Essentials  
  
 **Description**  
  
 Les ordinateurs qui comportent des caractères non-ASCII dans le nom de l’ordinateur ne peuvent pas être joints à Windows Server Essentials. Si le nom de l’ordinateur comprend des caractères non-ASCII, le message d’erreur « Une erreur inattendue s’est produite » s’affiche.  
  
 **Solution**  
  
 Renommez votre ordinateur client avec un nom qui contient uniquement des caractères ASCII, puis essayez de nouveau d’ajouter l’ordinateur à Windows Server Essentials.  
  
##  <a name="issue-3"></a><a name="BKMK_ConnectorIssue2a"></a>Problème 3  
 **Problème**  
  
 J’obtiens une erreur indiquant que l’installation du logiciel connecteur est annulée lors de la connexion d’un ordinateur au serveur  
  
 **Description**  
  
 Pour pouvoir connecter un ordinateur au serveur, le compte système doit disposer d’autorisations contrôle total sur les dossiers serveur affichés dans le tableau de bord Windows Server Essentials. Si les autorisations requises n’ont pas été accordées, vous recevez le message d’erreur « L’installation du logiciel Connecteur est annulée » .  
  
 **Solution**  
  
 Accordez au compte système un **contrôle total** sur chaque dossier de serveur.  
  
#### <a name="to-grant-the-system-account-full-control-permissions-on-a-server-folder"></a>Pour accorder au compte système des autorisations Contrôle total sur un dossier de serveur  
  
1.  Ouvrez le tableau de bord Windows Server Essentials.  
  
2.  Cliquez sur **Stockage**, puis sur **Dossiers du serveur**.  
  
3.  Cliquez avec le bouton droit sur le dossier de serveur, puis cliquez sur **Ouvrir le dossier**. (Si l’option **Ouvrir le dossier** n’est pas disponible, vous n’avez pas besoin de définir des autorisations sur le dossier.)  
  
4.  Dans le chemin d’accès réseau en haut de l’Explorateur Windows, cliquez sur le partage de serveurs pour afficher les dossiers partagés sur le serveur. Par exemple, si le chemin d’accès est **Réseau > Server01 > Sauvegardes de l’Historique des fichiers**, cliquez sur **Server01**.  
  
5.  Cliquez avec le bouton droit sur le dossier de serveur et cliquez sur **Propriétés**.  
  
6.  Cliquez sur l'onglet **Security**.  
  
7.  Si un **Contrôle total** n’est pas autorisé pour le compte système, cliquez sur **Modifier**, puis sur **Système**. Sous **Autorisations pour le compte système**, sélectionnez la case à cocher **Autoriser** en regard de **Contrôle total**.  
  
8.  Cliquez sur **OK** deux fois pour mettre à jour les autorisations et fermer **Propriétés**.  
  
##  <a name="issue-4"></a><a name="BKMK_ConnectorIssueNetFramework"></a>Problème 4  
 **Problème**  
  
 J’obtiens un pour exécuter cette application, vous devez installer l’une des versions suivantes de l’erreur .NET Framework : V 4.5.50709» lors de la connexion d’un ordinateur au serveur  
  
 **Description**  
  
 Lorsque vous connectez un ordinateur à un serveur qui exécute Windows Server Essentials ou Windows Server Essentials, l’Assistant tente d’installer .NET Framework version 4.5.50709 sur l’ordinateur. Toutefois, si une version antérieure de .NET Framework version 4,5 est présente, la version mise à jour ne peut pas être installée et la tentative de connexion échoue avec le message d’erreur suivant : pour exécuter cette application, vous devez installer l’une des versions suivantes du .NET Framework : V 4.5.50709. Pour obtenir des instructions sur l’obtention de la version appropriée du .NET Framework, contactez votre éditeur d’allocation.  
  
 **Solution**  
  
 Désinstallez .NET Framework 4.5 de l’ordinateur, puis connectez l’ordinateur au serveur.  
  
#### <a name="to-uninstall-net-framework-45"></a>Pour désinstaller .NET Framework 4.5  
  
1.  Dans la page **Démarrer** de l’ordinateur client, ouvrez le **Panneau de configuration**.  
  
2.  Dans le panneau de configuration, sous **Programmes**, cliquez sur **Désinstaller un programme**.  
  
3.  Cliquez avec le bouton droit sur **Microsoft .NET Framework 4.5**, puis cliquez sur **Désinstaller**.  
  
4.  Après avoir désinstallé .NET Framework 4.5, connectez l’ordinateur au serveur. La version appropriée de .NET Framework 4.5 est installée avec le logiciel du connecteur.  
  
##  <a name="issue-5"></a><a name="BKMK_Time"></a>Problème 5  
 **Problème**  
  
 J’obtiens un serveur qui n’est pas disponible. Pour résoudre ce problème, contactez la personne responsable de votre réseau. lors de la connexion d’un ordinateur au serveur  
  
 **Description**  
  
 Cela peut se produire si la date et l’heure de l’ordinateur connecté ne sont pas synchronisées avec celles du serveur.  Windows Server Essentials et Windows Server Essentials utilisent le service de synchronisation d’heure pour synchroniser la date et l’heure des ordinateurs s’exécutant sur un réseau Windows Server Essentials ou Windows Server Essentials. Il s’agit d’une opération cruciale, car le protocole d’authentification par défaut utilise l’heure du serveur dans le cadre du processus d’authentification. Par exemple, si l’horloge d’un ordinateur client n’est pas synchronisée avec la date et l’heure correctes, l’authentification Windows Server Essentials ou Windows Server Essentials peut interpréter de façon erronée une demande d’ouverture de session comme une tentative d’intrusion et refuser l’accès à l’utilisateur.  
  
 Cela peut se produire si la mémoire disponible du serveur est inférieure à 5%.  
  
 Cela peut se produire si vous disposez déjà d’une connexion VPN à Windows Server Essentials et que vous essayez de configurer le logiciel Connecteur hors site à l’aide d’une adresse de domaine.  
  
 **Solution**  
  
1.  Synchronisez la date et l’heure de l’ordinateur client avec celles du serveur. Connectez ensuite l’ordinateur au serveur.  
  
2.  Fermez certaines applications côté serveur, puis connectez l’ordinateur au serveur.  
  
3.  Fermez la connexion VPN, puis connectez l’ordinateur au serveur.  
  
#### <a name="to-change-the-date-and-time-on-the-client-computer"></a>Pour modifier la date et l’heure sur l’ordinateur client  
  
1. Dans la page Démarrer de l’ordinateur client, ouvrez le **Panneau de configuration**.  
  
2. Dans le Panneau de configuration, cliquez sur **Horloge, langue et région**, puis sur **Date et heure**.  
  
3. Cliquez sur **Modifier la date et l’heure**, définissez la date et l’heure adéquates, puis cliquez sur **OK**.  
  
4. Cliquez sur **OK**, puis fermez le Panneau de configuration.  
  
5. Réessayez de connecter l’ordinateur client au serveur. Pour obtenir des instructions, consultez Connecter des ordinateurs au serveur.  
  
   Si vous ne pouvez toujours pas connecter l’ordinateur client au serveur, assurez-vous que la date et l’heure du serveur sont correctes. Si elles sont erronées, modifiez-les.  
  
#### <a name="to-change-the-date-and-time-on-the-server"></a>Pour modifier la date et l’heure du serveur  
  
1.  Connectez-vous au serveur à l’aide du mot de passe que vous avez configuré lors de l’installation et de la configuration de Windows Server Essentials ou Windows Server Essentials.  
  
    > [!NOTE]
    >  Si vous gérez le serveur à distance, vous devez utiliser Connexion Bureau à distance pour vous connecter au serveur.  
  
2.  Dans la page de **démarrage**, ouvrez le **Panneau de configuration**.  
  
3.  Dans le Panneau de configuration, cliquez sur **Horloge, langue et région**, puis sur **Date et heure**.  
  
4.  Cliquez sur **Modifier la date et l’heure**, définissez la date et l’heure adéquates, puis cliquez sur **OK**.  
  
5.  Cliquez sur **OK**, puis fermez le Panneau de configuration.  
  
6.  Sur l’ordinateur client, réessayez de connecter l’ordinateur client au serveur. Pour obtenir des instructions, consultez Connecter des ordinateurs au serveur.  
  
##  <a name="issue-6"></a><a name="BKMK_ServiceStopped"></a>Problème 6  
 **Problème**  
  
 J’obtiens une erreur inattendue s’est produite. Pour résoudre ce problème, contactez la personne responsable de votre réseau. lors de la connexion d’un ordinateur au serveur  
  
 **Description**  
  
 Si vous recevez ce message d’erreur, cela est peut-être dû au fait que le service web Certificat WSS ne s’exécute pas.  
  
 **Solution**  
  
 Démarrez le service web Certificat WSS.  
  
#### <a name="to-start-the-wss-certificate-web-service"></a>Pour démarrer le service web Certificat WSS  
  
1.  Sur la page **Démarrer** du serveur, ouvrez **Outils d’administration**.  
  
     Dans **Outils d’administration**, cliquez avec le bouton droit sur **Gestionnaire des services Internet (IIS)** , puis cliquez sur **Ouvrir**.  
  
2.  Dans le volet de navigation, cliquez sur **Service Web de certificat WSS**.  
  
3.  Dans le volet **Actions**, cliquez sur **Démarrer**.  
  
##  <a name="issue-7"></a><a name="BKMK_ConnectorIssueReconnect"></a>Problème 7  
 **Problème**  
  
 Lorsque j’essaie de connecter un ordinateur au serveur après une tentative de connexion ayant échoué, je reçois l’avertissement un ordinateur portant ce nom est déjà connecté au serveur  
  
 **Description**  
  
 Si une tentative précédente de connexion d’un ordinateur au serveur a été annulée ou interrompue, l’avertissement suivant peut s’afficher lorsque vous essayez de vous connecter à nouveau : un ordinateur portant ce nom est déjà connecté au serveur. Cela se produit, car un certificat a été émis lorsque vous avez essayé de vous connecter au serveur la première fois.  
  
 **Solution** Si vous êtes sûr qu’aucun autre ordinateur portant le même nom n’est déjà connecté au serveur, cliquez sur **Suivant**, puis suivez les instructions pour terminer l’Assistant **Connecter un ordinateur au serveur**.  
  
##  <a name="issue-8"></a><a name="BKMK_JoinWin7"></a>Problème 8  
 **Problème**  
  
 Lorsque je tente de connecter un ordinateur client qui exécute Windows 7 Édition familiale au serveur, la page web d’exécution du logiciel Connecteur s’ouvre, mais l’ordinateur client ne peut pas se connecter au serveur  
  
 **Description**  
  
 Si le routeur de votre réseau dispose de la multidiffusion, les communications entre le serveur et un ordinateur client exécutant Windows 7 Édition Familiale Basique ou Windows 7 Édition Familiale Premium ne fonctionnent pas correctement.  
  
 **Solution**  
  
 Désactivez la multidiffusion sur votre routeur. Sur certains routeurs, vous devrez peut-être également désactiver le protocole de routage RIP-2M. Pour plus d’informations, reportez-vous à la documentation du fournisseur du routeur.  
  
##  <a name="issue-9"></a><a name="BKMK_ConnectorIssueAutologon"></a>Problème 9  
 **Problème**  
  
 L’ouverture de session automatique ne fonctionne plus une fois que j’ai connecté l’ordinateur au serveur  
  
 **Description**  
  
 Si l’ouverture de session automatique est définie pour le compte d’utilisateur lorsque vous connectez l’ordinateur à Windows Server Essentials, le paramètre est remplacé lorsque le logiciel connecteur est installé sur l’ordinateur.  
  
 **Solution** Pour résoudre ce problème, lorsque vous connectez l’ordinateur au serveur, notez le mot de passe utilisé pour le compte d’utilisateur. Une fois le logiciel du connecteur installé, configurez l’ouverture de session automatique pour utiliser ce compte.  
  
> [!NOTE]
>  Le compte de domaine Windows Server Essentials requiert un mot de passe qui répond aux critères de la stratégie de mot de passe par défaut.  
  
##  <a name="issue-10"></a><a name="BKMK_ConnectorIssueOldLogs"></a>Problème 10  
 **Problème**  
  
 La désinstallation d’une version préliminaire du logiciel de connecteur ne supprime pas les journaux existants  
  
 **Description**  
  
 Après la mise à jour d’une version préliminaire (bêta ou RC) de Windows Server Essentials vers la version finale, vous devez supprimer le logiciel connecteur de chaque ordinateur connecté au serveur, puis reconnecter l’ordinateur pour installer le version du logiciel connecteur.  
  
 Toutefois, lorsque vous supprimez le logiciel de connecteur d’un ordinateur réseau, les fichiers journaux existants dans le dossier %ProgramData%\Microsoft\Windows Server\Logs\ sur cet ordinateur ne sont pas supprimés. Si vous ne supprimez pas le dossier logs, les fichiers journaux peuvent être endommagés lorsque vous connectez l’ordinateur à la version finale de Windows Server Essentials.  
  
 **Solution** Pour éviter l’endommagement des fichiers journaux, supprimez manuellement le dossier Journaux avant de reconnecter l’ordinateur client au serveur mis à jour.  
  
#### <a name="to-reconnect-a-computer-after-a-server-update-without-corrupting-the-log-files"></a>Pour reconnecter un ordinateur après la mise à jour d’un serveur sans altérer les fichiers journaux  
  
1.  Désinstallez le logiciel Connecteur de l’ordinateur client.  
  
2.  Supprimez le dossier Journaux du dossier %ProgramData%\Microsoft\Windows Server\.  
  
3.  Reconnectez l'ordinateur au serveur. Cette opération installe la version finale du logiciel de connecteur et créé un nouveau dossier Journaux et de nouveaux fichiers journaux.  
  
##  <a name="issue-11"></a><a name="BKMK_UpgradeClientOS"></a>Problème 11  
 **Problème**  
  
 Je souhaite mettre à niveau le système d’exploitation sur un ordinateur client  
  
 **Description**  
  
 Pendant l’installation du logiciel de connecteur, de nombreuses vérifications sont effectuées sur le système d’exploitation client pour garantir que le client possède la configuration requise. Si vous mettez à niveau le système d’exploitation client après avoir installé le logiciel du connecteur, certains des composants requis peuvent être absents et le connecteur client risque d’échouer.  
  
 **Solution**  
  
 Avant de mettre à niveau votre système d’exploitation client vers une version différente (par exemple, Windows XP vers Windows Vista ou Windows Vista vers Windows 7), vous devez désinstaller le logiciel du connecteur. Dans le Panneau de configuration, ouvrez **Ajout/Suppression de programmes**. Une fois la mise à niveau du système d’exploitation client terminée, vous pouvez réinstaller le connecteur client en ouvrant http://<*server*>/Connect dans un navigateur Web, où <*Server*> est le nom du serveur Windows Server Essentials.  
  
 Si vous avez déjà mis à niveau le client avec le logiciel de connecteur installé, utilisez **Ajout/Suppression de programmes** ou **Programmes et fonctionnalités** pour désinstaller le logiciel du connecteur. Réinstallez ensuite le logiciel du connecteur.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer Windows Server Essentials](../manage/Manage-Windows-Server-Essentials.md)  
  
-   [Résolution des problèmes liés à Windows 2012 Server Essentials dépannage (TechNet wiki)](https://social.technet.microsoft.com/wiki/contents/articles/14370.windows-2012-server-essentials-connectcomputer-troubleshooting.aspx)
