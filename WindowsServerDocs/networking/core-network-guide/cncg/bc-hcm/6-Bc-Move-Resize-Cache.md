---
title: Déplacer et redimensionner le cache hébergé (facultatif)
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server 2016 et Windows 10
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: bb0eb349-914d-4596-9140-d3aae7597d55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0b0e3b6b490dead32071d99becccd9dca937f1f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406367"
---
# <a name="move-and-resize-the-hosted-cache-optional"></a>Déplacer et redimensionner le cache hébergé \(facultatif\)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez utiliser cette procédure pour déplacer le cache hébergé vers le lecteur et le dossier que vous préférez, et pour spécifier la quantité d’espace disque que le serveur de cache hébergé peut utiliser pour le cache hébergé.

Cette procédure est facultative. Si l’emplacement du cache par défaut \(% windir%\\ServiceProfiles\\NetworkService\\AppData\\local\\PeerDistPub\) et Size, ce qui correspond à 5% de l’espace total du disque dur – sont adaptés à votre déploiement, vous n’avez pas besoin de les modifier.

Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs.

### <a name="to-move-and-resize-the-hosted-cache"></a>Pour déplacer et redimensionner le cache hébergé

1. Ouvrez Windows PowerShell avec des privilèges d’administrateur.

2. Tapez la commande suivante pour déplacer le cache hébergé vers un autre emplacement sur l’ordinateur local, puis appuyez sur entrée.

    > [!IMPORTANT]
    > Avant d’exécuter la commande suivante, remplacez les valeurs de paramètre, telles que – Path et – MoveTo, par les valeurs appropriées pour votre déploiement.

    ``` 
    Set-BCCache -Path C:\datacache –MoveTo D:\datacache
    ``` 

3.  Tapez la commande suivante pour redimensionner le cache hébergé, en particulier le \- DataCache sur l’ordinateur local. Appuyez sur ENTRÉE.

    > [!IMPORTANT]
    > Avant d’exécuter la commande suivante, remplacez les valeurs de paramètre, telles que \-pourcentage, par les valeurs appropriées pour votre déploiement.  

    ``` 
    Set-BCCache -Percentage 20
    ``` 

4.  Pour vérifier la configuration du serveur de cache hébergé, tapez la commande suivante et appuyez sur entrée.

    ``` 
    Get-BCStatus
    ``` 

    Les résultats de la commande affichent l’état de tous les aspects de votre installation BranchCache. Voici quelques-uns des paramètres BranchCache et la valeur correcte pour chaque élément :

    -   DataCache | CacheFileDirectoryPath : affiche l’emplacement du disque dur qui correspond à la valeur que vous avez fournie avec le paramètre – MoveTo de la commande SetBCCache. Par exemple, si vous avez fourni la valeur D :\\DataCache, cette valeur est affichée dans la sortie de la commande.

    -   DataCache | MaxCacheSizeAsPercentageOfDiskVolume : affiche le nombre qui correspond à la valeur que vous avez fournie avec le paramètre – percentage de la commande SetBCCache. Par exemple, si vous avez fourni la valeur 20, cette valeur est affichée dans la sortie de la commande.

Pour continuer ce guide, consultez [la page préhachage et préchargement du contenu sur &#40;le&#41;serveur de cache hébergé (facultatif](7-Bc-Prehash-Preload.md)).