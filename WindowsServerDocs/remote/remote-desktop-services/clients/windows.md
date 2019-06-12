---
title: Bien démarrer avec le Bureau à distance sur Windows
description: Configuration de base comme suit pour le client Bureau à distance pour Windows.
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
ms.openlocfilehash: b2141a6a57629ccbb585b2f74c581eba5ba2b1b1
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446687"
---
# <a name="get-started-with-remote-desktop-on-windows"></a>Bien démarrer avec le Bureau à distance sur Windows

>S’applique à : Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Vous pouvez utiliser le client Bureau à distance pour Windows pour travailler avec les applications Windows et les ordinateurs de bureau à distance à partir d’un autre appareil Windows.

Utilisez les informations suivantes pour commencer. Veillez à consulter le [FAQ](remote-desktop-client-faq.md) si vous avez des questions.

> [!NOTE]
> - Curieux de savoir les nouvelles versions du client Windows ? Découvrez [quelles sont les nouveautés pour Bureau à distance sur Windows ?](windows-whatsnew.md)
> - Vous pouvez exécuter le client sur n’importe quelle version de Windows 10.

## <a name="get-the-rd-client-and-start-using-it"></a>Obtenir le client Bureau à distance et l’utiliser

Suivez ces étapes pour bien démarrer avec le Bureau à distance sur votre appareil Windows 10 :

1. Télécharger le client Bureau à distance à partir de [Microsoft Store](https://www.microsoft.com/store/p/microsoft-remote-desktop/9wzdncrfj3ps). 
2. [Configurer votre PC pour accepter les connexions à distance](remote-desktop-allow-access.md).
3. Ajouter une connexion Bureau à distance ou une ressource distante. Vous utilisez une connexion pour vous connecter directement à un PC Windows et à une ressource distante d’utiliser un programme RemoteApp, basés sur des sessions de bureau ou le bureau virtuel publiées par votre administrateur. 
4. Épingler des éléments afin de pouvoir obtenir rapidement au Bureau à distance.

### <a name="add-a-remote-desktop-connection"></a>Ajouter une connexion Bureau à distance

Pour créer une connexion Bureau à distance :

1. Dans, appuyez sur le centre de connexion **+ ajouter**, puis appuyez sur **Desktop**.
2. Entrez les informations suivantes pour l’ordinateur que vous souhaitez vous connecter :
   - **Nom du PC** – le nom de l’ordinateur. Cela peut être un nom d’ordinateur Windows, un nom de domaine Internet ou une adresse IP. Vous pouvez également ajouter des informations de port pour le nom du PC (par exemple, **MyDesktop:3389** ou **10.0.0.1:3389**).
   - **Compte d’utilisateur** – le compte d’utilisateur à utiliser pour accéder à l’ordinateur distant. Appuyez sur **+** pour ajouter un nouveau compte ou sélectionnez un compte existant. Vous pouvez utiliser les formats suivants pour le nom d’utilisateur : *user_name*, *domaine\nom_utilisateur*, ou <em>user_name@domain.com</em>. Vous pouvez également spécifier s’il faut demander un nom d’utilisateur et mot de passe lors de la connexion en sélectionnant **me demander chaque fois**.
3. Vous pouvez également définir des options supplémentaires en appuyant sur **afficher plus**:
   - **Nom d’affichage** – un nom facile à mémoriser pour le PC que vous vous connectez à. Vous pouvez utiliser n’importe quelle chaîne, mais si vous ne spécifiez pas un nom convivial, le nom du PC s’affiche.
   - **Groupe** – spécifier un groupe pour le rendre plus facile à repérer vos connexions. Vous pouvez ajouter un nouveau groupe en appuyant sur **+** ou sélectionnez-en un dans la liste.
   - **Passerelle** – passerelle le Bureau à distance que vous souhaitez utiliser pour se connecter à des bureaux virtuels, des programmes RemoteApp et des ordinateurs de bureau basés sur des sessions sur un réseau d’entreprise interne. Obtenir les informations sur la passerelle de votre administrateur système.
   - **Se connecter à la session d’administration** -Utilisez cette option pour vous connecter à une session de console pour administrer un serveur Windows.
   - **Permuter les boutons de la souris** – Utilisez cette option pour échanger des fonctions du bouton gauche de la souris pour le bouton droit de la souris. (Cela est particulièrement utile si l’ordinateur distant est configuré pour un utilisateur gaucher mais que vous utilisez une souris droite.)
   - **La valeur de la résolution de mon session à distance :** – sélectionnez la résolution que vous souhaitez utiliser dans la session. **Choisissez pour moi** définira la résolution en fonction de la taille du client.
   - **Modifier la taille de l’affichage :** – lorsque vous sélectionnez une haute résolution statique pour la session, vous pouvez agrandir des éléments à l’écran pour améliorer la lisibilité. Remarque: Cela s’applique uniquement lorsque vous vous connectez à Windows 8.1 ou version ultérieure.
   - **Mettre à jour de la résolution de la session à distance sur le redimensionnement** – lorsque l’option est activée, le client met à jour dynamiquement la résolution de session basée sur la taille du client. Remarque: Cela s’applique uniquement lorsque vous vous connectez à Windows 8.1 ou version ultérieure.
   - **Presse-papiers** – lorsqu’il est activé, vous permet de copier le texte et des images vers/à partir de l’ordinateur distant.
   - **Lecture audio** – sélectionnez l’appareil à utiliser pour l’audio pendant votre session à distance. Vous pouvez choisir de lire un son sur les appareils locaux, l’ordinateur distant, ou pas du tout.
   - **L’enregistrement audio** – lorsqu’il est activé, vous permet d’utiliser un microphone local avec les applications sur un ordinateur distant.
4. Appuyez sur **enregistrer**.

Vous avez besoin pour modifier ces paramètres ? Appuyez sur le menu de dépassement de capacité ( **...** ) en regard du nom du bureau et puis appuyez sur **modifier**.

Voulez-vous supprimer la connexion ? Là encore, appuyez sur le menu de dépassement de capacité ( **...** ), puis appuyez sur **supprimer**.

### <a name="add-a-remote-resource"></a>Ajouter une ressource distante
Ressources distantes sont des programmes RemoteApp, bureaux basés sur une session et des bureaux virtuels publiées par votre administrateur à l’aide des Services Bureau à distance.

Pour ajouter une ressource distante :

1. Dans l’écran de centre de connexion, appuyez sur **+ ajouter**, puis appuyez sur **ressources distantes**. 
2. Entrez le **URL du flux** fourni par votre administrateur et le drainage **rechercher des flux**.
3. Lorsque vous y êtes invité, fournissez les informations d’identification à utiliser pour vous abonner au flux.

Les ressources à distance seront affichera dans le centre de connexion.


Pour supprimer les ressources distantes :

1. Dans le centre de connexion, appuyez sur le menu de dépassement de capacité ( **...** ) en regard de la ressource distante.
2. Appuyez sur **supprimer**.

### <a name="pin-a-saved-desktop-to-your-start-menu"></a>Épingler une session Bureau enregistrée sur votre menu Démarrer

Pour épingler une connexion à votre menu Démarrer, appuyez sur le menu de dépassement de capacité ( **...** ) en regard du nom du bureau et puis appuyez sur **épingler au menu Démarrer**.

Vous pouvez maintenant démarrer la connexion Bureau à distance directement à partir de votre menu Démarrer en appuyant dessus.

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Se connecter à une passerelle Bureau à distance pour accéder aux ressources internes

Une passerelle des services Bureau à distance (passerelle RD) vous permet de vous connecter à un ordinateur distant sur un réseau d’entreprise à partir de n’importe où sur Internet. Vous pouvez créer et gérer vos passerelles à l’aide du client Bureau à distance.

Pour configurer une nouvelle passerelle :

1. Dans le centre de connexion, appuyez sur **paramètres**.
2. En regard de la passerelle, appuyez sur **+** pour ajouter une nouvelle passerelle. Remarque: Une passerelle peut également être ajoutée lors de l’ajout d’une nouvelle connexion.
3. Entrez les informations suivantes :
   - **Nom du serveur** – le nom de l’ordinateur que vous souhaitez utiliser en tant que passerelle. Cela peut être un nom d’ordinateur Windows, un nom de domaine Internet ou une adresse IP. Vous pouvez également ajouter des informations de port au nom du serveur (par exemple : **RDGateway:443** ou **10.0.0.1:443**).
   - **Compte d’utilisateur** : sélectionnez ou ajoutez un compte d’utilisateur à utiliser avec la passerelle des services Bureau à distance vous vous connectez à. Vous pouvez également sélectionner **utiliser le compte d’utilisateur du bureau** à utiliser les informations d’identification que celles utilisées pour la connexion Bureau à distance.
4. Appuyez sur **enregistrer**.  

## <a name="global-app-settings"></a>Paramètres d’application globale

Vous pouvez définir les paramètres globaux suivants dans votre client en appuyant sur **paramètres**:

ÉLÉMENTS DE GÉRÉS
- **Compte d’utilisateur** : vous permet d’ajouter, modifier et supprimer des comptes d’utilisateur enregistrés dans le client. Il s’agit d’un bon moyen de mettre à jour le mot de passe d’un compte une fois qu’il a changé.
- **Passerelle** : vous permet d’ajouter, modifier et supprimer des serveurs de passerelle enregistrées dans le client.
- **Groupe** : vous permet d’ajouter, modifier et supprimer des groupes enregistrés dans le client. Ceux-ci vous permettent de facilement les connexions de groupe.

PARAMÈTRES DE SESSION
- **Démarrer les connexions en mode plein écran** -si activé, chaque fois qu’une connexion est lancée, le client utilisera la totalité de l’écran de l’écran actif.
- **Démarrer chaque connexion dans une nouvelle fenêtre** -lorsque l’option est activée, chaque connexion est lancée dans une fenêtre distincte, ce qui vous permet de les placer sur des moniteurs différents et basculer entre eux à l’aide de la barre des tâches.
- **Lors du redimensionnement de l’application :** -permet de contrôler ce qui se passe lors du redimensionnement de la fenêtre du client. Valeur par défaut est **étirer le contenu, en conservant ses proportions**.
- **Utiliser des commandes de clavier avec :** -vous permet de spécifier où les commandes de clavier comme *WIN* ou *ALT + TAB* sont utilisés. La valeur par défaut consiste à les envoyer uniquement à la session lorsque la connexion est en plein écran.
- **Empêcher l’écran de délai d’expiration** -vous permet de conserver l’écran à partir de l’expiration du délai lorsqu’une session est active. Cela est utile lors de la connexion ne nécessite pas d’intervention pour de longues périodes.

PARAMÈTRES DE L’APPLICATION
- **Afficher les aperçus de bureau** -vous permet d’afficher un aperçu d’un ordinateur de bureau dans le centre de connexion avant de vous connecter à celui-ci. Par défaut, elle est définie **sur**.
- **Aidez-nous à améliorer le Bureau à distance** -envoie des données anonymes à Microsoft. Nous utilisons ces données pour améliorer le client. Pour en savoir plus sur la façon dont nous utilisons ces données anonymes, privées, consultez le [déclaration de confidentialité Microsoft](https://privacy.microsoft.com/en-us/privacystatement). Par défaut, ce paramètre est **sur**.

### <a name="manage-your-user-accounts"></a>Gérer vos comptes d’utilisateur

Lorsque vous vous connectez à un bureau ou à distance des ressources, vous pouvez enregistrer les comptes d’utilisateur à sélectionner à nouveau. Vous pouvez également définir des comptes d’utilisateur dans le client lui-même, par opposition à l’enregistrement des données utilisateur lorsque vous vous connectez à un ordinateur de bureau.

Pour créer un nouveau compte d’utilisateur :

1. Dans le centre de connexion, appuyez sur **paramètres**.
2. En regard du compte d’utilisateur, appuyez sur **+** pour ajouter un nouveau compte d’utilisateur.
3. Entrez les informations suivantes :
   - **Nom d’utilisateur** -le nom de l’utilisateur à enregistrer pour une utilisation avec une connexion à distance. Vous pouvez entrer le nom d’utilisateur dans un des formats suivants : user_name, DOMAINE\nom_utilisateur, ou user_name@domain.com.
   - **Mot de passe** -le mot de passe pour l’utilisateur spécifié. Cela peut être vide pour être invité à entrer un mot de passe lors de la connexion.
4. Appuyez sur **enregistrer**.

Pour supprimer un compte d’utilisateur :

1. Dans le centre de connexion, appuyez sur **paramètres**.
2. Sélectionnez le compte à supprimer de la liste sous le compte d’utilisateur.
3. En regard du compte d’utilisateur, appuyez sur l’icône de modification.
4. Appuyez sur **supprimer ce compte** en bas pour supprimer le compte d’utilisateur.
5. Vous pouvez également modifier le compte d’utilisateur et appuyez sur **enregistrer**.

## <a name="navigate-the-remote-desktop-session"></a>Accédez à la session Bureau à distance
Lorsque vous démarrez une connexion Bureau à distance, il existe des outils que vous pouvez utiliser pour parcourir la session.

### <a name="start-a-remote-desktop-connection"></a>Démarrer une connexion Bureau à distance

1. Appuyez sur la connexion Bureau à distance pour démarrer la session.
2. Si vous n’avez pas enregistré les informations d’identification pour la connexion, vous devrez fournir un **nom d’utilisateur** et **mot de passe**.
3. Si vous êtes invité à vérifier le certificat pour le Bureau à distance, passez en revue les informations et assurez-vous que l’est un PC que vous faites confiance à avant d’appuyer sur **Connect**. Vous pouvez également sélectionner **ne plus me demander sur ce certificat** toujours accepter ce certificat.

### <a name="connection-bar"></a>Barre de connexion

La connexion barre vous donne qu'accès aux contrôles de navigation supplémentaires. Par défaut, la barre de connexion est placée dans le milieu en haut de l’écran. Appuyez sur, puis faites glisser la barre à gauche ou droite pour le déplacer.

- **Contrôle panoramique** -le contrôle panoramique permet à l’écran être agrandi et déplacé. Notez que contrôle Panorama est uniquement disponible sur les appareils tactiles et de l’utilisation du mode d’interaction tactile directe.
   - Activer / désactiver le contrôle Panorama : Appuyez sur l’icône de panoramique dans la barre de connexion pour afficher le contrôle Panoramique et Zoom sur l’écran. Appuyez sur l’icône de panoramique dans la barre de connexion à nouveau à masquer le contrôle et de retourner l’écran à sa résolution d’origine.
   - Utiliser le contrôle panoramique - Tap, maintenez le contrôle Panoramique et faites-le glisser dans la direction que vous souhaitez déplacer l’écran.
   - Déplacer le contrôle panoramique - appuyer deux fois et maintenez le contrôle panoramique pour déplacer le contrôle sur l’écran.
- **Options supplémentaires** -appuyez sur l’icône des options supplémentaires pour afficher la sélection de session de la barre et commande de la barre (voir ci-dessous).
- **Clavier** -appuyez sur l’icône de clavier pour afficher ou masquer le clavier visuel. Le contrôle Panorama s’affiche automatiquement lorsque le clavier est affiché.

### <a name="command-bar"></a>Barre de commandes

Appuyez sur la **...**  sur la barre de connexion pour afficher la barre de commandes sur le côté droit de l’écran.

- **Accueil** -utiliser le bouton d’accueil pour revenir au centre de connexion à partir de la barre de commandes.
  - Vous pouvez également utiliser le bouton Précédent pour la même action.
  - Votre session active n’est pas déconnectée.
  - Cela vous permet de lancer des connexions supplémentaires.
- **Déconnexion** -utilisez le bouton de déconnexion pour mettre fin à la connexion.
  - Vos applications reste actives tant que la session n’est pas terminée sur un ordinateur distant.
- **Plein écran** - entre ou sort du mode plein écran.
- **Tactile / de la souris** -vous pouvez basculer entre les modes de la souris (interaction tactile directe et pointeur de la souris).

### <a name="use-direct-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Utilisation directe touch mouvements et les modes de la souris dans une session à distance

Deux modes de la souris sont disponibles pour interagir avec la session.
- **Diriger touch**: Transmet tous les contacts tactiles à la session doit être interprété à distance.
  - Utilisé dans la même façon que vous utilisez Windows avec un écran tactile.
- **Pointeur de la souris**: Transforme votre écran tactile local en un pavé tactile grand permettant de déplacer un pointeur de la souris dans la session.
  - Utilisé dans la même façon que vous utiliseriez numérique avec un pavé tactile.

> [!NOTE]
> Interaction avec Windows 8 ou version ultérieure les mouvements tactiles natif sont pris en charge en mode d’interaction tactile directe. 

| Mode souris    | Opération de la souris      | Mouvement                                                               |
|---------------|----------------------|-----------------------------------------------------------------------|
| Interaction tactile directe  | Clic gauche           | Appuyez sur 1 doigt                                                          |
| Interaction tactile directe  | Clic droit          | 1 doigt maintenez                                                 |
| Pointeur de la souris | Clic gauche           | Appuyez sur 1 doigt                                                          |
| Pointeur de la souris | Clic gauche et glissement  | appuyer deux fois 1 doigt et maintenir, puis faites glisser                               |
| Pointeur de la souris | Clic droit          | Appuyez sur 2 doigt                                                          |
| Pointeur de la souris | Clic avec le bouton droit et glissement | 2 double pression du doigt et maintenir, puis faites glisser                               |
| Pointeur de la souris | Roulette de la souris          | 2 doigt appuyez sur et maintenez la touche, puis faites glisser vers le haut ou vers le bas                           |
| Pointeur de la souris | Zoom                 | Utilisez les 2 doigts et pincer pour faire un zoom ou déplacer les unes des autres les doigts pour effectuer un zoom arrière. |

> [!TIP]
> Questions et vos commentaires sont toujours les bienvenus. Toutefois, ne posez pas une demande de résolution des problèmes à l’aide de la fonctionnalité de commentaire à la fin de cet article. Au lieu de cela, accédez à la [forum du client Bureau à distance](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) et démarrer un nouveau thread. Vous avez une suggestion de fonctionnalité ? Dites-nous à l’aide de la [Hub de commentaires](feedback-hub://?tabid=2&contextid=605).
