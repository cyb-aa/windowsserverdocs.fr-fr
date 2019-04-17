---
title: "Actions de Service de contrôle d’intégrité"
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: efdf8f04e68fcbdc7051e78d6725cb919e740ffa
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="health-service-actions"></a>Actions de Service de contrôle d’intégrité

> S’applique à Windows Server2016

Le Service de contrôle d’intégrité est une nouvelle fonctionnalité de Windows Server 2016 qui améliore l’analyse quotidienne et expérience opérationnel pour les clusters exécutant espaces de stockage Direct.

## <a name="actions"></a>Actions  

La section suivante décrit le flux de travail automatisés par le Service de contrôle d’intégrité. Pour vérifier qu’une action est en effet entreprise de façon autonome, ou pour suivre sa progression ou son résultat, le Service de contrôle d’intégrité génère des «Actions». Contrairement aux journaux, les Actions disparaissent peu de temps après qu’ils ont terminé et sont conçues principalement pour fournir des informations sur l’activité en continu qui peut affecter les performances ou la capacité (par exemple, restauration de la résilience ou rééquilibrage des données).  

### <a name="usage"></a>Utilisation  

Une nouvelle applet de commande PowerShell affiche toutes les Actions:  

```PowerShell
Get-StorageHealthAction  
```

### <a name="coverage"></a>Couverture  

Dans Windows Server 2016, la **Get-StorageHealthAction** applet de commande permet de retourner les informations suivantes:  

-   Mise hors service ayant échouée, perte de connectivité ou absence de réponse disque physique  

-   Changement de pool de stockage à utiliser le disque de remplacement  

-   Restauration de la résilience complète aux données  

-   Rééquilibrage de pool de stockage  

## <a name="see-also"></a>Voir aussi

- [Service de contrôle d’intégrité dans Windows Server 2016](health-service-overview.md)
- [Référence de l’API sur MSDN, exemples de code et documentation pour développeurs](https://msdn.microsoft.com/windowshealthservice)
