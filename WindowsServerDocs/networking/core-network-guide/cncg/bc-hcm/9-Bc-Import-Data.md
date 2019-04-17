---
title: Importer des Packages de données sur le serveur de Cache hébergé (facultatif)
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server2016 et Windows10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: d6159e91-f77c-42ec-9180-14bbb230ad17
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e0bd8f12ab76c8e2bf03ba79ce46a4cbea2f4dc5
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="import-data-packages-on-the-hosted-cache-server-optional"></a>Importer des Packages de données sur hébergé \(Optional\) serveur de Cache

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016, Windows Server2012R2, Windows Server2012

Vous pouvez utiliser cette procédure pour importer des packages de données et précharger du contenu sur vos serveurs de cache hébergé.

Cette procédure est facultative, car vous n’êtes pas obligé de prehash et préchargement du contenu sur vos serveurs de cache hébergé.

Si vous le faites pas le contenu de chargement pre\, données sont automatiquement ajoutées au cache hébergé que les clients la téléchargent via la connexion de réseau étendue.

Vous devez être membre du groupe Administrateurs pour effectuer cette procédure.

## <a name="to-import-data-packages-on-the-hosted-cache-server"></a>Pour importer des packages de données sur le serveur de cache hébergé  

1. Sur l’ordinateur du serveur, ouvrez Windows PowerShell avec des privilèges d’administrateur.

2. Tapez la commande suivante, en remplaçant la valeur pour le chemin d’accès au paramètre – avec l’emplacement du dossier où stocker vos packages de données et appuyez sur ENTRÉE.

    ```  
    Import-BCCachePackage –Path D:\temp\PeerDistPackage.zip
    ```  

3. Si vous avez plusieurs serveurs de cache hébergé où vous souhaitez précharger du contenu, effectuez cette procédure sur chaque serveur de cache hébergé.

Pour continuer avec ce guide, consultez [configurer Client hébergé Cache de la découverte automatique par le Point de connexion de Service](10-Bc-Client-By-Scp.md).
