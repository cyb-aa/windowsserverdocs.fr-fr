---
title: Bien démarrer avec le client Windows Desktop
description: Informations de base sur le client Windows Desktop.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: daveba
ms.author: helohr
ms.date: 09/13/2019
ms.localizationpriority: medium
ms.openlocfilehash: c864ba0e51054a553bfd53f845bd4d1c9ff3c8ba
ms.sourcegitcommit: 61767c405da44507bd3433967543644e760b20aa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/14/2019
ms.locfileid: "70988237"
---
# <a name="get-started-with-the-windows-desktop-client"></a>Bien démarrer avec le client Windows Desktop

>S’applique à : Windows 10 et Windows 7

Vous pouvez utiliser le client Bureau à distance pour Windows Desktop afin d’accéder aux applications et aux appareils de bureau Windows à distance à partir d’un autre appareil Windows.

> [!NOTE]
> - Cette documentation ne concerne pas le client Connexion Bureau à distance (MSTSC) fourni avec Windows. Elle s’applique au client Bureau à distance (MSRDC).
> - Actuellement, ce client ne permet d’accéder aux applications et aux appareils de bureau Windows à distance qu’à partir de [Windows Virtual Desktop](https://aka.ms/wvd).
> - Vous êtes curieux de découvrir les nouvelles versions du client Windows Desktop ? Consultez [Nouveautés du client Windows Desktop](windowsdesktop-whatsnew.md).

## <a name="install-the-client"></a>Installer le client

Vous pouvez actuellement télécharger le client pour Windows 64 bits. Nous mettrons à jour cette liste quand le client sera disponible pour d’autres versions de Windows.

- [Client Windows 64 bits](https://go.microsoft.com/fwlink/?linkid=2068602)

Vous pouvez installer le client pour l’utilisateur actuel, ce qui ne nécessite pas de droits d’administrateur, ou votre administrateur peut installer et configurer le client afin que tous les utilisateurs de l’appareil puissent y accéder.

Une fois le client installé, vous pouvez le lancer à partir du menu Démarrer en recherchant **Bureau à distance**.

## <a name="feeds"></a>Flux

Accédez à la liste des ressources gérées auxquelles vous pouvez accéder, notamment les applications et les appareils de bureau, en vous abonnant au flux fourni par votre administrateur. Quand vous vous abonnez, les ressources deviennent disponibles sur votre ordinateur local. Le client Windows Desktop prend actuellement en charge les ressources publiées à partir de Windows Virtual Desktop.

### <a name="subscribe-to-a-feed"></a>S’abonner à un flux

1. Dans la page principale du client, également appelée Centre de connexion, cliquez sur **S’abonner**.
2. Connectez-vous avec votre compte d’utilisateur quand vous y êtes invité.
3. Les ressources qui apparaissent dans le Centre de connexion sont regroupées par espace de travail.

Pour lancer des ressources, utilisez l’une des méthodes suivantes :

- Accédez au Centre de connexion, puis double-cliquez sur une ressource pour la lancer.
- Vous pouvez également accéder au menu Démarrer et rechercher un dossier avec le nom de l’espace de travail ou entrer le nom de la ressource dans la barre de recherche.

### <a name="workspace-details"></a>Détails de l’espace de travail

Une fois abonné, vous pouvez afficher des informations supplémentaires sur un espace de travail dans le panneau Détails :

- Nom de l’espace de travail
- URL et nom d’utilisateur utilisés pour s’abonner
- Nombre d’applications et d’appareils de bureau
- Date/heure de la dernière mise à jour
- État de la dernière mise à jour

Accès au panneau Détails :

1. Dans le Centre de connexion, appuyez sur le menu de dépassement ( **...** ) à côté de l’espace de travail.
2. Sélectionnez **Détails** dans le menu déroulant.
3. Le panneau Détails s’affiche sur le côté droit du client.

Une fois que vous êtes abonné, l’espace de travail est automatiquement mis à jour à intervalles réguliers. Les ressources peuvent être ajoutées, changées ou supprimées en fonction des modifications apportées par votre administrateur.

Vous pouvez également rechercher manuellement les mises à jour des ressources si nécessaire en sélectionnant **Mettre à jour maintenant** dans le panneau Détails.

### <a name="unsubscribe-from-a-feed"></a>Se désabonner d’un flux

Cette section vous apprend à vous désabonner d’un flux. Vous pouvez vous désabonner pour vous réabonner avec un autre compte ou supprimer vos ressources du système.

1. Dans le Centre de connexion, appuyez sur le menu de dépassement ( **...** ) à côté de l’espace de travail.
2. Dans le menu déroulant, sélectionnez **Se désabonner**.
3. Passez en revue la boîte de dialogue et sélectionnez **Continuer**.

## <a name="update-the-client"></a>Mettre à jour le client

Sauf si cette option est désactivée par un administrateur, vous êtes notifié quand une nouvelle version du client est disponible. Cette notification peut s’afficher directement dans le Centre de connexion ou dans le Centre de notifications Windows. Sélectionnez la notification pour démarrer le processus de mise à jour.

Vous pouvez également rechercher manuellement les nouvelles mises à jour du client :

1. Dans le Centre de connexion, appuyez sur le menu de dépassement ( **...** ) dans la barre de commandes en haut du client.
2. Sélectionnez **À propos de** dans le menu déroulant.
3. Appuyez sur **Rechercher les mises à jour**.
4. Si une mise à jour est disponible, appuyez sur **Installer la mise à jour** pour mettre à jour le client.

## <a name="providing-feedback"></a>Formulation de commentaires

Vous souhaitez suggérer une fonctionnalité ou signaler un problème ? Utilisez le [Hub de commentaires](feedback-hub://?tabid=2&contextid=883) (également accessible à partir du client) :

1. Dans le Centre de connexion, appuyez sur le menu de dépassement ( **...** ) dans la barre de commandes en haut du client.
2. Sélectionnez **Commentaires** dans le menu déroulant pour ouvrir le Hub de commentaires.
