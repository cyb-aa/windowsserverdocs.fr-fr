---
title: Bien démarrer avec le client web
description: Décrit la procédure à suivre pour se connecter au client web Bureau à distance.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 08/27/2019
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: 11e68821fb095617cea19ee83c057d247a909604
ms.sourcegitcommit: 51eaab0f860312d97293fd90f3e632e7caee3df1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70150972"
---
# <a name="get-started-with-the-web-client"></a>Bien démarrer avec le client web

Le client web Bureau à distance vous permet d’utiliser un navigateur web compatible pour accéder aux ressources distantes de votre organisation (applications et bureaux) publiées pour vous par votre administrateur. Vous pouvez interagir avec les bureaux et applications distants comme vous le feriez avec un PC local, quel que soit l’endroit où vous êtes, sans avoir à basculer sur un autre PC de bureau. Une fois que votre administrateur a configuré vos ressources distantes, vous avez uniquement besoin des éléments suivants : votre domaine, votre nom d’utilisateur, votre mot de passe, l’URL envoyé par votre administrateur et un navigateur web pris en charge.

>[!NOTE]
>Vous êtes curieux de découvrir les nouvelles versions du client web ? Consultez [Nouveautés du client web Bureau à distance](web-client-whatsnew.md)

## <a name="what-youll-need-to-use-the-web-client"></a>Ce dont vous aurez besoin pour utiliser le client web

* Pour le client web, vous avez besoin d’un PC exécutant Windows, macOS, ChromeOS ou Linux. Les appareils mobiles ne sont pas pris en charge pour l’instant.
* Un navigateur moderne, tel que Microsoft Edge, Internet Explorer 11, Google Chrome, Safari ou Mozilla Firefox (v55.0 et versions ultérieures).
* L’URL que votre administrateur vous a envoyée.

>[!NOTE]
>La version d’Internet Explorer du client web n’a pas d’audio pour l’instant.
>Safari peut afficher un écran gris si le navigateur est redimensionné ou passe à plusieurs reprises en mode plein écran.

## <a name="start-using-the-remote-desktop-client"></a>Commencer à utiliser le client Bureau à distance

Pour vous connecter au client, accédez à l’URL que votre administrateur vous a envoyée. Sur la page de connexion, entrez votre domaine et nom d’utilisateur au format ```DOMAIN\username```, entrez votre mot de passe, puis sélectionnez **Se connecter**.

>[!NOTE]
>En vous connectant au client web, vous acceptez que votre PC soit conforme à la stratégie de sécurité de votre organisation.

Après vous être connecté, le client vous redirige vers l’onglet **Toutes les ressources**, qui contient tous les éléments publiés pour vous dans un ou plusieurs groupes réductibles, tels que le groupe « Ressources de travail ». Vous pouvez voir plusieurs icônes représentant les applications, bureaux ou dossiers contenant plusieurs applications ou bureaux que l’administrateur a mis à disposition du groupe de travail. Vous pouvez revenir à cet onglet à tout moment pour lancer des ressources supplémentaires.

Pour commencer à utiliser une application ou un bureau, sélectionnez l’élément que vous souhaitez utiliser, entrez les mêmes nom d’utilisateur et mot de passe que ceux ayant servi à vous connecter au client web si vous y êtes invité, puis sélectionnez **Envoyer**. Une boîte de dialogue de consentement peut également s’afficher pour vous permettre d’accéder aux ressources locales, telles que le Presse-papiers et l’imprimante. Vous pouvez choisir de n’effectuer aucune redirection ou de sélectionner **Autoriser** pour utiliser les paramètres par défaut. Attendez que le client web établisse la connexion, puis commencez à utiliser la ressource comme vous le feriez normalement.

Lorsque vous avez fini, vous pouvez terminer votre session en sélectionnant le bouton **Se déconnecter** dans la barre d’outils en haut de votre écran ou en fermant la fenêtre du navigateur.

## <a name="printing-from-the-remote-desktop-web-client"></a>Impression à partir du client web Bureau à distance

Suivez ces étapes pour imprimer à partir du client web :

1. Démarrez le processus d’impression, comme vous le feriez normalement pour l’application à partir de laquelle vous souhaitez imprimer.
2. Lorsque vous êtes invité à choisir une imprimante, sélectionnez **Remote Desktop Virtual Printer** (Imprimante virtuelle Bureau à distance).
3. Après avoir choisi vos préférences, sélectionnez **Imprimer**.
4. Votre navigateur génère un fichier PDF de votre travail d’impression.
5. Vous pouvez choisir d’ouvrir le fichier PDF et d’imprimer son contenu vers votre imprimante locale ou de l’enregistrer sur votre PC pour une utilisation ultérieure.

## <a name="copy-and-paste-from-the-remote-desktop-web-client"></a>Copier et coller à partir du client web Bureau à distance

Le client web prend actuellement en charge les opérations de copie et de collage de textes uniquement. Les fichiers ne peuvent pas être copiés ni collés vers et depuis le client web. En outre, vous pouvez uniquement utiliser **Ctrl + C** et **Ctrl + V** pour copier et coller du texte.

## <a name="get-help-with-the-web-client"></a>Obtenir de l’aide concernant le client web

Si vous avez rencontré un problème qui ne peut pas être résolu en utilisant les informations de cet article, vous pouvez obtenir de l’aide concernant le client web en envoyant un e-mail à l’adresse indiquée sur la page « À propos de » du client web.