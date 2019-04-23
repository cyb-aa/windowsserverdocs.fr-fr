---
title: Express mises à jour pour Windows Server 2016 est réactivée pour novembre 2018 mettre à jour
description: Fournit des informations sur les mises à jour Windows Server 2016 Express
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.openlocfilehash: c48979440ab7c5cfa86aa1287b354a1e43692f48
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867440"
---
# <a name="express-updates-for-windows-server-2016-re-enabled-for-november-2018-update"></a>Express mises à jour pour Windows Server 2016 est réactivée pour novembre 2018 mettre à jour

>Par Joel Frauenheim

>S'applique à : Windows Server 2016

En commençant avec 13 novembre 2018 mardi mise à jour, Windows sera à nouveau publier les mises à jour Express pour Windows Server 2016. Mises à jour rapides pour Windows Server 2016 s’est arrêté dans mi-année 2017 dès lors qu’un problème significatif a été trouvé qui conservé les mises à jour de s’installer correctement. Bien que le problème a été résolu en novembre 2017, l’équipe de mise à jour a eu une méthode plus classique pour publier les packages Express pour vous assurer de la plupart des clients aurait la mise à jour le 14 novembre 2017 ([Ko 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) installé sur leur serveur environnements et ne pas concernées par le problème.

Les administrateurs système pour WSUS et System Center Configuration Manager (SCCM) doivent être conscient qu’en novembre 2018 ils là aussi verront deux packages pour la mise à jour de Windows Server 2016 : une mise à jour complète et une mise à jour Express. Les administrateurs système qui souhaitent utiliser Express pour leurs environnements de serveur doivent vérifier que l’appareil a mis une mise à jour complète depuis le 14 novembre 2017 ([Ko 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) pour garantir la mise à jour Express s’installe correctement. N’importe quel appareil qui n’a pas été mis à jour depuis le 14 novembre 2017 mise à jour ([Ko 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) s’affiche à des échecs répétés qui consomment de la bande passante et des ressources de processeur dans une boucle infinie si la mise à jour Express est tentée.  Mise à jour pour cet état serait pour l’administrateur système pour arrêter l’envoi de la mise à jour Express et envoyer une mise à jour complète récente pour arrêter la boucle de défaillance.

Le 13 novembre 2018 Express mise à jour clients affichent une réduction immédiate de la taille du package entre leur système de gestion et les points de terminaison Windows Server 2016.  