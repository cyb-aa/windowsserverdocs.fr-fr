---
title: Actions de Service de contrôle d’intégrité
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: ''
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: efdf8f04e68fcbdc7051e78d6725cb919e740ffa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843020"
---
# <a name="health-service-actions"></a>Actions de Service de contrôle d’intégrité

> S’applique à Windows Server 2016

Le Service de contrôle d’intégrité est une nouvelle fonctionnalité de Windows Server 2016 qui améliore l’analyse quotidienne et une expérience opérationnelle pour les clusters exécutant des espaces de stockage Direct.

## <a name="actions"></a>Actions  

La section suivante décrit les flux de travail automatisés par le service de contrôle d’intégrité. Pour vérifier qu’une action est en effet entreprise de façon autonome, ou pour suivre sa progression ou son résultat, le service de contrôle d’intégrité génère des « actions ». Contrairement aux journaux, les actions disparaissent peu de temps après avoir été exécutées. Elles ont principalement vocation à donner une visibilité à l’activité continue susceptible d’affecter les performances ou la capacité (par exemple, une restauration de la résilience ou un rééquilibrage des données).  

### <a name="usage"></a>Utilisation  

Une nouvelle applet de commande PowerShell permet d’afficher toutes les actions :  

```PowerShell
Get-StorageHealthAction  
```

### <a name="coverage"></a>Couverture  

Dans Windows Server 2016, le **Get-StorageHealthAction** applet de commande permet de retourner les informations suivantes :  

-   Mise hors service ayant échoué, perte de connectivité ou absence de réponse d’un disque physique  

-   Changement de pool de stockage pour utiliser un disque physique de remplacement  

-   Restauration de la résilience complète dans les données  

-   Rééquilibrage de pool de stockage  

## <a name="see-also"></a>Voir aussi

- [Service d’intégrité de Windows Server 2016](health-service-overview.md)
- [Documentation du développeur, exemple de code et référence API sur MSDN](https://msdn.microsoft.com/windowshealthservice)
