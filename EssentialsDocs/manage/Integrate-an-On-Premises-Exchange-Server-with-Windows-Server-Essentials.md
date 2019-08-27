---
title: Intégrer un serveur Exchange local à Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
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
ms.openlocfilehash: 689f293acf1e87e135f6f8cf5c7eac2a7d8033b9
ms.sourcegitcommit: 213989f29cc0c30a39a78573bd4396128a59e729
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70031507"
---
# <a name="integrate-an-on-premises-exchange-server-with-windows-server-essentials"></a>Intégrer un serveur Exchange local à Windows Server Essentials

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Ce guide fournit des informations et des instructions de base pour vous aider à configurer un serveur local qui exécute Exchange Server et à l'intégrer à un serveur qui exécute Windows Server Essentials.  

 Vous devez lire ce guide avant de tenter de déployer un serveur local qui exécute Exchange Server sur un réseau Windows Server Essentials.  

> [!NOTE]
>  Exchange Server 2010 ne prend pas en charge l'installation sur des ordinateurs qui exécutent Windows Server 2012.  

## <a name="prerequisites"></a>Prérequis  
 Avant d'installer Exchange Server sur un réseau Windows Server Essentials, assurez-vous d'effectuer les tâches présentées dans cette section.  

-   [Configurer un serveur qui exécute Windows Server Essentials](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_SetUpSBS8)  

-   [Préparer un second serveur sur lequel installer Exchange Server](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_SecondServer)  

-   [Configurer votre nom de domaine Internet](Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md#BKMK_DomainNames)  

###  <a name="BKMK_SetUpSBS8"></a>Configurer un serveur qui exécute Windows Server Essentials  
 Vous devez déjà avoir configuré un serveur qui exécute Windows Server Essentials. Il s’agit du contrôleur de domaine pour le serveur qui exécute Exchange Server. Pour plus d’informations sur la façon de configurer Windows Server Essentials, consultez [Install Windows Server Essentials](../install/Install-Windows-Server-Essentials.md).  

###  <a name="BKMK_SecondServer"></a>Préparer un second serveur sur lequel installer Exchange Server  
 Vous devez installer Exchange Server sur un second serveur qui exécute une version du système d’exploitation Windows Server qui prend officiellement en charge l’exécution d’Exchange Server 2010 ou d’Exchange Server 2013. Vous devez ensuite joindre le second serveur au domaine Windows Server Essentials.  

 Pour plus d’informations sur la façon de joindre un second serveur au domaine Windows Server Essentials, voir joindre un second serveur au réseau dans [connecter](../use/Get-Connected-in-Windows-Server-Essentials.md).  

> [!NOTE]
>  Microsoft ne prend pas en charge l'installation d'Exchange Server sur un serveur qui exécute Windows Server Essentials.  

###  <a name="BKMK_DomainNames"></a>Configurer votre nom de domaine Internet  
 Pour intégrer un serveur local qui exécute Exchange Server à Windows Server Essentials, vous devez avoir inscrit un nom de domaine Internet valide pour votre entreprise (par exemple *contoso.com*). Vous devez également collaborer avec votre fournisseur de noms de domaine pour créer les enregistrements de ressources DNS requis par Exchange Server.  

 Par exemple, si le nom de domaine Internet de votre société est contoso.com et que vous voulez utiliser le nom de domaine complet *mail.contoso.com* pour faire référence à votre serveur local qui exécute Exchange Server, collaborez avec votre fournisseur de noms de domaine pour créer les enregistrements de ressources DNS dans le tableau suivant.  


| Nom de l’enregistrement de ressource |     Type d’enregistrement     |                                                                         Paramètre d’enregistrement                                                                          |                                                                                                                                                                                                                                                              Description                                                                                                                                                                                                                                                              |
|----------------------|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         mail         |      hôte (A)       |                                                        Adresse=*adresse IP publique attribuée par votre fournisseur de services Internet*                                                         |                                                                                                                                                                                                   Exchange Server reçoit le courrier adressé à mail.contoso.com.<br /><br /> Vous pouvez utiliser un autre nom que vous choisissez.                                                                                                                                                                                                    |
|          MX          | serveur de messagerie (MX) |                                            Nom_hôte=@<br /><br /> Adresse=mail.contoso.com<br /><br /> Préférence=0                                             |                                                                                                                                                                                                      Fournit le routage des messages email@contoso.com électroniques pour que se trouve sur votre serveur local qui exécute Exchange Server.                                                                                                                                                                                                       |
|         SPF          |     texte (TXT)      |                                                                        v=spf1 a mx ~all                                                                         |                                                                                                                                                                                                                      Enregistrement de ressource qui permet d’empêcher que les messages électroniques envoyés depuis votre serveur ne soient identifiés comme courrier indésirable.                                                                                                                                                                                                                      |
|  autodiscover._tcp   |    service (SRV)    | Service : _autodiscover<br /><br /> Protocole : _tcp<br /><br /> Priorité : 0<br /><br /> Poids : 0<br /><br /> Port : 443<br /><br /> Hôte cible : mail.contoso.com | Permet à Microsoft Office Outlook et aux appareils mobiles de détecter automatiquement votre serveur local qui exécute Exchange Server.<br /><br /> **Remarque :** Vous pouvez également configurer un enregistrement de ressource d’hôte (A) de découverte automatique et pointer l’enregistrement vers l’adresse IP publique de votre serveur local qui exécute Exchange Server. Toutefois, si vous implémentez cette option, vous devez également fournir un certificat SSL de l’autre nom de l’objet (SAN) qui prend en charge à la fois les noms de domaine mail.contoso.com et autodiscover.contoso.com. |

> [!NOTE]
>  -   Remplacez les instances de *contoso.com* dans cet exemple par le nom de domaine Internet que vous avez inscrit.  

 Vous devez choisir un nom de domaine complet pour votre serveur local qui exécute Exchange Server différent de celui que vous utilisez pour le serveur qui exécute Windows Server Essentials. Par exemple, vous pouvez choisir d'utiliser *remote.contoso.com* comme nom de domaine complet que les ordinateurs utilisent pour accéder au serveur exécutant Windows Server Essentials à partir d'Internet. Vous pouvez utiliser *mail.contoso.com* comme le nom de domaine complet qui est utilisé pour router les messages électroniques vers votre serveur local qui exécute Exchange Server.  

## <a name="install-exchange-server"></a>Installer Exchange Server  
 La fonctionnalité d'intégration Exchange Server dans Windows Server Essentials prend en charge les versions d'Exchange Server suivantes :  

- Exchange Server 2013  

- Exchange Server 2010 avec Service Pack 1 (SP1)  

  Avant d'installer Exchange Server sur le second serveur, vous devez d'abord ajouter le compte d'administrateur actuel au groupe **Enterprise Admins**.  

#### <a name="to-add-the-current-administrator-account-to-the-enterprise-admins-group"></a>Pour ajouter le compte d’administrateur actuel au groupe Administrateurs de l’entreprise  

1.  Ouvrez une session Windows Server Essentials en tant qu'administrateur.  

2.  Exécutez Windows PowerShell en tant qu’administrateur.  

3.  À l’invite de commandes Windows PowerShell, tapez **Add-ADGroupMember "Enterprise Admins" $env: username**, puis appuyez sur entrée.  

#### <a name="to-install-exchange-server"></a>Pour installer Exchange Server  

1.  Ouvrez une session sur le second serveur en tant qu’administrateur.  

2.  Ouvrez votre navigateur Internet, puis accédez au site web [Assistant de déploiement Exchange Server](https://go.microsoft.com/fwlink/p/?LinkID=249163).  

3.  Cliquez sur **On-Premises Only**.  

4.  Cliquez sur la nouvelle option d’installation pour la version d’Exchange Server que vous installez.  

    > [!NOTE]
    >  Si vous procédez à une migration à partir d’une installation de Windows Small Business Server, vous devez sélectionner l’option de mise à niveau appropriée qui couvre les étapes de la migration.  

5.  Dans la page suivante, acceptez les paramètres par défaut, puis cliquez sur **Suivant**.  

    > [!NOTE]
    >  Si vous envisagez d’utiliser des dossiers publics dans la nouvelle installation d’Exchange Server, remplacez ce paramètre par **Oui**.  

6.  Suivez les instructions pas à pas de la liste de vérification pour déployer Exchange Server.  

     L’Assistant de déploiement Exchange Server vous permet également d’effectuer les opérations suivantes :  

    -   imprimer une copie de la liste de vérification ;  

    -   envoyer une copie de la liste de vérification à un destinataire du courrier électronique ;  

    -   télécharger la liste de vérification sous la forme d’un fichier PDF.  

> [!NOTE]
> - Vous devez toujours choisir d’installer les outils de gestion sur le serveur qui exécute Exchange Server. Les outils de gestion sont requis par la fonctionnalité d'intégration Exchange Server dans Windows Server Essentials.  
>   -   Si vous devez configurer des répertoires virtuels, nous vous recommandons d’affecter également à la propriété **InternalUrl** la même URL qu’à la propriété **ExternalUrl** pour chaque répertoire virtuel. Pour plus d’informations, consultez [Gérer les répertoires virtuels du serveur d’accès au client](https://go.microsoft.com/fwlink/p/?LinkId=251058) sur le site web d’aide en ligne d’Exchange Server 2010.  
>   -   Si vous voulez accéder à OWA (Outlook Web Access) à partir du site d'accès web à distance sur Windows Server Essentials, vous devez définir la propriété URL externe pour OWA.  

 Si vous installez Exchange Server 2010 dans le cadre d'une nouvelle installation, vous pouvez également utiliser les scripts suivants pour configurer Exchange Server.  

#### <a name="to-use-scripts-to-set-up-exchange-server"></a>Pour utiliser des scripts pour configurer Exchange Server  

1.  Ouvrez le Bloc-notes et collez le script suivant dans un nouveau fichier :  

```powershell
Import-Module ServerManager

Add-WindowsFeature NET-Framework,RSAT-ADDS,Web-Server,Web-Basic-Auth,Web-Windows-Auth,Web-Metabase,Web-Net-Ext,Web-Lgcy-Mgmt-Console,WAS-Process-Model,RSAT-Web-Server,Web-ISAPI-Ext,Web-Digest-Auth,Web-Dyn-Compression,NET-HTTP-Activation,Web-Asp-Net,Web-Client-Auth,Web-Dir-Browsing,Web-Http-Errors,Web-Http-Logging,Web-Http-Redirect,Web-Http-Tracing,Web-ISAPI-Filter,Web-Request-Monitor,Web-Static-Content,Web-WMI,RPC-Over-HTTP-Proxy  Restart
```

2. Enregistrez le fichier sous le nom **InstallDependencies.ps1**.  

3. Copiez le certificat SSL Exchange à un emplacement sur le serveur.  

4. Ouvrez un nouveau fichier Bloc-notes, puis copiez le texte suivant dans le fichier :  

```powershell
param (
    [string]
    [Parameter(Mandatory=$true, HelpMessage = "The path to your Certificate file, must be a *.pfx format")]
    $CertPath = "c:\certificates\ExchangeCertificate.pfx",
    [Security.SecureString]
    [Parameter(Mandatory=$true, HelpMessage = "The password of your cert")]
    $CertPassword = $null,
    [string]
    [Parameter(Mandatory=$true, HelpMessage = "Domain Name, eg. contoso.com")]
    $DomainName = "contoso.com",
    [string]
    [Parameter(Mandatory=$true, HelpMessage = "Server IP Address, eg. 192.168.0.1")]
    $ServerIpAddress = "192.168.0.1",
    [string]
    [Parameter(Mandatory=$true, HelpMessage = "Internal Ip Range, eg. 192.168.0.0-192.168.0.255")]
    $InternalIpRange = "192.168.0.0-192.168.0.255"
)

#Import Exchange Certificate, and Enable it for POP IIS IMAP SMTP services.

Import-ExchangeCertificate  -FileData ([Byte[]]$(Get-content -Path $CertPath  -Encoding byte  -ReadCount 0)) -Password:$CertPassword -Force | Enable-ExchangeCertificate -Services 'POP, IIS, IMAP, SMTP' -Force

#New AcceptedDomain and set it to default

New-AcceptedDomain  -Name "official name"  -DomainName $domainname

Set-AcceptedDomain  -Identity "official name"  -MakeDefault $true

#New EmailAddress Policy

$address = "%m@" + $DomainName

New-EmailAddressPolicy -Name "Windows Server Essentials Email Address Policy" -IncludedRecipients AllRecipients -EnabledPrimarySMTPAddressTemplate $address

#Set owa and ecp VirtualDirectory ExternalUrl

$hostname = "mail." + $DomainName

$owa = "https://" + $hostname + "/owa"

$ecp = "https://" + $hostname + "/ecp"

$activesync = "https://" + $hostname + "/Microsoft-Server-ActiveSync"

$oab = "https://" + $hostname + "/OAB"

$ews = "https://" + $hostname + "/EWS/Exchange.asmx"

Get-OwaVirtualDirectory | Set-OwaVirtualDirectory  -ExternalUrl $owa  -InternalUrl $owa

Get-EcpVirtualDirectory | Set-EcpVirtualDirectory  -ExternalUrl $ecp  -InternalUrl $ecp

Get-ActiveSyncVirtualDirectory | Set-ActiveSyncVirtualDirectory -ExternalUrl $activesync  -InternalUrl $activesync

Get-OABVirtualDirectory | Set-OABVirtualDirectory -ExternalUrl $oab -InternalUrl $oab -RequireSSL:$true

Get-WebServicesVirtualDirectory | Set-WebServicesVirtualDirectory -ExternalUrl $ews -InternalUrl $ews -BasicAuthentication:$True -Force

#Enable outlook Anywhere

Enable-OutlookAnywhere  -ClientAuthenticationMethod:Basic  -ExternalHostname:$hostname  -SSLOffloading:$false

#new receive/send connector

$machinename = get-content env:computername

$bindingIpaddress = $ServerIpAddress + ":25"

$ReceiveConnectorName = $machinename + "\Default " + $machinename

Set-ReceiveConnector $ReceiveConnectorName -RemoteIPRanges $InternalIpRange

New-ReceiveConnector -Name "WSE Internet Receive Connector" -Usage "Internet" -Bindings $bindingIpaddress -Fqdn $hostname -Enabled $true -Server $machinename -AuthMechanism Tls,BasicAuth,BasicAuthRequireTLS,Integrated

New-SendConnector -Name "WSE Internet SendConnector" -Usage "Internet" -AddressSpaces 'SMTP:*;1' -IsScopedConnector $false -DNSRoutingEnabled $true -UseExternalDNSServersEnabled $true -SourceTransportServers $machinename
```

5. Définissez les paramètres au début du script pour refléter votre environnement réseau.  

6. Enregistrez le fichier avec le nom **ConfigureExchange.ps1**.  

7. Exécutez Windows PowerShell en tant qu’administrateur.  

8. À l’invite de commandes Windows PowerShell, tapez **Set-ExecutionPolicy RemoteSigned**, puis appuyez sur Entrée.  

9. Exécutez le script **InstallDependencies.ps1**.  

10. Redémarrez le serveur, puis exécutez Windows PowerShell en tant qu'administrateur.  

11. À l'invite de commandes Windows PowerShell, exécutez le script suivant :  

    `E:\setup.com /mode:install /roles:mb,ht,ca /OrganizationName:"First Organization"`  

   > [!NOTE]
   >  Assurez-vous de taper le chemin d’accès correct au programme d’installation d’Exchange Server.  

12. Lorsque l’installation d’Exchange Server est terminée, ouvrez l’environnement de ligne de commande Exchange Management Shell en tant qu’administrateur.  

13. À l'invite de commandes de l'environnement Exchange Management Shell, tapez **Set-ExecutionPolicy RemoteSigned**, puis appuyez sur Entrée.  

14. Exécutez le script **ConfigureExchange.ps1**.  

15. Redémarrez le serveur.  

> [!NOTE]
>  Si vous décidez d’utiliser un certificat SSL publiquement approuvé au lieu d’un certificat auto-émis, vous pouvez suivre les instructions du Guide d’installation pour créer une demande de certificat et l’envoyer à votre autorité de certification sélectionnée. Vous pouvez également utiliser une applet de commande PowerShell Exchange pour créer une demande de certificat. Un exemple suit.  
>   
>  `New-ExchangeCertificate -GenerateRequest -SubjectName "C=US, S=Washington, L=Redmond, O=contoso, OU=contoso, CN=mail.contoso.com" -DomainName mail.contoso.com -PrivateKeyExportable $true | Set-Content -path "c:\Docs\MyCertRequest.req"`  
>   
>  Personnalisez les paramètres du script pour refléter votre environnement réseau.  

## <a name="post-installation-tasks"></a>Tâches de post-installation  
 Cette section décrit les tâches de configuration du serveur que vous devrez peut-être effectuer dans la phase de post-installation qui contient des informations spécifiques à la configuration d'un serveur local qui exécute Exchange Server sur un réseau Windows Server Essentials.  

### <a name="add-the-public-email-domain-and-configure-the-email-address-policies"></a>Ajouter le domaine de messagerie public et configurer les stratégies d’adresses de messagerie  

> [!NOTE]
>  Il s’agit d’une tâche obligatoire si vous procédez à une nouvelle installation. Ignorez cette étape si vous effectuez une migration à partir de Windows Small Business Server.  

 Vous devez spécifier votre domaine de messagerie comme étant le domaine accepté par défaut, puis configurer la stratégie d’adresses de messagerie.  

##### <a name="to-add-your-email-domain-as-the-default-accepted-domain"></a>Pour ajouter votre domaine de messagerie comme le domaine accepté par défaut  

1.  Suivez les instructions de l’article Exchange Server [Créer un domaine accepté](https://go.microsoft.com/fwlink/p/?LinkId=249174) pour ajouter un domaine accepté.  

2.  Ouvrez une session sur le second serveur en tant qu’administrateur, ouvrez la console de gestion Exchange, puis accédez à l’onglet **Transport Hub** de **Configuration de l’organisation**.  

3.  Dans le volet de travail de la console de gestion Exchange, cliquez avec le bouton droit sur le nouveau domaine accepté, puis cliquez sur **Par défaut**.  

4.  Suivez les instructions de l’article Exchange Server [Créer une stratégie d’adresses de messagerie](https://go.microsoft.com/fwlink/p/?LinkId=249179) pour créer une stratégie d’adresses de messagerie. Vous pouvez accepter toutes les valeurs par défaut, à l'exception de l'adresse de messagerie. Pour l'adresse de messagerie, indiquez votre domaine de messagerie public.  

### <a name="create-smtp-send-and-receive-connectors"></a>Créer des connecteurs d'envoi et de réception SMTP  

> [!NOTE]
>  Cette tâche est obligatoire.  

 Vous devez configurer un connecteur d'envoi SMTP et un connecteur de réception SMTP pour la transmission sortante/entrante des messages électroniques.  

 Pour créer un connecteur d’envoi SMTP, suivez les instructions de l’article Exchange Server [Créer un connecteur d’envoi SMTP](https://technet.microsoft.com/library/aa997285.aspx).  

 Pour créer un connecteur de réception SMTP, suivez les instructions de l’article Exchange Server [Créer un connecteur de réception SMTP](https://technet.microsoft.com/library/bb125159.aspx).  

 Vous pouvez également vous reporter au script abordé plus tôt dans ce document pour la création des connecteurs d'envoi et de réception à l'aide des applets de commande PowerShell Exchange.  

### <a name="configure-the-network-router"></a>Configurer le routeur réseau  

> [!NOTE]
>  Il s’agit d’une tâche obligatoire si vous procédez à une nouvelle installation. Si vous effectuez une migration à partir de Windows Small Business Server, consultez la rubrique [Migrate Server Data to Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md) pour obtenir des instructions sur la configuration du réseau.  

 Vous devez au minimum configurer les paramètres de port suivants sur le routeur :  

|Port de routeur|IP de destination|Port de destination|Remarque|  
|-----------------|--------------------|----------------------|----------|  
|25 (SMTP)|Adresse IP interne du serveur local qui exécute Exchange Server.|25||  
|80 (HTTP)|Adresse IP interne du serveur qui exécute Windows Server Essentials|80||  
|443 (HTTPS)|Adresse IP interne du serveur qui exécute Windows Server Essentials|443||  

 Si vous prenez en charge les protocoles de messagerie POP3 ou IMAP sur votre réseau, vous devez également configurer les réacheminements de ports pour ces protocoles. Pour obtenir des informations connexes, consultez la section **Serveurs d’accès au client** de la rubrique [Référence de port de réseau Exchange](https://go.microsoft.com/fwlink/p/?LinkId=250773) dans la bibliothèque technique Exchange Server TechNet.  

> [!NOTE]
> - Nous vous recommandons de configurer des adresses IP statiques pour le serveur qui exécute Windows Server Essentials et pour le second serveur qui exécute Exchange Server. Pour obtenir des instructions sur la manière de configurer une adresse IP statique sur un ordinateur exécutant Windows Server 2003 ou Windows Server 2008 R2, consultez la rubrique [Configurer une adresse IP statique](https://technet.microsoft.com/library/cc754203\(v=ws.10\).aspx) dans la bibliothèque Windows Server TechNet.  
> 
>   **Remarque :** Le paramètre de serveur DNS doit toujours pointer vers l'adresse IP du serveur qui exécute Windows Server Essentials.  
>   -   Sur le routeur, assurez-vous que les adresses IP du serveur qui exécute Windows Server Essentials et du serveur qui exécute Exchange Server sont réservées ou hors de la plage d'adresses IP DHCP.  
>   -   La configuration du routeur dans cette section part du principe que vous ne disposez que d’une seule adresse IP publique attribuée par votre fournisseur de services Internet. Si vous avez plusieurs adresses IP publiques, vous pouvez attribuer une autre adresse IP au serveur qui exécute Windows Server Essentials et au serveur qui exécute Exchange Server, puis créer des réacheminements de ports selon les adresses IP publiques.  

### <a name="enable-on-premises-exchange-server-integration-on-windows-server-essentials"></a>Activer l'intégration du serveur Exchange local sur Windows Server Essentials  

> [!NOTE]
>  Si vous effectuez une migration à partir d’une installation de Windows Small Business Server, nous vous conseillons d’ignorer cette étape pour l’instant et de l’exécuter après avoir désinstallé l’installation précédente d’Exchange Server sur le serveur source.  

 Après avoir installé et configuré un serveur qui exécute Exchange Server, vous devez activer l'intégration du serveur Exchange local sur le serveur qui exécute Windows Server Essentials.  

##### <a name="to-enable-on-premises-exchange-server-integration-from-the-dashboard"></a>Pour activer l'intégration du serveur Exchange local à partir du tableau de bord  

1.  Ouvrez une session sur le serveur qui exécute Windows Server Essentials en tant qu'administrateur, puis ouvrez le tableau de bord.  

2.  Dans la page d’ **accueil** , cliquez sur **Connexion à mon service de messagerie**, puis sur **Intégrer votre serveur Exchange**.  

3.  Dans le volet d’informations, cliquez sur **Configurer l’intégration du serveur Exchange**.  

4.  Suivez les instructions de l'Assistant.  

### <a name="configure-a-reverse-proxy"></a>Configurer un proxy inverse  

> [!NOTE]
>  Il s'agit d'une tâche obligatoire si vous ne disposez que d'une seule connexion Internet de votre fournisseur de services Internet.  

 Windows Server Essentials et Exchange Server prennent tous deux en charge des scénarios d'accès à distance pour les utilisateurs du réseau. Par exemple, si vous activez l'accès en tout lieu sur le serveur qui exécute Windows Server Essentials, vous pouvez accéder à distance au site d'accès web à distance ou utiliser un réseau privé virtuel (VPN) pour vous connecter à distance au réseau Windows Server Essentials. Pour accéder à distance aux messages électroniques, vous devez utiliser Outlook Anywhere, OWA (Outlook Web Access) ou ActiveSync.  

 Si Windows Server Essentials et le serveur exécutant Exchange Server sont tous deux connectés au même routeur et qu'il n'existe qu'une seule connexion Internet entrante de votre fournisseur de services Internet au routeur, vous devez utiliser une solution de proxy inverse pour router différents types de demandes d'accès à distance à partir d'Internet selon les noms des hôtes de destination. Nous vous conseillons d’utiliser l’extension ARR (Application Request Routing) IIS prise en charge par Microsoft en tant que solution de proxy inverse. Pour plus d’informations sur ARR IIS, visitez le [site web Application Request Routing](https://go.microsoft.com/fwlink/p/?LinkId=249181).  

##### <a name="to-install-and-configure-application-request-routing"></a>Pour installer et configurer ARR  

1. Ouvrez une session Windows Server Essentials en tant qu'administrateur.  

2. Ouvrez votre navigateur Internet, puis accédez au [site Web Application Request Routing](https://go.microsoft.com/fwlink/p/?LinkId=249181).  

3. Sur le site web ARR, cliquez sur le bouton **Installer**, puis suivez les instructions pour installer ARR.  

   > [!NOTE]
   >  Vous devez sélectionner le module de réécriture d’URL pendant l’installation d’ARR.  
   >   
   >  Vous pouvez recevoir un message d’erreur à la fin de l’installation d’ARR indiquant que le correctif de la Base de connaissances 2589179 pour ARR 2.5 n’est pas correctement installé. Vous pouvez ignorer cette erreur en toute sécurité.  

4. Lorsque l’installation d’ARR est terminée, redémarrez le service **Passerelle des services Bureau à distance** s’il n’est pas en cours d’exécution.  

   > [!NOTE]
   >  Après avoir installé ARR, le service **Passerelle des services Bureau à distance** peut être arrêté. Pour redémarrer manuellement le service, ouvrez l’outil d’administration **Services** , puis redémarrez le service **Passerelle des services Bureau à distance** .  

5. [Téléchargez le correctif de la Base de connaissances KB2732764 pour ARR 2.5](https://go.microsoft.com/fwlink/?LinkID=258302), puis installez la mise à jour sur le serveur qui exécute Windows Server Essentials.  

6. Copiez le fichier de certificat SSL pour Exchange Server sur le serveur qui exécute Windows Server Essentials. Le fichier de certificat doit contenir la clé privée et se présenter au format de fichier PFX.  

   > [!NOTE]
   >  Si vous utilisez un certificat auto-émis, suivez les instructions de l’article Exchange Server [Exporter un certificat Exchange](https://technet.microsoft.com/library/dd351274.aspx) pour exporter le certificat.  

7. Selon la version de Windows Server Essentials que vous utilisez, effectuez l’une des opérations suivantes :  

   -   Sur Windows Server Essentials: Ouvrez une fenêtre de commandes en tant qu’administrateur, puis le répertoire %ProgramFiles%\Windows Server\Bin.  

   -   Sur Windows Server Essentials: Ouvrez une fenêtre de commandes en tant qu’administrateur, puis le répertoire %Windir%\System32\Essentials.  

8. Selon votre scénario d'installation, procédez de l'une des façons suivantes pour configurer ARR :  

   - Si vous procédez à une nouvelle installation, exécutez la commande suivante :  

      **Configuration de ARRConfig-CERT** _chemin d’accès au fichier de certificat_ **-noms d’hôte** _noms d’hôtes pour Exchange Server_  

     > [!NOTE]
     >  Par exemple, **Configuration de ARRConfig-CERT** _c:\temp\certificate.pfx_ **-noms d’hôte** _mail.contoso.com_  
     > 
     >  Remplacez *mail.contoso.com* par le nom de votre domaine qui est protégé par le certificat.  

   - Si vous effectuez une migration à partir de Windows Small Business Server, exécutez la commande suivante :  

      **Configuration de ARRConfig-CERT** _chemin d’accès au fichier de certificat_ **-noms d’hôte** _noms d’hôtes pour Exchange Server_ **-TargetServer** _nom du serveur Exchange Server_  

      Par exemple, **Configuration de ARRConfig-CERT** _c:\temp\certificate.pfx_ **-noms d’hôte** _mail.contoso.com_ **-TargetServer** _ExchangeSvr_  

      Remplacez *mail.contoso.com* par le nom de votre domaine. Remplacez *ExchangeSvr* par le nom de votre serveur qui exécute Exchange Server.  

9. Lorsque vous y êtes invité, tapez le mot de passe pour le certificat.  

> [!NOTE]
> - Les noms des hôtes que vous indiquez doivent être contenus dans le certificat SSL que vous avez acheté pour Exchange Server.  
>   -   Si vous avez plusieurs noms d’hôtes, utilisez une virgule (,) pour les séparer.  

 Pour vérifier que la configuration fonctionne, essayez d’accéder au site Web d’OWA pour votre serveur qui exécute Exchange Server https://mail (. *votre_nom_de_domaine*.com/owa) à partir d’un ordinateur qui n’est pas membre du domaine. Pour résoudre les problèmes de connectivité, vous pouvez également employer l’outil [Analyseur de connectivité à distance Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=249455) en ligne.  

### <a name="configure-split-dns-for-exchange-server"></a>Configurer un environnement DNS fractionné pour Exchange Server  

> [!NOTE]
>  Il s’agit d’une tâche conseillée.  

 Un environnement DNS fractionné vous permet de configurer différentes adresses IP dans DNS pour le même nom d’hôte, en fonction de l’origine de la requête DNS. Si l’ordinateur client se trouve sur l’intranet, la requête DNS est résolue en une adresse IP intranet. Si l’ordinateur client se trouve sur Internet, la requête DNS est résolue en une adresse IP Internet. Ceci est transparent pour les utilisateurs.  

 Nous vous conseillons de configurer un environnement DNS fractionné de façon à permettre aux utilisateurs de toujours employer le même nom d’hôte pour accéder aux services Exchange Server, quel que soit leur emplacement.  

##### <a name="to-configure-split-dns-for-exchange-server"></a>Pour configurer un environnement DNS fractionné pour Exchange Server  

1.  Ouvrez une session sur Windows Server Essentials en tant qu'administrateur, puis ouvrez le Gestionnaire DNS.  

2.  Dans l'arborescence de la console du Gestionnaire DNS, cliquez avec le bouton droit sur votre serveur, puis cliquez sur **Nouvelle zone**. L’ **Assistant Nouvelle zone** s’affiche.  

3.  Dans la page **Type de zone** de l’Assistant, acceptez l’option par défaut et cliquez sur **Suivant**.  

4.  Dans la page **Étendue de la zone de réplication de Active Directory**, acceptez l’option par défaut et cliquez sur **Suivant**.  

5.  Dans la page **Zone de recherche directe ou inversée** , acceptez ou sélectionnez **Zone de recherche directe**, puis cliquez sur **Suivant**.  

6.  Dans la page **Nom de la zone** , tapez le nom de domaine complet de votre serveur qui exécute Exchange Server (par exemple, *mail.contoso.com*), puis cliquez sur **Suivant**.  

7.  Dans la page **Mise à niveau dynamique**, acceptez l’option par défaut, cliquez sur **Suivant**, puis sur **Terminer**.  

8.  Dans l’arborescence de la console du Gestionnaire DNS, cliquez avec le bouton droit sur la nouvelle zone de recherche directe, puis cliquez sur **Nouvel hôte (A ou AAAA)** .  

9. Dans la page **Nouvel hôte**, laissez le champ **Nom** vide, tapez l'adresse IP intranet de votre serveur qui exécute Exchange Server, puis cliquez sur **Ajouter un hôte**.  

    > [!NOTE]
    >  Lorsque vous laissez le champ **Nom** vide, le serveur utilise le nom de domaine parent par défaut.  

10. Dans la page **Nouvel hôte**, cliquez sur **Terminé**.  

> [!NOTE]
>  Si vous utilisez ActiveSync mais que vous ne pouvez pas synchroniser le courrier électronique pour certains comptes de boîte aux lettres, déterminez si ces comptes sont membres d’un ou de plusieurs groupes protégés, par exemple Administrateurs de domaine. Pour obtenir des informations connexes qui peuvent vous aider à résoudre ce problème, voir [Exchange ActiveSync a renvoyé une erreur HTTP 500](https://technet.microsoft.com/library/dd439375\(EXCHG.80\).aspx).  

## <a name="related-topics"></a>Rubriques connexes  
 Pour plus d'informations sur l'intégration d'un serveur Exchange Server local, consultez les sections suivantes.  

### <a name="what-happens-if-i-disable-exchange-integration"></a>Que se passe-t-il si je désactive l'intégration Exchange ?  
 Si vous désactivez l'intégration avec un serveur Exchange Server local, vous ne pourrez plus utiliser le tableau de bord Windows Server Essentials pour afficher, créer et gérer les boîtes aux lettres Exchange Server.  

### <a name="what-do-i-need-to-know-about-email-accounts"></a>Que dois-je savoir sur les comptes de messagerie ?  
 Une solution de messagerie hébergée est configurée sur votre serveur. Une solution d’un fournisseur de messagerie hébergé, telle que Microsoft Office 365, peut fournir des comptes de messagerie individuels pour les utilisateurs du réseau. Quand vous exécutez l'Assistant Ajouter un compte d'utilisateur dans Windows Server Essentials pour créer un compte d'utilisateur, l'Assistant tente d'ajouter le compte d'utilisateur à la solution de messagerie hébergée disponible. En même temps, l'Assistant attribue un nom de messagerie (alias) à l'utilisateur et il définit la taille maximale de la boîte aux lettres (quota). La taille maximale de la boîte aux lettres varie en fonction du fournisseur de messagerie que vous utilisez. Après avoir ajouté le compte d'utilisateur, vous pouvez continuer à gérer les informations d'alias et les quotas de boîtes aux lettres à partir de la page de propriétés de l'utilisateur. Pour effectuer la gestion complète de vos comptes d'utilisateur et du fournisseur de messagerie hébergé, utilisez la console de gestion de votre fournisseur hébergé. Selon votre fournisseur, vous pouvez accéder à sa console de gestion à partir d'un portail web ou d'un onglet dans le tableau de bord du serveur.  

 L'alias que vous fournissez quand vous exécutez l'Assistant Ajouter un compte d'utilisateur est envoyé au fournisseur de messagerie hébergé comme nom suggéré pour l'alias de l'utilisateur. Par exemple, si l’alias d’utilisateur est *Frank*, l’adresse de messagerie de l’utilisateur <em>FrankM@Contoso.com</em>peut être.  

 De plus, le mot de passe que vous définissez pour l'utilisateur dans l'Assistant Ajouter un compte d'utilisateur sera le mot de passe initial de l'utilisateur dans la solution de messagerie hébergé.  

 Pour finir, si vous supprimez l'utilisateur à l'aide de l'Assistant Supprimer un compte d'utilisateur sur le serveur, l'Assistant envoie une demande au fournisseur de messagerie hébergé pour qu'il supprime aussi l'utilisateur de son système. Le fournisseur peut supprimer le compte de l’utilisateur et l’adresse de messagerie associée au compte.  

 Pour l'utilisateur d'informations sur la façon de configurer le logiciel de messagerie client nécessaire ou d'accéder à un compte de messagerie, consultez la documentation fournie par votre fournisseur de messagerie hébergé.  

### <a name="what-is-a-mailbox-quota"></a>Qu'est-ce qu'un quota de boîte aux lettres ?  
 La quantité d’espace de stockage allouée pour les données de boîte aux lettres Exchange d’un utilisateur réseau est appelée quota de boîtes aux lettres.  

 Quand vous exécutez la tâche **Configurer l'intégration du serveur Exchange** du tableau de bord, l'Assistant ajoute une page à l'Assistant Ajouter un compte d'utilisateur qui vous permet de choisir si vous souhaitez appliquer des quotas de boîtes aux lettres et de spécifier la taille des quotas. Par défaut, l'option **Appliquer des quotas de boîtes aux lettres** est activée et 2 Go d'espace de stockage sont affectés aux boîtes aux lettres des utilisateurs. Les administrateurs Exchange peuvent personnaliser les paramètres de quota de boîtes aux lettres en fonction des besoins de leur entreprise.  

## <a name="see-also"></a>Voir aussi  

-   [Configuration requise pour Windows Server Essentials](../get-started/system-requirements.md)  

-   [Gérer l’intégration du service de messagerie](Manage-Email-Service-Integration-in-Windows-Server-Essentials.md)  

-   [Gérer Windows Server Essentials](Manage-Windows-Server-Essentials.md)
