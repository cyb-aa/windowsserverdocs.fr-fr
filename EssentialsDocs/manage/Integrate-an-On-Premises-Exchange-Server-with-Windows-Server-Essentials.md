---
title: "Intégrer un serveur de Exchange local à WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b56a21e2-c9e3-4ba9-97d9-719ea6a0854b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c2020e08b94800a9750f095a2f772afb14ba5f0b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="integrate-an-on-premises-exchange-server-with-windows-server-essentials"></a>Intégrer un serveur de Exchange local à WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Ce guide fournit des informations et des instructions de base pour vous aider à configurer et intégrer un serveur local qui exécute Exchange Server avec un serveur qui exécute WindowsServerEssentials.  
  
 Vous devez lire ce guide avant de tenter de déployer un serveur local qui exécute Exchange Server sur un réseau WindowsServerEssentials.  
  
> [!NOTE]
>  Exchange Server2010 ne prend pas en charge l’installation sur les ordinateurs qui exécutent Windows Server2012.  
  
## <a name="prerequisites"></a>Conditions préalables  
 Avant d’installer Exchange Server sur un réseau WindowsServerEssentials, assurez-vous d’effectuer les tâches présentées dans cette section.  
  
-   [Configurer un serveur qui exécute WindowsServerEssentials](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_SetUpSBS8)  
  
-   [Préparer un second serveur sur lequel installer Exchange Server](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_SecondServer)  
  
-   [Configurer votre nom de domaine Internet](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_DomainNames)  
  
###  <a name="BKMK_SetUpSBS8"></a>Configurer un serveur qui exécute WindowsServerEssentials  
 Vous devez avoir déjà configuré un serveur qui exécute WindowsServerEssentials. Ce sera le contrôleur de domaine pour le serveur qui exécute Exchange Server. Pour plus d’informations sur comment configurer WindowsServerEssentials, consultez [installer WindowsServerEssentials](../install/Install-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_SecondServer"></a>Préparer un second serveur sur lequel installer Exchange Server  
 Vous devez installer Exchange Server sur un second serveur qui exécute une version du système d’exploitation Windows Server qui prend officiellement en charge en cours d’exécution d’Exchange Server2010 ou Exchange Server2013. Vous devez alors joindre le second serveur au domaine WindowsServerEssentials.  
  
 Pour plus d’informations sur la façon de joindre un second serveur au domaine WindowsServerEssentials, consultez joindre un second serveur au réseau de [connexion](../use/Get-Connected-in-Windows-Server-Essentials.md).  
  
> [!NOTE]
>  Microsoft ne prend pas en charge installer Exchange Server sur un serveur qui exécute WindowsServerEssentials.  
  
###  <a name="BKMK_DomainNames"></a>Configurer votre nom de domaine Internet  
 Pour intégrer un serveur local qui exécute Exchange Server avec WindowsServerEssentials, vous devez avoir inscrit un nom de domaine Internet valide pour votre entreprise (comme *contoso.com*). Vous devez également collaborer avec votre fournisseur de noms de domaine pour créer les enregistrements de ressources DNS requis par Exchange Server.  
  
 Par exemple, si le nom de domaine Internet de votre société est contoso.com et que vous souhaitez utiliser le nom de domaine complet (FQDN) de *mail.contoso.com* pour faire référence à votre serveur local qui exécute Exchange Server, collaborez avec votre fournisseur de nom de domaine pour créer des enregistrements de ressource DNS dans le tableau suivant.  
  
|Nom d’enregistrement de ressource|Type d’enregistrement|Paramètre d’enregistrement|Description|  
|--------------------------|-----------------|--------------------|-----------------|  
|messagerie|Hôte (A)|Adresse =*adresse IP publique attribuée par votre fournisseur de services Internet*|Exchange Server reçoit le courrier adressé à mail.contoso.com.<br /><br /> Vous pouvez utiliser un autre nom à votre propre sélection.|  
|MX|Serveur de messagerie (MX)|Nom d’hôte = @<br /><br /> Adresse = mail.contoso.com<br /><br /> Préférence = 0|Fournit le routage de messages électroniques pour email@contoso.comparviennent à votre serveur local qui exécute Exchange Server.|  
|SPF|Texte (TXT)|V = spf1 un mx environ tous|Enregistrement de ressource qui contribue à empêche les messages électroniques envoyés depuis votre serveur ne soient identifiés comme courrier indésirable.|  
|Autodiscover._tcp|Service (SRV)|Service: _autodiscover<br /><br /> Protocole: _tcp<br /><br /> Priorité: 0<br /><br /> Poids: 0<br /><br /> Port: 443<br /><br /> Ordinateur hôte cible: mail.contoso.com|Permet à MicrosoftOffice Outlook et les périphériques mobiles détecter automatiquement votre serveur local qui exécute Exchange Server.<br /><br /> **Remarque:** vous pouvez également configurer un enregistrement de ressource hôte (A) la découverte automatique et pointer l’enregistrement vers l’adresse IP publique de votre serveur local qui exécute Exchange Server. Toutefois, si vous implémentez cette option, vous devez également fournir les certificats SSL sujet autre nom d’objet (SAN) qui prend en charge des noms de domaine mail.contoso.com et autodiscover.contoso.com.|  
  
> [!NOTE]
>  -   Remplacez les instances de *contoso.com* dans cet exemple avec le nom de domaine Internet que vous avez enregistré.  
  
 Vous devez choisir un autre nom de domaine complet pour votre serveur local qui exécute Exchange Server celui que vous utilisez pour le serveur qui exécute WindowsServerEssentials. Par exemple, vous pouvez choisir d’utiliser *remote.contoso.com* en tant que le nom de domaine complet que les ordinateurs utilisent pour accéder au serveur exécutant WindowsServerEssentials à partir d’Internet. Vous pouvez utiliser *mail.contoso.com* en tant que le nom de domaine complet qui est utilisé pour router les messages électroniques à votre serveur local qui exécute Exchange Server.  
  
## <a name="install-exchange-server"></a>Installer Exchange Server  
 La fonctionnalité d’intégration Exchange Server sur WindowsServerEssentials prend en charge les versions suivantes d’Exchange Server:  
  
-   Exchange Server2013  
  
-   Exchange Server2010 avec Service Pack1 (SP1)  
  
 Avant d’installer le serveur Exchange sur le second serveur, vous devez d’abord ajouter le compte d’administrateur actuel pour le **administrateurs de l’entreprise** groupe.  
  
#### <a name="to-add-the-current-administrator-account-to-the-enterprise-admins-group"></a>Pour ajouter le compte d’administrateur actuel au groupe Administrateurs de l’entreprise  
  
1.  Connectez-vous à WindowsServerEssentials en tant qu’administrateur.  
  
2.  Exécutez Windows PowerShell en tant qu’administrateur.  
  
3.  À l’invite de commandes Windows PowerShell, tapez **Add-ADGroupMember ˜Enterprise administrateurs $env: nom d’utilisateur**, puis appuyez sur ENTRÉE.  
  
#### <a name="to-install-exchange-server"></a>Pour installer Exchange Server  
  
1.  Ouvrez une session sur le second serveur en tant qu’administrateur.  
  
2.  Ouvrez votre navigateur Internet, puis accédez à la [Assistant de déploiement Exchange Server](https://go.microsoft.com/fwlink/p/?LinkID=249163) site Web.  
  
3.  Cliquez sur **On-Premises Only**.  
  
4.  Cliquez sur la nouvelle option d’installation pour la version d’Exchange Server que vous installez.  
  
    > [!NOTE]
    >  Si vous effectuez une migration à partir d’une installation de WindowsSmallBusinessServer, vous devez sélectionner l’option de mise à niveau appropriée qui couvre les étapes de migration.  
  
5.  Sur la page suivante, acceptez les paramètres par défaut, puis cliquez sur **suivant**.  
  
    > [!NOTE]
    >  Si vous prévoyez d’utiliser des dossiers publics dans la nouvelle installation d’Exchange Server, modifier ce paramètre pour **Oui**.  
  
6.  Suivez les instructions pas à pas dans la liste de vérification pour déployer Exchange Server.  
  
     L’Assistant de déploiement Exchange Server vous permet également:  
  
    -   Imprimer une copie de la liste de vérification.  
  
    -   Envoyer une copie de la liste de vérification à un destinataire de courrier électronique.  
  
    -   Télécharger la liste de vérification sous la forme d’un fichier PDF.  
  
> [!NOTE]
>  -   Vous devez toujours choisir d’installer les outils de gestion sur le serveur qui exécute Exchange Server. Les outils de gestion sont requis par la fonctionnalité d’intégration Exchange Server sur WindowsServerEssentials.  
> -   Si vous avez besoin configurer des répertoires virtuels, nous vous conseillons également le **InternalUrl** à la même URL en tant que propriété du **ExternalUrl** propriété pour chaque répertoire virtuel. Pour plus d’informations, voir [la gestion des clients accès aux répertoires virtuels du serveur](https://go.microsoft.com/fwlink/p/?LinkId=251058) au site Web aide en ligne Exchange Server2010.  
> -   Si vous souhaitez accéder à Outlook Web Access (OWA) à partir du site d’accès Web à distance sur WindowsServerEssentials, vous devez définir la propriété URL externe pour OWA.  
  
 Si vous installez Exchange Server2010dans une nouvelle installation, vous pouvez également utiliser les scripts suivants pour configurer le serveur Exchange.  
  
#### <a name="to-use-scripts-to-set-up-exchange-server"></a>Pour utiliser des scripts pour configurer le serveur Exchange  
  
1.  Ouvrez le bloc-notes et collez le script suivant dans un nouveau fichier:  
  
     `Import-Module ServerManager`  
  
     `Add-WindowsFeature NET-Framework,RSAT-ADDS,Web-Server,Web-Basic-Auth,Web-Windows-Auth,Web-Metabase,Web-Net-Ext,Web-Lgcy-Mgmt-Console,WAS-Process-Model,RSAT-Web-Server,Web-ISAPI-Ext,Web-Digest-Auth,Web-Dyn-Compression,NET-HTTP-Activation,Web-Asp-Net,Web-Client-Auth,Web-Dir-Browsing,Web-Http-Errors,Web-Http-Logging,Web-Http-Redirect,Web-Http-Tracing,Web-ISAPI-Filter,Web-Request-Monitor,Web-Static-Content,Web-WMI,RPC-Over-HTTP-Proxy  �Restart`  
  
2.  Enregistrez le fichier en tant que **InstallDependencies.ps1**.  
  
3.  Copiez le certificat SSL Exchange à un emplacement sur le serveur.  
  
4.  Ouvrez un nouveau fichier Bloc-notes et copiez le texte suivant dans le fichier:  
  
     `param (`  
  
     `[string]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "The path to your Certificate file, must be a *.pfx format")]`  
  
     `$CertPath = "c:\certificates\ExchangeCertificate.pfx",`  
  
     `[Security.SecureString]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "The password of your cert")]`  
  
     `$CertPassword = $null,`  
  
     `[string]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "Domain Name, eg. contoso.com")]`  
  
     `$DomainName = "contoso.com",`  
  
     `[string]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "Server IP Address, eg. 192.168.0.1")]`  
  
     `$ServerIpAddress = "192.168.0.1",`  
  
     `[string]`  
  
     `[Parameter(Mandatory=$true, HelpMessage = "Internal Ip Range, eg. 192.168.0.0-192.168.0.255")]`  
  
     `$InternalIpRange = "192.168.0.0-192.168.0.255"`  
  
     `)`  
  
     `#Import Exchange Certificate, and Enable it for POP IIS IMAP SMTP services.`  
  
     `Import-ExchangeCertificate  �FileData ([Byte[]]$(Get-content -Path $CertPath  �Encoding byte  �ReadCount 0)) -Password:$CertPassword -Force | Enable-ExchangeCertificate -Services 'POP, IIS, IMAP, SMTP' -Force`  
  
     `#New AcceptedDomain and set it to default`  
  
     `New-AcceptedDomain  �Name "official name"  �DomainName $domainname`  
  
     `Set-AcceptedDomain  �Identity "official name"  �MakeDefault $true`  
  
     `#New EmailAddress Policy`  
  
     `$address = "%m@" + $DomainName`  
  
     `New-EmailAddressPolicy -Name "Windows Server Essentials Email Address Policy" -IncludedRecipients AllRecipients -EnabledPrimarySMTPAddressTemplate $address`  
  
     `#Set owa and ecp VirtualDirectory ExternalUrl`  
  
     `$hostname = "mail." + $DomainName`  
  
     `$owa = "https://" + $hostname + "/owa"`  
  
     `$ecp = "https://" + $hostname + "/ecp"`  
  
     `$activesync = "https://" + $hostname + "/Microsoft-Server-ActiveSync"`  
  
     `$oab = "https://" + $hostname + "/OAB"`  
  
     `$ews = "https://" + $hostname + "/EWS/Exchange.asmx"`  
  
     `Get-OwaVirtualDirectory | Set-OwaVirtualDirectory  �ExternalUrl $owa  �InternalUrl $owa`  
  
     `Get-EcpVirtualDirectory | Set-EcpVirtualDirectory  �ExternalUrl $ecp  �InternalUrl $ecp`  
  
     `Get-ActiveSyncVirtualDirectory | Set-ActiveSyncVirtualDirectory -ExternalUrl $activesync  �InternalUrl $activesync`  
  
     `Get-OABVirtualDirectory | Set-OABVirtualDirectory -ExternalUrl $oab -InternalUrl $oab -RequireSSL:$true`  
  
     `Get-WebServicesVirtualDirectory | Set-WebServicesVirtualDirectory -ExternalUrl $ews -InternalUrl $ews -BasicAuthentication:$True -Force`  
  
     `#Enable outlook Anywhere`  
  
     `Enable-OutlookAnywhere  �ClientAuthenticationMethod:Basic  �ExternalHostname:$hostname  �SSLOffloading:$false`  
  
     `#new receive/send connector`  
  
     `$machinename = get-content env:computername`  
  
     `$bindingIpaddress = $ServerIpAddress + ":25"`  
  
     `$RecevieConnectorName = $machinename + "\Default " + $machinename`  
  
     `Set-ReceiveConnector $RecevieConnectorName -RemoteIPRanges $InternalIpRange`  
  
     `New-ReceiveConnector -Name "WSE Internet Receive Connector" -Usage "Internet" -Bindings $bindingIpaddress -Fqdn $hostname -Enabled $true -Server $machinename -AuthMechanism Tls,BasicAuth,BasicAuthRequireTLS,Integrated`  
  
     `New-SendConnector -Name "WSE Internet SendConnector" -Usage "Internet" -AddressSpaces 'SMTP:*;1' -IsScopedConnector $false -DNSRoutingEnabled $true -UseExternalDNSServersEnabled $true -SourceTransportServers $machinename`  
  
5.  Définir les paramètres au début du script pour refléter votre environnement réseau.  
  
6.  Enregistrez le fichier en tant que **ConfigureExchange.ps1**.  
  
7.  Exécutez Windows PowerShell en tant qu’administrateur.  
  
8.  À l’invite de commandes Windows PowerShell, tapez **Set-ExecutionPolicy RemoteSigned**, puis appuyez sur ENTRÉE.  
  
9. Exécutez le script **InstallDependencies.ps1**.  
  
10. Redémarrez le serveur, puis exécutez Windows PowerShell en tant qu’administrateur.  
  
11. À l’invite de commande Windows PowerShell, exécutez le script suivant:  
  
     `E:\setup.com /mode:install /roles:mb,ht,ca /OrganizationName:"First Organization"`  
  
    > [!NOTE]
    >  Veillez à taper le chemin d’accès correct au programme d’installation de Exchange Server.  
  
12. Lorsque le programme d’installation Exchange Server est terminée, ouvrez Exchange Management Shell en tant qu’administrateur.  
  
13. À l’invite de commande Exchange Management Shell, tapez **Set-ExecutionPolicy RemoteSigned**, puis appuyez sur ENTRÉE.  
  
14. Exécutez le script **ConfigureExchange.ps1**.  
  
15. Redémarrez le serveur.  
  
> [!NOTE]
>  Si vous décidez d’utiliser un certificat SSL publiquement approuvé au lieu d’un certificat auto-émis, vous pouvez suivre les instructions du Guide d’installation pour créer une demande de certificat et l’envoyer à votre autorité de Certification sélectionnée. Vous pouvez également utiliser une applet de commande PowerShell Exchange pour créer une demande de certificat. Voici un exemple.  
>   
>  `New-ExchangeCertificate -GenerateRequest -SubjectName "C=US, S=Washington, L=Redmond, O=contoso, OU=contoso, CN=mail.contoso.com" -DomainName mail.contoso.com -PrivateKeyExportable $true | Set-Content -path "c:\Docs\MyCertRequest.req"`  
>   
>  Personnaliser les paramètres de script pour refléter votre environnement réseau.  
  
## <a name="post-installation-tasks"></a>Tâches de Post-installation après  
 Cette section décrit les tâches de configuration de serveur que vous devrez peut-être effectuer dans la phase de post-installation qui contient des informations spécifiques à la configuration d’un serveur local qui exécute Exchange Server sur un réseau WindowsServerEssentials.  
  
### <a name="add-the-public-email-domain-and-configure-the-email-address-policies"></a>Ajouter le domaine de messagerie public et configurer les stratégies d’adresse de messagerie  
  
> [!NOTE]
>  Il s’agit d’une tâche obligatoire si vous effectuez une nouvelle installation. Ignorez cette étape si vous effectuez une migration à partir de WindowsSmallBusinessServer.  
  
 Vous devez spécifier votre domaine de messagerie pour le domaine accepté par défaut et puis configurez la stratégie d’adresses de messagerie.  
  
##### <a name="to-add-your-email-domain-as-the-default-accepted-domain"></a>Pour ajouter votre domaine de messagerie par défaut de domaine accepté  
  
1.  Suivez les instructions de l’article Exchange Server [créer un domaine accepté](https://go.microsoft.com/fwlink/p/?LinkId=249174) pour ajouter un domaine accepté.  
  
2.  Ouvrez une session sur le second serveur en tant qu’administrateur, ouvrez la Console de gestion Exchange et ensuite accéder à la **Transport Hub** onglet de la **Configuration de l’organisation**.  
  
3.  Dans le volet Console de gestion Exchange, cliquez sur le nouveau domaine accepté, puis cliquez sur **par défaut**.  
  
4.  Suivez les instructions de l’article Exchange Server [créer une stratégie d’adresse de messagerie](https://go.microsoft.com/fwlink/p/?LinkId=249179) pour créer une stratégie d’adresse de messagerie. Vous pouvez accepter toutes les valeurs par défaut, à l’exception de l’adresse de messagerie. Pour une adresse de messagerie, indiquez votre domaine de messagerie public.  
  
### <a name="create-smtp-send-and-receive-connectors"></a>Création d’envoi SMTP et de recevoir des connecteurs  
  
> [!NOTE]
>  Il s’agit d’une tâche obligatoire.  
  
 Vous devez configurer un connecteur d’envoi SMTP et un connecteur de réception SMTP pour la transmission sortante/entrante des messages électroniques.  
  
 Pour créer un connecteur d’envoi SMTP, suivez les instructions de l’article Exchange Server [créer un connecteur d’envoi SMTP](https://technet.microsoft.com/library/aa997285.aspx).  
  
 Pour créer un connecteur de réception SMTP, suivez les instructions de l’article Exchange Server [créer un connecteur de réception SMTP](https://technet.microsoft.com/library/bb125159.aspx).  
  
 En option, vous pouvez reporter au script précédemment dans ce document pour la création de l’envoyer et recevoir des connecteurs à l’aide des applets de commande PowerShell Exchange.  
  
### <a name="configure-the-network-router"></a>Configurer le routeur réseau  
  
> [!NOTE]
>  Il s’agit d’une tâche obligatoire si vous effectuez une nouvelle installation. Si vous effectuez une migration à partir de WindowsSmallBusinessServer, voir [Migrate Server Data to WindowsServerEssentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md) pour obtenir des instructions sur la configuration du réseau.  
  
 Au minimum, vous devez configurer les paramètres de port suivants sur le routeur:  
  
|Port du routeur|Destination IP|Port de destination|Remarque|  
|-----------------|--------------------|----------------------|----------|  
|25 (SMTP)|Adresse IP interne du serveur local qui exécute Exchange Server.|25||  
|80 (HTTP)|Adresse IP interne du serveur qui exécute WindowsServerEssentials|80||  
|443 (HTTPS)|Adresse IP interne du serveur qui exécute WindowsServerEssentials|443||  
  
 Si vous prenez en charge la messagerie POP3 ou IMAP protocoles sur votre réseau, vous devez également configurer les réacheminements de ports pour ces protocoles. For-related informations, voir la section **serveurs d’accès Client** dans la rubrique [référence de Port de réseau Exchange](https://go.microsoft.com/fwlink/p/?LinkId=250773) dans la bibliothèque TechNet Exchange Server.  
  
> [!NOTE]
>  -   Nous recommandons que vous configurez des adresses IP statiques pour le serveur qui exécute WindowsServerEssentials et pour le second serveur qui exécute Exchange Server. Pour obtenir des instructions sur la façon de configurer une adresse IP statique sur un ordinateur exécutant Windows Server2003 ou Windows Server2008R2, voir [configurer une adresse IP statique](https://technet.microsoft.com/library/cc754203\(v=ws.10\).aspx) dans la bibliothèque TechNet de Windows Server  
>   
>      **Remarque:** le paramètre de serveur DNS doit toujours pointer vers l’adresse IP du serveur qui exécute WindowsServerEssentials.  
> -   Sur le routeur, assurez-vous que les adresses IP du serveur qui exécute WindowsServerEssentials et le serveur qui exécute Exchange Server sont réservées ou hors de la plage d’adresses IP DHCP.  
> -   La configuration du routeur dans cette section suppose que vous n'avez qu’une seule adresse IP publique attribuée par votre fournisseur de services Internet (ISP). Si vous avez plusieurs adresses IP publiques, vous pouvez attribuer une adresse IP différente pour le serveur qui exécute WindowsServerEssentials et le serveur qui exécute Exchange Server et puis créer des réacheminements de ports selon les adresses IP publiques.  
  
### <a name="enable-on-premises-exchange-server-integration-on-windows-server-essentials"></a>Activer l’intégration du serveur Exchange local sur WindowsServerEssentials  
  
> [!NOTE]
>  Si vous effectuez une migration à partir d’une installation de WindowsSmallBusinessServer, nous recommandons que vous ignorez cette étape pour le moment et l’exécutez après la désinstallation de l’installation précédente d’Exchange Server sur le serveur Source.  
  
 Après avoir installé et configuré un serveur qui exécute Exchange Server, vous devez activer l’intégration du serveur Exchange local sur le serveur qui exécute WindowsServerEssentials.  
  
##### <a name="to-enable-on-premises-exchange-server-integration-from-the-dashboard"></a>Pour activer l’intégration du serveur Exchange local à partir du tableau de bord  
  
1.  Ouvrez une session sur le serveur qui exécute WindowsServerEssentials en tant qu’administrateur, puis ouvrez le tableau de bord.  
  
2.  Sur le **accueil**, cliquez sur **se connecter à mon Service de messagerie**, puis cliquez sur **intégrer votre serveur Exchange**.  
  
3.  Dans le volet d’informations, cliquez sur **configurer l’intégration d’Exchange Server**.  
  
4.  Suivez les instructions de l’Assistant.  
  
### <a name="configure-a-reverse-proxy"></a>Configurer un proxy inverse  
  
> [!NOTE]
>  Il s’agit d’une tâche obligatoire si vous n'avez qu’une seule connexion Internet de votre fournisseur de services Internet.  
  
 WindowsServerEssentials et Exchange Server prennent en charge des scénarios d’accès à distance pour les utilisateurs du réseau. Par exemple, si vous activez l’accès en tout lieu sur le serveur qui exécute WindowsServerEssentials, vous pouvez à distance accéder au site accès Web à distance ou utiliser un réseau privé virtuel (VPN) pour se connecter à distance au réseau WindowsServerEssentials. Pour accéder à distance aux messages électroniques, vous devez utiliser Outlook Anywhere, Outlook Web Access (OWA) ou ActiveSync.  
  
 Si WindowsServerEssentials et le serveur exécutant Exchange Server sont tous deux connectés au même routeur et qu’il n'existe qu’une seule connexion Internet entrante de votre fournisseur de services Internet au routeur, vous devez utiliser une solution de proxy inverse pour router différents types de demandes d’accès à distance à partir d’Internet selon les noms d’hôte de destination. Nous vous recommandons d’utiliser Microsoft pris en charge d’extension IIS Application Request Routing (ARR) en tant que solution de proxy inverse. Pour plus d’informations sur arr IIS, visitez le [site Web Application Request Routing](https://go.microsoft.com/fwlink/p/?LinkId=249181).  
  
##### <a name="to-install-and-configure-application-request-routing"></a>Pour installer et configurer l’Application Request Routing  
  
1.  Connectez-vous à WindowsServerEssentials en tant qu’administrateur.  
  
2.  Ouvrez votre navigateur Internet et accédez à la [site Web Application Request Routing](https://go.microsoft.com/fwlink/p/?LinkId=249181).  
  
3.  Sur le site Web ARR, cliquez sur **installer**, puis suivez les instructions pour installer ARR.  
  
    > [!NOTE]
    >  Vous devez sélectionner le Module de réécriture d’URL pendant l’installation d’ARR.  
    >   
    >  Vous pouvez recevoir une erreur à la fin de l’installation d’ARR Ko2589179 pour ARR 2.5 n’a pas installé correctement. Vous pouvez ignorer cette erreur.  
  
4.  Lorsque l’installation d’ARR est terminée, redémarrez le **passerelle des services Bureau à distance** service s’il n’est pas en cours d’exécution.  
  
    > [!NOTE]
    >  Après avoir installé ARR, le **passerelle des services Bureau à distance** service peut être arrêté. Pour redémarrer manuellement le service, ouvrez le **Services** outil d’administration, puis redémarrez le **passerelle des services Bureau à distance** service.  
  
5.  [Téléchargez le correctif KB2732764 pour ARR 2.5](https://go.microsoft.com/fwlink/?LinkID=258302), puis installez la mise à jour sur le serveur qui exécute WindowsServerEssentials.  
  
6.  Copiez le fichier de certificat SSL pour Exchange Server sur le serveur qui exécute WindowsServerEssentials. Le fichier de certificat doit contenir la clé privée, et il doit être au format de fichier PFX.  
  
    > [!NOTE]
    >  Si vous utilisez un certificat auto-émis, suivez les instructions de l’article Exchange Server [exporter un certificat Exchange](https://technet.microsoft.com/library/dd351274.aspx) pour exporter le certificat.  
  
7.  En fonction de la version de WindowsServerEssentials vous êtes en cours d’exécution, effectuez l’une des opérations suivantes:  
  
    -   Sur WindowsServerEssentials: Ouvrez une fenêtre de commande en tant qu’administrateur, puis ouvrez le répertoire %ProgramFiles%\Windows Server\Bin.  
  
    -   Sur WindowsServerEssentials: Ouvrez une fenêtre de commande en tant qu’administrateur, puis ouvrez le répertoire %Windir%\System32\Essentials.  
  
8.  Selon votre scénario d’installation, suivez l’une de ces étapes pour configurer ARR:  
  
    -   Si vous effectuez une nouvelle installation, exécutez la commande suivante:  
  
         ** ARRConfig config cert ** *chemin d’accès au fichier de certificat* ** hostnames ** *noms des hôtes pour Exchange Server* ****  
  
        > [!NOTE]
        >  Par exemple: ** ARRConfig config cert ***c:\temp\certificate.pfx*** hostnames *** mail.contoso.com ***  
        >   
        >  Remplacer *mail.contoso.com* avec le nom du domaine qui est protégé par le certificat.  
  
    -   Vous effectuez une migration à partir de WindowsSmallBusinessServer, exécutez la commande suivante:  
  
         ** ARRConfig config cert ** *chemin d’accès au fichier de certificat* ** hostnames ** *noms des hôtes pour Exchange Server* ** targetserver ** *nom du serveur Exchange Server* ****  
  
         Par exemple: ** ARRConfig config cert ***c:\temp\certificate.pfx*** hostnames ***mail.contoso.com*** targetserver *** ExchangeSvr ***  
  
         Remplacer *mail.contoso.com* avec le nom de votre domaine. Remplacer *ExchangeSvr* avec le nom de votre serveur qui exécute Exchange Server.  
  
9. Lorsque vous y êtes invité, tapez le mot de passe pour le certificat.  
  
> [!NOTE]
>  -   Les noms d’hôte que vous fournissez doivent être contenus dans le certificat SSL que vous avez acheté pour Exchange Server.  
> -   Si vous avez plusieurs noms d’hôte, utilisez une virgule (,) pour les séparer.  
  
 Pour vérifier que la configuration fonctionne, essayez d’accéder au site Web d’OWA pour votre serveur qui exécute Exchange Server (https://mail. *nom_domaine*.com/owa) à partir d’un ordinateur qui n’est pas membre du domaine. Pour résoudre les problèmes de connectivité, vous pouvez également utiliser l’en ligne [Analyseur de connectivité à distance Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=249455) outil.  
  
### <a name="configure-split-dns-for-exchange-server"></a>Configurer DNS fractionné pour Exchange Server  
  
> [!NOTE]
>  Il s’agit d’une tâche conseillée.  
  
 DNS fractionné vous permet de configurer des adresses IP différentes dans DNS pour le même nom d’hôte, en fonction de l’origine de la requête DNS. Si l’ordinateur client se trouve sur l’intranet, la requête DNS résout une adresse IP intranet. Si l’ordinateur client est sur Internet, la requête DNS est résolue en une adresse IP Internet. Ceci est transparent pour les utilisateurs.  
  
 Nous vous recommandons de configurer fractionnement des services DNS de manière à permettre aux utilisateurs de toujours utiliser le même nom d’hôte pour accéder à Exchange Server, quel que soit leur emplacement.  
  
##### <a name="to-configure-split-dns-for-exchange-server"></a>Pour configurer un environnement DNS fractionné pour Exchange Server  
  
1.  Ouvrez une session sur WindowsServerEssentials en tant qu’administrateur, puis ouvrez le Gestionnaire DNS.  
  
2.  Dans l’arborescence de la console Gestionnaire DNS, cliquez sur votre serveur, puis cliquez sur **nouvelle Zone **. Le **Assistant Nouvelle Zone** s’affiche.  
  
3.  Sur le **Type de Zone** page de l’Assistant, acceptez l’option par défaut, puis cliquez sur **suivant **.  
  
4.  Sur le **étendue de la réplication ActiveDirectory Zone** page, acceptez l’option par défaut, puis cliquez sur **suivant **.  
  
5.  Sur le **directe ou la Zone de recherche inversée** page, acceptez ou sélectionnez **zone de recherche directe**, puis cliquez sur **suivant **.  
  
6.  Sur le **nom de la Zone**, tapez le nom de domaine complet de votre serveur qui exécute Exchange Server (par exemple: *mail.contoso.com*), puis cliquez sur **suivant **.  
  
7.  Sur le **mise à jour dynamique**, acceptez l’option par défaut, cliquez sur **suivant**, puis cliquez sur **Terminer **.  
  
8.  Dans l’arborescence de la console Gestionnaire DNS, la nouvelle zone de recherche directe avec le bouton droit, puis cliquez sur **nouvel hôte (A ou AAAA)**.  
  
9. Sur le **nouvel hôte** page, laissez le **nom** champ vide, tapez l’adresse IP intranet de votre serveur qui exécute Exchange Server, puis cliquez sur **ajouter un hôte **.  
  
    > [!NOTE]
    >  Lorsque vous laissez le **nom** champ vide, le serveur utilise le nom de domaine parent par défaut.  
  
10. Sur le **nouvel hôte**, cliquez sur **fait **.  
  
> [!NOTE]
>  Si vous utilisez ActiveSync mais que vous ne pouvez pas synchroniser le courrier électronique pour certains comptes de boîte aux lettres, déterminez si ces comptes sont membres d’un ou plusieurs groupes protégés tels que les administrateurs de domaine. Informations For-related qui peuvent vous aider à résoudre ce problème, consultez [Exchange ActiveSync a renvoyé une erreur HTTP 500](https://technet.microsoft.com/library/dd439375\(EXCHG.80\).aspx).  
  
## <a name="related-topics"></a>Rubriques connexes  
 Pour plus d’informations sur l’intégration d’un serveur local Exchange Server, consultez les sections suivantes.  
  
### <a name="what-happens-if-i-disable-exchange-integration"></a>Que se passe-t-il si je désactive l’intégration Exchange?  
 Si vous désactivez l’intégration avec un serveur d’Exchange sur site, vous serez n’est plus en mesure d’utiliser le tableau de bord WindowsServerEssentials pour afficher, créer ou gérer des boîtes aux lettres Exchange Server.  
  
### <a name="what-do-i-need-to-know-about-email-accounts"></a>Que dois-je savoir sur les comptes de messagerie?  
 Une solution de messagerie hébergé est configurée sur votre serveur. Une solution à partir d’un fournisseur de messagerie hébergé, tels que MicrosoftOffice 365, peut fournir des comptes individuels pour les utilisateurs du réseau. Lorsque vous exécutez l’Assistant Ajouter un utilisateur compte dans WindowsServerEssentials pour créer un compte d’utilisateur, l’Assistant tente d’ajouter le compte d’utilisateur à la solution de messagerie hébergée disponible. En même temps, l’Assistant attribue un nom de messagerie (alias) à l’utilisateur et définit la taille maximale de la boîte aux lettres (quota). La taille maximale de la boîte aux lettres varie selon le fournisseur de messagerie que vous utilisez. Après avoir ajouté le compte d’utilisateur, vous pouvez continuer à gérer les informations d’alias et les quotas de boîtes aux lettres à partir de la page de propriétés de l’utilisateur. Pour la gestion complète de vos comptes d’utilisateur et le fournisseur de messagerie hébergé, utilisez la console de gestion de votre fournisseur hébergé. En fonction de votre fournisseur, vous pouvez accéder sa console de gestion à partir d’un portail web ou d’un onglet dans le tableau de bord du serveur.  
  
 L’alias que vous fournissez quand vous exécutez l’Assistant Ajouter un utilisateur compte est envoyé au fournisseur de messagerie hébergé comme nom suggéré pour l’alias de l’utilisateur. Par exemple, si l’alias de l’utilisateur est *FrankM*, l’adresse de messagerie de l’utilisateur s peut être *FrankM@Contoso.com*.  
  
 En outre, le mot de passe que vous définissez pour l’utilisateur dans l’Assistant Ajouter un utilisateur compte sera le mot de passe initial de l’utilisateur de la solution de messagerie hébergé.  
  
 Enfin, si vous supprimez l’utilisateur à l’aide de la suppression un Assistant de compte d’utilisateur sur le serveur, l’Assistant envoie également une demande au fournisseur de messagerie hébergé pour supprimer l’utilisateur à partir de leur système également. Le fournisseur peut supprimer de compte d’utilisateur s et l’adresse de messagerie associée au compte.  
  
 Pour l’utilisateur des informations sur comment configurer le logiciel client de messagerie requis, ou pour accéder à un compte de messagerie, reportez-vous à la documentation fournie par votre fournisseur de messagerie hébergé.  
  
### <a name="what-is-a-mailbox-quota"></a>Qu’est un quota de boîte aux lettres?  
 La quantité d’espace de stockage qui est allouée pour un utilisateur réseau s données de boîtes aux lettres Exchange est appelée le quota de boîte aux lettres.  
  
 Lorsque vous exécutez le **configurer l’intégration d’Exchange Server** tâches du tableau de bord, l’Assistant ajoute une page à l’Assistant Ajout de compte d’utilisateur qui vous permet de choisir ou non pour appliquer des quotas de boîtes aux lettres et pour spécifier la taille du quota. Par défaut, le **appliquer des quotas de boîtes aux lettres** option est sélectionnée (activée), et les boîtes aux lettres sont attribués à 2Go d’espace de stockage. Administrateurs Exchange peuvent personnaliser les paramètres de quota de boîte aux lettres en fonction des besoins de leur entreprise.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Configuration système requise pour WindowsServerEssentials](../get-started/system-requirements.md)  
  
-   [Gérer l’intégration de Service de messagerie](Manage-Email-Service-Integration-in-Windows-Server-Essentials.md)  
  
-   [Gérer WindowsServerEssentials](Manage-Windows-Server-Essentials.md)
