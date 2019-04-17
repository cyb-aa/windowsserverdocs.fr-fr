---
title: Configurer DirectAccess dans WindowsServerEssentials
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c959b6fc-c67e-46cd-a9cb-cee71a42fa4c
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cc336dcd2a5418aa79254108c941a02147112e8f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="configure-directaccess-in-windows-server-essentials"></a>Configurer DirectAccess dans WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Cette rubrique fournit des instructions pas à pas pour configurer DirectAccess dans WindowsServerEssentials pour activer votre personnel itinérant connecter de façon transparente au réseau s de votre organisation à partir de n’importe quel emplacement distant équipé d’Internet sans établir une connexion réseau privé virtuel (VPN). DirectAccess offre aux utilisateurs itinérants la même expérience de connectivité à l’intérieur et en dehors de leurs ordinateurs Windows8.1, Windows8 et Windows7.  
  
 Dans WindowsServerEssentials, si le domaine contient plusieurs serveurs WindowsServerEssentials, DirectAccess doit être configurés sur le contrôleur de domaine.  
  
> [!NOTE]
>  Cette rubrique fournit des instructions pour configurer DirectAccess lorsque votre serveur WindowsServerEssentials est le contrôleur de domaine. Si le serveur WindowsServerEssentials est membre d’un domaine, suivez les instructions pour configurer DirectAccess sur un membre du domaine dans [ajouter DirectAccess à un déploiement de l’accès à distance existants (VPN)](https://technet.microsoft.com/library/jj574220.aspx) à la place.  
  
## <a name="process-overview"></a>Vue d’ensemble du processus  
 Pour configurer DirectAccess dans WindowsServerEssentials, procédez comme suit.  
  
> [!IMPORTANT]
>  Avant d’utiliser les procédures décrites dans ce guide pour configurer DirectAccess dans WindowsServerEssentials, vous devez activer VPN sur le serveur. Pour obtenir des instructions, voir [gérer un VPN](Manage-VPN-in-Windows-Server-Essentials.md).  
  
-   [Étape1: Ajouter des outils de gestion de l’accès à distance à votre serveur](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_AddRAM)  
  
-   [Étape2: Modifiez l’adresse de la carte réseau du serveur à une adresse IP statique](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_AddStaticIP)  
  
-   [Étape3: Préparer un certificat et un enregistrement DNS pour le serveur emplacement réseau](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS)  
  
    -   [Étape3 a: accorder des autorisations maximales aux utilisateurs authentifiés pour le modèle de certificat de serveur s Web](#BKMK_GrantFullPermissions)  
  
    -   [Étape3 b: inscrire un certificat pour le serveur d’emplacement réseau avec un nom commun qui ne peut pas être résolu à partir du réseau externe](#BKMK_EnrollaCertificate)  
  
    -   [Étape3c: ajouter un nouvel hôte sur le serveur DNS et le mapper à l’adresse du serveur WindowsServerEssentials](#BKMK_MapNewHosttoServerAddress)  
  
-   [Étape4: Créer un groupe de sécurité pour les ordinateurs clients DirectAccess](#BKMK_AddSecurityGroup)  
  
-   [Étape5: Activer et configurer DirectAccess](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableConfigureDA)  
  
    -   [Étape5 a: activer DirectAccess à l’aide de la Console de gestion de l’accès à distance](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableDA)  
  
    -   [Étape5 b: supprimer les IPv6Prefix non valide dans l’objet stratégie de groupe RRAS (WindowsServerEssentials uniquement)](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_RemoveIPv6)  
  
    -   [Étape5c: autoriser les ordinateurs clients exécutant Windows7 entreprise à utiliser DirectAccess](#BKMK_Step4cWindows7Setup)  
  
    -   [Étape5D: configurer le serveur d’emplacement réseau](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NLS)  
  
    -   [Étape5e: ajouter une clé de Registre pour contourner l’autorité de certification quand vous établissez un canal IPsec](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_CA)  
  
-   [Étape6: Configurer les paramètres de Table de stratégie de résolution de noms pour le serveur DirectAccess](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NRPT)  
  
-   [Étape7: Configurer les règles de pare-feu TCP et UDP pour les objets stratégie de groupe du serveur DirectAccess](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_TCPUDP)  
  
-   [Étape8: Modifier la configuration DNS64 pour écouter l’interface IP-HTTPS](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS64)  
  
-   [Étape9: Réserver des ports pour le service WinNat](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_ExemptPort)  
  
-   [Étape10: Redémarrer le service WinNat](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_WinNAT)  
  
> [!NOTE]
>  [Annexe: Installer DirectAccess à l’aide de Windows PowerShell](#BKMK_AppendixBPowerShellScript) fournit un script Windows PowerShell que vous pouvez utiliser pour configurer DirectAccess.  
  
##  <a name="BKMK_AddRAM"></a>Étape1: Ajouter des outils de gestion de l’accès à distance à votre serveur  
  
#### <a name="to-add-remote-access-management-tools"></a>Pour ajouter les outils de gestion de l’accès à distance  
  
1.  Sur le serveur, dans l’angle inférieur gauche de la page de démarrage, cliquez sur le **le Gestionnaire de serveur** icône.  
  
     Dans WindowsServerEssentials, vous devez rechercher le Gestionnaire de serveur pour l’ouvrir. Sur la page de démarrage, tapez **le Gestionnaire de serveur**, puis cliquez sur **le Gestionnaire de serveur** dans les résultats de recherche. Pour épingler le Gestionnaire de serveur à la page de démarrage, cliquez sur Gestionnaire de serveur dans les résultats de recherche, puis cliquez sur **épingler au menu Démarrer**.  
  
2.  Si un **contrôle de compte d’utilisateur** message d’avertissement s’affiche, cliquez sur **Oui**.  
  
3.  Dans le tableau de bord du Gestionnaire de serveur, cliquez sur **gérer**, puis cliquez sur **Ajout de rôles et fonctionnalités**.  
  
4.  Dans l’ajout de rôles et fonctionnalités Assistant, procédez comme suit:  
  
    1.  Sur le **Type d’Installation**, cliquez sur **installation basée sur un rôle ou une fonctionnalité**.  
  
    2.  Sur le **page de sélection du serveur** (ou le **serveur de destination sélectionnez** page dans WindowsServerEssentials), cliquez sur **sélectionner un serveur du pool de serveurs**.  
  
    3.  Sur le **fonctionnalités** page, développez **outils d’Administration de serveur distant (installés)**, développez **outils de gestion de l’accès à distance (installés)**, développez **outils d’Administration de rôles (installés)**, développez **outils de gestion de l’accès à distance**, puis sélectionnez **interface graphique utilisateur de l’accès à distance et outils de ligne de commande**.  
  
    4.  Suivez les instructions pour terminer l’Assistant.  
  
##  <a name="BKMK_AddStaticIP"></a>Étape2: Modifiez l’adresse de la carte réseau du serveur à une adresse IP statique  
 DirectAccess requiert une carte avec une adresse IP statique. Vous devez modifier l’adresse IP de la carte réseau local sur votre serveur.  
  
#### <a name="to-add-a-static-ip-address"></a>Pour ajouter une adresse IP statique  
  
1.  Sur la page de démarrage, ouvrez **le panneau de configuration**.  
  
2.  Cliquez sur **réseau et Internet**, puis cliquez sur **afficher l’état du réseau et les tâches**.  
  
3.  Dans le volet des tâches de la **Centre réseau et partage**, cliquez sur **modifier les paramètres de carte**.  
  
4.  Avec le bouton droit de la carte réseau local, puis cliquez sur **propriétés**.  
  
5.  Sur le **réseau**, cliquez sur **Internet Protocol Version4 (TCP/IPv4)**, puis cliquez sur **propriétés**.  
  
6.  Sur le **général**, cliquez sur **utiliser l’adresse IP suivante**, puis tapez l’adresse IP que vous souhaitez utiliser.  
  
     Une valeur par défaut pour le masque de sous-réseau s’affiche automatiquement dans le **masque de sous-réseau** zone. Acceptez la valeur par défaut, ou tapez la valeur du masque de sous-réseau que vous souhaitez utiliser.  
  
7.  Dans le **passerelle par défaut**, tapez l’adresse IP de votre passerelle par défaut.  
  
8.  Dans le **serveur DNS préféré**, tapez l’adresse IP de votre serveur DNS.  
  
    > [!NOTE]
    >  Utilisez l’adresse IP qui est attribuée à votre carte réseau par DHCP (par exemple, 192.168.X.X) au lieu d’un réseau de bouclage (par exemple, 127.0.0.1). Pour déterminer l’adresse IP attribuée, exécutez **ipconfig** à une invite de commandes.  
  
9. Dans le **serveur DNS auxiliaire**, tapez l’adresse IP de votre serveur DNS auxiliaire, le cas échéant.  
  
10. Cliquez sur **OK**, puis cliquez sur **fermer**.  
  
> [!IMPORTANT]
>  Veillez à configurer le routeur pour réacheminer les ports 80 et 443vers la nouvelle adresse IP statique du serveur.  
  
##  <a name="BKMK_DNS"></a>Étape3: Préparer un certificat et un enregistrement DNS pour le serveur emplacement réseau  
 Pour préparer un certificat et un enregistrement DNS pour le serveur emplacement réseau, effectuez les tâches suivantes:  
  
-   [Étape3 a: accorder des autorisations maximales aux utilisateurs authentifiés pour le modèle de certificat de serveur s Web](#BKMK_GrantFullPermissions)  
  
-   [Étape3 b: inscrire un certificat pour le serveur d’emplacement réseau avec un nom commun qui ne peut pas être résolu à partir du réseau externe](#BKMK_EnrollaCertificate)  
  
-   [Étape3c: ajouter un nouvel hôte sur le serveur DNS et le mapper à l’adresse du serveur WindowsServerEssentials.](#BKMK_MapNewHosttoServerAddress)  
  
###  <a name="BKMK_GrantFullPermissions"></a>Étape3 a: accorder des autorisations maximales aux utilisateurs authentifiés pour le modèle de certificat de serveur s Web  
 Votre première tâche consiste à accorder des autorisations maximales aux utilisateurs authentifiés pour le modèle de certificat Web server s dans l’autorité de certification.  
  
####  <a name="BKMK_ToGrantFullPermissions"></a>Pour accorder des autorisations maximales aux utilisateurs authentifiés pour le serveur Web de modèle de certificat s  
  
1.  Sur le **Démarrer** page, ouvrez **autorité de Certification**.  
  
2.  Dans l’arborescence de la console, sous **autorité de Certification (Local)**, développez **< servername\ >-autorité de certification**, avec le bouton droit **modèles de certificats**, puis cliquez sur **gérer**.  
  
3.  Dans **autorité de Certification (Local)**, avec le bouton droit **serveur Web**, puis cliquez sur **propriétés**.  
  
4.  Dans les propriétés du serveur Web, sur le **sécurité**, cliquez sur **utilisateurs authentifiés**, sélectionnez **contrôle total**, puis cliquez sur **OK**.  
  
5.  Redémarrez **Services de certificats ActiveDirectory**. Dans le panneau de configuration, ouvrez **afficher les services locaux**. Dans la liste des services, cliquez sur **ActiveDirectory Certificate Services**, puis cliquez sur **redémarrer**.  
  
###  <a name="BKMK_EnrollaCertificate"></a>Étape3 b: inscrire un certificat pour le serveur d’emplacement réseau avec un nom commun qui ne peut pas être résolu à partir du réseau externe  
 Inscrivez ensuite un certificat pour le serveur d’emplacement réseau avec un nom commun qui ne peut pas être résolu à partir du réseau externe.  
  
####  <a name="BKMK_ToEnrollaCertificate"></a>Pour inscrire un certificat pour le serveur emplacement réseau  
  
1.  Sur le **Démarrer** page, ouvrez **mmc** (MicrosoftManagement Console).  
  
2.  Si un **contrôle de compte d’utilisateur** message d’avertissement s’affiche, cliquez sur **Oui**.  
  
     La Console MMC (MicrosoftManagement Console) s’ouvre.  
  
3.  Sur le **fichier** menu, cliquez sur **ajouter ou supprimer les composants logiciels enfichables**.  
  
4.  Dans le **ajouter ou des composants logiciels enfichables à distance**, cliquez sur **certificats**, puis cliquez sur **ajouter**.  
  
5.  Sur le **enfichable Certificats**, cliquez sur **compte d’ordinateur**, puis cliquez sur **suivant**.  
  
6.  Sur le **sélectionner un ordinateur**, cliquez sur **ordinateur Local**, cliquez sur **Terminer**, puis cliquez sur **OK**.  
  
7.  Dans l’arborescence de la console, développez **certificats (ordinateur Local)**, développez **personnel**, avec le bouton droit **certificats**, puis, dans **toutes les tâches**, cliquez sur **demander un nouveau certificat**.  
  
8.  Lorsque l’Assistant Inscription de certificats s’affiche, cliquez sur **suivant**.  
  
9. Sur le **sélectionner la stratégie d’inscription certificat**, cliquez sur **suivant**.  
  
10. Sur le **demander des certificats** page, sélectionnez le **serveur Web** case à cocher, puis cliquez sur **pour inscrire ce certificat nécessite des informations plus**.  
  
11. Dans le **propriétés du certificat**, entrez les paramètres suivants pour **nom de l’objet**:  
  
    1.  Pour **Type**, sélectionnez **nom commun**.  
  
    2.  Pour **valeur**, tapez le nom du serveur d’emplacement réseau (par exemple, DirectAccess-NLS.contoso.local), puis cliquez sur **ajouter**.  
  
    3.  Cliquez sur **OK**, puis cliquez sur **inscription**.  
  
12. Lorsque l’inscription de certificats est terminée, cliquez sur **Terminer**.  
  
###  <a name="BKMK_MapNewHosttoServerAddress"></a>Étape3c: ajouter un nouvel hôte sur le serveur DNS et le mapper à l’adresse du serveur WindowsServerEssentials  
 Pour terminer la configuration DNS, ajoutez un nouvel hôte sur le serveur DNS et le mapper à l’adresse du serveur WindowsServerEssentials.  
  
####  <a name="BKMK_ToMapNewHosttoServerAddress"></a>Pour mapper un nouvel hôte à l’adresse du serveur WindowsServerEssentials  
  
1.  Sur la page de démarrage, ouvrez le Gestionnaire DNS. Pour ouvrir le Gestionnaire DNS, recherchez **dnsmgmt.msc**, puis cliquez sur **dnsmgmt.msc** dans les résultats.  
  
2.  Dans l’arborescence de la console Gestionnaire DNS, développez le serveur local, **Zones de recherche directe**, avec le bouton droit de la zone avec un suffixe de domaine serveur s, puis cliquez sur **nouvel hôte (A ou AAAA)**.  
  
3.  Tapez le nom et l’adresse IP du serveur (par exemple, DirectAccess-NLS.contoso.local) et elle est correspondant à l’adresse du serveur (par exemple, 192.168.x.x).  
  
4.  Cliquez sur **ajouter un hôte**, cliquez sur **OK**, puis cliquez sur **fait**.  
  
##  <a name="BKMK_AddSecurityGroup"></a>Étape4: Créer un groupe de sécurité pour les ordinateurs clients DirectAccess  
 Ensuite, créer un groupe de sécurité à utiliser pour les ordinateurs clients DirectAccess et ajoutez les comptes d’ordinateur au groupe.  
  
#### <a name="to-add-a-security-group-for-client-computers-that-use-directaccess"></a>Pour ajouter un groupe de sécurité pour les ordinateurs clients qui utilisent DirectAccess  
  
1.  Dans le tableau de bord du Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **ActiveDirectory Users and Computers**.  
  
    > [!NOTE]
    >  Si vous ne voyez pas **ActiveDirectory Users and Computers** sur le **outils** menu, vous devez installer la fonctionnalité. Pour installer des utilisateurs ActiveDirectory et des groupes, exécutez l’applet de commande Windows PowerShell suivante en tant qu’administrateur:`Install-WindowsFeature RSAT-ADDS-Tools`. Pour plus d’informations, voir [l’installation ou la suppression du Pack Outils d’Administration à distance serveur](https://technet.microsoft.com/library/cc730825.aspx).  
  
2.  Dans l’arborescence de la console, développez le serveur, faites un clic droit **utilisateurs**, cliquez sur **New**, puis cliquez sur **groupe**.  
  
3.  Entrez un nom de groupe, l’étendue du groupe et type de groupe (créer un groupe de sécurité), puis cliquez sur **OK**.  
  
 Le nouveau groupe de sécurité est ajouté à la **utilisateurs** dossier.  
  
#### <a name="to-add-computer-accounts-to-the-security-group"></a>Pour ajouter des comptes d’ordinateur au groupe de sécurité  
  
1.  Dans le tableau de bord du Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **ActiveDirectory Users and Computers**.  
  
2.  Dans l’arborescence de la console, développez le serveur, puis cliquez sur **utilisateurs**.  
  
3.  Dans la liste des comptes d’utilisateurs et groupes de sécurité sur le serveur, le groupe de sécurité que vous avez créé pour DirectAccess avec le bouton droit, puis cliquez sur **propriétés**.  
  
4.  Sur le **membres**, cliquez sur **ajouter**.  
  
5.  Dans la boîte de dialogue, tapez les noms des comptes d’ordinateur que vous souhaitez ajouter au groupe, en les séparant par un point-virgule (;). Puis cliquez sur **vérifier les noms**.  
  
6.  Une fois les comptes d’ordinateur sont validés, cliquez sur **OK**. Puis cliquez sur **OK** à nouveau.  
  
> [!NOTE]
>  Vous pouvez également utiliser le **membre** onglet dans les propriétés de compte d’ordinateur pour ajouter le compte au groupe de sécurité.  
  
##  <a name="BKMK_EnableConfigureDA"></a>Étape5: Activer et configurer DirectAccess  
 Pour activer et configurer DirectAccess dans WindowsServerEssentials, vous devez effectuer les étapes suivantes:  
  
-   [Étape5 a: activer DirectAccess à l’aide de la Console de gestion de l’accès à distance](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_EnableDA)  
  
-   [Étape5 b: supprimer les IPv6Prefix non valide dans l’objet stratégie de groupe RRAS (WindowsServerEssentials uniquement)](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_RemoveIPv6)  
  
-   [Étape5c: autoriser les ordinateurs clients exécutant Windows7 entreprise à utiliser DirectAccess](#BKMK_Step4cWindows7Setup)  
  
-   [Étape5D: configurer le serveur d’emplacement réseau](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_NLS)  
  
-   [Étape5e: ajouter une clé de Registre pour contourner l’autorité de certification quand vous établissez un canal IPsec](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_CA)  
  
###  <a name="BKMK_EnableDA"></a>Étape5 a: activer DirectAccess à l’aide de la Console de gestion de l’accès à distance  
 Cette section fournit des instructions pas à pas pour activer DirectAccess dans WindowsServerEssentials. Si vous avez configuré pas encore de VPN sur le serveur, vous devez le faire avant de commencer cette procédure. Pour obtenir des instructions, voir [gérer un VPN](Manage-VPN-in-Windows-Server-Essentials.md).  
  
##### <a name="to-enable-directaccess-by-using-the-remote-access-management-console"></a>Pour activer DirectAccess à l’aide de la Console de gestion de l’accès à distance  
  
1.  Sur la page de démarrage, ouvrez **gestion de l’accès à distance**.  
  
2.  Dans l’Assistant Activation de DirectAccess, procédez comme suit:  
  
    1.  Passez en revue **les conditions préalables DirectAccess**, puis cliquez sur **suivant**.  
  
    2.  Sur le **sélectionner des groupes** onglet, ajoutez le groupe de sécurité que vous avez créé précédemment pour les clients DirectAccess. (Si vous n’avez pas créé le groupe de sécurité, voir [étape4: créer un groupe de sécurité pour le client DirectAccess ordinateurs](#BKMK_AddSecurityGroup) pour obtenir des instructions.)  
  
    3.  Sur le **sélectionner des groupes**, cliquez sur **activer DirectAccess pour les ordinateurs portables uniquement** si vous souhaitez permettre aux ordinateurs portables à utiliser DirectAccess pour accéder à distance au serveur, puis cliquez sur **suivant**.  
  
    4.  Dans **topologie de réseau**, sélectionnez la topologie du serveur, puis cliquez sur **suivant**.  
  
    5.  Dans **liste de recherche de suffixes DNS**, ajoutez le suffixe DNS supplémentaire pour les ordinateurs clients, si nécessaire, puis cliquez sur **suivant**.  
  
        > [!NOTE]
        >  Par défaut, l’Assistant Activation de DirectAccess ajoute déjà le suffixe DNS pour le domaine actuel. Toutefois, vous pouvez ajouter plusieurs si nécessaire.  
  
    6.  Passez en revue les objets de stratégie de groupe (GPO) qui seront appliqués et modifiez-les si nécessaire.  
  
    7.  Cliquez sur **suivant**, puis cliquez sur **Terminer**.  
  
    8.  Redémarrez le service de gestion de l’accès à distance en exécutant la commande Windows PowerShell suivante avec élévation de privilèges:  
  
        ```powershell  
        Restart-Service RaMgmtSvc   
        ```  
  
###  <a name="BKMK_RemoveIPv6"></a>Étape5 b: supprimer les IPv6Prefix non valide dans l’objet stratégie de groupe RRAS (WindowsServerEssentials uniquement)  
  Cette section s’applique à un serveur exécutant WindowsServerEssentials.  
  
 Ouvrez Windows PowerShell en tant qu’administrateur et exécutez les commandes suivantes:  
  
```powershell  
gpupdate  
$key = Get-ChildItem -Path HKLM:\SOFTWARE\Policies\Microsoft\Windows\RemoteAccess\config\MachineSIDs | Where-Object{$_.GetValue("IPv6RrasPrefix") -ne $null}  
Remove-GPRegistryValue -Name "DirectAccess Server Settings" -Key $key.Name -ValueName IPv6RrasPrefix  
gpupdate  
```  
  
###  <a name="BKMK_Step4cWindows7Setup"></a>Étape5c: autoriser les ordinateurs clients exécutant Windows7 entreprise à utiliser DirectAccess  
 Si vous avez des ordinateurs clients qui exécutent Windows7 entreprise, effectuez la procédure suivante pour activer DirectAccess à partir de ces ordinateurs.  
  
##### <a name="to-enable--windows-7-enterprise-computers-to-use-directaccess"></a>Pour permettre aux ordinateurs Windows7 entreprise à utiliser DirectAccess  
  
1.  Sur la page de démarrage serveur s, ouvrez **gestion de l’accès à distance**.  
  
2.  Dans la Console de gestion de l’accès à distance, cliquez sur **Configuration**. Ensuite, dans le **détails de la configuration** volet, sous **étape2**, cliquez sur **modifier**.  
  
     Assistant d’installation du serveur d’accès à distance s’ouvre.  
  
3.  Sur le **authentification** onglet, cliquez sur le certificat d’autorité de certification qui sera le certificat racine approuvé (vous pouvez choisir le certificat d’autorité de certification du serveur WindowsServerEssentials). Cliquez sur **activer Windows7les ordinateurs clients à se connecter via DirectAccess**, puis cliquez sur **suivant**.  
  
4.  Suivez les instructions pour terminer l’Assistant.  
  
> [!IMPORTANT]
>  Il existe un problème connu pour les ordinateurs Windows7 entreprise qui se connectent via DirectAccess si le serveur WindowsServerEssentials ur1 pas préinstallé. Pour activer les connexions DirectAccess dans cet environnement, vous devez effectuer les étapes supplémentaires suivantes:  
>   
>  1.  Installez le correctif décrit dans [l’article de la Base de connaissances Microsoft (Ko) 2796394](https://support.microsoft.com/kb/2796394) sur le serveur WindowsServerEssentials. Redémarrez le serveur.  
> 2.  Puis installez le correctif décrit dans [l’article de la Base de connaissances Microsoft (Ko) 2615847](https://support.microsoft.com/kb/2615847) sur chaque ordinateur Windows7.  
>   
>      Ce problème a été résolu dans WindowsServerEssentials.  
  
###  <a name="BKMK_NLS"></a>Étape5D: configurer le serveur d’emplacement réseau  
 Cette section fournit des instructions pas à pas pour configurer les paramètres du serveur emplacement réseau.  
  
> [!NOTE]
>  Avant de commencer, copiez le contenu du dossier < systemDrive\ > \inetpub\wwwroot dans le dossier de Server\Bin\WebApps\Site\insideoutside \Program Files\Windows < systemDrive\ >. Également copier le fichier default.aspx à partir du dossier de Server\Bin\WebApps\Site \Program Files\Windows < systemDrive\ > au dossier < systemDrive\ > \Program Files\Windows Server\Bin\WebApps\Site\insideoutside.  
  
##### <a name="to-configure-the-network-location-server"></a>Pour configurer le serveur emplacement réseau  
  
1.  Sur la page de démarrage, ouvrez **gestion de l’accès à distance**.  
  
2.  Dans la Console de gestion de l’accès à distance, cliquez sur **Configuration**et dans le **configuration de l’accès à distance** décrit en détail dans le volet, **étape3**, cliquez sur **modifier**.  
  
3.  Dans l’Assistant d’installation de serveur accès à distance, sur le **serveur d’emplacement réseau** onglet, sélectionnez **le serveur emplacement réseau est déployé sur le serveur d’accès à distance**, puis sélectionnez le certificat émis précédemment (dans [étape3: préparer un certificat et un enregistrement DNS pour le serveur emplacement réseau](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS)).  
  
4.  Suivez les instructions pour terminer l’Assistant, puis cliquez sur **Terminer**.  
  
###  <a name="BKMK_CA"></a>Étape5e: ajouter une clé de Registre pour contourner l’autorité de certification quand vous établissez un canal IPsec  
 L’étape suivante consiste à configurer le serveur pour contourner l’autorité de certification quand un canal IPsec est établi.  
  
##### <a name="to-add-a-registry-key-to-bypass-the-ca-certification"></a>Pour ajouter une clé de Registre pour contourner l’autorité de certification  
  
1.  Sur la page de démarrage, ouvrez **regedit** (l’Éditeur du Registre).  
  
2.  Dans l’Éditeur du Registre, développez **HKEY_LOCAL_MACHINE**, développez **système**, développez **CurrentControlSet**, développez **Services**, puis développez **IKEEXT**.  
  
3.  Sous **IKEEXT**, avec le bouton droit **paramètres**, cliquez sur **New**, puis cliquez sur **DWORD (32bits) valeur**.  
  
4.  Renommez la valeur nouvellement ajoutée **ikeflags**.  
  
5.  Double-cliquez sur **ikeflags**, définissez le **Type** à **hexadécimal**, définissez la valeur sur **8000**, puis cliquez sur **OK**.  
  
> [!NOTE]
>  Vous pouvez utiliser la commande Windows PowerShell suivante avec élévation de privilèges pour ajouter cette clé de Registre:  
>   
>  `Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\IKEEXT\Parameters -Name ikeflags -Type DWORD -Value 0x8000`  
  
##  <a name="BKMK_NRPT"></a>Étape6: Configurer les paramètres de Table de stratégie de résolution de noms pour le serveur DirectAccess  
 Cette section fournit des instructions pour modifier les nom résolution stratégie tableau entrées pour les adresses internes (par exemple, les entrées avec un suffixe contoso.local) pour les objets stratégie de groupe client DirectAccess, puis définissez l’adresse de l’interface IP-HTTPS.  
  
#### <a name="to-configure-name-resolution-policy-table-entries"></a>Pour configurer les entrées de Table de stratégie de résolution de noms  
  
1.  Sur la page de démarrage, ouvrez **gestion des stratégies de groupe**.  
  
2.  Dans la console de gestion des stratégies de groupe, cliquez sur la forêt par défaut et le domaine, avec le bouton droit **paramètres des clients DirectAccess**, puis cliquez sur **modifier**.  
  
3.  Cliquez sur **Configurations d’ordinateur**, cliquez sur **stratégies**, cliquez sur **paramètres Windows**, puis cliquez sur **stratégie de résolution de noms**. Choisissez l’entrée qui a l’espace de noms est identique à votre suffixe DNS, puis cliquez sur **modifier la règle**.  
  
4.  Cliquez sur le **paramètres DNS pour DirectAccess** onglet; puis sélectionnez **activer les paramètres DNS pour DirectAccess dans cette règle**. Ajoutez l’adresse IPv6 pour l’interface IP-HTTPS dans la liste des serveurs DNS.  
  
    > [!NOTE]
    >  Vous pouvez utiliser la commande Windows PowerShell suivante pour obtenir l’adresse IPv6:  
    >   
    >  `(Get-NetIPInterface -InterfaceAlias IPHTTPSInterface | Get-NetIPAddress -PrefixLength 128)[1].IPAddress`  
  
##  <a name="BKMK_TCPUDP"></a>Étape7: Configurer les règles de pare-feu TCP et UDP pour les objets stratégie de groupe du serveur DirectAccess  
 Cette section comprend des instructions pour configurer des règles de pare-feu TCP et UDP pour la stratégie de groupe du serveur DirectAccess.  
  
#### <a name="to-configure-firewall-rules"></a>Pour configurer des règles de pare-feu  
  
1.  Sur la page de démarrage, ouvrez **gestion des stratégies de groupe**.  
  
2.  Dans la console de gestion des stratégies de groupe, cliquez sur la forêt par défaut et le domaine, avec le bouton droit **paramètres du serveur DirectAccess**, puis cliquez sur **modifier**.  
  
3.  Cliquez sur **Configuration ordinateur**, cliquez sur **stratégies**, cliquez sur **paramètres Windows**, cliquez sur **paramètres de sécurité**, cliquez sur **pare-feu Windows avec fonctions avancées de sécurité**, cliquez sur le niveau suivant **pare-feu Windows avec fonctions avancées de sécurité**, puis cliquez sur **règles de trafic entrant**. Avec le bouton droit **nom de domaine serveur (TCP-entrant)**, puis cliquez sur **propriétés**.  
  
4.  Cliquez sur le **étendue** onglet, puis, dans le **adresse IP locale** liste, ajoutez l’adresse IPv6 de l’interface IP-HTTPS.  
  
5.  Répétez la même procédure pour **serveur de nom de domaine (UDP-entrant)**.  
  
##  <a name="BKMK_DNS64"></a>Étape8: Modifier la configuration DNS64 pour écouter l’interface IP-HTTPS  
 Vous devez modifier la configuration DNS64 pour écouter l’interface IP-HTTPS à l’aide de la commande Windows PowerShell suivante.  
  
```powershell  
Set-NetDnsTransitionConfiguration  �AcceptInterface IPHTTPSInterface  
```  
  
##  <a name="BKMK_ExemptPort"></a>Étape9: Réserver des ports pour le service WinNat  
 Utilisez la commande Windows PowerShell suivante pour réserver des ports pour le service WinNat. Remplacez «192.168.1.100» par l’adresse IPv4 réelle de votre serveur WindowsServerEssentials.  
  
```powershell  
Set-NetNatTransitionConfiguration  �IPv4AddressPortPool @("192.168.1.100, 10000-47000")  
```  
  
> [!IMPORTANT]
>  Pour éviter les conflits de port avec les applications, veillez à ce que la plage de ports que vous réservez pour le service WinNat n’inclut pas le port 6602.  
  
##  <a name="BKMK_WinNAT"></a>Étape10: Redémarrer le service WinNat  
 Redémarrez le service pilote NAT Windows (WinNat) en exécutant la commande Windows PowerShell suivante.  
  
```powershell  
Restart-Service winnat  
```  
  
##  <a name="BKMK_AppendixBPowerShellScript"></a>Annexe: Installer DirectAccess à l’aide de Windows PowerShell  
 Cette section décrit comment installer et configurer DirectAccess à l’aide de Windows PowerShell.  
  
### <a name="preparation"></a>Préparation  
 Avant de commencer la configuration de votre serveur pour DirectAccess, vous devez procéder comme suit:  
  
1.  Suivez la procédure décrite dans [étape3: préparer un certificat et un enregistrement DNS pour le serveur emplacement réseau](Configure-DirectAccess-in-Windows-Server-Essentials.md#BKMK_DNS) pour inscrire un certificat nommé **DirectAccess-NLS.contoso.com** (où **contoso.com** est remplacé par le nom de domaine interne réel) et pour ajouter un enregistrement DNS pour le serveur d’emplacement réseau (NLS).  
  
2.  Ajouter un groupe de sécurité nommé **DirectAccessClients** dans ActiveDirectory, puis ajoutez les ordinateurs clients pour lesquels vous souhaitez fournir la fonctionnalité DirectAccess. Pour plus d’informations, voir [étape4: créer un groupe de sécurité pour le client DirectAccess ordinateurs](#BKMK_AddSecurityGroup).  
  
### <a name="commands"></a>Commandes  
  
> [!IMPORTANT]
>  Dans WindowsServerEssentials, vous n’avez pas besoin de supprimer l’objet stratégie de groupe de préfixe IPv6 inutile. Supprimez la section de code avec l’intitulé suivant:`# [WINDOWS SERVER 2012 ESSENTIALS ONLY] Remove the unnecessary IPv6 prefix GPO`.  
  
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
Set-DAServer  �IPSecRootCertificate $rootcert[0]  
Set  �DAClient  �OnlyRemoteComputers Disabled -Downlevel Enabled  
  
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
Set-NetNatTransitionConfiguration  �IPv4AddressPortPool @("192.168.1.100, 10000-47000")  
  
# Restart the WinNat service  
Restart-Service winnat  
```  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Gérer l’accès en tout lieu](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gérer WindowsServerEssentials](Manage-Windows-Server-Essentials.md)
