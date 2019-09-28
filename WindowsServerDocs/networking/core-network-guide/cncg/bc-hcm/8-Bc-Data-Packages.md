---
title: Créer des packages de données de serveur de contenu Web et de fichier (facultatif)
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server 2016 et Windows 10
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: 31e8428f-a482-4734-be1b-213912e34825
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 104e3cfd0525c43857bb37d781f6b2475978238e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406386"
---
# <a name="create-content-server-data-packages-for-web-and-file-content-optional"></a>Créer des packages de données de serveur de contenu Web et de fichier (facultatif)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Vous pouvez utiliser cette procédure pour préhacher le contenu sur les serveurs Web et de fichiers, puis créer des packages de données à importer sur votre serveur de cache hébergé. 

Cette procédure est facultative car vous n’êtes pas obligé d’effectuer un préhachage et de précharger du contenu sur vos serveurs de cache hébergé. Si vous ne Préchargez pas le contenu, les données sont automatiquement ajoutées au cache hébergé au fur et à mesure que les clients les téléchargent via la connexion WAN.

Cette procédure fournit des instructions pour le préhachage du contenu sur les serveurs de fichiers et les serveurs Web. Si vous n’avez pas l’un de ces types de serveurs de contenu, vous n’êtes pas obligé d’effectuer les instructions pour ce type de serveur de contenu.

>[!IMPORTANT]
>Avant d’effectuer cette procédure, vous devez installer et configurer BranchCache sur vos serveurs de contenu. En outre, si vous envisagez de modifier le secret du serveur sur un serveur de contenu, faites-le avant le contenu pre @ no__t-0hashing : la modification du secret du serveur invalide les hachages @ no__t-1generated précédents.

Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs.

## <a name="to-create-content-server-data-packages"></a>Pour créer des packages de données de serveur de contenu

1. Sur chaque serveur de contenu, localisez les dossiers et les fichiers que vous souhaitez préhacher et ajouter à un package de données. Identifiez ou créez un dossier dans lequel vous souhaitez enregistrer votre package de données plus loin dans cette procédure.

2. Sur l’ordinateur serveur, ouvrez Windows PowerShell avec des privilèges d’administrateur.

3. Effectuez l’une des opérations suivantes, ou les deux, selon les types de serveurs de contenu que vous avez :

    > [!NOTE]
    > La valeur du paramètre – Path est le dossier dans lequel se trouve votre contenu. Vous devez remplacer les exemples de valeurs dans les commandes ci-dessous par un emplacement de dossier valide sur votre serveur de contenu qui contient les données que vous souhaitez préhacher et ajouter à un package.
  
    - Si le contenu que vous souhaitez préhacher se trouve sur un serveur de fichiers, tapez la commande suivante, puis appuyez sur entrée.

        ```  
        Publish-BCFileContent -Path D:\share -StageData
        ```  

    -   Si le contenu que vous souhaitez préhacher se trouve sur un serveur Web, tapez la commande suivante, puis appuyez sur entrée.

        ```  
        Publish-BCWebContent –Path D:\inetpub\wwwroot -StageData
        ```  

4. Créez le package de données en exécutant la commande suivante sur chacun de vos serveurs de contenu. Remplacez l’exemple de valeur \(D : \\temp @ no__t-2 pour le paramètre – destination par l’emplacement que vous avez identifié ou créé au début de cette procédure.

    ```  
    Export-BCDataPackage –Destination D:\temp
    ```  

5. À partir du serveur de contenu, accédez au partage sur vos serveurs de cache hébergé sur lesquels vous souhaitez précharger du contenu, puis copiez les packages de données vers les partages sur les serveurs de cache hébergé.

Pour continuer ce guide, consultez [importer des packages de données sur le serveur &#40;de cache&#41;hébergé (facultatif](9-Bc-Import-Data.md)).

