---
title: Prise en charge mensuelle des ISV de mise à jour Delta sans WSUS
description: 'Rubrique Windows Server Update Service (WSUS) : comment les éditeurs de logiciels indépendants (ISV) peuvent utiliser temporairement la mise à jour différentielle mensuelle au lieu de la remise des mises à jour de WSUS Express pour réduire la taille des packages'
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: dougkim
ms.date: 10/16/2017
ms.openlocfilehash: 4607827d73c34f50f721a2774fa498eb95f9dbb8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361733"
---
# <a name="monthly-delta-update-isv-support-without-wsus"></a>Prise en charge mensuelle des ISV de mise à jour Delta sans WSUS

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10

Les téléchargements de mises à jour Windows 10 peuvent être volumineux, car chaque package contient tous les correctifs précédemment publiés pour garantir la cohérence et la simplicité.  

Depuis la version 7, Windows a pu réduire la taille des téléchargements de Windows Update avec une fonctionnalité appelée [Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2), et bien que les appareils grand public le prennent en charge par défaut, les appareils Windows 10 entreprise requièrent que Windows Server Update Services (WSUS) prenne avantage d’Express. Si WSUS est disponible, consultez [prise en charge des ISV de remise de mise à jour Express](express-update-delivery-ISV-support.md). Nous vous recommandons de l’utiliser pour activer la remise des mises à jour Express. 

Si vous n’avez pas encore installé WSUS, mais que vous avez besoin de tailles de package de mise à jour plus petites, vous pouvez utiliser la mise à jour Delta mensuelle. La mise à jour Delta réduit considérablement la taille des packages, mais pas autant qu’avec WSUS Express Update Delivery. Nous vous recommandons de déployer WSUS Express Update chaque fois que cela est possible pour une plus grande réduction de la taille des packages. Vous trouverez ci-dessous un graphique comparatif des tailles de téléchargement Delta, cumulative et rapide pour Windows 10 version 1607 :

![Comparaison de la taille du téléchargement](../../media/express-update-delivery-isv-support/delta-1.png)

## <a name="what-is-monthly-delta-update"></a>Qu’est-ce que la mise à jour Delta mensuelle ?

Il existe deux variantes de la mise à jour de sécurité mensuelle : Delta et cumul.

La mise à jour mensuelle mensuelle est une nouveauté et une solution temporaire pour les éditeurs de logiciels indépendants qui ne disposent pas de WSUS pour aider à réduire la taille des packages de mise à jour.

>[!IMPORTANT]
>**La mise à jour Delta est disponible pour la maintenance de Windows 10, version 1607 (mise à jour anniversaire), version 1703 (Creators Update) et version 1709 (automne Creators Update).** Pour les versions ultérieures à la version 1709, vous devez implémenter une infrastructure de déploiement qui prend en charge la [remise de mise à jour rapide](express-update-delivery-ISV-support.md) pour continuer à tirer parti des mises à jour incrémentielles.

En utilisant la mise à jour Delta mensuelle, les packages ne contiennent que des mises à jour d’un mois. Le cumul mensuel contient toutes les mises à jour de cette version de mise à jour, ce qui génère un fichier volumineux qui augmente chaque mois. Les mises à jour Delta et mensuelles sont publiées le deuxième mardi de chaque mois, également appelée « mise à jour Tuesday ». Le tableau suivant compare les mises à jour cumulatives et Delta :

|                    | Mise à jour **Delta** mensuelle                                                                                                                                                                                                       | Mise à jour **cumulative** mensuelle                                                                                                                                                                                             |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Portée**          | Mise à jour unique avec de **nouveaux correctifs pour ce mois** -ci                                                                                                                                                                           | Mise à jour unique avec tous les nouveaux correctifs pour le mois précédent et tous les mois précédents                                                                                                                                                   |
| **Application**    | Peut uniquement être appliqué si la mise à jour du mois précédent a été appliquée (cumulative ou Delta)                                                                                                                                           | Peut être appliqué à tout moment                                                                                                                                                                                                |
| **Envoi**       | **Publié uniquement dans Windows Update catalogue** où il peut être téléchargé pour être utilisé avec d’autres outils ou processus. Non proposé aux PC connectés à Windows Update                                                         | Publié sur Windows Update (où sont installés tous les PC grand public), WSUS et le catalogue Windows Update                                                                                                                |

Delta et cumulative ont le même numéro de base de connaissances, avec la même classification et la même version en même temps. Les mises à jour peuvent être distinguées par le titre de la mise à jour dans le catalogue ou par le nom du MSU :

- 2017-02 *\***mise à**jour\** DeltapourWindows10version1607pourlessystèmesx64(KB1234567) 
- 2017-02 *\***mise à**jour\** cumulativepourWindows10version1607pourlessystèmesbaséssurx86(KB1234567)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       

### <a name="when-to-use-monthly-delta-update"></a>Quand utiliser la mise à jour Delta mensuelle

Si la taille de la mise à jour de l’appareil client est un problème, nous vous recommandons d’utiliser la mise à jour Delta sur les appareils qui ont la mise à jour du mois précédent et la mise à jour cumulative sur les appareils qui se trouvent derrière. De cette façon, tous les appareils ne requièrent qu’une seule mise à jour pour les mettre à jour. Cela nécessite un petit ajustement dans le processus global de gestion des mises à jour, car vous devrez déployer différentes mises à jour en fonction de la façon dont les appareils sont à jour dans l’Organisation :

![Comparaison de la taille du téléchargement](../../media/express-update-delivery-isv-support/delta-2.png)

### <a name="prevent-deployment-of-delta-and-cumulative-updates-in-the-same-month"></a>Empêcher le déploiement de mises à jour Delta et cumulatives au cours du même mois

Étant donné que la mise à jour Delta et la mise à jour cumulative sont disponibles en même temps, il est important de comprendre ce qui se passe si vous déployez les deux mises à jour au cours du même mois.

Si vous approuvez et déployez la même version du Delta et la mise à jour cumulative, vous ne générez pas uniquement le trafic réseau supplémentaire, car les deux sont téléchargés sur le PC, mais vous ne pourrez peut-être pas redémarrer votre ordinateur vers Windows après le redémarrage.

Si les mises à jour Delta et cumulatives sont installées par inadvertance et que votre ordinateur n’est plus en cours de démarrage, vous pouvez effectuer la récupération en procédant comme suit :

1. Démarrer avec l’invite de commandes WinRE
2. Répertorier les packages dans un état d’attente :

    `x:\windows\system32\dism.exe /image:<drive letter for windows directory> /Get-Packages >> <path to text file>`
 
    > **Exemple**:` x:\windows\system32\dism.exe /image:c:\ /Get-Packages >> c:\temp\packages.txt`
 
3. Ouvrez le fichier texte dans lequel vous avez dirigé **« obten-packages »** . Si vous voyez des correctifs d’installation en attente, exécutez **Remove-Package** pour chaque nom de package :
 
   `dism.exe /image:<drive letter for windows directory> /remove-package /packagename:<package name>`
 
    > **Exemple**:`x:\windows\system32\dism.exe /image:c:\ /remove-package /packagename:Package_for_KB4014329~31bf3856ad364e35~amd64~~10.0.1.0`
 
    >[!NOTE]
    >Ne supprimez pas les correctifs de désinstallation en attente.

>[!IMPORTANT]
>**La mise à jour Delta est disponible pour la maintenance de Windows 10, version 1607 (mise à jour anniversaire), version 1703 (Creators Update) et version 1709 (automne Creators Update).** Pour les versions ultérieures à la version 1709, vous devez implémenter une infrastructure de déploiement qui prend en charge la [remise de mise à jour rapide](express-update-delivery-ISV-support.md) pour continuer à tirer parti des mises à jour incrémentielles.
