---
title: Vue d’ensemble de la Redirection de dossiers, les fichiers hors connexion et les profils utilisateur itinérants
description: Vue d’ensemble des technologies de Redirection de dossiers, les fichiers hors connexion et les profils utilisateur itinérants.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 4e3cf32cd718b906f16fc09901284d8520177df8
ms.sourcegitcommit: 375e94dc70c76e7aa5679c32f0f4dbc26cf7dd83
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/10/2018
ms.locfileid: "2233495"
---
# <a name="folder-redirection-offline-files-and-roaming-user-profiles-overview"></a>Vue d’ensemble de la Redirection de dossiers, les fichiers hors connexion et les profils utilisateur itinérants

>S’applique à: Windows 10, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

Cette rubrique décrit la Redirection de dossiers, des fichiers en mode hors connexion (la mise en cache côté client ou CSC) et les technologies de profils utilisateur itinérants (parfois appelé profil itinérant), notamment les nouveautés et où trouver des informations supplémentaires.

## <a name="technology-description"></a>Description de la technologie

La redirection de dossiers et les fichiers hors connexion sont utilisés ensemble pour rediriger le chemin d’accès des dossiers locaux (tels que le dossier Documents) vers un emplacement réseau, tout en mettant en cache le contenu localement pour augmenter la vitesse et la disponibilité. Les profils utilisateurs itinérants permettent de rediriger un profil utilisateur vers un emplacement réseau. Ces fonctionnalités utilisées à être appelé Intellimirror.

- **Redirection de dossiers** permet aux utilisateurs et aux administrateurs de rediriger le chemin d’accès d’un dossier connu vers un nouvel emplacement, manuellement ou à l’aide de la stratégie de groupe. Le nouvel emplacement peut être un dossier sur l’ordinateur local ou un répertoire sur un partage de fichiers. Les utilisateurs interagissent avec les fichiers dans le dossier redirigé comme s’il existait toujours sur le disque local. Par exemple, vous pouvez rediriger le dossier de Documents, qui est généralement stocké sur un lecteur local, à un emplacement réseau. Les fichiers dans le dossier sont ensuite disponibles à l’utilisateur à partir de n’importe quel ordinateur sur le réseau.
- **Fichiers en mode hors connexion** rend les fichiers réseau disponibles à un utilisateur, même si la connexion réseau au serveur est indisponible ou lente. Lorsque vous travaillez en ligne, les performances d’accès au fichier est à la vitesse du réseau et du serveur. Lorsque vous travaillez en mode hors connexion, les fichiers sont récupérés à partir du dossier des fichiers en mode hors connexion à la vitesse d’accès local. Un ordinateur passe en Mode hors connexion lorsque:
  - Mode **Toujours en mode hors connexion** a été activé.
  - Le serveur n’est pas disponible
  - La connexion réseau est inférieure à un seuil configurable
  - L’utilisateur bascule manuellement en Mode hors connexion à l’aide du bouton **travailler hors connexion** dans l’Explorateur Windows
- **Profils utilisateur itinérants** redirige les profils utilisateur vers un partage de fichiers afin que les utilisateurs reçoivent le même système d’exploitation et les paramètres de l’application sur plusieurs ordinateurs. Lorsqu’un utilisateur se connecte à un ordinateur en utilisant un compte qui est configuré avec un partage de fichiers en tant que le chemin d’accès du profil, le profil utilisateur est téléchargé sur l’ordinateur local et fusionné avec le profil local (le cas échéant). Lorsque l’utilisateur se connecte en dehors de l’ordinateur, la copie locale de son profil, y compris les modifications, est fusionnée avec la copie serveur du profil. En règle générale, un administrateur réseau permet de profils utilisateur itinérants sur les comptes de domaine.

## <a name="practical-applications"></a>Applications pratiques

Les administrateurs peuvent utiliser la Redirection de dossiers, les fichiers en mode hors connexion et les profils utilisateur itinérants pour centraliser le stockage de données et paramètres utilisateur et pour fournir aux utilisateurs la possibilité d’accéder à leurs données en mode hors connexion ou en cas d’une panne de réseau ou du serveur. Certaines applications spécifiques sont les suivantes:

- Centralisation des données à partir des ordinateurs clients pour les tâches d’administration, par exemple à l’aide d’un outil de sauvegarde sur le serveur pour sauvegarder les paramètres et les dossiers de l’utilisateur.
- Permettre aux utilisateurs de continuer à accéder à des fichiers du réseau, même s’il existe une panne de réseau ou du serveur.
- Optimiser l’utilisation de la bande passante et améliorer l’expérience des utilisateurs dans les succursales qui a accès aux fichiers et dossiers qui sont hébergés par hors site des serveurs d’entreprise.
- Permettre aux utilisateurs mobiles d’accéder aux fichiers réseau lors de l’utilisation en mode hors connexion ou sur des réseaux lents.

## <a name="new-and-changed-functionality"></a>Fonctionnalités nouvelles et modifiées

Le tableau suivant décrit quelques-unes des modifications majeures dans les profils utilisateur itinérants sont disponibles dans cette version, la Redirection de dossiers et fichiers hors connexion.

|Fonctionnalité/fonction|Nouveauté ou mise à jour?|Description|
|---|---|---|
|Mode toujours en mode hors connexion|Nouveau|Fournit un accès rapide aux fichiers et l’utilisation de la bande passante inférieure en travaillant toujours en mode hors connexion, même lorsque connecté via une connexion réseau haut débit.|
|Synchronisation prenant en charge les coûts|Nouveau|Permet aux utilisateurs d’éviter les coûts d’utilisation de données élevé de la synchronisation lors de l’utilisation des connexions qui ont des limites d’utilisation, ou pendant l’itinérance sur le réseau d’un autre fournisseur.|
|Ordinateur principal prise en charge|Nouveau|Vous permet de limiter l’utilisation de la Redirection de dossiers, les profils utilisateur itinérants ou les deux uniquement les ordinateurs principal d’un utilisateur.|

## <a name="always-offline-mode"></a>Mode toujours en mode hors connexion

Démarrage avec Windows 8 et Windows Server 2012, les administrateurs peuvent configurer l’expérience des utilisateurs de fichiers en mode hors connexion à toujours travailler hors connexion, même lorsqu’ils sont connectés via une connexion réseau haut débit. Windows met à jour les fichiers dans le cache de fichiers hors connexion de synchronisation toutes les heures en arrière-plan, par défaut.

### <a name="what-value-does-always-offline-mode-add"></a>Quelle valeur est toujours en mode hors connexion en mode ajouter?

Le mode hors connexion toujours offre les avantages suivants:

- Les utilisateurs rencontrent un accès rapide aux fichiers dans les dossiers redirigés, tels que le dossier Documents.
- Bande passante réseau est réduite, réduction des coûts sur les connexions WAN coûteuses ou contrôlé par exemple un réseau mobile 4 G.

### <a name="how-has-always-offline-mode-changed-things"></a>Comment le mode toujours en mode hors connexion a changé choses?

Avant de Windows 8, Windows Server 2012, les utilisateurs seraient effectuer la transition entre les modes en ligne et hors connexion, en fonction de la disponibilité du réseau et les conditions, même lorsque le mode de liaison lente (également connu sous le mode de connexion lente) a été activé et défini sur une 1 milliseconde. seuil de latence.

Toujours en mode hors connexion en mode, les ordinateurs jamais effectuer la transition vers le mode en ligne lorsque le paramètre **configurer le mode de liaison lente** de stratégie de groupe est configuré et le paramètre de seuil de **latence** est défini à 1 milliseconde. Les modifications sont synchronisées en arrière-plan toutes les 120 minutes, par défaut, mais la synchronisation est configurable en utilisant le paramètre de stratégie de groupe de **Configurer la synchronisation en arrière-plan** .

Pour plus d’informations, voir [Activer le toujours Mode hors connexion pour fournir plus rapidement l’accès aux fichiers](enable-always-offline.md).

## <a name="cost-aware-synchronization"></a>Synchronisation prenant en charge les coûts

Avec la synchronisation prenant en charge les coûts, Windows désactive la synchronisation d’arrière-plan lorsque l’utilisateur à l’aide d’une connexion réseau contrôlé, tel qu’un réseau mobile 4G et l’abonné est proche ou supérieur à leur limite de bande passante, ou sur le réseau d’un autre fournisseur d’itinérance.

>[!NOTE]
>Connexions réseau contrôlé ont généralement latences aller-retour réseau qui sont inférieures à la valeur de latence de 35 millisecondes par défaut pour la transition vers le mode hors connexion (connexion lente) dans Windows 8, Windows Server 2012 et Windows Server 2016. Par conséquent, ces connexions généralement effectuer la transition vers le mode hors connexion (connexion lente) automatiquement.

### <a name="what-value-does-cost-aware-synchronization-add"></a>Quelle valeur ajouter par la synchronisation prenant en charge les coûts?

Synchronisation prenant en charge les coûts permet aux utilisateurs d’éviter les coûts d’utilisation de données anormalement élevée lors de l’utilisation des connexions qui ont des limites d’utilisation, ou pendant l’itinérance sur le réseau d’un autre fournisseur.

### <a name="how-has-cost-aware-synchronization-changed-things"></a>Comment prenant en charge les coûts synchronisation a changé choses?

Avant de Windows 8 et Windows Server 2012, les utilisateurs souhaitant réduire les frais lors de l’utilisation de fichiers hors connexion sur des connexions devaient suivre leur utilisation de données à l’aide des outils à partir du fournisseur de réseau mobile. Les utilisateurs peuvent ensuite manuellement basculer en mode hors connexion lorsqu’ils ont été itinérance, près de leur limite de bande passante, au-delà de leur limite.

Avec la synchronisation prenant en charge les coûts, Windows suit automatiquement les limites d’utilisation d’itinérance et la bande passante sur des connexions. Lorsque l’utilisateur se déplace vers leur limite de bande passante ou au-delà de leur limite, Windows bascule en mode hors connexion et empêche l’ensemble de la synchronisation. Les utilisateurs peuvent lancer manuellement la synchronisation, et les administrateurs peuvent écraser synchronisation prenant en charge les coûts pour des utilisateurs spécifiques, tels que des cadres.

Pour plus d’informations, voir [Activer arrière-plan la synchronisation des fichiers sur les réseaux mesuré](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj127408(v%3dws.11)).

## <a name="primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>Ordinateurs principales pour la Redirection de dossiers et les profils utilisateur itinérants

Vous pouvez désormais désigner un ensemble d’ordinateurs, appelés ordinateurs principales, pour chaque utilisateur de domaine, ce qui vous permet de contrôler les ordinateurs qui utilisent la Redirection de dossiers, les profils utilisateur itinérants ou les deux. Désignation des ordinateurs principales est une méthode simple et puissante pour associer les données utilisateur et des paramètres à particuliers ordinateurs ou périphériques, simplifier la supervision de l’administrateur, améliorer la sécurité des données et protéger les profils utilisateur à partir de corruption.

### <a name="what-value-do-primary-computers-add"></a>Quelle valeur ajouter des ordinateurs principales?

Il existe quatre principaux avantages pour désigner les ordinateurs principales pour les utilisateurs:

- L’administrateur peut spécifier les ordinateurs sur lesquels les utilisateurs peuvent utiliser pour accéder à leurs données redirigé et les paramètres. Par exemple, l’administrateur peut choisir de mettre en itinérance des données utilisateur et des paramètres entre les ordinateurs portables et de bureau d’un utilisateur et mettre en n'itinérance pas les informations lorsque cet utilisateur ouvre une session sur un autre ordinateur, par exemple un ordinateur de salle de conférence.
- Désignation des ordinateurs principales réduit la sécurité et les risques de confidentialité de quitter résiduelles données personnelles ou d’entreprise sur des ordinateurs où l’utilisateur a ouvert une session sur. Par exemple, un directeur général qui ouvre une session sur l’ordinateur d’un employé pour l’accès temporaire ne laisse derrière toutes les données personnelles ou d’entreprise.
- Ordinateurs principales permettent à l’administrateur réduire le risque d’une mauvaise configuration ou dans le cas contraire un profil endommagé, ce qui peut entraîner d’itinérance entre différemment n’est configuré systèmes, tels qu’entre les ordinateurs x86 et x64 64.
- La quantité de temps nécessaire pour l’ouverture d’un utilisateur première connexion sur un ordinateur non principal, tel un serveur, est plus rapide, car le profil utilisateur et/ou les dossiers redirigés de l’utilisateur ne sont pas téléchargés. Heures de déconnexion sont également réduits, car les modifications au profil de l’utilisateur n’avez pas besoin d’être téléchargés depuis le partage de fichiers.

### <a name="how-have-primary-computers-changed-things"></a>Comment les ordinateurs principales ont changé choses?

Pour limiter le téléchargement des données utilisateur privées sur les ordinateurs principales, les technologies de Redirection de dossiers et les profils utilisateur itinérants effectuer les vérifications de logique suivants lorsqu’un utilisateur se connecte à un ordinateur:

1. Le système d’exploitation Windows vérifie les nouveaux paramètres de stratégie de groupe (**profils itinérants télécharger uniquement les ordinateurs principal** et **redirection de dossiers sur des ordinateurs principales uniquement**) pour déterminer si l’attribut de l' **Attribut msDS-principal-ordinateur** actif Services de domaine Directory (AD DS) doivent influencer la décision de passer le profil utilisateur ou appliquer la Redirection de dossiers.
2. Si le paramètre de stratégie permet la prise en charge de l’ordinateur principal, Windows vérifie que le schéma de domaine Active Directory prend en charge l’attribut **MsDS-principal-ordinateur** . Si c’est le cas, Windows détermine si l’utilisateur ouvre une session sur l’ordinateur est désigné comme un ordinateur principal de l’utilisateur comme suit:
    1. Si l’ordinateur est un des ordinateurs principal de l’utilisateur, Windows applique les paramètres de Redirection de dossiers et les profils utilisateur itinérants.
    2. Si l’ordinateur n’est pas un des ordinateurs principal de l’utilisateur, Windows charge profil mis en cache local de l’utilisateur, s’il est présent, ou il crée un nouveau profil local. Windows supprime également tous les dossiers existants redirigés en fonction de l’action de suppression a été spécifiée par le paramètre de stratégie de groupe appliqué précédemment, qui est conservé dans la configuration de la Redirection de dossiers locale.

Pour plus d’informations, voir [Déployer des ordinateurs principal pour la Redirection de dossiers et les profils utilisateur itinérants](deploy-primary-computers.md)

## <a name="hardware-requirements"></a>Configuration matérielle requise

Redirection de dossiers, les fichiers hors connexion et les profils utilisateur itinérants requièrent un ordinateur x64 ou x86, et ils ne sont pas pris en charge par Windows sur les ordinateurs aoe ARM.

## <a name="software-requirements"></a>Configuration logicielle requise

Pour désigner les ordinateurs principales, votre environnement doit satisfaire les conditions suivantes:

- Le schéma des Services de domaine Active Directory (AD DS) doit être mis à jour pour inclure le schéma Windows Server 2012 et conditions (installation d’une version ultérieure contrôleur de domaine ou de Windows Server 2012 automatiquement des mises à jour le schéma). Pour plus d’informations sur la mise à niveau le schéma AD DS, voir [Mise à niveau des contrôleurs de domaine à Windows Server 2016](../../identity/ad-ds/deploy/upgrade-domain-controllers.md).
- Ordinateurs clients doivent exécuter Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 et être joints au domaine Active Directory que vous gérez.

## <a name="more-information"></a>Informations supplémentaires

Pour plus d’informations connexes, voir les ressources suivantes.

|Type de contenu|Références|
|---|---|
|Évaluation du produit|[Prise en charge des travailleurs de l’Information avec les Services de fichiers fiable et stockage](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831495(v%3dws.11)>)<br>[Nouveautés de fichiers en mode hors connexion](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff183315(v=ws.10)>) (Windows 7 et Windows Server 2008 R2)<br>[Nouveautés dans les fichiers en mode hors connexion pour Windows Vista](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc749449(v=ws.10)>)<br>[Modifications apportées aux fichiers en mode hors connexion dans Windows Vista](<https://technet.microsoft.com/library/2007.11.offline.aspx>) (TechNet Magazine)|
|Déploiement|[Déployer la Redirection de dossiers, des fichiers en mode hors connexion et les profils utilisateur itinérants](deploy-folder-redirection.md)<br>[Implémentation d’une Solution de centralisation de données de l’utilisateur final: Redirection de dossiers et de Validation de la technologie des fichiers en mode hors connexion et de déploiement](http://download.microsoft.com/download/3/0/1/3019A3DA-2F41-4F2D-BBC9-A6D24C4C68C4/Implementing%20an%20End-User%20Data%20Centralization%20Solution.docx)<br>[Gestion de Guide de déploiement de données utilisateur d’itinérance](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc766489(v=ws.10)>)<br>[Configuration de nouveau en mode hors connexion les fichiers de fonctionnalités pour Guide pas à pas d’ordinateurs Windows 7](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff633429(v=ws.10)>)<br>[À l’aide de la Redirection de dossiers](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753996(v=ws.11)>)<br>[L’implémentation de la Redirection de dossiers](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc737434(v=ws.10)>) (Windows Server 2003)|
|Outils et paramètres|[Fichiers en mode hors connexion sur MSDN](https://msdn.microsoft.com/library/cc296092.aspx)<br>[Référence de stratégie de groupe de fichiers en mode hors connexion](https://msdn.microsoft.com/library/ms878937.aspx) (Windows 2000)|
|Ressources de la communauté|[Forum sur le stockage et les services de fichiers](https://social.technet.microsoft.com/forums/windowsserver/home?forum=winserverfiles)<br>[Bonjour scripteur! Comment puis-je utiliser la fonctionnalité de fichiers hors connexion dans Windows?](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>)<br>[Bonjour scripteur! Comment puis-je activer et désactiver des fichiers en mode hors connexion?](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>)|
Technologies associées|[Identité et accès dans Windows Server](../../identity/identity-and-access.md)<br>[Stockage dans WindowsServer](../storage.md)<br>[Accès à distance et gestion de serveur](../../remote/index.md)|