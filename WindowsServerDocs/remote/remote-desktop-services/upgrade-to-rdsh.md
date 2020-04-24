---
title: Mise à niveau de votre hôte de session Bureau à distance vers Windows Server 2016
description: Cet article explique comment mettre à niveau vos déploiements des services Bureau à distance existants vers Windows Server 2016.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 08/01/2016
ms.topic: article
ms.assetid: 5c9b98b8-4eca-4a39-b10b-2bac729f7f44
author: spatnaik
manager: scottman
ms.openlocfilehash: e685c51a003a7121dab19c74d82796311ef0889a
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80857122"
---
# <a name="upgrading-your-remote-desktop-session-host-to-windows-server-2016"></a>Mise à niveau de votre hôte de session Bureau à distance vers Windows Server 2016

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

> [!IMPORTANT]
> Toutes les applications doivent être désinstallées avant la mise à niveau, puis réinstallées après celle-ci pour éviter les problèmes de compatibilité d’application pouvant survenir en raison de la mise à niveau.

## <a name="supported-os-upgrades-with-rds-role-installed"></a>Mises à niveau des systèmes d’exploitation pris en charge avec le rôle Services Bureau à distance installé
Les mises à niveau vers Windows Server 2016 sont prises en charge uniquement à partir de Windows Server 2012 R2 et Windows Server 2016 TP5.

## <a name="upgrading-a-rds-session-based-collection"></a>Mise à niveau d’une collection basée sur une session des services Bureau à distance
Afin de réduire au maximum le temps d’arrêt, il est préférable de suivre les étapes ci-dessous lors de la mise à niveau d’une collection basée sur une session des services Bureau à distance :

1. Identifiez les serveurs à mettre à niveau, c’est-à-dire la moitié des serveurs dans la collection.
2. Empêchez les nouvelles connexions à ces serveurs en définissant **Autoriser les nouvelles connexions** sur « false ».
3. Fermez toutes les sessions sur ces serveurs. 
4. Supprimez ces serveurs de la collection.
5. Mettez à niveau les serveurs vers Windows Server 2016.
6. Sur les serveurs restants dans la collection, définissez **Autoriser les nouvelles connexions** sur « false ».
7. Ajoutez de nouveau les serveurs mis à niveau dans leurs collections correspondantes.
8. Supprimez le jeu restant de serveurs à mettre à niveau à partir de la collection.
9. Sur les serveurs mis à niveau dans la collection, définissez **Autoriser les nouvelles connexions** sur « true ».
10. À présent, mettez à niveau les serveurs restants dans le déploiement, en suivant les étapes de 3 à 9 ci-dessus.

## <a name="upgrading-a-standalone-rd-session-host-server"></a>Mise à niveau d’un serveur autonome Hôte de session Bureau à distance
Un serveur Hôte de session Bureau à distance autonome peut être mis à niveau à tout moment.