---
title: Gestion des fonctionnalités
description: Système Insights expose divers paramètres qui peuvent être configurés pour chaque fonctionnalité, et ces paramètres peuvent être ajustées à l’adresse des besoins de votre déploiement. Cette rubrique décrit comment gérer les différents paramètres pour chaque fonctionnalité par le biais de Windows Admin Center ou PowerShell, en fournissant des exemples PowerShell simples et des captures d’écran Windows Admin Center pour montrer comment ajuster ces paramètres.
ms.custom: na
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 6/05/2018
ms.openlocfilehash: 9081a0b576ab9871b47df38255047b6cbe889419
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868020"
---
# <a name="managing-capabilities"></a>Gestion des fonctionnalités

>S'applique à : Windows Server 2019

Dans Windows Server 2019, système Insights expose divers paramètres qui peuvent être configurés pour chaque fonctionnalité, et ces paramètres peuvent être ajustées à l’adresse des besoins de votre déploiement. Cette rubrique décrit comment gérer les différents paramètres pour chaque fonctionnalité par le biais de Windows Admin Center ou PowerShell, en fournissant des exemples PowerShell simples et des captures d’écran Windows Admin Center pour montrer comment ajuster ces paramètres. 

>[!TIP]
>Vous pouvez également utiliser ces courtes vidéos pour vous aider à commencer et gérer en toute confiance les informations système : [Bien démarrer avec les informations système dans 10 minutes](https://blogs.technet.microsoft.com/filecab/2018/07/24/getting-started-with-system-insights-in-10-minutes/)

Bien que cette section fournit des exemples de PowerShell, vous pouvez utiliser la [documentation système Insights PowerShell](https://aka.ms/systeminsightspowershell) pour voir tous les jeux d’applets de commande, des paramètres et des paramètres dans les informations système. 

## <a name="viewing-capabilities"></a>Les fonctionnalités de visualisation

Pour commencer, vous pouvez répertorier toutes les fonctionnalités disponibles à l’aide de la **Get-InsightsCapability** applet de commande : 

```PowerShell
Get-InsightsCapability
``` 
Ces fonctionnalités sont également visibles dans l’extension Insights du système :

![Page Vue d’ensemble des informations système répertoriant les fonctionnalités disponibles](media/overview-page-contoso.png)

## <a name="enabling-and-disabling-a-capability"></a>Activation et désactivation d’une fonctionnalité
Chaque fonctionnalité peut être activée ou désactivée. La désactivation d’une fonctionnalité empêche cette fonctionnalité ne soit appelée, et pour les fonctionnalités non définis par défaut, la désactivation d’une fonctionnalité arrête la collecte de données tous les pour cette fonctionnalité. Par défaut, toutes les fonctionnalités sont activées, et vous pouvez vérifier l’état d’une fonctionnalité en utilisant le **Get-InsightsCapability** applet de commande. 

Pour activer ou désactiver une fonctionnalité, utilisez le **Enable-InsightsCapability** et **Disable-InsightsCapability** applets de commande :

```PowerShell
Enable-InsightsCapability -Name "CPU capacity forecasting"
Disable-InsightsCapability -Name "Networking capacity forecasting"
``` 
Ces paramètres peuvent également être activées par sélectionné une fonctionnalité dans Windows Admin Center en cliquant sur le **activer** ou **désactiver** boutons.

### <a name="invoking-a-capability"></a>Appel d’une fonctionnalité
Appel d’une fonctionnalité immédiatement s’exécute la fonction de récupération d’une prédiction, et les administrateurs peuvent appeler une fonctionnalité de tout moment en cliquant sur le **Invoke** bouton dans Windows Admin Center ou en utilisant la  **InsightsCapability appeler** applet de commande :

```PowerShell
Invoke-InsightsCapability -Name "CPU capacity forecasting"
```

>[!TIP]
>Pour vous assurer d’appeler une fonction n’entre en conflit avec les opérations critiques sur votre ordinateur, pensez à planifier des prédictions pendant hors des heures d’ouverture.

## <a name="retrieving-capability-results"></a>Récupération des résultats de la fonctionnalité
Une fois qu’une fonctionnalité a été appelée, les résultats les plus récents sont visibles à l’aide de **Get-InsightsCapability** ou **Get-InsightsCapabilityResult**. Ces applets de commande de sortie la plus récente **état** et **Description de l’état** de chaque fonctionnalité, qui décrivent le résultat de chaque prédiction. Le **état** et **Description de l’état** champs sont décrites en détail dans le [document de fonctionnalités de compréhension](understanding-capabilities.md). 

En outre, vous pouvez utiliser la **Get-InsightsCapabilityResult** applet de commande pour afficher les derniers résultats de 30 prédiction et récupérer les données associées à la prédiction : 

```PowerShell
# Specify the History parameter to see the last 30 prediction results.
Get-InsightsCapabilityResult -Name "CPU capacity forecasting" -History

# Use the Output field to locate and then show the results of "CPU capacity forecasting."
# Specify the encoding as UTF8, so that Get-Content correctly parses non-English characters.
$Output = Get-Content (Get-InsightsCapabilityResult -Name "CPU capacity forecasting").Output -Encoding UTF8 | ConvertFrom-Json
$Output.ForecastingResults
```
L’extension système Insights affiche l’historique de prédiction automatiquement et analyse les résultats du résultat JSON, ce qui vous donne un graphique intuitif, haute fidélité de chaque prévision :

![Page de fonctionnalité unique qui affiche un graphique de prévision et l’historique de prédiction](media/cpu-forecast-2.png)

### <a name="using-the-event-log-to-retrieve-capability-results"></a>À l’aide du journal des événements pour récupérer les résultats de la fonctionnalité
Les informations système consigne un événement chaque fois qu’une fonction termine une prédiction. Ces événements sont visibles dans le **Microsoft-Windows-System-Insights/Admin** channel et système Insights publie un ID différent pour chaque état :   

| État de prédiction | ID d’événement |
| --------------- | --------------- |
| OK | 151 |
| Warning | 148 |
| Critique | 150 |
| Erreur | 149 |
| Aucune | 132 |

>[!TIP]
>Utilisez [Azure Monitor](https://azure.microsoft.com/services/monitor/) ou [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807) agréger ces événements et les résultats de prédiction sur un groupe de machines.


## <a name="setting-a-capability-schedule"></a>Définition d’une planification de capacité
En plus des prédictions à la demande, vous pouvez configurer des prévisions périodiques pour chaque fonctionnalité afin que la fonctionnalité spécifiée est appelée automatiquement selon une planification prédéfinie. Utilisez le **Get-InsightsCapabilitySchedule** applet de commande pour afficher les planifications de fonctionnalité : 

>[!TIP]
>Utilisez l’opérateur de pipeline dans PowerShell pour afficher des informations pour toutes les fonctionnalités retournées par la **Get-InsightsCapability** applet de commande.

```PowerShell
Get-InsightsCapability | Get-InsightsCapabilitySchedule
```

Des prévisions périodiques sont activées par défaut, bien que vous pouvez les désactiver à tout moment le **Enable-InsightsCapabilitySchedule** et **Disable-InsightsCapabilitySchedule** applets de commande :

```PowerShell
Enable-InsightsCapabilitySchedule -Name "Total storage consumption forecasting"
Disable-InsightsCapabilitySchedule -Name "Volume consumption forecasting"
```

Chaque fonctionnalité par défaut est planifiée pour s’exécuter tous les jours à 3 h 00. Vous pouvez, cependant, créer des planifications personnalisées pour chaque fonctionnalité, et les informations système prend en charge divers types de planification, qui peuvent être configurés à l’aide de la **Set-InsightsCapabilitySchedule** applet de commande : 

```PowerShell
Set-InsightsCapabilitySchedule -Name "CPU capacity forecasting" -Daily -DaysInterval 2 -At 4:00PM
Set-InsightsCapabilitySchedule -Name "Networking capacity forecasting" -Daily -DaysOfWeek Saturday, Sunday -At 2:30AM
Set-InsightsCapabilitySchedule -Name "Total storage consumption forecasting" -Hourly -HoursInterval 2 -DaysOfWeek Monday, Wednesday, Friday
Set-InsightsCapabilitySchedule -Name "Volume consumption forecasting" -Minute -MinutesInterval 30 
```
>[!NOTE]
>Étant donné que les fonctions par défaut analyser les données quotidiennes, il est recommandé d’utiliser des planifications quotidiennes pour ces fonctionnalités. En savoir plus sur les fonctions par défaut [ici](understanding-capabilities.md).

Vous pouvez également utiliser Windows Admin Center pour afficher et définir des planifications pour chaque fonctionnalité en cliquant sur **paramètres**. La planification actuelle est affichée sur le **planification** onglet et vous pouvez utiliser les outils de GUI pour créer une planification :

![Planification de la page Paramètres montrant actuelle](media/schedule-page-contoso.png)

## <a name="creating-remediation-actions"></a>Création d’actions de mise à jour
Système Insights vous permet de lancer des scripts de correction personnalisés en fonction du résultat d’une capacité. Pour chaque fonctionnalité, vous pouvez configurer un script PowerShell personnalisé pour chaque état de la prédiction, permettant aux administrateurs corriger automatiquement, au lieu d’intervention manuelle. 

Actions de mise à jour de modèles incluent le nettoyage de disque en cours d’exécution, l’extension d’un volume, exécute la déduplication, migrer des machines virtuelles et la configuration Azure File Sync.

Vous pouvez voir les actions pour chaque fonctionnalité en utilisant le **Get-InsightsCapabilityAction** applet de commande :

```PowerShell
Get-InsightsCapability | Get-InsightsCapabilityAction
```

Vous pouvez créer des actions ou supprimer des actions existantes à l’aide de la **Set-InsightsCapabilityAction** et **Remove-InsightsCapabilityAction** applets de commande. Chaque action est exécutée à l’aide des informations d’identification qui sont spécifiées dans le **ActionCredential** paramètre.

>[!NOTE]
>Dans la première version de système Insights, vous devez spécifier les scripts de mise à jour en dehors de répertoires de l’utilisateur. Ce problème sera résolu dans une prochaine version.

```PowerShell
$Cred = Get-Credential
Set-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Warning -Action "C:\Users\Public\WarningScript.ps1" -ActionCredential $Cred
Set-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Critical -Action "C:\Users\Public\CriticalScript.ps1" -ActionCredential $Cred

Remove-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Warning
```

Vous pouvez également utiliser Windows Admin Center pour définir des actions de mise à jour à l’aide de la **Actions** onglet dans le **paramètres** page :

![Page Paramètres où utilisateur peut spécifier des actions de correction](media/actions-page-contoso.png)


## <a name="see-also"></a>Voir aussi
Pour en savoir plus sur les informations système, utilisez les ressources suivantes :

- [Vue d’ensemble des informations système](overview.md)
- [Fonctionnalités de présentation](understanding-capabilities.md)
- [Ajout et le développement de fonctionnalités](adding-and-developing-capabilities.md)
- [Système Insights Forum aux questions](faq.md)