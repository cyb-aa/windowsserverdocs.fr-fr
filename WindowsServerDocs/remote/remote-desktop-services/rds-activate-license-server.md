---
title: Activer le serveur de licences des Services Bureau à distance
description: Installer et activer le serveur de licences des Services Bureau à distance
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: eb24ddd2-0361-41fe-bd6b-c7c63427cb71
author: lizap
ms.author: elizapo
ms.date: 09/20/2016
manager: dongill
ms.openlocfilehash: 3eaa999c03c97ad3188d4dcd8514b2705bf0a3b1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852972"
---
# <a name="activate-the-remote-desktop-services-license-server"></a>Activer le serveur de licences des Services Bureau à distance

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Le serveur de licences des services Bureau à distance délivre des licences d'accès client (CAL) aux utilisateurs et aux périphériques lorsqu'ils accèdent à l'hôte de session de Bureau à distance. Vous pouvez activer le serveur de licences à l’aide du Gestionnaire de licences des services Bureau à distance. 

## <a name="install-the-rd-licensing-role"></a>Installer le rôle Gestionnaire de licences des services Bureau à distance

1. Utilisez un compte d’administrateur pour vous connecter au serveur que vous souhaitez utiliser en tant que serveur de licences.
2. Dans le Gestionnaire de serveur, cliquez sur **Résumé des rôles**, puis sur **Ajouter des rôles**.
   Sur la première page de l’Assistant Rôles, cliquez sur **Suivant**.
3. Sélectionnez **Services Bureau à distance**, puis cliquez sur **Suivant**, puis, sur la page Services Bureau à distance, cliquez de nouveau sur **Suivant**.
4. Sélectionnez **Gestionnaire de licences des services Bureau à distance**, puis cliquez sur **Suivant**.
5. Configurez le domaine : sélectionner **Configurer une étendue de découverte pour ce serveur de licences**, cliquez sur **Ce domaine**, puis sur **Suivant**.
6. Cliquez sur **Installer**.

## <a name="activate-the-license-server"></a>Activer le serveur de licences

1. Ouvrez le Gestionnaire de licences des services Bureau à distance : cliquez sur **Démarrer > Outils d'administration > Services Bureau à distance > Gestionnaire de licences des services Bureau à distance**.
2. Faites un clic droit sur le serveur de licences, puis cliquez sur **Activer le serveur**.
3. Dans la page d’accueil, cliquez sur **Suivant**.
4. Pour la méthode de connexion, sélectionnez **Connexion automatique (recommandé)** , puis cliquez sur **Suivant**.
5. Entrez les informations relatives à votre entreprise (votre nom, le nom de l’entreprise, votre région), puis cliquez sur **Suivant**.
6. Si nécessaire, entrez toute autre information sur l'entreprise (par exemple, son adresse e-mail et son adresse physique), puis cliquez sur **Suivant**. 
7. Vérifiez que l’option **Démarrer l’Assistant Installation de licences** n’est pas sélectionnée (nous installerons les lienses lors d’une étape ultérieure), puis cliquez sur **Suivant**.

Votre serveur de licences est maintenant prêt à commencer l’émission et la gestion de vos licences. 