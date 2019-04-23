---
title: Bien démarrer avec le Bureau à distance sur iOS
description: Découvrez comment configurer le client Bureau à distance pour iOS
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 03ec5a3d-d3f2-4afd-9405-ae58b6ecc91c
author: lizap
manager: dongill
ms.author: elizapo
date: 01/13/2017
ms.localizationpriority: medium
ms.openlocfilehash: 1a1939c7fd6d25d756369c85e4adaa6c15195b37
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889630"
---
# <a name="get-started-with-remote-desktop-on-ios"></a>Bien démarrer avec le Bureau à distance sur iOS

>S'applique à : Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2016

Vous pouvez utiliser le client Bureau à distance pour iOS pour travailler avec les applications Windows, les ressources et les postes de travail à partir de votre appareil iOS (iPhone et iPad).

Utilisez les informations suivantes pour commencer. Veillez à consulter le [FAQ](remote-desktop-client-faq.md) si vous avez des questions.

> [!NOTE]
> - Curieux de savoir les nouvelles versions du client iOS ? Découvrez [quelles sont les nouveautés pour Bureau à distance sur iOS ?](ios-whatsnew.md)
> - Le client iOS prend en charge les appareils exécutant iOS 6.x et les versions ultérieures.

## <a name="get-the-remote-desktop-client-and-start-using-it"></a>Obtenir le client Bureau à distance et l’utiliser

### <a name="download-the-remote-desktop-client-from-the-ios-store"></a>Télécharger le client Bureau à distance à partir de l’App store iOS
Suivez ces étapes pour bien démarrer avec le Bureau à distance sur votre appareil iOS :

1. Télécharger le client Bureau à distance Microsoft à partir de [iTunes](https://itunes.apple.com/us/app/microsoft-remote-desktop/id714464092?mt=8).
2. [Configurer votre PC pour accepter les connexions à distance](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop).
3. Ajouter un [connexion Bureau à distance](#add-a-remote-desktop-connection) ou un [ressource distante](#add-a-remote-resource). Vous utilisez une connexion pour se connecter à un directement à un PC Windows et à une ressource distante à utiliser un programme RemoteApp, basés sur des sessions de bureau ou un bureau virtuel publié en local à l’aide de connexions RemoteApp et bureau. Cette fonctionnalité est généralement disponible dans les environnements d’entreprise.

### <a name="download-the-remote-desktop-ios-beta-client"></a>Télécharger le client de bêta de bureau à distance iOS
Sur votre appareil iOS, suivez [ces instructions](https://aka.ms/rdiosbeta) pour télécharger le client Bureau à distance iOS version bêta.

### <a name="add-a-remote-desktop-connection"></a>Ajouter une connexion Bureau à distance

Pour créer une connexion Bureau à distance : 
1. Dans, appuyez sur le centre de connexion **+**, puis appuyez sur **ajouter un PC ou un serveur**.
2. Entrez les informations suivantes pour la connexion Bureau à distance :
  - **Nom du PC** – le nom de l’ordinateur. Cela peut être un nom d’ordinateur Windows, un nom de domaine Internet ou une adresse IP. Vous pouvez également ajouter des informations de port pour le nom du PC (par exemple, **MyDesktop:3389** ou **10.0.0.1:3389**).
  - **Nom d’utilisateur** – le nom d’utilisateur à utiliser pour accéder à l’ordinateur distant. Vous pouvez utiliser les formats suivants : *user_name*, *domaine\nom_utilisateur*, ou *user_name@domain.com*. Vous pouvez également spécifier s’il faut demander un nom d’utilisateur et mot de passe.
3. Vous pouvez également définir des options supplémentaires suivantes :
  - **Nom convivial (facultatif)** : un nom facile à mémoriser pour le PC que vous vous connectez à. Vous pouvez utiliser n’importe quelle chaîne, mais si vous ne spécifiez pas un nom convivial, le nom du PC s’affiche.
  - **(Facultative) de la passerelle** – passerelle le Bureau à distance que vous souhaitez utiliser pour se connecter à des bureaux virtuels, des programmes RemoteApp et des ordinateurs de bureau basés sur des sessions sur un réseau d’entreprise interne. Obtenir les informations sur la passerelle de votre administrateur système.
  - **Son** – sélectionnez l’appareil à utiliser pour l’audio pendant votre session à distance. Vous pouvez choisir de lire un son sur les appareils locaux, le périphérique distant, ou pas du tout.
  - **Permuter les boutons de la souris** : chaque fois qu’un mouvement de souris envoie une commande avec le bouton gauche de la souris, il envoie la même commande avec le bouton droit de la souris à la place. Cela est nécessaire si l’ordinateur distant est configuré pour le mode gaucher de la souris.
  - **Mode administrateur** -se connecter à une session d’administration sur un serveur exécutant Windows Server 2003 ou version ultérieure.
4. Appuyez sur **enregistrer**.

Vous avez besoin pour modifier ces paramètres ? Appuyez sur et maintenez le poste de travail que vous souhaitez modifier puis appuyez sur l’icône des paramètres. 

### <a name="add-a-remote-resource"></a>Ajouter une ressource distante
Ressources distantes sont des programmes RemoteApp, bureaux basés sur session et des bureaux virtuels publiées à l’aide de connexions RemoteApp et bureau.

- L’URL affiche le lien au serveur d’accès Web de bureau à distance qui vous donne accès aux connexions RemoteApp et bureau.
- Le configuré connexions RemoteApp et bureau sont répertoriées.

Pour ajouter une ressource distante :

1. Dans l’écran de centre de connexion, appuyez sur **+**, puis appuyez sur **ajouter des ressources à distance**. 
2. Entrez les informations pour la ressource distante :
   - **URL du flux** -l’URL du serveur d’accès Web de bureau à distance. Vous pouvez également entrer votre compte de messagerie d’entreprise dans ce champ : cela indique au client pour rechercher le serveur d’accès Web Bureau à distance associé à votre adresse de messagerie.
   - **Nom d’utilisateur** -nom d’utilisateur à utiliser pour le serveur d’accès Web de bureau à distance que vous vous connectez à.
   - **Mot de passe** -mot de passe à utiliser pour le serveur d’accès Web de bureau à distance que vous vous connectez à.
3. Appuyez sur **enregistrer**.

Les ressources à distance seront affichera dans le centre de connexion.


## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Se connecter à une passerelle Bureau à distance pour accéder aux ressources internes

Une passerelle des services Bureau à distance (passerelle RD) vous permet de vous connecter à un ordinateur distant sur un réseau d’entreprise à partir de n’importe où sur Internet. Vous pouvez créer et gérer vos passerelles à l’aide du client Bureau à distance.

Pour configurer une nouvelle passerelle :

1. Dans le centre de connexion, appuyez sur **Paramètres > passerelles**. 
2. Appuyez sur **passerelle d’ajouter le Bureau à distance**.
3. Entrez les informations suivantes :
  - **Nom du serveur** – le nom de l’ordinateur que vous souhaitez utiliser en tant que passerelle. Cela peut être un nom d’ordinateur Windows, un nom de domaine Internet ou une adresse IP. Vous pouvez également ajouter des informations de port au nom du serveur (par exemple : **RDGateway:443** ou **10.0.0.1:443**).
  - **Nom d’utilisateur** -le nom d’utilisateur et le mot de passe à utiliser pour la passerelle Bureau à distance que vous êtes connecté. Vous pouvez également sélectionner **utiliser les informations d’identification de connexion** à utiliser le même nom d’utilisateur et le mot de passe que ceux utilisés pour la connexion Bureau à distance.


## <a name="manage-your-user-accounts"></a>Gérer vos comptes d’utilisateur 

Lorsque vous vous connectez à un bureau ou à distance des ressources, vous pouvez enregistrer les comptes d’utilisateur à sélectionner à nouveau. Vous pouvez gérer vos comptes d’utilisateur en utilisant le client Bureau à distance.

Pour créer un nouveau compte d’utilisateur :

1. Dans le centre de connexion, appuyez sur **paramètres**, puis appuyez sur **noms d’utilisateur**.
2. Appuyez sur **ajouter le compte d’utilisateur**.
3. Entrez les informations suivantes :
   - **Nom d’utilisateur** -le nom de l’utilisateur à enregistrer pour une utilisation avec une connexion à distance. Vous pouvez entrer le nom d’utilisateur dans un des formats suivants : user_name, DOMAINE\nom_utilisateur, ou user_name@domain.com.
   - **Mot de passe** -le mot de passe pour l’utilisateur spécifié. Chaque compte d’utilisateur que vous souhaitez enregistrer pour utiliser des connexions à distance doit avoir un mot de passe associé.
4. Appuyez sur **enregistrer**, puis appuyez sur **paramètres**.
5. Appuyez sur **fait** pour enregistrer la nouvelle configuration.

Pour supprimer un compte d’utilisateur :

1. Dans le centre de connexion, appuyez sur **Paramètres > noms d’utilisateur**.
2. Faites défiler la ligne de droite à gauche pour sélectionner l’utilisateur.
3. Appuyez sur **supprimer**.



## <a name="navigate-the-remote-desktop-session"></a>Accédez à la session Bureau à distance
Lorsque vous démarrez une session Bureau à distance, il existe des outils que vous pouvez utiliser pour parcourir la session.

### <a name="start-a-remote-desktop-connection"></a>Démarrer une connexion Bureau à distance

1. Appuyez sur la connexion Bureau à distance pour démarrer la session Bureau à distance. 
2. Si vous êtes invité à vérifier le certificat pour le Bureau à distance, appuyez sur **Accept**. Vous pouvez choisir de toujours accepter en faisant glisser le **me redemander pour les connexions à cet ordinateur** activer/désactiver pour **ON**. 

### <a name="connection-bar"></a>Barre de connexion

La connexion barre vous donne qu'accès aux contrôles de navigation supplémentaires. 

- **Contrôle Panorama**: Le contrôle panoramique permet à l’écran être agrandi et déplacé. Notez que contrôle Panorama est uniquement disponible si vous utilisez l’interaction tactile directe.
   - Activer / désactiver le contrôle Panorama : Appuyez sur l’icône de panoramique dans la barre de connexion pour afficher le contrôle Panoramique et Zoom sur l’écran. Appuyez sur l’icône de panoramique dans la barre de connexion à nouveau à masquer le contrôle et de retourner l’écran à sa résolution d’origine.
   - Utilisez le contrôle Panorama : Appuyez sur le contrôle Panoramique et faites-le glisser dans la direction que vous souhaitez déplacer l’écran.
   - Déplacer le contrôle Panorama : Double cliquez et maintenez le contrôle panoramique pour déplacer le contrôle sur l’écran.
- **Nom de la connexion**: Le nom de connexion actuel est affiché. Appuyez sur le nom de connexion pour afficher la barre de sélection de session.
- **Clavier**: Appuyez sur l’icône de clavier pour afficher ou masquer le clavier. Le contrôle Panorama s’affiche automatiquement lorsque le clavier est affiché.
- **Déplacer la barre de connexion**: Appuyez et maintenez la barre de connexion et puis faites glisser vers un nouvel emplacement en haut de l’écran.

### <a name="session-selection"></a>Sélection de la session
Vous pouvez ouvrir plusieurs connexions à différents PC en même temps. Appuyez sur la barre de connexion pour afficher la barre de sélection de session sur le côté gauche de l’écran. La barre de sélection de session vous permet d’afficher vos connexions ouvertes et basculer entre eux. 

- Basculer entre les applications dans une session d’ouvrir une ressource à distance.

    Lorsque vous êtes connecté à des ressources distantes, vous pouvez basculer entre les applications ouvertes au sein de cette session en appuyant sur le menu de l’expander et en choisissant dans la liste des éléments disponibles.
- Démarrez une nouvelle session

  Vous pouvez démarrer de nouvelles applications ou les sessions Bureau à partir d’au sein de votre connexion actuelle : appuyez sur **démarrer nouveau**, puis choisissez dans la liste des éléments disponibles.

- Déconnexion une session

  Pour déconnecter un drainage de session X dans la partie gauche de la vignette de la session.

### <a name="command-bar"></a>Barre de commandes

La barre de commandes remplacé l’utilitaire barre depuis la version 8.0.1. Vous pouvez basculer entre les modes de la souris et revenir au centre de connexion à partir de la barre de commandes.

## <a name="use-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Utiliser des gestes tactiles et les modes de la souris dans une session à distance

Le client utilise des gestes tactiles standard. Vous pouvez également utiliser des gestes tactiles pour répliquer les actions de la souris sur le Bureau à distance. Les modes de la souris disponibles sont définies dans le tableau ci-dessous.

> [!NOTE]
> Interaction avec Windows 8 ou version ultérieure les mouvements tactiles natif sont pris en charge en mode d’interaction tactile directe. Pour plus d’informations sur Windows 8 mouvements consultez [Touch : Effectuez un balayage, appuyez sur et au-delà](https://windows.microsoft.com/en-US/windows-8/touch-swipe-tap-beyond).

| Mode souris    | Opération de la souris      | Mouvement                                                    |
|---------------|----------------------|------------------------------------------------------------|
| Interaction tactile directe  | Clic gauche           | Appuyez sur 1 doigt                                               |
| Interaction tactile directe  | Clic droit          | 1 doigt maintenez                                      |
| Pointeur de la souris | Clic gauche           | Appuyez sur 1 doigt                                               |
| Pointeur de la souris | Clic gauche et glissement  | appuyer deux fois 1 doigt et maintenir, puis faites glisser                    |
| Pointeur de la souris | Clic droit          | Appuyez sur 2 doigt                                               |
| Pointeur de la souris | Clic avec le bouton droit et glissement | 2 double pression du doigt et maintenir, puis faites glisser                    |
| Pointeur de la souris | Roulette de la souris          | 2 doigt appuyez sur et maintenez la touche, puis faites glisser vers le haut ou vers le bas                |
| Pointeur de la souris | Zoom                 | Pincement 2 doigts pour faire un zoom ou répartir les 2 doigts pour effectuer un zoom arrière |

## <a name="supported-input-devices"></a>Prise en charge des périphériques d’entrée

Le [client de bureau à distance iOS bêta](https://aka.ms/rdiosbeta) prend en charge des souris Swiftpoint GT et ProPoint physiques. Swiftpoint offre un [remise exclusive](https://www.swiftpoint.com/microsoft/) sur le total général pour les utilisateurs de client iOS version bêta.

Actuellement, le client iOS prend uniquement en charge Swiftpoint mice. Reportez-vous à la [quelles sont les nouveautés dans le client iOS](ios-whatsnew.md) page et le [iOS App Store](https://aka.ms/rdios) des actualités sur la prise en charge pour d’autres appareils à l’avenir.

## <a name="use-a-keyboard-in-a-remote-session"></a>Utiliser un clavier dans une session à distance

Vous pouvez utiliser l’une à l’écran clavier ou du clavier physique dans votre session à distance.

Pour l’écran de claviers, utilisez le bouton sur le bord droit de la barre située au-dessus du clavier pour basculer entre le clavier standard et d’autres.

Si Bluetooth est activé pour votre appareil iOS, le client détecte automatiquement le clavier Bluetooth.

N’oubliez pas que, en raison des limitations sur le système d’exploitation, touches spéciales telles que Ctrl, Option et la fonction ne fonctionnent pas comme prévu avec un clavier Bluetooth. Le travail clés suivantes :

- Clés d’alphanumériques
- Clés du curseur
- Onglet : Onglet fonctionne, mais Maj + Tab ne fonctionne pas
- Accueil / Pos1 : ALT + gauche = Home
- Fin : ALT + flèche droite = fin
- Page précédente : ALT + flèche haut = Pg préc
- Page suivante : ALT + flèche Bas = Pg suiv
- Sélectionnez toutes les : Commande + A = Ctrl + A (Sélectionner tout dans la plupart des programmes)
- Couper : Commande + X = Ctrl + X (Couper dans la plupart des programmes)
- Copier : Commande + C = Ctrl + C (copier dans la plupart des programmes)
- Coller : Commande + V = Ctrl + V (coller dans la plupart des programmes)
- Symboles : Les touches ALT + alphanumérique produira des symboles différents selon la langue configurée

> [!TIP]
> Questions et vos commentaires sont toujours les bienvenus. Toutefois, ne posez pas une demande de résolution des problèmes à l’aide de la fonctionnalité de commentaire à la fin de cet article. Au lieu de cela, accédez à la [forum du client Bureau à distance](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) et démarrer un nouveau thread. Vous avez une suggestion de fonctionnalité ? Dites-nous dans le [forum vocal des utilisateurs client](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).

