---
title: Problèmes connus de Windows Admin Center
description: Problèmes connus de Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 06/07/2019
ms.openlocfilehash: b222cd4b97beecd25c14b9f8f39627bf46cb7716
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869538"
---
# <a name="windows-admin-center-known-issues"></a>Problèmes connus de Windows Admin Center

> S’applique à : Windows Admin Center, Windows Admin Center Preview

Si vous rencontrez un problème non décrit dans cette page, veuillez [nous en faire part](http://aka.ms/WACfeedback).

## <a name="lenovo-xclarity-integrator"></a>Intégrateur XClarity Lenovo

Le problème d’incompatibilité précédemment divulgué de l’extension de l’intégrateur XClarity Lenovo et du centre d’administration Windows version 1904 est désormais résolu avec le centre d’administration Windows version 1904,1. Nous vous recommandons vivement de mettre à jour vers la dernière version prise en charge du centre d’administration Windows.

- La version 1,1 de l’extension d’intégrateur Lenovo XClarity est entièrement compatible avec le centre d’administration Windows 1904,1. Nous vous recommandons vivement d’effectuer la mise à jour vers la dernière version du centre d’administration Windows et de l’extension Lenovo.
- Pour une raison quelconque, si vous devez continuer à utiliser le centre d’administration Windows 1809,5 pour le moment, vous pouvez utiliser XClarity Integrator 1.0.4 qui sera également disponible dans le flux d’extension du centre d’administration Windows jusqu’à ce que le centre d’administration Windows 1809,5 ne soit plus pris en charge en fonction de notre [stratégie de support](../support/index.md).

## <a name="installer"></a>Programme d’installation

- Lorsque vous installez Windows Admin Center à l’aide de votre propre certificat, n'oubliez pas que si vous copiez l’empreinte numérique à partir de l’outil MMC du Gestionnaire de certificat, [il contiendra un caractère non valide au début.](https://support.microsoft.com/help/2023835/certificate-thumbprint-displayed-in-mmc-certificate-snap-in-has-extra) Pour résoudre ce problème, tapez le premier caractère de l’empreinte numérique et copiez/collez le reste.

- L’utilisation du port inférieur à 1024 n’est pas prise en charge. En mode de service, vous pouvez éventuellement configurer le port 80 pour rediriger vers le port spécifié.

- Si le service Windows Update (wuauserv) est arrêté et désactivé, le programme d’installation échoue. [19100629]

### <a name="upgrade"></a>Mise à niveau

- Lors de la mise à niveau du centre d’administration Windows en mode de service à partir d’une version antérieure, si vous utilisez msiexec en mode silencieux, vous risquez de rencontrer un problème où la règle de pare-feu entrante pour le port du centre d’administration Windows est supprimée.
  - Pour recréer la règle, exécutez la commande suivante à partir d’une console PowerShell avec élévation \<de privilèges, en remplaçant le port > par le port configuré pour le centre d’administration Windows (par défaut 443).

    ```powershell
    New-NetFirewallRule -DisplayName "SmeInboundOpenException" -Description "Windows Admin Center inbound port exception" -LocalPort <port> -RemoteAddress Any -Protocol TCP
    ```

## <a name="general"></a>Généralités

- Si le centre d’administration Windows est installé en tant que passerelle sur **Windows Server 2016** en cas d’utilisation intensive, le service peut se bloquer avec une erreur dans ```Faulting application name: sme.exe``` le ```Faulting module name: WsmSvc.dll```journal des événements qui contient et. Cela est dû à un bogue qui a été corrigé dans Windows Server 2019. Le correctif pour Windows Server 2016 a été inclus dans la mise à jour cumulative du 2019 du 1er février, [KB4480977](https://www.catalog.update.microsoft.com/Search.aspx?q=4480977).

- Si vous avez installé le centre d’administration Windows en tant que passerelle et que votre liste de connexions semble endommagée, procédez comme suit :

   > [!WARNING]
   >Cette opération supprime la liste des connexions et les paramètres de tous les utilisateurs du centre d’administration Windows sur la passerelle.

  1. Désinstaller Windows Admin Center
  2. Supprimez le dossier **Server Management Experience** sous **C:\Windows\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft**
  3. Réinstaller Windows Admin Center

- Si vous laissez l’outil ouvert et inactif pendant une longue période de temps, vous risquez **de recevoir plusieurs erreurs : L’état de l’instance d’exécution n’est** pas valide pour cette opération. Si cela se produit, actualisez votre navigateur. Si vous rencontrez ce problème, [envoyez-nous vos commentaires](http://aka.ms/WACfeedback).

- Vous pouvez rencontrer une **Erreur 500** lors de l’actualisation des pages avec des URL très longues. [12443710]

- Dans certains outils, le vérificateur d’orthographe de votre navigateur peut marquer certaines valeurs de champ comme mal orthographiées. [12425477]

- Dans certains outils, les boutons de commande peuvent ne pas refléter les modifications d'état immédiatement après un clic de l’utilisateur et l’interface utilisateur de l’outil ne reflète pas automatiquement les modifications apportées à certaines propriétés. Vous pouvez cliquer sur **Actualiser** pour récupérer l’état le plus récent à partir du serveur cible. [11445790]

- Filtrage des balises sur la liste des connexions : Si vous sélectionnez connexions à l’aide des cases à cocher multisélections, puis filtrez votre liste de connexions par balises, la sélection d’origine est conservée, de sorte que toute action sélectionnée s’appliquera à tous les ordinateurs sélectionnés précédemment. [18099259]

- Il peut y avoir une variance mineure entre les numéros de version des systèmes d’exploitation en cours d’exécution dans les modules du centre d’administration Windows et ce qui est indiqué dans l’avis du logiciel tiers.

### <a name="extension-manager"></a>Gestionnaire d’extensions

- Lorsque vous mettez à jour le centre d’administration Windows, vous devez réinstaller vos extensions.
- Si vous ajoutez un flux d’extension qui n’est pas accessible, aucun avertissement n’est présent. [14412861]

## <a name="browser-specific-issues"></a>Problèmes spécifiques du navigateur

### <a name="microsoft-edge"></a>Microsoft Edge

- Dans certains cas, vous pouvez constater des temps de chargement longs lorsque vous utilisez Microsoft Edge pour accéder à une passerelle Windows Admin Center via Internet. Cela peut se produire sur les machines virtuelles Azure où la passerelle Windows Admin Center utilise un certificat auto-signé. [13819912]

- Si vous utilisez Azure Active Directory comme fournisseur d’identité et si Windows Admin Center est configuré avec un certificat auto-signé ou autrement non approuvé, vous ne pouvez pas effectuer l’authentification AAD dans Microsoft Edge.  [15968377]

- Si vous avez déployé le centre d’administration Windows en tant que service et que vous utilisez Microsoft Edge comme navigateur, la connexion de votre passerelle à Azure peut échouer après la génération d’une nouvelle fenêtre de navigateur. Essayez de contourner ce problème en ajoutant https://login.microsoftonline.com , https://login.live.com et l’URL de votre passerelle en tant que sites de confiance et sites autorisés pour les paramètres du bloqueur de fenêtres publicitaires sur votre navigateur côté client. Pour plus d’informations sur la résolution de ce problème, dans le [Guide de résolution des problèmes](troubleshooting.md#azure-features-dont-work-properly-in-edge). [17990376]

- Si le centre d’administration Windows est installé en mode Bureau, l’onglet navigateur de Microsoft Edge n’affiche pas le favicon. [17665801]

### <a name="google-chrome"></a>Google Chrome

- Avant la version 70 (publiée fin octobre, 2018) chrome présentait un [bogue](https://bugs.chromium.org/p/chromium/issues/detail?id=423609) concernant le protocole WebSocket et l’authentification NTLM. Cela affecte les outils suivants : Événements, PowerShell Bureau à distance.

- Chrome peut afficher plusieurs demandes d'informations d'identification, en particulier pendant l’expérience d'ajout de connexion dans un environnement de **groupe de travail** (hors domaine).

- Si vous avez déployé le centre d’administration Windows en tant que service, les fenêtres contextuelles de l’URL de la passerelle doivent être activées pour que toutes les fonctionnalités d’intégration Azure fonctionnent. Ces services incluent la carte réseau Azure, Azure Update Management et Azure Site Recovery.

### <a name="mozilla-firefox"></a>Mozilla Firefox

Windows Admin Center n’est pas testé avec Mozilla Firefox, mais la plupart des fonctionnalités doivent être opérationnelles.

- Installation de Windows 10 : Mozilla Firefox possède son propre magasin de certificats. vous devez donc importer le ```Windows Admin Center Client``` certificat dans Firefox pour utiliser le centre d’administration Windows sur Windows 10.

## <a name="websocket-compatibility-when-using-a-proxy-service"></a>Compatibilité WebSocket lors de l’utilisation d’un service proxy

Les modules Bureau à distance, PowerShell et Événements dans Windows Admin Center utilisent le protocole WebSocket, qui n’est souvent pas pris en charge lors de l’utilisation d’un service de proxy. La compatibilité de la prise en charge de WebSocket dans le proxy d’application Azure AD est en [préversion](https://blogs.technet.microsoft.com/applicationproxyblog/2018/03/28/limited-websocket-support-now-in-public-preview/) et nécessite des commentaires sur la compatibilité.

## <a name="support-for-windows-server-versions-before-2016-2012-r2-2012-2008-r2"></a>Prise en charge des versions de Windows Server antérieures à 2016 (2012 R2, 2012, 2008 R2)

> [!NOTE]
> Le centre d’administration Windows requiert des fonctionnalités PowerShell qui ne sont pas incluses dans Windows Server 2012 R2, 2012 ou 2008 R2. Si vous souhaitez gérer Windows Server à l’aide du centre d’administration Windows, vous devez installer WMF version 5,1 ou ultérieure sur ces serveurs.

Saisissez `$PSVersiontable` dans PowerShell pour vérifier que WMF est installé, et que sa version est 5.1 ou ultérieure.

S’il n’est pas installé, vous pouvez [télécharger et installer WMF 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616).

## <a name="role-based-access-control-rbac"></a>Access Control basée sur les rôles (RBAC)

- Le déploiement de RBAC échoue sur les ordinateurs configurés pour utiliser le Contrôle d'application Windows Defender (WDAC, anciennement appelé Intégrité du Code). [16568455]

- Pour utiliser RBAC dans un cluster, vous devez déployer la configuration sur chaque nœud membre individuellement.

- Lorsque RBAC est déployé, vous pouvez obtenir des erreurs non autorisées qui sont attribuées de manière incorrecte à la configuration de RBAC. [16369238]

## <a name="server-manager-solution"></a>Solution du Gestionnaire de serveur

### <a name="server-settings"></a>Paramètres du serveur

- Si vous modifiez un paramètre, puis essayez de quitter sans l’enregistrer, la page vous avertit des modifications non enregistrées, mais continue à quitter. Vous pouvez vous retrouver dans un État où l’onglet Paramètres sélectionné ne correspond pas au contenu de la page. [19905798] [19905787]

### <a name="certificates"></a>Certificats

- Impossible d’importer un certificat .PFX chiffré dans le magasin de l’utilisateur actuel. [11818622]

### <a name="devices"></a>Appareils

- Lorsque vous parcourez la table à l’aide de votre clavier, la sélection peut accéder au haut du groupe de tables. [16646059]

### <a name="events"></a>Events

- Les événements sont affectés par la [compatibilité de websocket lors de l’utilisation d’un service de proxy.](#websocket-compatibility-when-using-a-proxy-service)

- Vous pouvez recevoir une erreur qui fait référence à la « taille de paquet » lors de l’exportation de fichiers journaux volumineux. [16630279]

  - Pour résoudre ce cas, utilisez la commande suivante dans une invite de commandes avec élévation de privilèges sur l’ordinateur passerelle :```winrm set winrm/config @{MaxEnvelopeSizekb="8192"}```

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

- L’outil Bureau à distance peut ne pas réussir à se connecter lors de la gestion de Windows Server 2012. [20258278]

- Lorsque vous utilisez la Bureau à distance pour vous connecter à un ordinateur qui n’est pas joint à un domaine, vous devez ```MACHINENAME\USERNAME``` entrer votre compte au format.

- Certaines configurations peuvent bloquer le client Bureau à distance du centre d’administration Windows avec la stratégie de groupe. Si vous rencontrez ce problème, ```Allow users to connect remotely by using Remote Desktop Services``` activez sous```Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Connections```

- La Bureau à distance est appliquée par la [compatibilité WebSocket.](#websocket-compatibility-when-using-a-proxy-service)

- L’outil Bureau à distance ne prend actuellement pas en charge la fonction copier/coller de texte, d'image ou de fichier entre le bureau local et la session à distance.

- Pour tout copier/coller dans la session à distance, vous pouvez copier comme d'habitude (clic droit + copier ou Ctrl + C), mais coller nécessite clic droit + Coller (Ctrl + V NE fonctionne PAS)

- Vous ne pouvez pas envoyer les commandes de touches suivantes à la session à distance
  - Alt+Tabulation
  - Touches de fonction
  - Touche Windows
  - Imp. écr.

- Application distante : après avoir activé l’outil Remote App à partir de Bureau à distance paramètres, l’outil peut ne pas apparaître dans la liste des outils lors de la gestion d’un serveur avec expérience utilisateur. [18906904]

### <a name="roles-and-features"></a>Rôles et fonctionnalités

- Lorsque vous sélectionnez des rôles ou des fonctionnalités avec des sources non disponibles pour l’installation, ils sont ignorés. [12946914]

- Si vous choisissez de ne pas redémarrer automatiquement après l’installation du rôle, nous ne vous le redemanderons pas. [13098852]

- Si vous choisissez de redémarrer automatiquement, le redémarrage se produit avant que l’état soit mis à jour à 100 %. [13098852]

### <a name="storage"></a>Stockage

- La récupération des informations de quota peut échouer sans notification d’erreur (il y aura toujours une erreur dans la console du navigateur) [18962274]

- De niveau supérieur : Les lecteurs de DVD/CD/disquettes n’apparaissent pas en tant que volumes sur le niveau de défaillance.

- De niveau supérieur : Certaines propriétés des volumes et des disques ne sont pas disponibles de niveau inférieure, de sorte qu’elles apparaissent inconnues ou vides dans le panneau détails.

- De niveau supérieur : Lors de la création d’un nouveau volume, ReFS ne prend en charge qu’une taille d’unité d’allocation de 64 Ko sur les ordinateurs Windows 2012 et 2012 R2. Si un volume ReFS est créé avec une plus petite taille d’unité d’allocation sur les cibles de bas niveau, le formatage du système de fichiers échoue. Le nouveau volume ne sera pas utilisable. La solution consiste à supprimer le volume et à utiliser la taille d’unité d’allocation de 64 Ko.

### <a name="updates"></a>Mises à jour

- Après l’installation des mises à jour, l’état de l’installation peut être mis en cache et nécessiter une actualisation du navigateur.

- Vous pouvez rencontrer l’erreur suivante : « Le jeu de clés n’existe pas » lors de la tentative de configuration de la gestion des mises à jour Azure. Dans ce cas, essayez les étapes de mise à jour suivantes sur le nœud géré-
    1. Arrêtez le service « services de chiffrement ».
    2. Modifiez les options des dossiers pour afficher les fichiers masqués (si nécessaire).
    3. A obtenu le dossier « %allusersprofile%\Microsoft\Crypto\RSA\S-1-5-18 » et a supprimé tout son contenu.
    4. Redémarrez le service « services de chiffrement ».
    5. Répéter la configuration de Update Management à l’aide du centre d’administration Windows

### <a name="virtual-machines"></a>Virtual Machines

- Lors de la gestion des ordinateurs virtuels sur un ordinateur hôte Windows Server 2012, l’outil connexion à un ordinateur virtuel dans le navigateur ne parvient pas à se connecter à la machine virtuelle. Le téléchargement du fichier. RDP pour se connecter à la machine virtuelle doit continuer à fonctionner. [20258278]

- Azure Site Recovery : si la récupération automatique du système est configurée sur l’hôte en dehors de WAC, vous ne pourrez pas protéger une machine virtuelle à partir de WAC [18972276]

- Des fonctionnalités avancées disponibles dans le Gestionnaire Hyper-V (telles que le Gestionnaire de réseau SAN virtuel, Déplacer un ordinateur virtuel, Export VM, Réplication de l’ordinateur virtuel) ne sont actuellement pas prises en charge.

### <a name="virtual-switches"></a>Commutateurs virtuels

- Switch Embedded Teaming (SET) : Lorsque vous ajoutez des cartes réseau à une équipe, celles-ci doivent se trouver sur le même sous-réseau.

## <a name="computer-management-solution"></a>Solution de gestion de l'ordinateur

La solution de gestion de l’ordinateur contient un sous-ensemble d'outils de la solution du Gestionnaire de serveur, donc les mêmes problèmes connus s’appliquent, ainsi que des problèmes spécifiques de la solution de gestion de l’ordinateur suivants :

- Si vous utilisez un compte Microsoft ([MSA](https://account.microsoft.com/account/)) ou si vous utilisez Azure Active Directory (AAD) pour vous connecter à votre ordinateur Windows 10, vous devez spécifier les informations d’identification « gérer en tant que » pour gérer votre ordinateur local [16568455]

- Lorsque vous essayez de gérer l’hôte local, vous êtes invité à élever le processus de passerelle. Si vous cliquez sur **non** dans la fenêtre contextuelle du Contrôle de compte d’utilisateur qui suit, Windows Admin Center ne pourra pas s’afficher de nouveau. Dans ce cas, quittez le processus de passerelle en cliquant avec le bouton droit sur l’icône Windows Admin Center dans la barre d’état système et en choisissant Quitter, puis redémarrez Windows Admin Center à partir du Menu Démarrer.

- Windows 10 n'a pas la communication à distance WinRM/PowerShell activée par défaut
  
  - Pour activer la gestion du client Windows 10, vous devez exécuter la commande ```Enable-PSRemoting``` à partir d’une invite de commandes PowerShell avec élévation de privilèges.

  - Vous devez également mettre à jour votre pare-feu pour autoriser les connexions à partir de l’extérieur du sous-réseau local avec ```Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any```. Pour les scénarios de réseaux plus restrictifs, reportez-vous à [cette documentation](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-5.1).

## <a name="failover-cluster-manager-solution"></a>Solution du Gestionnaire du cluster de basculement

- Lors de la gestion d’un cluster, (hyperconvergé ou classique ?) vous pouvez rencontrer une erreur **shell was not found**. Dans ce cas, rechargez votre navigateur ou quittez la page pour accéder à un autre outil, puis revenez. [13882442]

- Un problème peut se produire lors de la gestion d’un cluster de bas niveau (Windows Server 2012 ou 2012 R2) qui n’a pas été entièrement configuré. La solution à ce problème est de vous assurer que la fonctionnalité Windows **RSAT-Clustering-PowerShell** a été installée et activée sur **chaque nœud membre** du cluster. Pour le faire avec PowerShell, entrez la commande `Install-WindowsFeature -Name RSAT-Windows-PowerShell` sur tous les nœuds du cluster. [12524664]

- Le Cluster peut devoir être ajouté avec le nom de domaine complet pour être correctement détecté.

- Lors de la connexion à un cluster à l’aide de Windows Admin Center installé sous forme de passerelle et de l'indication d'un nom d’utilisateur/mot de passe explicite pour vous authentifier, vous devez sélectionner **Use these credentials for all connections** pour que les informations d’identification soient disponibles pour interroger les nœuds membres.

## <a name="hyper-converged-cluster-manager-solution"></a>Solution de gestion de cluster hyperconvergé

- Certaines commandes telles que **Drives - Update firmware**, **Servers - Remove** et **Volumes - Open** sont désactivées et actuellement non prises en charge.