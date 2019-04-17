---
title: Vous familiariser avec les services Bureau à distance sur iOS
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
ms.openlocfilehash: aab84dde3ded2a3d3f17f4bb318302321c444606
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297228"
---
# Vous familiariser avec les services Bureau à distance sur iOS

>S’applique à: Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Vous pouvez utiliser le client Bureau à distance pour iOS pour travailler avec les applications Windows, les ressources et les postes de travail à partir de votre appareil iOS (iPhone et iPads).

Utilisez les informations suivantes pour commencer. Veillez à examiner le [Forum aux questions sur](remote-desktop-client-faq.md) si vous avez des questions.

> [!NOTE]
> - Curieux de savoir sur les nouvelles versions du client iOS? Consultez l’article [Quelles sont les nouveautés pour les services Bureau à distance sur iOS?](ios-whatsnew.md)
> - Le client iOS prend en charge les appareils iOS en cours d’exécution 6.x et versions ultérieures.

## Obtenir le client Bureau à distance et démarrer l’utiliser

### Télécharger le client Bureau à distance à partir du store iOS
Suivez ces étapes pour vous familiariser avec les services Bureau à distance sur votre appareil iOS:

1. Télécharger le client Bureau à distance Microsoft à partir de [iTunes](https://itunes.apple.com/us/app/microsoft-remote-desktop/id714464092?mt=8).
2. [Configuration de votre PC pour accepter les connexions à distance](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop).
3. Ajoutez une [connexion Bureau à distance](#add-a-remote-desktop-connection) ou à une [ressource distante](#add-a-remote-resource). Vous utilisez une connexion pour se connecter à un directement sur un PC Windows et une ressource distante d’utiliser un programme RemoteApp, basée sur la session de bureau ou un ordinateur de bureau virtuel publié sur site à l’aide de connexions RemoteApp et bureau. Cette fonctionnalité est généralement disponible dans les environnements d’entreprise.

### Télécharger le client de bêta iOS Bureau à distance
Sur votre appareil iOS, suivez [ces instructions](https://aka.ms/rdiosbeta) pour télécharger le client de bêta iOS Bureau à distance.

### Ajouter une connexion Bureau à distance

Pour créer une connexion Bureau à distance: 
1. Dans l’appui du centre de connexion **+**, puis cliquez sur **Ajouter un PC ou un serveur**.
2. Entrez les informations suivantes pour la connexion Bureau à distance:
  - **Nom du PC** – le nom de l’ordinateur. Cela peut être un nom d’ordinateur Windows, un nom de domaine Internet ou une adresse IP. Vous pouvez également ajouter des informations de port pour le nom du PC (par exemple, **MyDesktop:3389** ou **10.0.0.1:3389**).
  - **Nom d’utilisateur** : le nom d’utilisateur à utiliser pour accéder à l’ordinateur distant. Vous pouvez utiliser les formats suivants: *nom_utilisateur*, *domaine\nom_utilisateur*ou *user_name@domain.com*. Vous pouvez également spécifier si vous souhaitez demander un nom d’utilisateur et mot de passe.
3. Vous pouvez également définir les options supplémentaires suivantes:
  - **Nom convivial (facultatif)** – un nom facile à retenir pour le PC que vous vous connectez à. Vous pouvez utiliser n’importe quelle chaîne, mais si vous ne spécifiez pas un nom convivial, le nom du PC s’affiche.
  - **Passerelle (facultatif)** – passerelle le Bureau à distance que vous souhaitez utiliser pour se connecter à des bureaux virtuels, des programmes RemoteApp et basée sur la session de postes de travail sur un réseau d’entreprise interne. Obtenir les informations sur la passerelle de votre administrateur système.
  - **Son** : sélectionner l’appareil à utiliser pour l’audio au cours de votre session à distance. Vous pouvez choisir de lire du contenu audio sur les appareils locales, l’appareil distant, ou pas du tout.
  - **Boutons de la souris d’échange** – chaque fois qu’un mouvement de souris envoie une commande avec le bouton gauche de la souris, il envoie la même commande avec le bouton droit de la souris à la place. Cela est nécessaire si l’ordinateur distant est configuré pour le mode souris gaucher.
  - **Mode administrateur** - se connecter à une session d’administration sur un serveur exécutant Windows Server 2003 ou version ultérieure.
4. Appuyez sur **Enregistrer**.

Vous avez besoin de modifier ces paramètres? Appuyez sur le bureau que vous souhaitez modifier de mise en attente et puis appuyez sur l’icône Paramètres. 

### Ajouter une ressource à distance
Ressources distantes sont des programmes RemoteApp, postes de travail basée sur la session et les bureaux virtuels publiées à l’aide de connexions RemoteApp et bureau.

- L’URL affiche le lien vers le serveur d’accès Web de bureau à distance qui vous donne accès aux connexions RemoteApp et bureau.
- Programmes RemoteApp et les connexions Bureau configuré sont répertoriées.

Pour ajouter une ressource à distance:

1. Sur l’écran de connexion centre, appuyez sur **+**, puis cliquez sur **Ajouter les ressources distantes**. 
2. Entrez les informations de la ressource à distance:
   - **URL du flux** - l’URL du serveur d’accès Web de bureau à distance. Vous pouvez également entrer votre compte de messagerie d’entreprise dans ce champ, cela indique au client pour rechercher le serveur d’accès Web Bureau à distance associées à votre adresse de messagerie.
   - **Nom d’utilisateur** : le nom d’utilisateur à utiliser pour le serveur d’accès Web de bureau à distance à que vous vous connectez.
   - **Mot de passe** - le mot de passe à utiliser pour le serveur d’accès Web de bureau à distance à que vous vous connectez.
3. Appuyez sur **Enregistrer**.

Les ressources distantes seront affichera dans le centre de la connexion.


## Se connecter à une passerelle des services Bureau à distance pour accéder à des ressources internes

Une passerelle des services Bureau à distance (passerelle des services Bureau à distance) vous permet de vous connecter à un ordinateur distant sur un réseau d’entreprise à partir de n’importe où sur Internet. Vous pouvez créer et gérer vos passerelles à l’aide du client Bureau à distance.

Pour configurer une nouvelle passerelle:

1. Dans le centre de la connexion, appuyez sur **paramètres > passerelles**. 
2. Cliquez sur **Ajouter des services Bureau à distance passerelle**.
3. Entrez les informations suivantes:
  - **Nom du serveur** : le nom de l’ordinateur que vous souhaitez utiliser sous forme de passerelle. Cela peut être un nom d’ordinateur Windows, un nom de domaine Internet ou une adresse IP. Vous pouvez également ajouter des informations de port pour le nom du serveur (par exemple: **RDGateway:443** ou **10.0.0.1:443**).
  - **Nom d’utilisateur** : le nom d’utilisateur et mot de passe à utiliser pour la passerelle des services Bureau à distance à que vous vous connectez. Vous pouvez également sélectionner **utiliser les informations d’identification de connexion** pour utiliser le même nom d’utilisateur et mot de passe que ceux utilisés pour la connexion Bureau à distance.


## Gérer vos comptes d’utilisateurs 

Lorsque vous vous connectez à un ordinateur de bureau ou à distance des ressources, vous pouvez enregistrer les comptes d’utilisateur pour sélectionner à partir d’une nouvelle fois. Vous pouvez gérer vos comptes d’utilisateurs à l’aide du client Bureau à distance.

Pour créer un compte d’utilisateur:

1. Dans le centre de la connexion, appuyez sur les **paramètres**et puis appuyez sur le **Nom d’utilisateur**.
2. Cliquez sur **Ajouter le compte d’utilisateur**.
3. Entrez les informations suivantes:
   - **Nom d’utilisateur** : le nom de l’utilisateur à enregistrer pour une utilisation avec une connexion à distance. Vous pouvez entrer le nom d’utilisateur dans un des formats suivants: nom_utilisateur, DOMAINE\nom_utilisateur, ou user_name@domain.com.
   - **Mot de passe** - le mot de passe pour l’utilisateur que vous avez spécifié. Chaque compte d’utilisateur que vous souhaitez enregistrer de manière à utiliser pour les connexions à distance doit avoir un mot de passe associé.
4. Appuyez sur **Enregistrer**, puis appuyez sur **paramètres**.
5. Appuyez sur **terminé** pour enregistrer la nouvelle configuration.

Pour supprimer un compte d’utilisateur:

1. Dans le centre de la connexion, appuyez sur **paramètres > noms d’utilisateur**.
2. Effectuez un balayage de la ligne de droite à gauche pour sélectionner l’utilisateur.
3. Cliquez sur **Supprimer**.



## Accédez à la session Bureau à distance
Lorsque vous démarrez une session Bureau à distance, il existe des outils que vous pouvez utiliser pour naviguer dans la session.

### Démarrer une connexion Bureau à distance

1. Appuyez sur la connexion Bureau à distance pour démarrer la session Bureau à distance. 
2. Si vous êtes invité à vérifier le certificat pour le Bureau à distance, appuyez sur **Accepter**. Vous pouvez choisir d’accepter toujours en faisant glisser le bouton bascule de **ne pas me demander à nouveau pour les connexions à cet ordinateur** à la **suite**. 

### Barre de connexion

La connexion barre vous donne qu'accès aux contrôles de navigation supplémentaires. 

- **Contrôle Panorama**: le contrôle Panorama permet à l’écran être agrandi et déplacé. Notez que contrôle Panorama n’est plus disponible à l’aide de direct tactile.
   - Activer / désactiver le contrôle Panorama: appuyez sur l’icône de panoramique dans la barre de connexion pour afficher le contrôle Panoramique et Zoom sur l’écran. Appuyez sur l’icône de panoramique dans la barre de connexion à nouveau pour masquer le contrôle et renvoyer l’écran à sa résolution d’origine.
   - Utilisez le contrôle Panorama: appuyer et maintenir le mouvement panoramique de contrôle et faites glisser dans la direction que vous voulez déplacer l’écran.
   - Déplacer le contrôle Panorama: appuyez deux fois longuement sur le contrôle Panorama pour déplacer le contrôle sur l’écran.
- **Nom de la connexion**: le nom de connexion actuel est affiché. Appuyez sur le nom de connexion pour afficher la barre de sélection de session.
- **Clavier**: appuyez sur l’icône du clavier pour afficher ou masquer le clavier. Le contrôle Panorama s’affiche automatiquement lorsque le clavier s’affiche.
- **Déplacer la barre de connexion**: appui et mise en attente de la barre de connexion, puis faites glisser et déplacer vers un nouvel emplacement dans la partie supérieure de l’écran.

### Sélection de session
Vous pouvez ouvrir plusieurs connexions à un autres PC en même temps. Appuyez sur la barre de connexion pour afficher la barre de sélection de session sur le côté gauche de l’écran. La barre de sélection de session vous permet d’afficher votre connexions ouvertes et de basculer entre les deux. 

- Basculer entre les applications dans une session de ressource à distance ouverte.

    Lorsque vous êtes connecté à des ressources distantes, vous pouvez basculer entre les applications ouvertes au sein de cette session en fait d’appuyer sur le menu de développement et en choisissant dans la liste des éléments disponibles.
- Démarrer une nouvelle session

  Vous pouvez démarrer des nouvelles applications ou les sessions de bureau au sein de votre connexion en cours: cliquez sur **Démarrer une nouvelle**et puis choisissez dans la liste des éléments disponibles.

- Déconnexion une session

  Pour déconnecter un appui de session X sur le côté gauche de la vignette de la session.

### Barre de commandes

La barre de commandes remplacé l’utilitaire version8.0.1 à partir de la barre. Vous pouvez basculer entre les modes de la souris et revenir au centre de connexion à partir de la barre de commandes.

## Utiliser des mouvements tactiles et des modes de la souris dans une session à distance

Le client utilise des mouvements tactiles standard. Vous pouvez également utiliser des mouvements tactiles pour répliquer les actions de la souris sur le Bureau à distance. Les modes de souris sont définis dans le tableau ci-dessous.

> [!NOTE]
> Interagir avec Windows 8 ou une version ultérieure les mouvements tactiles native sont pris en charge en mode tactile Direct. Pour plus d’informations sur Windows 8 mouvements voir [Touch: balayage, appui et au-delà](https://windows.microsoft.com/en-US/windows-8/touch-swipe-tap-beyond).

| Mode souris    | Fonctionnement de la souris      | Mouvement                                                    |
|---------------|----------------------|------------------------------------------------------------|
| Tactile direct  | Clic gauche           | appui avec 1 doigts                                               |
| Tactile direct  | Clic droit          | 1 appui du doigt et mise en attente                                      |
| Pointeur de souris | Clic gauche           | appui avec 1 doigts                                               |
| Pointeur de souris | Clic gauche et glissement  | Appuyez deux fois 1 doigt et mise en attente, puis faites glisser                    |
| Pointeur de souris | Clic droit          | appui avec doigts 2                                               |
| Pointeur de souris | Clic avec le bouton droit et glissement | 2 Appuyez deux fois le doigt et mise en attente, puis faites glisser                    |
| Pointeur de souris | Roulette de souris          | doigt 2 Appuyez et de mise en attente, puis faites glisser vers le haut ou vers le bas                |
| Pointeur de souris | Zoom                 | Pincement 2 doigts pour effectuer un zoom avant ou se propager 2 doigts pour effectuer un zoom arrière |

## Prise en charge des périphériques d’entrée

Le [client de bureau à distance iOS bêta](https://aka.ms/rdiosbeta) prend en charge les souris physiques Swiftpoint GT et ProPoint. Swiftpoint offre une [remise exclusive](https://www.swiftpoint.com/microsoft/) sur le GT pour les utilisateurs du client bêta iOS.

Actuellement, le client iOS prend uniquement en charge Swiftpoint souris. Reportez-vous à la page [Nouveautés du client iOS](ios-whatsnew.md) et le [Magasin d’application iOS](https://aka.ms/rdios) pour les actualités sur la prise en charge pour d’autres périphériques à l’avenir.

## Utiliser un clavier dans une session à distance

Vous pouvez utiliser un à l’écran clavier ou un clavier physique dans votre session à distance.

Pour à l’écran de claviers, utilisez le bouton sur le côté droit de la barre au-dessus du clavier pour basculer entre le clavier standard et d’autres.

Si Bluetooth est activé pour votre appareil iOS, le client détecte automatiquement le clavier Bluetooth.

N’oubliez pas que, en raison des limitations sur le système d’exploitation, les touches spéciales telles que Ctrl, Option et fonction ne fonctionnent pas comme prévu avec un clavier Bluetooth. Les clés suivantes fonctionnent:

- Touches alphanumériques
- Touches du curseur
- Onglet: Onglet celui-ci fonctionne, mais Maj + Tab ne fonctionne pas
- Famille / Pos1: Alt + gauche = Home
- Fin: Alt + droite = fin
- Page précédente: Alt + haut = Page précédente
- Page suivante: Alt + Bas = Page suivante
- Tout sélectionner: Commande + A = Ctrl + A (Sélectionner tout dans la plupart des programmes)
- Couper: Commande + X = Ctrl + X (Couper dans la plupart des programmes)
- Copie: Commande + C = Ctrl + C (Copy dans la plupart des programmes)
- Coller: Commande + V = Ctrl + V (coller dans la plupart des programmes)
- Symboles: Clés Alt + alphanumérique émet des symboles différents en fonction de la langue configurée

> [!TIP]
> Questions et commentaires sont toujours accueil. Toutefois, veuillez ne publiez pas une demande de résolution des problèmes à l’aide de la fonctionnalité de commentaire à la fin de cet article. Au lieu de cela, accédez au [Forum sur le client Bureau à distance](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) et démarrer un nouveau thread. Vous avez une suggestion de fonctionnalité? Faites-nous part de sur le [forum de voix utilisateur client](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).

