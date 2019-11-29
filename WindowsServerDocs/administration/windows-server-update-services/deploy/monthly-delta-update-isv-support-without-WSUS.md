---
title: Prise en charge par les ISV de la mise à jour delta mensuelle sans WSUS
description: 'Rubrique Windows Server Update Service (WSUS) : comment des éditeurs de logiciels indépendants (ISV) peuvent-ils utiliser temporairement la mise à jour delta mensuelle au lieu de la distribution des mises à jour Express WSUS pour réduire la taille des packages'
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
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361733"
---
# <a name="monthly-delta-update-isv-support-without-wsus"></a>Prise en charge par les ISV de la mise à jour delta mensuelle sans WSUS

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016, Windows 10

Les téléchargements de Windows 10 Update peuvent être volumineux car, à des fins de cohérence et de simplicité, chaque package contient l’ensemble des correctifs précédemment publiés.  

Depuis la version 7, Windows est parvenu à réduire la taille des téléchargements de Windows Update grâce à la mise en place d’une fonctionnalité appelée [Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2). Bien que les appareils grand public le prennent en charge par défaut, les appareils Windows 10 Entreprise ont besoin de Windows Server Update Services (WSUS) pour tirer parti de la fonctionnalité Express. Si Windows Server Update Services (WSUS) est disponible, voir [Prise en charge par les ISV de la distribution des mises à jour Express](express-update-delivery-ISV-support.md). Nous vous recommandons de l’utiliser pour activer la distribution des mises à jour Express. 

Si vous n’avez pas encore installé WSUS mais avez besoin de tailles de package de mise à jour plus petites en attendant, vous pouvez utiliser la mise à jour delta mensuelle. La mise à jour delta réduit considérablement les tailles de package, mais pas autant que la distribution de mises à jour Express WSUS. Nous vous recommandons de déployer la distribution de mises à jour Express WSUS chaque fois que c’est possible pour une plus grande réduction des tailles de package. Vous trouverez ci-dessous un graphique comparatif des tailles de téléchargement delta, cumulative et rapide pour Windows 10 version 1607 :

![Comparaison de la taille du téléchargement](../../media/express-update-delivery-isv-support/delta-1.png)

## <a name="what-is-monthly-delta-update"></a>Qu’est-ce que la mise à jour delta mensuelle ?

Il existe deux variantes de la mise à jour de sécurité mensuelle : delta et cumulative.

La mise à jour delta mensuelle est une nouveauté et une solution provisoire pour les éditeurs de logiciels indépendants qui ne disposent pas de WSUS pour réduire les tailles de package de mise à jour.

>[!IMPORTANT]
>**La mise à jour Delta est disponible pour la maintenance de Windows 10, version 1607 (mise à jour anniversaire), version 1703 (Creators Update) et version 1709 (Fall Creators Update).** Pour les mises en production postérieures à la version 1709, vous devez implémenter une infrastructure de déploiement qui prend en charge la [distribution de mises à jour](express-update-delivery-ISV-support.md) pour continuer à profiter des mises à jour incrémentielles.

Avec un mise à jour delta mensuelle, les packages ne contiennent que les mises à jour d’un mois. Une mise à jour cumulative mensuelle contient toutes les mises à jour jusqu’à cette mise en production de mise à jour. Elle produit ainsi un fichier volumineux dont la taille augmente chaque mois. Les mises à jour Delta et mensuelles sont publiées le deuxième mardi de chaque mois. On les appelle aussi « mises à jour du mardi ». Le tableau suivant compare les mises à jour delta et cumulatives :

|                    | Mise à jour **delta** mensuelle                                                                                                                                                                                                       | Mise à jour **cumulative** mensuelle                                                                                                                                                                                             |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Étendue**          | Mise à jour unique avec **seulement quelques nouveaux correctifs pour ce mois**                                                                                                                                                                           | Mise à jour unique avec tous les nouveaux correctifs pour ce mois et tous les mois précédents                                                                                                                                                   |
| **Application**    | Ne peut être appliqués que si la mise à jour (cumulative ou delta) du mois précédent a été appliquée                                                                                                                                           | Peut être appliquée à tout moment                                                                                                                                                                                                |
| **Distribution**       | **Publiée uniquement dans le Catalogue Windows Update** d’où vous pouvez la télécharger pour l’utilisé avec d’autres outils ou processus. Non proposée aux PC connectés à Windows Update                                                         | Publiée sur Windows Update (à partir où tous les PC grand public l’installent), dans WSUS et dans le Catalogue Windows Update                                                                                                                |

Les mises à jour delta et cumulative ont le même numéro de base de connaissances, avec les mêmes classification et même version en même temps. Les mises à jour peuvent être distinguées par leur titre dans le catalogue ou par leur nom de MSU :

- 2017-02 *\***Mise à jour delta**\**  pour Windows 10 version 1607 pour systèmes x64 (KB1234567)
- 2017-02 *\***mise à jour cumulative**\**  pour Windows 10 version 1607 pour les systèmes x86 (KB1234567)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      

### <a name="when-to-use-monthly-delta-update"></a>Quand utiliser la mise à jour delta mensuelle

Si la taille de la mise à jour de l’appareil client est un souci, nous vous recommandons d’utiliser la mise à jour delta sur des appareils qui disposent de la mise à jour du mois précédent, et la mise à jour cumulative sur des appareils en retard de mise à jour. De cette façon, tous les appareils ne requièrent qu’une seule mise à jour. Cela nécessite un petit ajustement du processus global de gestion des mises à jour, car vous devrez déployer différentes mises à jour en fonction du niveau de mise à jour des appareils dans l’organisation :

![Comparaison de la taille du téléchargement](../../media/express-update-delivery-isv-support/delta-2.png)

### <a name="prevent-deployment-of-delta-and-cumulative-updates-in-the-same-month"></a>Empêcher le déploiement de mises à jour delta et cumulative au cours du même mois

Étant donné que les mises à jour delta et cumulative sont disponibles en même temps, il est important de comprendre ce qui se passe si vous déployez les deux mises à jour le même mois.

Si vous approuvez et déployez la même version de mises à jour delta cumulative, non seulement vous ne générez du trafic réseau supplémentaire, car les deux mises à jour sont téléchargées sur le PC, mais il se peut que vous ne puissiez plus redémarrer votre ordinateur sous Windows après le redémarrage.

Si les deux mises à jour delta et cumulative sont installées par inadvertance et que votre ordinateur n’est plus en cours de démarrage, vous pouvez rattraper l’erreur en procédant comme suit :

1. Démarrez dans l’invite de commandes WinRE
2. Répertoriez les packages en état d’attente :

    `x:\windows\system32\dism.exe /image:<drive letter for windows directory> /Get-Packages >> <path to text file>`
 
    > **Exemple** : ` x:\windows\system32\dism.exe /image:c:\ /Get-Packages >> c:\temp\packages.txt`
 
3. Ouvrez le fichier texte vers lequel vous avez canalisé les **get-packages**. Si vous voyez des correctifs en attente, exécutez la commande **remove-package** pour chaque nom de package :
 
   `dism.exe /image:<drive letter for windows directory> /remove-package /packagename:<package name>`
 
    > **Exemple** : `x:\windows\system32\dism.exe /image:c:\ /remove-package /packagename:Package_for_KB4014329~31bf3856ad364e35~amd64~~10.0.1.0`
 
    >[!NOTE]
    >Ne supprimez pas les correctifs de désinstallation en attente.

>[!IMPORTANT]
>**La mise à jour Delta est disponible pour la maintenance de Windows 10, version 1607 (mise à jour anniversaire), version 1703 (Creators Update) et version 1709 (Fall Creators Update).** Pour les mises en production postérieures à la version 1709, vous devez implémenter une infrastructure de déploiement qui prend en charge la [distribution de mises à jour](express-update-delivery-ISV-support.md) pour continuer à profiter des mises à jour incrémentielles.
