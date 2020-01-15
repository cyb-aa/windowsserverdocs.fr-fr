---
title: Windows Server Essentials hébergé
description: Décrit comment utiliser Windows Server Essentials
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
ms.openlocfilehash: 84464c69d4b8576906e5fb0d0a7de7e382a59537
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75947506"
---
# <a name="hosted-windows-server-essentials"></a>Windows Server Essentials hébergé

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Ce document contient des informations spécifiques aux hébergeurs qui envisagent de déployer Windows Server Essentials dans leur laboratoire et offrent Windows Server Essentials en tant que service à leurs clients.  
  
## <a name="what-is-windows-server-essentials"></a>Qu’est-ce que Windows Server Essentials ?  
 Windows Server Essentials est une solution d’entreprise de petite taille, qui intègre des technologies de produit 64 bits optimales pour fournir un environnement de serveur adapté à la grande majorité des petites entreprises. Les technologies suivantes sont incluses dans Windows Server Essentials.  
  
 **Système d’exploitation serveur :** Les technologies de produit Windows Server 2012 fournissent le cœur de Windows Server Essentials. Pour plus d’informations, consultez le [site web Windows Server 2012](https://www.microsoft.com/server-cloud/products/windows-server-2012-r2/default.aspx#fbid=ZH0GD_CRAWh).  
  
 **Protection des données :** Windows Server Essentials exploite plusieurs nouvelles fonctionnalités disponibles dans Windows Server 2012 pour fournir des fonctionnalités de protection des données très améliorées. La [nouvelle fonctionnalité Espaces de stockage](https://technet.microsoft.com/library/hh831739.aspx) vous permet d’agréger la capacité de stockage physique de différents disques durs, d’ajouter dynamiquement des disques durs et de créer des volumes de données avec des niveaux de résilience spécifiques. Windows Server Essentials peut effectuer des sauvegardes complètes du système et des restaurations complètes du serveur lui-même, ainsi que des ordinateurs clients connectés au réseau. désormais, avec la prise en charge des volumes supérieurs à 2 to. [Windows Azure Online Backup](https://technet.microsoft.com/library/hh831419.aspx) , une nouveauté de Windows Server 2012, peut être utilisé pour protéger les fichiers et les dossiers au sein d’un service de stockage cloud géré par Microsoft. Windows Server Essentials gère et configure également de manière centralisée la fonctionnalité historique des fichiers de Windows 8.1 clients, ce qui permet aux utilisateurs de récupérer des fichiers accidentellement supprimés ou remplacés sans nécessiter d’assistance de la part de l’administrateur.  
  
 **Accès en tout lieu :** l’accès web à distance permet d’accéder à des applications et des données, via un navigateur simple d’emploi et tactile, et ce depuis pratiquement n’importe quel endroit et à l’aide de pratiquement tout appareil doté d’une connexion Internet. Windows Server Essentials fournit également une application de Windows Phone mise à jour et une nouvelle application pour les ordinateurs clients Windows 8.1, ce qui permet aux utilisateurs de se connecter intuitivement à, de rechercher des fichiers et des dossiers sur le serveur et d’y accéder. Les fichiers sont également automatiquement mis en cache pour un accès hors ligne et synchronisés si une connexion au serveur est disponible. Windows Server Essentials active la configuration du réseau privé virtuel (VPN) dans un processus simple, piloté par un Assistant, en quelques clics et simplifie la gestion de l’accès VPN pour les utilisateurs. Les ordinateurs clients peuvent exploiter une connexion VPN pour se connecter à distance à un environnement Windows SBS sans avoir besoin de se rendre au bureau.  
  
 **Flexibilité de la charge de travail :** Windows Server Essentials a été conçu pour offrir aux clients la possibilité de choisir les applications et services qui s’exécutent localement et qui s’exécutent dans le Cloud. Dans les anciennes versions, le serveur Exchange était un composant de Windows Small Business Server Standard, ce qui entraînait des dépenses et une complexité supplémentaires pour les clients souhaitant exploiter une messagerie et des services de collaboration basés dans le nuage. Avec Windows Server Essentials, les clients peuvent tirer parti du même type d’expérience de gestion intégrée, qu’ils choisissent d’exécuter une copie locale d’Exchange Server, s’abonnent à un service Exchange hébergé ou s’abonnent à Microsoft Office 365.  
  
 **Surveillance de l’intégrité :** Windows Server Essentials surveille son propre état d’intégrité et l’état des ordinateurs clients qui exécutent Windows 8.1, Windows 7 et Mac OS X version 10,5 et versions ultérieures. L'état d'intégrité vous signale des problèmes liés aux sauvegardes d'ordinateurs, au stockage de serveur, à un espace disque faible, etc.  
  
 **Extensibilité :** Windows Server Essentials repose sur le modèle d’extensibilité de Windows SBS 2011 Essentials, qui permet à d’autres fournisseurs de logiciels d’ajouter des fonctionnalités au produit de base et ajoute un nouvel ensemble d’API de services Web. Il permet également de conserver la compatibilité avec le [Kit de développement logiciel](https://msdn.microsoft.com/library/gg513958.aspx) (SDK) et les [compléments](https://pinpoint.microsoft.com/applications/search?fpt=300105&q=small+business+server+essentials) créés pour Windows SBS 2011 Essentials.  
  
## <a name="how-can-i-customize-an-image"></a>Comment personnaliser une image ?  
 Reportez-vous à [Windows Server Essentials](https://go.microsoft.com/fwlink/p/?LinkID=249124), qui est un processus Sysprep standard de Windows Server avec des étapes de personnalisation supplémentaires de Windows Server Essentials. Pour terminer la personnalisation, suivez les instructions dans [Création d’une image personnalisée simple](https://technet.microsoft.com/library/jj200117) et [Personnalisation de l’image](https://technet.microsoft.com/library/jj200161), puis suivez les instructions figurant dans [Préparation de l’image en vue du déploiement](https://technet.microsoft.com/library/jj200142) afin de capturer votre image finale.  
  
 Tenez compte des points suivants :  
  
1. Vous devez ignorer la configuration initiale (IC) en ajoutant un fichier SkipIC.txt à la racine de chaque lecteur. Après avoir installé le serveur, et avant la configuration initiale, appuyez sur Maj+F10 pour lancer la fenêtre cmd et créer un fichier SkipIC.txt sous le lecteur C:/. Une fois la personnalisation terminée, vous devez vous rappeler de supprimer le fichier SkipIC.txt.  
  
2. Si vous devez déployer Windows Server Essentials sur un disque d’une taille inférieure à 90 Go, vous devez ajouter une clé de Registre avant Sysprep :  
  
   ```  
   %systemroot%\system32\reg.exe add "HKLM\Software\microsoft\windows server\setup" /v HWRequirementChecks /t REG_DWORD /d 0 /f  
   ```  
  
   Après sysprep, vous pouvez utiliser l'image de disque préparée avec sysprep, ou la retransférer dans le fichier Install.wim pour une nouvelle mise en œuvre ultérieure.  
  
   Si vous utilisez le Virtual Machine Manager, vous pouvez créer un modèle en utilisant l'instance en cours d'exécution. La création d'un modèle prépare l'instance avec sysprep et éteint le serveur. Une fois qu'elle est stockée dans votre bibliothèque, vous pouvez utiliser l'instance au cas par cas.  
  
##  <a name="BKMK_automatedeployment"></a>Comment faire automatiser le déploiement ?  
 Une fois que vous avez obtenu l'image personnalisée, vous pouvez effectuer le déploiement avec votre propre image. Pour effectuer une installation en partie sans assistance, vous devez fournir/déployer le fichier unattend.xml pour la configuration de WinPE. Pour effectuer une installation entièrement sans assistance, vous devez également fournir le fichier cfg. ini pour la configuration initiale de Windows Server Essentials.  
  
1. Effectuer uniquement une configuration sans assistance de WinPE. Cela automatisera uniquement la configuration de WinPE et arrêtera la configuration avant la configuration initiale, afin de permettre aux utilisateurs de fournir eux-mêmes des informations sur la société, le domaine et l'administrateur pour la session de serveur via RDP. Pour cela, procédez comme suit :  
  
   1.  Fournissez le fichier unattend.xml Windows. Suivez le [Windows 8.1 ADK](https://go.microsoft.com/fwlink/?LinkId=248694) pour générer le fichier et fournissez toutes les informations nécessaires, y compris le nom du serveur, les clés de produit et le mot de passe administrateur. Dans la section Microsoft-Windows-Setup du fichier Unattend. xml, fournissez les informations ci-dessous.  
  
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
  
   2.  Le port RDP 3389 doit être ouvert sur une adresse IP publique afin que le client puisse utiliser le mot de passe d’administrateur et le mot de passe spécifiés dans le fichier Unattend. xml sur le serveur pour terminer la configuration initiale.  
  
   > [!NOTE]
   >  Si vous ne modifiez pas le mot de passe par défaut, l'installation du serveur s'arrêtera sur un écran vous demandant de saisir un mot de passe.**Remarque** Les utilisateurs finaux doivent utiliser le compte d'administrateur par défaut pour se connecter au serveur et exécuter la configuration initiale.  
  
   Si vous utilisez le Virtual Machine Manager, vous pouvez spécifier le mot de passe de l'administrateur dans la console lorsque vous créez une nouvelle instance à partir du modèle.  
  
2. Exécutez une configuration sans assistance complète, y compris la configuration initiale sans assistance. Pour cela, procédez comme suit :  
  
   1.  Fournissez le fichier unattend.xml comme vous l'avez-fait ci-dessus si le déploiement démarre depuis la configuration de WinPE.  
  
   2.  Pour générer le fichier cfg. ini, reportez-vous à la section Windows Server Essentials ADK intitulée [créer le fichier cfg. ini](https://technet.microsoft.com/library/jj200150).  
  
   3.  Fournissez les informations nécessaires dans [InitialConfiguration].  
  
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
  
   4.  Fournissez les informations nécessaires dans [PostOSInstall].  
  
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
  
   5.  Si vous fournissez le paramètre WebDomainName, assurez-vous que l’enregistrement DNS est également mis à jour pour pointer vers l’adresse IP publique du serveur.  
  
   6.  Si vous n'avez pas fourni les informations du WebDomainName ci-dessus, assurez-vous que le port 3389 est ouvert afin que les utilisateurs puissent utiliser le RDP pour se connecter au serveur et terminer les configurations du VPN.  
  
> [!NOTE]
>  Assurez-vous que le paramètre de fuseau horaire du serveur hôte d’ordinateur virtuel et de la machine virtuelle Windows Server Essentials est le même. Dans le cas contraire, vous pourriez rencontrer différentes erreurs (un échec de la configuration initiale du à des tâches nécessitant un certificat peut se produire, le certificat peut ne pas être valable avant quelques heures après l'installation, les informations concernant le périphérique peuvent ne pas être mises à jour correctement, etc.).  
  
 Après le déploiement, contrôlez la clé de registre suivante sous HKLM\software\microsoft\windows server\setup pour vérifier si la configuration initiale a été effectuée correctement. Si SetupStage == ICDone && ICStatus == 1, la configuration initiale a été effectuée correctement.  
  
## <a name="what-is-the-supported-network-topology"></a>Quelle est la topologie de réseau prise en charge ?  
 Pour utiliser Windows Server Essentials à partir d’un client itinérant, vous devez activer le VPN. Nous recommandons l'activation du port 443 pour les connexions VPN SSTP. Si la fonctionnalité de serveur multimédia est nécessaire pour l'Accès web à distance ou des applications de service Web, le port 80 doit être également activé.  
  
 L'activation du VPN peut être effectuée pendant le déploiement sans assistance via notre script Windows PowerShell, ou elle peut être configurée à l'aide de notre Assistant après la configuration initiale.  
  

- Pour plus d’informations sur l’activation du VPN pendant le déploiement sans assistance, consultez [How do I automate the deployment?](Hosted-Windows-Server-Essentials.md#BKMK_automatedeployment) dans le présent document.  

- Pour plus d’informations sur l’activation du VPN pendant le déploiement sans assistance, consultez [How do I automate the deployment?](../install/Hosted-Windows-Server-Essentials.md#BKMK_automatedeployment) dans le présent document.  

  
- Pour activer le VPN via Windows PowerShell, exécutez la cmdlet suivante avec une autorisation d'administrateur et fournissez toutes les informations nécessaires.  
  
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
  
  Si vous ne pouvez pas fournir une connexion VPN avant d'effectuer la livraison aux clients, le serveur aux clients, assurez-vous que le port serveur 3389 est accessible via Internet afin que les utilisateurs finaux puissent utiliser le RDP pour se connecter au serveur et effectuer eux-mêmes la configuration.  
  
  Voici les deux topologies de mise en réseau typiques côté serveur et la manière dont le VPN / l'accès web à distance peut être configuré :  
  
- Topologie 1 (préférée)  
  
  -   Un serveur dans un réseau virtuel séparé dans un périphérique NAT.  
  
  -   Le service DHCP est activé dans le réseau virtuel, ou une adresse IP statique est affectée au serveur.  
  
  -   Le port de serveur 443 est accessible depuis le port IP public 443.  
  
  -   Le relais VPN est autorisé pour le port 443.  
  
  -   Le pool d’adresses VPN IPv4 doit se trouver dans le même sous-réseau d'adresses de serveur.  
  
  -   Une adresse IP statique doit être affectée à tout second serveur dans le même sous-réseau, mais pas dans le VPN.  
  
- Topologie 2 :  
  
  -   Le serveur dispose d'une adresse IP privée.  
  
  -   Le port 443 sur le serveur est accessible à partir d’une adresse IP publique, port 443.  
  
  -   Le relais VPN est autorisé pour le port 443.  
  
  -   Le pool d’adresses VPN IPv4 se trouve dans une autre plage de l'adresse de serveur.  
  
  Les scénarios incluant un second serveur ne sont pas pris en charge avec la topologie 2.  
  
## <a name="how-do-i-perform-common-tasks-via-windows-powershell"></a>Comment exécuter des tâches communes via Windows PowerShell ?  
 **Activer les Accès web distantes**  
  
```  
Enable-WssRemoteWebAccess [-SkipRouter] [-DenyAccessByDefault] [-ApplyToExistingUsers]  
```  
  
 Exemple :  
  
```  
$Enable-WssRemoteWebAccess  œDenyAccessByDefault  œApplyToExistingUsers  
```  
  
 cette commande activera l'Accès web à distance avec le routeur configuré automatiquement et modifiera les autorisations d'accès par défaut pour tous les utilisateurs existants.  
  
 **Ajouter un utilisateur**  
  
```  
Add-WssUser [-Name] <string> [-Password] <securestring> [-AccessLevel <string> {User | Administrator}] [-FirstName <string>] [-LastName <string>] [-AllowRemoteAccess] [-AllowVpnAccess]   [<CommonParameters>]  
```  
  
 Exemple :  
  
```  
$password = ConvertTo-SecureString "Passw0rd!" -asplaintext  œforce  
$Add-WssUser -Name User2Test -Password $password -Accesslevel Administrator -FirstName User2 -LastName Test  
```  
  
 Cette commande ajoute un administrateur nommé User2Test avec le mot de passe Passw0rd !.  
  
 **Activer/désactiver un utilisateur**  
  
 Exemple :  
  
```  
$CurrentUser = get-wssuser  œname user2test  
$CurrentUser.UserStatus = 0  
$CurrentUser.Commit()  
```  
  
 Cette commande va désactiver l’utilisateur nommé user2test. Définir UserStatus sur 1 activera l'utilisateur.  
  
 **Ajouter un dossier serveur**  
  
```  
Add-WssFolder [-Name] <string> [-Path] <string> [[-Description] <string>] [-KeepPermissions] [<CommonParameters>]  
```  
  
 Exemple :  
  
```  
$Add-WssFolder -Name "MyTestFolder" -Path "C:\ServerFolders\MyTestFolder"  
```  
  
 Cette commande ajoute un dossier de serveur nommé MyTestFolder à l’emplacement spécifié.  
  
## <a name="how-do-i-add-a-second-server-to-the-windows-server-essentials-domain"></a>Comment faire ajouter un second serveur au domaine Windows Server Essentials ?  
 Étant donné que Windows Server Essentials est un contrôleur de domaine, vous pouvez joindre un second serveur au domaine de manière standard.  
  
## <a name="which-email-solutions-can-be-integrated"></a>Quelles solutions de messageries peuvent être intégrées ?  
 Windows Server Essentials prend en charge l’intégration à deux solutions de messagerie prêtes à l’emploi : Office 365 et Exchange local. Si vous exécutez votre propre solution de messagerie hébergée, vous devrez développer un complément pour intégrer Windows Server Essentials à votre solution de messagerie hébergée.  
  
## <a name="how-do-i-migrate-on-premises-windows-sbs-201120082003-to-the-hosted-windows-server-essentials"></a>Comment faire migrer localement Windows SBS (2011/2008/2003) vers le Windows Server Essentials hébergé ?  
 Des guides de migration sont disponibles pour les migrations Windows Small Business Server (Windows SBS) local vers Windows Server Essentials. Certaines étapes ne sont pas exactement identiques dans votre environnement hébergé. Cependant, les tâches générales et les charges de travail à migrer doivent être identiques. Nous vous recommandons de vous référer aux [guides de migration](https://go.microsoft.com/fwlink/p/?LinkID=254292) et de procéder aux personnalisations nécessaires selon votre environnement d’hébergement.  
  
 Il est recommandé de placer le serveur source et le serveur de destination dans le même sous-réseau. Si ce n'est pas possible, assurez-vous que :  
  
-   le nom DNS interne du serveur source et celui du serveur de destination sont accessibles mutuellement.  
  
-   tous les ports nécessaires sont ouverts.  
  
## <a name="how-can-i-upgrade-windows-server-essentials-to-windows-server-standard"></a>Comment puis-je mettre à niveau Windows Server Essentials vers Windows Server standard ?  
 Vous pouvez mettre à niveau Windows Server Essentials vers Windows Server standard. Supprimez les verrouillages et les valeurs limites, et ajoutez les packs qui manquent depuis Windows Server Standard. Pour plus d’informations, [téléchargez le document](https://go.microsoft.com/fwlink/p/?LinkID=253181).  
  
## <a name="what-are-the-native-tools-for-monitoring-and-management"></a>Quels sont les outils natifs pour la surveillance et la gestion ?  
  
### <a name="group-policy-management"></a>Gestion des stratégies de groupe  
 Windows Server Essentials exploite la prise en charge stratégie de groupe native dans Windows Server 2012 et fournit l’interface utilisateur pour configurer la redirection de dossiers et les paramètres de sécurité.  
  
> [!NOTE]
>  Si la redirection de dossiers est activée dans un environnement hébergé pour un profil utilisateur, le temps nécessaire aux utilisateurs finaux pour se connecter peut augmenter si le volume de données est important.  
  
### <a name="management-pack"></a>Pack d'administration  
 Le pack d’administration Windows Server Essentials fournit une fonction de surveillance sur le système d’alerte d’intégrité de Windows Server Essentials pour aider les hébergeurs à gérer un grand nombre de serveurs Windows Server Essentials dédiés à différentes petites entreprises. La surveillance dans cette version inclut uniquement des alertes critiques dans le système.  
  
#### <a name="management-pack-scope"></a>Étendue du pack d'administration  
 Ce pack d’administration vous aide à surveiller les fonctionnalités spécifiques à Windows Server Essentials. Il ne surveille pas de fonctionnalités génériques dans le système d'exploitation Windows Server 2012 Standard. Pour surveiller Windows Server Essentials, vous devez utiliser le pack d’administration Windows Server Essentials et le pack d’administration pour Windows Server 2012 standard.  
  
#### <a name="mandatory-configuration"></a>Configuration obligatoire  
 Les étapes suivantes doivent être exécutées avant que vous ne puissiez utiliser le pack d'administration :  
  
1.  Installer l'agent et configurez le mode de confiance à l'aide d'un certificat de confiance. Étant donné que Windows Server Essentials est préconfiguré en tant que contrôleur de domaine et ne peut pas avoir de relation de confiance avec d’autres domaines ou forêts, l’agent System Center Operation Manager doit être installé sur Windows Server Essentials et configuré en confiance avec la gestion serveur utilisant des certificats.  
  
2.  Téléchargez le pack d'administration. Pour surveiller Windows Server Essentials à l’aide de Operations Manager 2007, vous devez tout d’abord télécharger le [Pack d’administration du système d’exploitation Windows Server](https://connect.microsoft.com/WindowsServer/Downloads/DownloadDetails.aspx?DownloadID=45010) à partir du catalogue des packs d’administration.  
  
3.  Importez le fichier du pack d'administration. Si vous utilisez une version localisée du pack d'administration, vous devez importer le fichier de pack d'administration principal et le module linguistique.  
  
#### <a name="files-in-this-monitoring-pack"></a>Fichiers dans le pack d'analyse  
 Le Pack de surveillance pour Windows Server Essentials comprend les fichiers suivants :  
  
-   Microsoft.Windows.Server.2012.Essentials.mp  
  
-   Microsoft. Windows. Server. 2012. Essentials. < paramètres régionaux\>. MP  
  
### <a name="back-up-and-restore"></a>Sauvegarder et restaurer  
 Windows Server Essentials vous permet de sauvegarder le serveur et le client.  
  
#### <a name="back-up-the-server"></a>Sauvegarder le serveur  
 Windows Server Essentials prend en charge deux manières de sauvegarder le serveur : la sauvegarde locale et la sauvegarde hors site.  
  
 **La sauvegarde locale** vous permet d'exécuter régulièrement une sauvegarde incrémentielle au niveau du bloc sur un disque distinct. En tant qu’hébergeur, vous pouvez attacher un disque virtuel à la machine virtuelle Windows Server Essentials et configurer la sauvegarde du serveur sur ce disque virtuel. Le disque virtuel doit se trouver sur un disque physique différent de celui de la machine virtuelle Windows Server Essentials.  
  
- Si vous disposez d’un autre mécanisme pour sauvegarder la machine virtuelle Windows Server Essentials et que vous ne souhaitez pas que votre utilisateur puisse voir la fonctionnalité de sauvegarde de serveur native de Windows Server Essentials, vous pouvez la désactiver et supprimer toute l’interface utilisateur de Windows Server Essentials Commandes. Pour plus d’informations, reportez-vous à la section personnaliser la sauvegarde du serveur du [document ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124).  
  
  **La sauvegarde externe** vous permet de sauvegarder périodiquement les données de serveur dans un service de nuage. Vous pouvez télécharger et installer le module d’intégration Sauvegarde Microsoft Azure pour Windows Server Essentials pour tirer parti de la sauvegarde Azure fournie par Microsoft.  
  
  Si vous ou vos utilisateurs préférez un autre service de nuage, vous devez :  
  
1.  Mettez à jour l’interface utilisateur du tableau de bord Windows Server Essentials afin qu’elle fournisse un lien vers votre service Cloud préféré au lieu de la sauvegarde Azure par défaut. Pour plus d’informations, consultez la section « Personnaliser l’image » de la [documentation sur le Kit de déploiement et d’évaluation](https://go.microsoft.com/fwlink/p/?LinkID=249124).  
  
2.  Facultatif Développez un complément pour le tableau de bord Windows Server Essentials afin de configurer et de gérer le service de sauvegarde Cloud.  
  
#### <a name="back-up-the-client"></a>Sauvegarder le client  
 Windows Server Essentials prend en charge deux types de sauvegarde de données client : sauvegarde complète du client et historique des fichiers.  
  
> [!NOTE]
>  La sauvegarde du client peut avoir un impact sur les performances, car les données doivent être transférées du client au serveur via un réseau privé virtuel.  
  
 La **sauvegarde complète du client** est activée par défaut pour tous les périphériques clients connectés au réseau Windows Server Essentials. Elle sauvegarde le client complet (système et données) de manière incrémentielle et prend en charge la déduplication de données. Les données de sauvegarde seront sur le serveur exécutant Windows Server Essentials. Un client défaillant peut récupérer ses données à un point de sauvegarde antérieur. Vous pouvez désactiver cette fonctionnalité en suivant les étapes de la section créer le fichier cfg. ini du [document ADK](https://technet.microsoft.com/library/jj200150).  
  
 Voici certains points à prendre en compte lors de la sauvegarde complète du client :  
  
- Performance : il se peut que la sauvegarde initiale du serveur nécessite du temps en raison du volume de données à charger.  
  
- Stabilité : la connexion Internet n'est parfois pas stable côté client. La sauvegarde du client est conçue pour pouvoir être reprise, et le point de contrôle par défaut est 40 Go (la base de données du client créera un point de contrôle chaque fois qu'un volume de données de 40 Go sera sauvegardé). Vous pouvez modifier cette valeur et la réduire si vous pensez que la connexion Internet n'est pas fiable.  
  
  -   Pour activer une tâche de point de contrôle, définissez la clé de registre HKLM\Software\Microsoft\Windows Server\Backup\GetCheckPointJobs sur 1 sur le serveur.  
  
  -   Pour modifier le seuil de point de contrôle, modifiez la valeur par défaut (40 Go) de HKLM\Software\Microsoft\Windows Server\Backup\CheckPointThreshold sur le client.  
  
- Restauration du client à froid : l'environnement de préinstallation de Windows ne prenant pas en charge une connexion VPN, la restauration du client à froid n'est pas prise en charge.  
  
  **L’historique des fichiers** est une fonctionnalité Windows 8.1 pour sauvegarder des données de profil (bibliothèques, bureau, contacts, favoris) sur un partage réseau. Dans Windows Server Essentials, nous autorisons la gestion centralisée du paramètre historique des fichiers de tous les clients Windows 8.1 joints à Windows Server Essentials. Les données de sauvegarde sont stockées sur le serveur Windows Server Essentials. Vous pouvez désactiver cette fonctionnalité en suivant les étapes de la section créer le fichier cfg. ini du [document ADK](https://technet.microsoft.com/library/jj200150).  
  
### <a name="storage-management"></a>Gestion du stockage  
 La [nouvelle fonctionnalité Espaces de stockage](https://technet.microsoft.com/library/hh831739.aspx) vous permet d’agréger la capacité de stockage physique de différents disques durs, d’ajouter dynamiquement des disques durs et de créer des volumes de données avec des niveaux de résilience spécifiques. Vous pouvez également attacher un disque iSCSI à Windows Server Essentials pour étendre son stockage.  
  
## <a name="what-are-the-main-scenarios-i-should-test"></a>Quels sont les principaux scénarios à tester ?  
 Du point de vue de l'hébergement, il est recommandé de tester les scénarios suivants :  
  
 **Déploiement du serveur**  
  
- Déployez le serveur Windows Server Essentials dans votre environnement Lab.  
  
- Personnaliser l'image Windows Server 2012 Essentials en fonction des besoins.  
  
- Automatisez le déploiement de Windows Server Essentials avec un fichier sans assistance et cfg. ini.  
  
- Migration de Windows SBS local vers Windows Server Essentials hébergé.  
  
- Effectuez une mise à niveau de Windows Server Essentials vers Windows Server 2012.  
  
  **Configuration du serveur**  
  
- Configuration de l'Accès en tout lieu (VPN, accès web à distance, DirectAccess).  
  
- Configuration du stockage et du dossier de serveur.  
  
- Configuration de la sauvegarde du serveur, de la sauvegarde en ligne, de la sauvegarde du client et de l'historique des fichiers (le cas échéant).  
  
- Configurer gérer les espaces de stockage (le cas échéant).  
  
- Configuration de l'intégration d'une solution de messagerie (Office 365, Exchange hébergé, etc.) (le cas échéant).  
  
- Configuration du serveur multimédia (le cas échéant).  
  
  **Gestion de serveur**  
  
- Gestion des utilisateurs.  
  
- Configuration et réception d'une notification des alertes par courrier électronique.  
  
- Exécution de BPA en cas d'erreur/d'avertissement.  
  
- Configuration du pack d'analyse System Center.  
  
- Configuration de la récupération du serveur en cas de corruption.  
  
  **Expérience client**  
  
- Déploiement du client via Internet (ordinateur personnel ou Mac OS).  
  
- Utilisation du Launchpad sur le client pour accéder au dossier partagé.  
  
- Accès à des ressources du serveur via l'Accès à distance depuis différents périphériques (ordinateur personnel, téléphone, tablette).  
  
- Application Mon Serveur pour Windows Phone.  
  
- Historique des fichiers, sauvegarde et restauration du client (pas de récupération complète), redirection de dossiers (le cas échéant).  
  
- Utilisation de l'intégration d'une messagerie (le cas échéant).  
  
## <a name="where-can-i-get-more-support"></a>Où obtenir un support supplémentaire ?  
 Vous pouvez obtenir une documentation sur le Kit de développement logiciel et le Kit de déploiement et d’évaluation depuis le lien ci-dessous :  
  
- [Kit SDK](https://go.microsoft.com/fwlink/p/?LinkID=248648)  
  
- [ADK](https://go.microsoft.com/fwlink/p/?LinkID=249124)  
  
  Vous pouvez signaler un problème à l'équipe en charge via Connect. Pour générer des journaux, compressez le dossier sur le serveur et les clients associés au serveur : C:\ProgramData\Microsoft\Windows Server\Logs.
