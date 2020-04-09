---
title: Flux d’intégrité ReFS
author: gawatu
ms.author: jgerend
manager: dmoss
ms.date: 10/16/2018
ms.topic: article
ms.prod: windows-server
ms.technology: storage
ms.assetid: 1f1215cd-404f-42f2-b55f-3888294d8a1f
ms.openlocfilehash: 5e4ce1870d8aea01de0ab621d7efe197026643db
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861332"
---
# <a name="refs-integrity-streams"></a>Flux d’intégrité ReFS
>S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server (canal semi-annuel), Windows 10

La fonctionnalité Flux d’intégrité est facultative dans ReFS. Elle valide et maintient l’intégrité des données à l’aide de sommes de contrôle. ReFS utilise toujours des sommes de contrôle pour les métadonnées, mais, par défaut, ReFS ne génère pas et ne valide pas de sommes de contrôle pour les données de fichier. La fonctionnalité facultative Flux d’intégrité permet aux utilisateurs d’utiliser les sommes de contrôle pour les données de fichier. Lorsque les flux d’intégrité sont activés, ReFS peut déterminer clairement si les données sont valides ou endommagées. De plus, l’utilisation conjointe de ReFS et des espaces de stockage permet de corriger automatiquement les données et métadonnées endommagées.

## <a name="how-it-works"></a>Fonctionnement 

Les flux d’intégrité peuvent être activés pour des fichiers ou des répertoires spécifiques, ou pour l’intégralité du volume. Les paramètres des flux d’intégrité peuvent être activés et désactivés à tout moment. Par ailleurs, les paramètres des flux d’intégrité pour les fichiers et les répertoires sont hérités des répertoires parents. 

Une fois que les flux d’intégrité sont activés, ReFS crée et gère une somme de contrôle pour les fichiers spécifiés dans les métadonnées de ces fichiers. Cette somme de contrôle permet à ReFS de valider l’intégrité des données avant d’y accéder. Avant de retourner des données pour lesquelles le flux d’intégrité est activé, ReFS commence par calculer leur somme de contrôle :

![Calculer la somme de contrôle pour les données de fichier](media/compute-checksum.gif)

Cette somme de contrôle est alors comparée à la somme de contrôle contenue dans les métadonnées de fichier. Si les sommes de contrôle correspondent, les données sont marquées comme valides et retournées à l’utilisateur. Si les sommes de contrôle ne correspondent pas, les données sont endommagées. La résilience du volume détermine comment ReFS répond aux altérations :

- Si ReFS est monté dans un espace simple non résilient ou un lecteur nu, ReFS retourne une erreur à l’utilisateur sans retourner les données endommagées. 
- Si ReFS est monté dans un miroir résilient ou un espace de parité, il tente de corriger l’altération. 
    - Si la tentative réussit, ReFS applique une écriture corrective pour restaurer l’intégrité des données, puis retourne les données valides à l’application. Les altérations ne sont pas connues de l’application.
    - Si la tentative échoue, ReFS retourne une erreur. 

ReFS enregistre toutes les altérations dans le journal des événements système, lequel indique si les altérations ont été corrigées. 

![L’écriture corrective rétablit l’intégrité des données](media/corrective-write.gif)

## <a name="performance"></a>Performances 

Si le flux d’intégrité fournit une meilleure intégrité des données pour le système, il entraîne également une baisse des performances. Cette baisse des performances s’explique pour deux raisons :
- Si les flux d’intégrité sont activés, toutes les opérations d’écriture deviennent des opérations d’allocation à l’écriture. Même si cela évite des goulots d’étranglement en lecture, modification et écriture puisque ReFS n’a pas besoin de lire ou de modifier les données existantes, les données de fichier sont fréquemment fragmentées, ce qui retarde les lectures. 
- En fonction de la charge de travail et du stockage sous-jacent du système, le coût du calcul et de la validation de la somme de contrôle peut provoquer une augmentation de la latence d’E/S. 

Comme la baisse des performances est inhérente aux flux d’intégrité, nous vous recommandons de les laisser désactivés sur les systèmes sensibles aux performances. 

## <a name="integrity-scrubber"></a>Programme de nettoyage d'intégrité

Comme expliqué précédemment, ReFS valide automatiquement l’intégrité des données avant d’y accéder. ReFS utilise également un programme de nettoyage en arrière-plan, ce qui permet à ReFS de valider les données rarement utilisées. Ce programme de nettoyage analyse régulièrement le volume, identifie les endommagements latents et déclenche de manière proactive la réparation des données endommagées.

  >[!NOTE]
  >Le programme de nettoyage d'intégrité des données peut valider uniquement les données des fichiers dans lesquels les flux d’intégrité sont activés.

Par défaut, le programme de nettoyage s’exécute toutes les quatre semaines, mais cet intervalle peut être configuré dans le Planificateur de tâches sous Microsoft\Windows\Data Integrity Scan. 

## <a name="examples"></a>Exemples
Pour surveiller et modifier les paramètres d’intégrité des données de fichier, ReFS utilise les applets de commande **Get-FileIntegrity** et **Set-FileIntegrity**.

### <a name="get-fileintegrity"></a>Get-FileIntegrity
Pour savoir si les flux d’intégrité sont activés pour les données de fichier, utilisez l’applet de commande **Get-FileIntegrity**. 

```PowerShell
PS C:\> Get-FileIntegrity -FileName 'C:\Docs\TextDocument.txt'
```

Vous pouvez également utiliser l’applet de commande **Get-Item** pour obtenir les paramètres de flux d’intégrité pour tous les fichiers d’un répertoire spécifique. 

```PowerShell
PS C:\> Get-Item -Path 'C:\Docs\*' | Get-FileIntegrity
```

### <a name="set-fileintegrity"></a>Set-FileIntegrity
Pour activer/désactiver les flux d’intégrité pour les données de fichier, utilisez l’applet de commande **Get-FileIntegrity**. 

```PowerShell
PS C:\> Set-FileIntegrity -FileName 'H:\Docs\TextDocument.txt' -Enable $True
```

Vous pouvez également utiliser l’applet de commande **Get-Item** pour définir les paramètres de flux d’intégrité pour tous les fichiers d’un dossier spécifique. 

```PowerShell
PS C:\> Get-Item -Path 'H\Docs\*' | Set-FileIntegrity -Enable $True 
```

L’applet de commande **Set-FileIntegrity** peut également être utilisée directement dans des volumes et des répertoires. 

```PowerShell
PS C:\> Set-FileIntegrity H:\ -Enable $True
PS C:\> Set-FileIntegrity H:\Docs -Enable $True
```

## <a name="see-also"></a>Voir aussi

-   [Vue d’ensemble des ReFS](refs-overview.md)
-   [Clonage de bloc ReFS](block-cloning.md)
-   [Présentation de espaces de stockage direct](../storage-spaces/storage-spaces-direct-overview.md)
