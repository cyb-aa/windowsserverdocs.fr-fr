---
title: Problèmes connus du centre d’administration Windows
description: Problèmes connus du centre d’administration Windows (projet Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 06/07/2019
ms.openlocfilehash: 5c9e8b7e1e11deaa82fbec6f451b4f194609c299
ms.sourcegitcommit: 1d83ca198c50eef83d105151551c6be6f308ab94
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82605549"
---
# <a name="windows-admin-center-known-issues"></a>Problèmes connus du centre d’administration Windows

> S'applique à : Windows Admin Center, Windows Admin Center Preview

Si vous rencontrez un problème qui n’est pas décrit dans cette page, [faites-le nous savoir](https://aka.ms/WACfeedback).

## <a name="installer"></a>Programme d’installation

- Lorsque vous installez le centre d’administration Windows à l’aide de votre propre certificat, gardez à l’esprit que si vous copiez l’empreinte numérique à partir de l’outil MMC du gestionnaire de certificats, [il contiendra un caractère non valide au début.](https://support.microsoft.com/help/2023835/certificate-thumbprint-displayed-in-mmc-certificate-snap-in-has-extra) Pour contourner ce problème, tapez le premier caractère de l’empreinte numérique, puis copiez/collez le reste.

- L’utilisation du port inférieur à 1024 n’est pas prise en charge. En mode de service, vous pouvez éventuellement configurer le port 80 pour rediriger vers le port spécifié.

## <a name="general"></a>Général

- Si le centre d’administration Windows est installé en tant que passerelle sur **Windows Server 2016** en cas d’utilisation intensive, le service peut se bloquer avec une erreur dans ```Faulting application name: sme.exe``` le ```Faulting module name: WsmSvc.dll```journal des événements qui contient et. Cela est dû à un bogue qui a été corrigé dans Windows Server 2019. Le correctif pour Windows Server 2016 a été inclus dans la mise à jour cumulative du 2019 du 1er février, [KB4480977](https://www.catalog.update.microsoft.com/Search.aspx?q=4480977).

- Si vous avez installé le centre d’administration Windows en tant que passerelle et que votre liste de connexions semble endommagée, procédez comme suit :

   > [!WARNING]
   >Cette opération supprime la liste des connexions et les paramètres de tous les utilisateurs du centre d’administration Windows sur la passerelle.

  1. Désinstaller le centre d’administration Windows
  2. Supprimer le dossier d' **expérience d’administration de serveur** sous **C:\Windows\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft**
  3. Réinstaller le centre d’administration Windows

- Si vous laissez l’outil ouvert et inactif pendant une longue période de temps, vous pouvez obtenir plusieurs **Erreurs : l’état de l’instance d’exécution n’est pas valide pour cette opération** . Si cela se produit, actualisez votre navigateur. Si vous rencontrez ce problème, [envoyez-nous vos commentaires](https://aka.ms/WACfeedback).

- Il peut y avoir une variance mineure entre les numéros de version des systèmes d’exploitation en cours d’exécution dans les modules du centre d’administration Windows et ce qui est indiqué dans l’avis du logiciel tiers.

### <a name="extension-manager"></a>Gestionnaire d’extensions

- Lorsque vous mettez à jour le centre d’administration Windows, vous devez réinstaller vos extensions.
- Si vous ajoutez un flux d’extension qui n’est pas accessible, aucun avertissement n’est présent. [14412861]

## <a name="browser-specific-issues"></a>Problèmes spécifiques au navigateur

### <a name="microsoft-edge"></a>Microsoft Edge

- Si vous avez déployé le centre d’administration Windows en tant que service et que vous utilisez Microsoft Edge comme navigateur, la connexion de votre passerelle à Azure peut échouer après la génération d’une nouvelle fenêtre de navigateur. Essayez de contourner ce problème en ajoutant https://login.microsoftonline.com, https://login.live.comet l’URL de votre passerelle en tant que sites de confiance et sites autorisés pour les paramètres du bloqueur de fenêtres publicitaires sur votre navigateur côté client. Pour plus d’informations sur la résolution de ce problème, dans le [Guide de résolution des problèmes](troubleshooting.md#azure-features-dont-work-properly-in-edge). [17990376]

### <a name="google-chrome"></a>Google Chrome

- Avant la version 70 (publiée fin octobre, 2018) chrome présentait un [bogue](https://bugs.chromium.org/p/chromium/issues/detail?id=423609) concernant le protocole WebSocket et l’authentification NTLM. Cela affecte les outils suivants : événements, PowerShell Bureau à distance.

- Chrome peut contextuellement plusieurs invites d’informations d’identification, en particulier lors de l’expérience ajouter une connexion dans un environnement **de groupe de travail** (non-domaine).

- Si vous avez déployé le centre d’administration Windows en tant que service, les fenêtres contextuelles de l’URL de la passerelle doivent être activées pour que toutes les fonctionnalités d’intégration Azure fonctionnent.

### <a name="mozilla-firefox"></a>Mozilla Firefox

Le centre d’administration Windows n’est pas testé avec Mozilla Firefox, mais la plupart des fonctionnalités doivent fonctionner.

- Installation de Windows 10 : Mozilla Firefox possède son propre magasin de certificats. vous devez donc importer ```Windows Admin Center Client``` le certificat dans Firefox pour utiliser le centre d’administration Windows sur Windows 10.

## <a name="websocket-compatibility-when-using-a-proxy-service"></a>Compatibilité WebSocket lors de l’utilisation d’un service proxy

Les modules Bureau à distance, PowerShell et Events du centre d’administration Windows utilisent le protocole WebSocket, qui n’est souvent pas pris en charge lors de l’utilisation d’un service proxy. 

## <a name="support-for-windows-server-versions-before-2016-2012-r2-2012-2008-r2"></a>Prise en charge des versions de Windows Server antérieures à 2016 (2012 R2, 2012, 2008 R2)

> [!NOTE]
> Le centre d’administration Windows requiert des fonctionnalités PowerShell qui ne sont pas incluses dans Windows Server 2012 R2, 2012 ou 2008 R2. Si vous souhaitez gérer Windows Server à l’aide du centre d’administration Windows, vous devez installer WMF version 5,1 ou ultérieure sur ces serveurs.

Tapez `$PSVersiontable` dans PowerShell pour vérifier que WMF est installé, et que sa version est 5.1 ou ultérieure.

S’il n’est pas installé, vous pouvez [télécharger et installer WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616).

## <a name="role-based-access-control-rbac"></a>Contrôle d’accès en fonction du rôle (RBAC)

- Le déploiement RBAC ne fonctionnera pas sur les ordinateurs qui sont configurés pour utiliser le contrôle d’application Windows Defender (WDAC, anciennement appelé intégrité du code). [16568455]

- Pour utiliser RBAC dans un cluster, vous devez déployer la configuration sur chaque nœud de membre individuellement.

- Lorsque RBAC est déployé, vous pouvez recevoir des erreurs non autorisées qui sont attribuées de manière incorrecte à la configuration RBAC. [16369238]

## <a name="server-manager-solution"></a>Solution Gestionnaire de serveur

### <a name="certificates"></a>Certificats

- Importation impossible. Certificat chiffré PFX dans le magasin de l’utilisateur actuel. [11818622]

### <a name="events"></a>Événements

- Les événements sont affectés par [la compatibilité WebSocket lors de l’utilisation d’un service proxy.](#websocket-compatibility-when-using-a-proxy-service)

- Vous pouvez recevoir une erreur qui fait référence à « taille des paquets » lors de l’exportation de fichiers journaux volumineux.

  - Pour résoudre ce cas, utilisez la commande suivante dans une invite de commandes avec élévation de privilèges sur l’ordinateur passerelle :```winrm set winrm/config @{MaxEnvelopeSizekb="8192"}```

### <a name="files"></a>Fichiers

- Le chargement ou le téléchargement de fichiers volumineux ne sont pas encore pris en charge. (\~limite de 100 Mo) [12524234]

### <a name="powershell"></a>PowerShell

- PowerShell est pris en compte par [la compatibilité WebSocket lors de l’utilisation d’un service proxy](#websocket-compatibility-when-using-a-proxy-service)

- Le collage à l’aide d’un simple clic avec le bouton droit de la console PowerShell de bureau ne fonctionne pas. Au lieu de cela, vous obtiendrez le menu contextuel du navigateur, dans lequel vous pouvez sélectionner coller. Ctrl-V fonctionne également.

- Ctrl-C pour copier ne fonctionne pas, il enverra toujours la commande Ctrl-C Break à la console. Copiez à partir du menu contextuel en cliquant avec le bouton droit.

- Lorsque vous réduisez la fenêtre du centre d’administration Windows, le contenu du terminal est redistribué, mais lorsque vous le rendez plus volumineux, le contenu peut ne pas revenir à son état précédent. Si les choses sont brouillées, vous pouvez essayer Clear-Host, ou vous déconnecter et vous reconnecter en utilisant le bouton situé au-dessus du terminal.

### <a name="registry-editor"></a>Éditeur du Registre

- La fonctionnalité de recherche n’est pas implémentée. [13820009]

### <a name="remote-desktop"></a>Bureau à distance

- Lorsque le centre d’administration Windows est déployé en tant que service, le chargement de l’outil Bureau à distance peut échouer après la mise à jour du service Centre d’administration Windows vers une nouvelle version. Pour contourner ce problème, effacez le cache de votre navigateur.   [23824194]

- L’outil Bureau à distance peut ne pas réussir à se connecter lors de la gestion de Windows Server 2012. [20258278]

- Lorsque vous utilisez la Bureau à distance pour vous connecter à un ordinateur qui n’est pas joint à un domaine, vous devez ```MACHINENAME\USERNAME``` entrer votre compte au format.

- Certaines configurations peuvent bloquer le client Bureau à distance du centre d’administration Windows avec la stratégie de groupe. Si vous rencontrez ce problème, ```Allow users to connect remotely by using Remote Desktop Services``` activez sous```Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Connections```

- La Bureau à distance est appliquée par la [compatibilité WebSocket.](#websocket-compatibility-when-using-a-proxy-service)

- L’outil Bureau à distance ne prend actuellement en charge aucun texte, image ou copie/collage de fichiers entre le bureau local et la session à distance.

- Pour effectuer des opérations de copier/coller dans la session à distance, vous pouvez copier en mode normal (clic droit + copier ou Ctrl + C), mais le collage requiert un clic droit + coller (Ctrl + V ne fonctionne pas)

- Vous ne pouvez pas envoyer les commandes clés suivantes à la session à distance
  - Alt + Tab
  - Touches de fonction
  - Touche Windows
  - Écr

### <a name="roles-and-features"></a>Rôles et fonctionnalités

- Lorsque vous sélectionnez des rôles ou des fonctionnalités dont les sources ne sont pas disponibles pour l’installation, elles sont ignorées. [12946914]

- Si vous choisissez de ne pas redémarrer automatiquement après l’installation du rôle, nous ne redemanderons pas. [13098852]

- Si vous choisissez de redémarrer automatiquement, le redémarrage se produit avant que l’État soit mis à jour à 100%. [13098852]

### <a name="storage"></a>Stockage

- De niveau supérieur : les lecteurs de DVD/CD/disquettes n’apparaissent pas en tant que volumes sur le niveau de défaillance.

- De niveau supérieur : certaines propriétés des volumes et des disques ne sont pas disponibles de niveau inférieure, de sorte qu’elles apparaissent inconnues ou vides dans le panneau détails.

- De niveau supérieur : lors de la création d’un nouveau volume, ReFS ne prend en charge qu’une taille d’unité d’allocation de 64 Ko sur les ordinateurs Windows 2012 et 2012 R2. Si un volume ReFS est créé avec une taille d’unité d’allocation inférieure sur les cibles de niveau inférieur, la mise en forme du système de fichiers échoue. Le nouveau volume ne sera pas utilisable. La solution consiste à supprimer le volume et à utiliser une taille d’unité d’allocation de 64 Ko.

### <a name="updates"></a>Mises à jour

- Après l’installation des mises à jour, l’état de l’installation peut être mis en cache et nécessiter une actualisation du navigateur.

- Vous pouvez rencontrer l’erreur : « le jeu de clés n’existe pas » lors de la tentative de configuration de la gestion des mises à jour Azure. Dans ce cas, essayez les étapes de mise à jour suivantes sur le nœud géré-
    1. Arrêtez le service « services de chiffrement ».
    2. Modifiez les options des dossiers pour afficher les fichiers masqués (si nécessaire).
    3. A obtenu le dossier « %allusersprofile%\Microsoft\Crypto\RSA\S-1-5-18 » et a supprimé tout son contenu.
    4. Redémarrez le service « services de chiffrement ».
    5. Répéter la configuration de Update Management à l’aide du centre d’administration Windows

### <a name="virtual-machines"></a>Virtual Machines

- Lors de la gestion des ordinateurs virtuels sur un ordinateur hôte Windows Server 2012, l’outil connexion à un ordinateur virtuel dans le navigateur ne parvient pas à se connecter à la machine virtuelle. Le téléchargement du fichier. RDP pour se connecter à la machine virtuelle doit continuer à fonctionner. [20258278]

- Azure Site Recovery : si la récupération automatique du système est configurée sur l’hôte en dehors de WAC, vous ne pourrez pas protéger une machine virtuelle à partir de WAC [18972276]

- Les fonctionnalités avancées disponibles dans le Gestionnaire Hyper-V, telles que le gestionnaire de réseau SAN virtuel, le déplacement de machines virtuelles, l’exportation de machine virtuelle, ne sont actuellement pas prises en charge.

### <a name="virtual-switches"></a>Commutateurs virtuels

- Switch Embedded Teaming (SET) : quand vous ajoutez des cartes réseau à une équipe, celles-ci doivent se trouver sur le même sous-réseau.

## <a name="computer-management-solution"></a>Solution de gestion des ordinateurs

La solution de gestion de l’ordinateur contient un sous-ensemble des outils de la solution Gestionnaire de serveur, de sorte que les mêmes problèmes connus s’appliquent, ainsi que les problèmes spécifiques à la solution de gestion de l’ordinateur :

- Si vous utilisez un compte Microsoft ([MSA](https://account.microsoft.com/account/)) ou si vous utilisez Azure Active Directory (AAD) pour vous connecter à votre ordinateur Windows 10, vous devez utiliser « gérer en tant que » pour fournir les informations d’identification d’un compte d’administrateur local [16568455]

- Lorsque vous tentez de gérer l’hôte local, vous êtes invité à élever le processus de la passerelle. Si vous cliquez sur **non** dans le menu contextuel contrôle de compte d’utilisateur qui suit, vous devez annuler la tentative de connexion et recommencer.

- Par défaut, Windows 10 ne dispose pas de la communication à distance WinRM/PowerShell.
  
  - Pour activer la gestion du client Windows 10, vous devez exécuter la commande ```Enable-PSRemoting``` à partir d’une invite PowerShell avec élévation de privilèges.

  - Vous devrez peut-être également mettre à jour votre pare-feu pour autoriser les connexions ```Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any```à partir de l’extérieur du sous-réseau local avec. Pour les scénarios de réseaux plus restrictifs, reportez-vous à [cette documentation](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-5.1).

## <a name="failover-cluster-manager-solution"></a>Solution Gestionnaire du cluster de basculement

- Lors de la gestion d’un cluster, (Hyper-convergé ou traditionnel ?), vous risquez de rencontrer une erreur d' **interpréteur de commandes introuvable** . Si cela se produit, rechargez votre navigateur ou naviguez jusqu’à un autre outil et revenez. [13882442]

- Un problème peut se produire lors de la gestion d’un cluster de niveau supérieur (Windows Server 2012 ou 2012 R2) qui n’a pas été configuré complètement. Le correctif pour résoudre ce problème consiste à s’assurer que les composants Windows de la fonctionnalité Windows **clustering-PowerShell** ont été installés et activés sur **chaque nœud de membre** du cluster. Pour effectuer cette opération avec PowerShell, entrez la `Install-WindowsFeature -Name RSAT-Clustering-PowerShell` commande sur tous les nœuds de cluster. [12524664]

- Le cluster doit peut-être être ajouté avec l’intégralité du nom de domaine complet pour être détecté correctement.

- Quand vous vous connectez à un cluster à l’aide du centre d’administration Windows installé en tant que passerelle et que vous fournissez un nom d’utilisateur/mot de passe explicite pour l’authentification, vous devez sélectionner **utiliser ces informations d’identification pour toutes les connexions** afin que les informations d’identification soient disponibles pour interroger les nœuds membres.

## <a name="hyper-converged-cluster-manager-solution"></a>Solution hyper-Converged cluster Manager

- Certaines commandes, telles que **Drives-Update Firmware**, **Servers-Remove** et **volumes-Open** , sont désactivées et ne sont pas prises en charge actuellement.

## <a name="azure-services"></a>Services Azure

### <a name="azure-file-sync-permissions"></a>Autorisations de Azure File Sync

Azure File Sync nécessite des autorisations dans Azure que le centre d’administration Windows n’a pas fournies avant la version 1910. Si vous avez enregistré votre passerelle du centre d’administration Windows avec Azure à l’aide d’une version antérieure à la version 1910 du centre d’administration Windows, vous devrez mettre à jour votre application Azure Active Directory pour obtenir les autorisations appropriées pour utiliser Azure File Sync dans la dernière version du centre d’administration Windows. L’autorisation supplémentaire permet Azure File Sync d’effectuer la configuration automatique de l’accès au compte de stockage, comme décrit dans cet article : [Assurez-vous Azure file Sync a accès au compte de stockage](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#tabpanel_CeZOj-G++Q-5_azure-portal).

Pour mettre à jour votre application Azure Active Directory, vous pouvez effectuer l’une des deux opérations suivantes :
1. Accédez à **paramètres** > **Azure** > **désinscrire**Azure, puis inscrivez à nouveau le centre d’administration Windows avec Azure, en veillant à créer une application de Azure Active Directory. 
2. Accédez à votre application Azure Active Directory et ajoutez manuellement l’autorisation nécessaire à votre application Azure Active Directory existante inscrite auprès du centre d’administration Windows. Pour ce faire, accédez à **paramètres** > affichage**Azure** > **dans Azure**. Dans le panneau inscription de l' **application** dans Azure, accédez à **autorisations d’API**, puis sélectionnez **Ajouter une autorisation**. Faites défiler la liste pour sélectionner **Azure Active Directory graphique**, sélectionnez **autorisations déléguées**, développez **répertoire**, puis sélectionnez **Directory. AccessAsUser. All**. Cliquez sur **Ajouter des autorisations** pour enregistrer les mises à jour apportées à l’application.

### <a name="options-for-setting-up-azure-management-services"></a>Options de configuration des services de gestion Azure

Les services de gestion Azure, y compris Azure Monitor, Azure Update Management et Azure Security Center, utilisent le même agent pour un serveur local : le Microsoft Monitoring Agent. Azure Update Management dispose d’un ensemble plus limité de régions prises en charge et requiert la liaison de l’espace de travail Log Analytics à un compte Azure Automation. En raison de cette limitation, si vous souhaitez configurer plusieurs services dans le centre d’administration Windows, vous devez d’abord configurer Azure Update Management, puis Azure Security Center ou Azure Monitor. Si vous avez configuré des services de gestion Azure qui utilisent la Microsoft Monitoring Agent, puis que vous essayez de configurer Azure Update Management à l’aide du centre d’administration Windows, le centre d’administration Windows vous permet de configurer Azure Update Management uniquement si les ressources existantes liées à la Microsoft Monitoring Agent prennent en charge Azure Update Management. Si ce n’est pas le cas, vous avez deux options :

1. Accédez au panneau de configuration > Microsoft Monitoring Agent pour [déconnecter votre serveur des solutions de gestion Azure existantes](https://docs.microsoft.com/azure/azure-monitor/platform/log-faq#q-how-do-i-stop-an-agent-from-communicating-with-log-analytics) (comme Azure Monitor ou Azure Security Center). Configurez ensuite Azure Update Management dans le centre d’administration Windows. Après cela, vous pouvez revenir à la configuration de vos autres solutions de gestion Azure via le centre d’administration Windows sans problèmes.
2. Vous pouvez [configurer manuellement les ressources Azure nécessaires pour azure Update Management](https://docs.microsoft.com/azure/automation/automation-update-management) , puis [mettre à jour manuellement les Microsoft Monitoring agent](https://docs.microsoft.com/azure/azure-monitor/platform/agent-manage#adding-or-removing-a-workspace) (en dehors du centre d’administration Windows) pour ajouter le nouvel espace de travail correspondant à la solution Update Management que vous souhaitez utiliser.
