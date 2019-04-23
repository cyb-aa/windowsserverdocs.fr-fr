---
title: Déplacer et redimensionner le cache hébergé (facultatif)
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server 2016 et Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: bb0eb349-914d-4596-9140-d3aae7597d55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cb75e06b5da8ff95fcf763b22c5160ea200035f3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853530"
---
# <a name="move-and-resize-the-hosted-cache-optional"></a>Déplacez et redimensionnez le Cache hébergé \(facultatif\)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez utiliser cette procédure pour déplacer le cache hébergé sur le lecteur et le dossier que vous préférez et pour spécifier la quantité d’espace disque que le serveur de cache hébergé peut utiliser pour le cache hébergé.

Cette procédure est facultative. Si l’emplacement du cache de la valeur par défaut \(%Windir%\\ServiceProfiles\\NetworkService\\AppData\\Local\\PeerDistPub\) et taille – ce qui est de 5 % du disque dur total espace – sont appropriées pour votre déploiement, vous n’avez pas besoin de les modifier.

Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs.

### <a name="to-move-and-resize-the-hosted-cache"></a>Pour déplacer et redimensionner le cache hébergé

1. Ouvrez Windows PowerShell avec des privilèges d’administrateur.

2. Tapez la commande suivante pour déplacer le cache hébergé à un autre emplacement sur l’ordinateur local, puis appuyez sur ENTRÉE.

    > [!IMPORTANT]
    > Avant d’exécuter la commande suivante, remplacez les valeurs de paramètres, tels que – chemin d’accès et – MoveTo, avec les valeurs appropriées pour votre déploiement.

    ``` 
    Set-BCCache -Path C:\datacache –MoveTo D:\datacache
    ``` 

3.  Tapez la commande suivante pour redimensionner hébergé cache – en particulier le datacache \- sur l’ordinateur local. Appuyez sur ENTRÉE.

    > [!IMPORTANT]
    > Avant d’exécuter la commande suivante, remplacez les valeurs des paramètres, tels que \-pourcentage, avec les valeurs appropriées pour votre déploiement.  

    ``` 
    Set-BCCache -Percentage 20
    ``` 

4.  Pour vérifier la configuration de serveur de cache hébergé, tapez la commande suivante et appuyez sur ENTRÉE.

    ``` 
    Get-BCStatus
    ``` 

    Les résultats de la commande affichent l’état de tous les aspects de votre installation de BranchCache. Voici quelques-uns des paramètres BranchCache et la valeur correcte pour chaque élément :

    -   DataCache | CacheFileDirectoryPath: Affiche l’emplacement de disque dur qui correspond à la valeur que vous avez fourni avec le paramètre – MoveTo de la commande SetBCCache. Par exemple, si vous avez fourni la valeur D:\\datacache, dont la valeur s’affiche dans la sortie de commande.

    -   DataCache | MaxCacheSizeAsPercentageOfDiskVolume: Affiche le nombre qui correspond à la valeur que vous avez fourni avec le paramètre de pourcentage de la commande SetBCCache. Par exemple, si vous avez fourni la valeur 20, cette valeur s’affiche dans la sortie de commande.

Pour continuer avec ce guide, consultez [Prehash et préchargement du contenu sur le serveur de Cache hébergé &#40;facultatif&#41;](7-Bc-Prehash-Preload.md).