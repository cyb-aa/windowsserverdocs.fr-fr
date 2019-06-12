---
title: Déployer le rôle Expérience Windows Server Essentials en tant que serveur hébergé
description: Décrit comment utiliser Windows Server Essentials
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
ms.openlocfilehash: 94d4040b65a63fe64e5d49d55f82c4deead5a121
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433585"
---
# <a name="deploy-windows-server-essentials-experience-as-a-hosted-server"></a>Déployer le rôle Expérience Windows Server Essentials en tant que serveur hébergé

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Ce document inclut des informations spécifiques aux hébergeurs souhaitant déployer Microsoft Windows Server 16 avec le rôle expérience Windows Server Essentials (appelé Windows Server Essentials dans le reste du document) installé dans leur laboratoire et l’intention de proposer l’expérience Windows Server Essentials en tant que service à leurs clients. Ce document comprend les sections suivantes :  
  

-   [Vue d’ensemble de l’expérience Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_WSEEOverview)  
  
-   [Avantages de l’hébergement d’expérience Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Benefits)  
  
-   [Options de déploiement pris en charge](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedDeployment)  
  
-   [Topologies de réseau prises en charge](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedToplogy)  
  
-   [Personnaliser l’image du rôle expérience Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_CustomizeImage)  
  
-   [Automatiser le déploiement de l’expérience Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_AutomateDeployment)  
  
-   [Migrer des données à partir de Windows Small Business Server vers Windows Server Essentials Experience](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Migrate)  
  
-   [Effectuer des tâches courantes à l’aide de Windows PowerShell](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_PowerShell)  
  
-   [Intégration de messagerie avec Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_EmailIntegration)  
  
-   [Surveiller et gérer à l’aide des outils natifs](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Monitoring)  
  
-   [Scénarios de test](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Scenarios)  
  
-   [Informations de prise en charge](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Support)  

-   [Vue d’ensemble de l’expérience Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_WSEEOverview)  
  
-   [Avantages de l’hébergement d’expérience Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Benefits)  
  
-   [Options de déploiement pris en charge](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedDeployment)  
  
-   [Topologies de réseau prises en charge](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_SupportedToplogy)  
  
-   [Personnaliser l’image du rôle expérience Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_CustomizeImage)  
  
-   [Automatiser le déploiement de l’expérience Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_AutomateDeployment)  
  
-   [Migrer des données à partir de Windows Small Business Server vers Windows Server Essentials Experience](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Migrate)  
  
-   [Effectuer des tâches courantes à l’aide de Windows PowerShell](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_PowerShell)  
  
-   [Intégration de messagerie avec Windows Server Essentials](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_EmailIntegration)  
  
-   [Surveiller et gérer à l’aide des outils natifs](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Monitoring)  
  
-   [Scénarios de test](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Scenarios)  
  
-   [Informations de prise en charge](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_Support)  

  
##  <a name="BKMK_WSEEOverview"></a> Vue d’ensemble de l’expérience Windows Server Essentials  
 L’expérience Windows Server Essentials est un rôle de serveur qui est disponible dans le Standard de Windows Server 2012 R2 et Windows Server 2012 R2 Datacenter. Lorsque le rôle expérience Windows Server Essentials est installé sur un serveur exécutant Windows Server 2012 R2, le client peut tirer parti de toutes les fonctionnalités qui sont disponibles dans Windows Server Essentials sans les verrous et les limites. L’expérience Windows Server Essentials permet les solutions intersites suivantes pour les petites et moyennes entreprises :  
  
-   **Stockage de données et la protection** vous pouvez stocker le client « données de s ¢ dans un emplacement centralisé et protéger les données de serveur et client en sauvegardant les ordinateurs serveurs et clients (moins de 75) au sein du réseau.  
  
-   **Gestion des utilisateurs** Vous pouvez gérer les utilisateurs et les groupes par le biais du tableau de bord de serveur simplifié. En outre, l’intégration avec Microsoft Azure Active Directory (Azure AD) permet d’accéder facilement aux données pour Microsoft online services (par exemple, Office 365, Exchange Online et SharePoint Online) pour les utilisateurs à l’aide de leurs informations d’identification de domaine.  
  
-   **Intégration de service** vous pouvez intégrer le serveur avec les services en ligne Microsoft (par exemple, Office 365, SharePoint Online et Microsoft Azure Backup). Vous pouvez aussi intégrer le serveur à vos services ou à des services fournis par des tiers.  
  
-   **Accès en tout lieu** Le client peut accéder au serveur, aux ordinateurs du réseau et aux données à partir de presque n'importe quel emplacement disposant d'une connexion Internet et à l'aide de presque n'importe quel périphérique. L'accès web à distance leur permet d'accéder aux applications et aux données avec une expérience de navigateur rationalisée et tactile. L’application mon serveur leur permet d’accéder aux données à partir d’un Windows Phone ou d’une application Microsoft Store.  
  
-   **Diffusion multimédia en continu** si vous installez le package multimédia sur un serveur avec expérience Windows Server Essentials activé, l’utilisateur final peut stocker la musique, des vidéos et des photographies dans des dossiers partagés, puis accéder à ces fichiers multimédias à partir d’ordinateurs en réseau ou Accès Web à distance.  
  
-   **Contrôle d'intégrité** Vous pouvez surveiller l'état du réseau et obtenir des rapports d'intégrité personnalisés.  
  
##  <a name="BKMK_Benefits"></a> Avantages de l’hébergement d’expérience Windows Server Essentials  
  Expérience Windows Server Essentials est un rôle dans Windows Server, vous pouvez réutiliser l’infrastructure de gestion dans Windows Server pour déployer et configurer le rôle expérience Windows Server Essentials et de déploiement existante. Qui héberge le rôle expérience Windows Server Essentials offre les avantages suivants :  
  
-   **Déploiement rationalisé** en activant simplement le rôle expérience Windows Server Essentials, parmi les plus couramment utilisé des rôles et fonctionnalités sont activées et configurées avec les meilleures pratiques pour les petites et moyennes entreprises. Vous pouvez personnaliser les fonctionnalités de Windows Server Essentials ou masquer certaines des fonctionnalités locales. Si vous utilisez le Pack Windows Azure, vous pouvez télécharger le modèle de galerie pour Windows Server Essentials Experience sur Windows Server 2012 R2.  
  
-   **Tableau de bord simplifié** Le tableau de bord Windows Server Essentials simplifie des tâches courantes telles que la gestion des dossiers de serveur, du stockage, de la sauvegarde et de la restauration de serveur, des comptes d'utilisateur et de groupe ou des périphériques, de l'accès à distance et de la messagerie électronique. Les petites et moyennes entreprises peuvent effectuer des tâches quotidiennes de gestion au lieu d'appeler le support technique.  
  
-   **Extensibilité** Le tableau de bord Windows Server Essentials et le logiciel connecteur Windows Server Essentials sont extensibles. Vous pouvez ajouter votre propre marque et intégration de service pour que vos clients disposent d'un point d'entrée pour tout ce qui concerne leur serveur et leur service.  
  
-   **Moniteur** Une nouvelle version du pack d'analyse System Center est disponible pour surveiller et gérer plusieurs serveurs exécutant Windows Server Essentials. Pour télécharger le pack d’administration, consultez [System Center Management Pack pour Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=40809).  
  
##  <a name="BKMK_SupportedDeployment"></a> Options de déploiement pris en charge  
  Expérience Windows Server Essentials peut être déployé en tant que contrôleur de domaine dans un environnement Active Directory ; ou il peut être déployé dans un environnement Active Directory existant en tant que membre d’un domaine.  
  
 Il est recommandé de commencer par déployer Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter puis installer le rôle expérience Windows Server Essentials. Avec cette méthode de déploiement, vous obtenez toutes les fonctionnalités d’édition de Windows Server Essentials, sans les verrous et les limites.  
  

 Pour plus d’informations sur l’installation de Windows Server 2012 R2 avec le rôle expérience Windows Server Essentials, consultez [installer et configurer Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).  


  
##  <a name="BKMK_SupportedToplogy"></a> Topologies de réseau prises en charge  
 Pour utiliser l’expérience Windows Server Essentials à partir d’un client itinérant, VPN doit être activé. Pour activer l'accès à distance au serveur à partir de clients itinérants, vous devez ouvrir les ports 443 et 80 sur le serveur.  
  
 Voici les deux topologies de mise en réseau par défaut côté serveur et la manière dont les fonctionnalité de VPN / accès web à distance peuvent être configurées :  
  
- **Topologie 1** (Il s'agit de la topologie par défaut ; elle place tous les serveurs et la plage IP VPN dans le même sous-réseau.) :  
  
  -   Configurez le serveur sur un réseau virtuel distinct sous un périphérique de traduction d'adresses réseau (NAT).  
  
  -   Activez le service DHCP sur le réseau virtuel ou attribuez une adresse IP statique au serveur.  
  
  -   Transférez le port 443 d'adresse IP publique sur le routeur vers l'adresse de réseau local du serveur.  
  
  -   Autorisez le relais VPN pour le port 443.  
  
  -   Définissez le pool d'adresses IPv4 VPN dans la même plage de sous-réseau que l'adresse du serveur.  
  
  -   Affectez au second serveur une adresse IP statique sur le même sous-réseau, mais hors du pool d'adresses VPN.  
  
- **Topologie 2**:  
  
  -   Attribuez au serveur une adresse IP privée.  
  
  -   Autorisez le port 443 sur le serveur à atteindre une adresse IP publique sur le port 443.  
  
  -   Autorisez le relais VPN pour le port 443.  
  
  -   Affectez des plages différentes pour le pool d'adresses IPv4 VPN et l'adresse du serveur.  
  
  Avec la Topologie 2, les scénarios avec second serveur ne sont pas pris en charge, car vous ne pouvez pas ajouter un autre serveur dans le même domaine.  
  
  Vous pouvez activer la fonctionnalité VPN pendant un déploiement sans assistance à l'aide de notre script Windows PowerShell ou la configurer avec l'Assistant après la configuration initiale.  
  
  Pour activer la fonctionnalité VPN à l'aide de Windows PowerShell, exécutez la commande suivante avec des privilèges d'administration sur le serveur exécutant Windows Server Essentials et fournissez toutes les informations nécessaires.  
  
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
>  Si vous ne pouvez pas fournir de connexion VPN avant que le client prenne possession du serveur, assurez-vous que le port serveur 3389 est accessible via Internet pour que le client puisse utiliser le protocole RDP (Remote Desktop Protocol) pour se connecter au serveur et le configurer.  
  
##  <a name="BKMK_CustomizeImage"></a> Personnaliser l’image du rôle expérience Windows Server Essentials  
 Vous pouvez personnaliser l'image avant de configurer le rôle Expérience Windows Server Essentials. Pour en savoir plus sur le processus Sysprep standard de Windows Server, consultez la rubrique [Kit de déploiement et d’évaluation Windows](https://msdn.microsoft.com/library/hh825420.aspx). Après avoir préparé l'image à l'aide de Sysprep, vous pouvez l'utiliser ou la resceller dans Install.wim pour un nouveau déploiement.  
  
 Si vous utilisez Virtual Machine Manager, vous pouvez créer un modèle en utilisant l'instance en cours d'exécution. Ce processus utilise Sysprep pour préparer l'instance et il arrête l'ordinateur. Une fois que vous avez stocké le modèle dans votre bibliothèque, vous pouvez l'utiliser au cas par cas.  
  
 Après avoir installé le rôle expérience Windows Server Essentials, vous pouvez personnaliser les fonctionnalités de Windows Server Essentials. L'une des personnalisations les plus importantes est la clé de Registre **IsHosted** : **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\Deployment\IsHosted**.  
  
 Si cette clé a la valeur 0x1, certaines des fonctionnalités locales changent de comportement. Ces modifications de fonctionnalité sont les suivantes :  
  
- **Sauvegarde du client** La sauvegarde du client est désactivée par défaut pour les ordinateurs clients qui viennent d'être joints.  
  
- **Service de restauration du client** Le service de restauration du client est désactivé et l'interface utilisateur est masquée dans le tableau de bord.  
  
- **Historique des fichiers** Les paramètres de l'historique des fichiers pour les comptes d'utilisateur qui viennent d'être créés ne sont pas gérés automatiquement par le serveur.  
  
- **Sauvegarde du serveur** Le service de sauvegarde du serveur est désactivée et l'interface utilisateur de sauvegarde du serveur est masquée dans le tableau de bord.  
  
- **Espaces de stockage** L'interface utilisateur de création ou de gestion des espaces de stockage est masquée dans le tableau de bord.  
  
- **Accès en tout lieu** La configuration VPN et de routeur est ignorée par défaut quand vous exécutez l'Assistant Configurer l'Accès en tout lieu.  
  
  Si vous voulez contrôler le comportement de chaque fonctionnalité répertoriée, vous pouvez définir la clé de Registre correspondante pour chacune d'elles. Pour plus d’informations sur la façon de définir la clé de Registre, consultez [Personnaliser et déployer Windows Server Essentials dans Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
##  <a name="BKMK_AutomateDeployment"></a> Automatiser le déploiement de l’expérience Windows Server Essentials  
 Pour automatiser le déploiement, vous devez d’abord déployer le système d’exploitation, puis installer le rôle expérience Windows Server Essentials.  
  
-   Pour déployer automatiquement Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter, suivez les instructions de [Windows Assessment and Deployment Kit](https://msdn.microsoft.com/library/hh825420.aspx).  
  
-   Pour savoir comment installer le rôle expérience Windows Server Essentials à l’aide de Windows PowerShell, consultez [installer et configurer Windows Server Essentials](https://technet.microsoft.com/library/dn281793.aspx).  
  
> [!NOTE]
>  Assurez-vous que les paramètres de fuseau horaire de la machine virtuelle hôte et l’expérience Windows Server Essentials sont identiques. Sinon, plusieurs erreurs risquent de se produire. Citons notamment : la configuration initiale du serveur n’est peut-être pas réussie sur les tâches liées aux certificats, le certificat peut ne pas fonctionne pendant quelques heures une fois que le rôle expérience Windows Server Essentials est installé, et les informations de périphérique ne met pas à jour correctement.  
  
 Après le déploiement, utilisez l’applet de commande Windows PowerShell **Get-WssConfigurationStatus** pour vérifier si la configuration initiale a réussi. L'état renvoyé doit être l'un des suivants : **Notstarted**, **FinishedWithWarning**, **Running**, **Finished**, **Failed** ou **PendingReboot**.  
  
 Le serveur est redémarré pendant la configuration initiale. Si vous devez éviter ce redémarrage automatique, vous pouvez utiliser la commande suivante pour ajouter une clé de Registre avant de commencer la configuration initiale :  
  
```  
New-ItemProperty "HKLM:\Software\Microsoft\Windows Server\Setup"Ã‚Â  -Name "WaitForReboot" -Value 1 -PropertyType "DWord" -Force -Confirm:$false  
  
```  
  
 Après le démarrage de la configuration initiale, vous pouvez utiliser **Get-WssConfigurationStatus** pour vérifier l’état de la configuration initiale et, quand l’état est **PendingReboot**, vous pouvez redémarrer votre serveur.  
  
##  <a name="BKMK_Migrate"></a> Migrer des données à partir de Windows Small Business Server vers Windows Server Essentials Experience  
 Vous pouvez migrer des données depuis des serveurs exécutant Windows Small Business Server 2011, Windows Small Business Server 2008, Windows Small Business Server 2003 ou Windows Server Essentials sur le serveur exécutant Windows Server Essentials. Examinez le [migrer vers Windows Server Essentials](../migrate/Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md) migration guide pour en local 2migrations et procéder aux personnalisations nécessaires selon votre environnement d’hébergement.  
  
> [!NOTE]
>  Nous vous recommandons de placer le serveur source et le serveur de destination sur le même sous-réseau. Si ce n'est pas possible, assurez-vous que :  
> 
> - Le serveur source et le serveur de destination peuvent accéder à l’autre « ¢ s noms DNS internes.  
>   -   tous les ports nécessaires sont ouverts.  
  
 Après la migration, vous pouvez mettre à niveau vos licences pour supprimer les verrous et les limites. Pour plus d’informations, consultez [Transition de Windows Server Essentials pour Windows Server 2012 Standard](https://technet.microsoft.com/library/jj247582.aspx).  
  
##  <a name="BKMK_PowerShell"></a> Effectuer des tâches courantes à l’aide de Windows PowerShell  
 Cette section décrit certaines des tâches courantes que vous pouvez effectuer à l'aide de Windows PowerShell.  
  
### <a name="enable-remote-web-access"></a>Activer l'accès web à distance  
 **Syntaxe** :  
  
 Enable-WssRemoteWebAccess [-SkipRouter] [-DenyAccessByDefault] [-ApplyToExistingUsers]  
  
 **Exemple** :  
  
 $Enable-WssRemoteWebAccess  œDenyAccessByDefault  œApplyToExistingUsers  
  
 cette commande activera l'Accès web à distance avec le routeur configuré automatiquement et modifiera les autorisations d'accès par défaut pour tous les utilisateurs existants.  
  
### <a name="add-user"></a>Ajouter un utilisateur  
 **Syntaxe** :  
  
 Ajouter-WssUser [-nom] < chaîne\> [-mot de passe] < securestring\> [-AccessLevel < chaîne\> {utilisateur &#124; administrateur}] [-FirstName < chaîne\>] [-LastName < chaîne\>] [- AllowRemoteAccess] [-AllowVpnAccess] [< Paramètres_courants\>]  
  
 **Exemple** :  
  
 $password = ConvertTo-SecureString « Passw0rd ! » -asplaintext  œforce$Add-WssUser -Name User2Test -Password $password -Accesslevel Administrator -FirstName User2 -LastName Test  
  
 Cette commande ajoutera un administrateur nommé User2Test avec le mot de passe Passw0rd !.  
  
### <a name="add-server-folder"></a>Ajouter un dossier de serveur  
 **Syntaxe** :  
  
 Ajouter-WssFolder [-nom] < chaîne\> [-chemin d’accès] < chaîne\> [[-Description] < chaîne\>] [-KeepPermissions] [< Paramètres_courants\>]  
  
 **Exemple** :  
  
 $Add-WssFolder -Name "MyTestFolder" -Path "C:\ServerFolders\MyTestFolder"  
  
 Cette commande ajoutera un dossier de serveur nommé MyTestFolder à l’emplacement spécifié.  
  
##  <a name="BKMK_EmailIntegration"></a> Intégration de messagerie avec Windows Server Essentials  
 Vous pouvez intégrer l’expérience de Windows Server Essentials à Office 365 ou hébergée d’Exchange Server. Si vous voulez que votre client utiliser votre messagerie hébergée, vous devez développer un complément pour intégrer l'Expérience Windows Server Essentials à votre solution de messagerie hébergée. Pour plus d’informations, consultez la rubrique [Kit de développement logiciel (SDK) Windows Server Essentials](https://msdn.microsoft.com/library/gg513877.aspx).  
  
##  <a name="BKMK_Monitoring"></a> Surveiller et gérer à l’aide des outils natifs  
 Cette section décrit les outils natifs disponibles dans Windows Server 2012 R2 pour surveiller et gérer le serveur.  
  
### <a name="group-policy"></a>Stratégie de groupe  
  Expérience Windows Server Essentials exploite la prise en charge native à la stratégie de groupe dans Windows Server 2012 R2 et fournit une interface utilisateur pour configurer les paramètres de sécurité et de la Redirection de dossiers.  
  
> [!NOTE]
>  Si la redirection de dossiers est activée dans un environnement hébergé pour un profil utilisateur, le temps nécessaire aux utilisateurs finaux pour se connecter peut augmenter si le volume de données est important.  
  
### <a name="system-center-monitoring-pack"></a>Pack d'analyse System Center  
 Pack de surveillance de System Center pour les analyses de Windows Server Essentials Experience le système d’alerte d’intégrité pour vous aider à gérer un grand nombre de serveurs qui exécutent Windows Server Essentials qui sont dédiés aux petites entreprises. Pour plus d’informations, consultez [System Center Management Pack pour Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=40809).  
  
### <a name="backup-and-restore"></a>Sauvegarde et restauration  
  Windows Server 2012 R2 avec l’expérience de Windows Server Essentials vous permet de sauvegarder des ordinateurs serveurs et clients dans le réseau.  
  
#### <a name="server-backup"></a>Sauvegarde du serveur  
 Windows Server 2012 Essentials prend en charge deux manières de sauvegarder le serveur : la sauvegarde locale et la sauvegarde externe. Vous pouvez personnaliser ces options si vous voulez déployer votre propre solution de sauvegarde de serveur.  
  
-   **Sauvegarde locale** Elle vous permet d'exécuter régulièrement des sauvegardes incrémentielles au niveau du bloc sur un disque distinct. En tant qu'hébergeur, vous pouvez attacher un disque dur virtuel à l'ordinateur virtuel exécutant Windows Server Essentials et configurer ensuite la sauvegarde de serveur sur ce disque dur virtuel. Le disque dur virtuel doit se trouver sur un disque physique différent de l'ordinateur virtuel exécutant Windows Server Essentials.  
  
    > [!NOTE]
    >  Si vous disposez d'autres solutions de sauvegarde pour vos ordinateurs virtuels et que vous ne voulez pas que vos utilisateurs voient la fonctionnalité de sauvegarde de serveur native de Windows Server Essentials, vous pouvez la désactiver et supprimer l'interface utilisateur correspondante du tableau de bord. Pour plus d’informations, consultez la section [Personnaliser la sauvegarde du serveur](https://technet.microsoft.com/library/dn293413.aspx) dans [Personnaliser et déployer Windows Server Essentials dans Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
-   **Sauvegarde externe** Elle vous permet de sauvegarder périodiquement vos données de serveur dans un service cloud. Vous pouvez télécharger et installer le Microsoft Azure sauvegarde Integration Module for Windows Server Essentials pour tirer parti de la sauvegarde Azure qui est fourni par Microsoft.  
  
     Pour plus d’informations, consultez intégration Windows Azure Backup avec section de Windows Server Essentials dans [Manage Server Backup](../manage/Manage-Server-Backup-in-Windows-Server-Essentials.md).  
  
     Si vos utilisateurs ou vous-même préférez un autre service cloud, vous devez :  
  
    -   Mettre à jour l’interface utilisateur du tableau de bord afin qu’il est lié à votre service cloud préféré plutôt qu’à la valeur par défaut de sauvegarde Azure.  
  
    -   (Facultatif) développer un complément pour le tableau de bord pour configurer et gérer le service cloud de sauvegarde.  
  
#### <a name="client-computer-backup"></a>Sauvegarde d'ordinateurs clients  
 Windows Server Essentials Experience prend en charge deux types de sauvegarde de données du client : la sauvegarde complète du client et l'historique des fichiers.  
  
> [!NOTE]
>  La sauvegarde du client peut avoir un impact sur les performances, car les données doivent être transférées du client au serveur via un réseau privé virtuel.  
  
##### <a name="full-client-backup"></a>Sauvegarde complète du client  
 La sauvegarde complète du client est activée par défaut pour tous les périphériques clients connectés au réseau Windows Server Essentials. Elle sauvegarde toutes les informations système et les données pour le client et prend en charge la déduplication des données. Les données de sauvegarde doivent être stockées sur le serveur exécutant Windows Server Essentials. Cela permet à un client ayant subi une défaillance de récupérer des données à partir d'un point de sauvegarde précédent.  
  
 Voici quelques aspects clés de la sauvegarde complète du client :  
  
-   **Performances** : il se peut que la sauvegarde initiale du serveur nécessite du temps en raison du volume de données à charger.  
  
-   **Stabilité** : la connexion Internet n'est parfois pas stable côté client. Sauvegarde du client est conçue pour reprendre automatiquement, et la base de données de sauvegarde cliente crée un point de contrôle chaque fois que 40 Go de données est sauvegardée. Vous pouvez modifier cette valeur et la réduire si vous pensez que la connexion Internet n'est pas fiable.  
  
    -   Pour activer une tâche de point de contrôle Sur le serveur, affectez la valeur 1 à la clé de Registre **HKLM\Software\Microsoft\Windows Server\Backup\GetCheckPointJobs**.  
  
    -   Pour modifier le seuil de point de contrôle Sur le client, remplacez **HKLM\Software\Microsoft\Windows Server\Backup\CheckPointThreshold** à partir de sa valeur par défaut de 40 Go.  
  
-   **Restauration du client à froid** : l'environnement de préinstallation de Windows ne prenant pas en charge les connexions VPN, la restauration du client à froid n'est pas prise en charge. Vous devez masquer la tâche Service de restauration de client en suivant les étapes décrites dans [Personnaliser et déployer Windows Server Essentials dans Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
##### <a name="file-history"></a>Historique des fichiers  
 L'historique des fichiers est une fonctionnalité de Windows 8.1 et Windows 8 pour la sauvegarde de données de profil (bibliothèques, bureau, contacts, favoris) dans un partage réseau. Vous pouvez gérer de manière centralisée les paramètres de l'historique des fichiers de tous les ordinateurs Windows 8.1 ou Windows 8 qui appartiennent à un réseau Windows Server Essentials. Les données de sauvegarde sont stockées sur le serveur Windows Server Essentials. Vous devez masquer la tâche Service de restauration de client en suivant les étapes décrites dans [Personnaliser et déployer Windows Server Essentials dans Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
### <a name="storage-management"></a>Gestion du stockage  
 La fonctionnalité Espaces de stockage vous permet d'agréger la capacité de stockage physique de différents disques durs, d'ajouter dynamiquement des disques durs et de créer des volumes de données avec des niveaux de résilience spécifiques. Vous pouvez effectuer ces opérations sur l'hôte ou sur l'ordinateur virtuel. Si vous souhaitez masquer cette fonctionnalité sur un ordinateur virtuel exécutant Windows Server Essentials, suivez les instructions fournies dans [Personnaliser et déployer Windows Server Essentials dans Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx).  
  
##  <a name="BKMK_Scenarios"></a> Scénarios de test  
 Du point de vue de l'hébergement, nous vous recommandons de tester les scénarios suivants :  
  

-   [Déploiement de serveur](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerDeploy)  
  
-   [Configuration du serveur](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerConfig2)  
  
-   [Gestion de serveur](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ServerManage)  
  
-   [Expérience client](Deploy-Windows-Server-Essentials-Experience-as-a-Hosted-Server.md#BKMK_ClientXP)  

  
###  <a name="BKMK_ServerDeploy"></a> Déploiement de serveur  
 Vous pouvez tester les scénarios de déploiement de serveur suivants :  
  
-   Déployer un serveur exécutant Windows Server 2012 R2 en tant que contrôleur de domaine dans votre environnement de laboratoire et puis installer le rôle expérience Windows Server Essentials.  
  
-   Déployer un serveur exécutant Windows Server 2012 R2 dans votre environnement de laboratoire, joindre ce serveur à un domaine existant, puis installer le rôle expérience Windows Server Essentials.  
  
-   Personnaliser l'image Windows Server 2012 Essentials en fonction des besoins.  
  
-   Automatiser le déploiement de Windows Server Essentials avec un fichier sans assistance et Windows PowerShell.  
  
-   Migrer des serveurs locaux Windows Small Business Server vers des serveurs hébergés Windows Server Essentials.  
  
###  <a name="BKMK_ServerConfig2"></a> Configuration du serveur  
 Vous pouvez tester les scénarios de configuration de serveur suivants :  
  
-   Configurer l'accès en tout lieu (réseau privé virtuel, accès web à distance et DirectAccess).  
  
-   Configurer des dossiers de stockage et de serveur.  
  
-   Activer BranchCache.  
  
-   Configurer la sauvegarde du serveur, la sauvegarde en ligne, la sauvegarde du client et l'historique des fichiers (le cas échéant).  
  
-   Configurer gérer les espaces de stockage (le cas échéant).  
  
-   Configurer l'intégration à une solution de messagerie (Office 365 et Exchange Server hébergé) (le cas échéant).  
  
-   Configurer l'intégration à d'autres services en ligne Microsoft (le cas échéant).  
  
-   Configuration du serveur multimédia (le cas échéant).  
  
###  <a name="BKMK_ServerManage"></a> Gestion de serveur  
 Vous pouvez tester les scénarios de gestion de serveur suivants :  
  
-   Gérer les utilisateurs et groupes.  
  
-   Configuration et réception d'une notification des alertes par courrier électronique.  
  
-   Exécuter l'outil Best Practices Analyzer pour voir s'il existe un message d'erreur ou d'avertissement.  
  
-   Configurer le pack System Center Operations Manager.  
  
-   Configurer la récupération de serveur en cas d'endommagement du système d'exploitation.  
  
###  <a name="BKMK_ClientXP"></a> Expérience client  
 Vous pouvez tester les scénarios d'utilisateur final suivants :  
  
-   Déployer les clients sur Internet (systèmes d'exploitation PC ou Mac).  
  
-   Utiliser Launchpad sur le client pour accéder aux dossiers partagés.  
  
-   Accéder à des ressources du serveur via l'accès web à distance depuis différents périphériques (PC, téléphone, tablette).  
  
-   Utiliser l'application Mon Serveur pour Windows Phone.  
  
-   Accéder à l'historique des fichiers, la sauvegarde et restauration du client et la redirection de dossiers (le cas échéant).  
  
-   Vérifier l'expérience d'intégration de la messagerie (le cas échéant).  
  
##  <a name="BKMK_Support"></a> Informations de prise en charge  
 Vous pouvez télécharger le Kit de développement logiciel (SDK) Windows Server Essentials et l’évaluation de Windows Server Essentials et le Kit de déploiement (ADK) :  
  
-   [Kit de développement logiciel Windows Server Essentials](https://msdn.microsoft.com/library/gg513877.aspx)SDK  
  
-   [Personnaliser et déployer Windows Server Essentials dans Windows Server 2012 R2](https://technet.microsoft.com/library/dn293241.aspx)  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Quelles sont les nouveautés dans Windows Server Essentials](../get-started/what-s-new.md)  

-   [Installer Windows Server Essentials](Install-Windows-Server-Essentials.md)  

-   [Prise en main de Windows Server Essentials](../get-started/get-started.md)
