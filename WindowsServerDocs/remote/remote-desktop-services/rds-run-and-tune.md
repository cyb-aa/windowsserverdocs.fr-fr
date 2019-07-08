---
title: Services Bureau à distance - Exécution et optimisation
description: Fournit des informations sur la gestion des services Bureau à distance.
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
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "63712090"
---
# <a name="run-and-tune-your-remote-desktop-services-environment"></a>Exécuter et optimiser votre environnement de services Bureau à distance

L'optimisation de votre déploiement prend du temps et requiert instrumentation et surveillance. Utilisez les processus ci-dessous pour affiner votre déploiement Bureau à distance, préserver son fonctionnement et faciliter sa mise à l'échelle en fonction des besoins. 

Il convient d'évaluer en permanence les métriques et de trouver un équilibre par rapport aux coûts d'exploitation.

## <a name="management-and-monitoring"></a>Gestion et surveillance

Consultez [Gérer les utilisateurs de votre collection de services Bureau à distance](rds-user-management.md) pour en savoir plus sur la gestion des accès à vos appareils de bureau et à vos ressources distantes.

Utilisez **Microsoft Operations Management Suite (OMS)** pour surveiller les déploiements Bureau à distance afin de détecter les éventuels goulots d'étranglement et de les gérer de l'une des façons suivantes : 

- **Gestionnaire de serveur** : utilisez l'outil de gestion des services Bureau à distance intégré à Windows Server pour gérer les déploiements comprenant jusqu'à 500 utilisateurs finaux distants simultanés. 
- **PowerShell** : utilisez le module PowerShell Bureau à distance, également intégré à Windows Server, pour gérer les déploiements comprenant jusqu'à 5 000 utilisateurs finaux distants simultanés.

## <a name="scale-bigger-better-faster"></a>Des mises à l'échelle plus importantes, plus performantes et plus rapides

Grâce à la visibilité dont vous disposez sur le déploiement, vous pouvez contrôler les mises à l'échelle avec plus de précision. Ajoutez ou supprimez facilement des serveurs hôtes Bureau à distance en fonction des besoins de mise à l'échelle. 

Les déploiements Bureau à distance basés sur Azure peuvent utiliser des services Azure, tels qu'Azure SQL, pour une mise à l'échelle automatique et à la demande.

## <a name="automation-script-for-success"></a>Automatisation : le scénario du succès

Pour préserver le fonctionnement d'une application à grande échelle, un certain nombre d'opérations doivent être régulièrement répétées. Utilisez les applets de commande PowerShell des services Bureau à distance et les fournisseurs WMI pour développer des scripts exécutables sur plusieurs déploiements en cas de besoin. Exécutez les règles BPA (Best Practice Analyzer) des services Bureau à distance sur vos déploiements afin de les optimiser.

## <a name="load-testing-avoid-surprises"></a>Test de charge : éviter les surprises

Soumettez le déploiement à un test de charge en procédant à des tests de contrainte et à une simulation d'utilisation en conditions réelles. Faites varier la taille de charge pour éviter les surprises ! Assurez-vous que la réactivité répond aux besoins des utilisateurs et que l'ensemble du système est résilient. Créez des tests de charge avec des outils de simulation, tels que LoginVSI, permettant de vérifier si votre déploiement répond aux besoins des utilisateurs. 