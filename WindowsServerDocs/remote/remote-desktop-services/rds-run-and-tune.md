---
title: Services Bureau à distance - exécution et paramétrage
description: Fournit des informations de gestion pour les Services Bureau à distance.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 02/08/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 79909767-a4c3-4ecf-8d3f-77d37a663153
author: spatnaik
manager: scottman
ms.openlocfilehash: 40f8dbd560da359e8764ed715e7776cc2d230a7f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862180"
---
# <a name="run-and-tune-your-remote-desktop-services-environment"></a>Exécuter et optimiser votre environnement de Services Bureau à distance

Réglage de votre déploiement prend du temps et requiert instrumentation et surveillance. Utilisez les processus ci-dessous pour affiner votre déploiement de bureau à distance, de garantir son fonctionnement et de permettre l’évolutivité horizontale () en fonction des besoins. 

Il est recommandé d’évaluer régulièrement les mesures et le solde contre les frais de fonctionnement.

## <a name="management-and-monitoring"></a>Gestion et surveillance

Découvrez [gérer les utilisateurs dans votre collection de services Bureau à distance](rds-user-management.md) pour plus d’informations sur la façon de gérer l’accès à vos ordinateurs de bureau et les ressources distantes.

Utilisez **Microsoft Operations Management Suite (OMS)** pour surveiller les déploiements Bureau à distance pour les goulots d’étranglement potentiels et de les gérer à l’aide d’une des manières suivantes : 

- **Le Gestionnaire de serveur**: Utilisez l’outil de gestion de bureau à distance est intégré à Windows Server pour gérer les déploiements avec les utilisateurs finaux à distance simultanées jusqu'à 500. 
- **PowerShell**: Utiliser le module PowerShell de bureau à distance, également intégré à Windows Server, pour gérer les déploiements avec les utilisateurs finaux à distance simultanées jusqu'à 5 000.

## <a name="scale-bigger-better-faster"></a>Mise à l’échelle : Plus important, plus performant, plus rapidement

Avec une visibilité pour le déploiement, vous pouvez contrôler la mise à l’échelle avec davantage de précision. Facilement ajouter ou supprimer des serveurs hôtes de bureau à distance en fonction des besoins de mise à l’échelle. 

Les déploiements Bureau à distance qui reposent sur Azure peuvent rendre l’utilisation des services Azure, tel que SQL Azure, pour mettre à l’échelle automatiquement à la demande.

## <a name="automation-script-for-success"></a>Automation : Création de scripts

La gestion d’une application en cours d’exécution très évolutive implique des opérations répétées sur une base régulière. Utilisez les applets de commande PowerShell de Services Bureau à distance et les fournisseurs WMI pour développer des scripts qui peuvent être exécutés sur plusieurs déploiements au besoin. Exécuter les règles de Best Practice Analyzer (BPA) pour les Services Bureau à distance sur vos déploiements à paramétrer vos déploiements.

## <a name="load-testing-avoid-surprises"></a>Test de charge : Évitez les surprises

Test de charge le déploiement avec les tests de stress et de simulation de l’utilisation réelle. Faire varier la taille de charge pour éviter les surprises ! Assurez-vous que la réactivité répond aux besoins des utilisateurs, et que l’ensemble du système est résilient. Créer des tests de charge avec des outils de simulation, comme LoginVSI, qui vérifient la capacité de votre déploiement pour répondre aux besoins des utilisateurs. 