---
title: Déployer le système de gestion de fichiers en réseau
description: Décrit le déploiement de système de fichiers réseau.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 2f3671283720d515cd3e3e609d98e02343c15892
ms.sourcegitcommit: fcc26ec5a2cc73b59c5752377b39c070d288655e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/19/2018
ms.locfileid: "8976685"
---
# Déployer le système de gestion de fichiers en réseau

>S’applique à: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Système de fichiers réseau (NFS) fournit une solution qui vous permet de transférer des fichiers entre les ordinateurs exécutant Windows Server et les systèmes d’exploitation UNIX utilisant le protocole NFS de partage de fichiers. Cette rubrique décrit les étapes à suivre pour déployer NFS.

## Quelles sont les nouveautés dans le système de fichiers réseau

Voici ce qui a changé pour NFS dans Windows Server 2012:

- **Prise en charge pour NFS version 4.1**. Cette version de protocole inclut les améliorations suivantes.
  - La navigation de pare-feu est plus facile, améliorer l’accessibilité.
  - Prend en charge le protocole RPCSEC\_GSS, en fournissant une sécurité renforcée et ce qui permet aux clients et serveurs de négociation de la sécurité.
  - Prend en charge la sémantique de fichier UNIX et Windows.
  - Tire parti des déploiements de serveurs de fichiers en cluster.
  - Prend en charge les procédures composés WAN compatible.

- **Module NFS pour Windows PowerShell**. La disponibilité des applets de commande intégrées NFS facilite l’automatiser diverses opérations. Les noms de l’applet de commande sont cohérentes avec les autres applets de commande Windows (à l’aide des verbes tels que «Get» et «Set»), ce qui facilite pour les utilisateurs familiarisé avec Windows PowerShell pour apprendre à utiliser les nouvelles applets de commande.
- **Améliorations de la gestion NFS**. Une nouvelle console de gestion centralisée via l’interface utilisateur simplifie la configuration et la gestion des SMB et les partages NFS, les quotas, les filtres de fichiers et les classification, en plus de la gestion des serveurs de fichiers en cluster.
- **Améliorations de mappage des identités**. Nouvelle prise en charge de l’interface utilisateur et les applets de commande Windows basée sur les tâches de configuration de mappage d’identité, ce qui permet aux administrateurs de configurer rapidement une source de mappage d’identité et créez des identités mappées individuelles pour les utilisateurs. Améliorations facilitent l’aux administrateurs de configurer un partage pour un accès multi-protocol sur NFS et SMB.
- **Modèle de ressource de cluster énormément**. Cette amélioration apporte la cohérence entre le modèle de ressource de cluster pour NFS Windows et les serveurs de protocole SMB et simplifie l’administration. Pour les serveurs NFS qui ont de nombreux partages, le réseau de ressources et le nombre d’appels WMI requis basculent sur un volume qui contient qu'un grand nombre de partages NFS est réduit.
- **Intégration avec le Gestionnaire de clés de reprise**. Le Gestionnaire de clés de reprise est un composant qui effectue le suivi des serveurs de fichiers et l’état du système de fichiers et permet aux serveurs protocole SMB de Windows et NFS basculer sans perturber des clients ou des applications serveur qui stockent leurs données sur le serveur de fichiers. Cette amélioration est un composant clé de la capacité d’une disponibilité permanente au serveur de fichiers exécutant Windows Server 2012.

## Scénarios d’utilisation de système de fichiers réseau

NFS prend en charge un environnement mixte de systèmes d’exploitation basé sur Windows et basés sur UNIX. Les scénarios de déploiement suivants sont des exemples de la façon dont vous pouvez déployer un serveur de fichiers Windows Server 2012 disponible en permanence à l’aide de NFS.

### Partages de fichiers de disposition d’environnements hétérogènes

Ce scénario s’applique aux organisations via le d’environnements hétérogènes constitués Windows et les autres systèmes d’exploitation, par exemple, UNIX ou client basé sur Linux ordinateurs. Avec ce scénario, vous pouvez fournir des accès multi-protocol dans le même partage de fichiers sur les protocoles NFS et SMB. En règle générale, lorsque vous déployez un serveur de fichiers Windows dans ce scénario, vous souhaitez favoriser la collaboration entre les utilisateurs sur Windows et les ordinateurs UNIX. Lorsqu’un partage de fichiers est configuré, il est partagé avec les protocoles les SMB et NFS, avec accès à leurs fichiers via le protocole SMB, les utilisateurs de Windows et les utilisateurs sur les ordinateurs UNIX généralement accèdent à leurs fichiers via le protocole NFS.

Pour ce scénario, vous devez disposer d’une configuration de source de mappage identité valide. Windows Server 2012 prend en charge les magasins de mappage d’identité suivantes:

- Fichier de mappage
- Services de domaine Active Directory (AD DS)
- RFC 2307 conforme LDAP stocke comme Active Directory Lightweight Directory Services (AD LDS)
- Serveur de mappage de nom (UNM) utilisateur

### Partages de fichiers de disposition d’environnements UNIX

Dans ce scénario, les serveurs de fichiers Windows sont déployés dans un environnement principalement UNIX pour fournir l’accès aux partages de fichiers NFS pour les ordinateurs clients basés sur UNIX. Une option non mappés UNIX utilisateur accès (UUUA) a été initialement implémentée pour les partages NFS dans Windows Server 2008 R2 afin que le mappage de compte Windows serveurs peuvent être utilisés pour stocker des données NFS sans avoir à créer UNIX vers Windows. UUUA permet aux administrateurs de rapidement provisionner et déployer NFS sans avoir à configurer le mappage de compte. Lorsqu’elle est activée pour NFS, UUUA crée des identificateurs de sécurité personnalisés (SID) pour représenter les utilisateurs non mappés. Les comptes d’utilisateurs mappés utilisent des identificateurs de sécurité Windows standard (SID) et les utilisateurs non mappés SID NFS personnalisés.

## Configuration requise

Serveur pour NFS peut être installé sur n’importe quelle version de Windows Server 2012. Vous pouvez utiliser NFS avec des ordinateurs UNIX qui exécutent un serveur NFS ou un client NFS si ces implémentations du client et serveur NFS répondent avec l’une des spécifications de protocole suivant:

1. Spécification du protocole NFS Version 4.1 (tel que défini dans RFC [5661](https://tools.ietf.org/html/rfc5661))
2. Spécification du protocole NFS Version 3 (tel que défini dans RFC [1813](https://tools.ietf.org/html/rfc1813))
3. Spécification du protocole NFS Version 2 (tel que défini dans RFC [1094](https://tools.ietf.org/html/rfc1094))

## Déployer l’infrastructure NFS

Vous avez besoin déployer les ordinateurs suivants et le connecter à un réseau local (LAN):

- Un ou plusieurs ordinateurs exécutant Windows Server 2012 sur lequel vous allez installer les deux Services principaux composants NFS: serveur pour NFS et Client pour NFS. Vous pouvez installer ces composants sur le même ordinateur ou sur des ordinateurs différents.
- Un ou plusieurs ordinateurs UNIX qui exécutent serveur NFS et un logiciel client NFS. L’ordinateur UNIX qui exécute le serveur NFS héberge un partage de fichiers NFS ou d’exportation, qui est accessible par un ordinateur qui exécute Windows Server 2012 en tant que client à l’aide de Client pour NFS. Vous pouvez installer des logiciels client et serveur NFS dans le même ordinateur UNIX ou sur des ordinateurs basés sur UNIX les différents, selon vos besoins.
- Un contrôleur de domaine en cours d’exécution au niveau fonctionnel de Windows Server 2008 R2. Le contrôleur de domaine fournit des informations d’authentification utilisateur et mappage pour l’environnement Windows.
- Quand un contrôleur de domaine n’est pas déployé, vous pouvez utiliser un serveur d’informations NIS (Network Service) pour fournir des informations d’authentification utilisateur pour l’environnement UNIX. Ou, si vous préférez, vous pouvez utiliser le mot de passe et de groupe de fichiers qui sont stockés sur l’ordinateur qui exécute le service de mappage de nom de l’utilisateur.

### Installer le système de fichiers réseau sur le serveur avec le Gestionnaire de serveur

1. À partir de l’ajout de rôles et l’Assistant de fonctionnalités, rôles de serveur, sélectionnez **les Services de stockage de fichiers et** si elle n’a pas encore été installé.
2. Sous **fichiers et iSCSI Services**, sélectionnez le **Serveur de fichiers** et de **serveur pour NFS**. Sélectionnez **Ajouter des fonctionnalités** à inclure des fonctionnalités NFS sélectionnées.
3. Sélectionnez **installer** pour installer les composants NFS sur le serveur.

### Installer le système de fichiers réseau sur le serveur avec Windows PowerShell

1. Démarrez Windows PowerShell. Avec le bouton droit sur la barre des tâches, l’icône de PowerShell, puis sélectionnez **Exécuter en tant qu’administrateur**.
2. Exécutez les commandes Windows PowerShell suivantes:

```PowerShell
Import-Module ServerManager
Add-WindowsFeature FS-NFS-Service
Import-Module NFS
```

## Configurer l’authentification NFS

Lorsque vous utilisez la version NFS 4.1 et les protocoles de version 3.0 NFS, vous disposez des options d’authentification et de sécurité suivantes.

- RPCSEC\_GSS
  - **Krb5**. Utilise le protocole Kerberos version 5 pour authentifier les utilisateurs avant d’accorder l’accès au partage de fichiers.
  - **Krb5i**. Utilise le protocole Kerberos version 5 pour s’authentifier auprès de l’intégrité de la vérification (les sommes de contrôle), qui vérifie que les données n’a pas été modifiées.
  - **Krb5p** Utilise le protocole Kerberos version 5, qui authentifie le trafic NFS avec le chiffrement pour la confidentialité.
- AUTH\_SYS

Vous pouvez également choisir de ne doit ne pas utiliser de serveur d’autorisation (AUTH\_SYS), ce qui vous donne la possibilité d’activer l’accès des utilisateurs non mappés. Lorsque vous utilisez l’accès des utilisateurs non mappés, vous pouvez spécifier pour autoriser l’accès des utilisateurs non mappés par UID / GID, qui est la valeur par défaut, ou autoriser l’accès anonyme.

Instructions pour la configuration de l’authentification NFS sur décrits dans la section suivante.

## Créer un partage de fichiers NFS

Vous pouvez créer un partage de fichiers NFS à l’aide du Gestionnaire de serveur ou Windows PowerShell NFS applets de commande.

### Créer un partage de fichiers NFS avec le Gestionnaire de serveur

1. Connectez-vous au serveur en tant que membre du groupe Administrateurs local.
2. Le Gestionnaire de serveur démarrera automatiquement. Si elle ne démarre pas automatiquement, sélectionnez **Démarrer**, tapez **servermanager.exe**et sélectionnez **Le Gestionnaire de serveur**.
3. Sur la gauche, sélectionnez le **fichier et les Services de stockage**et sélectionnez **partages**.
4. Sélectionnez **pour créer un partage de fichiers, démarrez l’Assistant Nouveau partage**.
5. Sur la page **Sélectionner un profil** , sélectionnez **Partage NFS – rapide** ou **partage NFS - avancée**, puis sélectionnez **suivant**.
6. Sur la page de **l’Emplacement de partage** , sélectionnez un serveur et un volume, puis **suivant**.
7. Sur la page **Nom du partage** , spécifiez un nom pour le nouveau partage, puis cliquez sur **suivant**.
8. Sur la page de **l’authentification** , spécifiez la méthode d’authentification à utiliser pour ce partage.
9. Sur la page **Autorisations du partage** , sélectionnez **Ajouter**et spécifiez l’hôte, le groupe de clients ou le groupe que vous souhaitez accorder l’autorisation au partage.
10. Dans **les autorisations**, configurer le type de contrôle d’accès vous souhaitez que les utilisateurs ont et sélectionnez **OK**.
11. Sur la page de **Confirmation** , passez en revue votre configuration, puis sélectionnez **créer** pour créer le partage de fichiers NFS.

### Commandes équivalentes de Windows PowerShell

L’applet de commande Windows PowerShell suivante peut également créer un partage de fichiers NFS (où `nfs1` est le nom du partage et `C:\\shares\\nfsfolder` est le chemin d’accès):

```PowerShell
New-NfsShare -name nfs1 -Path C:\shares\nfsfolder
```

### Problème connu
NFS version 4.1 autorise les noms de fichiers à être créée ou copiée à l’aide de caractères non autorisés. Si vous essayez d’ouvrir les fichiers avec l’éditeur vi, il affiche comme étant endommagé. Vous ne pouvez pas enregistrer le fichier à partir de vi, renommer, déplacez-le ou modifier les autorisations. Évitez d’utiliser des caractères illigal.
