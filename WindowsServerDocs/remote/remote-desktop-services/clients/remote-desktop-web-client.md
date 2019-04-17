---
title: Accéder au client web Bureau à distance
description: Décrit comment se connecter au client web services Bureau à distance.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/20/2018
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: f4433ad592219d6ed15b28fd0514790b078525fd
ms.sourcegitcommit: d5f10c0c98a9976a86be9f4fa8866650c7fcfc1a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/12/2019
ms.locfileid: "9065856"
---
# Accéder au client web Bureau à distance

Le client de web des services Bureau à distance vous permet d’utiliser un navigateur web compatible pour accéder à des ressources distantes de votre organisation (applications et les bureaux) publiées pour vous par votre administrateur. Vous serez en mesure d’interagir avec les applications à distance et les ordinateurs de bureau comme vous le feriez avec un PC local, quelle que soit l’où vous vous trouvez, sans avoir à basculer vers un autre PC de bureau. Une fois que votre administrateur configure vos ressources à distance, il que vous suffit sont votre domaine, le nom d’utilisateur, le mot de passe, l’URL de votre administrateur envoyé vous et un navigateur web pris en charge, et vous pouvez commencer.

>[!NOTE]
>Curieux de savoir sur les nouvelles versions du client web? Consultez l’article [Nouveautés pour le client de bureau à distance web?](web-client-whatsnew.md)

## Ce que vous devez utiliser le client web

* Pour que le client web, vous aurez besoin d’un PC exécutant Windows, Mac OS, ChromeOS ou Linux. Les appareils mobiles ne sont pas prises en charge pour l’instant.
* Un navigateur modern telles que Microsoft Edge, Internet Explorer 11, Google Chrome, Safari ou Mozilla Firefox (v55.0 et versions ultérieures).
* L’URL de votre administrateur vous a envoyé.

>[!NOTE]
>La version d’Internet Explorer du client web n’a pas de données audio pour l’instant.
>Safari peut afficher un écran gris si le navigateur est redimensionné ou entre plein écran plusieurs fois.

## Commencer à utiliser le client Bureau à distance

Pour vous connecter au client, accédez à l’URL de votre administrateur vous a envoyé. À la page de connexion, entrez votre nom d’utilisateur et de domaine dans le format ```DOMAIN\username```, entrez votre mot de passe et sélectionnez **se connecter**.

>[!NOTE]
>En vous connectant au client web, vous acceptez que votre ordinateur est conforme à la stratégie de sécurité de votre organisation.

Une fois que vous vous connectez, le client vous serez redirigé vers l’onglet **Toutes les ressources** , qui contient tous les éléments de publication pour vous sous un ou plusieurs groupes réductibles, par exemple, le groupe «Ressources de travail». Vous verrez plusieurs icônes représentant les applications, ordinateurs de bureau ou des dossiers contenant plusieurs applications ou des postes de travail que l’administrateur a mis à disposition pour le groupe de travail. Vous pouvez revenir à cet onglet à tout moment pour lancer des ressources supplémentaires.

Pour commencer à utiliser une application ou le bureau, sélectionnez l’élément que vous souhaitez utiliser, entrez le même nom d’utilisateur et mot de passe utilisé pour se connecter au client web si vous y êtes invité, puis **Soumettre**. Vous pouvez affichent également une boîte de dialogue de consentement pour accéder aux ressources locales, telles que le Presse-papiers et l’imprimante. Vous pouvez choisir de pas rediriger une de ces, ou sélectionnez **Autoriser** à utiliser les paramètres par défaut. Attendez que le client web établir la connexion, puis démarrez à l’aide de la ressource comme vous le feriez normalement.

Lorsque vous avez terminé, vous pouvez mettre fin à votre session en sélectionnant le bouton de **Déconnexion** dans la barre d’outils dans la partie supérieure de votre écran ou fermer la fenêtre de navigateur.

## Impression à partir du client de web des services Bureau à distance

Suivez ces étapes pour imprimer à partir du client web:

1. Démarrez le processus d’impression comme vous le feriez normalement pour l’application que vous voulez imprimer à partir de.
2. Lorsque vous êtes invité à choisir une imprimante, sélectionnez **l’Imprimante virtuel de bureau à distance**.
3. Après avoir choisi vos préférences, sélectionnez **Imprimer**.
4. Votre navigateur génère un fichier PDF de votre travail d’impression.
5. Vous pouvez choisir ouvrir le fichier PDF et imprimer son contenu sur votre imprimante locale ou enregistrez-le sur votre PC pour une utilisation ultérieure.

## Copier- coller à partir du client de web des services Bureau à distance

Le client web prend actuellement en charge la copie et collage de texte uniquement. Les fichiers ne peuvent pas être copiées ou collées vers et depuis le client web. En outre, vous pouvez utiliser uniquement **Ctrl + C** et **Ctrl + V** pour copier et coller du texte.

## Obtenez de l’aide avec le client web

Si vous avez rencontré un problème qui ne peut pas être résolu par les informations contenues dans cet article, vous pouvez obtenir des informations sur le client web par courrier électronique à l’adresse sur la page à propos du client web.