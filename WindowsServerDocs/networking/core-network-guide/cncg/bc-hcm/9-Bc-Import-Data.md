---
title: Importer des packages de données sur le serveur de cache hébergé (facultatif)
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server 2016 et Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: d6159e91-f77c-42ec-9180-14bbb230ad17
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 440ef1e04143cba09213ffea634aa9d4fea51dab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888000"
---
# <a name="import-data-packages-on-the-hosted-cache-server-optional"></a>Importer des Packages de données sur le serveur de Cache hébergé \(facultatif\)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez utiliser cette procédure pour importer des packages de données et de précharger du contenu sur vos serveurs de cache hébergé.

Cette procédure est facultative, car vous n’êtes pas obligé de prehash et préchargement du contenu sur vos serveurs de cache hébergé.

Si vous le faites pas pre\-charge contenu, les données sont ajoutées automatiquement au cache hébergé comme les clients la téléchargent via la connexion WAN.

Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs.

## <a name="to-import-data-packages-on-the-hosted-cache-server"></a>Pour importer des packages de données sur le serveur de cache hébergé  

1. Sur l’ordinateur serveur, ouvrez Windows PowerShell avec des privilèges d’administrateur.

2. Tapez la commande suivante, en remplaçant la valeur pour le paramètre – Path avec l’emplacement du dossier où vous avez stocké vos packages de données, puis appuyez sur ENTRÉE.

    ```  
    Import-BCCachePackage –Path D:\temp\PeerDistPackage.zip
    ```  

3. Si vous avez plusieurs serveurs de cache hébergé où vous souhaitez précharger du contenu, effectuez cette procédure sur chaque serveur de cache hébergé.

Pour continuer avec ce guide, consultez [configurer Client hébergé Cache la découverte automatique par Point de connexion de Service](10-Bc-Client-By-Scp.md).
