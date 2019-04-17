---
title: "Gérer l’accès Web à distance dans WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="manage-remote-web-access-in-windows-server-essentials"></a>Gérer l’accès Web à distance dans WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials
 
 Accès Web à distance dans WindowsServerEssentials, ou dans Windows Server2012R2 avec le rôle expérience de WindowsServerEssentials installé, fournit une expérience de navigateur rationalisée et tactile pour accéder à des applications et des données à partir de n’importe quel emplacement que vous disposez d’une connexion Internet et à l’aide de presque n’importe quel appareil. Pour utiliser la fonctionnalité accès Web à distance, vous devez tout d’abord activer à l’aide de l’Assistant Configuration de n’importe quel endroit accès et ensuite configurer votre routeur et votre nom de domaine.  
  
## <a name="in-this-topic"></a>Dans cette rubrique.  
  
-   [Activer et configurer l’accès Web à distance](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Configurer votre routeur](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Configurer votre nom de domaine](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Personnaliser l’accès Web à distance](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Résoudre les problèmes d’accès Web à distance](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_5)  
  
##  <a name="BKMK_1"></a>Activer et configurer l’accès Web à distance  
 Les rubriques suivantes vous aideront à activer et configurer l’accès Web à distance:  
  
-   [Vue d’ensemble de l’accès Web à distance](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Overview)  
  
-   [Activer l’accès Web à distance](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA)  
  
-   [Modifier votre région](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Region)  
  
-   [Gérer les autorisations d’accès Web à distance](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ManagePerms)  
  
-   [Sécuriser l’accès Web à distance](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SecureRWA)  
  
-   [Gérer les utilisateurs de l’accès Web à distance et VPN](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ManageRWAVPN)  
  
###  <a name="BKMK_Overview"></a>Vue d’ensemble de l’accès Web à distance  
 Lorsque vous êtes en déplacement, vous pouvez ouvrir un navigateur web et l’accès Web à distance à partir de n’importe quel endroit disposant d’un accès à Internet. Accès Web à distance, vous pouvez:  
  
-   Accéder aux fichiers partagés et des dossiers sur le serveur.  
  
-   Accéder à votre serveur et les ordinateurs sur le réseau. Cela signifie que vous pouvez accéder le bureau d’un ordinateur en réseau comme si vous étiez assis devant elle à votre bureau.  
  
  Accès Web à distance n’est pas activé par défaut. Lorsque vous exécutez l’Assistant accès en tout lieu configurer, l’Assistant tente de configurer votre routeur et la connectivité Internet. Une fois que l’accès Web à distance est activée, vous pouvez configurer un nom de domaine pour votre serveur et personnaliser l’accès Web à distance. Vous pouvez également configurer le routeur à nouveau si vous modifiez votre routeur.  
  
 Autorisation d’accès Web à distance n’est pas accordée automatiquement lorsque vous ajoutez un nouveau compte d’utilisateur. Lorsque vous ajoutez un compte d’utilisateur, vous pouvez choisir d’autoriser l’accès aux dossiers partagés, la bibliothèque multimédia, ordinateurs, liens de la page d’accueil et le serveur du tableau de bord. Vous pouvez également spécifier qu’un utilisateur ne pas être autorisés à utiliser l’accès Web à distance.  
  
 Le paramètre d’accès Web à distance est affiché pour chaque compte d’utilisateur sur le **utilisateurs** onglet du tableau de bord WindowsServerEssentials. Pour modifier le paramètre d’accès Web à distance, cliquez sur le compte d’utilisateur, puis cliquez sur **permet d’afficher les propriétés du compte**.  
  
###  <a name="BKMK_TurnOnRWA"></a>Activer l’accès Web à distance  
 Vous pouvez activer l’accès Web à distance en exécutant l’Assistant accès en tout lieu configurer à partir du tableau de bord du serveur.  
  
##### <a name="to-turn-on-remote-web-access"></a>Pour activer l’accès Web à distance  
  
1.  Ouvrez le tableau de bord.  
  
2.  Cliquez sur **paramètres**, puis cliquez sur le **accès en tout lieu** onglet.  
  
3.  Cliquez sur **configurer**. L’Assistant Configuration de n’importe quel endroit accès s’affiche.  
  
4.  Sur le **fonctionnalités choisir accès en tout lieu à activer** page, sélectionnez le **accès Web à distance** case à cocher.  
  
5.  Suivez les instructions pour terminer l’Assistant.  
  
###  <a name="BKMK_Region"></a>Modifier votre région  
 Vous devez être un administrateur réseau pour modifier le paramètre de région dans WindowsServerEssentials.  
  
##### <a name="to-change-the-region-setting"></a>Pour modifier le paramètre de région  
  
1.  Sur un ordinateur qui est connecté à WindowsServerEssentials, ouvrez le tableau de bord.  
  
2.  Cliquez sur **paramètres**.  
  
3.  Sur le **général**, cliquez sur la liste déroulante dans le **pays/région du serveur** section.  
  
4.  Dans la liste déroulante, sélectionnez la nouvelle région, puis cliquez sur **appliquer** pour accepter le nouveau paramètre de région.  
  
###  <a name="BKMK_ManagePerms"></a>Gérer les autorisations d’accès Web à distance  
 Lorsque vous ajoutez un compte d’utilisateur dans WindowsServerEssentials, le nouvel utilisateur est autorisé par défaut à utiliser accès Web à distance. Si vous avez choisi pas à autoriser l’accès Web à distance pour un compte d’utilisateur, puis trouver que l’utilisateur doit utiliser l’accès Web à distance, vous pouvez mettre à jour les propriétés de compte d’utilisateur s.  
  
##### <a name="to-manage-remote-web-access-permissions-for-a-user-account"></a>Pour gérer les autorisations d’accès Web à distance pour un compte d’utilisateur  
  
1.  Ouvrez une session sur le tableau de bord, puis cliquez sur **utilisateurs**.  
  
2.  Cliquez sur le compte d’utilisateur que vous souhaitez gérer, puis cliquez sur **permet d’afficher les propriétés du compte** dans les **tâches** volet.  
  
3.  Dans le **propriétés** boîte de dialogue, cliquez sur le **accès en tout lieu** onglet.  
  
4.  Sur le **accès en tout lieu** onglet, sélectionnez le **autoriser l’accès Web à distance et l’accès aux applications de services web** case à cocher pour autoriser un utilisateur à se connecter au serveur via l’accès Web à distance.  
  
5.  Cliquez sur **appliquer**, puis cliquez sur **OK**.  
  
 Pour plus d’informations, voir [gérer les comptes d’utilisateur](Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_SecureRWA"></a>Sécuriser l’accès Web à distance  
 WindowsServerEssentials utilise un certificat de sécurité pour vous aider à sécuriser les informations qui sont échangées entre le logiciel et un navigateur web. Lorsque vous installez le logiciel Connecteur sur vos ordinateurs, le certificat de sécurité pour WindowsServerEssentials est ajouté à la liste de certificats approuvés sur vos ordinateurs. La meilleure façon pour les utilisateurs de l’accès Web à distance lorsque vous êtes en déplacement consiste à utiliser un ordinateur portable qui a installé le logiciel du connecteur.  
  
> [!WARNING]
>  Les utilisateurs qui utilisent l’accès Web à distance à partir des emplacements publics ou autres ordinateurs non approuvés doivent s’assurer qu’ils se déconnectent du site Web avant de quitter l’ordinateur en mode sans assistance ou lorsqu’ils ont terminé leur session.  
  
###  <a name="BKMK_ManageRWAVPN"></a>Gérer les utilisateurs de l’accès Web à distance et VPN  
 Vous pouvez utiliser VPN pour se connecter à Windows Server Essentials et accéder à toutes les ressources qui sont stockés sur le serveur. Cela est particulièrement utile si vous disposez d’un ordinateur client qui est configuré avec des comptes de réseau qui peuvent être utilisés pour se connecter à un serveur Windows Server Essentials hébergé via une connexion VPN. Tous les comptes d’utilisateur récemment créé sur le serveur Windows Server Essentials hébergé doivent utiliser VPN pour se connecter à l’ordinateur client pour la première fois.  
  
##### <a name="to-set-vpn-and-remote-web-access-permissions-for-network-users"></a>Pour définir les autorisations VPN et accès Web à distance pour les utilisateurs réseau  
  
1.  Ouvrez le tableau de bord.  
  
2.  Dans la barre de navigation, cliquez sur **utilisateurs**.  
  
3.  Dans la liste des comptes d’utilisateur, sélectionnez le compte d’utilisateur que vous voulez accorder des autorisations pour accéder au Bureau à distance.  
  
4.  Dans le **< utilisateur\ > tâches** volet, cliquez sur **propriétés**.  
  
5.  Dans **< utilisateur\ > propriétés**, cliquez sur le **accès en tout lieu** onglet.  
  
6.  Sur le **accès en tout lieu** onglet, procédez comme suit:  
  
    1.  Pour autoriser un utilisateur à se connecter au serveur à l’aide de VPN, sélectionnez le **autoriser privé réseau virtuel (VPN)** case à cocher.  
  
    2.  Pour autoriser un utilisateur à se connecter au serveur à l’aide de l’accès Web à distance, sélectionnez le **autoriser l’accès Web à distance et l’accès aux applications de services web** case à cocher.  
  
7.  Cliquez sur **appliquer**, puis cliquez sur **OK**.  
  
##  <a name="BKMK_2"></a>Configurer votre routeur  
 Lorsque vous configurez votre serveur pour l’accès Web à distance, l’Assistant Configuration de n’importe quel endroit accès tente de configurer le routeur. Si vous changez les routeurs ou modifiez les paramètres sur le routeur, vous devez réexécuter l’Assistant Configuration de votre routeur. Pour plus d’informations, consultez les rubriques suivantes:  
  
-   [Configurer votre routeur](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetUpRouter)  
  
-   [Remplacer un routeur](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ReplaceRouter)  
  
-   [Emplacement réseau défini](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_NetworkLocation)  
  
-   [Activer les contrôles ActiveX des Services Bureau à distance](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ActiveX)  
  
###  <a name="BKMK_SetUpRouter"></a>Configurer votre routeur  
 Au cours de cette étape, WindowsServerEssentials tente de configurer automatiquement votre routeur à l’aide de commandes UPnP. Pour ce faire, votre routeur doit prendre en charge les normes UPnP, et le paramètre UPnP doit être activé sur votre routeur.  
  
> [!NOTE]
>  Votre configuration réseau doit respecter les exigences de réseau prises en charge pour WindowsServerEssentials. Il doit être qu’un seul routeur sur votre réseau.  
  
 Si le routeur n’est pas configuré par l’Assistant Configuration de votre domaine nom, vous devez manuellement réacheminer le port 443. Pour plus d’informations sur comment configurer le transfert de ports sur votre routeur, consultez [la configuration du routeur](https://social.technet.microsoft.com/wiki/contents/articles/windows-small-business-server-2011-essentials-router-setup.aspx).  
  
###  <a name="BKMK_ReplaceRouter"></a>Remplacer un routeur  
 Remplacez le routeur conformément aux instructions s fabricant, puis exécutez l’Assistant Configuration de votre routeur pour configurer le nouveau routeur.  
  
##### <a name="to-set-up-your-new-router"></a>Pour configurer votre routeur  
  
1.  Dans le tableau de bord WindowsServerEssentials, cliquez sur **paramètres**.  
  
2.  Cliquez sur le **accès en tout lieu** onglet, puis dans le **routeur**, cliquez sur **configurer**. L’Assistant Configuration de votre routeur démarre.  
  
3.  Suivez les instructions de l’Assistant pour terminer la configuration de votre routeur.  
  
###  <a name="BKMK_NetworkLocation"></a>Emplacement réseau défini  
 Un emplacement réseau est une collection de paramètres de réseau Windows s’applique lorsque vous vous connectez à un réseau. Les paramètres varient et peuvent être personnalisés en fonction du type de réseau que vous utilisez. Les paramètres d’un emplacement réseau déterminent si certaines fonctionnalités (telles que le partage de fichiers et de partage d’imprimante, la découverte du réseau et des dossiers publics) sont activées ou désactivé. Emplacements réseau sont utiles lorsque vous avez besoin pour se connecter à différents réseaux.  
  
 Par exemple, vous possédez peut-être un ordinateur portable que vous utilisez à la maison et sur la tâche. Lorsque vous êtes au bureau, vous vous connectez au réseau office. Toutefois, quand vous rentrez chez vous, vous utilisez votre ordinateur portable pour accéder à et lire des vidéos et musique qui sont stockés sur votre serveur domestique. Lorsque vous vous connectez à un nouveau réseau et spécifiez le type d’emplacement, Windows affecte un profil réseau prédéfini pour ce type d’emplacement. La prochaine fois que vous vous connectez à ce réseau, Windows reconnaît le réseau et affecte automatiquement les paramètres appropriés. Cela ajoute une couche de sécurité pour protéger les informations sur votre ordinateur, et uniquement les fonctionnalités réseau dont vous avez besoin pour cet emplacement sont sous tension.  
  
 Il existe quatre types d’emplacement réseau:  
  
-   **Réseau domestique** choisissez ce réseau pour les réseaux domestiques ou quand vous connaissez et que vous approuvez les personnes et les périphériques sur le réseau. Ordinateurs sur un réseau domestique peuvent appartenir à un groupe résidentiel. La découverte du réseau est activée pour les réseaux domestiques, ce qui vous permet de voir les autres ordinateurs et périphériques sur le réseau et permet d’autres utilisateurs du réseau de voir votre ordinateur.  
  
-   **Réseau professionnel** choisissez ce réseau pour une petite entreprise ou autres réseaux d’entreprise. La découverte du réseau, qui vous permet de voir les autres ordinateurs et périphériques sur un réseau et permet aux autres utilisateurs réseau de voir votre ordinateur, est activée par défaut, mais vous ne pouvez pas créer ou joindre un groupe résidentiel.  
  
-   **Réseau public** choisissez ce réseau pour des lieux publics (par exemple, dans un café ou un aéroport). Cet emplacement est conçu pour empêcher votre ordinateur d’être visible pour les autres ordinateurs et pour aider à protéger votre ordinateur contre les logiciels malveillants à partir d’Internet. Groupe résidentiel n’est pas disponible sur les réseaux publics et la découverte du réseau est désactivée. Vous devez également choisir cette option si vous êtes connecté directement à Internet sans utiliser de routeur, ou si vous avez une connexion haut débit mobile.  
  
-   **Domaine** choisissez ce réseau pour les domaines tels que ceux locaux d’entreprise. Ce type d’emplacement réseau est contrôlé par votre administrateur réseau, et il ne peut pas être sélectionné ou modifié.  
  
###  <a name="BKMK_ActiveX"></a>Activer les contrôles ActiveX des Services Bureau à distance  
 Les contrôles ActiveX des Services Bureau à distance permet d’accéder à votre ordinateur domestique ou Professionnel, via Internet, à partir d’un autre ordinateur à l’aide de l’accès Web à distance.  
  
##### <a name="to-enable-remote-desktop-services-activex-controls"></a>Pour activer les contrôles ActiveX des Services Bureau à distance  
  
1.  Dans Internet Explorer, cliquez sur **outils**, puis cliquez sur **Options Internet**.  
  
2.  Sur le **sécurité**, cliquez sur **niveau personnalisé**.  
  
3.  Dans le **ActiveX contrôles et les plug-ins** section, procédez comme suit:  
  
    1.  Sous **télécharger les contrôles ActiveX signés**, cliquez sur **invite**.  
  
    2.  Sous **exécuter les contrôles ActiveX et plug-ins**, cliquez sur **activer**.  
  
4.  Cliquez sur **OK** deux fois pour accepter les modifications et fermer la boîte de dialogue.  
  
##  <a name="BKMK_3"></a>Configurer votre nom de domaine  
 Une fois que l’accès Web à distance est activée, vous pouvez configurer un nom de domaine pour votre serveur qui exécute WindowsServerEssentials. Il s’agit d’une étape nécessaire si vous envisagez d’utiliser l’accès Web à distance à partir d’un ordinateur distant. Pour plus d’informations, consultez les rubriques suivantes:  
  
-   [Vue d’ensemble des noms de domaine](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_DNOverview)  
  
-   [Comprendre les noms de domaine personnalisés Microsoft](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_PersonalizedNames)  
  
-   [Utilisez un nom de domaine nouveau ou existant](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UseNewName)  
  
-   [Configurer un nom de domaine](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetUpName)  
  
-   [Choisir un fournisseur de service de nom de domaine](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ChooseProvider)  
  
-   [Choisissez un nom de domaine](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ChooseDomainName)  
  
-   [Choisir un préfixe de nom de domaine](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Prefixes)  
  
-   [Choisissez une extension de nom de domaine](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Extension)  
  
-   [Mettre à jour ou à niveau votre service de nom de domaine](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_UpdateService)  
  
-   [Exporter ou importer votre certificat sur votre serveur](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_ExportCert)  
  
-   [Configurer manuellement un nom de domaine](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_SetNameManually)  
  
-   [Rechercher votre fournisseur de service de noms de domaine](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_Find)  
  
###  <a name="BKMK_DNOverview"></a>Vue d’ensemble des noms de domaine  
 Un nom de domaine identifie de manière unique votre serveur sur Internet. Les noms de domaine sont constitués d’au moins deux parties: un nom de domaine de premier niveau (TLD) et un second nom de domaine de niveau. Par exemple, dans contoso.com, com est le domaine de premier niveau et contoso est le deuxième nom de domaine de niveau.  
  
 Pendant que vous êtes en déplacement, vous pouvez utiliser votre nom de domaine pour accéder aux fichiers partagés sur le serveur ou les ordinateurs sur le réseau. Vous pouvez également gérer votre serveur lorsque vous êtes absent. Par exemple, vous inscrire contoso.com pour votre serveur. Lorsque vous êtes en déplacement, vous pouvez ouvrir un navigateur web sur votre ordinateur portable et le type **contoso.com** dans la zone de texte d’adresse pour se connecter à l’instance de l’accès Web à distance que vous avez configurée sur WindowsServerEssentials.  
  
###  <a name="BKMK_PersonalizedNames"></a>Comprendre les noms de domaine personnalisés Microsoft  
 Un nom de domaine personnalisé Microsoft inclut les fonctionnalités suivantes:  
  
-   Un nom de domaine personnalisé pour l’accès Web à distance (par exemple, *votre_nom_hôte*. remotewebaccess.com). Votre nom de domaine est associé à votre adresse IP publique.  
  
-   DNS dynamiques mettre à jour le service de protocole afin que l’accès Web à distance à l’aide de votre nom de domaine ne sera pas interrompu si votre adresse IP publique change. En règle générale, les fournisseurs de services Internet (ISP) pour votre organisation s haut débit les connexions fournissent des adresses IP publiques dynamiques qui peuvent changer.  
  
-   Un certificat approuvé associé au nom de domaine.  
  
 Pour intégrer un nom de domaine personnalisé Microsoft à votre serveur, vous avez besoin d’un compte Microsoft (anciennement WindowsLiveID). Si vous ne disposez pas d’un compte Microsoft, vous pouvez inscrire un sur le [MicrosoftHotmail](https://login.live.com/) site Web.  
  
> [!IMPORTANT]
>  Windows Live permet de caractères spéciaux dans votre mot de passe du compte Microsoft que le serveur ne prend en charge pas. Si vous utilisez un domaine personnalisé Microsoft, assurez-vous que votre mot de passe du compte Microsoft contient uniquement des caractères qui prend en charge par le serveur. Le serveur ne prend pas en charge les caractères $, /, ' et %.  
  
###  <a name="BKMK_UseNewName"></a>Utilisez un nom de domaine nouveau ou existant  
 Pour configurer automatiquement votre nom de domaine sur un serveur exécutant WindowsServerEssentials, vous devez utiliser un fournisseur de service de noms de domaine est répertorié dans l’Assistant Configuration de votre domaine nom. Vous pouvez choisir obtenir un nouveau nom de domaine ou utiliser un nom de domaine existant. Effectuez l’une des opérations suivantes:  
  
-   Si vous souhaitez obtenir un nouveau nom de domaine à partir d’un du domaine fournisseurs de noms sont répertoriés dans l’Assistant, cliquez sur **je souhaite configurer un nouveau nom de domaine**.  
  
-   Si vous avez un nom de domaine existant que vous avez acheté à partir d’un des fournisseurs de service de noms de domaine pris en charge, vous pouvez utiliser l’Assistant Configuration de votre domaine nom pour configurer le nom de domaine pour votre serveur. Cliquez sur **je souhaite utiliser un nom de domaine que je possède déjà**, puis tapez le nom de domaine dans le **de votre domaine nom** zone de texte. Vous devez fournir le nom d’utilisateur et mot de passe que vous avez utilisé pour acheter le nom de domaine.  
  
-   Si vous disposez d’un nom de domaine existant que vous avez acheté chez un fournisseur de service de nom de domaine qui n’est pas pris en charge par WindowsServerEssentials, et vous souhaitez utiliser l’Assistant Configuration de votre domaine nom pour configurer le nom de domaine pour votre serveur, vous pouvez transférer le nom de domaine à un des fournisseurs de services de nom du domaine répertoriés dans l’Assistant. Cliquez sur **je souhaite utiliser un nom de domaine que je possède déjà**, tapez le nom de domaine dans le **nom de domaine** texte zone, puis suivez les instructions sur le domaine nom fournisseur s site Web du service pour transférer le nom de domaine.  
  
###  <a name="BKMK_SetUpName"></a>Configurer un nom de domaine  
 Lorsque vous activez l’accès Web à distance, vous pouvez choisir de configurer le nom de domaine Internet du serveur.  
  
##### <a name="to-set-up-or-manage-an-internet-domain-name"></a>Pour configurer ou gérer un nom de domaine Internet  
  
1.  Ouvrez le tableau de bord.  
  
2.  Cliquez sur **paramètres du serveur**, puis cliquez sur le **accès en tout lieu** onglet.  
  
3.  Dans le **nom de domaine**, cliquez sur **configurer**.  
  
4.  Suivez les instructions pour terminer l’Assistant. Si vous ne possédez pas déjà un nom de domaine et un certificat, l’Assistant vous permet de trouver un fournisseur de noms de domaine pour acheter un nom de domaine et un certificat, ou vous pouvez obtenir un nom de domaine Microsoft personnalisé.  
  
###  <a name="BKMK_ChooseProvider"></a>Choisir un fournisseur de service de nom de domaine  
 Vous devez choisir un fournisseur de service de nom de domaine qui prend en charge l’extension de nom de domaine que vous souhaitez utiliser. L’Assistant Configuration de votre domaine nom comprend une liste des fournisseurs qualifiés que vous pouvez utiliser avec un lien vers le site Web s chaque fournisseur. Cliquez sur le **Infos** lien en regard de chaque nom du fournisseur s pour obtenir des informations sur les services et tarifs offerts par le fournisseur.  
  
> [!NOTE]
>  Certains fournisseurs de service de nom de domaine servent de grandes régions internationales et d’autres servent des marchés plus petits. Pour cette raison, certains fournisseurs ne proposent pas d’un site Web traduit dans votre langue de préférence.  
  
 Lorsque vous achetez votre nom de domaine, vous pouvez également envisager d’acheter le service de protocole de mise à jour dynamique de système DNS (Domain Name) à partir de votre fournisseur de service de noms de domaine. Le protocole de mise à jour dynamique DNS est un service qui toute personne sur Internet vous permet d’accéder aux ressources sur un réseau local lorsque l’adresse IP de ce réseau change constamment. Ou vous pouvez acheter une adresse IP statique à partir de votre fournisseur (FAI) pour vous assurer que votre adresse IP ne change pas.  
  
###  <a name="BKMK_ChooseDomainName"></a>Choisissez un nom de domaine  
 Choisissez un nom qui identifie de manière unique votre serveur d’entreprise. Par exemple, si le nom de votre entreprise est Contoso Ltd, vous pouvez choisir Contoso pour identifier de façon unique votre serveur domestique ou Professionnel sur Internet. Si le nom de domaine n’est pas disponible, essayez une autre variante de ce nom ou peut-être quelque chose de complètement différent.  
  
 Le nom que vous tapez peut contenir les éléments suivants:  
  
-   63caractères au maximum  
  
-   Lettres (anglaises ou vos caractères localisés), des nombres ou des traits d’union (-). Le nom doit commencer et se terminer par une lettre ou un chiffre.  
  
    > [!NOTE]
    >  Les noms de domaine ne respectent pas la casse.  
  
###  <a name="BKMK_Prefixes"></a>Choisir un préfixe de nom de domaine  
 Un nom de domaine se compose d’étiquettes hiérarchiques.  
  
 **L’extension de domaine de niveau supérieur** est la partie la plus à droite dans le nom de domaine. Par exemple, dans www.contoso.com, com est l’extension de nom de domaine de niveau supérieur.  
  
 **Le nom de domaine de second niveau** est la partie en regard de l’extension de nom de domaine de niveau supérieur. Le nom de domaine de second niveau est souvent créé selon le nom de la société, produits ou services. Par exemple, dans www.contoso.com, contoso est le nom de domaine de second niveau et a été choisi pour le nom de la société Contoso Pharmaceuticals. Le domaine de second niveau est parfois appelé le nom d’hôte, ce qui a une adresse IP associée.  
  
 **Le préfixe de nom de domaine** identifie un sous-domaine. Le nom de sous-domaine peut être utilisé pour identifier les services, des appareils ou des régions. Par exemple, Contoso Pharmaceuticals souhaite autoriser les utilisateurs à distance pour vous connecter à accès Web à distance, mais il ne souhaite pas que le site Web soit disponible au public, afin qu’ils créent un sous-domaine qui autorise uniquement les utilisateurs disposant des autorisations appropriées pour accéder au site Web. Contoso Pharmaceuticals définit remote.contoso.com comme sous-domaine et à distance est le préfixe de nom de domaine.  
  
> [!TIP]
>  Il est recommandé d’utiliser la valeur par défaut **distant** comme préfixe de nom de domaine.  
  
###  <a name="BKMK_Extension"></a>Choisissez une extension de nom de domaine  
 Lorsque vous choisissez un nom de domaine pour votre site Web Internet, vous devez également spécifier l’extension de nom de domaine que vous souhaitez utiliser. L’extension est identifiée par les lettres qui suivent le point final de tout nom de domaine. (Le terme formel utilisé pour l’extension est le domaine de niveau supérieur ou le domaine de premier niveau.)  
  
 Il existe deux principaux types d’extensions de domaine que vous pouvez utiliser: générique et le code de pays.  
  
#### <a name="generic-top-level-domains"></a>Domaines de premier niveau génériques  
 Extensions de domaine génériques sont moins trois lettres, et ils sont généralement utilisés par certains types d’organisations.  
  
 **Exemples de domaines de premier niveau génériques**  
  
|Extension de domaine|Description|  
|----------------------|-----------------|  
|.com|Généralement utilisé par les organisations commerciales, mais elle peut être utilisée par tout le monde.|  
|.NET|Conçu pour les entreprises qui offrent des services d’infrastructure réseau.|  
|.org|Initialement utilisée par les organismes à but non lucratif et autres professionnels qui n’est pas compris dans une autre catégorie de domaine de premier niveau génériques. Peut être utilisé par tout le monde.|  
|.edu|Limité à une utilisation par des organisations pédagogiques.|  
  
#### <a name="country-code-top-level-domains"></a>Domaines de niveau supérieur de code de pays  
 Ces extensions de domaine sont deux lettres. Ils sont conçues pour être utilisées par les organisations dans le pays ou la région qui est associée à ce code. Certains domaines de premier niveau du code de pays sont limités pour une utilisation par les citoyens de ce pays ou la région. D’autres sont disponibles pour une utilisation par quiconque.  
  
 **Exemples de domaines de niveau supérieur de code de pays**  
  
|Extension de domaine|Description|  
|----------------------|-----------------|  
|.CA|Pour une utilisation par les sites Web au Canada|  
|.CN|Pour une utilisation par les sites Web en Chine|  
|.de|Pour une utilisation par les sites Web en Allemagne|  
|. co.uk|Pour une utilisation par les sites Web au Royaume-Uni|  
  
 Pour afficher la liste complète des domaines de premier niveau, voir la [site Web Internet Assigned Numbers Authority](https://go.microsoft.com/fwlink/?LinkId=117438).  
  
#### <a name="if-a-domain-extension-is-not-available-to-select-in-the-set-up-domain-name-wizard"></a>Si une extension de domaine n’est pas disponible dans l’Assistant Configuration de nom de domaine  
 Lorsque vous exécutez l’Assistant Configuration de nom de domaine, l’Assistant recherche vos informations système pour déterminer votre pays ou région. L’Assistant affiche uniquement les extensions de domaine qui prennent en charge par les fournisseurs participants de votre région. Si l’extension de domaine que vous souhaitez n’apparaît pas dans la liste, vous devez choisir une autre extension de domaine pour continuer. Sélectionnez une extension dans la liste retournée par l’Assistant.  
  
###  <a name="BKMK_UpdateService"></a>Mettre à jour ou à niveau votre service de nom de domaine  
 Vous devrez peut-être mettre à jour ou mettre à niveau votre service de nom de domaine si vous avez acheté un nom de domaine, mais que vous n’avez pas acheté un certificat. Vous devez avoir un certificat pour votre nom de domaine à partir de votre fournisseur de service de noms de domaine.  
  
> [!NOTE]
>  Fonctionne avec votre fournisseur de service de nom de domaine pour déterminer le type de certificat dont vous avez besoin. Le certificat peut être un des certificats bon marché qui sont proposés. Toutefois, vous devez passer en revue la documentation et les fonctionnalités des certificats de sécurité de niveau supérieur pour déterminer s’ils satisfont mieux aux besoins de votre entreprise.  
  
###  <a name="BKMK_ExportCert"></a>Exporter ou importer votre certificat sur votre serveur  
 Si vous souhaitez créer une copie de sauvegarde d’un certificat ou l’utiliser sur un autre serveur, vous devez exporter le certificat. Pour plus d’informations sur l’exportation des certificats, consultez [exporter un certificat](https://go.microsoft.com/fwlink/p/?LinkId=214362).  
  
###  <a name="BKMK_SetNameManually"></a>Configurer manuellement un nom de domaine  
 Si vous choisissez cette option, le serveur de ne pas analyser ou mettre à jour votre nom de domaine, et ne pas vous avertit s’il existe un problème de configuration. Vous pouvez également envisager cette option si une des opérations suivantes est vraie:  
  
-   Aucun fournisseur de nom de domaine partenaire n’est répertoriées pour votre pays ou région.  
  
-   Les fournisseurs de domaines partenaires répertoriés ne prennent pas en charge votre extension de nom de domaine.  
  
-   Vous disposez d’un nom de domaine existant à partir d’un fournisseur de noms de domaine qui n’est pas un partenaire, et vous ne souhaitez pas transférer ce nom de domaine vers un fournisseur de noms de domaine pris en charge de WindowsServerEssentials.  
  
-   L’Assistant ne répertorie pas l’extension de nom de domaine que vous souhaitez utiliser, mais l’extension est disponible à partir d’un fournisseur de noms de domaine qui n’est pas un partenaire.  
  
 Si vous choisissez de configurer manuellement votre nom de domaine, fonctionnent avec votre fournisseur de service de nom de domaine pour créer un enregistrement A pour votre domaine.  
  
##### <a name="to-create-an-a-record"></a>Pour créer un enregistrement A  
  
1.  Choisir un nom d’hôte, par exemple, à distance. Il s’agit du préfixe de nom de domaine. Le préfixe de nom de domaine et votre nom de domaine définissent l’URL pour ouvrir votre page d’ouverture de session accès Web à distance; par exemple, **http://remote.contoso.com**.  
  
2.  Dans votre domaine nom service fournisseurs configuration du tableau de bord (généralement dans leur page Web), créez l’enregistrement A pour le nom d’hôte choisi à l’étape1. Assurez-vous que l’adresse IP que vous spécifiez dans l’enregistrement A est l’adresse IP du côté WAN de votre routeur (côté accessible sur Internet). Consultez la documentation de votre routeur pour trouver votre adresse IP WAN.  
  
3.  Il est recommandé de contacter votre fournisseur de Service Internet (ISP) pour acheter une adresse IP statique pour votre réseau. Cela garantit que l’adresse IP ne change pas et que votre entrée DNS ne devient-elle pas obsolète.  
  
     Si vous n’avez pas la possibilité d’obtenir une adresse IP statique à partir de votre fournisseur de services Internet, vous pouvez également envisager d’acheter le service de protocole de mise à jour dynamique de système DNS (Domain Name) à partir de votre fournisseur de service de noms de domaine ou un autre fournisseur de services. Le protocole de mise à jour dynamique DNS est un service qui maintient l’adresse IP WAN pour votre réseau à jour afin que l’adresse IP peut être résolu en votre nom de domaine même si l’adresse IP change.  
  
4.  Importer un certificat approuvé lors de l’Assistant vous invite. Si vous ne disposez pas d’un certificat approuvé, vous pouvez obtenir un à partir d’un des fournisseurs de noms de domaine pris en charge répertoriés dans l’Assistant ou acheter un auprès du fournisseur approuvé de votre choix. Pour plus d’informations sur un certificat approuvé, contactez votre fournisseur de noms de domaine.  
  
###  <a name="BKMK_Find"></a>Rechercher votre fournisseur de service de noms de domaine  
  
##### <a name="to-find-the-domain-name-service-provider-for-your-domain-name"></a>Pour trouver le fournisseur de service de nom de domaine de votre nom de domaine  
  
1.  Ouvrez un navigateur web et tapez **www.internic.com** dans la barre d’adresses pour accéder à la page d’accueil Internic.  
  
2.  Sur la page d’accueil Internic, cliquez sur **Whois**.  
  
3.  Dans le **Whois**, tapez votre nom de domaine (par exemple contoso.com).  
  
4.  Cliquez sur le **domaine** option, puis cliquez sur **envoyer**.  
  
5.  Dans les résultats de recherche, le nom de votre fournisseur de service de nom de domaine est répertorié sous **bureau d’enregistrement**.  
  
##  <a name="BKMK_4"></a>Personnaliser l’accès Web à distance  
 Vous pouvez personnaliser votre site accès Web à distance en ajoutant une image personnelle logo ou en arrière-plan. Vous pouvez également ajouter des liens sur la page d’accueil afin que ces informations sont disponibles pour tous les utilisateurs. Pour plus d’informations, consultez les rubriques suivantes:  
  
-   [Personnaliser l’accès Web à distance](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_CustomizeRWA)  
  
-   [Personnaliser les images pour les arrière-plans et les logos](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_CustomizeImages)  
  
-   [Réparer l’accès Web à distance](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_RepairRWA)  
  
###  <a name="BKMK_CustomizeRWA"></a>Personnaliser l’accès Web à distance  
 Vous pouvez personnaliser l’accès Web à distance en modifiant le titre du site Web, la modification de l’image d’arrière-plan et le logo et ajout de liens vers d’autres sites Web sur la page d’accueil.  
  
##### <a name="to-customize-remote-web-access"></a>Pour personnaliser l’accès Web à distance  
  
1.  Ouvrez le tableau de bord.  
  
2.  Cliquez sur **paramètres**, puis cliquez sur le **accès en tout lieu** onglet.  
  
3.  Dans le **paramètres du site Web**, cliquez sur **personnaliser**.  
  
4.  Lorsque vous avez terminé de personnaliser l’accès Web à distance, cliquez sur **OK**. Tester vos modifications sur l’accès Web à distance.  
  
###  <a name="BKMK_CustomizeImages"></a>Personnaliser les images pour les arrière-plans et les logos  
 Cette section fournit des informations sur les images que vous pouvez utiliser pour personnaliser l’accès Web à distance.  
  
#### <a name="image-size"></a>Taille de l’image  
 **Images de logo**  
  
 Il est recommandé d’utiliser des images de logo 32 x 32pixels. Grandes images sont réduites à 32 x 32 et images plus petites sont agrandies au 32 x 32, ce qui pourrait déformer l’image.  
  
 **Images d’arrière-plan**  
  
 Il n’existe aucune limite de taille pour les images d’arrière-plan, pour de meilleurs résultats, il est recommandé d’utiliser des images sont environ 800 x 500pixels. L’image d’arrière-plan est placée au centre (horizontal et vertical) de la page d’ouverture de session. Pour vous aider à rendre le texte sur la page d’ouverture de session facile à lire, le centre de l’image d’arrière-plan doit être de couleur claire.  
  
#### <a name="image-file-types"></a>Types de fichiers image  
 Les types de fichier image suivants peuvent servir à remplacer le logo du site Web et en arrière-plan par défaut:  
  
-   Image bitmap (*.bmp, \*.dib, \*.rle)  
  
-   GIF (*.gif)  
  
-   PNG (*.png)  
  
-   JPG (*.jpg)  
  
###  <a name="BKMK_RepairRWA"></a>Réparer l’accès Web à distance  
 L’Assistant Réparation vous aide à détecter et résoudre les problèmes avec votre routeur ou le nom de domaine. Il existe deux façons de détecter des problèmes avec l’accès Web à distance:  
  
-   Dans Paramètres du serveur sur le tableau de bord, sous l’onglet accès en tout lieu, une icône s’affiche avec une croix rouge ainsi qu’une description du problème.  
  
-   Une alerte dans l’Afficheur des alertes.  
  
> [!NOTE]
>  L’Assistant réparation n’est pas disponible jusqu'à ce que vous activez l’accès Web à distance. Pour plus d’informations sur l’activation de l’accès Web à distance, consultez [Turn on Remote Web Access](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md#BKMK_TurnOnRWA).  
  
##### <a name="to-repair-remote-web-access"></a>Pour réparer l’accès Web à distance  
  
1.  Ouvrez une session sur le tableau de bord.  
  
2.  Cliquez sur **paramètres**, puis cliquez sur le **accès en tout lieu** onglet.  
  
3.  Cliquez sur **réparation**. Le **accès Web à distance de réparation** Assistant démarre.  
  
4.  Cliquez sur **suivant**. L’Assistant analyse l’accès Web à distance, identifie le problème et essaie alors de résoudre le problème.  
  
5.  Si vous recevez une alerte lorsque l’Assistant a terminé, vous pouvez cliquer sur **réessayer** pour essayer de résoudre le problème à nouveau. Si vous continuez à recevoir une alerte, vérifiez l’alerte pour plus d’informations sur le problème et les étapes de dépannage.  
  
##  <a name="BKMK_5"></a>Résoudre les problèmes d’accès Web à distance  
  
-   [Résoudre les problèmes de connectivité d’accès Web à distance](../support/Troubleshoot-Remote-Web-Access-connectivity-in-Windows-Server-Essentials.md)  
  
-   [Résoudre les problèmes de votre pare-feu](../support/Troubleshoot-your-firewall-in-Windows-Server-Essentials.md)  
  
-   [Dépanner l’accès en tout lieu](../support/Troubleshoot-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Options du Bureau à distance](Remote-desktop-options.md)  
  
-   [Utiliser l’accès Web à distance](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md)  
  
-   [Gérer l’accès en tout lieu](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gérer WindowsServerEssentials](Manage-Windows-Server-Essentials.md)
