---
title: Vue d’ensemble de la redirection de dossiers, des fichiers hors connexion et des profils utilisateurs itinérants
description: Vue d’ensemble des technologies relatives à la redirection de dossiers, aux fichiers hors connexion et aux profils utilisateur itinérants.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: a7c37638e25fc0d16447ab57bf369255dab9c859
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "75950258"
---
# <a name="folder-redirection-offline-files-and-roaming-user-profiles-overview"></a>Vue d’ensemble de la redirection de dossiers, des fichiers hors connexion et des profils utilisateurs itinérants

>S'applique à : Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2

Cette rubrique décrit les technologies relatives à la redirection de dossiers, aux fichiers hors connexion (mise en cache côté client) et aux profils utilisateur itinérants. Elle inclut notamment des détails sur les nouveautés et sur les emplacements permettant d’obtenir des informations supplémentaires.

## <a name="technology-description"></a>Description de la technologie

La redirection de dossiers et les fichiers hors connexion sont utilisés ensemble pour rediriger le chemin d’accès des dossiers locaux (tels que le dossier Documents) vers un emplacement réseau, tout en mettant en cache le contenu localement pour augmenter la vitesse et la disponibilité. Les profils utilisateurs itinérants permettent de rediriger un profil utilisateur vers un emplacement réseau. Ces fonctionnalités étaient référencées sous le nom d’Intellimirror.

- La **redirection de dossiers** permet aux utilisateurs et aux administrateurs de rediriger le chemin d’un dossier connu vers un nouvel emplacement, manuellement ou à l’aide d’une stratégie de groupe. Le nouvel emplacement peut être un dossier sur l’ordinateur local ou un répertoire sur un partage de fichiers. Les utilisateurs interagissent avec les fichiers dans le dossier redirigé comme s’il se trouvait toujours sur le lecteur local. Par exemple, vous pouvez rediriger le dossier Documents, généralement stocké sur un lecteur local, vers un emplacement réseau. Les fichiers figurant dans le dossier sont alors accessibles à l’utilisateur à partir de n’importe quel ordinateur sur le réseau.
- **Fichiers hors connexion** est une fonctionnalité qui rend les fichiers réseau accessibles à un utilisateur, même si la connexion réseau au serveur est non disponible ou lente. Lorsque vous travaillez en ligne, les performances d’accès aux fichiers dépendent de la vitesse du réseau et du serveur. Lorsque vous travaillez hors connexion, les fichiers sont récupérés du dossier Fichiers hors connexion aux vitesses d’accès local. Un ordinateur passe en mode hors connexion lorsque :
  - le mode **Toujours hors connexion** a été activé ;
  - le serveur est indisponible ;
  - la connexion réseau est plus lente qu’un seuil configurable ;
  - l’utilisateur bascule manuellement en mode hors connexion à l’aide du bouton **Travailler hors connexion** dans l’Explorateur Windows.
- **Profils utilisateur itinérants** est une fonctionnalité qui permet de rediriger les profils utilisateur vers un partage de fichiers pour que les utilisateurs reçoivent les mêmes paramètres de système d’exploitation et d’application sur plusieurs ordinateurs. Quand un utilisateur se connecte à un ordinateur à l’aide d’un compte configuré avec un partage de fichiers en tant que chemin du profil, le profil de l’utilisateur est téléchargé sur l’ordinateur local et fusionné avec le profil local (le cas échéant). Lorsque l’utilisateur ferme la session sur l’ordinateur, la copie locale de son profil, y compris toutes les modifications, est fusionnée avec la copie serveur du profil. En règle générale, un administrateur réseau active les profils utilisateur itinérants sur les comptes de domaine.

## <a name="practical-applications"></a>Cas pratiques

Les administrateurs peuvent utiliser la redirection de dossiers, les fichiers hors connexion et les profils utilisateurs itinérants pour centraliser le stockage des paramètres et données utilisateur et pour fournir aux utilisateurs la capacité d’accéder à leurs données en mode hors connexion ou dans le cas d’une indisponibilité du réseau ou d’une panne du serveur. Certaines applications spécifiques incluent les fonctionnalités suivantes :

- centraliser les données issues des ordinateurs clients pour les tâches administratives, telles que l’utilisation d’un outil de sauvegarde serveur pour sauvegarder les paramètres et dossiers utilisateur ;
- permettre aux utilisateurs de continuer à accéder aux fichiers réseau, même en cas d’indisponibilité du réseau ou de panne du serveur ;
- optimiser l’utilisation de la bande passante et améliorer l’expérience des utilisateurs des filiales qui accèdent à des fichiers et des dossiers hébergés par les serveurs d’entreprise situés hors site ;
- permettre aux utilisateurs itinérants d’accéder aux fichiers réseau lorsqu’ils travaillent hors connexion ou sur des réseaux lents.

## <a name="new-and-changed-functionality"></a>Fonctionnalités nouvelles et modifiées

Le tableau ci-dessous décrit quelques-unes des principales modifications apportées aux fonctionnalités de redirection de dossiers, fichiers hors connexion et profils utilisateurs itinérants, disponibles dans cette version.

| Fonctionnalité/fonction | Nouveauté ou mise à jour ? | Description |
| --- | --- | --- |
| Mode Toujours hors connexion | Nouveau | Assure un accès plus rapide aux fichiers et une utilisation réduite de la bande passante en travaillant toujours hors connexion, même lors d’une connexion via une connexion réseau haut débit. |
| Synchronisation prenant en charge les coûts | Nouveau | Aide les utilisateurs à éviter les coûts élevés de consommation des données liés à la synchronisation sur les connexions limitées ayant des seuils d’utilisation, ou en cas d’itinérance sur le réseau d’un autre fournisseur. |
| Prise en charge des ordinateurs principaux | Nouveau | Vous permet de limiter l’utilisation de la redirection de dossiers et/ou des profils utilisateur itinérants aux ordinateurs principaux d’un utilisateur. |

## <a name="always-offline-mode"></a>Mode Toujours hors connexion

Depuis Windows 8 et Windows Server 2012, les administrateurs peuvent configurer la fonctionnalité Fichiers hors connexion pour permettre aux utilisateurs de travailler hors connexion en permanence, même quand ils se servent d’une connexion réseau haut débit. Windows met à jour les fichiers dans le cache des fichiers hors connexion en les synchronisant toutes les heures en arrière-plan, par défaut.

### <a name="what-value-does-always-offline-mode-add"></a>Qu’apporte le mode Toujours hors connexion ?

Le mode Toujours hors connexion présente les avantages suivants :

- les utilisateurs bénéficient d’un accès plus rapide aux fichiers figurant dans les dossiers redirigés, tels que le dossier Documents ;
- la bande passante réseau est réduite, ce qui réduit les coûts des connexions WAN ou des connexions limitées onéreuses, telles qu’un réseau mobile 4G.

### <a name="how-has-always-offline-mode-changed-things"></a>Comment le mode Toujours hors connexion a-t-il changé les choses ?

Avant Windows 8 et Windows Server 2012, les utilisateurs passaient du mode En ligne au mode Hors connexion (et inversement) en fonction de la disponibilité et des conditions du réseau, même quand le mode Liaison lente (également connu sous le nom de mode Connexion lente) était activé et fixé à un seuil de latence de 1 milliseconde.

En mode Toujours hors connexion, les ordinateurs ne passent jamais en mode En ligne quand le paramètre de stratégie de groupe **Configurer le mode de liaison lente** est configuré, et que le seuil du paramètre **Latence** est fixé à 1 milliseconde. Les modifications sont synchronisées en arrière-plan toutes les 120 minutes, par défaut, mais la synchronisation est configurable à l’aide du paramètre de stratégie de groupe **Configure Background Sync** .

Pour plus d'informations, voir [Enable the Always Offline Mode to Provide Faster Access to Files](enable-always-offline.md).

## <a name="cost-aware-synchronization"></a>Synchronisation prenant en charge les coûts

Avec la synchronisation basée sur les coûts, Windows désactive la synchronisation en arrière-plan quand l’utilisateur se sert d’une connexion réseau limitée, par exemple un réseau mobile 4G, et que l’abonné se trouve à proximité ou au-delà de sa limite de bande passante, ou en itinérance sur le réseau d’un autre fournisseur.

> [!NOTE]
> Les connexions réseau limitées ont généralement des temps de réponse (allers-retours) sur le réseau qui sont plus lents que la valeur de latence par défaut de 35 millisecondes pour le passage en mode Hors connexion (connexion lente) dans Windows 8, Windows Server 2019, Windows Server 2016 et Windows Server 2012. Par conséquent, ces connexions effectuent en général une transition automatique vers le mode Hors connexion (connexion lente).

### <a name="what-value-does-cost-aware-synchronization-add"></a>Qu’apporte la synchronisation basée sur les coûts ?

La synchronisation basée sur les coûts aide les utilisateurs à éviter des frais de consommation des données élevés et inattendus quand des limites d’utilisation s’appliquent aux connexions, ou en cas d’itinérance sur le réseau d’un autre fournisseur.

### <a name="how-has-cost-aware-synchronization-changed-things"></a>Comment la synchronisation basée sur les coûts a changé les choses ?

Avant Windows 8 et Windows Server 2012, les utilisateurs qui souhaitaient réduire les frais liés à la fonctionnalité Fichiers hors connexion sur des connexions réseau limitées devaient suivre leur consommation des données en utilisant les outils du fournisseur de réseau mobile. Les utilisateurs pouvaient alors basculer manuellement en mode Hors connexion lorsqu’ils étaient en itinérance, proches de leur limite de bande passante ou au-delà de cette limite.

Avec la synchronisation basée sur les coûts, Windows effectue automatiquement le suivi des limites d’utilisation liées à l’itinérance et à la bande passante pour les connexions limitées. Lorsque l’utilisateur est itinérant, proche de sa limite de bande passante ou au-delà de cette limite, Windows bascule en mode hors connexion et empêche toute synchronisation. Les utilisateurs peuvent encore initier manuellement la synchronisation et les administrateurs peuvent remplacer la synchronisation prenant en charge les coûts pour des utilisateurs spécifiques, tels que les dirigeants.

Pour plus d'informations, voir [Enable Background File Synchronization on Metered Networks](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj127408(v%3dws.11)).

## <a name="primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>Ordinateurs principaux pour la redirection de dossiers et les profils utilisateurs itinérants

Vous pouvez désormais désigner un ensemble d’ordinateurs, appelés ordinateurs principaux, pour chaque utilisateur de domaine, ce qui vous permet de déterminer quels sont les ordinateurs qui doivent utiliser la redirection de dossiers, les profils utilisateur itinérants, ou les deux. La désignation des ordinateurs principaux est une méthode simple et efficace qui permet d'associer les paramètres et les données utilisateur à des périphériques ou des ordinateurs particuliers, de simplifier la supervision de l'administrateur, d'améliorer la sécurité des données et d'aider à protéger les profils utilisateur contre d'éventuels dommages.

### <a name="what-value-do-primary-computers-add"></a>Qu’apportent les ordinateurs principaux ?

La désignation d’ordinateurs principaux pour les utilisateurs présente quatre avantages principaux :

- L’administrateur peut spécifier les ordinateurs que les utilisateurs peuvent utiliser pour accéder à leurs données et paramètres redirigés. Par exemple, l’administrateur peut choisir de rendre itinérants les paramètres et les données d’un utilisateur entre son poste de travail et son ordinateur portable, et de ne pas rendre itinérantes ces informations quand l’utilisateur se connecte à une autre machine, telle que l’ordinateur d’une salle de conférence.
- La désignation d’ordinateurs principaux réduit les risques en matière de sécurité et de confidentialité liés au fait de laisser des données personnelles ou d’entreprise résiduelles sur des ordinateurs où l’utilisateur a ouvert une session. Par exemple, un directeur général qui se connecte à l’ordinateur d’un employé pour y accéder temporairement ne laisse derrière lui aucune donnée personnelle ni de l’entreprise.
- Les ordinateurs principaux permettent à l’administrateur de limiter le risque lié à un profil configuré de façon incorrecte ou endommagé, qui pourrait résulter de l’itinérance entre des systèmes configurés de manières différentes, tels que des ordinateurs avec architecture x64 et x86.
- Le temps nécessaire à la première connexion d’un utilisateur sur un ordinateur non principal, par exemple un serveur, est plus rapide, car le profil utilisateur itinérant de l’utilisateur et/ou les dossiers redirigés ne sont pas téléchargés. Les temps de déconnexion sont également réduits, car les modifications du profil utilisateur ne sont pas tenues d’être téléchargées vers le partage de fichiers.

### <a name="how-have-primary-computers-changed-things"></a>Comment les ordinateurs principaux ont changé les choses ?

Pour restreindre le téléchargement des données utilisateur privées vers les ordinateurs principaux, les technologies de redirection de dossiers et de profils utilisateurs itinérants effectuent les vérifications logiques suivantes lorsqu’un utilisateur se connecte à un ordinateur :

1. Le système d’exploitation Windows vérifie les nouveaux paramètres de stratégie de groupe (**Télécharger les profils itinérants sur les ordinateurs principaux uniquement** et **Rediriger les dossiers uniquement sur les ordinateurs principaux**) pour déterminer si l’attribut **msDS-Primary-Computer** dans AD DS (Active Directory Domain Services) doit influencer la décision de rendre itinérant le profil de l’utilisateur ou d’appliquer la redirection de dossiers.
2. Si le paramètre de stratégie active la prise en charge des ordinateurs principaux, Windows vérifie que le schéma AD DS prend en charge l’attribut **msDS-Primary-Computer** . Si tel est le cas, Windows détermine si l’ordinateur sur lequel l’utilisateur ouvre une session est désigné comme ordinateur principal pour l’utilisateur, comme suit :
    1. Si l’ordinateur est l’un des ordinateurs principaux de l’utilisateur, Windows applique les paramètres liés aux profils utilisateur itinérants et à la redirection de dossiers.
    2. Si l’ordinateur n’est pas l’un des ordinateurs principaux de l’utilisateur, Windows charge le profil local mis en cache de l’utilisateur, le cas échéant, ou crée un profil local. Windows supprime également tous les dossiers redirigés existants conformément à l’action de suppression qui a été spécifiée par le paramètre de stratégie de groupe précédemment appliqué, qui est conservé dans la configuration locale de la redirection de dossiers.

Pour plus d'informations, consultez [Deploy Primary Computers for Folder Redirection and Roaming User Profiles](deploy-primary-computers.md)

## <a name="hardware-requirements"></a>Configuration matérielle requise

La redirection de dossiers, les fichiers hors connexion et les profils utilisateurs itinérants requièrent un ordinateur avec architecture x64 ou x86, et ils ne sont pas pris en charge par les ordinateurs avec architecture Windows sur ARM (WOA).

## <a name="software-requirements"></a>Configuration logicielle requise

Pour désigner les ordinateurs principaux, votre environnement doit répondre aux exigences suivantes :

- Le schéma AD DS (Active Directory Domain Services) doit être mis à jour pour inclure le schéma et les conditions de Windows Server 2012 (l’installation d’un contrôleur de domaine Windows Server 2012 ou version ultérieure met automatiquement à jour le schéma). Pour plus d’informations sur la mise à niveau du schéma AD DS, consultez [Mettre à niveau des contrôleurs de domaine vers Windows Server 2016](../../identity/ad-ds/deploy/upgrade-domain-controllers.md).
- Les ordinateurs clients doivent exécuter Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012, et être membres du domaine Active Directory que vous gérez.

## <a name="more-information"></a>Autres informations

Pour plus d’informations connexes, voir les ressources suivantes.

| Type de contenu | Références |
| --- | --- |
| Évaluation du produit | [Prise en charge des travailleurs de l’information avec des services de fichiers et de stockage fiables](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831495(v%3dws.11)>)<br>[Nouveautés des fichiers hors connexion](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff183315(v=ws.10)>) (Windows 7 et Windows Server 2008 R2)<br>[Nouveautés des fichiers hors connexion pour Windows Vista](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc749449(v=ws.10)>)<br>[Changements apportés aux fichiers hors connexion dans Windows Vista](<https://technet.microsoft.com/library/2007.11.offline.aspx>) (TechNet Magazine) |
| Déploiement | [Déployer la redirection de dossiers, les fichiers hors connexion et les profils utilisateur itinérants](deploy-folder-redirection.md)<br>[Implémentation d’une solution de centralisation des données pour l’utilisateur final : Validation et déploiement de la technologie relative à la redirection de dossiers et aux fichiers hors connexion](https://download.microsoft.com/download/3/0/1/3019A3DA-2F41-4F2D-BBC9-A6D24C4C68C4/Implementing%20an%20End-User%20Data%20Centralization%20Solution.docx)<br>[Guide de déploiement pour la gestion des données de l’utilisateur itinérant](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc766489(v=ws.10)>)<br>[Guide pas à pas de la configuration des nouvelles fonctionnalités Fichiers hors connexion pour les ordinateurs Windows 7](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff633429(v=ws.10)>)<br>[Utilisation de la redirection de dossiers](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753996(v=ws.11)>)<br>[Implémentation de la redirection de dossiers](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc737434(v=ws.10)>) (Windows Server 2003) |
| Outils et paramètres | [Fichiers hors connexion sur MSDN](https://msdn.microsoft.com/library/cc296092.aspx)<br>[Référence de la stratégie de groupe Fichiers hors connexion](https://msdn.microsoft.com/library/ms878937.aspx) (Windows 2000) |
| Ressources de la communauté | [Forum sur le stockage et les services de fichiers](https://social.technet.microsoft.com/forums/windowsserver/home?forum=winserverfiles)<br>[Hey, Scripting Guy! How Can I Work with the Offline Files Feature in Windows?](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>)<br>[Hey, Scripting Guy! How Can I Enable and Disable Offline Files?](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>) |
| Technologies connexes|[Identité et accès dans Windows Server](../../identity/identity-and-access.md)<br>[Storage](../storage.md) (Stockage)<br>[Accès à distance et gestion de serveur](../../remote/index.md) |