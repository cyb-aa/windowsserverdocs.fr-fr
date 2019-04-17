---
title: Express des mises à jour pour Windows Server 2016 réactivé pour novembre 2018 mettre à jour
description: Fournit des informations sur les mises à jour rapides dans Windows Server 2016
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.openlocfilehash: c48979440ab7c5cfa86aa1287b354a1e43692f48
ms.sourcegitcommit: 23e0a68e21985d709e029e7771d3c52d6815bcb4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6507850"
---
# Express des mises à jour pour Windows Server 2016 réactivé pour novembre 2018 mettre à jour

>Par Joel Frauenheim

>S’applique à WindowsServer2016

Démarrer avec le 13 novembre 2018 mise à jour mardi, Windows sera à nouveau publier des mises à jour Express pour Windows Server 2016. Mises à jour Express pour Windows Server 2016 arrêté en milieu 2017 après qu’un problème majeur a été trouvé qui conservé les mises à jour de s’installer correctement. Alors que le problème a été résolu en novembre 2017, l’équipe de mise à jour a eu une approche classique pour les packages Express pour vous assurer que la plupart des clients serait Update 14 novembre 2017 ([Ko 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) est installé sur leur environnement de serveur et ne pas de publication concernés par le problème.

Les administrateurs de système pour WSUS et System Center Configuration Manager (SCCM) doivent à l’esprit qu’en novembre 2018 ils nouveau verront deux packages pour la mise à jour de Windows Server 2016: une mise à jour complète et une mise à jour Express. Les administrateurs système qui souhaitent utiliser Express pour leur environnement de serveur ont besoin de confirmer que l’appareil a effectué une mise à jour complet depuis le 14 novembre 2017 ([Ko 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) pour vous assurer de que la mise à jour Express s’installe correctement. N’importe quel appareil qui n’a pas été mis à jour dans la mesure où la mise à jour du 14 novembre 2017 ([Ko 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) verront répétées échecs de consommer la bande passante et les ressources de processeur dans une boucle infinie si la tentative de la mise à jour Express.  Mise à jour pour cet état serait pour l’administrateur de système d’arrêter la distribution de la mise à jour Express et push une mise à jour récente complète pour arrêter la boucle de défaillance.

13 novembre 2018 clients de mise à jour Express affichent une réduction immédiate de la taille de package entre leur système de gestion et les points de terminaison de Windows Server 2016.  