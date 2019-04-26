---
title: Gérer l'accès web à distance dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f3ea40fa-b6ba-4d66-b754-221ca6271387
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: de01b2fd2395377b6e7b3349b9862eb0e51a59b8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870000"
---
# <a name="manage-remote-web-access-in-windows-server-essentials"></a>Gérer l'accès web à distance dans Windows Server Essentials

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials
 
 Accès Web à distance dans Windows Server Essentials, ou dans Windows Server 2012 R2 avec le rôle d’expérience Windows Server Essentials installé, fournit une expérience simplifiée du navigateur tactile pour l’accès aux applications et données à partir de n’importe quel emplacement que vous avez une connexion Internet et à l’aide de presque n’importe quel appareil. Pour utiliser la fonctionnalité d’accès web à distance, vous devez d’abord l’activer à l’aide de l’Assistant Configurer l’accès en tout lieu, puis configurer vos routeur et nom de domaine.  
  
## <a name="in-this-topic"></a>Dans cette rubrique  
  
-   [Activer et configurer l’accès Web à distance](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Configurer votre routeur](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Configurer votre nom de domaine](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Personnaliser l’accès Web à distance](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Résoudre les problèmes d’accès Web à distance](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_5)  
  
##  <a name="BKMK_1"></a> Activer et configurer l’accès Web à distance  
 Les rubriques suivantes vous aident à activer et configurer l’accès web à distance :  
  
-   [Vue d’ensemble de l’accès Web à distance](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Overview)  
  
-   [Activer l’accès Web à distance](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA)  
  
-   [Modifier votre région](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Region)  
  
-   [Gérer les autorisations d’accès Web à distance](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ManagePerms)  
  
-   [Sécuriser l’accès Web à distance](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SecureRWA)  
  
-   [Gérer les utilisateurs de l’accès Web à distance et VPN](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ManageRWAVPN)  
  
###  <a name="BKMK_Overview"></a> Vue d’ensemble de l’accès Web à distance  
 Lorsque vous êtes en déplacement, vous pouvez ouvrir un navigateur web et l’accès Web à distance à partir de n’importe quel endroit qui a accès à Internet. Accès Web à distance, vous pouvez :  
  
-   accéder aux fichiers et dossiers partagés sur le serveur ;  
  
-   accéder à votre serveur et à vos ordinateurs sur le réseau. Cela signifie que vous pouvez accéder au Bureau d'un ordinateur en réseau comme si vous étiez assis en face de lui dans votre bureau.  
  
  Accès Web à distance n’est pas activé par défaut. Quand vous exécutez l'Assistant Configurer l'accès en tout lieu, il tente de configurer votre routeur et la connectivité Internet. Une fois l’accès Web à distance est activé, vous pouvez configurer un nom de domaine pour votre serveur et personnaliser l’accès Web à distance. Vous pouvez également reconfigurer le routeur si vous le modifiez.  
  
 Autorisation d’accès Web à distance ne reçoit pas automatiquement lorsque vous ajoutez un nouveau compte d’utilisateur. Quand vous ajoutez un compte d'utilisateur, vous pouvez décider d'autoriser l'accès aux dossiers partagés, à la bibliothèque multimédia, aux ordinateurs, aux liens de la page d'accueil et au tableau de bord du serveur. Vous pouvez également spécifier qu’un utilisateur ne pas censé utiliser accès Web à distance.  
  
 Le paramètre d’accès Web à distance s’affiche pour chaque compte d’utilisateur dans le **utilisateurs** onglet du tableau de bord Windows Server Essentials. Pour modifier le paramètre de l’accès Web à distance, cliquez sur le compte d’utilisateur, puis cliquez sur **afficher les propriétés du compte**.  
  
###  <a name="BKMK_TurnOnRWA"></a> Activer l’accès Web à distance  
 Vous pouvez activer l’accès web à distance en exécutant l’Assistant Configurer l’accès en tout lieu à partir du tableau de bord du serveur.  
  
##### <a name="to-turn-on-remote-web-access"></a>Pour activer l’accès web à distance  
  
1.  Ouvrez le tableau de bord.  
  
2.  Cliquez sur **Paramètres**, puis sur l’onglet **Accès en tout lieu** .  
  
3.  Cliquez sur **configurer**. L’Assistant Configurer l’accès en tout lieu s’affiche.  
  
4.  Dans la page **Choisir les fonctions d'Accès en tout lieu à activer** , cochez la case **Accès web à distance** .  
  
5.  Suivez les instructions pour exécuter l'Assistant.  
  
###  <a name="BKMK_Region"></a> Modifier votre région  
 Vous devez être administrateur réseau pour pouvoir modifier le paramètre de région dans Windows Server Essentials.  
  
##### <a name="to-change-the-region-setting"></a>Pour modifier le paramètre de région  
  
1.  Sur un ordinateur connecté à Windows Server Essentials, ouvrez le tableau de bord.  
  
2.  Cliquez sur **Paramètres**.  
  
3.  Sous l'onglet **Général** , cliquez sur la liste déroulante dans la section **Pays/région du serveur** .  
  
4.  Dans la liste déroulante, sélectionnez la nouvelle région, puis cliquez sur **Appliquer** pour accepter le nouveau paramètre de région.  
  
###  <a name="BKMK_ManagePerms"></a> Gérer les autorisations d’accès Web à distance  
 Quand vous ajoutez un compte d'utilisateur dans Windows Server Essentials, le nouvel utilisateur est autorisé par défaut à utiliser l'accès web à distance. Si vous avez choisi pas autoriser l’accès Web à distance pour un compte d’utilisateur et constatez que l’utilisateur doit utiliser l’accès Web à distance, vous pouvez mettre à jour les propriétés s du compte utilisateur.  
  
##### <a name="to-manage-remote-web-access-permissions-for-a-user-account"></a>Pour gérer les autorisations d'accès web à distance pour un compte d'utilisateur  
  
1.  Ouvrez une session sur le tableau de bord, puis cliquez sur **Utilisateurs**.  
  
2.  Cliquez sur le compte d'utilisateur que vous voulez gérer, puis cliquez sur **Afficher les propriétés du compte** dans le volet **Tâches** .  
  
3.  Dans la boîte de dialogue **Propriétés**, cliquez sur l'onglet **Accès en tout lieu**.  
  
4.  Sous l'onglet **Accès en tout lieu** , cochez la case **Autoriser l'accès web à distance et l'accès aux applications de services Web** pour autoriser un utilisateur à se connecter au serveur à l'aide de l'accès web à distance.  
  
5.  Cliquez sur **Appliquer**, puis sur **OK**.  
  
 Pour plus d’informations, consultez [Manage User Accounts](Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_SecureRWA"></a> Sécuriser l’accès Web à distance  
 Windows Server Essentials utilise un certificat de sécurité pour sécuriser les informations qui sont échangées entre le logiciel et un navigateur web. Quand vous installez le logiciel Connecteur sur vos ordinateurs, le certificat de sécurité pour Windows Server Essentials est ajouté à la liste des certificats approuvés sur vos ordinateurs. Pour des utilisateurs, la meilleure façon d'accéder à l'accès web à distance quand ils sont en déplacement consiste à utiliser un ordinateur portable doté du logiciel Connecteur.  
  
> [!WARNING]
>  Les utilisateurs qui utilisent l'accès web à distance à partir d'emplacements publics ou d'autres ordinateurs non approuvés doivent veiller à se déconnecter du site web avant de laisser l'ordinateur sans surveillance ou lorsqu'ils ont terminé leur session.  
  
###  <a name="BKMK_ManageRWAVPN"></a> Gérer les utilisateurs de l’accès Web à distance et VPN  
 Vous pouvez utiliser un réseau VPN pour vous connecter à Windows Server Essentials et accéder à toutes les ressources stockées sur le serveur. Cela est particulièrement utile si vous disposez d'un ordinateur client configuré avec des comptes réseau pouvant être utilisés pour se connecter à un serveur Windows Server Essentials hébergé via une connexion VPN. Tous les comptes d'utilisateur récemment créés sur le serveur Windows Server Essentials hébergé doivent utiliser le réseau VPN pour se connecter à l'ordinateur client pour la première fois.  
  
##### <a name="to-set-vpn-and-remote-web-access-permissions-for-network-users"></a>Pour définir des autorisations VPN et d'accès web à distance pour les utilisateurs réseau  
  
1.  Ouvrez le tableau de bord.  
  
2.  Dans la barre de navigation, cliquez sur **UTILISATEURS**.  
  
3.  Dans la liste des comptes d'utilisateur, sélectionnez le compte d'utilisateur auquel vous voulez accorder des autorisations pour accéder au Bureau à distance.  
  
4.  Dans le **< compte d’utilisateur\> tâches** volet, cliquez sur **propriétés**.  
  
5.  Dans **< compte d’utilisateur\> propriétés**, cliquez sur le **accès en tout lieu** onglet.  
  
6.  Sous l'onglet **Accès en tout lieu** , procédez comme suit :  
  
    1.  Pour autoriser un utilisateur à se connecter au serveur à l'aide d'un réseau VPN, cochez la case **Autoriser le Réseau privé virtuel (VPN)**.  
  
    2.  Pour autoriser un utilisateur à se connecter au serveur à l'aide de l'accès web à distance, cochez la case **Autoriser l'accès web à distance et l'accès aux applications de services Web**.  
  
7.  Cliquez sur **Appliquer**, puis sur **OK**.  
  
##  <a name="BKMK_2"></a> Configurer votre routeur  
 Quand vous configurez votre serveur pour l'accès web à distance, l'Assistant Configurer l'accès en tout lieu tente de configurer le routeur. Si vous changez les routeurs ou les paramètres du routeur, vous devez exécuter à nouveau l’Assistant Configurer votre routeur. Pour plus d’informations, consultez les rubriques suivantes :  
  
-   [Configurer votre routeur](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetUpRouter)  
  
-   [Remplacer un routeur](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ReplaceRouter)  
  
-   [Emplacement réseau défini](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_NetworkLocation)  
  
-   [Activer les contrôles ActiveX des Services Bureau à distance](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ActiveX)  
  
###  <a name="BKMK_SetUpRouter"></a> Configurer votre routeur  
 Dans cette étape, Windows Server Essentials tente de configurer automatiquement votre routeur à l'aide de commandes UPnP. Pour ce faire, votre routeur doit prendre en charge les normes UPnP, et le paramètre UPnP doit être activé sur votre routeur.  
  
> [!NOTE]
>  Votre configuration réseau doit respecter les conditions réseau prises en charge pour Windows Server Essentials. Il ne doit exister qu’un seul routeur sur votre réseau.  
  
 Si le routeur n’est pas configuré par l’Assistant Configuration de votre nom de domaine, vous devez manuellement réacheminer le port 443. Pour plus d’informations sur la configuration du réacheminement de port sur votre routeur, consultez [Configuration du routeur](https://social.technet.microsoft.com/wiki/contents/articles/windows-small-business-server-2011-essentials-router-setup.aspx).  
  
###  <a name="BKMK_ReplaceRouter"></a> Remplacer un routeur  
 Remplacez le routeur conformément aux instructions s fabricant et puis exécutez l’Assistant Configuration de votre routeur pour configurer le nouveau routeur.  
  
##### <a name="to-set-up-your-new-router"></a>Pour configurer votre routeur  
  
1.  Dans le tableau de bord Windows Server Essentials, cliquez sur **Paramètres**.  
  
2.  Cliquez sur l'onglet **Accès en tout lieu** , puis dans la section **Routeur** , cliquez sur **Configurer**. L'Assistant Configuration de votre routeur démarre.  
  
3.  Suivez les instructions de cet Assistant pour terminer la configuration de votre nouveau routeur.  
  
###  <a name="BKMK_NetworkLocation"></a> Emplacement réseau défini  
 Un emplacement réseau est une collection de paramètres réseau que Windows applique quand vous vous connectez à un réseau. Ces paramètres varient et peuvent être personnalisés en fonction du type de réseau que vous utilisez. Les paramètres relatifs à un emplacement réseau déterminent si certaines fonctionnalités (telles que le partage de fichiers et d'imprimantes, la découverte de réseau et le partage de dossiers publics) sont activées ou non. Les emplacements réseau sont utiles quand vous devez vous connecter à différents réseaux.  
  
 Par exemple, vous possédez peut-être un ordinateur portable que vous utilisez à la maison et à votre travail. Quand vous êtes au bureau, vous vous connectez au réseau de votre bureau. Toutefois, quand vous rentrez chez vous, vous utilisez votre ordinateur portable pour accéder à des vidéos et à de la musique qui sont stockés sur votre serveur domestique. Quand vous vous connectez à un nouveau réseau et spécifiez le type d'emplacement, Windows affecte un profil réseau prédéfini pour ce type d'emplacement. La prochaine fois que vous vous connectez à ce réseau, Windows reconnaît le réseau et affecte automatiquement les paramètres appropriés. Cela ajoute une couche de sécurité qui renforce la protection des informations figurant sur votre ordinateur, et seules les fonctionnalités réseau dont vous avez besoin pour cet emplacement sont activées.  
  
 Il existe quatre types d'emplacement réseau :  
  
-   **Réseau domestique** Choisissez ce réseau pour les réseaux domestiques ou quand vous connaissez les personnes et les périphériques présents sur le réseau et avez confiance en eux. Les ordinateurs figurant sur un réseau domestique peuvent appartenir à un groupe résidentiel. La découverte de réseau est activée pour les réseaux domestiques. Elle vous permet de voir les autres ordinateurs et périphériques figurant sur le réseau et elle permet aux autres utilisateurs du réseau de voir votre ordinateur.  
  
-   **Réseau professionnel** Choisissez ce réseau pour une petite entreprise ou d'autres réseaux de votre lieu de travail. La découverte de réseau, qui vous permet de voir les autres ordinateurs et périphériques sur un réseau et qui permet aux autres utilisateurs du réseau de voir votre ordinateur, est activée par défaut, mais vous ne pouvez pas créer ni rejoindre un groupe résidentiel.  
  
-   **Réseau public** Choisissez ce réseau pour des lieux publics (par exemple, dans un café ou un aéroport). Cet emplacement est conçu pour empêcher votre ordinateur d'être visible pour les autres ordinateurs et pour vous aider à protéger votre ordinateur contre les logiciels malveillants présents sur Internet. Les groupes résidentiels ne sont pas disponibles sur les réseaux publics et la découverte de réseau est désactivée. Vous devez également choisir cette option si vous êtes connecté directement à Internet sans utiliser de routeur ou si vous disposez d'une connexion haut débit mobile.  
  
-   **Domaine** Choisissez ce réseau pour les domaines tels que ceux des locaux d'entreprise. Ce type d'emplacement réseau est contrôlé par votre administrateur réseau et il ne peut pas être sélectionné ni modifié.  
  
###  <a name="BKMK_ActiveX"></a> Activer les contrôles ActiveX des Services Bureau à distance  
 Les contrôles ActiveX des Services Bureau à distance permet d’accéder à votre ordinateur personnel ou Professionnel, via Internet, depuis un autre ordinateur à l’aide de l’accès Web à distance.  
  
##### <a name="to-enable-remote-desktop-services-activex-controls"></a>Pour activer les contrôles ActiveX des services Bureau à distance  
  
1.  Dans Internet Explorer, cliquez sur **Outils**, puis sur **Options Internet**.  
  
2.  Sous l'onglet **Sécurité** , cliquez sur **Personnaliser le niveau**.  
  
3.  Dans la section **Contrôles ActiveX et plug-ins**, procédez comme suit :  
  
    1.  Sous **Télécharger les contrôles ActiveX signés**, cliquez sur **Demander**.  
  
    2.  Sous **Exécuter les contrôles ActiveX et les plug-ins**, cliquez sur **Activer**.  
  
4.  Cliquez sur **OK** deux fois pour accepter les modifications et fermer la boîte de dialogue.  
  
##  <a name="BKMK_3"></a> Configurer votre nom de domaine  
 Une fois l'accès web à distance activé, vous pouvez configurer un nom de domaine pour votre serveur qui exécute Windows Server Essentials. Il s’agit d’une étape nécessaire si vous envisagez d’utiliser l’accès web à distance à partir d’un ordinateur distant. Pour plus d’informations, consultez les rubriques suivantes :  
  
-   [Vue d’ensemble des noms de domaine](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_DNOverview)  
  
-   [Comprendre les noms de domaine personnalisés Microsoft](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_PersonalizedNames)  
  
-   [Utiliser un nom de domaine nouveau ou existant](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UseNewName)  
  
-   [Configurer un nom de domaine](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetUpName)  
  
-   [Choisir un fournisseur de service de nom de domaine](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ChooseProvider)  
  
-   [Choisissez un nom de domaine](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ChooseDomainName)  
  
-   [Choisissez un préfixe de nom de domaine](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Prefixes)  
  
-   [Choisissez une extension de nom de domaine](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Extension)  
  
-   [Mettre à jour ou mise à niveau de votre service de nom de domaine](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UpdateService)  
  
-   [Exporter ou importer votre certificat sur votre serveur](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ExportCert)  
  
-   [Configurer manuellement un nom de domaine](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetNameManually)  
  
-   [Rechercher votre fournisseur de service de noms de domaine](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Find)  
  
###  <a name="BKMK_DNOverview"></a> Vue d’ensemble des noms de domaine  
 Un nom de domaine identifie de manière unique votre serveur sur Internet. Les noms de domaine sont constitués d'au moins deux parties : un nom de domaine de premier niveau et un nom de domaine de second niveau. Par exemple, Contoso.com, com est le domaine de premier niveau et contoso est le deuxième nom de domaine de niveau.  
  
 Quand vous n'êtes pas dans votre bureau, vous pouvez utiliser votre nom de domaine pour accéder aux fichiers partagés sur le serveur ou les ordinateurs figurant dans le réseau. Vous pouvez également gérer votre serveur quand vous êtes absent. Par exemple, vous inscrivez contoso.com pour votre serveur. Quand vous n'êtes pas dans votre bureau, vous pouvez ouvrir un navigateur web sur votre ordinateur portable et taper **contoso.com** dans la zone textuelle d'adresse pour vous connecter à l'instance de l'accès web à distance que vous avez configurée sur Windows Server Essentials.  
  
###  <a name="BKMK_PersonalizedNames"></a> Comprendre les noms de domaine personnalisés Microsoft  
 Un nom de domaine personnalisé Microsoft inclut les fonctionnalités suivantes :  
  
-   Un nom de domaine personnalisé pour l’accès Web à distance (par exemple, *votre_nom_hôte*. remotewebaccess.com). Votre nom de domaine est associé à votre adresse IP publique.  
  
-   DNS dynamique mettre à jour le service de protocole afin que l’accès Web à distance à l’aide de votre nom de domaine ne sera pas interrompu si votre adresse IP publique change. En général, les fournisseurs de services Internet (ISP) pour votre organisation s haut débit les connexions fournissent des adresses IP publiques dynamiques qui peuvent changer.  
  
-   Un certificat approuvé associé au nom de domaine.  
  
 Pour intégrer un nom de domaine personnalisé Microsoft à votre serveur, vous avez besoin d'un compte Microsoft (anciennement appelé identifiant Windows Live ID). Si vous n’avez pas de compte Microsoft, vous pouvez en demander un sur le site web [Microsoft Hotmail](https://login.live.com/) .  
  
> [!IMPORTANT]
>  Windows Live autorise l'utilisation de caractères spéciaux dans le mot de passe de votre compte Microsoft que le serveur ne prend pas en charge. Si vous utilisez un domaine personnalisé Microsoft, assurez-vous que le mot de passe de votre compte Microsoft contient uniquement des caractères pris en charge par le serveur. Le serveur ne prend pas en charge les caractères $, /, ' et %.  
  
###  <a name="BKMK_UseNewName"></a> Utiliser un nom de domaine nouveau ou existant  
 Pour configurer automatiquement votre nom de domaine sur un serveur exécutant Windows Server Essentials, vous devez utiliser un fournisseur de services de noms de domaine répertorié dans l'Assistant Configuration de votre nom de domaine. Vous pouvez choisir d'obtenir un nouveau nom de domaine ou d'utiliser un nom de domaine existant. Faites une des actions suivantes :  
  
-   Si vous souhaitez obtenir un nouveau nom de domaine à partir d'un des fournisseurs de services de noms de domaine répertoriés dans l'Assistant, cliquez sur **Je souhaite configurer un nouveau nom de domaine**.  
  
-   Si vous possédez un nom de domaine existant que vous avez acheté auprès d'un des fournisseurs de services de noms de domaine pris en charge, vous pouvez utiliser l'Assistant Configuration de votre nom de domaine pour configurer le nom de domaine de votre serveur. Cliquez sur **Je souhaite utiliser un nom de domaine que je possède déjà**, puis tapez le nom de domaine dans la zone de texte **Configuration de votre nom de domaine**. Vous devez fournir le nom d'utilisateur et le mot de passe que vous avez utilisés pour acheter le nom de domaine.  
  
-   Si vous possédez un nom de domaine existant que vous avez acheté auprès d'un fournisseur de services de noms de domaine qui n'est pas pris en charge par Windows Server Essentials et que vous souhaitez utiliser l'Assistant Configuration de votre nom de domaine pour configurer le nom de domaine de votre serveur, vous pouvez transférer le nom de domaine à l'un des fournisseurs de services de noms de domaine répertoriés dans l'Assistant. Cliquez sur **je souhaite utiliser un nom de domaine que je possède déjà**, tapez le nom de domaine dans le **nom de domaine** texte zone, puis suivez les instructions sur le domaine nom fournisseur s site Web du service pour transférer le nom de domaine .  
  
###  <a name="BKMK_SetUpName"></a> Configurer un nom de domaine  
 Quand vous activez l'accès web à distance, vous pouvez choisir de configurer le nom de domaine Internet du serveur.  
  
##### <a name="to-set-up-or-manage-an-internet-domain-name"></a>Pour configurer ou gérer un nom de domaine Internet  
  
1.  Ouvrez le tableau de bord.  
  
2.  Cliquez sur **Paramètres du serveur**, puis sur l'onglet **Accès en tout lieu** .  
  
3.  Dans la section **Nom de domaine**, cliquez sur **Configurer**.  
  
4.  Suivez les instructions pour exécuter l'Assistant. Si vous ne possédez pas encore un nom de domaine et un certificat, l'Assistant vous aidera à trouver un fournisseur de noms de domaine pour acheter un nom de domaine et un certificat, où vous pouvez obtenir un nom de domaine Microsoft personnalisé.  
  
###  <a name="BKMK_ChooseProvider"></a> Choisir un fournisseur de service de nom de domaine  
 Vous devez choisir un fournisseur de services de noms de domaine qui prenne en charge l'extension de nom de domaine que vous voulez utiliser. La valeur votre Assistant nom de domaine inclut une liste des fournisseurs qualifiés que vous pouvez utiliser avec un lien vers chaque site Web de fournisseur s. Cliquez sur le **Infos** lien en regard de chaque nom de fournisseur s pour obtenir des informations sur les services et tarifs offerts par le fournisseur.  
  
> [!NOTE]
>  Certains fournisseurs de services de noms de domaine servent de grandes régions internationales, alors que d'autres servent des marchés plus petits. Par conséquent, certains fournisseurs ne proposent pas un site web traduit dans votre langue de préférence.  
  
 Lorsque vous achetez votre nom de domaine, vous pouvez également envisager d'acheter le service de protocole de mise à jour dynamique DNS (Domain Name System) auprès de votre fournisseur de services de noms de domaine. Le protocole de mise à jour dynamique DNS est un service qui permet à toute personne sur Internet d'accéder aux ressources présentes sur un réseau local quand l'adresse IP de ce réseau change constamment. Vous pouvez également acheter une adresse IP statique auprès de votre fournisseur de services Internet pour vous assurer que votre adresse IP ne change pas.  
  
###  <a name="BKMK_ChooseDomainName"></a> Choisissez un nom de domaine  
 Choisissez un nom qui identifie de façon unique votre serveur d'entreprise. Par exemple, si le nom de votre entreprise est Contoso Ltd, vous pouvez choisir Contoso pour identifier de façon unique votre serveur domestique ou d'entreprise sur Internet. Si ce nom de domaine n'est pas disponible, essayez une autre variante de ce nom ou peut-être quelque chose de complètement différent.  
  
 Le nom que vous tapez peut contenir :  
  
-   63 caractères au maximum ;  
  
-   des lettres (anglaises ou vos caractères localisés), des chiffres et des traits d'union (-). Le nom doit commencer et se terminer par une lettre ou un chiffre.  
  
    > [!NOTE]
    >  Les noms de domaine ne respectent pas la casse.  
  
###  <a name="BKMK_Prefixes"></a> Choisissez un préfixe de nom de domaine  
 Un nom de domaine se compose d'étiquettes hiérarchiques.  
  
 L'**extension de domaine de premier niveau** est la partie la plus à droite du nom de domaine. Par exemple, dans www.contoso.com, com est l’extension de nom de domaine de niveau supérieur.  
  
 Le **nom de domaine de second niveau** est la partie en regard de l'extension de nom de domaine de premier niveau. Le nom de domaine de second niveau est souvent créé à partir du nom, des produits ou des services de la société. Par exemple, dans www.contoso.com, contoso est le nom de domaine de second niveau et a été choisie pour le nom de la société Contoso Pharmaceuticals. Le nom de domaine de second niveau est parfois appelé le nom d'hôte, auquel une adresse IP est associée.  
  
 Le**préfixe de nom de domaine** identifie un sous-domaine. Le nom de sous-domaine peut être utilisé pour identifier des services, des appareils ou des régions. Par exemple, la société Contoso Pharmaceuticals souhaite autoriser les utilisateurs distants à se connecter via l'accès web à distance, mais elle ne veut pas que le site web soit disponible au public. Par conséquent, elle crée un sous-domaine qui autorise uniquement les utilisateurs disposant des autorisations appropriées à accéder au site web. Contoso Pharmaceuticals définit remote.contoso.com comme sous-domaine et « remote » est le préfixe de nom de domaine.  
  
> [!TIP]
>  Il est recommandé d'utiliser la valeur par défaut **Remote** comme préfixe de nom de domaine.  
  
###  <a name="BKMK_Extension"></a> Choisissez une extension de nom de domaine  
 Quand vous choisissez un nom de domaine pour votre site Internet, vous devez également spécifier l'extension de nom de domaine que vous voulez utiliser. Cette extension correspond aux lettres qui suivent le point final de tout nom de domaine. (Le terme formel utilisé pour l’extension est le domaine de niveau supérieur ou d’un domaine de premier niveau.)  
  
 Il existe deux principaux types d'extensions de domaine que vous pouvez utiliser : les extensions génériques et les codes de pays.  
  
#### <a name="generic-top-level-domains"></a>Domaines de premier niveau génériques  
 Les extensions de domaine génériques correspondent à au moins trois lettres et sont généralement utilisées par certains types d'organisations.  
  
 **Exemples de domaines de premier niveau génériques**  
  
|Extension de domaine|Description|  
|----------------------|-----------------|  
|.com|Généralement utilisée par les organisations commerciales, mais elle peut être utilisée par tout le monde.|  
|.net|Conçue pour les entreprises qui offrent des services d'infrastructure réseau.|  
|.org|Initialement utilisée par les organismes à but non lucratif et par d'autres professionnels qui ne correspondaient pas aux autres catégories génériques de domaine de premier niveau. Peut être utilisée par tout le monde.|  
|.edu|Limitée à une utilisation par des organisations pédagogiques.|  
  
#### <a name="country-code-top-level-domains"></a>Domaines de premier niveau correspondant à des codes de pays  
 Ces extensions de domaine comportent deux lettres. Elles sont conçues pour être utilisées par les organisations dans le pays ou la région associé(e) à ce code. Certains domaines de premier niveau correspondant à des codes de pays sont limités pour n'être utilisés que par les citoyens de ce pays ou les habitants de cette région. D'autres peuvent être utilisés par tout le monde.  
  
 **Exemples de code de pays les domaines de niveau supérieur**  
  
|Extension de domaine|Description|  
|----------------------|-----------------|  
|.ca|Utilisable par les sites web au Canada|  
|.cn|Utilisable par les sites web en Chine|  
|.de|Utilisable par les sites web en Allemagne|  
|.co.uk|Utilisable par les sites web au Royaume-Uni|  
  
 Pour afficher la liste complète des domaines de premier niveau, consultez le [site web IANA (Internet Assigned Numbers Authority)](https://go.microsoft.com/fwlink/?LinkId=117438).  
  
#### <a name="if-a-domain-extension-is-not-available-to-select-in-the-set-up-domain-name-wizard"></a>Si une extension de domaine n'est pas disponible dans l'Assistant Configuration de votre nom de domaine  
 Quand vous exécutez l'Assistant Configuration de votre nom de domaine, l'Assistant recherche vos informations système pour déterminer votre pays ou votre région. L'Assistant affiche uniquement les extensions de domaine prises en charge par les fournisseurs participants de votre localité. Si l'extension de domaine que vous souhaitez n'apparaît pas dans la liste, vous devez choisir une autre extension de domaine pour continuer. Sélectionnez une extension dans la liste retournée par l'Assistant.  
  
###  <a name="BKMK_UpdateService"></a> Mettre à jour ou mise à niveau de votre service de nom de domaine  
 Vous devrez peut-être mettre à jour ou à niveau votre service de noms de domaine si vous avez acheté un nom de domaine mais pas de certificat. Vous devez disposer d'un certificat pour votre nom de domaine, délivré par votre fournisseur de services de noms de domaine.  
  
> [!NOTE]
>  Associez-vous à votre fournisseur de services de noms de domaine pour déterminer le type de certificat dont vous avez besoin. Ce certificat peut être un des certificats bon marché qui sont proposés. Toutefois, vous devez passer en revue la documentation et les fonctionnalités des certificats de sécurité de niveau supérieur pour déterminer s'ils satisfont mieux aux besoins de votre entreprise.  
  
###  <a name="BKMK_ExportCert"></a> Exporter ou importer votre certificat sur votre serveur  
 Si vous souhaitez créer une copie de sauvegarde d'un certificat ou l'utiliser sur un autre serveur, vous devez exporter le certificat. Pour plus d’informations sur l’exportation des certificats, consultez [Exporter un certificat](https://go.microsoft.com/fwlink/p/?LinkId=214362).  
  
###  <a name="BKMK_SetNameManually"></a> Configurer manuellement un nom de domaine  
 Si vous choisissez cette option, le serveur n’analyse pas votre nom de domaine et ne le gère pas non plus, et il ne vous avertit pas en cas de problème de configuration. Vous pouvez également envisager cette option si l’une des conditions suivantes est remplie :  
  
-   Aucun fournisseur de noms de domaine partenaire n’est répertorié pour votre pays ou votre région.  
  
-   Les fournisseurs de domaines partenaires répertoriés ne prennent pas en charge l’extension de votre nom de domaine.  
  
-   Vous avez obtenu un nom de domaine auprès d'un fournisseur de noms de domaine qui n'est actuellement pas en partenariat, et vous ne voulez pas transférer ce nom de domaine vers un fournisseur de noms de domaine pris en charge par Windows Server Essentials.  
  
-   L’Assistant ne répertorie pas l’extension de nom de domaine que vous voulez utiliser, mais cette extension est disponible auprès d’un fournisseur de noms de domaine qui n’est pas en partenariat actuellement.  
  
 Si vous choisissez de configurer manuellement votre nom de domaine, fonctionnent avec votre fournisseur de service de nom de domaine pour créer un enregistrement A pour votre domaine.  
  
##### <a name="to-create-an-a-record"></a>Pour créer un enregistrement A  
  
1.  Choisissez un nom d’hôte, par exemple à distance. Il s’agit du préfixe de nom de domaine. Le préfixe de nom de domaine et votre nom de domaine définissent l’URL pour ouvrir la page d’ouverture de session accès Web à distance. par exemple, **http://remote.contoso.com**.  
  
2.  Dans votre domaine nom service fournisseurs configuration du tableau de bord (généralement sur la page Web), créez l’enregistrement A pour le nom d’hôte choisi à l’étape 1. Assurez-vous que l’adresse IP que vous spécifiez dans l’enregistrement A est l’adresse IP du côté WAN de votre routeur (Internet face côté). Pour trouver votre adresse IP WAN, voir la documentation de votre routeur.  
  
3.  Il est recommandé de contacter votre fournisseur de services Internet pour acheter une adresse IP statique pour votre réseau. Cela permet de garantir que l’adresse IP n’est pas modifiée et que votre entrée DNS ne devient pas obsolète.  
  
     Si vous n'avez pas la possibilité d'obtenir une adresse IP statique auprès de votre fournisseur de services Internet, vous pouvez également envisager d'acheter un service de protocole de mise à jour dynamique DNS auprès de votre fournisseur de services de noms de domaine ou d'un autre fournisseur de services. Le protocole de mise à jour dynamique DNS est un service qui maintient à jour l'adresse IP WAN pour votre réseau afin que cette adresse IP puisse être résolue en nom de domaine même si l'adresse IP change.  
  
4.  Importez un certificat approuvé lorsque l’Assistant vous le demande. Si vous n’avez pas de certificat approuvé, vous pouvez en obtenir un auprès de l’un des fournisseurs de noms de domaine pris en charge répertoriés dans l’Assistant ou en acheter un auprès du fournisseur approuvé de votre choix. Pour plus d’informations sur un certificat approuvé, contactez votre fournisseur de noms de domaine.  
  
###  <a name="BKMK_Find"></a> Rechercher votre fournisseur de service de noms de domaine  
  
##### <a name="to-find-the-domain-name-service-provider-for-your-domain-name"></a>Pour rechercher le fournisseur de services de noms de domaine pour votre nom de domaine  
  
1.  Ouvrez un navigateur web et tapez **www.internic.com** dans la barre d'adresse pour accéder à la page d'accueil du site web Internic.  
  
2.  Dans la page d'accueil du site web Internic, cliquez sur **Whois**.  
  
3.  Dans la zone de recherche****, tapez votre nom de domaine (par exemple, contoso.com).  
  
4.  Cochez l'option **Domain**, puis cliquez sur **Submit**.  
  
5.  Dans les résultats de recherche, le nom de votre fournisseur de services de noms de domaine est répertorié sous **Registrar**.  
  
##  <a name="BKMK_4"></a> Personnaliser l’accès Web à distance  
 Vous pouvez personnaliser votre site d’accès web à distance en ajoutant un logo personnel ou une image en arrière-plan. Vous pouvez également ajouter des liens à la page d’accueil afin que ces informations soient disponibles pour l’ensemble de vos utilisateurs. Pour plus d’informations, consultez les rubriques suivantes :  
  
-   [Personnaliser l’accès Web à distance](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_CustomizeRWA)  
  
-   [Personnaliser les images pour les arrière-plans et les logos](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_CustomizeImages)  
  
-   [Réparation de l’accès Web à distance](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_RepairRWA)  
  
###  <a name="BKMK_CustomizeRWA"></a> Personnaliser l’accès Web à distance  
 Vous pouvez personnaliser l'accès web à distance en modifiant le titre du site web, en modifiant l'image d'arrière-plan et le logo, et en ajoutant des liens vers d'autres sites web dans la page d'accueil.  
  
##### <a name="to-customize-remote-web-access"></a>Pour personnaliser l'accès web à distance  
  
1.  Ouvrez le tableau de bord.  
  
2.  Cliquez sur **Paramètres**, puis sur l’onglet **Accès en tout lieu** .  
  
3.  Dans la section **Paramètres du site Web** , cliquez sur **Personnaliser**.  
  
4.  Une fois que vous avez fini de personnaliser l'accès web à distance, cliquez sur **OK**. Testez vos modifications concernant l'accès web à distance.  
  
###  <a name="BKMK_CustomizeImages"></a> Personnaliser les images pour les arrière-plans et les logos  
 Cette section fournit des informations sur les images que vous pouvez utiliser pour personnaliser l'accès web à distance.  
  
#### <a name="image-size"></a>Taille des images  
 **Images de logo**  
  
 Il est recommandé d'utiliser des images de logo de 32 x 32 pixels. Les images de plus grande taille sont réduites au format 32 x 32 et les images plus petites sont agrandies au format 32 x 32, ce qui pourrait déformer l'image.  
  
 **Images d’arrière-plan**  
  
 Il n'existe aucune limite de taille pour les images d'arrière-plan, mais pour obtenir un résultat optimal, il est recommandé d'utiliser des images d'environ 800 x 500 pixels. L'image d'arrière-plan est placée au centre (horizontal et vertical) de la page de connexion. Pour faciliter la lecture du texte dans la page de connexion, le centre de l'image d'arrière-plan doit être de couleur claire.  
  
#### <a name="image-file-types"></a>Types des fichiers image  
 Les types de fichier image répertoriés ci-dessous peuvent être utilisés pour remplacer le logo du site web et d'arrière-plan par défaut :  
  
-   Bitmap (*.bmp, \*.dib, \*.rle)  
  
-   GIF (*.gif)  
  
-   PNG (*.png)  
  
-   JPG (*.jpg)  
  
###  <a name="BKMK_RepairRWA"></a> Réparation de l’accès Web à distance  
 L'Assistant Réparation vous aide à détecter et à résoudre d'éventuels problèmes avec votre routeur ou votre nom de domaine. Il existe deux façons de détecter des problèmes liés à l'accès web à distance :  
  
-   Dans Paramètres du serveur, dans le tableau de bord, sous l'onglet Accès en tout lieu, une icône avec une croix rouge accompagne la description du problème.  
  
-   Une alerte figure dans l'Afficheur des alertes.  
  
> [!NOTE]
>  L'Assistant Réparation n'est pas disponible tant que vous n'activez pas l'accès web à distance. Pour plus d’informations sur l’activation de l’accès web à distance, consultez [Turn on Remote Web Access](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA).  
  
##### <a name="to-repair-remote-web-access"></a>Pour réparer l'accès web à distance  
  
1.  Ouvrez une session dans le tableau de bord.  
  
2.  Cliquez sur **Paramètres**, puis sur l’onglet **Accès en tout lieu** .  
  
3.  Cliquez sur **Réparer**. L'Assistant **Réparation de l'accès web à distance** démarre.  
  
4.  Cliquez sur **Suivant**. L'Assistant analyse l'accès web à distance, identifie le problème, puis tente de réparer ce problème.  
  
5.  Si vous recevez une alerte quand l'Assistant a terminé, vous pouvez cliquer sur **Réessayer** pour essayer à nouveau de réparer le problème. Si vous continuez de recevoir une alerte, examinez les détails de cette alerte pour plus d'informations sur le problème et sur la procédure de résolution.  
  
##  <a name="BKMK_5"></a> Résoudre les problèmes d’accès Web à distance  
  
-   [Résoudre les problèmes de connectivité de l’accès Web à distance](../support/Troubleshoot-Remote-Web-Access-connectivity-in-Windows-Server-Essentials.md)  
  
-   [Résoudre les problèmes de votre pare-feu](../support/Troubleshoot-your-firewall-in-Windows-Server-Essentials.md)  
  
-   [Dépanner l’accès en tout lieu](../support/Troubleshoot-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Options du Bureau à distance](Remote-desktop-options.md)  
  
-   [Utiliser l’accès Web à distance](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gérer l’accès en tout lieu](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gérer Windows Server Essentials](Manage-Windows-Server-Essentials.md)
