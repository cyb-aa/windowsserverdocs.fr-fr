---
title: Activer le serveur de licences des Services Bureau à distance
description: Installer et activer le serveur de licences bureau à distance
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eb24ddd2-0361-41fe-bd6b-c7c63427cb71
author: lizap
ms.author: elizapo
ms.date: 09/20/2016
manager: dongill
ms.openlocfilehash: d28ceac9cde0ee2d4c92867bdd90d5c463a8cd4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888010"
---
# <a name="activate-the-remote-desktop-services-license-server"></a>Activer le serveur de licences des Services Bureau à distance

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Les Services Bureau à distance des licences d’accès serveur problèmes client (CAL) de licence aux utilisateurs et appareils, lorsqu’ils accèdent à l’hôte de Session Bureau à distance. Vous pouvez activer le serveur de licences à l’aide du Gestionnaire de licences bureau à distance. 

## <a name="install-the-rd-licensing-role"></a>Installer le rôle Gestionnaire de licences bureau à distance

1. Connectez-vous au serveur que vous souhaitez utiliser en tant que le serveur de licences à l’aide d’un compte d’administrateur.
2. Dans le Gestionnaire de serveur, cliquez sur **résumé des rôles**, puis cliquez sur **Ajout de rôles**.
   Cliquez sur **suivant** sur la première page de l’Assistant de rôles.
3. Sélectionnez **Services Bureau à distance**, puis cliquez sur **suivant**, puis **suivant** sur la page Services Bureau à distance.
4. Sélectionnez **licences distance**, puis cliquez sur **suivant**.
5. Configurer le domaine - sélectionner **configurer une étendue de découverte pour ce serveur de licences**, cliquez sur **ce domaine**, puis cliquez sur **suivant**.
6. Cliquez sur **Installer**.

## <a name="activate-the-license-server"></a>Activer le serveur de licences

1. Ouvrez le Gestionnaire de licences bureau à distance : cliquez sur **Démarrer > Outils d’administration > Services Bureau à distance > Gestionnaire de licences bureau à distance**.
2. Cliquez sur le serveur de licences, puis cliquez sur **activer le serveur**.
3. Cliquez sur **suivant** sur la page d’accueil.
4. Pour la méthode de connexion, sélectionnez **connexion automatique (recommandé)**, puis cliquez sur **suivant**.
5. Entrez les informations de votre société (votre nom, le nom de société, votre région géographique), puis cliquez sur **suivant**.
6. Entrez éventuellement d’autres informations d’entreprise (par exemple, les adresses de messagerie et d’entreprise), puis cliquez sur **suivant**. 
7. Assurez-vous que l’option **démarrer l’Assistant Installation de licences** n’est pas sélectionnée (nous allons installer les licences dans une étape ultérieure), puis cliquez sur **suivant**.

Votre serveur de licences est maintenant prêt à commencer l’émission et la gestion de licences. 