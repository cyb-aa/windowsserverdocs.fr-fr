---
title: "Hébergé WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fda5628c-ad23-49de-8d94-430a4f253802
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 23e586c22ca14af7b02550e2fa1c9522e379170c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="hosted-windows-server-essentials"></a>Hébergé WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Ce document contient des informations spécifiques aux hébergeurs souhaitant déployer Windows Server Essentials dans leur laboratoire et proposer Windows Server Essentials en tant que service à leurs clients.  
  
## <a name="what-is-windows-server-essentials"></a>Nouveautés de Windows Server Essentials?  
 Windows Server Essentials est une solution de petite entreprise intersite, qui intègre des technologies de pointe, 64 bits de produit pour fournir un environnement de serveur adapté à la grande majorité des petites entreprises. Les technologies suivantes sont incluses dans Windows Server Essentials.  
  
 **Système d’exploitation de serveur:** technologies Windows Server 2012 fournissent la base de Windows Server Essentials. Pour plus d’informations, visitez le [site Web de Windows Server 2012](https://www.microsoft.com/en-us/server-cloud/products/windows-server-2012-r2/default.aspx#fbid=ZH0GD_CRAWh).  
  
 **Protection des données:** Windows Server Essentials exploite plusieurs nouvelles fonctionnalités disponibles dans Windows Server 2012 pour fournir des capacités de protection de données considérablement améliorées. Le [nouvelle fonctionnalité espaces de stockage](https://technet.microsoft.com/library/hh831739.aspx) vous permet d’agréger la capacité de stockage physique de différents disques durs, ajouter des disques durs dynamiquement et de créer des volumes de données avec des niveaux de résilience. Windows Server Essentials peut effectuer des sauvegardes complètes du système et restaure les complète du serveur lui-même, ainsi que les ordinateurs clients connectés au réseau? prend désormais en charge pour les volumes supérieurs à 2 To. Nouveauté de Windows Server 2012, le [Windows Azure Online Backup](https://technet.microsoft.com/library/hh831419.aspx) peut être utilisé pour protéger les fichiers et dossiers dans un service de stockage en cloud est géré par Microsoft. Windows Server Essentials gère également de manière centralisée et configure la fonctionnalité de l’historique des fichiers de clients Windows 8.1, aider les utilisateurs à récupérer des fichiers accidentellement supprimés ou remplacés sans avoir besoin d’assistance de l’administrateur.  
  
 **Accès en tout lieu:** accès Web à distance fournit une expérience de navigateur rationalisée et tactile pour accéder à des applications et des données depuis pratiquement n’importe où vous disposez d’une connexion Internet et à l’aide de presque n’importe quel appareil. Windows Server Essentials fournit également une application Windows Phone mise à jour et une nouvelle application pour Windows 8.1 client ordinateurs, permettant aux utilisateurs de manière intuitive de se connecter pour rechercher dans et accéder aux fichiers et dossiers sur le serveur. Les fichiers sont également automatiquement mis en cache pour l’accès hors connexion et synchronisées lorsqu’une connexion au serveur est disponible. Windows Server Essentials Active la configuration d’un réseau privé virtuel (VPN) dans un processus simple, pilotée par Assistant de quelques clics et simplifie la gestion de l’accès VPN pour les utilisateurs. Les ordinateurs clients peuvent tirer parti d’une connexion VPN pour participer à l’environnement Windows SBS sans avoir à le rendre au Bureau à distance.  
  
 **Flexibilité de la charge de travail:** Windows Server Essentials a été conçu pour permettre aux clients la possibilité de choisir les applications et services exécutés sur le site et qui s’exécutent dans le cloud. Dans les versions précédentes, Windows Small Business Server Standard inclus Exchange Server en tant que composant produit, ce qui entraînait des dépenses et complexité pour les clients souhaitant exploiter les services de messagerie et de collaboration basés sur le cloud. Avec Windows Server Essentials, les clients peuvent tirer parti du même type d’expérience de gestion intégrée qu’ils choisissent d’exécuter une copie locale d’Exchange Server, vous abonner à un service Exchange hébergé ou vous abonner à Microsoft Office 365.  
  
 **Surveillance de l’intégrité:** Windows Server Essentials analyse son propre état d’intégrité et l’état des ordinateurs clients qui exécutent Windows 8.1, Windows 7 et Mac OS X version 10.5 ou version ultérieure. État d’intégrité vous informe des problèmes ou des problèmes liés aux sauvegardes de l’ordinateur, stockage de serveur, un espace disque faible et plus encore.  
  
 **Extensibilité:** Windows Server Essentials s’appuie sur le modèle d’extensibilité de Windows SBS 2011 Essentials, qui permet d’autres éditeurs de logiciels ajouter des capacités et des fonctionnalités au produit de base et ajoute un nouvel ensemble de web services API. Il assure aussi la compatibilité avec le [kit de développement logiciel](https://msdn.microsoft.com/library/gg513958.aspx) (SDK) et [compléments](https://pinpoint.microsoft.com/applications/search?fpt=300105&q=small+business+server+essentials) créés pour Windows SBS 2011 Essentials.  
  
## <a name="how-can-i-customize-an-image"></a>Comment puis-je personnaliser une image?  
 Reportez-vous à la [Windows Server Essentials](https://go.microsoft.com/fwlink/p/?LinkID=249124), qui est un processus sysprep standard de Windows Server avec des étapes de personnalisation supplémentaires de Windows Server Essentials. Pour terminer la personnalisation, suivez les instructions de [créer une Image personnalisée Simple](https://technet.microsoft.com/en-us/library/jj200117) et [personnaliser l’Image](https://technet.microsoft.com/en-us/library/jj200161), puis suivez les instructions de [préparation de l’Image pour le déploiement](https://technet.microsoft.com/en-us/library/jj200142) pour capturer votre image finale.  
  
 Vous devez prêter attention sur les points suivants:  
  
1.  Vous devez ignorer la Configuration initiale (IC) en ajoutant un fichier SkipIC.txt à la racine de n’importe quel lecteur. Après l’installation du serveur, avant la configuration initiale, appuyez sur MAJ + F10 pour lancer la fenêtre cmd et créer un fichier SkipIC.txt sous le lecteur C: / lecteur. Une fois la personnalisation, vous devez penser à supprimer le fichier SkipIC.txt.  
  
2.  Si vous avez besoin déployer Windows Server Essentials sur un disque inférieur à 90 Go, vous devez ajouter une clé de Registre avant sysprep:  
  
    ```  
    %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v HWRequirementChecks /t REG_DWORD /d 0 /f  
    ```  
  
 Après avoir exécuté sysprep, vous pouvez utiliser l’image de disque préparée avec Sysprep, ou la retransférer dans Install.wim pour le nouveau déploiement.  
  
 Si vous utilisez Virtual Machine Manager, vous pouvez créer un modèle à l’aide de l’instance en cours d’exécution. Création d’un modèle préparera l’instance et arrêter le serveur. Une fois que vous le stockez dans votre bibliothèque, vous pouvez mettre l’instance au cas par cas.  
  
##  <a name="BKMK_automatedeployment"></a>Comment pour automatiser le déploiement?  
 Après avoir obtenu l’image personnalisée, vous pouvez effectuer le déploiement avec votre propre image. Pour effectuer une installation, vous devez fournir/déployer le fichier unattend.xml pour la configuration de WinPE. Pour effectuer une installation entièrement sans assistance, vous devez également fournir le fichier cfg.ini pour la Configuration initiale de Windows Server Essentials.  
  
1.  Effectuer uniquement une installation sans assistance WinPE. Cela automatiser uniquement la configuration de WinPE et arrêtera avant la Configuration initiale, afin que les utilisateurs finaux peut fournir des informations Corp, domaine et l’administrateur par eux-mêmes, via RDP dans une session de serveur. Pour ce faire:  
  
    1.  Fournissez le fichier unattend.xml Windows. Suivez les [Windows 8.1 ADK](https://go.microsoft.com/fwlink/?LinkId=248694) pour générer le fichier et fournir toutes les informations nécessaires, y compris le nom du serveur, les clés de produit et mot de passe administrateur. Dans la section Microsoft-Windows-Setup du fichier unattend.xml, fournissez les informations comme indiqué ci-dessous.  
  
        ```  
        <InstallFrom>  
                 <MetaData>  
                     <Key>IMAGE/WINDOWS/EDITIONID</Key>  
                     <Value>ServerSolution</Value>  
                 </MetaData>  
                 <MetaData>  
                     <Key>IMAGE/WINDOWS/INSTALLATIONTYPE</Key>  
                     <Value>Server</Value>  
                 </MetaData>  
           </InstallFrom>  
        ```  
  
    2.  Le port RDP 3389 doit être ouvert sur un IP public afin que le client puisse utiliser administrateur et le mot de passe spécifié dans le fichier unattend.xml à RDP sur le serveur pour terminer la Configuration initiale.  
  
    > [!NOTE]
    >  Si vous ne modifiez pas le mot de passe par défaut, l’installation du serveur s’arrête sur un écran demandant à entrer un mot de passe. **Remarque** utilisateurs finaux doivent utiliser le compte d’administrateur par défaut pour ouvrir une session sur le serveur et effectuer la Configuration initiale.  
  
 Si vous utilisez Virtual Machine Manager, vous pouvez spécifier le mot de passe administrateur dans la console lorsque vous créez une nouvelle instance à partir du modèle.  
  
1.  Effectuer une installation sans assistance complète, y compris la Configuration initiale sans assistance. Pour ce faire:  
  
    1.  Fournissez le fichier unattend.xml comme vous l’avez fait ci-dessus, si le déploiement démarre à partir de la configuration de WinPE.  
  
    2.  Reportez-vous à la section Windows Server Essentials ADK, [créer le fichier Cfg.ini](https://technet.microsoft.com/en-us/library/jj200150), afin de générer le fichier cfg.ini.  
  
    3.  Fournir des informations dans [InitialConfiguration].  
  
        ```  
        WebDomainName=yourdomainname  
        TrustedCertFileName=c:\cert\a.pfx  
        TrustedCertPassword=Enteryourpassword  
        EnableVPN=true  
        EnableRWA=true  
        ; Provide all information so that after setup is complete, your customer can use your domain name to visit the server directly with the admin/user information you provide in the [InitialConfiguration] section.  
  
        VpnIPv4StartAddress=<IPV4Address>  
        VpnIPv4EndAddress=<IPV4Address>  
        VpnBaseIPv6Address=<IPV6Address>  
        VpnIPv6PrefixLength=<number>  
        ; Provide this information. IPv4StartAddress and IPv4Endaddress are required so that your VPN client can acquire valid IP through this range.  
  
        IPv4DNSForwarder=<IPV4Address,IPV4Address,Â¦>  
        IPv6DNSForwarder=<IPV6Address,IPV6Address,Â¦>  
        ; Provide this information as needed according to your network environment settings.  
        ```  
  
    4.  Fournir des informations dans [PostOSInstall].  
  
        ```  
        IsHosted=true   
        ; Must have, this will prevent Initial Configure webpage available for other computers under same subnet.  
  
        StaticIPv4Address=<IPV4Address>  
        StaticIPv4Gateway=<IPV4Address>  
        StaticIPv6Address=<IPV6Address>  
        StaticIPv6SubnetPrefixLength=<number>  
        StaticIPv6Gateway=<IPV6Address>  
        ; All these are optional if you have DHCP Server Service on the subnet, otherwise provide static IP here.  
        ```  
  
    5.  Si vous fournissez le paramètre WebDomainName, assurez-vous que l’enregistrement DNS est également mis à jour pour pointer vers l’adresse IP publique de s de serveur.  
  
    6.  Si vous n’avez pas fourni les informations du WebDomainName ci-dessus, veillez à ouvrir le port 3389 afin que les clients puissent utiliser le RDP pour se connecter au serveur et terminer les configurations du VPN.  
  
> [!NOTE]
>  Assurez-vous que le paramètre de fuseau horaire du serveur hôte de la machine virtuelle et l’ordinateur virtuel Windows Server Essentials est le même. Dans le cas contraire, vous pouvez rencontrer différentes erreurs (Échec de la Configuration initiale des tâches de certificat, certificat peut ne pas fonctionner pendant quelques heures après l’installation; informations sur le périphérique ne met pas à jour correctement, et ainsi de suite).  
  
 Après le déploiement, vérifiez la clé de Registre suivante sous HKLM\software\microsoft\windows server\setup pour vérifier si la Configuration initiale a réussi. Si SetupStage == ICDone & & ICStatus == 1, cela signifie que la Configuration initiale a abouti.  
  
## <a name="what-is-the-supported-network-topology"></a>Quelle est la topologie de réseau prises en charge?  
 Pour utiliser Windows Server Essentials à partir d’un client itinérant, VPN doit être activé. Nous vous recommandons d’activer le port 443 pour les connexions VPN SSTP. Si la fonctionnalité de serveur multimédia est nécessaire pour les applications accès Web à distance ou le Service Web, port 80 doit également être activé.  
  
 L’activation du VPN peut être effectuée pendant le déploiement sans assistance via notre script Windows PowerShell, ou il peut être configuré avec notre Assistant après la configuration initiale.  
  

-   Pour activer VPN pendant le déploiement sans assistance, voir [comment pour automatiser le déploiement? ](Hosted-Windows-Server-Essentials.md#BKMK_automatedeployment) dans ce document.  

-   Pour activer VPN pendant le déploiement sans assistance, voir [comment pour automatiser le déploiement? ](../install/Hosted-Windows-Server-Essentials.md#BKMK_automatedeployment) dans ce document.  

  
-   Pour activer VPN via Windows PowerShell, exécutez la cmdlet suivante avec des privilèges d’administration et fournir toutes les informations nécessaires.  
  
    ```  
    ##  
    ## To configure external domain and SSL certificate (if not yet done in unattended Initial Configuration).  
    ##  
  
    $myExternalDomainName = 'remote.contoso.com';   ## corresponds to A or AAAA DNS record(s) that can be resolved on Internet and routed to the server  
    $mySslCertificateFile = 'C:\ssl.pfx';   ## full path to SSL certificate file  
    $mySslCertificatePassword = ConvertTo-SecureString '******';   ## password for private key of the SSL certificate  
    $skipCertificateVerification = $true;   ## whether or not, skip verification for the SSL certificate  
  
    Add-Type -AssemblyName 'Wssg.Web.DomainManagerObjectModel';  
    [Microsoft.WindowsServerSolutions.RemoteAccess.Domains.DomainConfigurationHelper]::SetDomainNameAndCertificate($myExternalDomainName,$mySslCertificateFile,$mySslCertificatePassword,$skipCertificateVerification);  
    ##  
    ## To install VPN with static IPv4 pool (and allow all existing users to establish VPN).  
    ##  
  
    Install-WssVpnServer -IPv4AddressRange ('192.168.0.160','192.168.0.240') -ApplyToExistingUsers;  
    ```  
  
 Si vous ne pouvez pas fournir une connexion VPN avant de donner le serveur aux clients, assurez-vous que le port serveur 3389 est accessible sur Internet afin que les utilisateurs finaux peuvent utiliser le RDP pour se connecter au serveur et effectuer la configuration par eux-mêmes.  
  
 Voici les deux topologies de mise en réseau de côté serveur classiques, et comment le VPN/la télécommande Web accès à distance peut être configuré:  
  
-   Topologie 1 (préférée)  
  
    -   Serveur dans un réseau virtuel distinct sous un périphérique NAT.  
  
    -   Service DHCP est activé dans le réseau virtuel, ou le serveur est attribué avec une adresse IP statique.  
  
    -   Port de serveur 443 est accessible depuis le port IP public 443.  
  
    -   Passerelle VPN est autorisé pour le port 443.  
  
    -   Pool d’adresses VPN IPv4 doit se trouver dans le même sous-réseau de l’adresse du serveur.  
  
    -   Une adresse IP statique au sein du même sous-réseau, mais pas dans le pool d’adresses VPN doit être attribué à un second serveur.  
  
-   Topologie 2:  
  
    -   Le serveur a une adresse IP privée.  
  
    -   Le port 443 sur le serveur est accessible à partir d’un port de s adresse IP public 443.  
  
    -   Passerelle VPN est autorisé pour le port 443.  
  
    -   Pool d’adresses VPN IPv4 se trouve dans une plage différente de l’adresse du serveur.  
  
 Avec la topologie 2, deuxième scénarios de serveur ne sont pas pris en charge.  
  
## <a name="how-do-i-perform-common-tasks-via-windows-powershell"></a>Comment effectuer les tâches communes via Windows PowerShell?  
 **Activer l’accès Web à distance**  
  
```  
Enable-WssRemoteWebAccess [-SkipRouter] [-DenyAccessByDefault] [-ApplyToExistingUsers]  
```  
  
 Exemple:  
  
```  
$Enable-WssRemoteWebAccess  œDenyAccessByDefault  œApplyToExistingUsers  
```  
  
 Cette commande Activer l’accès Web à distance avec le routeur configuré automatiquement et modifier les autorisations d’accès par défaut pour tous les utilisateurs existants.  
  
 **Ajouter un utilisateur**  
  
```  
Add-WssUser [-Name] <string> [-Password] <securestring> [-AccessLevel <string> {User | Administrator}] [-FirstName <string>] [-LastName <string>] [-AllowRemoteAccess] [-AllowVpnAccess]   [<CommonParameters>]  
```  
  
 Exemple:  
  
```  
$password = ConvertTo-SecureString "Passw0rd!" -asplaintext  œforce  
$Add-WssUser -Name User2Test -Password $password -Accesslevel Administrator -FirstName User2 -LastName Test  
```  
  
 Cette commande ajoutera un administrateur nommé User2Test avec mot de passe Passw0rd!.  
  
 **Activer/désactiver un utilisateur**  
  
 Exemple:  
  
```  
$CurrentUser = get-wssuser  œname user2test  
$CurrentUser.UserStatus = 0  
$CurrentUser.Commit()  
```  
  
 Cette commande désactivera l’utilisateur nommé user2test. Définir UserStatus sur 1 activera l’utilisateur.  
  
 **Ajouter un dossier serveur**  
  
```  
Add-WssFolder [-Name] <string> [-Path] <string> [[-Description] <string>] [-KeepPermissions] [<CommonParameters>]  
```  
  
 Exemple:  
  
```  
$Add-WssFolder -Name "MyTestFolder" -Path "C:\ServerFolders\MyTestFolder"  
```  
  
 Cette commande ajoutera un dossier du serveur nommé MyTestFolder à l’emplacement spécifié.  
  
## <a name="how-do-i-add-a-second-server-to-the-windows-server-essentials-domain"></a>Comment ajouter un second serveur au domaine Windows Server Essentials?  
 Étant donné que Windows Server Essentials est un contrôleur de domaine, vous pouvez joindre un second serveur au domaine de la manière standard.  
  
## <a name="which-email-solutions-can-be-integrated"></a>Quelles solutions de messageries peuvent être intégrées?  
 Windows Server Essentials prend en charge l’intégration avec les deux solutions de messagerie prêtes à l’emploi: Office 365 et Exchange sur site. Si vous utilisez votre propre solution de messagerie hébergé, vous devrez développer un complément pour intégrer Windows Server Essentials à votre solution de messagerie hébergé.  
  
## <a name="how-do-i-migrate-on-premises-windows-sbs-201120082003-to-the-hosted-windows-server-essentials"></a>Comment migrer local Windows SBS (2011/2008/2003) vers Windows Server Essentials hébergé?  
 Guides de migration sont disponibles pour locaux Windows Small Business Server (Windows SBS) pour les migrations de Windows Server Essentials. Certaines étapes peuvent s’applique pas exactement à votre environnement hébergé. Toutefois, les tâches générales et les charges de travail à migrer doivent être le même. Nous vous recommandons de consulter à le [guides de migration](https://go.microsoft.com/fwlink/p/?LinkID=254292) et personnalisations nécessaires selon votre environnement d’hébergement.  
  
 Il est recommandé de placer le serveur source et le serveur de destination dans le même sous-réseau. Si ce n’est pas possible, vous devez vous assurer que:  
  
-   Le nom DNS interne du serveur source et du serveur de destination sont accessibles mutuellement.  
  
-   Tous les ports nécessaires sont ouverts.  
  
## <a name="how-can-i-upgrade-windows-server-essentials-to-windows-server-standard"></a>Comment puis-je mettre à niveau Windows Server Essentials vers Windows Server Standard?  
 Vous pouvez mettre à niveau Windows Server Essentials vers Windows Server Standard. Supprimer les verrous et les limites et ajouter les packages qui manquent depuis Windows Server Standard. Pour plus d’informations, [Téléchargez le document](https://go.microsoft.com/fwlink/p/?LinkID=253181).  
  
## <a name="what-are-the-native-tools-for-monitoring-and-management"></a>Quelles sont les outils natifs de surveillance et de gestion?  
  
### <a name="group-policy-management"></a>Gestion des stratégies de groupe  
 Windows Server Essentials exploite la prise en charge native de stratégie de groupe dans Windows Server 2012 et fournit l’interface utilisateur pour configurer les paramètres de sécurité et de redirection de dossiers.  
  
> [!NOTE]
>  Dans un environnement hébergé, si la redirection de dossiers pour un profil utilisateur est activée, il peut augmenter le temps pour les utilisateurs finaux pour se connecter si la taille des données est importante.  
  
### <a name="management-pack"></a>Pack d’administration  
 Pack d’administration Windows Server Essentials fournit une fonction de surveillance sur le système d’alerte d’intégrité dans Windows Server Essentials pour aider les hébergeurs à gérer un grand nombre de serveurs Windows Server Essentials dédiés à différentes petites et moyennes entreprises. La surveillance dans cette version inclut uniquement les alertes critiques dans le système.  
  
#### <a name="management-pack-scope"></a>Étendue du pack d’administration  
 Ce pack d’administration vous aide à surveiller des fonctionnalités spécifiques à Windows Server Essentials. Il ne surveille pas de fonctionnalités génériques dans le système d’exploitation Windows Server 2012 Standard. Pour surveiller Windows Server Essentials, vous devez utiliser le Pack d’administration Windows Server Essentials et le Management Pack pour Windows Server 2012 Standard.  
  
#### <a name="mandatory-configuration"></a>Configuration obligatoire  
 Les étapes suivantes doivent être effectuées avant de pouvoir utiliser le pack d’administration:  
  
1.  Installer l’Agent et configurer l’approbation à l’aide de certificats de confiance. Étant donné que Windows Server Essentials est préconfiguré comme un contrôleur de domaine et ne peut pas avoir d’approbation avec d’autres domaines ou forêts, l’Agent System Center Operation Manager doit être installé sur Windows Server Essentials et configuré d’approbation avec le serveur d’administration à l’aide de certificats.  
  
2.  Téléchargez le pack d’administration. Pour surveiller Windows Server Essentials à l’aide d’Operations Manager 2007, vous devez d’abord télécharger le [Pack d’administration système d’exploitation Windows Server](https://connect.microsoft.com/WindowsServer/Downloads/DownloadDetails.aspx?DownloadID=45010) à partir du catalogue des packs d’administration.  
  
3.  Importez le fichier de pack d’administration. Si vous utilisez une version localisée du pack d’administration, vous devez importer le fichier de pack d’administration principal et le pack de langue.  
  
#### <a name="files-in-this-monitoring-pack"></a>Fichiers dans ce Pack d’analyse  
 L’analyse Pack pour Windows Server Essentials inclut les fichiers suivants:  
  
-   Microsoft.Windows.Server.2012.Essentials.mp  
  
-   Microsoft.Windows.Server.2012.Essentials. < locale\ > .mp  
  
### <a name="back-up-and-restore"></a>Sauvegarder et restaurer  
 Windows Server Essentials vous permet de sauvegarder le serveur et le client.  
  
#### <a name="back-up-the-server"></a>Sauvegarder le serveur  
 Windows Server Essentials prend en charge deux manières de sauvegarder le serveur: sauvegarde locale et sauvegarde externe.  
  
 **Sauvegarde locale** vous permet d’effectuer la sauvegarde incrémentielle au niveau du bloc de façon régulière sur un disque distinct. En tant qu’hébergeur, vous pouvez attacher un disque virtuel à l’ordinateur virtuel Windows Server Essentials et configurer la sauvegarde du serveur sur ce disque virtuel. Le disque virtuel doit se trouver sur un disque physique différent de l’ordinateur virtuel Windows Server Essentials.  
  
-   Si vous disposez d’un autre mécanisme de sauvegarde de l’ordinateur virtuel Windows Server Essentials, et vous ne souhaitez pas que votre utilisateur puisse voir la fonctionnalité de sauvegarde du serveur Windows Server Essentials native, vous pouvez désactiver et supprimer tous les interface utilisateur correspondante du tableau de bord Windows Server Essentials. Pour plus d’informations, reportez-vous à la section Personnaliser la sauvegarde du serveur de la [document ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124).  
  
 **Sauvegarde externe** vous permet de sauvegarder périodiquement les données du serveur à un service cloud. Vous pouvez télécharger et installer le Microsoft Azure Backup intégration Module pour Windows Server Essentials pour tirer parti de la sauvegarde Azure fourni par Microsoft.  
  
 Si vous ou vos utilisateurs préfèrent un autre service cloud, vous devez:  
  
1.  Mettre à jour l’interface utilisateur du tableau de bord Windows Server Essentials pour qu’elle fournisse un lien vers votre service cloud préféré, au lieu de la valeur par défaut Azure Backup. Pour plus d’informations, reportez-vous à la personnaliser la section de l’Image de le [document ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124).  
  
2.  (Facultatif) Développer un complément pour le tableau de bord de Windows Server Essentials configurer et gérer le service de sauvegarde cloud.  
  
#### <a name="back-up-the-client"></a>Sauvegarder le client  
 Windows Server Essentials prend en charge deux types de sauvegarde des données client: sauvegarde complète du client et l’historique des fichiers.  
  
> [!NOTE]
>  Sauvegarde du client peut nuire aux performances, car les données doivent être transférés à partir du client vers le serveur VPN.  
  
 **Sauvegarde complète du client** concerne par défaut sur tous les périphériques clients connectés au réseau Windows Server Essentials. Il sauvegarde incrémentielle le client complet (système et données) et prend en charge la déduplication des données. Les données de sauvegarde sera sur le serveur exécutant Windows Server Essentials. Un client défaillant peut récupérer ses données à un point de sauvegarde précédente. Vous pouvez désactiver cette fonctionnalité en suivant les étapes décrites dans la créer la section du fichier Cfg.ini de la [document ADK](https://technet.microsoft.com/en-us/library/jj200150).  
  
 Certaines considérations pour la sauvegarde complète du client sont:  
  
-   Performances: sauvegarde initiale du client peut prendre beaucoup de temps en raison de la quantité de données à charger.  
  
-   Stabilité: la connexion Internet est parfois pas stable côté client. Sauvegarde du client est conçue pour être reprise, et le point de contrôle par défaut est 40 Go (la base de données de sauvegarde de client créera un point de contrôle chaque fois que 40 Go de données a été sauvegardé). Vous pouvez modifier cette valeur et la réduire si vous pensez que la connexion Internet n’est pas fiable.  
  
    -   Pour activer une tâche de point de contrôle sur le serveur, définissez la clé de Registre HKLM\Software\Microsoft\Windows Server\Backup\GetCheckPointJobs sur 1.  
  
    -   Pour modifier le seuil de point de contrôle, sur le client, remplacez HKLM\Software\Microsoft\Windows Server\Backup\CheckPointThreshold de sa valeur par défaut (40 Go).  
  
-   Client restauration à froid: étant donné que l’environnement de préinstallation de Windows ne prend pas en charge la connexion VPN, Client Bare Metal Restore n'est pas pris en charge.  
  
 **Historique des fichiers** est une fonctionnalité de Windows 8.1 pour sauvegarder les données de profil (bibliothèques, bureau, Contacts, Favoris) dans un partage réseau. Dans Windows Server Essentials, nous permettre une gestion centralisée du paramètre historique des fichiers de tous les clients Windows 8.1 joint à Windows Server Essentials. Les données de sauvegarde sont stockées sur le serveur exécutant Windows Server Essentials. Vous pouvez désactiver cette fonctionnalité en suivant les étapes décrites dans la créer la section du fichier Cfg.ini de la [document ADK](https://technet.microsoft.com/en-us/library/jj200150).  
  
### <a name="storage-management"></a>Gestion du stockage  
 Le [nouvelle fonctionnalité espaces de stockage](https://technet.microsoft.com/library/hh831739.aspx) vous permet d’agréger la capacité de stockage physique de différents disques durs, ajouter des disques durs dynamiquement et de créer des volumes de données avec des niveaux de résilience. Vous pouvez également attacher un disque iSCSI à Windows Server Essentials pour étendre son stockage.  
  
## <a name="what-are-the-main-scenarios-i-should-test"></a>Quelles sont les principaux scénarios à tester?  
 À partir de la perspective d’hébergement, il est recommandé de tester les scénarios suivants:  
  
 **Déploiement de serveur**  
  
-   Déployer un serveur Windows Server Essentials dans votre environnement de laboratoire.  
  
-   Personnaliser l’image de Windows Server Essentials en fonction des besoins.  
  
-   Automatiser le déploiement de Windows Server Essentials avec le fichier sans assistance et cfg.ini.  
  
-   Migrer local Windows SBS vers Windows Server Essentials hébergé.  
  
-   Mise à niveau à partir de Windows Server Essentials vers Windows Server 2012.  
  
 **Configuration du serveur**  
  
-   Configuration de n’importe où l’accès (VPN, accès Web à distance, DirectAccess).  
  
-   Configurer le stockage et le dossier du serveur.  
  
-   (Le cas échéant) Configurer la sauvegarde du serveur, la sauvegarde en ligne, la sauvegarde Client, l’historique des fichiers.  
  
-   (Le cas échéant) Configurer et gérer les espaces de stockage.  
  
-   (Le cas échéant) Configurer l’intégration de solution de messagerie (Office 365, hébergé Exchange et ainsi de suite).  
  
-   (Le cas échéant) Configurer le serveur multimédia.  
  
 **Gestion de serveur**  
  
-   Gérer les utilisateurs.  
  
-   Configurer et recevoir des notifications par courrier électronique des alertes.  
  
-   Exécuter l’outil BPA en cas d’erreur ou d’avertissement.  
  
-   Configurer le Pack d’analyse de System Center.  
  
-   Configurer la récupération de serveur, en cas de corruption.  
  
 **Expérience du client**  
  
-   Déploiement du client via internet (ordinateur personnel ou Mac OS).  
  
-   Utilisation de Launchpad pour accéder au dossier partagé sur le client.  
  
-   Accès des ressources du serveur via l’accès Web à distance, à partir de différents périphériques (PC, téléphone, tablette).  
  
-   Application de mon serveur pour Windows Phone.  
  
-   (Le cas échéant) L’historique des fichiers, de sauvegarde du Client et de restauration (aucune récupération complète du système), la Redirection de dossiers.  
  
-   (Le cas échéant) Expérience d’intégration de courrier électronique.  
  
## <a name="where-can-i-get-more-support"></a>Où puis-je obtenir plus de prise en charge?  
 Vous pouvez obtenir des documents SDK et ADK parmi les liens ci-dessous:  
  
-   [KIT DE DÉVELOPPEMENT LOGICIEL](https://go.microsoft.com/fwlink/p/?LinkID=248648)  
  
-   [ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124)  
  
 Vous pouvez signaler un bogue à l’équipe en charge par le biais de se connecter. Pour générer des journaux, compressez le dossier sur le serveur et les clients de joindre le serveur: C:\ProgramData\Microsoft\Windows Server\Logs.
