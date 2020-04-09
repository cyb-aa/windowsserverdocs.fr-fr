---
title: Actions Service de contrôle d’intégrité
ms.prod: windows-server
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: a4ef08a4ca552211b64d11677153775d6b18b4fa
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80827622"
---
# <a name="health-service-actions"></a>Actions Service de contrôle d’intégrité

> S’applique à : Windows Server 2019, Windows Server 2016

Le Service de contrôle d’intégrité est une nouvelle fonctionnalité de Windows Server 2016 qui améliore la surveillance quotidienne et l’expérience opérationnelle pour les clusters exécutant espaces de stockage direct.

## <a name="actions"></a>Actions  

La section suivante décrit les flux de travail automatisés par le service de contrôle d’intégrité. Pour vérifier qu’une action est en effet entreprise de façon autonome, ou pour suivre sa progression ou son résultat, le service de contrôle d’intégrité génère des « actions ». Contrairement aux journaux, les actions disparaissent peu de temps après avoir été exécutées. Elles ont principalement vocation à donner une visibilité à l’activité continue susceptible d’affecter les performances ou la capacité (par exemple, une restauration de la résilience ou un rééquilibrage des données).  

### <a name="usage"></a>Utilisation  

Une nouvelle applet de commande PowerShell permet d’afficher toutes les actions :  

```PowerShell
Get-StorageHealthAction  
```

### <a name="coverage"></a>Couverture  

Dans Windows Server 2016, l’applet de commande **StorageHealthAction** peut retourner les informations suivantes :  

-   Mise hors service ayant échoué, perte de connectivité ou absence de réponse d’un disque physique  

-   Changement de pool de stockage pour utiliser un disque physique de remplacement  

-   Restauration de la résilience complète dans les données  

-   Rééquilibrage de pool de stockage  

## <a name="see-also"></a>Voir aussi

- [Service de contrôle d’intégrité dans Windows Server 2016](health-service-overview.md)
- [Documentation pour développeurs, exemples de code et informations de référence sur les API sur MSDN](https://msdn.microsoft.com/windowshealthservice)
