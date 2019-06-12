---
title: Mettre à jour de Delta mensuel prise en charge de l’éditeur de logiciels indépendant sans WSUS
description: Rubrique de Windows Server Update Service (WSUS) - fournisseurs de logiciels comment indépendant (ISV) permettre utiliser temporairement mise à jour mensuelle Delta au lieu de la remise de mise à jour WSUS Express afin de réduire la taille du package
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: dougkim
ms.date: 10/16/2017
ms.openlocfilehash: 272d3865bbe1a9853f5349c5e878155351525ef0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439943"
---
# <a name="monthly-delta-update-isv-support-without-wsus"></a>Mettre à jour de Delta mensuel prise en charge de l’éditeur de logiciels indépendant sans WSUS

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10

Téléchargements de la mise à jour de Windows 10 peuvent être volumineux, car chaque package contient tous les correctifs précédemment publiés pour garantir la cohérence et la simplicité.  

Depuis la version 7, Windows a été en mesure de réduire la taille des téléchargements de Windows Update avec une fonctionnalité appelée [Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2), et bien que les périphériques grand public prennent en charge par défaut, les appareils Windows 10 entreprise nécessitent la mise à jour de Windows Server Services (WSUS) pour tirer parti d’Express. Si vous avez WSUS disponibles, consultez [prise en charge de remise ISV de mise à jour Express](express-update-delivery-ISV-support.md). Nous vous recommandons d’utiliser pour activer la remise de mise à jour d’Express. 

Si vous n’avez pas actuellement de WSUS est installée, mais devez plus petite mise à jour des tailles de package en attendant, vous pouvez utiliser la mise à jour mensuelle Delta. Mise à jour delta réduit sensiblement, tailles de package, mais pas autant qu’avec remise de mise à jour WSUS Express. Nous vous recommandons de déployer les mises à jour WSUS Express dès que possible pour la réduction de plus grande de tailles de package. Voici un tableau comparatif des tailles de téléchargement Delta, cumulés et Express pour Windows 10 version 1607 :

![Comparaison de la taille de téléchargement](../../media/express-update-delivery-isv-support/delta-1.png)

## <a name="what-is-monthly-delta-update"></a>Quelle est la mise à jour Delta mensuel ?

Il existe deux variantes de la mise à jour de sécurité mensuel : Delta et Cumulative.

Mise à jour mensuelle de Delta est nouveau et une solution temporaire pour les éditeurs de logiciels qui n’ont pas de WSUS disponibles pour aider à réduire les tailles de package de mise à jour.

>[!IMPORTANT]
>**Mise à jour delta est disponible pour la maintenance de Windows 10, version 1607 (mise à jour anniversaire), version 1703 (Creators Update) et la version 1709 (Fall Creators Update).** Pour les versions après la version 1709, vous devez implémenter une infrastructure de déploiement qui prend en charge [Express mise à jour de remise](express-update-delivery-ISV-support.md) pour continuer à tirer parti des mises à jour incrémentielles.

À l’aide de la mise à jour mensuelle Delta packages contiendra uniquement les mises à jour d’un mois. Cumul mensuel contient toutes les mises à jour jusqu'à cette version de mise à jour, résultant dans un fichier volumineux dont la taille augmente chaque mois. Delta et mises à jour mensuelles sont publiées le deuxième mardi de chaque mois, également appelé « Tuesday de la mise à jour ». Le tableau suivant compare les mises à jour cumulatives et Delta :

|                    | Mensuel **Delta** mettre à jour                                                                                                                                                                                                       | Mensuel **cumulés** mettre à jour                                                                                                                                                                                             |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Portée**          | Seule la mise à jour avec **uniquement de nouveaux correctifs pour le mois**                                                                                                                                                                           | Seule la mise à jour avec tous les nouveaux correctifs pour ce mois et les mois précédents                                                                                                                                                   |
| **Application**    | Applicable uniquement si la mise à jour du mois précédent a été appliqué (cumulés ou Delta)                                                                                                                                           | Peut être appliqué à tout moment                                                                                                                                                                                                |
| **Remise**       | **Publiées uniquement dans le catalogue de mise à jour de Windows** où il peut être téléchargé pour une utilisation avec d’autres outils ou d’un processus. Pas proposées aux PC qui sont connectés à Windows Update                                                         | Publié sur Windows Update (où tous les consommateurs PC installez-le), WSUS et le catalogue de mise à jour de Windows                                                                                                                |

Delta cumulé ont le même nombre de Ko, avec la même classification et libérer en même temps. Mises à jour puissent être distinguées par titre mise à jour dans le catalogue, ou par le nom du package msu :

- 2017-02 *\***mise à jour delta**\**  pour Windows 10 Version 1607 pour systèmes x64 64 (KB1234567)
- 2017-02 *\***mise à jour cumulative**\**  pour Windows 10 Version 1607 pour les systèmes x86 (KB1234567)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      

### <a name="when-to-use-monthly-delta-update"></a>Quand utiliser la mise à jour mensuelle de Delta

Si la taille de la mise à jour sur l’appareil client est un critère important, nous recommandons à l’aide de la mise à jour Delta sur les appareils qui disposent de mise à jour du mois précédent et la mise à jour sur les appareils qui sont en retard Cumulative. De cette façon, tous les appareils nécessitent uniquement une seule mise à jour pour afficher les mises à jour. Cela nécessite un petit ajustement dans le processus global de gestion des mises à jour, car vous devrez déployer différentes mises à jour basées sur les appareils sont à jour de l’organisation :

![Comparaison de la taille de téléchargement](../../media/express-update-delivery-isv-support/delta-2.png)

### <a name="prevent-deployment-of-delta-and-cumulative-updates-in-the-same-month"></a>Empêchent le déploiement des mises à jour Cumulative et Delta dans le même mois

Étant donné que la mise à jour Delta et mise à jour Cumulative sont disponibles en même temps, il est important de comprendre que se passe-t-il si vous déployez des mises à jour dans le même mois.

Si vous approuvez et déployez la même version du Delta et de la mise à jour Cumulative, vous ne générez pas uniquement le trafic réseau supplémentaire dans la mesure où les deux seront téléchargés sur le PC, mais vous n’êtes peut-être pas en mesure de redémarrer votre ordinateur pour Windows après le redémarrage.

Si le Delta et mises à jour cumulatives sont installés par inadvertance et démarrage de l’ordinateur n’est plus, vous pouvez récupérer en procédant comme suit :

1. Démarrer dans WinRE invite de commandes
2. Répertorier les packages dans un état d’attente :

    `x:\windows\system32\dism.exe /image:<drive letter for windows directory> /Get-Packages >> <path to text file>`
 
    > **Exemple**: ` x:\windows\system32\dism.exe /image:c:\ /Get-Packages >> c:\temp\packages.txt`
 
3. Ouvrez le fichier de texte où vous dirigée **get-packages**. Si vous voyez toutes les installations en attente de correctifs, exécutez **remove-package** pour chaque nom de package :
 
   `dism.exe /image:<drive letter for windows directory> /remove-package /packagename:<package name>`
 
    > **Exemple**: `x:\windows\system32\dism.exe /image:c:\ /remove-package /packagename:Package_for_KB4014329~31bf3856ad364e35~amd64~~10.0.1.0`
 
    >[!NOTE]
    >Ne supprimez pas désinstaller en attente de correctifs.

>[!IMPORTANT]
>**Mise à jour delta est disponible pour la maintenance de Windows 10, version 1607 (mise à jour anniversaire), version 1703 (Creators Update) et la version 1709 (Fall Creators Update).** Pour les versions après la version 1709, vous devez implémenter une infrastructure de déploiement qui prend en charge [Express mise à jour de remise](express-update-delivery-ISV-support.md) pour continuer à tirer parti des mises à jour incrémentielles.
