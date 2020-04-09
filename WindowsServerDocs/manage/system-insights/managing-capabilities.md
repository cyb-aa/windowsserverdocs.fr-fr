---
title: Gestion des fonctionnalités
description: System Insights expose différents paramètres qui peuvent être configurés pour chaque fonctionnalité, et ces paramètres peuvent être réglés pour répondre aux besoins spécifiques de votre déploiement. Cette rubrique explique comment gérer les différents paramètres de chaque fonctionnalité via le centre d’administration Windows ou PowerShell, en fournissant des exemples PowerShell de base et des captures d’écran du centre d’administration Windows pour illustrer comment ajuster ces paramètres.
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 6/05/2018
ms.openlocfilehash: b93365474e591ce6fde59867c42b851ec45de50c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819732"
---
# <a name="managing-capabilities"></a>Gestion des fonctionnalités

>S'applique à : Windows Server 2019

Dans Windows Server 2019, System Insights expose différents paramètres qui peuvent être configurés pour chaque fonctionnalité, et ces paramètres peuvent être réglés pour répondre aux besoins spécifiques de votre déploiement. Cette rubrique explique comment gérer les différents paramètres de chaque fonctionnalité via le centre d’administration Windows ou PowerShell, en fournissant des exemples PowerShell de base et des captures d’écran du centre d’administration Windows pour illustrer comment ajuster ces paramètres. 

>[!TIP]
>Vous pouvez également utiliser ces courtes vidéos pour vous aider à prendre en main et à gérer en toute confiance System Insights : [prise en main de System Insights en 10 minutes](https://blogs.technet.microsoft.com/filecab/2018/07/24/getting-started-with-system-insights-in-10-minutes/)

Bien que cette section fournisse des exemples PowerShell, vous pouvez utiliser la [documentation PowerShell de System Insights](https://aka.ms/systeminsightspowershell) pour voir toutes les applets de commande, tous les paramètres et tous les jeux de paramètres dans System Insights. 

## <a name="viewing-capabilities"></a>Affichage des fonctionnalités

Pour commencer, vous pouvez répertorier toutes les fonctionnalités disponibles à l’aide de l’applet de commande **InsightsCapability** : 

```PowerShell
Get-InsightsCapability
``` 
Ces fonctionnalités sont également visibles dans l’extension System Insights :

![Page vue d’ensemble de System Insights répertoriant les fonctionnalités disponibles](media/overview-page-contoso.png)

## <a name="enabling-and-disabling-a-capability"></a>Activation et désactivation d’une fonctionnalité
Chaque fonctionnalité peut être activée ou désactivée. La désactivation d’une fonctionnalité empêche l’appel de cette fonctionnalité et, pour les fonctionnalités autres que celles par défaut, la désactivation d’une fonctionnalité arrête toutes les collectes de données pour cette fonctionnalité. Par défaut, toutes les fonctionnalités sont activées, et vous pouvez vérifier l’état d’une fonctionnalité à l’aide de l’applet de commande **InsightsCapability** . 

Pour activer ou désactiver une fonctionnalité, utilisez les applets de commande **Enable-InsightsCapability** et **Disable-InsightsCapability** :

```PowerShell
Enable-InsightsCapability -Name "CPU capacity forecasting"
Disable-InsightsCapability -Name "Networking capacity forecasting"
``` 
Ces paramètres peuvent également être activés ou désactivés en sélectionnant une fonctionnalité dans le centre d’administration Windows, en cliquant sur les boutons **activer** ou **Désactiver** .

### <a name="invoking-a-capability"></a>Appel d’une fonctionnalité
L’appel d’une fonctionnalité permet d’exécuter immédiatement la fonctionnalité de récupération d’une prédiction, et les administrateurs peuvent appeler une fonctionnalité à tout moment en cliquant sur le bouton **appeler** dans le centre d’administration Windows ou en utilisant l’applet de commande **Invoke-InsightsCapability** :

```PowerShell
Invoke-InsightsCapability -Name "CPU capacity forecasting"
```

>[!TIP]
>Pour vous assurer que l’appel d’une fonctionnalité n’est pas en conflit avec les opérations critiques sur votre ordinateur, envisagez de planifier des prédictions pendant les heures creuses.

## <a name="retrieving-capability-results"></a>Récupération des résultats des fonctionnalités
Une fois qu’une fonctionnalité a été appelée, les résultats les plus récents sont visibles à l’aide de la méthode **InsightsCapability** ou de la méthode **obtenir-InsightsCapabilityResult**. Ces applets de commande génèrent la dernière **Description** de l' **État et de** l’état de chaque fonctionnalité, qui décrit le résultat de chaque prédiction. Les champs **État** et **Description** de l’État sont décrits plus en détail dans le [document présentation des fonctionnalités](understanding-capabilities.md). 

En outre, vous pouvez utiliser l’applet de commande **InsightsCapabilityResult** pour afficher les 30 derniers résultats de prédiction et récupérer les données associées à la prédiction : 

```PowerShell
# Specify the History parameter to see the last 30 prediction results.
Get-InsightsCapabilityResult -Name "CPU capacity forecasting" -History

# Use the Output field to locate and then show the results of "CPU capacity forecasting."
# Specify the encoding as UTF8, so that Get-Content correctly parses non-English characters.
$Output = Get-Content (Get-InsightsCapabilityResult -Name "CPU capacity forecasting").Output -Encoding UTF8 | ConvertFrom-Json
$Output.ForecastingResults
```
L’extension System Insights affiche automatiquement l’historique de prédiction et analyse les résultats du résultat JSON, ce qui vous donne un graphe intuitif et haute fidélité de chaque prévision :

![Page de capacité unique présentant un graphique de prévision et l’historique de prédiction](media/cpu-forecast-2.png)

### <a name="using-the-event-log-to-retrieve-capability-results"></a>Utilisation du journal des événements pour récupérer les résultats des fonctionnalités
System Insights enregistre un événement chaque fois qu’une fonctionnalité termine une prédiction. Ces événements sont visibles dans le canal **Microsoft-Windows-System-Insights/admin** , et System Insights publie un ID d’événement différent pour chaque État :   

| État de prédiction | ID d’événement |
| --------------- | --------------- |
| OK | 151 |
| Avertissement | 148 |
| Critique | 150 |
| Error | 149 |
| Aucune | 132 |

>[!TIP]
>Utilisez [Azure Monitor](https://azure.microsoft.com/services/monitor/) ou [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807) pour agréger ces événements et voir les résultats de prédiction sur un groupe d’ordinateurs.


## <a name="setting-a-capability-schedule"></a>Définition d’une planification des capacités
Outre les prédictions à la demande, vous pouvez configurer des prédictions périodiques pour chaque fonctionnalité afin que la fonction spécifiée soit automatiquement appelée selon une planification prédéfinie. Utilisez l’applet de commande **InsightsCapabilitySchedule** pour afficher les planifications de fonctionnalités : 

>[!TIP]
>Utilisez l’opérateur de pipeline dans PowerShell pour afficher des informations sur toutes les fonctionnalités retournées par l’applet de commande **InsightsCapability** .

```PowerShell
Get-InsightsCapability | Get-InsightsCapabilitySchedule
```

Les prédictions périodiques sont activées par défaut, bien qu’elles puissent être désactivées à tout moment à l’aide des applets de commande **Enable-InsightsCapabilitySchedule** et **Disable-InsightsCapabilitySchedule** :

```PowerShell
Enable-InsightsCapabilitySchedule -Name "Total storage consumption forecasting"
Disable-InsightsCapabilitySchedule -Name "Volume consumption forecasting"
```

Chaque fonction par défaut est planifiée pour s’exécuter tous les jours à 3:00. Toutefois, vous pouvez créer des planifications personnalisées pour chaque fonctionnalité, et System Insights prend en charge un large éventail de types de planifications, qui peuvent être configurés à l’aide de l’applet de commande **Set-InsightsCapabilitySchedule** : 

```PowerShell
Set-InsightsCapabilitySchedule -Name "CPU capacity forecasting" -Daily -DaysInterval 2 -At 4:00PM
Set-InsightsCapabilitySchedule -Name "Networking capacity forecasting" -Daily -DaysOfWeek Saturday, Sunday -At 2:30AM
Set-InsightsCapabilitySchedule -Name "Total storage consumption forecasting" -Hourly -HoursInterval 2 -DaysOfWeek Monday, Wednesday, Friday
Set-InsightsCapabilitySchedule -Name "Volume consumption forecasting" -Minute -MinutesInterval 30 
```
>[!NOTE]
>Étant donné que les fonctionnalités par défaut analysent les données quotidiennes, il est recommandé d’utiliser des planifications quotidiennes pour ces fonctionnalités. En savoir plus sur les fonctionnalités par défaut [ici](understanding-capabilities.md).

Vous pouvez également utiliser le centre d’administration Windows pour afficher et définir des planifications pour chaque fonctionnalité en cliquant sur **paramètres**. La planification actuelle est affichée sous l’onglet **calendrier** , et vous pouvez utiliser les outils de l’interface graphique utilisateur pour créer une nouvelle planification :

![Page Paramètres présentant la planification actuelle](media/schedule-page-contoso.png)

## <a name="creating-remediation-actions"></a>Création d’actions correctives
System Insights vous permet de lancer des scripts de correction personnalisés en fonction du résultat d’une fonctionnalité. Pour chaque fonctionnalité, vous pouvez configurer un script PowerShell personnalisé pour chaque État de prédiction, ce qui permet aux administrateurs de prendre automatiquement des mesures correctives, plutôt que d’exiger une intervention manuelle. 

Les exemples d’actions de correction incluent l’exécution du nettoyage de disque, l’extension d’un volume, l’exécution de la déduplication, la migration dynamique des machines virtuelles et la configuration de Azure File Sync.

Vous pouvez voir les actions pour chaque fonctionnalité à l’aide de l’applet de commande **InsightsCapabilityAction** :

```PowerShell
Get-InsightsCapability | Get-InsightsCapabilityAction
```

Vous pouvez créer des actions ou supprimer des actions existantes à l’aide des applets de commande **Set-InsightsCapabilityAction** et **Remove-InsightsCapabilityAction** . Chaque action est exécutée à l’aide des informations d’identification spécifiées dans le paramètre **ActionCredential** .

>[!NOTE]
>Dans la version initiale de System Insights, vous devez spécifier des scripts de correction en dehors des répertoires utilisateur. Ce problème sera résolu dans une prochaine version.

```PowerShell
$Cred = Get-Credential
Set-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Warning -Action "C:\Users\Public\WarningScript.ps1" -ActionCredential $Cred
Set-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Critical -Action "C:\Users\Public\CriticalScript.ps1" -ActionCredential $Cred

Remove-InsightsCapabilityAction -Name "CPU capacity forecasting" -Type Warning
```

Vous pouvez également utiliser le centre d’administration Windows pour définir des actions de correction à l’aide de l’onglet **actions** dans la page **paramètres** :

![Page des paramètres dans laquelle l’utilisateur peut spécifier des actions de correction](media/actions-page-contoso.png)


## <a name="see-also"></a>Voir aussi
Pour en savoir plus sur System Insights, utilisez les ressources suivantes :

- [Vue d’ensemble de System Insights](overview.md)
- [Présentation des fonctionnalités](understanding-capabilities.md)
- [Ajout et développement de fonctionnalités](adding-and-developing-capabilities.md)
- [FAQ System Insights](faq.md)