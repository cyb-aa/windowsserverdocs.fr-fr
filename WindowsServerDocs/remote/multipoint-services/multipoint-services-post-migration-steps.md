---
title: MultiPoint Services - tâches de post-migration
description: Découvrez comment valider et fermer votre migration à MultiPoint Services
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1497cae0-071e-467d-89b8-a7050815d7de
author: lizap
manager: dongill
ms.openlocfilehash: e3fa3c812355a14289ea4eeff3ab1e7e92e00d97
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863500"
---
# <a name="multipoint-services---post-migration-tasks"></a>MultiPoint Services - tâches de post-migration

>S'applique à : Windows Server 2016

Après la migration vers Services MultiPoint dans Windows Server 2016, utilisez les informations suivantes pour valider la migration et effectuer les étapes de nettoyage.

## <a name="validate-the-migration-by-running-a-pilot-program"></a>Valider la migration en exécutant un programme pilote

Vous pouvez valider votre migration MultiPoint Services en créant un projet pilote dans l’environnement de production. Exécutez le projet pilote sur les serveurs avant de mettre les services de rôle migrés en production pour vérifier que votre déploiement fonctionne comme prévu. Envisagez de limiter le nombre de connexions dans un premier temps, augmenter lentement le nombre d’utilisateurs qui accèdent à MultiPoint Services.

> [!NOTE] 
> Toujours utiliser des comptes de test pour tester la migration. Utiliser un compte avec des privilèges d’administrateur et un compte pour un utilisateur valide.

## <a name="retire-the-source-server"></a>Mettre le serveur source hors service
Après avoir validé votre migration, vous pouvez arrêter ou se déconnecter du serveur source à partir de votre réseau. Si le serveur a été joint au domaine, supprimez-le du domaine avant de le déconnecter.

