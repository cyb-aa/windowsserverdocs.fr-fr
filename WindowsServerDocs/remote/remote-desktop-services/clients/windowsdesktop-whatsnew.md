---
title: Nouveautés du client Windows Desktop
description: Découvrir les dernières modifications apportées au client Bureau à distance pour Windows Desktop
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 03/10/2020
ms.localizationpriority: medium
ms.openlocfilehash: 981a9071b3f5daf1ca97dcde8a8b32b2a2588d99
ms.sourcegitcommit: fc900eb19ac26c3d6bc2de179cc4b2c1e971043e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79038062"
---
# <a name="whats-new-in-the-windows-desktop-client"></a>Nouveautés du client Windows Desktop

Vous trouverez des informations plus détaillées sur le client Windows Desktop dans la page [Bien démarrer avec le client Windows Desktop](windowsdesktop.md). Vous trouverez les dernières mises à jour du client ci-dessous.

## <a name="latest-client-versions"></a>Versions les plus récentes du client

Le client peut être configuré pour différents [groupes d’utilisateurs](windowsdesktop-admin.md#configure-user-groups). Le tableau suivant liste les versions actuelles disponibles pour chaque groupe d’utilisateurs :

|Groupe d’utilisateurs |Version  |
|-----------|---------|
|Public     |1.2.675  |
|Insider    |1.2.787  |

## <a name="updates-for-version-12787"></a>Mises à jour pour la version 1.2.787

*Date de publication : 10/03/2020*

Télécharger²: [Windows 64 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4qZVh), [Windows 32 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4qXhC), [Windows ARM64](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4qRVA)

- Renommage de l’action « Mettre à jour » pour les espaces de travail en « Actualiser » afin d’assurer la cohérence avec d’autres clients Bureau à distance.
- Vous pouvez maintenant actualiser un espace de travail directement à partir de son menu contextuel.
- L’actualisation manuelle d’un espace de travail garantit désormais que tout le contenu local est mis à jour.
- Vous pouvez maintenant réinitialiser les données utilisateur du client à partir de la page À propos de sans avoir besoin de désinstaller l’application.
- Vous pouvez également réinitialiser les données utilisateur du client à l’aide de msrdcw.exe /reset avec un paramètre /f facultatif pour ignorer l’invite.
- Nous recherchons désormais automatiquement une mise à jour du client lors de l’accès à la page À propos de.
- Mise à jour de la couleur des boutons pour des raisons de cohérence.

## <a name="updates-for-version-12675"></a>Mises à jour pour la version 1.2.675

*Date de publication : 25/02/2020*

Télécharger²: [Windows 64 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4qeak), [Windows 32 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4qm7h), [Windows ARM64](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4qm7g)

- Les connexions à Windows Virtual Desktop sont maintenant bloquées si le fichier RDP ne contient pas la signature ou si une des propriétés de signscope a été modifiée.
- Quand un espace de travail est vide ou a été supprimé, le centre de connexions n’apparaît vide.
- Ajout de l’ID d’activité et du code d’erreur dans les messages de déconnexion pour améliorer la résolution des problèmes. Vous pouvez copier le message de la boîte de dialogue avec **Ctrl+C**.
- Correction d’un problème qui provoquait la non-détection des écrans par les paramètres de connexion au Bureau.
- Les mises à jour du client ne redémarrent plus automatiquement le PC.
- Les icônes sans fenêtre ne doivent normalement plus apparaître dans la barre des tâches.

## <a name="updates-for-version-12605"></a>Mises à jour pour la version 1.2.605

*Date de publication : 29/01/2020*

Télécharger²: [Windows 64 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4oHrD), [Windows 32 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4oJZs), [Windows ARM64](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4oXhD)

- Vous pouvez maintenant sélectionner les affichages à utiliser pour les connexions Bureau à distance. Pour changer ce paramètre, cliquez avec le bouton droit sur l’icône de la connexion Bureau à distance, puis sélectionnez **Paramètres**.
- Correction d’un problème empêchant les paramètres de connexion d’afficher les facteurs de mise à l’échelle disponibles corrects.
- Correction d’un problème empêchant le Narrateur de lire la boîte de dialogue affichée au lancement de la connexion.
- Correction d’un problème entraînant l’affichage d’un nom d’utilisateur incorrect quand les noms Azure Active Directory et Active Directory ne correspondaient pas.
- Résolution d’un problème entraînant le blocage du client lors du lancement d’une connexion alors qu’il n’était pas connecté à un réseau.
- Correction d’un problème entraînant le blocage du client lors de l’attachement d’un casque.

## <a name="updates-for-version-12535"></a>Mises à jour pour la version 1.2.535

*Date de publication : 12/04/2019*

Télécharger²: [Windows 64 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4k7jH), [Windows 32 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4k7jL), [Windows ARM64](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE4k27O)

- Vous pouvez désormais accéder à ces informations relatives aux mises à jour directement à partir du bouton Autres options de la barre de commandes située en haut du client.
- Vous pouvez désormais soumettre des commentaires à partir de la barre de commandes du client.
- L’option Commentaires s’affiche à présent uniquement si le hub de commentaires est disponible.
- Vérifiez que la notification de mise à jour n’est pas affichée lorsque les notifications sont désactivées via la stratégie.
- Correction d’un problème qui empêchait le lancement de certains fichiers RDP.
- Correction d’un incident au démarrage du client causé par l’altération de certains paramètres persistants.

## <a name="updates-for-version-12431"></a>Mises à jour relatives à la version 1.2.431

*Date de publication : 12/11/2019*

Télécharger²: [Windows 64 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE48kow), [Windows 32 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE48koA), [Windows ARM64](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE48zYj)

- Les versions 32 bits et ARM64 du client sont disponibles !
- Le client enregistre désormais les changements que vous apportez à la barre de connexion (par exemple, sa position, sa taille et son état épinglé) et les applique aux différentes sessions.
- Mise à jour des informations de passerelle et des boîtes de dialogue d’état de connexion.
- Résolution d’un problème qui entraînait l’invite de deux jeux d’informations d’identification en même temps lors de la tentative de connexion après l’expiration du jeton Azure Active Directory.
- Sur Windows 7, les utilisateurs sont maintenant invités correctement à fournir des informations d’identification s’ils en avaient enregistrées quand le serveur les interdit.
- L’invite Azure Active Directory s’affiche à présent devant la fenêtre de connexion lors de la reconnexion.
- Les éléments épinglés à la barre des tâches sont maintenant mis à jour pendant une actualisation de flux.
- Amélioration du défilement dans le centre de connexion sur écran tactile.
- Suppression de la ligne vide dans le menu déroulant de résolution.
- Suppression des entrées inutiles dans le Gestionnaire d’informations d’identification Windows.
- Les sessions de bureau sont maintenant à la bonne taille quand vous quittez le mode plein écran.
- La boîte de dialogue de déconnexion de RemoteApp apparaît désormais au premier plan quand vous reprenez votre session après être entré en mode veille.
- Résolution de problèmes d’accessibilité comme la navigation au clavier.

## <a name="updates-for-version-12247"></a>Mises à jour relatives à la version 1.2.247

*Date de publication : 17/09/2019*

Télécharger²: [Windows 64 bits](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3LkSa)

- Amélioration des langues de base pour la version localisée. (Par exemple, FR-CA affiche bien le texte en français et non en anglais.)
- Lors de la suppression d’un abonnement, le client supprime désormais correctement les informations d’identification enregistrées du Gestionnaire d’informations d’identification.
- Le processus de mise à jour du client est maintenant sans assistance une fois démarré, et le client est relancé une fois l’opération terminée.
- Le client peut désormais être utilisé sur Windows 10 en mode S.
- Correction d’un problème qui provoquait l’échec du processus de mise à jour pour les utilisateurs dont le nom comprenait un espace.
- Correction d’un plantage qui se produisait lors de l’authentification pendant une connexion.
- Correction d’un plantage qui se produisait lors de la fermeture du client.
