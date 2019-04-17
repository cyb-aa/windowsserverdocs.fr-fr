---
title: Déplacer et redimensionner le Cache hébergé (facultatif)
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server2016 et Windows10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: bb0eb349-914d-4596-9140-d3aae7597d55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2a3ed1d6de1281575b33cdf75a5970d2ed033085
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="move-and-resize-the-hosted-cache-optional"></a>Déplacer et redimensionner la \(Optional\) Cache hébergé

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016, Windows Server2012R2, Windows Server2012

Vous pouvez utiliser cette procédure pour déplacer le cache hébergé sur le lecteur et le dossier que vous préférez et pour spécifier la quantité d’espace disque que le serveur de cache hébergé peut utiliser pour le cache hébergé.

Cette procédure est facultative. Si le par défaut du cache emplacement \(%windir%\\ServiceProfiles\\NetworkService\\AppData\\Local\\PeerDistPub\) et la taille est de 5% de l’espace disque total – qui sont appropriées pour votre déploiement, il est inutile de les modifier.

Vous devez être membre du groupe Administrateurs pour effectuer cette procédure.

### <a name="to-move-and-resize-the-hosted-cache"></a>Pour déplacer et redimensionner le cache hébergé

1. Ouvrez Windows PowerShell avec des privilèges d’administrateur.

2. Tapez la commande suivante pour déplacer le cache hébergé dans un autre emplacement sur l’ordinateur local et appuyez sur ENTRÉE.

    > [!IMPORTANT]
    > Avant d’exécuter la commande suivante, remplacez les valeurs de paramètre, par exemple: chemin d’accès et – MoveTo, avec les valeurs appropriées pour votre déploiement.

    ``` 
    Set-BCCache -Path C:\datacache –MoveTo D:\datacache
    ``` 

3.  Tapez la commande suivante pour redimensionner hébergé cache – spécifiquement le datacache \ - sur l’ordinateur local. Appuyez sur ENTRÉE.

    > [!IMPORTANT]
    > Avant d’exécuter la commande suivante, remplacez les valeurs de paramètre, tels que \-Percentage, avec les valeurs appropriées pour votre déploiement.  

    ``` 
    Set-BCCache -Percentage 20
    ``` 

4.  Pour vérifier la configuration de serveur de cache hébergé, tapez la commande suivante et appuyez sur ENTRÉE.

    ``` 
    Get-BCStatus
    ``` 

    Les résultats de la commande affichent l’état de tous les aspects de votre installation de BranchCache. Voici quelques-uns des paramètres BranchCache et la valeur correcte pour chaque élément:

    -   DataCache | CacheFileDirectoryPath: Affiche l’emplacement du disque dur qui correspond à la valeur que vous avez fourni avec le paramètre – MoveTo de la commande SetBCCache. Par exemple, si vous avez fourni la valeur D:\\datacache, cette valeur s’affiche dans la sortie de commande.

    -   DataCache | MaxCacheSizeAsPercentageOfDiskVolume: Affiche le nombre qui correspond à la valeur que vous avez fourni avec le paramètre pourcentage de la commande SetBCCache. Par exemple, si vous avez fourni la valeur 20, cette valeur s’affiche dans la sortie de commande.

Pour continuer avec ce guide, consultez [Prehash et préchargement du contenu sur le serveur de Cache hébergé et #40; facultatif et #41;](7-Bc-Prehash-Preload.md).