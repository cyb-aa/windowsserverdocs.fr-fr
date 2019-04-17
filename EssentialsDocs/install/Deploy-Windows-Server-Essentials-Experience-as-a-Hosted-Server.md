---
title: "Déployer l’expérience WindowsServerEssentials en tant que serveur hébergé"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a455c6b4-b29f-4f76-8c6b-1578b6537717
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c299d2b5f3d6b48693c473754a5205a7d26b5d6a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-windows-server-essentials-experience-as-a-hosted-server"></a>Déployer l’expérience WindowsServerEssentials en tant que serveur hébergé

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Ce document contient des informations spécifiques aux hébergeurs souhaitant déployer MicrosoftWindows Server16 avec le rôle expérience WindowsServerEssentials (appelé WindowsServerEssentials dans le reste du document) installé dans leur laboratoire et souhaitant proposer l’expérience WindowsServerEssentials en tant que service à leurs clients. Ce document comprend les sections suivantes:  
  

-   [Vue d’ensemble de l’expérience WindowsServerEssentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_WSEEOverview)  
  
-   [Avantages de l’hébergement d’expérience WindowsServerEssentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Benefits)  
  
-   [Options de déploiement pris en charge](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedDeployment)  
  
-   [Topologies réseau prises en charge](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedToplogy)  
  
-   [Personnaliser l’image du rôle expérience WindowsServerEssentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_CustomizeImage)  
  
-   [Automatiser le déploiement d’expérience WindowsServerEssentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_AutomateDeployment)  
  
-   [Migrer les données à partir de WindowsSmallBusinessServer vers l’expérience WindowsServerEssentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Migrate)  
  
-   [Effectuer des tâches courantes à l’aide de Windows PowerShell](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_PowerShell)  
  
-   [Intégration de messagerie avec WindowsServerEssentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_EmailIntegration)  
  
-   [Surveiller et gérer à l’aide des outils natifs](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Monitoring)  
  
-   [Scénarios de test](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Scenarios)  
  
-   [Informations de prise en charge](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Support)  

-   [Vue d’ensemble de l’expérience WindowsServerEssentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_WSEEOverview)  
  
-   [Avantages de l’hébergement d’expérience WindowsServerEssentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Benefits)  
  
-   [Options de déploiement pris en charge](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedDeployment)  
  
-   [Topologies réseau prises en charge](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedToplogy)  
  
-   [Personnaliser l’image du rôle expérience WindowsServerEssentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_CustomizeImage)  
  
-   [Automatiser le déploiement d’expérience WindowsServerEssentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_AutomateDeployment)  
  
-   [Migrer les données à partir de WindowsSmallBusinessServer vers l’expérience WindowsServerEssentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Migrate)  
  
-   [Effectuer des tâches courantes à l’aide de Windows PowerShell](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_PowerShell)  
  
-   [Intégration de messagerie avec WindowsServerEssentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_EmailIntegration)  
  
-   [Surveiller et gérer à l’aide des outils natifs](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Monitoring)  
  
-   [Scénarios de test](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Scenarios)  
  
-   [Informations de prise en charge](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Support)  

  
##  <a name="BKMK_WSEEOverview"></a>Vue d’ensemble de l’expérience WindowsServerEssentials  
 L’expérience WindowsServerEssentials est un rôle de serveur qui est disponible dans le Windows Server2012R2 Standard et Datacenter de Windows Server2012R2. Lorsque le rôle expérience WindowsServerEssentials est installé sur un serveur exécutant Windows Server2012R2, le client peut tirer parti de toutes les fonctionnalités qui sont disponibles dans WindowsServerEssentials sans les verrous et les limites. L’expérience WindowsServerEssentials permet les solutions intersites suivantes pour les petites et moyennes entreprises:  
  
-   **Stockage des données et protection** vous pouvez stocker le client» la données dans un emplacement centralisé et protéger les données de serveur et client en sauvegardant les ordinateurs serveurs et clients (moins de 75) au sein du réseau.  
  
-   **Gestion des utilisateurs** vous pouvez gérer les utilisateurs et les groupes par le biais du tableau de bord de serveur simplifié. En outre, l’intégration avec MicrosoftAzure ActiveDirectory (AD Azure) permet d’accéder facilement aux données pour Microsoft online services (par exemple, Office 365, Exchange Online et SharePoint Online) pour les utilisateurs à l’aide de leurs informations d’identification de domaine.  
  
-   **Intégration de service** vous pouvez intégrer le serveur à Microsoft online services (par exemple, Office 365, SharePoint Online et MicrosoftAzure Backup). Vous pouvez également intégrer le serveur à vos services ou les services fournis par des fournisseurs tiers.  
  
-   **Accès en tout lieu** le client peut accéder au serveur, les ordinateurs du réseau et données à partir de presque n’importe où ils ont une connexion Internet et à l’aide de presque n’importe quel appareil. Accès Web à distance leur permet d’accéder aux applications et des données avec une expérience de navigateur rationalisée et tactile. L’application mon serveur leur permet d’accéder aux données à partir d’un Windows Phone ou une application MicrosoftStore.  
  
-   **Diffusion multimédia en continu** si vous installez le package multimédia sur un serveur avec expérience WindowsServerEssentials activé, l’utilisateur final peut stocker des photos, musique et vidéo dans les dossiers partagés, puis accéder à ces fichiers multimédias à partir des ordinateurs du réseau ou accès Web à distance.  
  
-   **Contrôle d’intégrité** vous pouvez surveiller l’intégrité du réseau et obtenir des rapports d’intégrité personnalisés.  
  
##  <a name="BKMK_Benefits"></a>Avantages de l’hébergement d’expérience WindowsServerEssentials  
  Expérience WindowsServerEssentials est un rôle dans Windows Server, vous pouvez réutiliser l’infrastructure de gestion dans Windows Server pour déployer et configurer le rôle expérience WindowsServerEssentials et de déploiement existant. Qui héberge le rôle expérience WindowsServerEssentials offre les avantages suivants:  
  
-   **Rationalisation du déploiement** en activant simplement le rôle expérience WindowsServerEssentials, parmi les plus couramment utilisés des rôles et fonctionnalités sont activées et configurées avec les meilleures pratiques pour les petites et moyennes entreprises. Vous pouvez personnaliser les fonctionnalités de WindowsServerEssentials ou masquer certaines des fonctionnalités locales. Si vous utilisez WindowsAzurePack, vous pouvez télécharger le modèle de galerie pour l’expérience WindowsServerEssentials sur Windows Server2012R2.  
  
-   **Tableau de bord simplifié** le bord de WindowsServerEssentials simplifie les tâches courantes telles que la gestion des dossiers du serveur, stockage de serveur, sauvegarde et restauration, utilisateur ou les comptes de groupe, les périphériques, l’accès à distance et par courrier électronique. Petites et moyennes entreprises peuvent effectuer des tâches quotidiennes de gestion au lieu d’appeler le support technique pour le support technique.  
  
-   **Extensibilité** logiciel le bord de WindowsServerEssentials et le connecteur WindowsServerEssentials sont extensibles. Vous pouvez ajouter votre propre marque et intégration de service afin que vos clients disposent d’un point d’entrée pour tout ce qui concerne leur serveur et le service.  
  
-   **Moniteur** une nouvelle version du Pack d’analyse SystemCenter est disponible pour surveiller et gérer plusieurs serveurs exécutant WindowsServerEssentials. Pour télécharger le pack d’administration, voir [SystemCenter Management Pack pour WindowsServerEssentials](https://www.microsoft.com/download/details.aspx?id=40809).  
  
##  <a name="BKMK_SupportedDeployment"></a>Options de déploiement pris en charge  
  Expérience WindowsServerEssentials peut être déployé en tant que contrôleur de domaine dans un environnement ActiveDirectory; ou il peut être déployé dans un environnement ActiveDirectory existant en tant que membre du domaine.  
  
 Nous recommandons que vous tout d’abord déployez Windows Server2012R2 Standard ou Windows Server2012R2 Datacenter, puis installez le rôle expérience WindowsServerEssentials. Avec cette méthode de déploiement, vous obtenez toutes les fonctionnalités d’édition de WindowsServerEssentials, sans les verrous et les limites.  
  

 Pour plus d’informations sur l’installation de Windows Server2012R2 avec le rôle expérience WindowsServerEssentials, consultez [installer et configurer WindowsServerEssentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  


  
##  <a name="BKMK_SupportedToplogy"></a>Topologies réseau prises en charge  
 Pour utiliser l’expérience WindowsServerEssentials à partir d’un client itinérant, VPN doit être activé. Pour activer l’accès à distance le serveur à partir de clients itinérants, vous devez ouvrir le port 443 et 80 sur le serveur.  
  
 Voici les deux topologies de mise en réseau de côté serveur classiques, et comment le VPN et l’accès Web à distance peuvent être configuré:  
  
-   **Topologie 1** (il s’agit de la topologie par défaut, et elle place tous les serveurs et la plage IP VPN dans le même sous-réseau):  
  
    -   Configurer le serveur dans un réseau virtuel distinct sous un périphérique de traduction d’adresses réseau (NAT).  
  
    -   Activer le service DHCP sur le réseau virtuel, ou attribuer une adresse IP statique pour le serveur.  
  
    -   Transférez le port IP public 443 sur le routeur à l’adresse de réseau local du serveur.  
  
    -   Autoriser le relais VPN pour le port 443.  
  
    -   Définissez le pool d’adresses VPN IPv4 dans la même plage de sous-réseau que l’adresse du serveur.  
  
    -   Affectez au second serveur une adresse IP statique au sein du même sous-réseau, mais pas dans le pool d’adresses VPN.  
  
-   **Topologie 2**:  
  
    -   Attribuez au serveur une adresse IP privée.  
  
    -   Autorisez le Port 443 sur le serveur pour atteindre une adresse IP de port public 443.  
  
    -   Autoriser le relais VPN pour le port 443.  
  
    -   Affectez des plages différentes pour le pool d’adresses VPN IPv4 et l’adresse du serveur.  
  
 Avec la topologie 2, les deuxième scénarios de serveur ne sont pas pris en charge, car vous ne pouvez pas ajouter un autre serveur dans le même domaine.  
  
 Vous pouvez activer VPN pendant un déploiement sans assistance à l’aide de notre script Windows PowerShell, ou il peut être configuré avec l’Assistant après la configuration initiale.  
  
 Pour activer VPN à l’aide de Windows PowerShell, exécutez la commande suivante avec des privilèges d’administrateur sur le serveur exécutant WindowsServerEssentials et fournissez toutes les informations nécessaires.  
  
```  
##  
## To configure external domain and SSL certificate (if not yet done in unattended Initial Configuration).  
##  
  
$myExternalDomainName = 'remote.contoso.com';   ## corresponds to A or AAAA DNS record(s) that can be resolved on Internet and routed to the server  
$mySslCertificateFile = 'C:\ssl.pfx';   ## full path to SSL certificate file  
$mySslCertificatePassword = ConvertTo-SecureString  œAsPlainText  œForce '******';   ## password for private key of the SSL certificate  
$skipCertificateVerification = $true;   ## whether or not, skip verification for the SSL certificate  
  
Set-WssDomainNameConfiguration  œDomainName $myExternalDomainName  œCertificatePath $mySslCertificateFile  œCertificateFilePassword $mySslCertificatePassword  œNoCertificateVerification  
##  
## To install VPN with static IPv4 pool (and allow all existing users to establish VPN).  
##  
  
Install-WssVpnServer -IPv4AddressRange ('192.168.0.160','192.168.0.240') -ApplyToExistingUsers;  
  
```  
  
> [!NOTE]
>  Si vous ne pouvez pas fournir une connexion VPN avant que le client prenne possession du serveur, assurez-vous que le port serveur 3389 est accessible via Internet afin que le client puisse utiliser le protocole Bureau à distance pour se connecter au serveur et le configurer.  
  
##  <a name="BKMK_CustomizeImage"></a>Personnaliser l’image du rôle expérience WindowsServerEssentials  
 Vous pouvez personnaliser l’image avant de configurer le rôle expérience WindowsServerEssentials. Pour en savoir plus sur le processus standard de WindowsServerSysprep, consultez [Kit de déploiement et d’évaluation Windows](https://msdn.microsoft.com/library/hh825420.aspx). Après avoir préparé l’image à l’aide de Sysprep, vous pouvez l’utiliser ou la resceller dans Install.wim pour un nouveau déploiement.  
  
 Si vous utilisez Virtual Machine Manager, vous pouvez créer un modèle à l’aide de l’instance en cours d’exécution. Ce processus utilise Sysprep pour préparer l’instance, et il arrête l’ordinateur. Une fois que vous stockez le modèle dans votre bibliothèque, vous pouvez l’utiliser sur un cas par cas.  
  
 Après avoir installé le rôle expérience WindowsServerEssentials, vous pouvez personnaliser les fonctionnalités dans WindowsServerEssentials. Une des personnalisations plus importantes est la **IsHosted** clé de Registre: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\Deployment\IsHosted**.  
  
 Si cette clé est définie sur 0 x 1, certaines des fonctionnalités locales changent de comportement. Ces modifications sont les suivantes:  
  
-   **Sauvegarde des clients** sauvegarde du Client est désactivée par défaut pour les ordinateurs clients qui vient d’être joint.  
  
-   **Le Service de restauration** sera désactivé le Service de restauration, et l’interface utilisateur est masquée dans le tableau de bord.  
  
-   **Historique des fichiers** les paramètres de l’historique des fichiers pour les comptes d’utilisateur récemment créé ne seront pas gérés automatiquement par le serveur.  
  
-   **Sauvegarde du serveur** service de sauvegarde du serveur est désactivé, et l’interface utilisateur de la sauvegarde du serveur est masquée dans le tableau de bord.  
  
-   **Espaces de stockage** l’interface utilisateur pour la création ou la gestion des espaces de stockage est masquée dans le tableau de bord.  
  
-   **Accès en tout lieu** configuration VPN et de routeur est ignorée par défaut lorsque vous exécutez l’Assistant Configuration de n’importe quel endroit accès.  
  
 Si vous voulez contrôler le comportement de chaque fonctionnalité répertoriée, vous pouvez définir la clé de Registre correspondante pour chacune d’elles. Pour plus d’informations sur la façon de définir la clé de Registre, reportez-vous à la [personnaliser et déployer WindowsServerEssentials dans Windows Server2012R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
##  <a name="BKMK_AutomateDeployment"></a>Automatiser le déploiement d’expérience WindowsServerEssentials  
 Pour automatiser le déploiement, vous devez d’abord déployer le système d’exploitation, puis installer le rôle expérience WindowsServerEssentials.  
  
-   Pour déployer automatiquement Windows Server2012R2 Standard ou Windows Server2012R2 Datacenter, suivez les instructions de [Kit de déploiement et d’évaluation Windows](https://msdn.microsoft.com/library/hh825420.aspx).  
  
-   Pour savoir comment installer le rôle expérience WindowsServerEssentials à l’aide de Windows PowerShell, voir [installer et configurer WindowsServerEssentials](https://technet.microsoft.com/library/dn281793.aspx).  
  
> [!NOTE]
>  Assurez-vous que les paramètres de fuseau horaire de l’ordinateur virtuel hôte et l’expérience WindowsServerEssentials sont identiques. Dans le cas contraire, vous pouvez rencontrer plusieurs erreurs. Ceux-ci incluent: la configuration initiale du serveur n’est peut-être pas réussie sur les tâches liées aux certificats, le certificat peut ne pas fonctionne pendant quelques heures après le rôle expérience WindowsServerEssentials est installé, et informations sur le périphérique ne seront pas mises à jour correctement.  
  
 Après le déploiement, utilisez l’applet de commande Windows PowerShell **Get-WssConfigurationStatus** pour vérifier si la configuration initiale a réussi. L’état renvoyé doit être une des opérations suivantes: **Notstarted**, **FinishedWithWarning**, **en cours d’exécution**, **terminé**, **échec**, ou **PendingReboot**.  
  
 Le serveur est redémarré pendant la configuration initiale. Si vous avez besoin éviter ce redémarrage automatique, vous pouvez utiliser la commande suivante pour ajouter une clé de Registre avant de commencer la Configuration initiale:  
  
```  
New-ItemProperty "HKLM:\Software\Microsoft\Windows Server\Setup"Ã‚Â  -Name "WaitForReboot" -Value 1 -PropertyType "DWord" -Force -Confirm:$false  
  
```  
  
 Après le démarrage de la configuration initiale, vous pouvez utiliser **Get-WssConfigurationStatus** pour vérifier l’état de la configuration initiale, et lorsque l’état est **PendingReboot**, vous pouvez redémarrer votre serveur.  
  
##  <a name="BKMK_Migrate"></a>Migrer les données à partir de WindowsSmallBusinessServer vers l’expérience WindowsServerEssentials  
 Vous pouvez migrer les données à partir de serveurs exécutant WindowsSmallBusinessServer2011, WindowsSmallBusinessServer2008, WindowsSmallBusinessServer2003 ou WindowsServerEssentials vers le serveur exécutant WindowsServerEssentials. Passez en revue les [migrer vers WindowsServerEssentials](../migrate/Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md) migration guide pour 2migrations locale et procéder aux personnalisations nécessaires selon votre environnement d’hébergement.  
  
> [!NOTE]
>  Nous vous recommandons de placer le serveur source et le serveur de destination dans le même sous-réseau. Si ce n’est pas possible, vous devez vous assurer que:  
>   
>  -   Le serveur source et le serveur de destination peuvent accéder aux autres «la s les noms DNS internes.  
> -   Tous les ports nécessaires sont ouverts.  
  
 Après la migration, vous pouvez mettre à niveau vos licences pour supprimer les verrous et les limites. Pour plus d’informations, voir [Transition de WindowsServerEssentials vers Windows Server2012 Standard](https://technet.microsoft.com/library/jj247582.aspx).  
  
##  <a name="BKMK_PowerShell"></a>Effectuer des tâches courantes à l’aide de Windows PowerShell  
 Cette section décrit certaines des tâches courantes que vous pouvez effectuer à l’aide de Windows PowerShell.  
  
### <a name="enable-remote-web-access"></a>Activer l’accès Web à distance  
 **Syntaxe**:  
  
 Enable-WssRemoteWebAccess [-SkipRouter] [-DenyAccessByDefault] [-ApplyToExistingUsers]  
  
 **Exemple**:  
  
 $Enable-WssRemoteWebAccess œDenyAccessByDefault œApplyToExistingUsers  
  
 Cette commande Activer l’accès Web à distance avec le routeur configuré automatiquement et modifier les autorisations d’accès par défaut pour tous les utilisateurs existants.  
  
### <a name="add-user"></a>Ajouter un utilisateur  
 **Syntaxe**:  
  
 Add-WssUser [-Nom] < string\ > [-mot de passe] < securestring\ > [-AccessLevel < string\ > {utilisateur et #124; administrateur}] [-FirstName < string\ >] [-LastName < string\ >] [-AllowRemoteAccess] [-AllowVpnAccess] [< commonParameters\ >]  
  
 **Exemple**:  
  
 $password = ConvertTo-SecureString «Passw0rd!» -asplaintext œforce$ Add-WssUser-User2Test nom-mot de passe $password - Accesslevel administrateur - FirstName User2 - LastName tester  
  
 Cette commande ajoutera un administrateur nommé User2Test avec mot de passe Passw0rd!.  
  
### <a name="add-server-folder"></a>Ajouter un dossier serveur  
 **Syntaxe**:  
  
 Add-WssFolder [-Nom] < string\ > [-chemin d’accès] < string\ > [[-Description] < string\ >] [-KeepPermissions] [< commonParameters\ >]  
  
 **Exemple**:  
  
 $Ajouter-WssFolder-nom «MyTestFolder»-chemin d’accès «C:\ServerFolders\MyTestFolder»  
  
 Cette commande ajoutera un dossier du serveur nommé MyTestFolder à l’emplacement spécifié.  
  
##  <a name="BKMK_EmailIntegration"></a>Intégration de messagerie avec WindowsServerEssentials  
 Vous pouvez intégrer l’expérience WindowsServerEssentials à Office 365 ou le serveur Exchange hébergé. Si vous souhaitez que votre client à utiliser votre messagerie hébergée, vous devez développer un complément pour intégrer l’expérience WindowsServerEssentials à votre solution de messagerie hébergé. Pour plus d’informations, voir [SDK WindowsServerEssentials](https://msdn.microsoft.com/library/gg513877.aspx).  
  
##  <a name="BKMK_Monitoring"></a>Surveiller et gérer à l’aide des outils natifs  
 Cette section décrit les outils natifs disponibles dans Windows Server2012R2 pour surveiller et gérer le serveur.  
  
### <a name="group-policy"></a>Stratégie de groupe  
  Expérience WindowsServerEssentials tire parti de la prise en charge native à la stratégie de groupe dans Windows Server2012R2 et fournit une interface utilisateur pour configurer les paramètres de sécurité et de la Redirection de dossiers.  
  
> [!NOTE]
>  Dans un environnement hébergé, si la Redirection de dossiers pour un profil utilisateur est activée, il peut augmenter le temps aux utilisateurs finaux pour se connecter si la taille des données est importante.  
  
### <a name="system-center-monitoring-pack"></a>Pack d’analyse SystemCenter  
 Pack d’analyse SystemCenter pour les moniteurs d’expérience WindowsServerEssentials du système d’alerte d’intégrité pour vous aider à gérer un grand nombre de serveurs WindowsServerEssentials dédiés aux petites entreprises. Pour plus d’informations, voir [SystemCenter Management Pack pour WindowsServerEssentials](https://www.microsoft.com/download/details.aspx?id=40809).  
  
### <a name="backup-and-restore"></a>Sauvegarde et restauration  
  Windows Server2012R2 avec l’expérience WindowsServerEssentials vous permet de sauvegarder des ordinateurs serveurs et clients du réseau.  
  
#### <a name="server-backup"></a>Sauvegarde du serveur  
 WindowsServerEssentials prend en charge deux manières de sauvegarder le serveur: sauvegarde locale et sauvegarde externe. Vous pouvez personnaliser ces options si vous voulez déployer votre propre solution de sauvegarde du serveur.  
  
-   **Sauvegarde locale** vous permet d’effectuer des sauvegardes incrémentielles au niveau du bloc de façon régulière sur un disque distinct. En tant qu’hébergeur, vous pourriez attacher un disque dur virtuel à l’ordinateur virtuel exécutant WindowsServerEssentials, puis configurez la sauvegarde du serveur sur ce disque dur virtuel. Le disque dur virtuel doit se trouver sur un disque physique différent de l’ordinateur virtuel exécutant WindowsServerEssentials.  
  
    > [!NOTE]
    >  Si vous disposez d’autres solutions de sauvegarde pour vos ordinateurs virtuels et que vous ne souhaitez pas que vos utilisateurs voient la fonctionnalité de sauvegarde de serveur native de WindowsServerEssentials, vous pouvez désactiver et supprimer l’interface utilisateur correspondante du tableau de bord. Pour plus d’informations, voir la [personnaliser la sauvegarde du serveur](https://technet.microsoft.com/library/dn293413.aspx) section [personnaliser et déployer WindowsServerEssentials dans Windows Server2012R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
-   **Sauvegarde externe** vous permet de sauvegarder périodiquement les données de votre serveur à un service cloud. Vous pouvez télécharger et installer le MicrosoftAzure Backup intégration Module pour WindowsServerEssentials pour tirer parti de la sauvegarde Azure qui est fourni par Microsoft.  
  
     Pour plus d’informations, voir intégrer WindowsAzureBackup avec la section WindowsServerEssentials dans [Manage Server Backup](../manage/Manage-Server-Backup-in-Windows-Server-Essentials.md).  
  
     Si vous ou vos utilisateurs préfèrent un autre service cloud, vous devez envisager les éléments suivants:  
  
    -   Mettre à jour l’interface utilisateur du tableau de bord afin qu’elle fournisse un lien à votre service cloud préféré au lieu de la valeur par défaut Azure Backup.  
  
    -   (Facultatif) Développer un complément pour le tableau de bord configurer et gérer le service de sauvegarde basée sur le cloud.  
  
#### <a name="client-computer-backup"></a>Sauvegarde de l’ordinateur client  
 Expérience WindowsServerEssentials prend en charge deux types de sauvegarde des données client: complète sauvegarde du client et l’historique des fichiers.  
  
> [!NOTE]
>  Sauvegarde du client peut nuire aux performances, car les données doivent être transférés à partir du client vers le serveur VPN.  
  
##### <a name="full-client-backup"></a>Sauvegarde complète du client  
 Sauvegarde complète du client est activée par défaut pour tous les périphériques clients sont connectés au réseau WindowsServerEssentials. Il entièrement sauvegarde des informations système et des données pour le client et prend en charge la déduplication des données. Les données de sauvegarde seront stockées sur le serveur exécutant WindowsServerEssentials. Cela permet à un client ayant échoué récupérer des données à partir d’un point de sauvegarde précédent.  
  
 Certaines considérations pour la sauvegarde complète du client sont:  
  
-   **Performances** sauvegarde initiale peut prendre beaucoup de temps en raison de la quantité de données à charger.  
  
-   **Stabilité** la connexion Internet n’est parfois pas stable côté client. Sauvegarde du client est conçue pour reprendre automatiquement et la base de données de sauvegarde client crée un point de contrôle chaque fois que 40Go de données sont sauvegardée. Vous pouvez modifier cette valeur et la réduire si vous pensez que la connexion Internet n’est pas fiable.  
  
    -   Pour activer une tâche de point de contrôle: sur le serveur, définissez la clé de Registre **HKLM\Software\Microsoft\Windows Server\Backup\GetCheckPointJobs** sur 1.  
  
    -   Pour modifier le seuil de point de contrôle: sur le client, remplacez **HKLM\Software\Microsoft\Windows Server\Backup\CheckPointThreshold** à partir de sa valeur par défaut de 40Go.  
  
-   **Restauration complète du client** l’environnement de préinstallation Windows ne prenant pas en charge une connexion VPN, restauration complète du client n’est pas pris en charge. Vous devez masquer la tâche Service de restauration du Client en suivant les étapes de [personnaliser et déployer WindowsServerEssentials dans Windows Server2012R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
##### <a name="file-history"></a>Historique des fichiers  
 Historique des fichiers est une fonctionnalité de Windows8.1 et Windows8 pour la sauvegarde des données de profil (bibliothèques, bureau, Contacts, Favoris) sur un partage réseau. Vous pouvez gérer de manière centralisée le paramètre de l’historique des fichiers de tous les ordinateurs exécutant Windows8.1 ou Windows8 qui appartiennent à un réseau WindowsServerEssentials. Les données de sauvegarde sont stockées sur le serveur exécutant Windows Server Essentials. Vous devez masquer la tâche Service de restauration du Client en suivant les étapes de [personnaliser et déployer WindowsServerEssentials dans Windows Server2012R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
### <a name="storage-management"></a>Gestion du stockage  
 Espaces de stockage permet d’agréger la capacité de stockage physique de différents disques durs, dynamiquement ajouter des disques durs et de créer des volumes de données avec des niveaux de résilience. Vous pouvez le faire sur l’ordinateur hôte ou sur l’ordinateur virtuel. Si vous souhaitez masquer cette fonctionnalité sur un ordinateur virtuel exécutant WindowsServerEssentials, suivez les instructions de [personnaliser et déployer WindowsServerEssentials dans Windows Server2012R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
##  <a name="BKMK_Scenarios"></a>Scénarios de test  
 À partir de la perspective d’hébergement, nous vous recommandons de tester les scénarios suivants:  
  

-   [Déploiement de serveur](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerDeploy)  
  
-   [Configuration du serveur](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerConfig2)  
  
-   [Gestion de serveur](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerManage)  
  
-   [Expérience du client](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ClientXP)  

  
###  <a name="BKMK_ServerDeploy"></a>Déploiement de serveur  
 Vous pouvez tester les scénarios de déploiement de serveur suivants:  
  
-   Déployer un serveur exécutant Windows Server2012R2 en tant que contrôleur de domaine dans votre environnement de laboratoire, puis installer le rôle expérience WindowsServerEssentials.  
  
-   Déployer un serveur exécutant Windows Server2012R2 dans votre environnement de laboratoire, joindre ce serveur à un domaine existant, puis installer le rôle expérience WindowsServerEssentials.  
  
-   Personnaliser l’image de Windows Server Essentials en fonction des besoins.  
  
-   Automatiser le déploiement de WindowsServerEssentials avec le fichier sans assistance et Windows PowerShell.  
  
-   Migrer des serveurs locaux qui exécutent WindowsSmallBusinessServer vers WindowsServerEssentials hébergé.  
  
###  <a name="BKMK_ServerConfig2"></a>Configuration du serveur  
 Vous pouvez tester les scénarios de configuration de serveur suivants:  
  
-   Configurer l’accès en tout lieu (accès Web à distance, réseau privé virtuel et DirectAccess).  
  
-   Configuration du stockage et des dossiers du serveur.  
  
-   Activer BranchCache.  
  
-   (Le cas échéant) Configurer la sauvegarde du serveur, la sauvegarde en ligne, la sauvegarde Client et l’historique des fichiers.  
  
-   (Le cas échéant) Configurer et gérer les espaces de stockage.  
  
-   (Le cas échéant) Configurer l’intégration de solution de messagerie (Office 365 et Exchange Server hébergé).  
  
-   (Le cas échéant) Configurer l’intégration avec d’autres services en ligne Microsoft.  
  
-   (Le cas échéant) Configurer le serveur multimédia.  
  
###  <a name="BKMK_ServerManage"></a>Gestion de serveur  
 Vous pouvez tester les scénarios de gestion de serveur suivants:  
  
-   Gérer les utilisateurs et groupes.  
  
-   Configurer et recevoir des notifications par courrier électronique des alertes.  
  
-   Exécutez l’outil Best Practices Analyzer pour voir s’il existe une erreur ou un message d’avertissement.  
  
-   Configurer le pack de SystemCenter Operations Manager.  
  
-   Configurer la récupération de serveur, en cas d’altération du système d’exploitation.  
  
###  <a name="BKMK_ClientXP"></a>Expérience du client  
 Vous pouvez tester les scénarios utilisateur final suivants:  
  
-   Déployer les clients sur Internet (systèmes d’exploitation PC ou Mac).  
  
-   Utilisation de Launchpad pour accéder aux dossiers partagés sur le client.  
  
-   Accès des ressources du serveur via l’accès Web à distance depuis différents périphériques (PC, téléphone et tablette).  
  
-   Accéder à mon application Server pour Windows Phone.  
  
-   (Le cas échéant) L’historique des fichiers de l’accès Client sauvegarde et restauration et la Redirection de dossiers.  
  
-   (Le cas échéant) Vérifiez l’expérience d’intégration par courrier électronique.  
  
##  <a name="BKMK_Support"></a>Informations de prise en charge  
 Vous pouvez télécharger le Kit de développement logiciel (SDK) WindowsServerEssentials et l’évaluation de WindowsServerEssentials et le Kit de déploiement (ADK):  
  
-   [Kit de développement logiciel WindowsServerEssentials](https://msdn.microsoft.com/library/gg513877.aspx)SDK  
  
-   [Personnaliser et déployer WindowsServerEssentials dans Windows Server2012R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Quelles sont les nouveautés dans WindowsServerEssentials](../get-started/what-s-new.md)  

-   [Installer WindowsServerEssentials](Install-Windows-Server-Essentials.md)  

-   [Prise en main WindowsServerEssentials](../get-started/get-started.md)
