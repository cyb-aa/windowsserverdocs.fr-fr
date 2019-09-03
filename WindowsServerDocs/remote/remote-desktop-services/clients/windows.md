---
title: Bien démarrer avec le client Windows Store
description: Étapes de configuration de base pour le client Bureau à distance pour Windows Store.
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
ms.date: 08/27/2019
ms.localizationpriority: medium
ms.openlocfilehash: 03927cd531617c6e0c9572fc4ce74768e10bc66a
ms.sourcegitcommit: 51eaab0f860312d97293fd90f3e632e7caee3df1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70150928"
---
# <a name="get-started-with-the-windows-store-client"></a>Bien démarrer avec le client Windows Store

>S’applique à : Windows 10

Le client Bureau à distance sur Windows vous permet d’utiliser des bureaux et applications Windows à distance depuis un autre appareil Windows.

Aidez-vous des informations suivantes pour démarrer. Consultez le [Forum aux questions (FAQ)](remote-desktop-client-faq.md) si vous avez des questions.

> [!NOTE]
> - Vous êtes curieux de découvrir les nouvelles versions du client Windows ? Consultez [Nouveautés du Bureau à distance sur Windows](windows-whatsnew.md).
> - Vous pouvez exécuter le client sur n’importe quelle version de Windows 10.

## <a name="get-the-rd-client-and-start-using-it"></a>Obtenir le client Bureau à distance et commencer à l’utiliser

Effectuez ces étapes pour bien démarrer avec le Bureau à distance sur votre appareil Windows 10 :

1. Téléchargez le client Bureau à distance à partir du [Microsoft Store](https://www.microsoft.com/store/p/microsoft-remote-desktop/9wzdncrfj3ps). 
2. [Configurez votre PC pour accepter les connexions à distance](remote-desktop-allow-access.md).
3. Ajoutez une connexion Bureau à distance ou une ressource distante. Utilisez une connexion pour vous connecter directement à un PC Windows, et une ressource distante pour accéder à un programme RemoteApp, un bureau basé sur une session ou un bureau virtuel publié par votre administrateur. 
4. Épinglez les éléments pour accéder rapidement au Bureau à distance.

### <a name="add-a-remote-desktop-connection"></a>Ajouter une connexion Bureau à distance

Pour créer une connexion Bureau à distance :

1. Dans le Centre de connexion, appuyez sur **+ Ajouter**, puis sur **Bureau**.
2. Entrez les informations suivantes pour l’ordinateur auquel vous souhaitez vous connecter :
   - **Nom du PC** : nom de l’ordinateur. Cela peut être un nom d’ordinateur Windows, un nom de domaine Internet ou une adresse IP. Vous pouvez aussi ajouter les informations du port au nom du PC (par exemple, **MyDesktop:3389** ou **10.0.0.1:3389**).
   - **Compte d’utilisateur** : compte d’utilisateur à utiliser pour accéder au PC distant. Appuyez sur **+** pour ajouter un nouveau compte ou sélectionnez un compte existant. Le nom d’utilisateur doit respecter l’un des formats suivants : *nom_utilisateur*, *domaine\nom_utilisateur* ou <em>user_name@domain.com</em>. Sélectionnez **Toujours me demander** si vous souhaitez que l’utilisateur soit invité à entrer un nom d’utilisateur et un mot de passe quand il se connecte.
3. Vous pouvez également définir des options supplémentaires en appuyant sur **Afficher plus** :
   - **Nom d’affichage**  : nom facile à mémoriser pour le PC auquel vous vous connectez. Vous pouvez choisir n’importe quelle chaîne, mais si vous ne spécifiez pas de nom convivial, le nom du PC est affiché.
   - **Groupe** : spécifiez un groupe pour retrouver plus facilement vos connexions plus tard. Ajoutez un nouveau groupe en appuyant sur **+** ou sélectionnez-en un dans la liste.
   - **Passerelle** : passerelle Bureau à distance par laquelle vous voulez vous connecter aux bureaux virtuels, programmes RemoteApp et bureaux basés sur une session dans un réseau interne d’entreprise. Demandez les informations sur la passerelle à votre administrateur système.
   - **Se connecter à la session admin** : avec cette option, vous pouvez vous connecter à une session de console en vue d’administrer un serveur Windows.
   - **Permuter les boutons de la souris** : avec cette option, vous pouvez permuter les fonctions du bouton gauche de la souris sur le bouton droit de la souris. (Cela est particulièrement utile si le PC distant est configuré pour un utilisateur gaucher mais que vous utilisez une souris pour droitier.)
   - **Définir la résolution de ma session à distance sur** : sélectionnez la résolution souhaitée pour la session. **Choisir pour moi** : définit la résolution en fonction de la taille du client.
   - **Modifier la taille de l’affichage** : si vous sélectionnez une haute résolution statique pour la session, vous pouvez agrandir les éléments à l’écran pour améliorer la lisibilité. Remarque: Cela s’applique uniquement pour les connexions à Windows 8.1 ou version ultérieure.
   - **Mettre à jour la résolution de la session à distance après redimensionnement** : quand cette option est activée, le client met à jour dynamiquement la résolution de la session en fonction de la taille du client. Remarque: Cela s’applique uniquement pour les connexions à Windows 8.1 ou version ultérieure.
   - **Presse-papiers** : quand elle est activée, cette option vous permet de copier du texte et des images vers ou à partir du PC distant.
   - **Lecture audio** : sélectionnez l’appareil à utiliser pour l’audio pendant votre session à distance. Vous pouvez choisir d’activer le son sur les appareils locaux ou le PC distant, ou de désactiver entièrement le son.
   - **Enregistrement audio** : quand elle est activée, cette option vous permet d’utiliser un microphone local avec les applications sur un PC distant.
4. Appuyez sur **Enregistrer**.

Vous devez modifier ces paramètres ? Appuyez sur le menu de dépassement ( **...** ) à côté du nom du bureau et appuyez ensuite sur **Modifier**.

Vous souhaitez supprimer la connexion ? Là encore, appuyez sur le menu de dépassement ( **...** ), puis appuyez sur **Supprimer**.

### <a name="add-a-remote-resource"></a>Ajouter une ressource distante

Les ressources distantes peuvent être des programmes RemoteApp, des bureaux basés sur une session et des bureaux virtuels publiés par votre administrateur à l’aide des services Bureau à distance.

Pour ajouter une ressource distante :

1. Dans l’écran du Centre de connexion, appuyez sur **+ Ajouter**, puis appuyez sur **Ressources distantes**.
2. Entrez l’**URL de flux** fournie par votre administrateur et appuyez sur **Rechercher les flux**.
3. Lorsque vous y êtes invité, entrez les informations d’identification nécessaires pour vous abonner au flux.

Les ressources distantes ajoutées seront affichées dans le Centre de connexion.

Pour supprimer des ressources distantes :

1. Dans le Centre de connexion, appuyez sur le menu de dépassement ( **...** ) à côté de la ressource distante.
2. Appuyez sur **Supprimer**.

### <a name="pin-a-saved-desktop-to-your-start-menu"></a>Épingler un bureau enregistré à votre menu Démarrer

Pour épingler une connexion à votre menu Démarrer, appuyez sur le menu de dépassement ( **...** ) à côté du nom du bureau et appuyez ensuite sur **Épingler au menu Démarrer**.

Vous pouvez maintenant démarrer la connexion au bureau à distance directement à partir de votre menu Démarrer en appuyant dessus.

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Se connecter à une passerelle Bureau à distance pour accéder aux ressources internes

Une passerelle Bureau à distance vous permet de vous connecter à un ordinateur distant sur un réseau d’entreprise à partir de n’importe où sur Internet. Vous pouvez créer et gérer les passerelles à l’aide du client Bureau à distance.

Pour configurer une nouvelle passerelle :

1. Dans le Centre de connexion, appuyez sur **Paramètres**.
2. À côté de Passerelle, appuyez sur **+** pour ajouter une nouvelle passerelle. Remarque: Une passerelle peut également être ajoutée quand vous ajoutez une nouvelle connexion.
3. Entrez les informations suivantes :
   - **Nom du serveur** : nom de l’ordinateur que vous souhaitez utiliser comme passerelle. Cela peut être un nom d’ordinateur Windows, un nom de domaine Internet ou une adresse IP. Vous pouvez aussi ajouter les informations de port au nom du serveur (par exemple : **RDGateway:443** ou **10.0.0.1:443**).
   - **Compte d’utilisateur** : sélectionnez ou ajoutez un compte d’utilisateur à utiliser avec la passerelle des services Bureau à distance à laquelle vous vous connectez. Vous pouvez également sélectionner **Utiliser le compte d’utilisateur du bureau** si vous préférez garder les mêmes informations d’identification que celles utilisées pour la connexion Bureau à distance.
4. Appuyez sur **Enregistrer**.  

## <a name="global-app-settings"></a>Paramètres d’application généraux

Vous pouvez définir les paramètres généraux suivants sur votre client en appuyant sur **Paramètres** :

ÉLÉMENTS GÉRÉS

- **Compte d’utilisateur** : vous permet d’ajouter, de modifier et de supprimer des comptes d’utilisateur enregistrés sur le client. Il s’agit d’un bon moyen de mettre à jour le mot de passe d’un compte qui a été changé.
- **Passerelle** : vous permet d’ajouter, de modifier et de supprimer des serveurs de passerelle enregistrés sur le client.
- **Groupe** : vous permet d’ajouter, de modifier et de supprimer des groupes enregistrés sur le client. Avec les groupes, vous pouvez facilement regrouper des connexions.

PARAMÈTRES DE SESSION

- **Démarrer les connexions en mode plein écran** : quand cette option est activée, à chaque démarrage d’une connexion, le client utilise l’écran actif dans sa totalité.
- **Démarrer chaque connexion dans une nouvelle fenêtre** : quand cette option est activée, chaque connexion est démarrée dans une fenêtre distincte. Vous pouvez ainsi avoir les connexions dans des écrans différents et passer de l’une à l’autre au moyen de la barre des tâches.
- **Lors du redimensionnement de l’application** : vous permet de contrôler ce qui se passe quand la fenêtre du client est redimensionnée. L’action par défaut est **Étirer le contenu, en conservant ses proportions**.
- **Utiliser les commandes clavier avec** : vous permet de spécifier où les commandes clavier comme *WIN* ou *ALT+TAB* sont utilisées. Par défaut, ces commandes sont envoyées à la session seulement quand la connexion est en mode plein écran.
- **Empêcher l’expiration de l’écran** : vous permet d’empêcher que l’écran expire quand une session est active. Cela est utile pour les connexions qui ne nécessitent pas d’interaction pendant de longues périodes.

PARAMÈTRES D’APPLICATION

- **Afficher les aperçus de bureau** : vous permet d’afficher l’aperçu d’un bureau dans le Centre de connexion avant de vous y connecter. Par défaut, ce paramètre est **activé**.
- **Aidez-nous à améliorer le Bureau à distance** : envoie des données anonymes à Microsoft. Nous nous servons de ces données pour améliorer le client. Pour en savoir plus sur la façon dont nous utilisons ces données personnelles anonymes, consultez la [Déclaration de confidentialité Microsoft](https://privacy.microsoft.com/en-us/privacystatement). Par défaut, ce paramètre est **activé**.

### <a name="manage-your-user-accounts"></a>Gérer vos comptes d’utilisateur

Quand vous vous connectez à un bureau ou à des ressources distantes, vous pouvez enregistrer les comptes d’utilisateur pour les resélectionner ultérieurement. Vous pouvez également définir des comptes d’utilisateur directement sur le client, au lieu d’enregistrer les données utilisateur quand vous vous connectez à un bureau.

Pour créer un compte d’utilisateur :

1. Dans le Centre de connexion, appuyez sur **Paramètres**.
2. À côté du compte d’utilisateur, appuyez sur **+** pour ajouter un nouveau compte d’utilisateur.
3. Entrez les informations suivantes :
   - **Nom d’utilisateur** : nom d’utilisateur à enregistrer pour l’utiliser avec une connexion à distance. Entrez le nom d’utilisateur dans un de ces formats : nom_utilisateur, domaine\nom_utilisateur ou user_name@domain.com.
   - **Mot de passe** : mot de passe associé à l’utilisateur spécifié. Laissez ce champ vide si vous souhaitez que l’utilisateur soit invité à entrer un mot de passe quand il se connecte.
4. Appuyez sur **Enregistrer**.

Pour supprimer un compte d’utilisateur :

1. Dans le Centre de connexion, appuyez sur **Paramètres**.
2. Sélectionnez le compte à supprimer dans la liste sous Compte d’utilisateur.
3. À côté de Compte d’utilisateur, appuyez sur l’icône de modification.
4. Appuyez sur **Supprimer ce compte** en bas pour supprimer le compte d’utilisateur.
5. Vous pouvez également modifier le compte d’utilisateur et appuyer sur **Enregistrer**.

## <a name="navigate-the-remote-desktop-session"></a>Naviguer dans la session Bureau à distance
Quand vous démarrez une connexion Bureau à distance, vous disposez d’outils utiles pour naviguer dans la session.

### <a name="start-a-remote-desktop-connection"></a>Démarrer une connexion Bureau à distance

1. Appuyez sur la connexion Bureau à distance pour démarrer la session.
2. Si vous n’avez pas enregistré les informations d’identification pour la connexion, vous êtes invité à entrer un **nom d’utilisateur** et un **mot de passe**.
3. Si vous êtes invité à vérifier le certificat du bureau à distance, passez en revue les informations et assurez-vous que l’ordinateur est un PC de confiance avant d’appuyer sur **Connexion**. Vous pouvez aussi sélectionner **Ne plus me demander d’informations sur ce certificat** afin d’accepter automatiquement ce certificat.

### <a name="connection-bar"></a>Barre de connexion

La barre de connexion vous donne accès à des contrôles de navigation supplémentaires. Par défaut, la barre de connexion est placée en haut de l’écran, au milieu. Appuyez sur la barre et faites-la glisser vers la gauche ou la droite pour la déplacer.

- **Contrôle panoramique** : vous permet d’agrandir et déplacer l’écran. Notez que le contrôle panoramique est uniquement disponible sur les appareils tactiles et en mode d’interaction tactile directe.
   - Activer/désactiver le contrôle panoramique : appuyez sur l’icône panoramique dans la barre de connexion pour afficher le contrôle panoramique et zoomer dans l’écran. Appuyez de nouveau sur l’icône panoramique dans la barre de connexion pour masquer le contrôle et réafficher l’écran dans sa résolution d’origine.
   - Utiliser le contrôle panoramique : appuyez longuement sur le contrôle panoramique et faites-le glisser dans la direction où vous souhaitez déplacer l’écran.
   - Déplacer le contrôle panoramique : appuyez longuement deux fois sur le contrôle panoramique pour le déplacer sur l’écran.
- **Options supplémentaires** : appuyez sur l’icône des options supplémentaires pour afficher la barre de sélection de session et la barre de commandes (voir ci-dessous).
- **Clavier** : appuyez sur l’icône du clavier pour afficher ou masquer le clavier visuel. Le contrôle panoramique s’affiche automatiquement quand le clavier est affiché.

### <a name="command-bar"></a>Barre de commandes

Appuyez sur **...** dans la barre de connexion pour afficher la barre de commandes sur le côté droit de l’écran.

- **Accueil** : utilisez le bouton Accueil pour revenir au Centre de connexion à partir de la barre de commandes.
  - Vous pouvez aussi utiliser le bouton Précédent pour la même action.
  - Votre session active ne sera pas déconnectée.
  - Cela vous permet de démarrer des connexions supplémentaires.
- **Déconnexion** : utilisez ce bouton pour terminer la connexion.
  - Vos applications restent actives tant que la session n’est pas terminée sur le PC distant.
- **Plein écran** : active ou désactive le mode plein écran.
- **Interaction tactile/Souris** : vous pouvez passer d’un mode souris à un autre (interaction tactile directe et pointeur de souris).

### <a name="use-direct-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Utiliser les mouvements d’interaction tactile directe et les modes souris dans une session à distance

Deux modes souris sont disponibles pour interagir avec la session.

- **Interaction tactile directe** : passe tous les contacts d’interaction tactile à la session afin qu’ils soient interprétés à distance.
  - Ce mode s’utilise de la même façon qu’avec un écran tactile sur un appareil Windows.
- **Pointeur de souris** : transforme votre écran tactile local en un grand pavé tactile permettant de déplacer un pointeur de souris dans la session.
  - Ce mode s’utilise de la même façon qu’avec un pavé tactile sur un appareil Windows.

> [!NOTE]
> Sur Windows 8 ou une version ultérieure, les mouvements d’interaction tactile natifs sont pris en charge en mode d’interaction tactile directe.

| Mode souris    | Action avec la souris      | Mouvement                                                               |
|---------------|----------------------|-----------------------------------------------------------------------|
| Interaction tactile directe  | Clic gauche           | Appuyez avec un doigt                                                          |
| Interaction tactile directe  | Clic droit          | Appuyez longuement avec un doigt                                                 |
| Pointeur de souris | Clic gauche           | Appuyez avec un doigt                                                          |
| Pointeur de souris | Clic gauche et glissement  | Appuyez longuement deux fois avec un doigt, puis faites glisser                               |
| Pointeur de souris | Clic droit          | Appuyez avec deux doigts                                                          |
| Pointeur de souris | Clic droit et glissement | Appuyez longuement deux fois avec deux doigts, puis faites glisser                               |
| Pointeur de souris | Roulette de la souris          | Appuyez longuement avec deux doigts, puis faites glisser vers le haut ou le bas                           |
| Pointeur de souris | Zoom                 | Resserrez les deux doigts pour faire un zoom ou écartez les doigts pour effectuer un zoom arrière. |

> [!TIP]
> Vos questions et vos commentaires sont toujours les bienvenus. Toutefois, merci de ne pas utiliser la fonctionnalité de commentaire qui figure à la fin de cet article pour nous envoyer une demande d’aide. Veuillez plutôt accéder au [forum du client Bureau à distance](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) et démarrez un nouveau fil de discussion. Vous avez une suggestion de fonctionnalité à nous faire ? Dites-nous laquelle sur le [Hub de commentaires](feedback-hub://?tabid=2&contextid=605).
