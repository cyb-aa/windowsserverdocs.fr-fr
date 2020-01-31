---
title: Problèmes connus de Windows Admin Center
description: Problèmes connus de Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 06/07/2019
ms.openlocfilehash: 4a91d09d6824795a21a9a7cdc7695c407aa70756
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822702"
---
# <a name="windows-admin-center-known-issues"></a>Problèmes connus de Windows Admin Center

> S’applique à : Centre d’administration Windows, version préliminaire du centre d’administration Windows

Si vous rencontrez un problème non décrit dans cette page, veuillez [nous en faire part](https://aka.ms/WACfeedback).

## <a name="installer"></a>Programme d’installation

- Lorsque vous installez Windows Admin Center à l’aide de votre propre certificat, n'oubliez pas que si vous copiez l’empreinte numérique à partir de l’outil MMC du Gestionnaire de certificat, [il contiendra un caractère non valide au début.](https://support.microsoft.com/help/2023835/certificate-thumbprint-displayed-in-mmc-certificate-snap-in-has-extra) Pour résoudre ce problème, tapez le premier caractère de l’empreinte numérique et copiez/collez le reste.

- L’utilisation du port inférieur à 1024 n’est pas prise en charge. En mode de service, vous pouvez éventuellement configurer le port 80 pour rediriger vers le port spécifié.

## <a name="general"></a>Général

- Si le centre d’administration Windows est installé en tant que passerelle sur **Windows Server 2016** en cas d’utilisation intensive, le service peut se bloquer avec une erreur dans le journal des événements qui contient ```Faulting application name: sme.exe``` et ```Faulting module name: WsmSvc.dll```. Cela est dû à un bogue qui a été corrigé dans Windows Server 2019. Le correctif pour Windows Server 2016 a été inclus dans la mise à jour cumulative du 2019 du 1er février, [KB4480977](https://www.catalog.update.microsoft.com/Search.aspx?q=4480977).

- Si vous avez installé le centre d’administration Windows en tant que passerelle et que votre liste de connexions semble endommagée, procédez comme suit :

   > [!WARNING]
   >Cette opération supprime la liste des connexions et les paramètres de tous les utilisateurs du centre d’administration Windows sur la passerelle.

  1. Désinstaller Windows Admin Center
  2. Supprimez le dossier **Server Management Experience** sous **C:\Windows\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft**
  3. Réinstaller Windows Admin Center

- Si vous laissez l’outil ouvert et inactif pendant une longue période, vous pouvez obtenir plusieurs erreurs **Error: The runspace state is not valid for this operation**. Si cela se produit, actualisez votre navigateur. Si vous rencontrez ce problème, [envoyez-nous vos commentaires](https://aka.ms/WACfeedback).

- Il peut y avoir une variance mineure entre les numéros de version des systèmes d’exploitation en cours d’exécution dans les modules du centre d’administration Windows et ce qui est indiqué dans l’avis du logiciel tiers.

### <a name="extension-manager"></a>Gestionnaire d’extensions

- Lorsque vous mettez à jour le centre d’administration Windows, vous devez réinstaller vos extensions.
- Si vous ajoutez un flux d’extension qui n’est pas accessible, aucun avertissement n’est présent. [14412861]

## <a name="browser-specific-issues"></a>Problèmes spécifiques du navigateur

### <a name="microsoft-edge"></a>Microsoft Edge

- Si vous avez déployé le centre d’administration Windows en tant que service et que vous utilisez Microsoft Edge comme navigateur, la connexion de votre passerelle à Azure peut échouer après la génération d’une nouvelle fenêtre de navigateur. Essayez de contourner ce problème en ajoutant https://login.microsoftonline.com , https://login.live.com et l’URL de votre passerelle en tant que sites approuvés et sites autorisés pour les paramètres du bloqueur de fenêtres publicitaires dans le navigateur côté client. Pour plus d’informations sur la résolution de ce problème, dans le [Guide de résolution des problèmes](troubleshooting.md#azure-features-dont-work-properly-in-edge). [17990376]

### <a name="google-chrome"></a>Google Chrome

- Avant la version 70 (publiée fin octobre, 2018) chrome présentait un [bogue](https://bugs.chromium.org/p/chromium/issues/detail?id=423609) concernant le protocole WebSocket et l’authentification NTLM. Cela affecte les outils suivants : Événements, PowerShell, Bureau à distance.

- Chrome peut afficher plusieurs demandes d'informations d'identification, en particulier pendant l’expérience d'ajout de connexion dans un environnement de **groupe de travail** (hors domaine).

- Si vous avez déployé le centre d’administration Windows en tant que service, les fenêtres contextuelles de l’URL de la passerelle doivent être activées pour que toutes les fonctionnalités d’intégration Azure fonctionnent.

### <a name="mozilla-firefox"></a>Mozilla Firefox

Windows Admin Center n’est pas testé avec Mozilla Firefox, mais la plupart des fonctionnalités doivent être opérationnelles.

- Installation de Windows 10 : Mozilla Firefox possède son propre magasin de certificats. vous devez donc importer le certificat de ```Windows Admin Center Client``` dans Firefox pour utiliser le centre d’administration Windows sur Windows 10.

## <a name="websocket-compatibility-when-using-a-proxy-service"></a>Compatibilité WebSocket lors de l’utilisation d’un service proxy

Les modules Bureau à distance, PowerShell et Événements dans Windows Admin Center utilisent le protocole WebSocket, qui n’est souvent pas pris en charge lors de l’utilisation d’un service de proxy. La compatibilité de la prise en charge de WebSocket dans le proxy d’application Azure AD est en [préversion](https://blogs.technet.microsoft.com/applicationproxyblog/2018/03/28/limited-websocket-support-now-in-public-preview/) et nécessite des commentaires sur la compatibilité.

## <a name="support-for-windows-server-versions-before-2016-2012-r2-2012-2008-r2"></a>Prise en charge des versions de Windows Server antérieures à 2016 (2012 R2, 2012, 2008 R2)

> [!NOTE]
> Le centre d’administration Windows requiert des fonctionnalités PowerShell qui ne sont pas incluses dans Windows Server 2012 R2, 2012 ou 2008 R2. Si vous souhaitez gérer Windows Server à l’aide du centre d’administration Windows, vous devez installer WMF version 5,1 ou ultérieure sur ces serveurs.

Saisissez `$PSVersiontable` dans PowerShell pour vérifier que WMF est installé, et que sa version est 5.1 ou ultérieure.

S’il n’est pas installé, vous pouvez [télécharger et installer WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616).

## <a name="role-based-access-control-rbac"></a>Access Control basée sur les rôles (RBAC)

- Le déploiement de RBAC échoue sur les ordinateurs configurés pour utiliser le Contrôle d'application Windows Defender (WDAC, anciennement appelé Intégrité du Code). [16568455]

- Pour utiliser RBAC dans un cluster, vous devez déployer la configuration sur chaque nœud membre individuellement.

- Lorsque RBAC est déployé, vous pouvez obtenir des erreurs non autorisées qui sont attribuées de manière incorrecte à la configuration de RBAC. [16369238]

## <a name="server-manager-solution"></a>Solution du Gestionnaire de serveur

### <a name="certificates"></a>Certificats

- Impossible d’importer un certificat .PFX chiffré dans le magasin de l’utilisateur actuel. [11818622]

### <a name="events"></a>Événements

- Les événements sont affectés par la [compatibilité de websocket lors de l’utilisation d’un service de proxy.](#websocket-compatibility-when-using-a-proxy-service)

- Vous pouvez recevoir une erreur qui fait référence à la « taille de paquet » lors de l’exportation de fichiers journaux volumineux.

  - Pour résoudre ce cas, utilisez la commande suivante dans une invite de commandes avec élévation de privilèges sur l’ordinateur de la passerelle : ```winrm set winrm/config @{MaxEnvelopeSizekb="8192"}```

### <a name="files"></a>Fichiers

- Le chargement ou téléchargement de fichiers volumineux n’est pas encore pris en charge. (\~limite de 100 Mo) [12524234]

### <a name="powershell"></a>PowerShell

- PowerShell est affecté par la [compatibilité de websocket lors de l’utilisation d’un service de proxy](#websocket-compatibility-when-using-a-proxy-service)

- Coller avec un seul clic droit comme dans la console de bureau PowerShell ne fonctionne pas. Au lieu de cela, vous obtiendrez le menu local du navigateur, dans lequel vous pouvez sélectionner coller. CTRL + V fonctionne également.

- CTRL + C pour copier ne fonctionne pas, il enverra toujours la commande de pause Ctrl + C à la console. Copier à partir du menu contextuel (avec un clic droit) fonctionne.

- Lorsque vous réduisez la fenêtre de Windows Admin Center, le contenu du terminal est réorganisé, mais lorsque vous l'agrandissez à nouveau, le contenu ne peut pas retourner à son état précédent. Si le contenu devient confus, essayez la commande Clear-Host, ou déconnectez et reconnectez-vous en utilisant le bouton au-dessus du terminal.

### <a name="registry-editor"></a>Éditeur du Registre

- La fonctionnalité de recherche n'est pas implémentée. [13820009]

### <a name="remote-desktop"></a>Bureau à distance

- Lorsque le centre d’administration Windows est déployé en tant que service, le chargement de l’outil Bureau à distance peut échouer après la mise à jour du service Centre d’administration Windows vers une nouvelle version. Pour contourner ce problème, effacez le cache de votre navigateur.   [23824194]

- L’outil Bureau à distance peut ne pas réussir à se connecter lors de la gestion de Windows Server 2012. [20258278]

- Lorsque vous utilisez la Bureau à distance pour vous connecter à un ordinateur qui n’est pas joint à un domaine, vous devez entrer votre compte au format ```MACHINENAME\USERNAME```.

- Certaines configurations peuvent bloquer le client Bureau à distance du centre d’administration Windows avec la stratégie de groupe. Si vous rencontrez ce problème, activez ```Allow users to connect remotely by using Remote Desktop Services``` sous ```Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Connections```

- La Bureau à distance est appliquée par la [compatibilité WebSocket.](#websocket-compatibility-when-using-a-proxy-service)

- L’outil Bureau à distance ne prend actuellement pas en charge la fonction copier/coller de texte, d'image ou de fichier entre le bureau local et la session à distance.

- Pour tout copier/coller dans la session à distance, vous pouvez copier comme d'habitude (clic droit + copier ou Ctrl + C), mais coller nécessite clic droit + Coller (Ctrl + V NE fonctionne PAS)

- Vous ne pouvez pas envoyer les commandes de touches suivantes à la session à distance
  - Alt+Tabulation
  - Touches de fonction
  - Touche Windows
  - Imp. écr.

### <a name="roles-and-features"></a>Rôles et fonctionnalités

- Lorsque vous sélectionnez des rôles ou des fonctionnalités avec des sources non disponibles pour l’installation, ils sont ignorés. [12946914]

- Si vous choisissez de ne pas redémarrer automatiquement après l’installation du rôle, nous ne vous le redemanderons pas. [13098852]

- Si vous choisissez de redémarrer automatiquement, le redémarrage se produit avant que l’état soit mis à jour à 100 %. [13098852]

### <a name="storage"></a>Stockage

- Bas niveau : les lecteurs de CD-ROM/DVD/disquette n’apparaissent pas en tant que volumes au bas niveau.

- Bas niveau : certaines propriétés de Volumes et disques ne sont pas disponibles au bas niveau, donc elles apparaissent comme inconnues ou vides dans le volet d’informations.

- Bas niveau : lorsque vous créez un nouveau volume, ReFS prend uniquement en charge une taille d’unité d’allocation de 64 Ko sur les ordinateurs Windows 2012 et 2012 R2. Si un volume ReFS est créé avec une plus petite taille d’unité d’allocation sur les cibles de bas niveau, le formatage du système de fichiers échoue. Le nouveau volume ne sera pas utilisable. La solution consiste à supprimer le volume et à utiliser la taille d’unité d’allocation de 64 Ko.

### <a name="updates"></a>Mises à jour

- Après l’installation des mises à jour, l’état de l’installation peut être mis en cache et nécessiter une actualisation du navigateur.

- Vous pouvez rencontrer l’erreur : « le jeu de clés n’existe pas » lors de la tentative de configuration de la gestion des mises à jour Azure. Dans ce cas, essayez les étapes de mise à jour suivantes sur le nœud géré-
    1. Arrêtez le service « services de chiffrement ».
    2. Modifiez les options des dossiers pour afficher les fichiers masqués (si nécessaire).
    3. A obtenu le dossier « %allusersprofile%\Microsoft\Crypto\RSA\S-1-5-18 » et a supprimé tout son contenu.
    4. Redémarrez le service « services de chiffrement ».
    5. Répéter la configuration de Update Management à l’aide du centre d’administration Windows

### <a name="virtual-machines"></a>Machines virtuelles

- Lors de la gestion des ordinateurs virtuels sur un ordinateur hôte Windows Server 2012, l’outil connexion à un ordinateur virtuel dans le navigateur ne parvient pas à se connecter à la machine virtuelle. Le téléchargement du fichier. RDP pour se connecter à la machine virtuelle doit continuer à fonctionner. [20258278]

- Azure Site Recovery : si la récupération automatique du système est configurée sur l’hôte en dehors de WAC, vous ne pourrez pas protéger une machine virtuelle à partir de WAC [18972276]

- Des fonctionnalités avancées disponibles dans le Gestionnaire Hyper-V (telles que le Gestionnaire de réseau SAN virtuel, Déplacer un ordinateur virtuel, Export VM, Réplication de l’ordinateur virtuel) ne sont actuellement pas prises en charge.

### <a name="virtual-switches"></a>Commutateurs virtuels

- Switch Embedded Teaming (SET) : lorsque vous ajoutez des cartes réseau à une équipe, elles doivent être sur le même sous-réseau.

## <a name="computer-management-solution"></a>Solution de gestion de l'ordinateur

La solution de gestion de l’ordinateur contient un sous-ensemble d'outils de la solution du Gestionnaire de serveur, donc les mêmes problèmes connus s’appliquent, ainsi que des problèmes spécifiques de la solution de gestion de l’ordinateur suivants :

- Si vous utilisez un compte Microsoft ([MSA](https://account.microsoft.com/account/)) ou si vous utilisez Azure Active Directory (AAD) pour vous connecter à votre ordinateur Windows 10, vous devez utiliser « gérer en tant que » pour fournir les informations d’identification d’un compte d’administrateur local [16568455]

- Lorsque vous essayez de gérer l’hôte local, vous êtes invité à élever le processus de passerelle. Si vous cliquez sur **non** dans le menu contextuel contrôle de compte d’utilisateur qui suit, vous devez annuler la tentative de connexion et recommencer.

- Par défaut, Windows 10 ne dispose pas de la communication à distance WinRM/PowerShell.
  
  - Pour activer la gestion du client Windows 10, vous devez exécuter la commande ```Enable-PSRemoting``` à partir d’une invite de commandes PowerShell avec élévation de privilèges.

  - Vous devez également mettre à jour votre pare-feu pour autoriser les connexions à partir de l’extérieur du sous-réseau local avec ```Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any```. Pour les scénarios de réseaux plus restrictifs, reportez-vous à [cette documentation](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-5.1).

## <a name="failover-cluster-manager-solution"></a>Solution du Gestionnaire du cluster de basculement

- Lors de la gestion d’un cluster, (hyperconvergé ou classique ?) vous pouvez rencontrer une erreur **shell was not found**. Dans ce cas, rechargez votre navigateur ou quittez la page pour accéder à un autre outil, puis revenez. [13882442]

- Un problème peut se produire lors de la gestion d’un cluster de bas niveau (Windows Server 2012 ou 2012 R2) qui n’a pas été entièrement configuré. La solution à ce problème est de vous assurer que la fonctionnalité Windows **RSAT-Clustering-PowerShell** a été installée et activée sur **chaque nœud membre** du cluster. Pour le faire avec PowerShell, entrez la commande `Install-WindowsFeature -Name RSAT-Clustering-PowerShell` sur tous les nœuds du cluster. [12524664]

- Le Cluster peut devoir être ajouté avec le nom de domaine complet pour être correctement détecté.

- Lors de la connexion à un cluster à l’aide de Windows Admin Center installé sous forme de passerelle et de l'indication d'un nom d’utilisateur/mot de passe explicite pour vous authentifier, vous devez sélectionner **Use these credentials for all connections** pour que les informations d’identification soient disponibles pour interroger les nœuds membres.

## <a name="hyper-converged-cluster-manager-solution"></a>Solution de gestion de cluster hyperconvergé

- Certaines commandes telles que **Drives - Update firmware**, **Servers - Remove** et **Volumes - Open** sont désactivées et actuellement non prises en charge.

## <a name="azure-services"></a>Services Azure

### <a name="azure-file-sync-permissions"></a>Autorisations de Azure File Sync

Azure File Sync nécessite des autorisations dans Azure que le centre d’administration Windows n’a pas fournies avant la version 1910. Si vous avez enregistré votre passerelle du centre d’administration Windows avec Azure à l’aide d’une version antérieure à la version 1910 du centre d’administration Windows, vous devrez mettre à jour votre application Azure Active Directory pour obtenir les autorisations appropriées pour utiliser Azure File Sync dans la dernière version de Centre d’administration Windows. L’autorisation supplémentaire permet Azure File Sync d’effectuer la configuration automatique de l’accès au compte de stockage, comme décrit dans cet article : [Assurez-vous Azure file Sync a accès au compte de stockage](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#tabpanel_CeZOj-G++Q-5_azure-portal).

Pour mettre à jour votre application Azure Active Directory, vous pouvez effectuer l’une des deux opérations suivantes :
1. Accédez à **paramètres** > **Azure** > **désinscrire**, puis inscrivez à nouveau le centre d’administration Windows avec Azure, en veillant à créer une application de Azure Active Directory. 
2. Accédez à votre application Azure Active Directory et ajoutez manuellement l’autorisation nécessaire à votre application Azure Active Directory existante inscrite auprès du centre d’administration Windows. Pour ce faire, accédez à **paramètres** > vue de > **Azure** **dans Azure**. Dans le panneau inscription de l' **application** dans Azure, accédez à **autorisations d’API**, puis sélectionnez **Ajouter une autorisation**. Faites défiler la liste pour sélectionner **Azure Active Directory graphique**, sélectionnez **autorisations déléguées**, développez **répertoire**, puis sélectionnez **Directory. AccessAsUser. All**. Cliquez sur **Ajouter des autorisations** pour enregistrer les mises à jour apportées à l’application.

### <a name="options-for-setting-up-azure-management-services"></a>Options de configuration des services de gestion Azure

Les services de gestion Azure, y compris Azure Monitor, Azure Update Management et Azure Security Center, utilisent le même agent pour un serveur local : le Microsoft Monitoring Agent. Azure Update Management dispose d’un ensemble plus limité de régions prises en charge et requiert la liaison de l’espace de travail Log Analytics à un compte Azure Automation. En raison de cette limitation, si vous souhaitez configurer plusieurs services dans le centre d’administration Windows, vous devez d’abord configurer Azure Update Management, puis Azure Security Center ou Azure Monitor. Si vous avez configuré des services de gestion Azure qui utilisent la Microsoft Monitoring Agent, puis que vous essayez de configurer Azure Update Management à l’aide du centre d’administration Windows, le centre d’administration Windows vous permet de configurer Azure Update Management uniquement si l’existant les ressources liées aux Microsoft Monitoring Agent prennent en charge Azure Update Management. Si ce n’est pas le cas, vous avez deux options :

1. Accédez au panneau de configuration > Microsoft Monitoring Agent pour [déconnecter votre serveur des solutions de gestion Azure existantes](https://docs.microsoft.com/azure/azure-monitor/platform/log-faq#q-how-do-i-stop-an-agent-from-communicating-with-log-analytics) (comme Azure Monitor ou Azure Security Center). Configurez ensuite Azure Update Management dans le centre d’administration Windows. Après cela, vous pouvez revenir à la configuration de vos autres solutions de gestion Azure via le centre d’administration Windows sans problèmes.
2. Vous pouvez [configurer manuellement les ressources Azure nécessaires pour azure Update Management](https://docs.microsoft.com/azure/automation/automation-update-management) , puis [mettre à jour manuellement les Microsoft Monitoring agent](https://docs.microsoft.com/azure/azure-monitor/platform/agent-manage#adding-or-removing-a-workspace) (en dehors du centre d’administration Windows) pour ajouter le nouvel espace de travail correspondant à la solution Update Management que vous souhaitez utiliser.
