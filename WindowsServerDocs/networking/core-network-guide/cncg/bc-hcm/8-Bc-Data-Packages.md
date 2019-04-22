---
title: Créer des packages de données de serveur de contenu Web et de fichier (facultatif)
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server 2016 et Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 31e8428f-a482-4734-be1b-213912e34825
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4b8cd284a83736d17859968947f381af171fd6bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817170"
---
# <a name="create-content-server-data-packages-for-web-and-file-content-optional"></a>Créer des packages de données de serveur de contenu Web et de fichier (facultatif)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez utiliser cette procédure pour préhacher le contenu sur le Web et serveurs de fichiers et ensuite créer des packages de données à importer sur votre serveur de cache hébergé. 

Cette procédure est facultative, car vous n’êtes pas obligé de prehash et préchargement du contenu sur vos serveurs de cache hébergé. Si vous ne pas précharger contenu, données sont automatiquement ajoutées au cache hébergé, comme les clients la téléchargent via la connexion WAN.

Cette procédure fournit des instructions pour le hachage préalable du contenu sur les serveurs de fichiers et de serveurs Web. Si vous n’avez pas un de ces types de serveurs de contenu, il est inutile d’effectuer les instructions pour ce type de serveur de contenu.

>[!IMPORTANT]
>Avant d’effectuer cette procédure, vous devez installer et configurer BranchCache sur vos serveurs de contenu. En outre, si vous prévoyez de modifier le secret de serveur sur un serveur de contenu, le faire avant pre\-hachage du contenu : modification du code secret server invalide précédemment\-généré hachages.

Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs.

## <a name="to-create-content-server-data-packages"></a>Pour créer des packages de données de serveur de contenu

1. Sur chaque serveur de contenu, recherchez les dossiers et fichiers que vous souhaitez préhacher et l’ajouter à un package de données. Identifiez ou créez un dossier dans lequel vous souhaitez enregistrer votre package de données plus loin dans cette procédure.

2. Sur l’ordinateur serveur, ouvrez Windows PowerShell avec des privilèges d’administrateur.

3. Effectuez au moins des opérations suivantes, selon les types de serveurs de contenu que vous avez :

    > [!NOTE]
    > La valeur pour le chemin d’accès paramètre est le dossier où se trouve votre contenu. Vous devez remplacer les exemples de valeurs dans les commandes ci-dessous avec un emplacement de dossier valide sur votre serveur de contenu qui contient les données que vous souhaitez préhacher et l’ajouter à un package.
  
    - Si le contenu que vous souhaitez préhacher se trouve sur un serveur de fichiers, tapez la commande suivante, puis appuyez sur ENTRÉE.

        ```  
        Publish-BCFileContent -Path D:\share -StageData
        ```  

    -   Si le contenu que vous souhaitez préhacher se trouve sur un serveur Web, tapez la commande suivante, puis appuyez sur ENTRÉE.

        ```  
        Publish-BCWebContent –Path D:\inetpub\wwwroot -StageData
        ```  

4. Créez le package de données en exécutant la commande suivante sur chacun de vos serveurs de contenu. Remplacez la valeur de l’exemple \(D:\\temp\) – paramètre de Destination avec l’emplacement que vous avez identifié ou créé au début de cette procédure.

    ```  
    Export-BCDataPackage –Destination D:\temp
    ```  

5. À partir du serveur de contenu, accéder au partage sur vos serveurs de cache hébergé où vous souhaitez précharger du contenu et copiez les packages de données pour les partages sur les serveurs de cache hébergé.

Pour continuer avec ce guide, consultez [importer des Packages de données sur le serveur de Cache hébergé &#40;facultatif&#41;](9-Bc-Import-Data.md).

