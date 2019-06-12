---
title: Vue d’ensemble de la redirection de dossiers, des fichiers hors connexion et des profils utilisateurs itinérants
description: Vue d’ensemble des technologies de Redirection de dossiers, fichiers hors connexion et les profils utilisateur itinérants.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 89506d0f7445f0df230945f45a31d4f58390c5c1
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812421"
---
# <a name="folder-redirection-offline-files-and-roaming-user-profiles-overview"></a>Vue d’ensemble de la redirection de dossiers, des fichiers hors connexion et des profils utilisateurs itinérants

>S’applique à : Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2

Cette rubrique traite de la Redirection de dossiers, fichiers hors connexion (mise en cache côté client ou CSC) et les technologies les profils utilisateur itinérants (parfois appelé RUP), y compris les fonctionnalités nouvelles et où trouver des informations supplémentaires.

## <a name="technology-description"></a>Description de la technologie

La redirection de dossiers et les fichiers hors connexion sont utilisés ensemble pour rediriger le chemin d’accès des dossiers locaux (tels que le dossier Documents) vers un emplacement réseau, tout en mettant en cache le contenu localement pour augmenter la vitesse et la disponibilité. Les profils utilisateurs itinérants permettent de rediriger un profil utilisateur vers un emplacement réseau. Ces fonctionnalités étaient référencées sous le nom d’Intellimirror.

- **La Redirection de dossiers** permet aux utilisateurs et aux administrateurs de rediriger le chemin d’accès d’un dossier connu vers un nouvel emplacement, manuellement ou en utilisant la stratégie de groupe. Le nouvel emplacement peut être un dossier sur l’ordinateur local ou un répertoire sur un partage de fichiers. Les utilisateurs interagissent avec les fichiers dans le dossier redirigé comme s’il se trouvait toujours sur le lecteur local. Par exemple, vous pouvez rediriger le dossier Documents, généralement stocké sur un lecteur local, vers un emplacement réseau. Les fichiers figurant dans le dossier sont alors accessibles à l’utilisateur à partir de n’importe quel ordinateur sur le réseau.
- **Fichiers hors connexion** rend les fichiers réseau accessibles à un utilisateur, même si la connexion réseau au serveur est lente ou non disponible. Lorsque vous travaillez en ligne, les performances d’accès aux fichiers dépendent de la vitesse du réseau et du serveur. Lorsque vous travaillez hors connexion, les fichiers sont récupérés du dossier Fichiers hors connexion aux vitesses d’accès local. Un ordinateur passe en mode hors connexion lorsque :
  - **Toujours hors connexion** mode a été activé
  - le serveur est indisponible ;
  - la connexion réseau est plus lente qu’un seuil configurable ;
  - l’utilisateur bascule manuellement en mode hors connexion à l’aide du bouton **Travailler hors connexion** dans l’Explorateur Windows.
- **Les profils utilisateur itinérants** redirigent les profils utilisateurs vers un partage de fichiers afin que les utilisateurs reçoivent le même système d’exploitation et les paramètres de l’application sur plusieurs ordinateurs. Lorsqu’un utilisateur se connecte à un ordinateur en utilisant un compte configuré avec un partage de fichiers comme chemin d’accès au profil, le profil de l’utilisateur est téléchargé sur l’ordinateur local et fusionné avec le profil local (le cas échéant). Lorsque l’utilisateur ferme la session sur l’ordinateur, la copie locale de son profil, y compris toutes les modifications, est fusionnée avec la copie serveur du profil. En règle générale, un administrateur réseau permet les profils utilisateur itinérants sur les comptes de domaine.

## <a name="practical-applications"></a>Cas pratiques

Les administrateurs peuvent utiliser la redirection de dossiers, les fichiers hors connexion et les profils utilisateurs itinérants pour centraliser le stockage des paramètres et données utilisateur et pour fournir aux utilisateurs la capacité d’accéder à leurs données en mode hors connexion ou dans le cas d’une indisponibilité du réseau ou d’une panne du serveur. Certaines applications spécifiques incluent les fonctionnalités suivantes :

- centraliser les données issues des ordinateurs clients pour les tâches administratives, telles que l’utilisation d’un outil de sauvegarde serveur pour sauvegarder les paramètres et dossiers utilisateur ;
- permettre aux utilisateurs de continuer à accéder aux fichiers réseau, même en cas d’indisponibilité du réseau ou de panne du serveur ;
- optimiser l’utilisation de la bande passante et améliorer l’expérience des utilisateurs des filiales qui accèdent à des fichiers et des dossiers hébergés par les serveurs d’entreprise situés hors site ;
- permettre aux utilisateurs itinérants d’accéder aux fichiers réseau lorsqu’ils travaillent hors connexion ou sur des réseaux lents.

## <a name="new-and-changed-functionality"></a>Fonctionnalités nouvelles et modifiées

Le tableau ci-dessous décrit quelques-unes des principales modifications apportées aux fonctionnalités de redirection de dossiers, fichiers hors connexion et profils utilisateurs itinérants, disponibles dans cette version.

| Fonctionnalité/fonction | Nouveauté ou mise à jour ? | Description |
| --- | --- | --- |
| Mode Toujours hors connexion | Nouveau | Assure un accès plus rapide aux fichiers et une utilisation réduite de la bande passante en travaillant toujours hors connexion, même lors d’une connexion via une connexion réseau haut débit. |
| Synchronisation prenant en charge les coûts | Nouveau | Aide les utilisateurs à éviter des coûts élevés d’utilisation de données de synchronisation lors de l’utilisation de connexions limitées dotées de limites d’utilisation ou lors d’une itinérance sur le réseau d’un autre fournisseur. |
| Prise en charge des ordinateurs principaux | Nouveau | Permet de limiter l’utilisation de la redirection de dossiers et/ou des profils utilisateurs itinérants aux ordinateurs principaux d’un utilisateur. |

## <a name="always-offline-mode"></a>Mode Toujours hors connexion

À partir de Windows 8 et Windows Server 2012, les administrateurs peuvent configurer l’expérience des utilisateurs des fichiers hors connexion toujours travailler hors connexion, même lorsqu’ils sont connectés via une connexion réseau haut débit. Windows met à jour les fichiers dans le cache des fichiers hors connexion en les synchronisant toutes les heures en arrière-plan, par défaut.

### <a name="what-value-does-always-offline-mode-add"></a>Quelle valeur est toujours hors connexion en mode ajouter ?

Le mode Toujours hors connexion présente les avantages suivants :

- les utilisateurs bénéficient d’un accès plus rapide aux fichiers figurant dans les dossiers redirigés, tels que le dossier Documents ;
- la bande passante réseau est réduite, ce qui réduit les coûts des connexions WAN ou des connexions limitées onéreuses, telles qu’un réseau mobile 4G.

### <a name="how-has-always-offline-mode-changed-things"></a>Comment le mode toujours hors connexion a changé choses ?

Avant Windows 8, Windows Server 2012, les utilisateurs effectuaient la transition entre les modes en ligne et hors connexion, en fonction de la disponibilité du réseau et des conditions, même lorsque le mode de liaison lente (également appelé mode connexion lente) était activé et paramétré à une 1 milliseconde. seuil de latence.

Avec le mode toujours hors connexion, les ordinateurs ne passent jamais en mode en ligne lorsque la **configurer le mode de liaison lente** paramètre de stratégie de groupe est configuré et le **latence** paramètre de seuil est défini sur 1 milliseconde. Les modifications sont synchronisées en arrière-plan toutes les 120 minutes, par défaut, mais la synchronisation est configurable à l’aide du paramètre de stratégie de groupe **Configurer la synchronisation en arrière-plan**.

Pour plus d'informations, voir [Enable the Always Offline Mode to Provide Faster Access to Files](enable-always-offline.md).

## <a name="cost-aware-synchronization"></a>Synchronisation prenant en charge les coûts

Avec la synchronisation prenant en charge les coûts, Windows désactive la synchronisation en arrière-plan lorsque l’utilisateur utilise une connexion réseau limitée, telle qu’un réseau mobile 4G, et si l’abonné est près ou au-delà de la limite de bande passante ou itinérant sur le réseau d’un autre fournisseur.

> [!NOTE]
> Connexions réseau limitées possèdent en général des latences réseau de parcours circulaire plus lentes que la valeur de latence de 35 millisecondes par défaut pour la transition en mode hors connexion (connexion lente) dans Windows 8, Windows Server 2019, Windows Server 2016 et Windows Server 2012. Par conséquent, ces connexions effectuent en général une transition automatique vers le mode Hors connexion (connexion lente).

### <a name="what-value-does-cost-aware-synchronization-add"></a>Quels avantages synchronisation prenant en charge pour un coût procure-t-elle ?

La synchronisation prenant en charge les coûts aide les utilisateurs à éviter des coûts élevés imprévisibles d’utilisation de données lors de l’utilisation de connexions limitées dotées de limites d’utilisation ou lors d’une itinérance sur le réseau d’un autre fournisseur.

### <a name="how-has-cost-aware-synchronization-changed-things"></a>Comment la synchronisation prenant en charge de coût a changé choses ?

Avant Windows 8 et Windows Server 2012, les utilisateurs qui souhaitent réduire les frais lors de l’utilisation de fichiers hors connexion sur des connexions réseau limitées devaient effectuer le suivi de leur utilisation des données à l’aide d’outils provenant du fournisseur de réseau mobile. Les utilisateurs pouvaient alors basculer manuellement en mode Hors connexion lorsqu’ils étaient en itinérance, proches de leur limite de bande passante ou au-delà de cette limite.

Avec la synchronisation prenant en charge les coûts, Windows effectue automatiquement le suivi des limites d’utilisation itinérants et la bande passante sur des connexions limitées. Lorsque l’utilisateur est itinérant, proche de sa limite de bande passante ou au-delà de cette limite, Windows bascule en mode hors connexion et empêche toute synchronisation. Les utilisateurs peuvent encore initier manuellement la synchronisation et les administrateurs peuvent remplacer la synchronisation prenant en charge les coûts pour des utilisateurs spécifiques, tels que les dirigeants.

Pour plus d'informations, voir [Enable Background File Synchronization on Metered Networks](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj127408(v%3dws.11)).

## <a name="primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>Ordinateurs principaux pour la redirection de dossiers et les profils utilisateurs itinérants

Vous pouvez désormais désigner un ensemble d’ordinateurs, appelés ordinateurs principaux, pour chaque utilisateur de domaine, ce qui permet de contrôler quels ordinateurs utilisent la Redirection de dossiers, les profils utilisateur itinérants ou les deux. La désignation des ordinateurs principaux est une méthode simple et efficace qui permet d’associer les paramètres et les données utilisateur à des périphériques ou des ordinateurs particuliers, de simplifier la supervision de l’administrateur, d’améliorer la sécurité des données et d’aider à protéger les profils utilisateurs contre d’éventuels dommages.

### <a name="what-value-do-primary-computers-add"></a>Quelle valeur ajoutent-ils des ordinateurs principaux ?

La désignation d’ordinateurs principaux pour les utilisateurs présente quatre avantages principaux :

- L’administrateur peut spécifier les ordinateurs que les utilisateurs peuvent utiliser pour accéder à leurs données et paramètres redirigés. Par exemple, l'administrateur peut choisir de rendre itinérants les paramètres et données utilisateur entre l'ordinateur de bureau et l'ordinateur portable d'un utilisateur, et de ne pas rendre itinérantes ces informations lorsque cet utilisateur ouvre une session sur tout autre ordinateur, tel que l'ordinateur d'une salle de conférence.
- La désignation d’ordinateurs principaux réduit les risques en matière de sécurité et de confidentialité liés au fait de laisser des données personnelles ou d’entreprise résiduelles sur des ordinateurs où l’utilisateur a ouvert une session. Par exemple, un directeur général qui ouvre une session sur l’ordinateur d’un de ses employés pour bénéficier d’un accès temporaire ne laisse derrière lui aucune donnée personnelle ni d’entreprise.
- Les ordinateurs principaux permettent à l’administrateur de limiter le risque lié à un profil configuré de façon incorrecte ou endommagé, qui pourrait résulter de l’itinérance entre des systèmes configurés de manières différentes, tels que des ordinateurs avec architecture x64 et x86.
- Le temps requis pour la première connexion d’un utilisateur sur un ordinateur non principal, tel qu’un serveur, est moindre car le profil utilisateur itinérant de l’utilisateur et/ou les dossiers redirigés ne sont pas téléchargés. Les temps de déconnexion sont également réduits, car les modifications du profil utilisateur ne sont pas tenues d’être téléchargées vers le partage de fichiers.

### <a name="how-have-primary-computers-changed-things"></a>Comment les choses ont changé ordinateurs principaux ?

Pour restreindre le téléchargement des données utilisateur privées vers les ordinateurs principaux, les technologies de redirection de dossiers et de profils utilisateurs itinérants effectuent les vérifications logiques suivantes lorsqu’un utilisateur se connecte à un ordinateur :

1. Le système d’exploitation Windows vérifie les nouveaux paramètres de stratégie de groupe (**Télécharger les profils itinérants sur les ordinateurs principaux uniquement** et **Rediriger les dossiers uniquement sur les ordinateurs principaux**) pour déterminer si l’attribut **msDS-Primary-Computer** dans les services de domaine Active Directory (AD DS) doit influencer la décision de rendre itinérant le profil utilisateur ou d’appliquer la redirection de dossiers.
2. Si le paramètre de stratégie active la prise en charge des ordinateurs principaux, Windows vérifie que le schéma AD DS prend en charge l’attribut **msDS-Primary-Computer**. Si tel est le cas, Windows détermine si l’ordinateur sur lequel l’utilisateur ouvre une session est désigné comme ordinateur principal pour l’utilisateur, comme suit :
    1. Si l’ordinateur est l’un des ordinateurs principaux de l’utilisateur, Windows applique les paramètres des profils utilisateurs itinérants et de redirection de dossiers.
    2. Si l’ordinateur n’est pas l’un des ordinateurs principaux de l’utilisateur, Windows charge le profil local mis en cache de l’utilisateur, s’il est présent, ou crée un nouveau profil local. Windows supprime également tous les dossiers redirigés existants conformément à l’action de suppression qui a été spécifiée par le paramètre de stratégie de groupe précédemment appliqué, qui est conservé dans la configuration locale de la redirection de dossiers.

Pour plus d’informations, voir [Déployer des ordinateurs principaux pour la redirection de dossiers et les profils utilisateurs itinérants](deploy-primary-computers.md)

## <a name="hardware-requirements"></a>Configuration matérielle requise

La redirection de dossiers, les fichiers hors connexion et les profils utilisateurs itinérants requièrent un ordinateur avec architecture x64 ou x86, et ils ne sont pas pris en charge par les ordinateurs avec architecture Windows sur ARM (WOA).

## <a name="software-requirements"></a>Configuration logicielle requise

Pour désigner les ordinateurs principaux, votre environnement doit répondre aux exigences suivantes :

- Le schéma des Services de domaine Active Directory (AD DS) doit être mis à jour pour inclure des conditions générales de schéma Windows Server 2012 (installation d’un Windows Server 2012 ou un contrôleur de domaine ultérieur automatiquement des mises à jour le schéma). Pour plus d’informations sur la mise à niveau le schéma AD DS, consultez [mise à niveau des contrôleurs de domaine vers Windows Server 2016](../../identity/ad-ds/deploy/upgrade-domain-controllers.md).
- Les ordinateurs clients doivent exécuter Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 et être joints au domaine Active Directory que vous gérez.

## <a name="more-information"></a>Informations supplémentaires

Pour plus d’informations connexes, voir les ressources suivantes.

| Type de contenu | Références |
| --- | --- |
| Évaluation du produit | [Prise en charge des travailleurs de l’Information avec les Services de fichiers fiable et de stockage](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831495(v%3dws.11)>)<br>[Quelles sont les nouveautés dans les fichiers hors connexion](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff183315(v=ws.10)>) (Windows 7 et Windows Server 2008 R2)<br>[Quelles sont les nouveautés des fichiers hors connexion pour Windows Vista](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc749449(v=ws.10)>)<br>[Modifications apportées aux fichiers hors connexion dans Windows Vista](<https://technet.microsoft.com/library/2007.11.offline.aspx>) (Magazine TechNet) |
| Déploiement | [Déployer la Redirection de dossiers, fichiers hors connexion et les profils utilisateur itinérants](deploy-folder-redirection.md)<br>[Implémentation d’une Solution de centralisation des données de l’utilisateur final : La Redirection de dossiers et de Validation de la technologie fichiers hors connexion et de déploiement](http://download.microsoft.com/download/3/0/1/3019A3DA-2F41-4F2D-BBC9-A6D24C4C68C4/Implementing%20an%20End-User%20Data%20Centralization%20Solution.docx)<br>[La gestion itinérance Guide de déploiement de données utilisateur](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc766489(v=ws.10)>)<br>[Guide pas à pas de la configuration des nouvelles fonctionnalités Fichiers hors connexion pour les ordinateurs Windows 7](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff633429(v=ws.10)>)<br>[À l’aide de la Redirection de dossiers](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753996(v=ws.11)>)<br>[Implémentation de la Redirection de dossiers](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc737434(v=ws.10)>) (Windows Server 2003) |
| Outils et paramètres | [Fichiers hors connexion sur MSDN](https://msdn.microsoft.com/library/cc296092.aspx)<br>[Référence de stratégie de groupe de fichiers hors connexion](https://msdn.microsoft.com/library/ms878937.aspx) (Windows 2000) |
| Ressources de la communauté | [Forum sur le stockage et les Services de fichiers](https://social.technet.microsoft.com/forums/windowsserver/home?forum=winserverfiles)<br>[Hey, Scripting Guy ! Comment puis-je travailler avec la fonctionnalité fichiers hors connexion dans Windows ?](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>)<br>[Hey, Scripting Guy ! Comment puis-je activer et désactiver la fonctionnalité fichiers hors connexion ?](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>) |
| Technologies associées|[Identité et accès dans Windows Server](../../identity/identity-and-access.md)<br>[Stockage dans Windows Server](../storage.md)<br>[Gestion d’accès et le serveur à distance](../../remote/index.md) |