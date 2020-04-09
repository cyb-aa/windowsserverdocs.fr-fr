---
title: Configurer DirectAccess dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: c959b6fc-c67e-46cd-a9cb-cee71a42fa4c
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d8029b954a5957433fb0fcc71d3bef610a187939
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819703"
---
# <a name="configure-directaccess-in-windows-server-essentials"></a>Configurer DirectAccess dans Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Cette rubrique fournit des instructions détaillées sur la configuration de DirectAccess dans Windows Server Essentials pour permettre à votre personnel itinérant de se connecter en toute transparence au réseau de votre organisation à partir de tout emplacement distant équipé d’Internet sans avoir à établir de connexion de réseau privé virtuel (VPN). DirectAccess peut offrir aux employés mobiles la même expérience de connectivité à l’intérieur et à l’extérieur du Bureau à partir de leurs ordinateurs Windows 8.1, Windows 8 et Windows 7.  
  
 Dans Windows Server Essentials, si le domaine contient plusieurs serveurs Windows Server Essentials, DirectAccess doit être configuré sur le contrôleur de domaine.  
  
> [!NOTE]
>  Cette rubrique fournit des instructions pour configurer DirectAccess lorsque votre serveur Windows Server Essentials est le contrôleur de domaine. Si le serveur Windows Server Essentials est membre d’un domaine, suivez les instructions pour configurer DirectAccess sur un membre du domaine dans [Ajouter DirectAccess à un déploiement d’accès à distance (VPN) existant à](https://technet.microsoft.com/library/jj574220.aspx) la place.  
  
## <a name="process-overview"></a>Vue d'ensemble du processus  
 Pour configurer DirectAccess dans Windows Server Essentials, procédez comme suit.  
  
> [!IMPORTANT]
>  Avant d’utiliser les procédures de ce guide pour configurer DirectAccess dans Windows Server Essentials, vous devez activer le VPN sur le serveur. Pour obtenir des instructions, consultez [gérer le VPN](Manage-VPN-in-Windows-Server-Essentials.md).  
  
-   [Étape 1 : ajouter les outils de gestion de l’accès à distance à votre serveur](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_AddRAM)  
  
-   [Étape 2 : remplacer l’adresse de la carte réseau du serveur par une adresse IP statique](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_AddStaticIP)  
  
-   [Étape 3 : préparer un certificat et un enregistrement DNS pour le serveur emplacement réseau](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS)  
  
    -   [Étape 3a : accorder les autorisations maximales aux utilisateurs authentifiés pour le modèle de certificat du serveur Web](#BKMK_GrantFullPermissions)  
  
    -   [Étape 3b : inscrire un certificat pour le serveur emplacement réseau avec un nom commun qui ne peut pas être résolu à partir du réseau externe](#BKMK_EnrollaCertificate)  
  
    -   [Étape 3c : ajouter un nouvel hôte sur le serveur DNS et le mapper à l’adresse du serveur Windows Server Essentials](#BKMK_MapNewHosttoServerAddress)  
  
-   [Étape 4 : créer un groupe de sécurité pour les ordinateurs clients DirectAccess](#BKMK_AddSecurityGroup)  
  
-   [Étape 5 : activer et configurer DirectAccess](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableConfigureDA)  
  
    -   [Étape 5A : activer DirectAccess à l’aide de la console de gestion de l’accès à distance](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableDA)  
  
    -   [Étape 5B : supprimer le IPv6Prefix non valide dans l’objet de stratégie de groupe RRAS (Windows Server Essentials uniquement)](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_RemoveIPv6)  
  
    -   [Étape 5c : autoriser les ordinateurs clients exécutant Windows 7 entreprise à utiliser DirectAccess](#BKMK_Step4cWindows7Setup)  
  
    -   [Étape 5D : configurer le serveur emplacement réseau](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NLS)  
  
    -   [Étape 5e : ajouter une clé de Registre pour contourner la certification de l’autorité de certification lorsque vous établissez un canal IPsec](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_CA)  
  
-   [Étape 6 : configurer les paramètres de la table de stratégie de résolution de noms pour le serveur DirectAccess](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NRPT)  
  
-   [Étape 7 : configurer les règles de pare-feu TCP et UDP pour les objets de stratégie de groupe du serveur DirectAccess](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_TCPUDP)  
  
-   [Étape 8 : modifier la configuration DNS64 pour écouter l’interface IP-HTTPs](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS64)  
  
-   [Étape 9 : Réserver des ports pour le service Winnat](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_ExemptPort)  
  
-   [Étape 10 : redémarrer le service Winnat](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_WinNAT)  
  
> [!NOTE]
>  [Annexe : Installer DirectAccess à l’aide de Windows PowerShell](#BKMK_AppendixBPowerShellScript) fournit un script Windows PowerShell que vous pouvez utiliser pour configurer DirectAccess.  
  
##  <a name="step-1-add-remote-access-management-tools-to-your-server"></a><a name="BKMK_AddRAM"></a>Étape 1 : ajouter les outils de gestion de l’accès à distance à votre serveur  
  
#### <a name="to-add-remote-accregss-management-tools--reg"></a>Pour ajouter les outils d’administration Remote&reg;SS &reg;
  
1.  Sur le serveur, dans l'angle inférieur gauche de la page de démarrage, cliquez sur l’icône **Gestionnaire de serveur**.  
  
     Dans Windows Server Essentials, vous devrez Rechercher Gestionnaire de serveur pour l’ouvrir. Dans la page de démarrage, tapez **Server Manager**, puis cliquez sur **Gestionnaire de serveur** dans les résultats de la recherche. Pour épingler le Gestionnaire de serveur à la page de démarrage, cliquez avec le bouton droit sur le Gestionnaire de serveur dans les résultats de la recherche, puis cliquez sur **Épingler à l'écran d'accueil**.  
  
2.  Si un message d’avertissement **Contrôle de compte d’utilisateur** s’affiche, cliquez sur **Oui**.  
  
3.  Dans le tableau de bord du Gestionnaire de serveur, cliquez sur **Gérer**, puis sur **Ajouter des rôles et des fonctionnalités**.  
  
4.  Dans l’Assistant Ajout de rôles et de fonctionnalités, procédez comme suit :  
  
    1.  Dans la page **Type d’installation**, cliquez sur **Installation basée sur un rôle ou une fonctionnalité**.  
  
    2.  Sur la **page sélection du serveur** (ou la page **Sélectionner le serveur de destination** dans Windows Server Essentials), cliquez sur **Sélectionner un serveur du pool de serveurs**.  
  
    3.  Dans la page **Fonctionnalités**, développez successivement **Outils d'administration de serveur distant (installés)** , **Outils de gestion de l'accès à distance (installés)** , **Outils d'administration de rôles (installés)** et **Outils de gestion de l'accès à distance**, puis sélectionnez **Interface GUI de l'accès à distance et outils en ligne de commande**.  
  
    4.  Suivez les instructions pour exécuter l'Assistant.  
  
##  <a name="step-2-change-the-network-adapter-address-of-the-server-to-a-static-ip-address"></a><a name="BKMK_AddStaticIP"></a>Étape 2 : remplacer l’adresse de la carte réseau du serveur par une adresse IP statique  
 DirectAccess requiert une carte avec une adresse IP statique. Vous devez modifier l’adresse IP de la carte réseau local sur votre serveur.  
  
#### <a name="to-add-a-static-ip-address"></a>Pour ajouter une adresse IP statique  
  
1.  Dans la page de démarrage, ouvrez le **Panneau de configuration**.  
  
2.  Cliquez sur **Réseau et Internet**, puis sur **Afficher l’état et la gestion du réseau**.  
  
3.  Dans le volet des tâches du **Centre Réseau et partage**, cliquez sur **Modifier les paramètres de la carte**.  
  
4.  Cliquez avec le bouton droit sur la carte réseau local, puis cliquez sur **Propriétés**.  
  
5.  Sous l’onglet **Réseau**, cliquez sur **Protocole Internet version 4 (TCP/IPv4)** , puis sur **Propriétés**.  
  
6.  Sous l’onglet **Général**, cliquez sur **Utiliser l’adresse IP suivante**, puis tapez l’adresse IP à utiliser.  
  
     Une valeur par défaut pour le masque de sous-réseau s’affiche automatiquement dans la zone **Masque de sous-réseau**. Acceptez la valeur par défaut, ou tapez la valeur de masque de sous-réseau que vous voulez utiliser.  
  
7.  Dans la zone **Passerelle par défaut**, tapez l’adresse IP de votre passerelle par défaut.  
  
8.  Dans la zone **Serveur DNS préféré**, tapez l’adresse IP de votre serveur DNS.  
  
    > [!NOTE]
    >  Utilisez l'adresse IP attribuée à votre carte réseau par DHCP (par exemple, 192.168.X.X) au lieu d'un réseau de bouclage (par exemple,127.0.0.1). Pour déterminer l'adresse IP attribuée, exécutez **ipconfig** à une invite de commandes.  
  
9. Dans la zone **Serveur DNS auxiliaire**, tapez l’adresse IP de votre serveur DNS auxiliaire, le cas échéant.  
  
10. Cliquez sur **OK**, puis sur **Fermer**.  
  
> [!IMPORTANT]
>  Assurez-vous de configurer le routeur pour réacheminer les ports 80 et 443 vers la nouvelle adresse IP statique du serveur.  
  
##  <a name="step-3-prepare-a-certificate-and-dns-record-for-the-network-location-server"></a><a name="BKMK_DNS"></a>Étape 3 : préparer un certificat et un enregistrement DNS pour le serveur emplacement réseau  
 Pour préparer un certificat et un enregistrement DNS pour le serveur Emplacement réseau, effectuez les tâches suivantes :  
  
-   [Étape 3a : accorder les autorisations maximales aux utilisateurs authentifiés pour le modèle de certificat du serveur Web](#BKMK_GrantFullPermissions)  
  
-   [Étape 3b : inscrire un certificat pour le serveur emplacement réseau avec un nom commun qui ne peut pas être résolu à partir du réseau externe](#BKMK_EnrollaCertificate)  
  
-   [Étape 3c : ajoutez un nouvel hôte sur le serveur DNS et mappez-le à l’adresse du serveur Windows Server Essentials.](#BKMK_MapNewHosttoServerAddress)  
  
###  <a name="step-3a-grant-full-permissions-to-authenticated-users-for-the-web-servers-certificate-template"></a><a name="BKMK_GrantFullPermissions"></a>Étape 3a : accorder les autorisations maximales aux utilisateurs authentifiés pour le modèle de certificat du serveur Web  
 Votre première tâche consiste à accorder des autorisations complètes pour authentifier les utilisateurs pour le modèle de certificat du serveur Web dans l’autorité de certification.  
  
####  <a name="to-grant-full-permissions-to-authenticated-users-for-the-web-servers-certificate-template"></a><a name="BKMK_ToGrantFullPermissions"></a>Pour accorder des autorisations complètes aux utilisateurs authentifiés pour le modèle de certificat du serveur Web  
  
1.  Dans la page de **démarrage** page, ouvrez **Autorité de certification**.  
  
2.  Dans l’arborescence de la console, sous **autorité de certification (local)** , développez **< servername\>-ca**, cliquez avec le bouton droit sur **modèles de certificats**, puis cliquez sur **gérer**.  
  
3.  Dans **Autorité de certification (local)** , cliquez avec le bouton droit sur **Serveur web**, puis cliquez sur **Propriétés**.  
  
4.  Dans les propriétés du serveur web, sous l'onglet **Sécurité**, cliquez sur **Utilisateurs authentifiés**, sélectionnez **Contrôle total**, puis cliquez sur **OK**.  
  
5.  Redémarrez les **Services de certificats Active Directory**. Dans le panneau de configuration, ouvrez **Afficher les services locaux**. Dans la liste des services, cliquez avec le bouton droit sur **Services de certificats Active Directory**, puis cliquez sur **Redémarrer**.  
  
###  <a name="step-3b-enroll-a-certificate-for-the-network-location-server-with-a-common-name-that-is-unresolvable-from-the-external-network"></a><a name="BKMK_EnrollaCertificate"></a>Étape 3b : inscrire un certificat pour le serveur emplacement réseau avec un nom commun qui ne peut pas être résolu à partir du réseau externe  
 Inscrivez ensuite un certificat pour le serveur Emplacement réseau avec un nom commun qui ne peut pas être résolu à partir du réseau externe.  
  
####  <a name="to-enroll-a-certificate-for-the-network-location-server"></a><a name="BKMK_ToEnrollaCertificate"></a>Pour inscrire un certificat pour le serveur emplacement réseau  
  
1.  Dans la page de **démarrage**, ouvrez **mmc** (Microsoft Management Console).  
  
2.  Si un message d’avertissement **Contrôle de compte d’utilisateur** s’affiche, cliquez sur **Oui**.  
  
     La console MMC s'affiche.  
  
3.  Dans le menu **Fichier**, cliquez sur **Ajouter ou supprimer des composants logiciels enfichables**.  
  
4.  Dans la zone **Ajouter ou supprimer des composants logiciels enfichables**, cliquez sur **Certificats**, puis sur **Ajouter**.  
  
5.  Dans la page **Composant logiciel enfichable Certificats**, cliquez sur **Un compte d’ordinateur**, puis sur **Suivant**.  
  
6.  Dans la page **Sélectionner un ordinateur**, cliquez sur **Ordinateur local**, sur **Terminer**, puis sur **OK**.  
  
7.  Dans l'arborescence de la console, développez **Certificats (ordinateur local)** , développez **Personnel**, cliquez avec le bouton droit sur **Certificats**. Ensuite, dans **Toutes les tâches**, cliquez sur **Demander un nouveau certificat**.  
  
8.  Quand l'Assistant Inscription de certificats s'affiche, cliquez sur **Suivant**.  
  
9. Dans la page **Sélectionner la stratégie d'inscription de certificat**, cliquez sur **Suivant**.  
  
10. Dans la page **Demander des certificats**, cochez la case **Serveur web**, puis cliquez sur **L'inscription pour obtenir ce certificat nécessite des informations supplémentaires**.  
  
11. Dans la zone **Propriétés du certificat**, entrez les paramètres suivants pour **Nom du sujet** :  
  
    1.  Pour **Type**, sélectionnez **Nom commun**.  
  
    2.  Pour **Valeur**, tapez le nom du serveur Emplacement réseau (par exemple, DirectAccess-NLS.contoso.local), puis cliquez sur **Ajouter**.  
  
    3.  Cliquez sur **OK**, puis sur **Inscrire**.  
  
12. Une fois l'inscription du certificat terminée, cliquez sur **Terminer**.  
  
###  <a name="step-3c-add-a-new-host-on-the-dns-server-and-map-it-to-the-windows-server-essentials-server-address"></a><a name="BKMK_MapNewHosttoServerAddress"></a>Étape 3c : ajouter un nouvel hôte sur le serveur DNS et le mapper à l’adresse du serveur Windows Server Essentials  
 Pour terminer la configuration DNS, ajoutez un nouvel hôte sur le serveur DNS et mappez-le à l’adresse du serveur Windows Server Essentials.  
  
####  <a name="to-map-a-new-host-to-the-windows-server-essentials-server-address"></a><a name="BKMK_ToMapNewHosttoServerAddress"></a>Pour mapper un nouvel hôte à l’adresse du serveur Windows Server Essentials  
  
1.  Dans la page de démarrage, ouvrez le Gestionnaire DNS. Pour ouvrir le Gestionnaire DNS, recherchez **dnsmgmt.msc**, puis cliquez sur **dnsmgmt.msc** dans les résultats.  
  
2.  Dans l’arborescence de la console du Gestionnaire DNS, développez le serveur local, développez **zones de recherche directes**, cliquez avec le bouton droit sur la zone avec le suffixe de domaine du serveur, puis cliquez sur **nouvel hôte (A ou AAAA)** .  
  
3.  Tapez le nom et l’adresse IP du serveur (par exemple, DirectAccess-NLS.contoso.local) et son adresse de serveur correspondante (par exemple, 192.168.x.x).  
  
4.  Cliquez sur **Ajouter un hôte**, sur **OK**, puis sur **Terminé**.  
  
##  <a name="step-4-create-a-security-group-for-directaccess-client-computers"></a><a name="BKMK_AddSecurityGroup"></a>Étape 4 : créer un groupe de sécurité pour les ordinateurs clients DirectAccess  
 Ensuite, créez un groupe de sécurité à utiliser pour les ordinateurs clients DirectAccess et ajoutez les comptes d'ordinateur au groupe.  
  
#### <a name="to-add-a-security-group-for-client-computers-that-use-directaccess"></a>Pour ajouter un groupe de sécurité aux ordinateurs clients qui utilisent DirectAccess  
  
1. Dans le tableau de bord du Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Utilisateurs et ordinateurs Active Directory**.  
  
   > [!NOTE]
   >  Si **Utilisateurs et ordinateurs Active Directory** n'apparaît pas dans le menu **Outils**, vous devez installer la fonctionnalité. Pour installer Utilisateurs et ordinateurs Active Directory, exécutez la cmdlet Windows PowerShell suivante en tant qu’administrateur : `Install-WindowsFeature RSAT-ADDS-Tools`. Pour plus d’informations, consultez la rubrique [Installation ou suppression du Pack des outils d’administration de serveur distant](https://technet.microsoft.com/library/cc730825.aspx).  
  
2. Dans l'arborescence de la console, développez le serveur, cliquez avec le bouton droit sur **Utilisateurs**, cliquez sur **Nouveau**, puis cliquez sur **Groupe**.  
  
3. Entrez un nom de groupe, une étendue de groupe et un type de groupe (créer un groupe de sécurité), puis cliquez sur **OK**.  
  
   Le nouveau groupe de sécurité est ajouté au dossier **Utilisateurs**.  
  
#### <a name="to-add-computer-accounts-to-the-security-group"></a>Pour ajouter des comptes d'ordinateur au groupe de sécurité  
  
1.  Dans le tableau de bord du Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Utilisateurs et ordinateurs Active Directory**.  
  
2.  Dans l'arborescence de la console, développez le serveur, puis cliquez sur **Utilisateurs**.  
  
3.  Dans la liste des comptes d'utilisateur et des groupes de sécurité sur le serveur, cliquez avec le bouton droit sur le groupe de sécurité que vous avez créé pour DirectAccess, puis cliquez sur **Propriétés**.  
  
4.  Sous l'onglet **Membres**, cliquez sur **Ajouter**.  
  
5.  Dans la boîte de dialogue, tapez les noms des comptes d'ordinateur à ajouter au groupe, en les séparant par un point-virgule (;). Cliquez ensuite sur **Vérifier les noms**.  
  
6.  Une fois les comptes d'ordinateur validés, cliquez sur **OK**. Cliquez à nouveau sur **OK**.  
  
> [!NOTE]
>  Vous pouvez également utiliser l'onglet **Membre de** dans les propriétés du compte d'ordinateur pour ajouter le compte au groupe de sécurité.  
  
##  <a name="step-5-enable-and-configure-directaccess"></a><a name="BKMK_EnableConfigureDA"></a>Étape 5 : activer et configurer DirectAccess  
 Pour activer et configurer DirectAccess dans Windows Server Essentials, vous devez effectuer les étapes suivantes :  
  
-   [Étape 5A : activer DirectAccess à l’aide de la console de gestion de l’accès à distance](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableDA)  
  
-   [Étape 5B : supprimer le IPv6Prefix non valide dans l’objet de stratégie de groupe RRAS (Windows Server Essentials uniquement)](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_RemoveIPv6)  
  
-   [Étape 5c : autoriser les ordinateurs clients exécutant Windows 7 entreprise à utiliser DirectAccess](#BKMK_Step4cWindows7Setup)  
  
-   [Étape 5D : configurer le serveur emplacement réseau](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NLS)  
  
-   [Étape 5e : ajouter une clé de Registre pour contourner la certification de l’autorité de certification lorsque vous établissez un canal IPsec](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_CA)  
  
###  <a name="step-5a-enable-directaccess-by-using-the-remote-access-management-console"></a><a name="BKMK_EnableDA"></a>Étape 5A : activer DirectAccess à l’aide de la console de gestion de l’accès à distance  
 Cette section fournit des instructions pas à pas pour activer DirectAccess dans Windows Server Essentials. Si vous n'avez pas encore configuré VPN sur le serveur, vous devez le faire avant de commencer cette procédure. Pour obtenir des instructions, consultez [gérer le VPN](Manage-VPN-in-Windows-Server-Essentials.md).  
  
##### <a name="to-enable-directaccess-by-using-the-remote-access-management-console"></a>Pour activer DirectAccess à l'aide de la console de gestion de l'accès à distance  
  
1.  Dans la page de démarrage, ouvrez **Gestion de l'accès à distance**.  
  
2.  Dans l’Assistant Activation de DirectAccess, procédez comme suit :  
  
    1.  Passez en revue **Conditions requises pour DirectAccess**, puis cliquez sur **Suivant**.  
  
    2.  Sous l'onglet **Sélectionner des groupes**, ajoutez le groupe de sécurité créé précédemment pour les clients DirectAccess. (Si vous n’avez pas créé le groupe de sécurité, consultez [étape 4 : créer un groupe de sécurité pour les ordinateurs clients DirectAccess](#BKMK_AddSecurityGroup) pour obtenir des instructions.)  
  
    3.  Sous l’onglet **Sélectionner des groupes**, cliquez sur **Activer DirectAccess pour les ordinateurs portables uniquement** si vous voulez permettre aux ordinateurs portables d’utiliser DirectAccess pour accéder à distance au serveur, puis cliquez sur **Suivant**.  
  
    4.  Dans **Topologie du réseau**, sélectionnez la topologie du serveur, puis cliquez sur **Suivant**.  
  
    5.  Dans **Liste de recherche de suffixes DNS**, ajoutez le suffixe DNS supplémentaire pour les ordinateurs clients, si nécessaire, puis cliquez sur **Suivant**.  
  
        > [!NOTE]
        >  Par défaut, l'Assistant Activation de DirectAccess ajoute déjà le suffixe DNS au domaine actuel. Toutefois, vous pouvez en ajouter d’autres si nécessaire.  
  
    6.  Examinez les objets de stratégie de groupe qui seront appliqués et modifiez-les si nécessaire.  
  
    7.  Cliquez sur **Suivant**, puis sur **Terminer**.  
  
    8.  Redémarrez le service de gestion de l'accès à distance en exécutant la commande Windows PowerShell suivante avec élévation de privilèges :  
  
        ```powershell  
        Restart-Service RaMgmtSvc   
        ```  
  
###  <a name="step-5b-remove-the-invalid-ipv6prefix-in-rras-gpo-windows-server-essentials-only"></a><a name="BKMK_RemoveIPv6"></a>Étape 5B : supprimer le IPv6Prefix non valide dans l’objet de stratégie de groupe RRAS (Windows Server Essentials uniquement)  
  Cette section s’applique à un serveur exécutant Windows Server Essentials.  
  
 Ouvrez Windows PowerShell en tant qu’administrateur, puis exécutez les commandes suivantes :  
  
```powershell  
gpupdate  
$key = Get-ChildItem -Path HKLM:\SOFTWARE\Policies\Microsoft\Windows\RemoteAccess\config\MachineSIDs | Where-Object{$_.GetValue("IPv6RrasPrefix") -ne $null}  
Remove-GPRegistryValue -Name "DirectAccess Server Settings" -Key $key.Name -ValueName IPv6RrasPrefix  
gpupdate  
```  
  
###  <a name="step-5c-enable-client-computers-running-windows-7-enterprise-to-use-directaccess"></a><a name="BKMK_Step4cWindows7Setup"></a>Étape 5c : autoriser les ordinateurs clients exécutant Windows 7 entreprise à utiliser DirectAccess  
 Si vous avez des ordinateurs clients qui exécutent Windows 7 entreprise, effectuez la procédure suivante pour activer DirectAccess à partir de ces ordinateurs.  
  
##### <a name="to-enable--windows-7-enterprise-computers-to-use-directaccess"></a>Pour permettre aux ordinateurs Windows 7 entreprise d’utiliser DirectAccess  
  
1.  Sur la page de démarrage du serveur, ouvrez **gestion de l’accès à distance**.  
  
2.  Dans la Console de gestion de l'accès à distance, cliquez sur **Configuration**. Ensuite, dans le volet **Détails de la configuration**, sous **Étape 2**, cliquez sur **Modifier**.  
  
     L'Assistant Installation du serveur d'accès à distance s'ouvre.  
  
3.  Sous l’onglet **authentification** , choisissez le certificat de l’autorité de certification qui sera le certificat racine approuvé (vous pouvez choisir le certificat de l’autorité de certification du serveur Windows Server Essentials). Cliquez sur **Autoriser les ordinateurs clients Windows 7 à se connecter par le biais de DirectAccess**, puis sur **suivant**.  
  
4.  Suivez les instructions pour exécuter l'Assistant.  
  
> [!IMPORTANT]
>  Il existe un problème connu pour les ordinateurs Windows 7 entreprise se connectant via DirectAccess si le serveur Windows Server Essentials n’a pas été fourni avec UR1 préinstallé. Pour activer les connexions DirectAccess dans cet environnement, vous devez effectuer les étapes supplémentaires suivantes :  
> 
> 1. Installez le correctif logiciel décrit dans [l’article 2796394 de la base de connaissances Microsoft](https://support.microsoft.com/kb/2796394) sur le serveur Windows Server Essentials. Redémarrez le serveur.  
>    2. Installez ensuite le correctif logiciel décrit dans [l’article 2615847 de la base de connaissances Microsoft](https://support.microsoft.com/kb/2615847) sur chaque ordinateur Windows 7.  
> 
>    Ce problème a été résolu dans Windows Server Essentials.  
  
###  <a name="step-5d-configure-the-network-location-server"></a><a name="BKMK_NLS"></a>Étape 5D : configurer le serveur emplacement réseau  
 Cette section fournit des instructions pas à pas pour configurer les paramètres du serveur Emplacement réseau.  
  
> [!NOTE]
>  Avant de commencer, copiez le contenu du dossier < lecteur_système\>\inetpub\wwwroot dans le dossier < SystemDrive\>\Program Files\Windows Server\bin\webapps\site\insideoutside. Copiez également le fichier default. aspx du dossier < SystemDrive\>\Program Files\Windows Server\Bin\WebApps\Site dans le dossier < SystemDrive\>\Program Files\Windows Server\bin\webapps\site\insideoutside.  
  
##### <a name="to-configure-the-network-location-server"></a>Pour configurer le serveur Emplacement réseau  
  
1.  Dans la page de démarrage, ouvrez **Gestion de l'accès à distance**.  
  
2.  Dans la console Gestion de l'accès à distance, cliquez sur **Configuration**, puis dans le volet d'informations **Configuration de l'accès à distance**, dans **Étape 3**, cliquez sur **Modifier**.  
  
3.  Dans l’Assistant Installation du serveur d’accès à distance, sous l’onglet **Server Emplacement réseau** , sélectionnez **Le serveur Emplacement réseau est déployé sur le serveur d’accès à distance**, puis sélectionnez le certificat émis précédemment (dans [Step 3: Prepare a certificate and DNS record for the network location server](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS)).  
  
4.  Suivez les instructions pour exécuter l’Assistant, puis cliquez sur **Terminer**.  
  
###  <a name="step-5e-add-a-registry-key-to-bypass-ca-certification-when-you-establish-an-ipsec-channel"></a><a name="BKMK_CA"></a>Étape 5e : ajouter une clé de Registre pour contourner la certification de l’autorité de certification lorsque vous établissez un canal IPsec  
 L'étape suivante consiste à configurer le serveur pour contourner l'autorité de certification quand un canal IPsec est établi.  
  
##### <a name="to-add-a-registry-key-to-bypass-the-ca-certification"></a>Pour ajouter une clé de Registre pour contourner l'autorité de certification  
  
1.  Dans la page de démarrage, ouvrez **regedit** (Éditeur du Registre).  
  
2.  Dans l'Éditeur du Registre, développez successivement **HKEY_LOCAL_MACHINE**, **Système**, **CurrentControlSet**, **Services**, puis **IKEEXT**.  
  
3.  Sous **IKEEXT**, cliquez avec le bouton droit sur **Paramètres**, cliquez sur **Nouveau**, puis cliquez sur **Valeur DWORD 32 bits**.  
  
4.  Renommez la valeur nouvellement ajoutée **ikeflags**.  
  
5.  Double-cliquez sur **ikeflags**, définissez le **Hexadécimal** comme **Type**, définissez la valeur sur **8000**, puis cliquez sur **OK**.  
  
> [!NOTE]
>  Vous pouvez utiliser la commande Windows PowerShell suivante avec élévation de privilèges pour ajouter cette clé de Registre :  
>   
>  `Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\IKEEXT\Parameters -Name ikeflags -Type DWORD -Value 0x8000`  
  
##  <a name="step-6-configure-name-resolution-policy-table-settings-for-the-directaccess-server"></a><a name="BKMK_NRPT"></a>Étape 6 : configurer les paramètres de la table de stratégie de résolution de noms pour le serveur DirectAccess  
 Cette section fournit des instructions pour modifier les entrées de la table de stratégie de résolution des noms pour les adresses internes (par exemple, celles avec un suffixe contoso.local) pour les objets de stratégie de groupe de clients DirectAccess, puis définir l'adresse de l'interface IP-HTTPS.  
  
#### <a name="to-configure-name-resolution-policy-table-entries"></a>Pour configurer les entrées de la table de stratégie de résolution des noms  
  
1.  Dans la page de démarrage, ouvrez **Gestion des stratégies de groupe**.  
  
2.  Dans la console Gestion des stratégies de groupe, cliquez sur la forêt et le domaine par défaut, cliquez avec le bouton droit sur **Paramètres du client DirectAccess**, puis cliquez sur **Modifier**.  
  
3.  Cliquez sur **Configuration ordinateur**, sur **Stratégies**, sur **Paramètres Windows**, puis sur **Stratégie de résolution de noms**. Choisissez l’entrée dont l’espace de noms est identique à votre suffixe DNS, puis cliquez sur **Modifier la règle**.  
  
4.  Cliquez sur l'onglet **Paramètres DNS pour DirectAccess**, puis sélectionnez **Activer les paramètres DNS pour DirectAccess dans cette règle**. Ajoutez l’adresse IPv6 pour l’interface IP-HTTPS dans la liste de serveurs DNS.  
  
    > [!NOTE]
    >  Vous pouvez utiliser la commande Windows PowerShell suivante pour obtenir l'adresse IPv6 :  
    >   
    >  `(Get-NetIPInterface -InterfaceAlias IPHTTPSInterface | Get-NetIPAddress -PrefixLength 128)[1].IPAddress`  
  
##  <a name="step-7-configure-tcp-and-udp-firewall-rules-for-the-directaccess-server-gpos"></a><a name="BKMK_TCPUDP"></a>Étape 7 : configurer les règles de pare-feu TCP et UDP pour les objets de stratégie de groupe du serveur DirectAccess  
 Cette section inclut des instructions pas à pas pour configurer des règles de pare-feu TCP et UDP pour les objets de stratégie de groupe du serveur DirectAccess.  
  
#### <a name="to-configure-firewall-rules"></a>Pour configurer des règles de pare-feu  
  
1.  Dans la page de démarrage, ouvrez **Gestion des stratégies de groupe**.  
  
2.  Dans la console Gestion des stratégies de groupe, cliquez sur la forêt et le domaine par défaut, cliquez avec le bouton droit sur **Paramètres du serveur DirectAccess**, puis cliquez sur **Modifier**.  
  
3.  Cliquez sur **Configuration ordinateur**, sur **Stratégies**, sur **Paramètres Windows**, sur **Paramètres de sécurité**, sur **Pare-feu Windows avec fonctions avancées de sécurité**, sur **Pare-feu Windows avec fonctions avancées de sécurité** (niveau suivant), puis sur **Règles de trafic entrant**. Cliquez avec le bouton droit sur **Serveur de noms de domaine (TCP-Entrant)** , puis cliquez sur **Propriétés**.  
  
4.  Cliquez sur l’onglet **Étendue** et, dans la liste **Adresse IP locale**, ajoutez l’adresse IPv6 de l’interface IP-HTTPS.  
  
5.  Répétez la même procédure pour **Serveur de noms de domaine (UDP-Entrant)** .  
  
##  <a name="step-8-change-the-dns64-configuration-to-listen-to-the-ip-https-interface"></a><a name="BKMK_DNS64"></a>Étape 8 : modifier la configuration DNS64 pour écouter l’interface IP-HTTPs  
 Vous devez modifier la configuration DNS64 pour écouter l’interface IP-HTTPS à l’aide de la commande Windows PowerShell suivante.  
  
```powershell  
Set-NetDnsTransitionConfiguration -AcceptInterface IPHTTPSInterface  
```  
  
##  <a name="step-9-reserve-ports-for-the-winnat-service"></a><a name="BKMK_ExemptPort"></a>Étape 9 : Réserver des ports pour le service Winnat  
 Utilisez la commande Windows PowerShell suivante pour réserver des ports pour le service WinNat. Remplacez « 192.168.1.100 » par l’adresse IPv4 réelle de votre serveur Windows Server Essentials.  
  
```powershell  
Set-NetNatTransitionConfiguration -IPv4AddressPortPool @("192.168.1.100, 10000-47000")  
```  
  
> [!IMPORTANT]
>  Pour éviter les conflits entre ports et applications, assurez-vous que la plage de ports que vous réservez pour le service WinNat n'inclut pas le port 6602.  
  
##  <a name="step-10-restart-the-winnat-service"></a><a name="BKMK_WinNAT"></a>Étape 10 : redémarrer le service Winnat  
 Redémarrez le service Pilote NAT Windows (WinNat) à l'aide de la commande Windows PowerShell suivante.  
  
```powershell  
Restart-Service winnat  
```  
  
##  <a name="appendix-set-up-directaccess-by-using-windows-powershell"></a><a name="BKMK_AppendixBPowerShellScript"></a>Annexe : configurer DirectAccess à l’aide de Windows PowerShell  
 Cette section décrit comment installer et configurer DirectAccess à l’aide de Windows PowerShell.  
  
### <a name="preparation"></a>Préparation  
 Avant de commencer la configuration de votre serveur pour DirectAccess, vous devez procéder comme suit :  
  
1.  Suivez la procédure décrite à l' [étape 3 : préparer un certificat et un enregistrement DNS pour le serveur emplacement réseau](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS) pour inscrire un certificat nommé **DirectAccess-NLS.contoso.com** (où **contoso.com** est remplacé par votre nom de domaine interne réel) et pour ajouter un enregistrement DNS pour le serveur emplacement réseau (nls).  
  
2.  Ajoutez un groupe de sécurité nommé **DirectAccessClients** dans Active Directory, puis ajoutez les ordinateurs clients pour lesquels vous voulez fournir les fonctionnalités DirectAccess. Pour plus d’informations, consultez [étape 4 : créer un groupe de sécurité pour les ordinateurs clients DirectAccess](#BKMK_AddSecurityGroup).  
  
### <a name="commands"></a>Commands  
  
> [!IMPORTANT]
>  Dans Windows Server Essentials, vous n’avez pas besoin de supprimer l’objet de stratégie de groupe de préfixe IPv6 inutile. Supprimez la section de code avec l’intitulé suivant : `# [WINDOWS SERVER 2012 ESSENTIALS ONLY] Remove the unnecessary IPv6 prefix GPO`.  
  
```powershell  
# Add Remote Access role if not installed yet  
$ra = Get-WindowsFeature RemoteAccess  
If ($ra.Installed -eq $FALSE) { Add-WindowsFeature RemoteAccess }  
  
# Server may need to restart if you installed RemoteAccess role in the above step  
  
# Set the internet domain name to access server, replace contoso.com below with your own domain name  
$InternetDomain = "www.contoso.com"  
#Set the SG name which you create for DA clients  
$DaSecurityGroup = "DirectAccessClients"  
#Set the internal domain name  
$InternalDomain = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().Name  
  
# Set static IP and DNS settings  
$NetConfig = Get-WmiObject Win32_NetworkAdapterConfiguration -Filter "IPEnabled=$true"  
$CurrentIP = $NetConfig.IPAddress[0]  
$SubnetMask = $NetConfig.IPSubnet | Where-Object{$_ -like "*.*.*.*"}  
$NetConfig.EnableStatic($CurrentIP, $SubnetMask)  
$NetConfig.SetGateways($NetConfig.DefaultIPGateway)  
$NetConfig.SetDNSServerSearchOrder($CurrentIP)  
  
# Get physical adapter name and the certificate for NLS server  
$Adapter = (Get-WmiObject -Class Win32_NetworkAdapter -Filter "NetEnabled=$true").NetConnectionId  
$Certs = dir cert:\LocalMachine\My  
$nlscert = $certs | Where-Object{$_.Subject -like "*CN=DirectAccess-NLS*"}  
  
# Add regkey to bypass CA cert for IPsec authentication  
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\IKEEXT\Parameters -Name ikeflags -Type DWORD -Value 0x8000  
  
# Install DirectAccess.   
Install-RemoteAccess -NoPrerequisite -DAInstallType FullInstall -InternetInterface $Adapter -InternalInterface $Adapter -ConnectToAddress $InternetDomain -nlscertificate $nlscert -force  
  
#Restart Remote Access Management service  
Restart-Service RaMgmtSvc  
  
# [WINDOWS SERVER 2012 ESSENTIALS ONLY] Remove the unnecessary IPv6 prefix GPO   
  
gpupdate  
$key = Get-ChildItem -Path HKLM:\SOFTWARE\Policies\Microsoft\Windows\RemoteAccess\config\MachineSIDs | Where-Object{$_.GetValue("IPv6RrasPrefix") -ne $null}  
Remove-GPRegistryValue -Name "DirectAccess Server Settings" -Key $key.Name -ValueName IPv6RrasPrefix  
gpupdate  
  
# Enable client computers running Windows 7 to use DirectAccess  
$allcertsinroot = dir cert:\LocalMachine\root  
$rootcert = $allcertsinroot | Where-Object{$_.Subject -like "*-CAA*"}  
Set-DAServer -IPSecRootCertificate $rootcert[0]  
Set -DAClient -OnlyRemoteComputers Disabled -Downlevel Enabled  
  
#Set the appropriate security group used for DA client computers. Replace the group name below with the one you created for DA clients  
Add-DAClient -SecurityGroupNameList $DaSecurityGroup   
Remove-DAClient -SecurityGroupNameList "Domain Computers"  
  
# Gather DNS64 IP address information  
$Remoteaccess = get-remoteaccess  
$IPinterface = get-netipinterface -InterfaceAlias IPHTTPSInterface | get-netipaddress -PrefixLength 128  
$DNS64IP=$IPInterface[1].IPaddress  
$Natconfig = Get-NetNatTransitionConfiguration  
  
# Configure TCP and UDP firewall rules for the DirectAccess server GPO  
$GpoName = 'GPO:'+$InternalDomain+'\DirectAccess Server Settings'  
Get-NetFirewallRule -PolicyStore $GpoName -Displayname "Domain Name Server (TCP-IN)"|Get-NetFirewallAddressFilter | Set-NetFirewallAddressFilter -LocalAddress $DNS64IP  
Get-NetFirewallrule -PolicyStore $GpoName -Displayname "Domain Name Server (UDP-IN)"|Get-NetFirewallAddressFilter | Set-NetFirewallAddressFilter -LocalAddress $DNS64IP  
  
# Configure the name resolution policy settings for the DirectAccess server, replace the DNS suffix below with the one in your domain  
$Suffix = '.' + $InternalDomain  
set-daclientdnsconfiguration -DNSsuffix $Suffix -DNSIPAddress $DNS64IP  
  
# Change the DNS64 configuration to listen to IP-HTTPS interface  
Set-NetDnsTransitionConfiguration -AcceptInterface IPHTTPSInterface  
  
# Copy the necessary files to NLS site folder  
XCOPY 'C:\inetpub\wwwroot' 'C:\Program Files\Windows Server\Bin\WebApps\Site\insideoutside' /E /Y  
XCOPY 'C:\Program Files\Windows Server\Bin\WebApps\Site\Default.aspx' 'C:\Program Files\Windows Server\Bin\WebApps\Site\insideoutside' /Y  
  
# Reserve ports for the WinNat service  
Set-NetNatTransitionConfiguration -IPv4AddressPortPool @("192.168.1.100, 10000-47000")  
  
# Restart the WinNat service  
Restart-Service winnat  
```  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer l’accès en tout lieu](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gérer Windows Server Essentials](Manage-Windows-Server-Essentials.md)
