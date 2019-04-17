---
title: Vous familiariser avec les services Bureau à distance sur Windows
description: Configurer les étapes pour le client Bureau à distance pour Windows de base.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 64f038e1-40ec-4c67-938b-72edea49e5d8
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 05/07/2018
ms.localizationpriority: medium
ms.openlocfilehash: 1c06e2eca725a825ac0fa4c7b617a26d89c898ff
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297457"
---
# Vous familiariser avec les services Bureau à distance sur Windows

>S’applique à: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Vous pouvez utiliser le client Bureau à distance pour Windows pour fonctionner avec les applications Windows et les ordinateurs de bureau à distance à partir d’un autre appareil Windows.

Utilisez les informations suivantes pour commencer. Veillez à examiner le [Forum aux questions sur](remote-desktop-client-faq.md) si vous avez des questions.

> [!NOTE]
> - Curieux de savoir sur les nouvelles versions du client Windows? Consultez l’article [Nouveautés pour les services Bureau à distance sur Windows?](windows-whatsnew.md)
> - Vous pouvez exécuter le client sur n’importe quelle version de Windows 10.

## Obtenir le client Bureau à distance et démarrer l’utiliser

Suivez ces étapes pour vous familiariser avec les services Bureau à distance sur votre appareil Windows 10:

1. Télécharger le client Bureau à distance à partir de [Microsoft Store](https://www.microsoft.com/store/p/microsoft-remote-desktop/9wzdncrfj3ps). 
2. [Configuration de votre PC pour accepter les connexions à distance](remote-desktop-allow-access.md).
3. Ajouter une connexion Bureau à distance ou une ressource distante. Vous utilisez une connexion pour se connecter directement à un PC Windows et une ressource distante d’utiliser un programme RemoteApp, basée sur la session de bureau ou bureau virtuel publiées par votre administrateur. 
4. Épingler des éléments que vous puissiez vous faire rapidement au Bureau à distance.

### Ajouter une connexion Bureau à distance

Pour créer une connexion Bureau à distance:

1. Dans le centre de connexion appuyez sur **+ Ajouter**et puis appuyez sur le **Bureau**.
2. Entrez les informations suivantes pour l’ordinateur que vous souhaitez vous connecter:
  - **Nom du PC** – le nom de l’ordinateur. Cela peut être un nom d’ordinateur Windows, un nom de domaine Internet ou une adresse IP. Vous pouvez également ajouter des informations de port pour le nom du PC (par exemple, **MyDesktop:3389** ou **10.0.0.1:3389**).
  - **Compte d’utilisateur** – le compte d’utilisateur à utiliser pour accéder à l’ordinateur distant. Appuyez sur **+** pour ajouter un nouveau compte ou sélectionner un compte existant. Vous pouvez utiliser les formats suivants pour le nom d’utilisateur: *nom_utilisateur*, *domaine\nom_utilisateur*ou *user_name@domain.com*. Vous pouvez également spécifier si vous souhaitez demander un nom d’utilisateur et mot de passe lors de la connexion en sélectionnant **me demander à chaque fois**.
3. Vous pouvez également définir des options supplémentaires appuyant sur **Afficher plus de**:
  - **Nom d’affichage** : un nom facile à retenir pour le PC que vous vous connectez à. Vous pouvez utiliser n’importe quelle chaîne, mais si vous ne spécifiez pas un nom convivial, le nom du PC s’affiche.
  - **Groupe** – permet de spécifier un groupe pour faciliter le retrouver vos connexions. Vous pouvez ajouter un nouveau groupe en appuyant sur **+** ou sélectionnez l’une à partir de la liste.
  - **Passerelle** – passerelle le Bureau à distance que vous souhaitez utiliser pour se connecter à des bureaux virtuels, des programmes RemoteApp et basée sur la session de postes de travail sur un réseau d’entreprise interne. Obtenir les informations sur la passerelle de votre administrateur système.
  - **Se connecter à la session de l’administrateur** - Utilisez cette option pour se connecter à une session de console pour administrer un serveur Windows.
  - **Les boutons de la souris échange** : utilisez cette option pour inverser des fonctions de bouton de la souris pour le bouton droit de la souris. (Cela est particulièrement utile si le PC distant est configuré pour un utilisateur gaucher, mais que vous utilisez une souris droitier.)
  - **Définir mes résolution de la session à distance:** : sélectionnez la résolution que vous souhaitez utiliser dans la session. **Choisissez me** définira la résolution basée sur la taille du client.
  - **Modifier la taille de l’écran:** – lorsque vous sélectionnez une haute résolution statique pour la session, vous avez la possibilité d’effectuer des éléments à l’écran apparaissent plus gros pour améliorer la lisibilité. Remarque: Cela s’applique uniquement lorsque vous vous connectez à Windows 8.1 ou version ultérieure.
  - **Mettre à jour de la session à distance résolution sur Redimensionner** – lorsqu’elle est activée, le client met à jour dynamiquement la résolution de session basée sur la taille du client. Remarque: Cela s’applique uniquement lorsque vous vous connectez à Windows 8.1 ou version ultérieure.
  - **Presse-papiers** – lorsqu’elle est activée, vous permet de copier le texte et les images vers/depuis l’ordinateur distant.
  - **La lecture audio** : sélectionner l’appareil à utiliser pour l’audio au cours de votre session à distance. Vous pouvez choisir de lire du contenu audio sur les appareils locales, l’ordinateur distant, ou pas du tout.
  - **Enregistrement audio** – lorsqu’elle est activée, vous permet d’utiliser un microphone local avec des applications sur l’ordinateur distant.
4. Appuyez sur **Enregistrer**.

Vous avez besoin de modifier ces paramètres? Cliquez sur le menu de dépassement (**...**) en regard du nom du bureau, puis **Modifier**.

Vous voulez supprimer la connexion? Là encore, cliquez sur le menu de dépassement de capacité (**...**) et appuyez sur **Supprimer**.

### Ajouter une ressource à distance
Ressources distantes sont des programmes RemoteApp, postes de travail basée sur la session et les bureaux virtuels publiées par votre administrateur à l’aide des Services Bureau à distance.

Pour ajouter une ressource à distance:

1. Sur l’écran de connexion centre, appuyez sur **+ Ajouter**et appuyez sur **des ressources distantes**. 
2. Entrez l' **URL du flux** fournie par votre administrateur et appuyez sur **Rechercher des flux**.
3. Lorsque vous y êtes invité, fournissez les informations d’identification à utiliser pour vous abonner au flux.

Les ressources distantes seront affichera dans le centre de la connexion.


Pour supprimer les ressources distantes:

1. Dans le centre de la connexion, cliquez sur le menu de dépassement de capacité (**...**) en regard de la ressource à distance.
2. Cliquez sur **Supprimer**.

### Épingler un ordinateur de bureau enregistré votre menu Démarrer

Pour épingler une connexion à votre menu Démarrer, cliquez sur le menu de dépassement (**...**) en regard du nom du bureau, puis **épingler au menu Démarrer**.

Vous pouvez maintenant commencer la connexion Bureau à distance directement à partir de votre menu Démarrer en appuyant dessus.

## Se connecter à une passerelle des services Bureau à distance pour accéder à des ressources internes

Une passerelle des services Bureau à distance (passerelle des services Bureau à distance) vous permet de vous connecter à un ordinateur distant sur un réseau d’entreprise à partir de n’importe où sur Internet. Vous pouvez créer et gérer vos passerelles à l’aide du client Bureau à distance.

Pour configurer une nouvelle passerelle:

1. Dans le centre de la connexion, appuyez sur **paramètres**.
2. En regard de la passerelle, appuyez sur **+** pour ajouter une nouvelle passerelle. Remarque: Une passerelle peut également être ajoutée lorsque vous ajoutez une nouvelle connexion.
3. Entrez les informations suivantes:
  - **Nom du serveur** : le nom de l’ordinateur que vous souhaitez utiliser sous forme de passerelle. Cela peut être un nom d’ordinateur Windows, un nom de domaine Internet ou une adresse IP. Vous pouvez également ajouter des informations de port pour le nom du serveur (par exemple: **RDGateway:443** ou **10.0.0.1:443**).
  - **Compte d’utilisateur** : sélectionnez ou ajoutez un compte d’utilisateur à utiliser avec la passerelle des services Bureau à distance vous vous connectez à. Vous pouvez également sélectionner **utiliser le compte d’utilisateur de bureau** pour utiliser les mêmes informations d’identification que ceux utilisés pour la connexion Bureau à distance.
4. Appuyez sur **Enregistrer**.  

## Paramètres d’application général

Vous pouvez définir les paramètres globaux suivants dans votre client en appuyant **paramètres**:

GÉRÉ DES ÉLÉMENTS
- **Compte d’utilisateur** : vous permet d’ajouter, modifier et supprimer des comptes d’utilisateur enregistrés dans le client. Il s’agit d’un bon moyen pour mettre à jour le mot de passe d’un compte après que qu’il a changé.
- **Passerelle** - vous permet d’ajouter, modifier et supprimer des serveurs de passerelle enregistrés dans le client.
- **Groupe** - vous permet d’ajouter, modifier et supprimer des groupes enregistrés dans le client. Ils permettent aux facilement les connexions de groupe.

PARAMÈTRES DE SESSION
- Pas **Démarrer des connexions en plein écran** , lorsqu’elle est activée, chaque fois qu’une connexion est lancée, le client utilisera la totalité de l’écran de l’écran actuel.
- Pas **Démarrer chaque connexion dans une nouvelle fenêtre** , lorsqu’elle est activée, chaque connexion est lancée dans une fenêtre distincte, ce qui vous permet de les placer sur plusieurs écrans et de passer à l’aide de la barre des tâches.
- **Lors du redimensionnement de l’application:** -permet de contrôler ce qui se produit lorsque la fenêtre du client est redimensionnée. Par défaut, **Étirer le contenu, en conservant ses proportions**.
- **Utiliser les commandes de clavier avec:** -nous allons vous spécifiez où les commandes du clavier comme *WIN* ou *ALT + TAB* sont utilisés. La valeur par défaut consiste à les envoyer uniquement à la session lorsque la connexion est scren complet.
- **Empêcher l’écran à partir de l’expiration du délai** - vous permet de maintenir l’écran à partir de l’expiration du délai lorsqu’une session est active. Ceci est utile lors de la connexion ne nécessite pas d’une interaction pendant de longues périodes de temps.

PARAMÈTRES D’APPLICATION
- **Afficher les aperçus de bureau** - vous permet d’afficher un aperçu d’un ordinateur de bureau dans le centre de la connexion avant de vous connecter à celui-ci. Par défaut, cela est définie sur **activé**.
- **Améliorer les services Bureau à distance** - envoie des données anonymes à Microsoft. Nous utilisons ces données pour améliorer le client. Pour en savoir plus sur la manière dont nous traiter ces données anonymes, privées, consultez la [Déclaration de confidentialité de Microsoft](https://privacy.microsoft.com/en-us/privacystatement). Par défaut, ce paramètre est **activé**.

### Gérer vos comptes d’utilisateurs

Lorsque vous vous connectez à un ordinateur de bureau ou à distance des ressources, vous pouvez enregistrer les comptes d’utilisateur pour sélectionner à partir d’une nouvelle fois. Vous pouvez également définir des comptes d’utilisateur dans le client lui-même, par opposition à l’enregistrement des données utilisateur lorsque vous vous connectez à un ordinateur de bureau.

Pour créer un compte d’utilisateur:

1. Dans le centre de la connexion, appuyez sur **paramètres**.
2. En regard du compte d’utilisateur, appuyez sur **+** pour ajouter un compte d’utilisateur.
3. Entrez les informations suivantes:
   - **Nom d’utilisateur** : le nom de l’utilisateur à enregistrer pour une utilisation avec une connexion à distance. Vous pouvez entrer le nom d’utilisateur dans un des formats suivants: nom_utilisateur, DOMAINE\nom_utilisateur, ou user_name@domain.com.
   - **Mot de passe** - le mot de passe pour l’utilisateur que vous avez spécifié. Cela peut être laissée vide être invité à entrer un mot de passe lors de la connexion.
4. Appuyez sur **Enregistrer**.

Pour supprimer un compte d’utilisateur:

1. Dans le centre de la connexion, appuyez sur **paramètres**.
2. Sélectionnez le compte à supprimer de la liste sous le compte d’utilisateur.
3. En regard du compte d’utilisateur, appuyez sur l’icône d’édition.
4. Cliquez sur **Supprimer ce compte** dans la partie inférieure pour supprimer le compte d’utilisateur.
5. Vous pouvez également modifier le compte d’utilisateur et appuyez sur **Enregistrer**.

## Accédez à la session Bureau à distance
Lorsque vous démarrez une connexion Bureau à distance, il existe des outils que vous pouvez utiliser pour naviguer dans la session.

### Démarrer une connexion Bureau à distance

1. Appuyez sur la connexion Bureau à distance pour démarrer la session.
2. Si vous n’avez pas encore enregistré les informations d’identification pour la connexion, vous devrez fournir un **nom d’utilisateur** et un **mot de passe**.
3. Si vous êtes invité à vérifier le certificat pour le Bureau à distance, passez en revue les informations et vérifiez que cela est un PC vous faites confiance à avant d’appuyer sur **se connecter**. Vous pouvez également sélectionner **ne demandez pas sur ce certificat à nouveau** toujours accepter ce certificat.

### Barre de connexion

La connexion barre vous donne qu'accès aux contrôles de navigation supplémentaires. Par défaut, la barre de connexion est placée au milieu du haut de l’écran. Appuyez sur et faites glisser la barre vers la gauche ou la droite pour le déplacer.

- **Contrôle Panorama** - le contrôle Panorama permet à l’écran être agrandi et déplacé. Notez que contrôle Panorama est uniquement disponible sur les appareils tactiles et l’utilisation du mode tactile direct.
   - Activer / désactiver le contrôle Panorama: appuyez sur l’icône de panoramique dans la barre de connexion pour afficher le contrôle Panoramique et Zoom sur l’écran. Appuyez sur l’icône de panoramique dans la barre de connexion à nouveau pour masquer le contrôle et renvoyer l’écran à sa résolution d’origine.
   - Utilisez le contrôle Panorama - appui, le contrôle Panorama de mise en attente et faites-le glisser dans la direction que vous voulez déplacer l’écran.
   - Déplacez le contrôle Panorama - appuyez deux fois et le contrôle Panorama pour déplacer le contrôle sur l’écran de mise en attente.
- **Des options supplémentaires** - sur l’icône des options supplémentaires pour afficher la sélection de session de la barre et de commande de la barre (voir ci-dessous).
- **Clavier** - sur l’icône de clavier pour afficher ou masquer le clavier visuel. Le contrôle Panorama s’affiche automatiquement lorsque le clavier s’affiche.

### Barre de commandes

Appuyez sur le bouton **...** sur la barre de connexion pour afficher la barre de commandes sur le côté droit de l’écran.

- **Édition familiale** - utilisez le bouton Accueil pour revenir au centre de connexion à partir de la barre de commandes.
  - Vous pouvez également utiliser le bouton Précédent pour la même action.
  - Votre session active ne sera pas déconnectée.
  - Cela vous permet de lancer des connexions supplémentaires.
- **Se déconnecter** - utilisez le bouton de déconnexion pour mettre fin à la connexion.
  - Vos applications reste actives tant que la session n’est pas terminée sur le PC distant.
- **Mode plein écran** - entre ou quitte le mode plein écran.
- **Tactile / de la souris** : vous pouvez basculer entre les modes de souris (Touch Direct et le pointeur de souris).

### Utilisez direct touch mouvements et modes de la souris dans une session à distance

Deux modes de souris sont disponibles pour interagir avec la session.
- **Tactile direct**: transmet tous les contacts tactiles à la session doit être interprété à distance.
  - Utilisée de la même manière que vous utiliseriez Windows avec un écran tactile.
- **Pointeur de souris**: transforme votre écran tactile local en un pavé tactile grand permettant de déplacer un pointeur de souris dans la session.
  - Utilisée de la même manière que vous utiliseriez Windows avec un pavé tactile.

> [!NOTE]
> Interagir avec Windows 8 ou une version ultérieure les mouvements tactiles native sont pris en charge en mode tactile Direct. 

| Mode souris    | Fonctionnement de la souris      | Mouvement                                                               |
|---------------|----------------------|-----------------------------------------------------------------------|
| Tactile direct  | Clic gauche           | appui avec 1 doigts                                                          |
| Tactile direct  | Clic droit          | 1 appui du doigt et mise en attente                                                 |
| Pointeur de souris | Clic gauche           | appui avec 1 doigts                                                          |
| Pointeur de souris | Clic gauche et glissement  | Appuyez deux fois 1 doigt et mise en attente, puis faites glisser                               |
| Pointeur de souris | Clic droit          | appui avec doigts 2                                                          |
| Pointeur de souris | Clic avec le bouton droit et glissement | 2 Appuyez deux fois le doigt et mise en attente, puis faites glisser                               |
| Pointeur de souris | Roulette de souris          | doigt 2 Appuyez et de mise en attente, puis faites glisser vers le haut ou vers le bas                           |
| Pointeur de souris | Zoom                 | Utilisez les 2 doigts et pincement pour effectuer un zoom avant ou déplacer des doigts des doigts pour effectuer un zoom arrière. |

> [!TIP]
> Questions et commentaires sont toujours accueil. Toutefois, veuillez ne publiez pas une demande de résolution des problèmes à l’aide de la fonctionnalité de commentaire à la fin de cet article. Au lieu de cela, accédez au [Forum sur le client Bureau à distance](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) et démarrer un nouveau thread. Vous avez une suggestion de fonctionnalité? Faites-nous part à l’aide du [Hub de commentaires](feedback-hub://?tabid=2&contextid=605).
