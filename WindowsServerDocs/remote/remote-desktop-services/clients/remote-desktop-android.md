---
title: Vous familiariser avec les services Bureau à distance sur Android
description: Basic paramétrer des étapes pour le client Bureau à distance pour Android.
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
ms.date: 07/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: 42b4b4ffb73bd9d5d1397d32bd36c41d7e404dd7
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297428"
---
# Vous familiariser avec les services Bureau à distance sur Android

>S’applique à: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Vous pouvez utiliser le client Bureau à distance pour Android pour travailler avec des ordinateurs de bureau et les applications Windows directement à partir de votre appareil Android.

Utilisez les informations suivantes pour commencer. Veillez à examiner le [Forum aux questions sur](remote-desktop-client-faq.md) si vous avez des questions.

> [!NOTE]
> - Curieux de savoir sur les nouvelles versions du client Android? Consultez l’article [Nouveautés pour les services Bureau à distance sur Android?](android-whatsnew.md)
> Vous pouvez exécuter le client sur Android 4.1 et des appareils plus récente, ainsi que sur Chromebooks avec 53 ChromeOS installé. En savoir plus sur les applications Android sur Chrome [ici](https://sites.google.com/a/chromium.org/dev/chromium-os/chrome-os-systems-supporting-android-apps).

## Obtenir le client Bureau à distance et démarrer l’utiliser

Suivez ces étapes pour vous familiariser avec les services Bureau à distance sur votre appareil Android:

1. Télécharger le client Bureau à distance à partir de [Google Play](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android). 
2. [Configuration de votre PC pour accepter les connexions à distance](remote-desktop-allow-access.md).
3. Ajouter une connexion Bureau à distance ou une ressource distante. Vous utilisez une connexion pour se connecter directement à un PC Windows et une ressource distante d’utiliser un programme RemoteApp, basée sur la session de bureau, ou bureau virtuel publiés sur site. 
4. Créer un widget que vous puissiez vous faire rapidement au Bureau à distance.

> [!NOTE]
> Si vous souhaitez que de nouvelles fonctionnalités de version d’évaluation précédemment, nous vous recommandons de téléchargement de notre application [Bêta de bureau à distance Microsoft](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android.beta) à partir du magasin de Google Play. 

### Ajouter une connexion Bureau à distance

Pour créer une connexion Bureau à distance:

1. Dans l’appui du centre de connexion **+**, puis appuyez sur le **Bureau**.
2. Entrez les informations suivantes pour l’ordinateur que vous souhaitez vous connecter:
  - **Nom du PC** – le nom de l’ordinateur. Cela peut être un nom d’ordinateur Windows, un nom de domaine Internet ou une adresse IP. Vous pouvez également ajouter des informations de port pour le nom du PC (par exemple, **MyDesktop:3389** ou **10.0.0.1:3389**).
  - **Nom d’utilisateur** : le nom d’utilisateur à utiliser pour accéder à l’ordinateur distant. Vous pouvez utiliser les formats suivants: *nom_utilisateur*, *domaine\nom_utilisateur*ou *user_name@domain.com*. Vous pouvez également spécifier si vous souhaitez demander un nom d’utilisateur et mot de passe.
3. Vous pouvez également définir les options supplémentaires suivantes:
  - **Nom convivial** : un nom facile à retenir pour le PC que vous vous connectez à. Vous pouvez utiliser n’importe quelle chaîne, mais si vous ne spécifiez pas un nom convivial, le nom du PC s’affiche.
  - **Passerelle** – passerelle le Bureau à distance que vous souhaitez utiliser pour se connecter à des bureaux virtuels, des programmes RemoteApp et basée sur la session de postes de travail sur un réseau d’entreprise interne. Obtenir les informations sur la passerelle de votre administrateur système.
    Vous avez besoin pour configurer une passerelle des services Bureau à distance?
  - **Son** : sélectionner l’appareil à utiliser pour l’audio au cours de votre session à distance. Vous pouvez choisir de lire du contenu audio sur les appareils locales, l’appareil distant, ou pas du tout.
  - **Résolution d’affichage personnaliser** - définissez une résolution personnalisée pour une connexion par l’activation de ce paramètre. Lors de désactiver la résolution de l’application que vous avez définis dans les paramètres globaux de l’application.
  - **Les boutons de la souris échange** : utilisez cette option pour inverser des fonctions de bouton de la souris pour le bouton droit de la souris. (Cela est particulièrement utile si le PC distant est configuré pour un utilisateur gaucher, mais que vous utilisez une souris droitier.)
  - **Se connecter à la session de l’administrateur** - Utilisez cette option pour se connecter à une session de console pour administrer un serveur Windows.
  - **Rediriger vers le stockage local** – monte votre stockage local en tant que système de fichiers à distance sur l’ordinateur distant.
4. Appuyez sur **Enregistrer**.

Vous avez besoin de modifier ces paramètres? Cliquez sur le menu de dépassement (**...**) en regard du nom du bureau, puis **Modifier**.

Vous voulez supprimer la connexion? Là encore, cliquez sur le menu de dépassement de capacité (**...**) et appuyez sur **Supprimer**.

>[!TIP]
> Si vous obtenez l’erreur 0xf07 un mot de passe incorrect («nous avons pas pu se connecter au PC distant dans la mesure où le mot de passe associé au compte d’utilisateur a expiré»), modifier votre mot de passe et essayez à nouveau.

### Ajouter une ressource à distance
Ressources distantes sont des programmes RemoteApp, postes de travail basée sur la session et les bureaux virtuels publiées à l’aide de connexions RemoteApp et bureau.

Pour ajouter une ressource à distance:

1. Sur l’écran de connexion centre, appuyez sur **+**, puis appuyez sur **Le flux de ressources à distance**. 
2. Entrez les informations de la ressource à distance:
   - **E-mail ou l’URL** - l’URL du serveur d’accès Web de bureau à distance. Vous pouvez également entrer votre compte de messagerie d’entreprise dans ce champ, cela indique au client pour rechercher le serveur d’accès Web Bureau à distance associées à votre adresse de messagerie.
   - **Nom d’utilisateur** : le nom d’utilisateur à utiliser pour le serveur d’accès Web de bureau à distance à que vous vous connectez.
   - **Mot de passe** - le mot de passe à utiliser pour le serveur d’accès Web de bureau à distance à que vous vous connectez.
3. Appuyez sur **Enregistrer**.

Les ressources distantes seront affichera dans le centre de la connexion.


Pour supprimer les ressources distantes:

1. Dans le centre de la connexion, cliquez sur le menu de dépassement de capacité (**...**) en regard de la ressource à distance.
2. Cliquez sur **Supprimer**.
3. Vérifiez que la suppression.

### Widgets – épingler un ordinateur de bureau enregistrée sur votre écran d’accueil

Les applications de bureau à distance prend en charge les connexions épingles à votre écran d’accueil à l’aide de la fonctionnalité widget Android. La manière que vous ajoutez un widget varie selon le type d’appareil Android, que vous utilisez et son système d’exploitation. Voici la méthode la plus courante pour ajouter un widget: 

1. Appuyez sur **les applications** pour lancer le menu d’applications.
2. Appuyez **widgets**.
3. Effectuez un balayage via les widgets et recherchez l’icône de bureau à distance avec la description, «Bureau à distance de code confidentiel.»
4. Appuyez sur ce widget Bureau à distance et déplacez-le vers l’écran d’accueil.
5. Lorsque vous lancez l’icône, vous verrez les postes de travail à distance enregistrés. Choisissez la connexion que vous souhaitez enregistrer sur votre écran d’accueil.

Maintenant, vous pouvez démarrer la connexion Bureau à distance directement à partir de votre écran d’accueil en appuyant dessus.

> [!NOTE]
> Si vous renommez la connexion de bureau dans l’application de bureau à distance, le libellé de ce bureau à distance épinglé ne mettra pas à jour.

## Se connecter à une passerelle des services Bureau à distance pour accéder à des ressources internes

Une passerelle des services Bureau à distance (passerelle des services Bureau à distance) vous permet de vous connecter à un ordinateur distant sur un réseau d’entreprise à partir de n’importe où sur Internet. Vous pouvez créer et gérer vos passerelles à l’aide du client Bureau à distance.

Pour configurer une nouvelle passerelle:

1. Dans le centre de la connexion, appuyez sur **paramètres > passerelles**. Appuyez sur **+** pour ajouter une nouvelle passerelle.
2. Entrez les informations suivantes:
  - **Nom du serveur** : le nom de l’ordinateur que vous souhaitez utiliser sous forme de passerelle. Cela peut être un nom d’ordinateur Windows, un nom de domaine Internet ou une adresse IP. Vous pouvez également ajouter des informations de port pour le nom du serveur (par exemple: **RDGateway:443** ou **10.0.0.1:443**).
  - **Nom d’utilisateur** : le nom d’utilisateur et mot de passe à utiliser pour la passerelle des services Bureau à distance vous vous connectez à. Vous pouvez également sélectionner **utiliser le compte d’utilisateur de bureau** pour utiliser les mêmes informations d’identification que ceux utilisés pour la connexion Bureau à distance.

## Gérer vos comptes d’utilisateurs

Lorsque vous vous connectez à un ordinateur de bureau ou à distance des ressources, vous pouvez enregistrer les comptes d’utilisateur pour sélectionner à partir d’une nouvelle fois. Vous pouvez également définir des comptes d’utilisateur dans le client lui-même, par opposition à l’enregistrement des données utilisateur lorsque vous vous connectez à un ordinateur de bureau.

Pour créer un compte d’utilisateur:

1. Dans le centre de la connexion, appuyez sur les **paramètres**et appuyez sur **les comptes d’utilisateurs**.
2. Appuyez sur **+** pour ajouter un compte d’utilisateur.
3. Entrez les informations suivantes:
   - **Nom d’utilisateur** : le nom de l’utilisateur à enregistrer pour une utilisation avec une connexion à distance. Vous pouvez entrer le nom d’utilisateur dans un des formats suivants: nom_utilisateur, DOMAINE\nom_utilisateur, ou user_name@domain.com.
   - **Mot de passe** - le mot de passe pour l’utilisateur que vous avez spécifié. Chaque compte d’utilisateur que vous souhaitez enregistrer de manière à utiliser pour les connexions à distance doit avoir un mot de passe associé.
4. Appuyez sur **Enregistrer**.

Pour supprimer un compte d’utilisateur:

1. Dans le centre de la connexion, appuyez sur **les comptes d’utilisateurs > paramètres**.
2. Appuyez longuement sur un compte d’utilisateur dans la liste pour le sélectionner. Vous pouvez sélectionner plusieurs utilisateurs.
3. Appuyez sur la Corbeille pour supprimer l’utilisateur sélectionné.

## Accédez à la session Bureau à distance
Lorsque vous démarrez une connexion Bureau à distance, il existe des outils que vous pouvez utiliser pour naviguer dans la session.

### Démarrer une connexion Bureau à distance

1. Appuyez sur la connexion Bureau à distance pour démarrer la session. 
2. Si vous êtes invité à vérifier le certificat pour le Bureau à distance, cliquez sur **se connecter**. Vous pouvez également sélectionner **ne pas me demander à nouveau pour les connexions à cet ordinateur** à toujours accepter le certificat.

### Gérer les paramètres d’application général

Vous pouvez définir les paramètres globaux suivants dans votre client Android:

- **Afficher les aperçus de bureau** - vous permet d’afficher un aperçu d’un ordinateur de bureau dans le centre de la connexion avant de vous connecter à celui-ci. Par défaut, cela est définie sur **activé**.
- **Pincement pour effectuer un Zoom** - vous permet d’utiliser des mouvements de pincement zoom. Si l’application que vous utilisez par le biais des services Bureau à distance prend en charge l’interaction tactile multipoint (introduit dans Windows 8), activer ce paramètre **désactivé**.
- **Vous aider à améliorer les services Bureau à distance** - envoie des données anonymes à Microsoft. Nous utilisons ces données pour améliorer le client. Vous pouvez en savoir plus sur la manière dont nous traiter ces données anonymes, privées, reportez-vous à la [Déclaration de confidentialité de Client Bureau à distance](https://www.microsoft.com/privacystatement/RemoteApp/Default.aspx). Par défaut, ce paramètre est **activé**.
- **Afficher** - il existe deux paramètres globaux de votre affichage:
   - **Orientation** - définit l’orientation par défaut (paysage ou portrait) pour votre session. 
   >[!NOTE]
   > Si vous vous connectez à un PC exécutant Windows 8 ou une version antérieure de Windows, la session ne puisse pas évoluer correctement. La meilleure solution consiste à se déconnecter de l’ordinateur, puis à reconnecter dans l’orientation que vous souhaitez utiliser. Une meilleure option consiste à mettre à niveau le PC au moins Windows 8.1.

   - **Résolution** - définit la résolution que vous souhaitez utiliser pour les connexions Bureau globalement. Si vous avez déjà défini une résolution personnalisée pour une application individuelle ou de la connexion, ce paramètre ne changez cela.
   >[!NOTE]
   >Lorsque vous modifiez un des paramètres d’affichage, ils s’appliquent uniquement aux nouvelles connexions à partir de ce point sur. Pour voir la modification dans une session, vous êtes déjà connecté pour se déconnecter, puis connectez-vous à nouveau.

### Barre de connexion

La connexion barre vous donne qu'accès aux contrôles de navigation supplémentaires. Par défaut, la barre de connexion est placée au milieu du haut de l’écran. L’appui prolongé et faites glisser la barre vers la gauche ou la droite pour le déplacer.

- **Contrôle Panorama**: le contrôle Panorama permet à l’écran être agrandi et déplacé. Notez que contrôle Panorama n’est plus disponible à l’aide de direct tactile.
   - Activer / désactiver le contrôle Panorama: appuyez sur l’icône de panoramique dans la barre de connexion pour afficher le contrôle Panoramique et Zoom sur l’écran. Appuyez sur l’icône de panoramique dans la barre de connexion à nouveau pour masquer le contrôle et renvoyer l’écran à sa résolution d’origine.
   - Utilisez le contrôle Panorama: appuyer et maintenir le mouvement panoramique de contrôle et faites glisser dans la direction que vous voulez déplacer l’écran.
   - Déplacer le contrôle Panorama: appuyez deux fois longuement sur le contrôle Panorama pour déplacer le contrôle sur l’écran.
- **Options supplémentaires**: appuyez sur l’icône des options supplémentaires pour afficher la sélection de session de la barre et de commande de la barre (voir ci-dessous).
- **Clavier**: appuyez sur l’icône du clavier pour afficher ou masquer le clavier. Le contrôle Panorama s’affiche automatiquement lorsque le clavier s’affiche.
- **Déplacer la barre de connexion**: appui et mise en attente de la barre de connexion, puis faites glisser et déplacer vers un nouvel emplacement dans la partie supérieure de l’écran.


### Barre de commandes

Appuyez sur la barre de connexion pour afficher la barre de commandes sur le côté droit de l’écran. Vous pouvez basculer entre les modes de souris (Touch Direct et le pointeur de souris). Utilisez le bouton Accueil pour revenir au centre de connexion à partir de la barre de commandes. Vous pouvez également utiliser le bouton Précédent pour la même action. Votre session active ne sera pas déconnectée. 


### Utilisez direct touch mouvements et modes de la souris dans une session à distance

Le client utilise des mouvements tactiles standard. Vous pouvez également utiliser des mouvements tactiles pour répliquer les actions de la souris sur le Bureau à distance. Les modes de souris sont définis dans le tableau ci-dessous.

> [!NOTE]
> Interagir avec Windows 8 ou une version ultérieure les mouvements tactiles native sont pris en charge en mode tactile Direct. 

| Mode souris    | Fonctionnement de la souris      | Mouvement                                                               |
|---------------|----------------------|-----------------------------------------------------------------------|
| Tactile direct  | Clic gauche           | appui avec 1 doigts                                                          |
| Tactile direct  | Clic droit          | 1 appui du doigt et mise en attente                                                 |
| Pointeur de souris | Zoom                 | Utilisez les 2 doigts et pincement pour effectuer un zoom avant ou déplacer des doigts des doigts pour effectuer un zoom arrière. |
| Pointeur de souris | Clic gauche           | appui avec 1 doigts                                                          |
| Pointeur de souris | Clic gauche et glissement  | Appuyez deux fois 1 doigt et mise en attente, puis faites glisser                               |
| Pointeur de souris | Clic droit          | appui avec doigts 2                                                          |
| Pointeur de souris | Clic avec le bouton droit et glissement | 2 Appuyez deux fois le doigt et mise en attente, puis faites glisser                               |
| Pointeur de souris | Roulette de souris          | doigt 2 Appuyez et de mise en attente, puis faites glisser vers le haut ou vers le bas                           |

> [!TIP]
> Questions et commentaires sont toujours accueil. Toutefois, veuillez ne publiez pas une demande de résolution des problèmes à l’aide de la fonctionnalité de commentaire à la fin de cet article. Au lieu de cela, accédez au [Forum sur le client Bureau à distance](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) et démarrer un nouveau thread. Vous avez une suggestion de fonctionnalité? Faites-nous part de sur le [forum de voix utilisateur client](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).
