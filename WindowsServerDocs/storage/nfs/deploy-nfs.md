---
title: Déployer le système de gestion de fichiers en réseau
description: Décrit comment déployer le système de fichiers réseau.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 2f3671283720d515cd3e3e609d98e02343c15892
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860770"
---
# <a name="deploy-network-file-system"></a>Déployer le système de gestion de fichiers en réseau

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Système NFS (Network File) fournit une solution qui vous permet de transférer des fichiers entre ordinateurs exécutant Windows Server et les systèmes d’exploitation UNIX utilisant le protocole NFS de partage de fichiers. Cette rubrique décrit les étapes à suivre pour déployer NFS.

## <a name="whats-new-in-network-file-system"></a>Nouveautés du système de fichiers réseau

Voici ce qui a changé pour NFS dans Windows Server 2012 :

- **Prise en charge de NFS version 4.1**. Cette version de protocole inclut les améliorations suivantes.
  - Il est plus facile, améliorer l’accessibilité de navigation dans les pare-feux.
  - Prend en charge le RPCSEC\_protocole GSS, fournissant une sécurité renforcée et permettant aux clients et serveurs négocier la sécurité.
  - Prend en charge les sémantiques de fichiers UNIX et Windows.
  - Tire parti des déploiements de serveurs de fichiers en cluster.
  - Prend en charge les procédures composées WAN conviviale.

- **Module NFS pour Windows PowerShell**. La disponibilité des applets de commande NFS intégrées facilite l’automatiser de nombreuses opérations. Les noms d’applet de commande sont cohérents avec d’autres cmdlets Windows PowerShell (à l’aide de verbes tels que « Get » et « Set »), ce qui facilite pour les utilisateurs familiarisés avec PowerShell de Windows pour apprendre à utiliser les nouvelles applets de commande.
- **Amélioration de la gestion NFS**. Une nouvelle console de gestion centralisée via l’interface utilisateur simplifie la configuration et la gestion de SMB et les partages NFS, les quotas, les filtres de fichiers et les classification, outre la gestion des serveurs de fichiers en cluster.
- **Identité de mappage des améliorations**. Nouvelle prise en charge de l’interface utilisateur et sur les tâches d’applets de commande Windows PowerShell pour configurer le mappage d’identité, ce qui permet aux administrateurs de rapidement configurer une source de mappage d’identité et ensuite créer des identités mappées pour les utilisateurs. Améliorations facilitent pour les administrateurs à configurer un partage pour un accès multiprotocole via NFS et SMB.
- **Restructuration de modèle de ressource de cluster**. Cette amélioration apporte la cohérence entre le modèle de ressource de cluster pour les NFS de Windows et les serveurs de protocole SMB et simplifie l’administration. Pour les serveurs NFS qui ont de nombreux partages, le réseau de la ressource et le nombre d’appels WMI requis basculent sur un volume contenant qu'un grand nombre de partages NFS est réduit.
- **Intégration avec le Gestionnaire de clés Resume**. Le Gestionnaire de clés de reprise est un composant qui effectue le suivi de serveur de fichiers et l’état de système de fichiers et permet aux serveurs de protocole Windows SMB et NFS effectuer le basculement sans interrompre les clients ou les applications serveur qui stockent leurs données sur le serveur de fichiers. Cette amélioration est un composant clé de la fonctionnalité de disponibilité continue du serveur de fichiers exécutant Windows Server 2012.

## <a name="scenarios-for-using-network-file-system"></a>Scénarios d’utilisation du système de fichiers réseau

NFS prend en charge un environnement mixte de systèmes d’exploitation Windows et UNIX. Les scénarios de déploiement suivants sont des exemples de la façon dont vous pouvez déployer un serveur de fichiers Windows Server 2012 disponible en continu à l’aide de NFS.

### <a name="provision-file-shares-in-heterogeneous-environments"></a>Partages de fichiers de configuration dans des environnements hétérogènes

Ce scénario s’applique aux organisations avec des environnements hétérogènes constitués de Windows et d’autres systèmes d’exploitation, tels que le client Linux ou UNIX ordinateurs. Avec ce scénario, vous pouvez fournir des accès multiprotocole au même partage de fichiers via des protocoles SMB et NFS. En règle générale, lorsque vous déployez un serveur de fichiers Windows dans ce scénario, vous souhaitez faciliter la collaboration entre les utilisateurs sur Windows et les ordinateurs basés sur UNIX. Lorsqu’un partage de fichiers est configuré, il est partagé avec les protocoles SMB et NFS, les utilisateurs Windows l’accès à leurs fichiers sur le protocole SMB, et les utilisateurs d’ordinateurs basés sur UNIX accèdent généralement au leurs fichiers via le protocole NFS.

Pour ce scénario, vous devez disposer d’une configuration de source de mappage identité valide. Windows Server 2012 prend en charge les magasins de mappage d’identité suivants :

- Fichier de mappage
- Services de domaine Active Directory (AD DS)
- LDAP 2307 conforme RFC stocke telles que Active Directory Lightweight Directory Services (AD LDS)
- Serveur de mappage de nom (UNM) d’utilisateur

### <a name="provision-file-shares-in-unix-based-environments"></a>Partages de fichiers de configuration dans les environnements UNIX

Dans ce scénario, les serveurs de fichiers Windows sont déployés dans un environnement principalement basés sur UNIX pour fournir l’accès aux partages de fichiers NFS pour les ordinateurs clients basés sur UNIX. Une option d’accès d’utilisateur non mappé UNIX (UUUA) a été initialement implémentée pour les partages NFS dans Windows Server 2008 R2 afin que le mappage de compte Windows de serveurs peuvent être utilisés pour stocker des données NFS sans créer d’UNIX vers Windows. UUUA permet aux administrateurs de rapidement configurer et déployer NFS sans avoir à configurer le mappage de compte. Lorsque activé pour NFS, UUUA crée des identificateurs de sécurité personnalisé (SID) pour représenter des utilisateurs non mappés. Comptes d’utilisateurs mappés utilisent des identificateurs de sécurité standards Windows (SID), et des utilisateurs non mappés utiliser personnalisé SID NFS.

## <a name="system-requirements"></a>Configuration requise

Serveur pour NFS peut être installé sur n’importe quelle version de Windows Server 2012. Vous pouvez utiliser NFS avec les ordinateurs basés sur UNIX qui exécutent un serveur NFS ou le client NFS si ces implémentations de serveur et client NFS sont conformes avec l’une des spécifications de protocole suivantes :

1. Spécification du protocole NFS Version 4.1 (tel que défini dans RFC [5661](https://tools.ietf.org/html/rfc5661))
2. Spécification du protocole NFS Version 3 (tel que défini dans RFC [1813](https://tools.ietf.org/html/rfc1813))
3. Spécification du protocole NFS Version 2 (tel que défini dans RFC [1094](https://tools.ietf.org/html/rfc1094))

## <a name="deploy-nfs-infrastructure"></a>Déployer une infrastructure NFS

Vous avez besoin déployer les ordinateurs suivants et de les connecter sur un réseau local (LAN) :

- Un ou plusieurs ordinateurs exécutant Windows Server 2012 sur lequel vous allez installer les deux Services principaux pour les composants NFS : Serveur pour NFS et Client pour NFS. Vous pouvez installer ces composants sur le même ordinateur ou sur des ordinateurs différents.
- Un ou plusieurs ordinateurs UNIX qui exécutent le serveur NFS et le logiciel client NFS. L’ordinateur UNIX qui exécute le serveur NFS héberge un partage de fichiers NFS ou d’exportation, qui est accessible par un ordinateur qui exécute Windows Server 2012 en tant que client à l’aide du Client pour NFS. Dans le même ordinateur UNIX ou sur différents ordinateurs basés sur UNIX, comme vous le souhaitez, vous pouvez installer le serveur NFS et le logiciel client.
- Un contrôleur de domaine en cours d’exécution au niveau fonctionnel Windows Server 2008 R2. Le contrôleur de domaine fournit des informations d’authentification utilisateur et de mappage pour l’environnement Windows.
- Quand un contrôleur de domaine n’est pas déployé, vous pouvez utiliser un serveur Service NIS (Network Information) pour fournir les informations d’identification utilisateur pour l’environnement UNIX. Ou, si vous préférez, vous pouvez utiliser le mot de passe et le groupe de fichiers qui sont stockés sur l’ordinateur qui exécute le service de mappage de noms d’utilisateur.

### <a name="install-network-file-system-on-the-server-with-server-manager"></a>Installer le système de fichiers réseau sur le serveur avec le Gestionnaire de serveur

1. Dans l’Assistant Ajout de rôles et fonctionnalités, sous Rôles de serveurs, sélectionnez **Services de fichiers et de stockage** si ce composant n’a pas déjà été installé.
2. Sous **fichiers et iSCSI Services**, sélectionnez **serveur de fichiers** et **serveur pour NFS**. Sélectionnez **ajouter des fonctionnalités** pour inclure des fonctionnalités NFS sélectionnées.
3. Sélectionnez **installer** pour installer les composants NFS sur le serveur.

### <a name="install-network-file-system-on-the-server-with-windows-powershell"></a>Installer le système de fichiers réseau sur le serveur avec Windows PowerShell

1. Démarrez Windows PowerShell. Cliquez avec le bouton droit sur l’icône PowerShell située dans la barre des tâches, puis sélectionnez **Exécuter en tant qu’administrateur**.
2. Exécutez les commandes Windows PowerShell suivantes :

```PowerShell
Import-Module ServerManager
Add-WindowsFeature FS-NFS-Service
Import-Module NFS
```

## <a name="configure-nfs-authentication"></a>Configurer l’authentification NFS

Lorsque vous utilisez NFS version 4.1 et des protocoles NFS version 3.0, vous disposez des options d’authentification et de sécurité suivantes.

- RPCSEC\_GSS
  - **Krb5**. Utilise le protocole Kerberos version 5 pour authentifier les utilisateurs avant d’accorder l’accès au partage de fichiers.
  - **Krb5i**. Utilise le protocole Kerberos version 5 pour s’authentifier auprès de l’intégrité de la vérification (sommes de contrôle), qui vérifie que les données n’a pas été modifiées.
  - **Krb5p** protocole utilise le Kerberos version 5, qui authentifie le trafic NFS avec chiffrement pour la confidentialité.
- AUTHENTIFICATION\_SYS

Vous pouvez également choisir ne pas d’utiliser l’autorisation du serveur (AUTH\_SYS), ce qui vous donne la possibilité d’activer l’accès des utilisateurs non mappés. Lorsque vous utilisez l’accès utilisateur non mappé, vous pouvez spécifier pour autoriser l’accès des utilisateurs non mappés par UID / GID, qui est la valeur par défaut, ou autoriser l’accès anonyme.

Instructions pour configurer l’authentification NFS sur abordées dans la section suivante.

## <a name="create-an-nfs-file-share"></a>Créer un partage de fichiers NFS

Vous pouvez créer un partage de fichiers NFS à l’aide des applets de commande Gestionnaire de serveur ou Windows PowerShell NFS.

### <a name="create-an-nfs-file-share-with-server-manager"></a>Créer un partage de fichiers NFS avec le Gestionnaire de serveur

1. Ouvrez une session sur le serveur en tant que membre du groupe Administrateurs local.
2. Le Gestionnaire de serveur démarre automatiquement. S’il ne démarre pas automatiquement, sélectionnez **Démarrer**, type **servermanager.exe**, puis sélectionnez **le Gestionnaire de serveur**.
3. Sur la gauche, sélectionnez **File and Storage Services**, puis sélectionnez **partages**.
4. Sélectionnez **pour créer un partage de fichiers, démarrez l’Assistant Nouveau partage**.
5. Sur le **sélectionner un profil** page, sélectionnez soit **partage NFS – rapide** ou **partage NFS - avancé**, puis sélectionnez **suivant**.
6. Sur le **emplacement du partage** page, sélectionnez un serveur et un volume, puis **suivant**.
7. Sur le **nom du partage** page, spécifiez un nom pour le nouveau partage, puis sélectionnez **suivant**.
8. Sur le **authentification** , spécifiez la méthode d’authentification que vous souhaitez utiliser pour ce partage.
9. Sur le **partager des autorisations** page, sélectionnez **ajouter**, puis spécifiez l’hôte, le groupe de client ou le groupe réseau que vous souhaitez accorder l’autorisation pour le partage.
10. Dans **autorisations**, configurer le type de contrôle d’accès vous souhaitez que les utilisateurs ont, puis sélectionnez **OK**.
11. Sur le **Confirmation** page, passez en revue votre configuration, puis sélectionnez **créer** pour créer le partage de fichiers NFS.

### <a name="windows-powershell-equivalent-commands"></a>Commandes Windows PowerShell équivalentes

L’applet de commande Windows PowerShell suivant peut également créer un partage de fichiers NFS (où `nfs1` est le nom du partage et `C:\\shares\\nfsfolder` est le chemin d’accès de fichier) :

```PowerShell
New-NfsShare -name nfs1 -Path C:\shares\nfsfolder
```

### <a name="known-issue"></a>Problème connu
NFS version 4.1 permet les noms de fichiers soit créé ou copié à l’aide de caractères non conformes. Si vous essayez d’ouvrir les fichiers avec l’éditeur vi, il montre comme étant endommagée. Impossible d’enregistrer le fichier à partir de vi, renommer, déplacer ou modifier les autorisations. Évitez d’utiliser des caractères d’illigal.
