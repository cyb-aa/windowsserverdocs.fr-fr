---
title: MultiPoint services-tâches de postconnexion
description: Découvrez comment valider et fermer votre migration vers MultiPoint services
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1497cae0-071e-467d-89b8-a7050815d7de
author: lizap
manager: dongill
ms.openlocfilehash: 3102a442b4668856050f603f30f57f6bbed20654
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389020"
---
# <a name="multipoint-services---post-migration-tasks"></a>MultiPoint services-tâches de postconnexion

>S'applique à : Windows Server 2016

Après la migration vers MultiPoint services dans Windows Server 2016, utilisez les informations suivantes pour valider la migration et effectuer des étapes de nettoyage.

## <a name="validate-the-migration-by-running-a-pilot-program"></a>Valider la migration en exécutant un programme pilote

Vous pouvez valider votre migration MultiPoint services en créant un projet pilote dans l’environnement de production. Exécutez le projet pilote sur les serveurs avant de placer les services de rôle migrés en production pour vérifier que votre déploiement fonctionne comme prévu. Envisagez de limiter le nombre de connexions dans un premier temps, ce qui augmente lentement le nombre d’utilisateurs qui accèdent à MultiPoint services.

> [!NOTE] 
> Utilisez toujours les comptes de test pour tester la migration. Utilisez un compte avec des privilèges d’administrateur et un compte pour un utilisateur valide.

## <a name="retire-the-source-server"></a>Mettre le serveur source hors service
Une fois que vous avez validé votre migration, vous pouvez arrêter ou déconnecter le serveur source de votre réseau. Si le serveur est joint à un domaine, supprimez-le du domaine avant de le déconnecter.

