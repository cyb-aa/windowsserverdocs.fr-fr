---
title: Création du fichier Cfg.ini
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 93a73556-22ef-402d-b8d4-582b74c22bcf
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 0702fb8616ced4e7e00de344da47995d540f074d
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312070"
---
# <a name="create-the-cfgini-file"></a>Création du fichier Cfg.ini

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Le fichier cfg.ini est utilisé pour automatiser une installation du système d'exploitation dans le scénario suivant :  
  
-   En cas de test de l'expérience de l'utilisateur final au moyen d'une image pré-installée sur l'ordinateur cible, la section Configuration initiale permet de procéder à l'installation, étape par étape, en mode manuel ou sans assistance. Pour ce faire, voir [Création de la section Configuration initiale](Create-the-Cfg.ini-File.md#BKMK_CreateInit2).  
  
##  <a name="create-the-initial-configuration-section"></a><a name="BKMK_CreateInit2"></a>Créer la section de configuration initiale  
 Utilisez la section Configuration initiale du fichier cfg.ini pour procéder à l'installation, étape par étape, en mode manuel ou sans assistance.  
  
#### <a name="to-define-the-initial-configuration-section"></a>Pour définir la section Configuration initiale  
  
1.  Créez un fichier cfg.ini ou ouvrez-le (s'il existe déjà) dans le Bloc-notes.  
  
2.  Ajoutez le texte suivant pour créer une section InitialConfiguration.  
  
    ```  
  
    [InitialConfiguration]  
    ;Optional, display language can only be one of the installed language  
    Language=en-us  
    ;Optional, The name of a script that runs after setupComplete.cmd but before the initial configuration begins.  
    ;Optional  
    Locale=en-us  
    ;Optional  
    Country=US  
    ;Optional  
    Keyboard=0409:00000409  
    AcceptEula=true  
    ;This is only required on a server where an OEM EULA has been specified   
    ;by using the OOBE.xml file  
    AcceptOEMEula=true  
    ;Optional. Example: My Company Name  
    CompanyName=EnterCompanyName  
    ServerName=EnterServerName  
    ; Example: CONTOSO  
    NetbiosName=EnterNetbiosDomainName  
    ; Example: contoso.local  
    DNSName=EnterDNSDomain   
    ; Used to set the user name for the domain admin  
    UserName=EnterDomainAdminUserName  
    ;The password has to be strong and at least 8 characters  
    PlainTextPassword=EnterAdminPassword  
    ;. Used to set the user name for the domain standard user account. Ignored in migration mode.  
    StdUserName=EnterDomainStandardUserName  
    ;. The password for the domain standard user account has to be strong and at least 8 characters  
    StdUserPlainTextPassword=EnterStandardUserPassword  
    ;Controls the Watson and automatic update settings  
    Settings=All or Updates or None  
    WebDomainName=www.abc.com  
    TrustedCertFileName=c:\cert\a.pfx  
    TrustedCertPassword=Enteryourpassword  
    EnableVPN=true  
    EnableRWA=true  
    IPv4DNSForwarder=<IPV4Address,IPV4Address,¦>  
    IPv6DNSForwarder=<IPV6Address,IPV6Address,¦>  
    VpnIPv4StartAddress=<IPV4Address>  
    VpnIPv4EndAddress=<IPV4Address>  
    VpnBaseIPv6Address=<IPV6Address>  
    VpnIPv6PrefixLength=<number>  
    ;All these section are optional.  
     [PostOSInstall]  
    ;Optional, The name of a script that runs after setupComplete.cmd but before the initial configuration begins.  
  
    IsHosted=true  
    StaticIPv4Address=<IPV4Address>  
    StaticIPv4Gateway=<IPV4Address>  
    StaticIPv4SubnetMask=<IPV4SubnetMask>   
    StaticIPv6Address=<IPV6Address>  
    StaticIPv6SubnetPrefixLength=<number>  
    StaticIPv6Gateway=<IPV6Address>  
    ClientBackupOn=true  
    FileHistoryOn=true  
    LaunchPadHiddenTasks=<Microsoft.LaunchPad.AdminDashboard,Microsoft.LaunchPad.Backup>  
  
    ```  
  
    > [!NOTE]
    >  Aucune option de sélection d'une autre langue n'a été fournie au cours de la configuration initiale. En cas de réinitialisation du système, la langue du système d'exploitation sera celle d'origine.  
  
    |Nom du paramètre|Description du paramètre|  
    |--------------------|---------------------------|  
    |*AcceptEula*|Indique que l'utilisateur accepte les termes du contrat de licence logiciel Microsoft La valeur peut être Vrai ou fausse, mais l'installation se déroule uniquement si ce paramètre est réglé à la valeur Vrai.|  
    |*AcceptOEMEula*|(Facultatif) Indique que l'utilisateur accepte le contrat de licence pour partenaire. La valeur peut être vraie ou fausse. Ce champ est obligatoire uniquement si le serveur a été acheté chez un partenaire ayant fourni un contrat de licence distinct.|  
    |*Prennent*|(Facultatif) Nom de la société. Le nom de la société sert à associer votre serveur à votre société et à personnaliser les rapports. Peut comporter jusqu’à 254 caractères|  
    |*Nationale*|(Facultatif) Une chaîne de caractères représentant le pays/la région souhaités. Exemple : US pour les États-Unis.|  
    |*Nom du serveur*|Le nom du serveur identifie de manière unique le serveur sur le réseau. Le nom de votre serveur doit respecter les critères suivants :<br /><br /> -Peut comporter jusqu’à 15 caractères.<br /><br /> -Peut contenir des lettres, des chiffres et des traits d’Union (-).<br /><br /> -Ne doit pas commencer par un trait d’Union.<br /><br /> -Ne doit pas contenir d’espaces.<br /><br /> -Ne doit pas contenir que des nombres.<br /><br /> Exemple : ServeurContoso.|  
    |*DNSName*|Un domaine interne regroupe les ordinateurs serveur et client afin de partager une base de données commune de noms d’utilisateur, de mots de passe et d’autres informations communes. Les utilisateurs voient ce nom lorsqu’ils se connectent à leur ordinateur, mais son usage est purement interne et n’équivaut pas à un nom de domaine Internet. Le nom de domaine interne doit répondre aux mêmes critères que ceux spécifiés pour le *NomServeur*.<br /><br /> Exemple : contoso.local.|  
    |*NetbiosName*|Un nom NetBIOS est utilisé pour identifier les ressources en cours d'exécution sur le serveur. Il peut comporter jusqu’à 15 caractères. Exemple : Contoso.|  
    |*Langue*|(Facultatif) Spécification de la langue d'affichage. Ce ne peut être que l'une des langues installées. Exemple: en-us pour l'anglais tel qu'il est utilisé aux États-Unis.|  
    |*Paramètres régionaux*|(Facultatif) Spécification du format de l'heure et de la monnaie en utilisant un format *LocaleID* . Exemple: en-us pour la monnaie et l'heure affichées en anglais et formatées selon les normes utilisées aux États-Unis|  
    |*Clavier*|Le clavier peut avoir le format suivant :<br /><br /> - **langue d’entrée : disposition du clavier.** Par exemple 0409:00000409 avec 0409 avant **:** est la langue d’entrée, et **00000409** est la disposition du clavier. Vous pouvez trouver une liste de dispositions de clavier sous la clé de registre **HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Keyboard Layouts**.<br /><br /> **langue d’entrée - : identificateur IME.** Vous trouverez ci-dessous une liste d’identificateurs IME.<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1} {8F96574E-C86C-4bd6-9666-3F7327D4CBE8} méthode d’entrée amharique<br /><br /> -{81d4e9c9-1d3b-41bc-9e6c-4b40bf79e35e} {FA550B04-5AD7-411F-A5AC-CA038EC515D7} Microsoft Pinyin-simple Fast (chinois simplifié)<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732} {B2F9C502-1742-11D4-9790-0080C882687E} chinois (traditionnel)-nouvelle phonétique<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732} {4BDF9F03-C7D3-11D4-B2AB-0080C882687E} chinois (traditionnel)-ChangJie<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732} {6024B45F-5C54-11D4-B921-0080C882687E} chinois (traditionnel)-rapide<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1} {D38EFF65-AA46-4FD5-91A7-67845FB02F5B} tableau traditionnel chinois<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1} {037B2C25-480C-4D7F-B027-D6CA6B69788A} chinois traditionnel-dayi<br /><br /> -{03B5835F-F03C-411B-9CE2-AA23E1171E36} {A76C93D9-5523-4E90-AAFA-4DB112F9AC76} Microsoft IME (japonais)<br /><br /> -{A028AE76-01B1-46C2-99C4-ACD9858AE02F} {B5FE1F02-D5F2-4445-9C03-C568F23C99A1} Microsoft IME (coréen)<br /><br /> -{A1E2B86B-924A-4D43-80F6-8A820DF7190F} {B60AF051-257A-46BC-B9D3-84DAD819BAFB} ancien IME Hangul (coréen)<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1} {409C8376-007B-4357-AE8E-26316EE3FB0D} méthode d’entrée Yi<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1} {3CAB88B7-CC3E-46A6-9765-B772AD7761FF} tigrinya méthode d’entrée|  
    |*Paramètres*|Définit le choix de l'utilisateur pour les mises à jour. Utilisez l’une des valeurs suivantes :<br /><br /> **-All équivaut à utiliser les** paramètres recommandés.<br /><br /> **-Les mises à jour** égales installent les mises à jour importantes. uniquement<br /><br /> **-Aucun** est égal à ne pas vérifier les mises à jour.|  
    |*Nom d’utilisateur*|-Le nom du nouveau compte administrateur créé pendant l’installation. Le nom des comptes d'utilisateur standard et d'administrateur doit respecter les critères suivants :<br /><br /> -Peut comporter jusqu’à 19 caractères.<br /><br /> -Ne peut pas contenir/\ &#124; [] < > + =; , ? *<br /><br /> -Ne doit pas commencer ou se terminer par un point.<br /><br /> -Ne doit pas contenir deux points consécutifs.<br /><br /> -Ne doit pas être le même que le nom du serveur ou le nom de domaine interne.<br /><br /> -Ne doit pas être identique à un nom d’utilisateur prédéfini comme administrateur ou invité.|  
    |*PlainTextPassword*|Il s’agit du mot de passe pour le nouveau compte administrateur créé durant l’installation.<br /><br /> -Doit comporter au moins huit caractères.<br /><br /> -Doit contenir au moins trois des quatre catégories suivantes :<br /><br /> -Caractères majuscules.<br /><br /> Caractères en minuscules.<br /><br /> Nombreux.<br /><br /> Sigle.|  
    |*StdUserName*|Le nom du nouveau compte utilisateur standard créé durant l'installation. Voir le paramètre *NomUtilisateur* pour les besoins.|  
    |*StdUserPlainTextPassword*|Le mot de passe du compte utilisateur standard créé durant l'installation.|  
    |WebDomainName|(Facultatif) Configuration du nom de domaine Internet du serveur. Ce fichier vous permet de configurer le nom de domaine avec une méthode similaire à celle utilisée pour la configuration manuelle dans l’Assistant Configuration du nom de domaine.|  
    |TrustedCertFileName|(Facultatif) Configuration du certificat approuvé pour le nom de domaine. Cela vous permet de créer un certificat .PFX qui contient la clé privée.|  
    |TrustedCertPassword|(Facultatif) Le mot de passe pour l’importation du fichier .PFX.|  
    |EnableVPN|(Facultatif) Activer le réseau privé virtuel (VPN) par défaut.|  
    |VpnIPv4StartAddress|(Facultatif) Définir l’adresse de démarrage du VPN.|  
    |VpnIPv4EndAddress|(Facultatif) Définir l’adresse de fin du VPN.|  
    |VpnBaseIPv6Address|(Facultatif) Définir l’adresse IPV6 de base pour le VPN.|  
    |VpnIPv6PrefixLength|(Facultatif) Définir la longueur du préfixe de l’adresse IPv6 du VPN.|  
    |IsHosted|(Facultatif) La valeur par défaut est Faux si elle n’est pas spécifiée. Définir cette valeur si vous définissez un environnement hébergeur. La configuration du routeur sera désactivée.|  
    |StaticIPv4Address|(Facultatif) Spécifier une adresse IP statique si vous souhaitez configurer une adresse IP statique au lieu d’une adresse dynamique.|  
    |StaticIPv4Gateway|(Facultatif) Spécifier une adresse de passerelle par défaut si vous souhaitez configurer une adresse IP statique au lieu d’une adresse dynamique.|  
    |StaticIPv4SubnetMask|(Facultatif) Spécifier le masque de sous-réseau si vous souhaitez configurer une adresse IP statique au lieu d'une adresse dynamique.|  
    |StaticIPv6Address|(Facultatif) Spécifier une adresse IP par défaut si vous souhaitez configurer une adresse IP statique au lieu d’une adresse dynamique.|  
    |StaticIPv6SubnetPrefixLength|(Facultatif) Spécifier la longueur par défaut du préfixe de sous-réseau IPV6 si vous souhaitez configurer une adresse IP statique au lieu d'une adresse dynamique.|  
    |StaticIPv6Gateway|(Facultatif) Spécifier une adresse de passerelle par défaut si vous souhaitez configurer une adresse IP statique au lieu d’une adresse dynamique.|  
    |ClientBackupOn|(Facultatif) Désactiver la sauvegarde du client par défaut lorsque de nouveaux clients deviennent membre du serveur.|  
    |FileHistoryOn|(Facultatif) Désactiver la sauvegarde de l’historique des fichiers par défaut lorsque de nouveaux clients exécutant Windows 8 Consumer Preview deviennent membre du serveur.|  
    |EnableRWA|Il activera les Accès web distantes lors de l’installation de Windows Server Essentials, mais ignorera la configuration du routeur. (prise en charge uniquement en cas de nouvelle installation du produit). La valeur par défaut est false.|  
    |IPv4DNSForwarder|Définir IPv4 DNS Forwarder.|  
    |IPv6DNSForwarder|Définir IPv6 DNS Forwarder.|  
    |LaunchPadHiddenTasks|-(Facultatif) vous pouvez masquer l’entrée de sauvegarde ou/et l’entrée du tableau de bord d’administration sur Launchpad.<br /><br /> -Pour désactiver le tableau de bord : LaunchPadHiddenTasks = Microsoft. LaunchPad. AdminDashboard<br /><br /> -Pour désactiver la sauvegarde : LaunchPadHiddenTasks = Microsoft. LaunchPad. Backup<br /><br /> -Pour désactiver la sauvegarde et le tableau de bord : LaunchPadHiddenTasks = Microsoft. LaunchPad. Backup, Microsoft. LaunchPad. AdminDashboard|  
  
3.  Enregistrez le fichier. Assurez-vous que vous enregistrez le fichier sous le nom cfg.ini et non pas cfg.ini.txt.  
  
    > [!NOTE]
    >  Vous pouvez enregistrer le fichier sur un lecteur flash USB, en vue de l'utiliser pour des phases spécifiques de l'installation. Il est possible également de le copier à la racine d'un disque dur sur le serveur cible. Le codage UTF-8 n'étant pas pris en charge, assurez-vous que le codage du fichier est défini sur ANSI ou Unicode.  
  
> [!IMPORTANT]
>  La section Configuration initiale du fichier cfg.ini est uniquement destinée à l'utilisateur final souhaitant personnaliser le serveur ou à un partenaire ayant l'intention de tester l'expérience utilisateur du serveur au moyen d'un fichier de réponses en mode sans assistance. Cette section du fichier n'est pas prévue pour créer l'image.  
  
## <a name="see-also"></a>Voir aussi  

 [Prise en main avec Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](Testing-the-Customer-Experience.md)

 [Prise en main avec Windows Server Essentials ADK](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Création et personnalisation de l’Image](../install/Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](../install/Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](../install/Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](../install/Testing-the-Customer-Experience.md)

