---
title: Problèmes connus de Windows Admin Center
description: Problèmes connus de Windows Admin Center (projet Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 04/12/2019
ms.openlocfilehash: 7bf23c5af5620241574864babd07fd852115a450
ms.sourcegitcommit: 39ab8041d166e6817a95417d6aa30bc7abeeef54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66260278"
---
# <a name="windows-admin-center-known-issues"></a>Problèmes connus de Windows Admin Center

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

Si vous rencontrez un problème non décrit dans cette page, veuillez [nous en faire part](http://aka.ms/WACfeedback).

## <a name="lenovo-xclarity-integrator"></a>Intégrateur de Lenovo XClarity
Le problème d’incompatibilité divulgués précédemment de l’extension de Lenovo XClarity intégrateur et Windows Admin Center version 1904 est maintenant résolu par Windows Admin Center version 1904.1. Nous vous recommandons vivement de mettre à jour vers la version la plus récente de Windows Admin Center.

- Lenovo XClarity intégrateur version 1.1 de l’extension est entièrement compatible avec Windows Admin Center 1904.1. Nous vous recommandons vivement de mettre à jour vers la dernière version de Windows Admin Center et l’extension de Lenovo.
- Pour une raison quelconque, vous devez continuer à utiliser Windows Admin Center 1809.5 à l’heure actuelle, vous pouvez utiliser l’intégrateur XClarity 1.0.4 qui seront également disponibles dans le flux d’Extension Windows Admin Center jusqu'à ce que Windows Admin Center 1809.5 est plus pris en charge en fonction de notre [politique de support](../support/index.md).

## <a name="installer"></a>Programme d’installation

- Lorsque vous installez Windows Admin Center à l’aide de votre propre certificat, n'oubliez pas que si vous copiez l’empreinte numérique à partir de l’outil MMC du Gestionnaire de certificat, [il contiendra un caractère non valide au début.](https://support.microsoft.com/help/2023835/certificate-thumbprint-displayed-in-mmc-certificate-snap-in-has-extra) Pour résoudre ce problème, tapez le premier caractère de l’empreinte numérique et copiez/collez le reste.

- À l’aide de port inférieurs à 1024 n’est pas pris en charge. En mode service, vous pouvez éventuellement configurer le port 80 pour rediriger vers votre port spécifié.

- Si le service de mise à jour de Windows (wuauserv) est arrêté et désactivé, le programme d’installation échoue. [19100629]

### <a name="upgrade"></a>Mettre à niveau/Mise à niveau

- Lors de la mise à niveau Windows Admin Center en mode de service à partir d’une version précédente, si vous utilisez msiexec en mode silencieux, vous pouvez rencontrer un problème où la règle de pare-feu entrante pour le port de Windows Admin Center est supprimée.
  - Pour recréer la règle, exécutez la commande suivante à partir d’une console PowerShell avec élévation de privilèges, en remplaçant \<port > avec le port configuré pour Windows Admin Center (443 par défaut.)

    ```powershell
    New-NetFirewallRule -DisplayName "SmeInboundOpenException" -Description "Windows Admin Center inbound port exception" -LocalPort <port> -RemoteAddress Any -Protocol TCP
    ```

## <a name="general"></a>Général

- Si vous avez Windows Admin Center est installé en tant que passerelle sur **Windows Server 2016** sous une utilisation intensive, le service peut se bloquer avec une erreur dans le journal des événements qui contient ```Faulting application name: sme.exe``` et ```Faulting module name: WsmSvc.dll```. Il s’agit d’un bogue a été résolu dans Windows Server 2019. Le correctif logiciel pour Windows Server 2016 a été inclus la mise à jour cumulative de février 2019, [KB4480977](https://www.catalog.update.microsoft.com/Search.aspx?q=4480977).

- Si vous avez Windows Admin Center est installé en tant que passerelle et votre liste de connexions semble être corrompues, procédez comme suit :

>[!WARNING]
>Cette opération supprimera la liste des connexions et les paramètres pour tous les utilisateurs Windows Admin Center sur la passerelle.

  1. Désinstaller Windows Admin Center
  2. Supprimez le dossier **Server Management Experience** sous **C:\Windows\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft**
  3. Réinstaller Windows Admin Center

- Si vous laissez l’outil ouverte et inactif pendant une longue période de temps, vous risquez d’obtenir plusieurs **erreur : L’état de l’instance d’exécution n’est pas valide pour cette opération** erreurs. Si cela se produit, actualisez votre navigateur. Si vous rencontrez ce problème, [envoyez-nous vos commentaires](http://aka.ms/WACfeedback).

- Vous pouvez rencontrer une **Erreur 500** lors de l’actualisation des pages avec des URL très longues. [12443710]

- Dans certains outils, le vérificateur d’orthographe de votre navigateur peut marquer certaines valeurs de champ comme mal orthographiées. [12425477]

- Dans certains outils, les boutons de commande peuvent ne pas refléter les modifications d'état immédiatement après un clic de l’utilisateur et l’interface utilisateur de l’outil ne reflète pas automatiquement les modifications apportées à certaines propriétés. Vous pouvez cliquer sur **Actualiser** pour récupérer l’état le plus récent à partir du serveur cible. [11445790]

- Filtrage de balise sur la liste de connexion - si vous sélectionnez les connexions à l’aide de la sélection multiple cases à cocher, puis filtrez votre liste de connexions par balises, la sélection d’origine conserve aussi une action que vous sélectionnez s’appliquent à tous les ordinateurs sélectionnés précédemment. [18099259]

- Il peut être mineur variance entre les numéros de version des systèmes d’exploitation en cours d’exécution dans les modules Windows Admin Center, et ce qui est indiqué dans l’avis de logiciels tiers 3e.

### <a name="extension-manager"></a>Gestionnaire d’extensions

- Lorsque vous mettez à jour Windows Admin Center, vous devez réinstaller vos extensions.
- Si vous ajoutez une extension de flux qui n’est pas accessible, il est sans avertissement. [14412861]

## <a name="browser-specific-issues"></a>Problèmes spécifiques du navigateur

### <a name="microsoft-edge"></a>Microsoft Edge

- Dans certains cas, vous pouvez constater des temps de chargement longs lorsque vous utilisez Microsoft Edge pour accéder à une passerelle Windows Admin Center via Internet. Cela peut se produire sur les machines virtuelles Azure où la passerelle Windows Admin Center utilise un certificat auto-signé. [13819912]

- Si vous utilisez Azure Active Directory comme fournisseur d’identité et si Windows Admin Center est configuré avec un certificat auto-signé ou autrement non approuvé, vous ne pouvez pas effectuer l’authentification AAD dans Microsoft Edge.  [15968377]

- Si vous avez Windows Admin Center est déployé en tant que service et que vous utilisez Microsoft Edge comme navigateur, connectez votre passerelle vers Azure peut-être échouer après une nouvelle fenêtre de navigateur lors de la génération. Essayer de contourner ce problème en ajoutant https://login.microsoftonline.com, https://login.live.com, et l’URL de votre passerelle en tant que sites de confiance et sites autorisés pour les paramètres de bloqueur de fenêtres publicitaires de votre navigateur de côté client. Pour plus d’informations sur la résolution dans le [guide de dépannage](troubleshooting.md#azlogin). [17990376]

- Si vous avez Windows Admin Center est installé en mode bureau, l’onglet du navigateur dans Microsoft Edge n’affichera pas le favicon. [17665801]

### <a name="google-chrome"></a>Google Chrome

- Avant la version 70 (publiée fin octobre, 2018) Chrome avait un [bogue](https://bugs.chromium.org/p/chromium/issues/detail?id=423609) concernant le protocole websockets et l’authentification NTLM. Cela affecte les outils suivants : Événements, PowerShell, le Bureau à distance.

- Chrome peut afficher plusieurs demandes d'informations d'identification, en particulier pendant l’expérience d'ajout de connexion dans un environnement de **groupe de travail** (hors domaine).

- Si vous avez Windows Admin Center est déployé en tant que service, les menus contextuels de l’URL de passerelle doivent être activés de fonctionnalité d’intégration d’Azure fonctionner. Ces services incluent l’adaptateur de réseau Azure, gestion des mises à jour Azure et Azure Site Recovery.

### <a name="mozilla-firefox"></a>Mozilla Firefox

Windows Admin Center n’est pas testé avec Mozilla Firefox, mais la plupart des fonctionnalités doivent être opérationnelles.

- Installation de Windows 10 : Mozilla Firefox a son propre magasin de certificats afin que vous devez importer le ```Windows Admin Center Client``` certificat dans Firefox à utiliser Windows Admin Center sur Windows 10.

<a id="websockets"></a>

## <a name="websocket-compatibility-when-using-a-proxy-service"></a>Compatibilité WebSocket lors de l’utilisation d’un service de proxy

Les modules Bureau à distance, PowerShell et Événements dans Windows Admin Center utilisent le protocole WebSocket, qui n’est souvent pas pris en charge lors de l’utilisation d’un service de proxy. La compatibilité de la prise en charge de WebSocket dans le proxy d’application Azure AD est en [préversion](https://blogs.technet.microsoft.com/applicationproxyblog/2018/03/28/limited-websocket-support-now-in-public-preview/) et nécessite des commentaires sur la compatibilité.

## <a name="support-for-windows-server-versions-before-2016-2012-r2-2012-2008-r2"></a>Prise en charge pour les versions de Windows Server antérieures 2016 (2012 R2, 2012, 2008 R2)

> [!NOTE]
> Windows Admin Center requiert des fonctionnalités de PowerShell qui ne sont pas incluses dans Windows Server 2012 R2, 2012 ou 2008 R2. Si vous permettra de gérer Windows Server avec Windows Admin Center, vous devrez installer WMF version 5.1 ou ultérieur sur ces serveurs.

Saisissez `$PSVersiontable` dans PowerShell pour vérifier que WMF est installé, et que sa version est 5.1 ou ultérieure.

S’il n’est pas installé, vous pouvez [télécharger et installer WMF 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616).

<a id="rbacknownissues"></a>

## <a name="role-based-access-control-rbac"></a>En fonction du rôle (RBAC) le contrôle d’accès

- Le déploiement de RBAC échoue sur les ordinateurs configurés pour utiliser le Contrôle d'application Windows Defender (WDAC, anciennement appelé Intégrité du Code). [16568455]

- Pour utiliser RBAC dans un cluster, vous devez déployer la configuration sur chaque nœud membre individuellement.

- Lorsque RBAC est déployé, vous pouvez obtenir des erreurs non autorisées qui sont attribuées de manière incorrecte à la configuration de RBAC. [16369238]

## <a name="server-manager-solution"></a>Solution du Gestionnaire de serveur

### <a name="server-settings"></a>Paramètres du serveur

- Si vous modifiez un paramètre, puis essayez de quitter sans enregistrer, la page vous alerter sur les modifications non enregistrées, mais continuer pour la quitter. Vous pouvez vous retrouver dans un état où l’onglet Paramètres sélectionné ne correspond pas au contenu de la page. [19905798] [19905787]

### <a name="certificates"></a>Certificats

- Impossible d’importer un certificat .PFX chiffré dans le magasin de l’utilisateur actuel. [11818622]

### <a name="devices"></a>Appareils

- Lorsque vous naviguez dans la table avec votre clavier, la sélection peut passer à la partie supérieure du groupe de la table. [16646059]

### <a name="events"></a>Événements

- Les événements sont affectés par la [compatibilité de websocket lors de l’utilisation d’un service de proxy.](#websockets)

- Vous pouvez recevoir une erreur qui fait référence à la « taille de paquet » lors de l’exportation de fichiers journaux volumineux. [16630279]

  - Pour résoudre ce problème, utilisez la commande suivante dans une invite de commandes avec élévation de privilèges sur l’ordinateur de passerelle : ```winrm set winrm/config @{MaxEnvelopeSizekb="8192"}```

### <a name="files"></a>Fichiers

- Le chargement ou téléchargement de fichiers volumineux n’est pas encore pris en charge. (\~limite de 100 Mo) [12524234]

### <a name="powershell"></a>PowerShell

- PowerShell est affecté par la [compatibilité de websocket lors de l’utilisation d’un service de proxy](#websockets)

- Coller avec un seul clic droit comme dans la console de bureau PowerShell ne fonctionne pas. Au lieu de cela, vous obtiendrez le menu local du navigateur, dans lequel vous pouvez sélectionner coller. CTRL + V fonctionne également.

- CTRL + C pour copier ne fonctionne pas, il enverra toujours la commande de pause Ctrl + C à la console. Copier à partir du menu contextuel (avec un clic droit) fonctionne.

- Lorsque vous réduisez la fenêtre de Windows Admin Center, le contenu du terminal est réorganisé, mais lorsque vous l'agrandissez à nouveau, le contenu ne peut pas retourner à son état précédent. Si le contenu devient confus, essayez la commande Clear-Host, ou déconnectez et reconnectez-vous en utilisant le bouton au-dessus du terminal.

### <a name="registry-editor"></a>Éditeur du Registre

- La fonctionnalité de recherche n'est pas implémentée. [13820009]

### <a name="remote-desktop"></a>Bureau à distance

- L’outil de bureau à distance peut ne pas se connecter lors de la gestion de Windows Server 2012. [20258278]

- Lorsque vous utilisez le Bureau à distance pour se connecter à un ordinateur qui n’est pas joint à un domaine, vous devez entrer votre compte dans le ```MACHINENAME\USERNAME``` format.

- Certaines configurations peuvent bloquer le client de bureau à distance de Windows Admin Center avec la stratégie de groupe. Si vous rencontrez cette, activer ```Allow users to connect remotely by using Remote Desktop Services``` sous ```Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Connections```

- Bureau à distance n’est affecté par [compatibilité de websocket.](#websockets)

- L’outil Bureau à distance ne prend actuellement pas en charge la fonction copier/coller de texte, d'image ou de fichier entre le bureau local et la session à distance.

- Pour tout copier/coller dans la session à distance, vous pouvez copier comme d'habitude (clic droit + copier ou Ctrl + C), mais coller nécessite clic droit + Coller (Ctrl + V NE fonctionne PAS)

- Vous ne pouvez pas envoyer les commandes de touches suivantes à la session à distance
  - Alt+Tabulation
  - Touches de fonction
  - Touche Windows
  - Imp. écr.

- Application distante – après l’activation de l’outil de l’application à distance à partir des paramètres du Bureau à distance, l’outil peuvent être absentes dans la liste d’outils lors de la gestion d’un serveur avec expérience utilisateur. [18906904]

### <a name="roles-and-features"></a>Rôles et fonctionnalités

- Lorsque vous sélectionnez des rôles ou des fonctionnalités avec des sources non disponibles pour l’installation, ils sont ignorés. [12946914]

- Si vous choisissez de ne pas redémarrer automatiquement après l’installation du rôle, nous ne vous le redemanderons pas. [13098852]

- Si vous choisissez de redémarrer automatiquement, le redémarrage se produit avant que l’état soit mis à jour à 100 %. [13098852]

### <a name="storage"></a>Stockage

- Récupération des informations de Quota peuvent échouer sans une notification d’erreur (il y aura toujours une erreur dans la console du navigateur) [18962274]

- Bas niveau : Les lecteurs de CD/DVD/disquette n’apparaissent pas en tant que volumes de bas niveau.

- Bas niveau : Certaines propriétés dans les disques et Volumes ne sont pas disponibles de bas niveau pour qu’elles apparaissent inconnu ou vide dans le volet de détails.

- Bas niveau : Lorsque vous créez un nouveau volume, ReFS prend uniquement en charge une taille d’unité d’allocation de 64 Ko sur Windows 2012 et 2012 R2 machines. Si un volume ReFS est créé avec une plus petite taille d’unité d’allocation sur les cibles de bas niveau, le formatage du système de fichiers échoue. Le nouveau volume ne sera pas utilisable. La solution consiste à supprimer le volume et à utiliser la taille d’unité d’allocation de 64 Ko.

### <a name="updates"></a>Mises à jour

- Après avoir installé les mises à jour, état de l’installation peuvent être mises en cache et nécessitent une actualisation du navigateur.

- Vous pouvez rencontrer l’erreur : « Le jeu de clés n’existe pas » lorsque vous tentez de configurer la gestion de la mise à jour d’Azure. Dans ce cas, essayez les étapes suivantes de la mise à jour sur le nœud géré-
    1. Arrêter le service Services de chiffrement.
    2. Modifier les options de dossier pour afficher les fichiers cachés (si nécessaire).
    3. Pour accéder à un dossier de « % allusersprofile%\Microsoft\Crypto\RSA\S-1-5-18 » et supprimer tout son contenu.
    4. Redémarrez le service Services de chiffrement.
    5. Répétez le paramétrage de la gestion de mise à jour avec Windows Admin Center

### <a name="virtual-machines"></a>Ordinateurs virtuels

- Lorsque vous gérez les machines virtuelles sur un ordinateur hôte Windows Server 2012, la machine virtuelle dans le navigateur se connecter outil n’arrivera pas à se connecter à la machine virtuelle. Télécharger le fichier .rdp pour se connecter à la machine virtuelle doit continuer à fonctionner. [20258278]

- Azure Site Recovery – si l’ASR est configurée sur l’ordinateur hôte en dehors de WAC, vous ne pourrez protéger une machine virtuelle depuis celle-ci WAC [18972276]

- Des fonctionnalités avancées disponibles dans le Gestionnaire Hyper-V (telles que le Gestionnaire de réseau SAN virtuel, Déplacer un ordinateur virtuel, Export VM, Réplication de l’ordinateur virtuel) ne sont actuellement pas prises en charge.

### <a name="virtual-switches"></a>Commutateurs virtuels

- Commutateur Embedded association () : Lorsque vous ajoutez des cartes réseau à une équipe, ils doivent être sur le même sous-réseau.

## <a name="computer-management-solution"></a>Solution de gestion de l'ordinateur

La solution de gestion de l’ordinateur contient un sous-ensemble d'outils de la solution du Gestionnaire de serveur, donc les mêmes problèmes connus s’appliquent, ainsi que des problèmes spécifiques de la solution de gestion de l’ordinateur suivants :

- Si vous utilisez un Account Microsoft ([MSA](https://account.microsoft.com/account/)) ou si vous utilisez Azure Active Directory (AAD) pour vous connecter à votre machine Windows 10, vous devez spécifier « gérer-en tant que « informations d’identification pour gérer votre ordinateur local [16568455]

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