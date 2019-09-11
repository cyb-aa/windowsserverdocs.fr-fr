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
ms.openlocfilehash: 761394f3190f02eedfea27a7d873c4c36535f23b
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865118"
---
# <a name="deploy-network-file-system"></a>Déployer le système de gestion de fichiers en réseau

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le système NFS (Network File System) fournit une solution de partage de fichiers qui vous permet de transférer des fichiers entre des ordinateurs exécutant des systèmes d’exploitation Windows Server et UNIX à l’aide du protocole NFS. Cette rubrique décrit les étapes à suivre pour déployer NFS.

## <a name="whats-new-in-network-file-system"></a>Nouveautés du système de fichiers réseau

Voici les modifications apportées à NFS dans Windows Server 2012 :

- **Prise en charge de NFS version 4,1**. Cette version de protocole comprend les améliorations suivantes.
  - La navigation dans les pare-feu est plus facile, ce qui améliore l’accessibilité.
  - Prend en charge\_le protocole GSS RpcSec, offrant une sécurité renforcée et permettant aux clients et aux serveurs de négocier la sécurité.
  - Prend en charge la sémantique des fichiers UNIX et Windows.
  - Tire parti des déploiements de serveurs de fichiers en cluster.
  - Prend en charge les procédures composées compatibles avec les réseaux étendus.

- **Module NFS pour Windows PowerShell**. La disponibilité des applets de commande NFS intégrées facilite l’automatisation de diverses opérations. Les noms d’applets de commande sont cohérents avec d’autres applets de commande Windows PowerShell (à l’aide de verbes tels que « obtenir » et « Set »), ce qui permet aux utilisateurs familiarisés avec Windows PowerShell d’apprendre à utiliser de nouvelles applets de commande.
- **Améliorations de la gestion NFS** Une nouvelle console de gestion centralisée basée sur l’interface utilisateur simplifie la configuration et la gestion des partages SMB et NFS, des quotas, des filtres de fichiers et de la classification, en plus de la gestion des serveurs de fichiers en cluster.
- **Améliorations du mappage d’identité**. Nouvelle prise en charge de l’interface utilisateur et applets de commande Windows PowerShell basées sur les tâches pour configurer le mappage des identités, ce qui permet aux administrateurs de configurer rapidement une source de mappage d’identité, puis de créer des identités mappées individuelles pour les utilisateurs. Les améliorations permettent aux administrateurs de configurer facilement un partage pour un accès multiprotocole sur NFS et SMB.
- **Restructuration du modèle de ressource de cluster**. Cette amélioration offre une cohérence entre le modèle de ressource de cluster pour les serveurs de protocole NFS et SMB Windows et simplifie l’administration. Pour les serveurs NFS qui ont de nombreux partages, le réseau de ressources et le nombre d’appels WMI requis basculent un volume contenant un grand nombre de partages NFS sont réduits.
- **Intégration avec le gestionnaire de clés de reprise**. Le gestionnaire de clés de reprise est un composant qui effectue le suivi de l’état du serveur de fichiers et du système de fichiers et permet aux serveurs de protocole Windows SMB et NFS de basculer sans interrompre les clients ou les applications serveur qui stockent leurs données sur le serveur de fichiers. Cette amélioration est un composant clé de la fonctionnalité de disponibilité continue du serveur de fichiers exécutant Windows Server 2012.

## <a name="scenarios-for-using-network-file-system"></a>Scénarios d’utilisation du système de fichiers réseau

NFS prend en charge un environnement mixte de systèmes d’exploitation Windows et UNIX. Les scénarios de déploiement suivants sont des exemples de la façon dont vous pouvez déployer un serveur de fichiers Windows Server 2012 disponible en continu à l’aide de NFS.

### <a name="provision-file-shares-in-heterogeneous-environments"></a>Approvisionner des partages de fichiers dans des environnements hétérogènes

Ce scénario s’applique aux organisations avec des environnements hétérogènes composés de Windows et d’autres systèmes d’exploitation, tels que les ordinateurs clients UNIX ou Linux. Dans ce scénario, vous pouvez fournir un accès multi-protocole au même partage de fichiers sur les protocoles SMB et NFS. En règle générale, lorsque vous déployez un serveur de fichiers Windows dans ce scénario, vous souhaitez faciliter la collaboration entre les utilisateurs sur des ordinateurs Windows et UNIX. Lorsqu’un partage de fichiers est configuré, il est partagé avec les protocoles SMB et NFS, les utilisateurs Windows accédant à leurs fichiers via le protocole SMB, et les utilisateurs sur les ordinateurs UNIX accèdent généralement à leurs fichiers via le protocole NFS.

Pour ce scénario, vous devez disposer d’une configuration de source de mappage d’identité valide. Windows Server 2012 prend en charge les magasins de mappage d’identités suivants :

- Fichier de mappage
- Services de domaine Active Directory (AD DS)
- Magasins LDAP conformes à la norme RFC 2307 tels que services AD LDS (Active Directory Lightweight Directory Services) (AD LDS)
- Serveur mappage de noms d’utilisateurs (UNM)

### <a name="provision-file-shares-in-unix-based-environments"></a>Approvisionner des partages de fichiers dans des environnements UNIX

Dans ce scénario, les serveurs de fichiers Windows sont déployés dans un environnement principalement basé sur UNIX pour fournir l’accès aux partages de fichiers NFS pour les ordinateurs clients basés sur UNIX. Une option d’accès utilisateur UNIX non mappée (UNMAPPED) a été initialement implémentée pour les partages NFS dans Windows Server 2008 R2 afin que les serveurs Windows puissent être utilisés pour le stockage des données NFS sans créer un mappage de compte UNIX vers Windows. UNMAPPED permet aux administrateurs d’approvisionner et de déployer rapidement NFS sans avoir à configurer le mappage de compte. Lorsqu’il est activé pour NFS, UNMAPPED crée des identificateurs de sécurité (SID) personnalisés pour représenter les utilisateurs non mappés. Les comptes d’utilisateur mappés utilisent des identificateurs de sécurité Windows standard (SID) et les utilisateurs non mappés utilisent des SID NFS personnalisés.

## <a name="system-requirements"></a>Configuration requise

Le serveur pour NFS peut être installé sur n’importe quelle version de Windows Server 2012. Vous pouvez utiliser NFS avec des ordinateurs UNIX qui exécutent un serveur NFS ou un client NFS si ces implémentations de client et de serveur NFS sont conformes à l’une des spécifications de protocole suivantes :

1. Spécification de protocole NFS version 4,1 (telle que définie dans la RFC [5661](https://tools.ietf.org/html/rfc5661))
2. Spécification du protocole NFS version 3 (telle que définie dans la norme RFC [1813](https://tools.ietf.org/html/rfc1813))
3. Spécification du protocole NFS version 2 (telle que définie dans la norme RFC [1094](https://tools.ietf.org/html/rfc1094))

## <a name="deploy-nfs-infrastructure"></a>Déployer une infrastructure NFS

Vous devez déployer les ordinateurs suivants et les connecter sur un réseau local (LAN) :

- Un ou plusieurs ordinateurs exécutant Windows Server 2012 sur lesquels vous allez installer les deux composants principaux des services pour NFS : Serveur pour NFS et client pour NFS. Vous pouvez installer ces composants sur le même ordinateur ou sur des ordinateurs différents.
- Un ou plusieurs ordinateurs UNIX qui exécutent le serveur NFS et le logiciel client NFS. L’ordinateur UNIX qui exécute le serveur NFS héberge un partage de fichiers ou une exportation NFS, accessible par un ordinateur exécutant Windows Server 2012 en tant que client utilisant le client pour NFS. Vous pouvez installer le serveur NFS et le logiciel client sur le même ordinateur UNIX ou sur des ordinateurs UNIX différents, en fonction de vos besoins.
- Un contrôleur de domaine s’exécutant au niveau fonctionnel de Windows Server 2008 R2. Le contrôleur de domaine fournit les informations d’authentification de l’utilisateur et le mappage de l’environnement Windows.
- Lorsqu’un contrôleur de domaine n’est pas déployé, vous pouvez utiliser un serveur service NIS (Network Information Service) (NIS) pour fournir des informations d’authentification utilisateur pour l’environnement UNIX. Ou, si vous préférez, vous pouvez utiliser des fichiers de mot de passe et de groupe qui sont stockés sur l’ordinateur qui exécute le service mappage de noms d’utilisateurs.

### <a name="install-network-file-system-on-the-server-with-server-manager"></a>Installez le système de fichiers réseau sur le serveur avec Gestionnaire de serveur

1. Dans l’Assistant Ajout de rôles et fonctionnalités, sous Rôles de serveurs, sélectionnez **Services de fichiers et de stockage** si ce composant n’a pas déjà été installé.
2. Sous **services de fichiers et iSCSI**, sélectionnez **serveur de fichiers** et **serveur pour NFS**. Sélectionnez **Ajouter des fonctionnalités** pour inclure les fonctionnalités NFS sélectionnées.
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

Lorsque vous utilisez les protocoles NFS version 4,1 et NFS version 3,0, vous disposez des options d’authentification et de sécurité suivantes.

- RPCSEC\_GSS
  - **Krb5**. Utilise le protocole Kerberos version 5 pour authentifier les utilisateurs avant d’accorder l’accès au partage de fichiers.
  - **Krb5i**. Utilise le protocole Kerberos version 5 pour s’authentifier avec la vérification de l’intégrité (checksums), qui vérifie que les données n’ont pas été modifiées.
  - **Krb5p** Utilise le protocole Kerberos version 5, qui authentifie le trafic NFS avec le chiffrement pour la confidentialité.
- AUTHENTIFICATION\_SYS

Vous pouvez également choisir de ne pas utiliser l’autorisation du\_serveur (auth sys), ce qui vous donne la possibilité d’activer l’accès utilisateur non mappé. Lorsque vous utilisez l’accès utilisateur non mappé, vous pouvez spécifier pour autoriser l’accès utilisateur non mappé par UID/GID, qui est la valeur par défaut, ou pour autoriser l’accès anonyme.

Instructions pour la configuration de l’authentification NFS décrite dans la section suivante.

## <a name="create-an-nfs-file-share"></a>Créer un partage de fichiers NFS

Vous pouvez créer un partage de fichiers NFS à l’aide de Gestionnaire de serveur ou des applets de commande NFS Windows PowerShell.

### <a name="create-an-nfs-file-share-with-server-manager"></a>Créer un partage de fichiers NFS avec Gestionnaire de serveur

1. Ouvrez une session sur le serveur en tant que membre du groupe Administrateurs local.
2. Le Gestionnaire de serveur démarre automatiquement. S’il ne démarre pas automatiquement, sélectionnez **Démarrer**, tapez **ServerManager. exe**, puis sélectionnez **Gestionnaire de serveur**.
3. Sur la gauche, sélectionnez **services de fichiers et de stockage**, puis sélectionnez **partages**.
4. Sélectionnez **cette option pour créer un partage de fichiers, puis démarrez l’assistant nouveau partage**.
5. Dans la **page Sélectionner un profil** , sélectionnez **partage NFS : rapide** ou **partage NFS-avancé**, puis sélectionnez **suivant**.
6. Sur la page **emplacement du partage** , sélectionnez un serveur et un volume, puis sélectionnez **suivant**.
7. Dans la page **nom du partage** , spécifiez un nom pour le nouveau partage, puis sélectionnez **suivant**.
8. Dans la page **authentification** , spécifiez la méthode d’authentification que vous souhaitez utiliser pour ce partage.
9. Sur la page **partager les autorisations** , sélectionnez **Ajouter**, puis spécifiez l’ordinateur hôte, le groupe de clients ou le groupe auquel vous souhaitez accorder l’autorisation d’accès au partage.
10. Dans **autorisations**, configurez le type de contrôle d’accès que les utilisateurs doivent avoir, puis sélectionnez **OK**.
11. Dans la page **confirmation** , passez en revue votre configuration, puis sélectionnez **créer** pour créer le partage de fichiers NFS.

### <a name="windows-powershell-equivalent-commands"></a>Commandes Windows PowerShell équivalentes

L’applet de commande Windows PowerShell suivante peut également créer un partage de fichiers `nfs1` NFS (où est le nom du `C:\\shares\\nfsfolder` partage et le chemin d’accès du fichier) :

```PowerShell
New-NfsShare -name nfs1 -Path C:\shares\nfsfolder
```

### <a name="known-issue"></a>Problème connu
NFS version 4,1 permet de créer ou de copier des noms de fichiers à l’aide de caractères non autorisés. Si vous tentez d’ouvrir les fichiers avec l’éditeur VI, cela indique qu’ils sont endommagés. Vous ne pouvez pas enregistrer le fichier à partir de VI, le renommer, le déplacer ou modifier les autorisations. Évitez d’utiliser des caractères non autorisés.
