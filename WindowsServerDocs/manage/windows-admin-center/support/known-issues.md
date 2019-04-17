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
ms.openlocfilehash: b0159d88251c7f4f6422dffd8f1e1414e1f30f33
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296942"
---
# Problèmes connus de Windows Admin Center

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Si vous rencontrez un problème non décrit dans cette page, veuillez [nous en faire part](http://aka.ms/WACfeedback).

## Lenovo XClarity intégrateur
Il existe une incompatibilité entre une combinaison de la version spécifique de Lenovo XClarity intégrateur et Windows Admin Center. Si vous utilisez ou envisagez d’utiliser l’extension Lenovo XClarity intégrateur d’applications dans Windows Admin Center, voici ce que vous devez savoir:

- Lenovo XClarity intégrateur version 1.0.4 de l’extension est entièrement compatible avec Windows Admin Center 1809.5.
- Lenovo XClarity intégrateur version de l’extension 1.0.4 a exposé un problème de compatibilité sur Windows Admin Center 1904. Les ingénieurs Microsoft et Lenovo sont recherche activement entre eux, et nous espérons fournir une solution dès que possible. Les mises à jour seront publiées sur le site de documentation de Windows Admin Center, et vous pouvez également faire référence à la [page de support de Lenovo](https://support.lenovo.com/solutions/ht507549) à titre de référence.
- Si vous êtes un utilisateur de la fonctionnalité de Lenovo XClarity fréquent, vous pouvez soit rester sur Windows Admin Center 1809.5 pour continuer à utiliser XClarity intégrateur 1.0.4, ou vous pouvez mettre à niveau vers Windows Admin Center 1904 et utiliser séparément autonome XClarity administrateur logiciel en tant qu’une solution de contournement pour le moment.


## Programme d’installation

- Lorsque vous installez Windows Admin Center à l’aide de votre propre certificat, n'oubliez pas que si vous copiez l’empreinte numérique à partir de l’outil MMC du Gestionnaire de certificat, [il contiendra un caractère non valide au début.](https://support.microsoft.com/help/2023835/certificate-thumbprint-displayed-in-mmc-certificate-snap-in-has-extra) Pour résoudre ce problème, tapez le premier caractère de l’empreinte numérique et copiez/collez le reste.

- À l’aide de port inférieurs à 1024 n’est pas pris en charge. En mode de service, vous pouvez éventuellement configurer le port 80 pour rediriger vers votre port spécifié.

- Si le service Windows Update (wuauserv) est arrêté et désactivé, le programme d’installation échouera. [19100629]

### Mettre à niveau/Mise à niveau

- Lors de la mise à niveau Windows Admin Center en mode de service à partir d’une version antérieure, si vous utilisez msiexec en mode silencieux, vous pouvez rencontrer un problème dans lequel la règle de pare-feu de trafic entrant pour le port Windows Admin Center est supprimée.
  - Pour recréer la règle, exécutez la commande suivante à partir d’une console PowerShell avec élévation de privilèges, en remplaçant \<port> par le port configuré pour Windows Admin Center (443 par défaut.)

    ```powershell
    New-NetFirewallRule -DisplayName "SmeInboundOpenException" -Description "Windows Admin Center inbound port exception" -LocalPort <port> -RemoteAddress Any -Protocol TCP
    ```

## Général

- Si vous avez installé sous forme de passerelle sur **Windows Server 2016** sous une utilisation intensive Windows Admin Center, le service peut se bloquer avec une erreur dans le journal des événements qui contient ```Faulting application name: sme.exe``` et ```Faulting module name: WsmSvc.dll```. Cela est dû à un bogue qui a été corrigé dans Windows Server 2019. Le correctif pour Windows Server 2016 a été inclus le 2019 février cumulative mettre à jour, [KB4480977](https://www.catalog.update.microsoft.com/Search.aspx?q=4480977).

- Si vous avez installé sous forme de passerelle Windows Admin Center et votre liste de connexion semble être endommagé, procédez comme suit:

>[!WARNING]
>Cela supprime la liste de connexion et les paramètres pour tous les utilisateurs de Windows Admin Center sur la passerelle.

  1. Désinstaller Windows Admin Center
  2. Supprimez le dossier **Server Management Experience** sous **C:\Windows\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft**
  3. Réinstaller Windows Admin Center

- Si vous laissez l’outil ouvert et inactif pendant une longue période, vous pouvez obtenir plusieurs erreurs **Error: The runspace state is not valid for this operation**. Si cela se produit, actualisez votre navigateur. Si vous rencontrez ce problème, [nous envoyer vos commentaires](http://aka.ms/WACfeedback).

- Vous pouvez rencontrer une **Erreur500** lors de l’actualisation des pages avec des URL très longues. [12443710]

- Dans certains outils, le vérificateur d’orthographe de votre navigateur peut marquer certaines valeurs de champ comme mal orthographiées. [12425477]

- Dans certains outils, les boutons de commande peuvent ne pas refléter les modifications d'état immédiatement après un clic de l’utilisateur et l’interface utilisateur de l’outil ne reflète pas automatiquement les modifications apportées à certaines propriétés. Vous pouvez cliquer sur **Actualiser** pour récupérer l’état le plus récent à partir du serveur cible. [11445790]

- Balise filtrage sur la liste de connexion - si vous sélectionnez des connexions à l’aide de la sélection multiple cases à cocher, puis filtrez votre liste de connexion par balises, la sélection d’origine est conservé afin que toute action que vous sélectionnez s’appliqueront à tous les ordinateurs précédemment sélectionnés. [18099259]

- Il peut exister des variantes mineures entre les numéros de version des systèmes d’exploitation en cours d’exécution dans les modules Windows Admin Center et ce qui est indiqué dans le 3e préalable du logiciels tiers information.

### Gestionnaire d’extensions

- Lorsque vous mettez à jour Windows Admin Center, vous devez réinstaller vos extensions.
- Si vous ajoutez une flux d’extension qui est inaccessible, il n’existe aucun message d’avertissement. [14412861]

## Problèmes spécifiques du navigateur

### Microsoft Edge

- Dans certains cas, vous pouvez constater des temps de chargement longs lorsque vous utilisez Microsoft Edge pour accéder à une passerelle Windows Admin Center via Internet. Cela peut se produire sur les machines virtuelles Azure où la passerelle Windows Admin Center utilise un certificat auto-signé. [13819912]

- Si vous utilisez Azure Active Directory comme fournisseur d’identité et si Windows Admin Center est configuré avec un certificat auto-signé ou autrement non approuvé, vous ne pouvez pas effectuer l’authentification AAD dans Microsoft Edge.  [15968377]

- Si vous avez déployé en tant que service Windows Admin Center et que vous utilisez Microsoft Edge comme votre navigateur, connectez votre passerelle vers Azure peut échouer après la génération d’une nouvelle fenêtre de navigateur. Essayez de contourner ce problème en ajoutant https://login.microsoftonline.com, https://login.live.com, et l’URL de votre passerelle en tant que sites de confiance et sites autorisés pour les paramètres bloqueur de fenêtres contextuelles sur votre navigateur de côté client. Pour plus d’informations sur la correction de cela dans le [guide de résolution des problèmes](troubleshooting.md#azlogin). [17990376]

- Si vous avez installé en mode bureau Windows Admin Center, l’onglet du navigateur dans Microsoft Edge ne sont pas afficher le favorite. [17665801]

### Google Chrome

- Avant la version 70 (relâchée 2018 octobre en retard) Chrome avait un [bogue](https://bugs.chromium.org/p/chromium/issues/detail?id=423609) concernant le protocole WebSocket et l’authentification NTLM. Cela affecte les outils suivants: Événements, PowerShell, Bureau à distance.

- Chrome peut afficher plusieurs demandes d'informations d'identification, en particulier pendant l’expérience d'ajout de connexion dans un environnement de **groupe de travail** (hors domaine).

- Si vous avez déployé en tant que service Windows Admin Center, les fenêtres contextuelles à partir de l’URL de la passerelle doivent être activée pour aucune fonctionnalité d’intégration d’Azure fonctionner. Ces services incluent l’adaptateur réseau Azure, gestion des mises à jour Azure et Azure Site Recovery.

### Mozilla Firefox

Windows Admin Center n’est pas testé avec Mozilla Firefox, mais la plupart des fonctionnalités doivent être opérationnelles.

- Installation de Windows10: Mozilla Firefox possède son propre magasin de certificats, donc vous devez importer le certificat ```Windows Admin Center Client``` dans Firefox pour utiliser Windows Admin Center sur Windows10.

<a id="websockets"></a>

## Compatibilité de WebSocket lors de l’utilisation d’un service de proxy

Les modules Bureau à distance, PowerShell et Événements dans Windows Admin Center utilisent le protocole WebSocket, qui n’est souvent pas pris en charge lors de l’utilisation d’un service de proxy. La compatibilité de la prise en charge de WebSocket dans le proxy d’application Azure AD est en [préversion](https://blogs.technet.microsoft.com/applicationproxyblog/2018/03/28/limited-websocket-support-now-in-public-preview/) et nécessite des commentaires sur la compatibilité.

## Prise en charge des versions de Windows Server antérieures à 2016 (2012 R2, 2012, 2008 R2)

> [!NOTE]
> Windows Admin Center requiert des fonctionnalités de PowerShell qui ne sont pas incluses dans Windows Server 2012 R2, 2012 ou 2008 R2. Si vous comptez gérer Windows Server avec Windows Admin Center, vous devez installer WMF version 5.1 ou ultérieure sur ces serveurs.

Saisissez `$PSVersiontable` dans PowerShell pour vérifier que WMF est installé et que sa version est5.1 ou ultérieure.

S’il n’est pas installé, vous pouvez [télécharger et installer WMF5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616).

<a id="rbacknownissues"></a>

## Contrôle d’accès en fonction du rôle (RBAC)

- Le déploiement de RBAC échoue sur les ordinateurs configurés pour utiliser le Contrôle d'application Windows Defender (WDAC, anciennement appelé Intégrité du Code). [16568455]

- Pour utiliser RBAC dans un cluster, vous devez déployer la configuration sur chaque nœud membre individuellement.

- Lorsque RBAC est déployé, vous pouvez obtenir des erreurs non autorisées qui sont attribuées de manière incorrecte à la configuration de RBAC. [16369238]

## Solution du Gestionnaire de serveur

### Paramètres du serveur

- Si vous modifiez un paramètre, puis essayez de quitter sans l’enregistrer, la page vous signaler les modifications non enregistrées, mais continuez à quitter. Vous pouvez se retrouver dans un état où l’onglet Paramètres sélectionné ne correspond pas le contenu de la page. [19905798] [19905787]

### Certificats

- Impossible d’importer un certificat .PFX chiffré dans le magasin de l’utilisateur actuel. [11818622]

### Appareils

- Lors de la navigation par le biais de la table avec le clavier, la sélection peut aller vers le haut du groupe de tableaux. [16646059]

### Événements

- Les événements sont affectés par la [compatibilité de websocket lors de l’utilisation d’un service de proxy.](#websockets)

- Vous pouvez recevoir une erreur qui fait référence à la «taille de paquet» lors de l’exportation de fichiers journaux volumineux. [16630279]

  - Pour résoudre ce problème, utilisez la commande suivante dans une invite de commandes avec élévation de privilèges sur l’ordinateur de la passerelle: ```winrm set winrm/config @{MaxEnvelopeSizekb="8192"}```

### Fichiers

- Le chargement ou téléchargement de fichiers volumineux n’est pas encore pris en charge. (\~100mb limit) [12524234]

### PowerShell

- PowerShell est affecté par la [compatibilité de websocket lors de l’utilisation d’un service de proxy](#websockets)

- Coller avec un seul clic droit comme dans la console de bureau PowerShell ne fonctionne pas. Au lieu de cela, vous obtiendrez le menu local du navigateur, dans lequel vous pouvez sélectionner coller. CTRL + V fonctionne également.

- CTRL + C pour copier ne fonctionne pas, il enverra toujours la commande de pause Ctrl + C à la console. Copier à partir du menu contextuel (avec un clic droit) fonctionne.

- Lorsque vous réduisez la fenêtre de Windows Admin Center, le contenu du terminal est réorganisé, mais lorsque vous l'agrandissez à nouveau, le contenu ne peut pas retourner à son état précédent. Si le contenu devient confus, essayez la commande Clear-Host, ou déconnectez et reconnectez-vous en utilisant le bouton au-dessus du terminal.

### Éditeur du Registre

- La fonctionnalité de recherche n'est pas implémentée. [13820009]

### Bureau à distance

- L’outil Bureau à distance risque de ne pas se connecter lors de la gestion de Windows Server 2012. [20258278]

- Lorsque vous utilisez le Bureau à distance pour se connecter à un ordinateur qui n’est pas joint au domaine, vous devez entrer votre compte dans le ```MACHINENAME\USERNAME``` format.

- Certaines configurations peuvent bloquer le client Bureau à distance du centre d’administration Windows avec la stratégie de groupe. Si vous rencontrez ce problème, activez ```Allow users to connect remotely by using Remote Desktop Services``` sous ```Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Connections```

- Services Bureau à distance est affecté par [compatibilité de websocket.](#websockets)

- L’outil Bureau à distance ne prend actuellement pas en charge la fonction copier/coller de texte, d'image ou de fichier entre le bureau local et la session à distance.

- Pour tout copier/coller dans la session à distance, vous pouvez copier comme d'habitude (clic droit + copier ou Ctrl + C), mais coller nécessite clic droit + Coller (Ctrl + V NE fonctionne PAS)

- Vous ne pouvez pas envoyer les commandes de touches suivantes à la session à distance
  - Alt+Tabulation
  - Touches de fonction
  - Touche Windows
  - Imp. écr.

- Application à distance: après l’activation de l’outil de l’application à distance à partir des paramètres du Bureau à distance, l’outil apparaissent ne peut-être pas dans la liste de l’outil lors de la gestion d’un serveur avec expérience utilisateur. [18906904]

### Rôles et fonctionnalités

- Lorsque vous sélectionnez des rôles ou des fonctionnalités avec des sources non disponibles pour l’installation, ils sont ignorés. [12946914]

- Si vous choisissez de ne pas redémarrer automatiquement après l’installation du rôle, nous ne vous le redemanderons pas. [13098852]

- Si vous choisissez de redémarrer automatiquement, le redémarrage se produit avant que l’état soit mis à jour à 100%. [13098852]

### Stockage

- L’extraction des informations sur les quotas peut échouer sans une notification d’erreur (il y aura toujours une erreur dans la console du navigateur) [18962274]

- Bas niveau: les lecteurs de CD-ROM/DVD/disquette n’apparaissent pas en tant que volumes au bas niveau.

- Bas niveau: certaines propriétés de Volumes et disques ne sont pas disponibles au bas niveau, donc elles apparaissent comme inconnues ou vides dans le volet d’informations.

- Bas niveau: lorsque vous créez un nouveau volume, ReFS prend uniquement en charge une taille d’unité d’allocation de 64Ko sur les ordinateurs Windows2012 et 2012R2. Si un volume ReFS est créé avec une plus petite taille d’unité d’allocation sur les cibles de bas niveau, le formatage du système de fichiers échoue. Le nouveau volume ne sera pas utilisable. La solution consiste à supprimer le volume et à utiliser la taille d’unité d’allocation de 64Ko.

### Mises à jour

- Après l’installation des mises à jour, état de l’installation peut être mis en cache et nécessitent une actualisation du navigateur.

- Vous pouvez rencontrer l’erreur: «Le jeu de clés n’existe pas» lorsque vous tentez de configurer la gestion de la mise à jour Azure. Dans ce cas, essayez les étapes suivantes de la mise à jour sur le nœud géré-
    1. Arrêtez le service Services de chiffrement.
    2. Modifier les options de dossier pour afficher les fichiers cachés (si nécessaire).
    3. Dossier «% allusersprofile%\Microsoft\Crypto\RSA\S-1-5-18» d’accord et effacer tout son contenu.
    4. Redémarrez le service Services de chiffrement.
    5. Répétez la configuration de la gestion des mises à jour avec Windows Admin Center

### Machines virtuelles

- Lorsque vous gérez les machines virtuelles sur un hôte Windows Server 2012, la machine virtuelle dans le navigateur connecter outil échouera pour se connecter à la machine virtuelle. Téléchargez le fichier .rdp pour se connecter à la machine virtuelle doit continuer à fonctionner. [20258278]

- Azure Site Recovery – si la récupération automatique du système est bien installé sur l’hôte en dehors de WAC, vous ne pourrez pas à protéger un ordinateur virtuel à partir de WAC [18972276]

- Des fonctionnalités avancées disponibles dans le Gestionnaire Hyper-V (telles que le Gestionnaire de réseau SAN virtuel, Déplacer un ordinateur virtuel, Export VM, Réplication de l’ordinateur virtuel) ne sont actuellement pas prises en charge.

### Commutateurs virtuels

- Switch Embedded Teaming (SET): lorsque vous ajoutez des cartes réseau à une équipe, elles doivent être sur le même sous-réseau.

## Solution de gestion de l'ordinateur

La solution de gestion de l’ordinateur contient un sous-ensemble d'outils de la solution du Gestionnaire de serveur, donc les mêmes problèmes connus s’appliquent, ainsi que des problèmes spécifiques de la solution de gestion de l’ordinateur suivants:

- Si vous utilisez un Account Microsoft ([MSA](https://account.microsoft.com/account/)) ou si vous utilisez Azure Active Directory (AAD) pour vous connecter à votre ordinateur Windows 10, vous devez spécifier «gérer-comme «informations d’identification pour gérer votre ordinateur local [16568455]

- Lorsque vous essayez de gérer l’hôte local, vous êtes invité à élever le processus de passerelle. Si vous cliquez sur **non** dans la fenêtre contextuelle du Contrôle de compte d’utilisateur qui suit, Windows Admin Center ne pourra pas s’afficher de nouveau. Dans ce cas, quittez le processus de passerelle en cliquant avec le bouton droit sur l’icône Windows Admin Center dans la barre d’état système et en choisissant Quitter, puis redémarrez Windows Admin Center à partir du Menu Démarrer.

- Windows10 n'a pas la communication à distance WinRM/PowerShell activée par défaut
  
  - Pour activer la gestion du client Windows10, vous devez exécuter la commande ```Enable-PSRemoting``` à partir d’une invite de commandes PowerShell avec élévation de privilèges.

  - Vous devez également mettre à jour votre pare-feu pour autoriser les connexions à partir de l’extérieur du sous-réseau local avec ```Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any```. Pour les scénarios de réseaux plus restrictifs, reportez-vous à [cette documentation](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-5.1).

## Solution du Gestionnaire du cluster de basculement

- Lors de la gestion d’un cluster, (hyperconvergé ou classique?) vous pouvez rencontrer une erreur **shell was not found**. Dans ce cas, rechargez votre navigateur ou quittez la page pour accéder à un autre outil, puis revenez. [13882442]

- Un problème peut se produire lors de la gestion d’un cluster de bas niveau (Windows Server2012 ou 2012R2) qui n’a pas été entièrement configuré. La solution à ce problème est de vous assurer que la fonctionnalité Windows **RSAT-Clustering-PowerShell** a été installée et activée sur **chaque nœud membre** du cluster. Pour le faire avec PowerShell, entrez la commande `Install-WindowsFeature -Name RSAT-Windows-PowerShell` sur tous les nœuds du cluster. [12524664]

- Le Cluster peut devoir être ajouté avec le nom de domaine complet pour être correctement détecté.

- Lors de la connexion à un cluster à l’aide de Windows Admin Center installé sous forme de passerelle et de l'indication d'un nom d’utilisateur/mot de passe explicite pour vous authentifier, vous devez sélectionner **Use these credentials for all connections** pour que les informations d’identification soient disponibles pour interroger les nœuds membres.

## Solution de gestion de cluster hyperconvergé

- Certaines commandes telles que **Drives - Update firmware**, **Servers - Remove** et **Volumes - Open** sont désactivées et actuellement non prises en charge.