---
title: Mises à jour express pour Windows Server 2016 réactivées pour la mise à jour de novembre 2018
description: Fournit des informations sur les mises à jour express Windows Server 2016
ms.prod: windows-server
ms.technology: server-general
ms.topic: article
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.openlocfilehash: 57a6d13836a4f8c633afa6d37e7fa42e6d817deb
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822052"
---
# <a name="express-updates-for-windows-server-2016-re-enabled-for-november-2018-update"></a>Mises à jour express pour Windows Server 2016 réactivées pour la mise à jour de novembre 2018

> Par Joel Frauenheim
> 
> S'applique à : Windows Server 2016

À compter du 13 novembre 2018, Windows publiera à nouveau les mises à jour express pour Windows Server 2016. Les mises à jour express pour Windows Server 2016 se sont arrêtées mi-2017, alors qu’un problème important avait a été détecté, problème qui empêchait la bonne installation des mises à jour. Bien que le problème ait été résolu en novembre 2017, l’équipe de mise à jour a adopté une méthode plus prudente pour publier les packages express pour s’assurer que la plupart des clients disposeraient de la mise à jour le 14 novembre 2017 ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) sur leur environnement serveur et ne soient pas concernés par le problème.

Les administrateurs système de WSUS et Configuration Manager doivent savoir qu’en novembre 2018 ils verront à nouveau deux packages pour la mise à jour de Windows Server 2016 : une mise à jour complète et une mise à jour Express. Les administrateurs système qui souhaitent utiliser Express pour leurs environnements de serveur doivent vérifier que l’appareil a appliqué une mise à jour complète depuis le 14 novembre 2017 ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) pour garantir que la mise à jour Express s’installe correctement. Tout appareil qui n’a pas été mis à jour depuis la mise à jour du 14 novembre 2017 ([KB 4048953](https://support.microsoft.com/help/4048953/windows-10-update-kb4048953)) connaîtra des échecs répétés qui consomment de la bande passante et des ressources processeur dans une boucle infinie si la mise à jour Express est tentée.  La solution pour cet état serait pour l’administrateur système d’arrêter la mise à jour Express et d’envoyer une mise à jour complète récente pour arrêter cette boucle.

Avec la mise à jour express du 13 novembre 2018, les clients pourront constater une réduction immédiate de la taille du package entre leur système de gestion et les points de terminaison Windows Server 2016.  