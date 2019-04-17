---
title: Créez un contenu lots de données de serveur Web et le fichier de contenu (facultatif)
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server2016 et Windows10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 31e8428f-a482-4734-be1b-213912e34825
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8f814bbac5c74081259d8eef6deda79d914bfec7
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="create-content-server-data-packages-for-web-and-file-content-optional"></a>Créez un contenu lots de données de serveur Web et le fichier de contenu (facultatif)

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016, Windows Server2012R2, Windows Server2012

Vous pouvez utiliser cette procédure pour préhacher le contenu sur des serveurs Web et de fichiers et puis créer des packages de données pour importer sur votre serveur de cache hébergé. 

Cette procédure est facultative, car vous n’êtes pas obligé de prehash et préchargement du contenu sur vos serveurs de cache hébergé. Si vous ne préchargez pas de contenu, données sont automatiquement ajoutées au cache hébergé que les clients la téléchargent via la connexion de réseau étendue.

Cette procédure fournit des instructions pour préhacher le contenu sur des serveurs de fichiers et de serveurs Web. Si vous n’avez pas un de ces types de serveurs de contenu, il est inutile d’effectuer les instructions pour ce type de serveur de contenu.

>[!IMPORTANT]
>Avant d’effectuer cette procédure, vous devez installer et configurer BranchCache sur vos serveurs de contenu. En outre, si vous prévoyez de modifier le secret de serveur sur un serveur de contenu, le faire avant le hachage de pre\ contenu: modification du code secret serveur invalide hachages previously\ généré.

Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs.

## <a name="to-create-content-server-data-packages"></a>Pour créer des packages de données de serveur de contenu

1. Sur chaque serveur de contenu, recherchez les dossiers et fichiers que vous souhaitez préhacher et l’ajouter à un package de données. Identifiez ou créez un dossier dans lequel vous voulez enregistrer votre package de données plus loin dans cette procédure.

2. Sur l’ordinateur du serveur, ouvrez Windows PowerShell avec des privilèges d’administrateur.

3. Effectuez au moins des opérations suivantes, selon les types de serveurs de contenu que vous avez:

    > [!NOTE]
    > La valeur pour le chemin d’accès paramètre est le dossier où se trouve votre contenu. Vous devez remplacer les valeurs d’exemple dans les commandes ci-dessous avec un emplacement de dossier valide sur votre serveur de contenu qui contient les données que vous souhaitez préhacher et l’ajouter à un package.
  
    - Si le contenu que vous souhaitez préhacher se trouve sur un serveur de fichiers, tapez la commande suivante et appuyez sur ENTRÉE.

        ```  
        Publish-BCFileContent -Path D:\share -StageData
        ```  

    -   Si le contenu que vous souhaitez préhacher se trouve sur un serveur Web, tapez la commande suivante et appuyez sur ENTRÉE.

        ```  
        Publish-BCWebContent –Path D:\inetpub\wwwroot -StageData
        ```  

4. Créer le package de données en exécutant la commande suivante sur chacun de vos serveurs de contenu. Remplacez le \(D:\\temp\) valeur exemple pour le paramètre – Destination avec l’emplacement que vous avez identifié ou créé au début de cette procédure.

    ```  
    Export-BCDataPackage –Destination D:\temp
    ```  

5. Accéder au partage sur vos serveurs de cache hébergé où vous souhaitez précharger du contenu à partir du serveur de contenu et copier les packages de données sur les partages sur les serveurs de cache hébergé.

Pour continuer avec ce guide, consultez [importer des Packages de données sur le serveur de Cache hébergé et #40; facultatif et #41;](9-Bc-Import-Data.md).

