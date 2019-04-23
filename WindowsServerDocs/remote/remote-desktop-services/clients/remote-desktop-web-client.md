---
title: Accéder au client web Bureau à distance
description: Décrit comment se connecter au client web Bureau à distance.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/20/2018
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: f4433ad592219d6ed15b28fd0514790b078525fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849370"
---
# <a name="access-the-remote-desktop-web-client"></a>Accéder au client web Bureau à distance

Le client web de bureau à distance vous permet d’utiliser un navigateur web compatible pour accéder aux ressources de votre organisation à distance (applications et bureau) publiés pour vous par votre administrateur. Vous serez en mesure d’interagir avec les applications à distance et les postes de travail comme vous le feriez avec un ordinateur local, où que vous soyez, sans avoir à basculer vers un autre PC de bureau. Une fois que votre administrateur a configuré vos ressources à distance, il que vous suffit sont votre domaine, le nom d’utilisateur, le mot de passe, l’URL de votre administrateur envoyé et un navigateur web pris en charge, et vous êtes fin prêt.

>[!NOTE]
>Curieux de savoir les nouvelles versions dans le client web ? Découvrez [Nouveautés de client web de bureau à distance ?](web-client-whatsnew.md)

## <a name="what-youll-need-to-use-the-web-client"></a>Ce dont vous aurez besoin utiliser le client web

* Pour le client web, vous aurez besoin d’un PC exécutant Windows, macOS, ChromeOS ou Linux. Les appareils mobiles ne sont pas pris en charge pour l’instant.
* Un navigateur moderne, telles que Microsoft Edge, Internet Explorer 11, Google Chrome, Safari ou Mozilla Firefox (v55.0 et versions ultérieures).
* L’URL de votre administrateur vous a envoyé.

>[!NOTE]
>La version d’Internet Explorer du client web n’a pas d’audio pour l’instant.
>Safari peut afficher un écran gris si le navigateur est redimensionné ou passe à plusieurs reprises en plein écran.

## <a name="start-using-the-remote-desktop-client"></a>Commencer à utiliser le client Bureau à distance

Pour vous connecter au client, accédez à l’URL de votre administrateur vous a envoyé. À la page de connexion, entrez votre nom de domaine et d’utilisateur au format ```DOMAIN\username```, entrez votre mot de passe, puis sélectionnez **connectez-vous**.

>[!NOTE]
>En vous connectant au client web, vous acceptez que votre ordinateur est conforme à la stratégie de sécurité de votre organisation.

Après vous être connecté, le client vous dirigera vers le **toutes les ressources** onglet, qui contient tous les éléments publiés pour vous sous un ou plusieurs groupes réductibles, tels que le groupe « Ressources de travail ». Vous verrez plusieurs icônes représentant des applications, des postes de travail ou des dossiers contenant plusieurs applications ou des postes de travail que l’administrateur a mis à disposition du groupe de travail. Vous pouvez revenir à cet onglet à tout moment pour lancer des ressources supplémentaires.

Pour commencer à utiliser une application ou le bureau, sélectionnez l’élément que vous souhaitez utiliser, entrez le même nom d’utilisateur et mot de passe utilisé pour vous connecter au client web si vous y êtes invité, puis **envoyer**. Vous pouvez également affichera une boîte de dialogue de consentement pour accéder aux ressources locales, telles que le Presse-papiers et d’imprimantes. Vous pouvez choisir de ne pas rediriger ces, ou sélectionnez **autoriser** à utiliser les paramètres par défaut. Attendez que le client web établir la connexion et puis démarrer à l’aide de la ressource comme vous le feriez normalement.

Lorsque vous avez terminé, vous pouvez terminer votre session en sélectionnant le **se déconnecter** bouton dans la barre d’outils en haut de votre écran ou en fermant la fenêtre du navigateur.

## <a name="printing-from-the-remote-desktop-web-client"></a>Impression à partir du client web de bureau à distance

Suivez ces étapes pour imprimer à partir du client web :

1. Démarrer le processus d’impression, comme vous le feriez normalement pour l’application que vous souhaitez imprimer à partir de.
2. Lorsque vous êtes invité à choisir une imprimante, sélectionnez **imprimante virtuel de bureau à distance**.
3. Après avoir choisi vos préférences, sélectionnez **impression**.
4. Votre navigateur génère un fichier PDF de votre travail d’impression.
5. Vous pouvez choisir ouvrir le fichier PDF et imprimer son contenu vers votre imprimante locale ou enregistrez-le sur votre PC pour une utilisation ultérieure.

## <a name="copy-and-paste-from-the-remote-desktop-web-client"></a>Copiez et collez à partir du client web de bureau à distance

Le client web prend actuellement en charge la copie et collage de texte uniquement. Fichiers ne peut pas être copiées ou collées vers et depuis le client web. En outre, vous pouvez uniquement utiliser **Ctrl + C** et **Ctrl + V** pour copier et coller du texte.

## <a name="get-help-with-the-web-client"></a>Obtenir de l’aide avec le client web

Si vous avez rencontré un problème qui ne peut pas être résolu par les informations contenues dans cet article, vous pouvez obtenir de l’aide avec le client web par courrier électronique à l’adresse sur la page About du client web.