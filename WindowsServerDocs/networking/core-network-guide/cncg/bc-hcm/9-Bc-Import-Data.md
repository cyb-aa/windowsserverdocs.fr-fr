---
title: Importer des packages de données sur le serveur de cache hébergé (facultatif)
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server 2016 et Windows 10
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: d6159e91-f77c-42ec-9180-14bbb230ad17
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 41bb7f5f0637bd4b1f1bfa3c0451debff58a3e5a
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318432"
---
# <a name="import-data-packages-on-the-hosted-cache-server-optional"></a>Importer des packages de données sur le serveur de cache hébergé \(facultatif\)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez utiliser cette procédure pour importer des packages de données et précharger du contenu sur vos serveurs de cache hébergé.

Cette procédure est facultative car vous n’êtes pas obligé d’effectuer un préhachage et de précharger du contenu sur vos serveurs de cache hébergé.

Si vous ne pré\-pas de charger le contenu, les données sont automatiquement ajoutées au cache hébergé à mesure que les clients les téléchargent via la connexion WAN.

Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs.

## <a name="to-import-data-packages-on-the-hosted-cache-server"></a>Pour importer des packages de données sur le serveur de cache hébergé  

1. Sur l’ordinateur serveur, ouvrez Windows PowerShell avec des privilèges d’administrateur.

2. Tapez la commande suivante, en remplaçant la valeur du paramètre – Path par l’emplacement du dossier où vous avez stocké vos packages de données, puis appuyez sur entrée.

    ```  
    Import-BCCachePackage –Path D:\temp\PeerDistPackage.zip
    ```  

3. Si vous avez plusieurs serveurs de cache hébergé sur lesquels vous souhaitez précharger du contenu, effectuez cette procédure sur chaque serveur de cache hébergé.

Pour continuer ce guide, consultez [configurer la découverte du cache hébergé automatiquement par le point de connexion de service](10-Bc-Client-By-Scp.md).
