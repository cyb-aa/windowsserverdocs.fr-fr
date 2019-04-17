---
title: "Créer le fichier Cfg.ini"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 93a73556-22ef-402d-b8d4-582b74c22bcf
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 967db5f36ea27fb04eab9a6682a106ba0072d45d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="create-the-cfgini-file"></a>Créer le fichier Cfg.ini

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Le fichier cfg.ini est utilisé pour automatiser l’installation du système d’exploitation dans le scénario suivant:  
  
-   Lorsque vous testez l’expérience de l’utilisateur final avec une image pré-installée sur l’ordinateur cible, la section Configuration initiale est utilisée pour procéder à l’installation dans un mode avec ou sans assistance. Pour ce faire, voir [créer la section Configuration initiale](Create-the-Cfg.ini-File.md#BKMK_CreateInit2).  
  
##  <a name="BKMK_CreateInit2"></a>Créer la section Configuration initiale  
 Utilisez la section Configuration initiale dans le fichier cfg.ini pour procéder à l’installation dans un mode avec ou sans assistance.  
  
#### <a name="to-define-the-initial-configuration-section"></a>Pour définir la section Configuration initiale  
  
1.  Ouvrez le fichier cfg.ini dans le bloc-notes, s’il existe; sinon, créez un nouveau fichier.  
  
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
    >  Une option pour sélectionner une autre langue lors de la Configuration initiale n’est pas fournie. Si le système est réinitialisé, la langue du système d’exploitation sera celle qui a été installé à l’origine.  
  
    |Nom de paramètre|Description du paramètre|  
    |--------------------|---------------------------|  
    |*AcceptEula*|Indique que l’utilisateur accepte les termes du contrat de licence logiciel Microsoft. La valeur peut être vraie ou fausse, mais l’installation se déroule uniquement si cela est définie sur True.|  
    |*AcceptOEMEula*|(Facultatif) Indique que l’utilisateur accepte le contrat de licence du partenaire. La valeur peut être vraie ou fausse. Ce champ est obligatoire uniquement si le serveur a été acheté chez un partenaire ayant fourni un contrat de licence distinct.|  
    |*CompanyName*|(Facultatif) Nom de la société. Nom de votre société est utilisé pour associer votre serveur à votre entreprise et pour personnaliser vos rapports de la société. Peut comporter jusqu'à 254caractères.|  
    |*Pays*|(Facultatif) Chaîne représentant le pays/la région souhaitée. Exemple: US pour les États-Unis.|  
    |*Nom du serveur*|Le nom du serveur identifie de manière unique le serveur sur le réseau. Nom de votre serveur doit respecter les critères suivants:<br /><br /> -Peut comporter jusqu'à 15caractères.<br /><br /> -Peut contenir des lettres, des chiffres et des traits d’union (-).<br /><br /> -Doit ne peut pas commencer par un trait d’union.<br /><br /> -Ne doit pas contenir d’espaces.<br /><br /> -Ne doit pas contenir uniquement des chiffres.<br /><br /> Exemple: ContosoServer.|  
    |*DNSName*|Un domaine interne regroupe les ordinateurs serveurs et clients afin de partager une base de données commune de noms d’utilisateur, les mots de passe et autres informations communes. Les utilisateurs voient ce nom lorsqu’ils se connectent à leurs ordinateurs, mais il est purement interne et il n’est pas identique à un nom de domaine Internet. Le nom de domaine interne doit répondre aux mêmes critères que ceux spécifiés pour le *nom_serveur*.<br /><br /> Exemple: contoso.local.|  
    |*NetbiosName*|Un nom NetBIOS est utilisé pour identifier les ressources qui sont en cours d’exécution sur le serveur. Il peut comporter jusqu'à 15caractères. Exemple: Contoso.|  
    |*Langue*|(Facultatif) Spécifie la langue d’affichage. Il peut uniquement être une des langues installées. Exemple: en-us pour l’anglais tel qu’utilisé aux États-Unis.|  
    |*Paramètres régionaux*|(Facultatif) Spécifie le format horaire et monétaire en utilisant un *LocaleID* format. Exemple: en-us pour la monnaie et l’heure affichées en anglais et formatées selon les normes utilisées aux États-Unis.|  
    |*Clavier*|Le clavier peut être dans les deux formats suivants:<br /><br /> - **disposition du clavier: de langue d’entrée.** Par exemple 0409: 00000409, où 0409 avant **:** est la langue d’entrée, et **00000409** est la disposition du clavier. Vous trouverez la liste de disposition de clavier sous la clé de Registre **dispositions HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Keyboard**.<br /><br /> - **langue d’entrée: l’identificateur de l’IME.** Vous trouverez ci-dessous la liste complète des identificateurs IME.<br /><br /> -Méthode d’entrée amharique {E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{8F96574E-C86C-4bd6-9666-3F7327D4CBE8}<br /><br /> -{81d4e9c9-1d3b-41bc-9e6c-4b40bf79e35e}{FA550B04-5AD7-411F-A5AC-CA038EC515D7} Pinyin de Microsoft - Simple rapide (chinois simplifié)<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732}{B2F9C502-1742-11D4-9790-0080C882687E} chinois (traditionnel) - nouvelle phonétique<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732}{4BDF9F03-C7D3-11D4-B2AB-0080C882687E} chinois (traditionnel) - ChangJie<br /><br /> -{531FDEBF-9B4C-4A43-A2AA-960E8FCDC732}{6024B45F-5C54-11D4-B921-0080C882687E} chinois (traditionnel) - rapide<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{D38EFF65-AA46-4FD5-91A7-67845FB02F5B} chinois traditionnel-matrice<br /><br /> -{E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{037B2C25-480C-4D7F-B027-D6CA6B69788A} chinois traditionnel-DaYi<br /><br /> -{03B5835F-F03C-411B-9CE2-AA23E1171E36}{A76C93D9-5523-4E90-AAFA-4DB112F9AC76} MicrosoftIME (japonais)<br /><br /> -{A028AE76-01B1-46C2-99C4-ACD9858AE02F}{B5FE1F02-D5F2-4445-9C03-C568F23C99A1} MicrosoftIME (coréen)<br /><br /> -{A1E2B86B-924A-4D43-80F6-8A820DF7190F}{B60AF051-257A-46BC-B9D3-84DAD819BAFB} IME Hangul ancien (coréen)<br /><br /> -Méthode d’entrée Yi {E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{409C8376-007B-4357-AE8E-26316EE3FB0D}<br /><br /> -Tigrinya {E429B25A-E5D3-4D1F-9BE3-0C608477E3A1}{3CAB88B7-CC3E-46A6-9765-B772AD7761FF} méthode d’entrée|  
    |*Paramètres*|Définit la sélection de l’utilisateur pour les mises à jour. Utilisez une des valeurs suivantes:<br /><br /> **-Tout** égal à utiliser les paramètres recommandés.<br /><br /> **-Les mises à jour** correspond à installer les mises à jour importantes. uniquement<br /><br /> **-Aucun** est égale à ne pas vérifier les mises à jour.|  
    |*Nom d’utilisateur*|-Nom du nouveau compte d’administrateur qui est créé pendant l’installation. Noms de compte d’utilisateur standard et d’administrateur doivent respecter les critères suivants:<br /><br /> -Peut comporter jusqu'à 19caractères.<br /><br /> -Ne peut pas contenir / \ [] et #124; < > + =;,? *<br /><br /> -Ne doit pas démarrer ou se terminer par une période.<br /><br /> -Ne doit pas contenir deux points consécutifs.<br /><br /> -Ne doit pas être le même que le nom du serveur ou le nom de domaine interne.<br /><br /> -Ne doit pas être identique à un nom d’utilisateur prédéfini comme administrateur ou invité.|  
    |*PlainTextPassword*|Il s’agit du mot de passe pour le nouveau compte administrateur créé pendant l’installation.<br /><br /> -Doit comporter au moins huit caractères.<br /><br /> -Doit contenir au moins trois des quatre catégories suivantes:<br /><br /> -Majuscules.<br /><br /> -Minuscules.<br /><br /> -Les numéros.<br /><br /> -Symboles.|  
    |*StdUserName*|Le nom du nouveau compte d’utilisateur standard qui est créé pendant l’installation. Consultez le *nom d’utilisateur* paramètre pour la configuration requise.|  
    |*StdUserPlainTextPassword*|Le mot de passe pour le compte d’utilisateur standard qui est créé pendant l’installation.|  
    |WebDomainName|(Facultatif) Configurer le nom de domaine Internet du serveur. Ce fichier vous permet de configurer le nom de domaine similaire à la méthode utilisée pour la configuration manuelle de l’Assistant Installation de nom de domaine.|  
    |TrustedCertFileName|(Facultatif) Configurer le certificat approuvé pour le nom de domaine. Cela vous permet de placer. Certificat PFX, qui contient la clé privée.|  
    |TrustedCertPassword|(Facultatif) Le mot de passe pour l’importation de. PFX.|  
    |EnableVPN|(Facultatif) Activer VPN par défaut.|  
    |VpnIPv4StartAddress|(Facultatif) Définir l’adresse de début VPN.|  
    |VpnIPv4EndAddress|(Facultatif) Définir l’adresse de fin VPN.|  
    |VpnBaseIPv6Address|(Facultatif) Définir l’adresse IPV6 de base pour le VPN.|  
    |VpnIPv6PrefixLength|(Facultatif) Définissez la longueur du préfixe de l’adresse IPv6 du VPN.|  
    |IsHosted|(Facultatif) Valeur par défaut est false si non spécifié. Définissez cette valeur si vous définissez un environnement hébergeur. Configuration du routeur sera désactivée.|  
    |StaticIPv4Address|(Facultatif) Spécifiez l’adresse IP statique si vous souhaitez configurer une adresse IP statique au lieu d’une adresse dynamique.|  
    |StaticIPv4Gateway|(Facultatif) Spécifiez l’adresse de passerelle par défaut si vous souhaitez configurer une adresse IP statique au lieu d’une adresse dynamique.|  
    |StaticIPv4SubnetMask|(Facultatif) Spécifier le masque de sous-réseau si vous souhaitez configurer une adresse IP statique au lieu d’une adresse dynamique.|  
    |StaticIPv6Address|(Facultatif) Spécifiez l’adresse IP par défaut si vous souhaitez configurer une adresse IP statique au lieu d’une adresse dynamique.|  
    |StaticIPv6SubnetPrefixLength|(Facultatif) Spécifiez la valeur par défaut longueur du préfixe de sous-réseau IPV6 si vous souhaitez configurer une adresse IP statique au lieu d’une adresse dynamique.|  
    |StaticIPv6Gateway|(Facultatif) Spécifiez l’adresse de passerelle par défaut si vous souhaitez configurer une adresse IP statique au lieu de dynamique.|  
    |ClientBackupOn|(Facultatif) Désactiver la fonctionnalité Client sauvegarde par défaut lorsque de nouveaux clients deviennent membre du serveur.|  
    |FileHistoryOn|(Facultatif) Désactiver l’historique des fichiers sauvegarde par défaut lorsque de nouveaux clients exécutant Windows8ConsumerPreview deviennent membre du serveur.|  
    |EnableRWA|Cela activera l’accès Web à distance lors de l’installation de WindowsServerEssentials, mais ignorera la configuration du routeur. Cela est uniquement pris en charge dans une nouvelle installation du produit. La valeur par défaut est false.|  
    |IPv4DNSForwarder|Définir IPv4 DNS Forwarder.|  
    |IPv6DNSForwarder|Définir IPv6 DNS Forwarder.|  
    |LaunchPadHiddenTasks|-(Facultatif) vous pouvez masquer entrée de sauvegarde ou / et l’entrée du tableau de bord administrateur Launchpad.<br /><br /> -Pour désactiver le tableau de bord: LaunchPadHiddenTasks=Microsoft.LaunchPad.AdminDashboard<br /><br /> -Pour désactiver la sauvegarde: LaunchPadHiddenTasks=Microsoft.LaunchPad.Backup<br /><br /> -Pour désactiver la sauvegarde et le tableau de bord: LaunchPadHiddenTasks=Microsoft.LaunchPad.Backup,Microsoft.LaunchPad.AdminDashboard|  
  
3.  Enregistrez le fichier. Assurez-vous que vous enregistrez le fichier sous cfg.ini, pas cfg.ini.txt.  
  
    > [!NOTE]
    >  Vous pouvez enregistrer le fichier sur un lecteur flash USB, ce qui peut être utilisé pour des phases spécifiques de l’installation, ou le fichier cfg.ini peut être situé à la racine d’un disque dur sur le serveur cible. Vous devez vous assurer que l’encodage pour le fichier est défini sur ANSI ou Unicode, UTF-8 codage n’est pas pris en charge.  
  
> [!IMPORTANT]
>  La section Configuration initiale de la cfg.ini doit uniquement être utilisée par l’utilisateur final souhaitant personnaliser le serveur ou d’un partenaire tester l’expérience utilisateur du serveur à l’aide d’un fichier de réponses sans assistance. Cette section du fichier n’est pas destinée à utiliser pour la création de l’image.  
  
## <a name="see-also"></a>Voir aussi  

 [Prise en main du kit ADK WindowsServerEssentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)

 [Prise en main du kit ADK WindowsServerEssentials](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Création et personnalisation de l’Image](../install/Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](../install/Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](../install/Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](../install/Testing-the-Customer-Experience.md)

