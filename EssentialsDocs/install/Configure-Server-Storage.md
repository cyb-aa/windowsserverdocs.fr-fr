---
title: Configuration du stockage serveur
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef7ddcdd-3a74-40ca-9487-c3a6fc5155a5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6de485f6fd46464ba707bc0871f60ac2fec5a1db
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="configure-server-storage"></a>Configuration du stockage serveur

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

## <a name="sample-hard-disk-configurations"></a>Exemples de configurations de disque dur  
 Le tableau suivant propose des exemples de configurations de disque dur. Les estimations sont basées sur les fonctionnalités et d’utilisation classiques, mais elles ne répondent pas les problèmes qui affectent les performances optimales. Vous pouvez utiliser n’importe quel type de prise en charge des disques durs pour ces configurations (tel que SATA ou SCSI), selon les préférences et les besoins de votre client.  
  
> [!IMPORTANT]
>   WindowsServerEssentials doit être installé en tant que volume C:, et la taille du volume doit être au moins 60Go. Il est recommandé de créer deux partitions sur votre disque de système d’exploitation et de pas utiliser C: (partition système) pour stocker des données d’entreprise.  
  
|Niveau serveur|Configuration de disque|  
|------------------|------------------------|  
|Entrée|-Deux disques physiques<br /><br /> -Configurés comme un jeu en miroir RAID 1 contenant les éléments suivants:<br /><br /> -C: volume? 60GO<br /><br /> -D: volume? 1000GO|  
|Moyenne|-Trois disques physiques<br /><br /> -Configuré comme un ensemble de RAID 5 contenant les éléments suivants:<br /><br /> -C: volume? 60GO<br /><br /> -D: volume? 1500GO|  
|Élevé|-Cinq ou plusieurs disques physiques totales<br /><br /> -Deux des disques dans un jeu en miroir RAID 1 contenant le volume C:? 100GO<br /><br /> -Tous les autres disques un jeu RAID 5 contenant les éléments suivants:<br /><br /> -D: volume? 1500GO<br /><br /> -E: volume? 1500GO|  
  
 Ces recommandations tiennent compte de la taille du système d’exploitation installé, la taille moyenne des que le serveur utilise le stockage des données et la croissance du stockage pendant la durée de vie prévue des données du serveur. Les volumes peuvent être des partitions sur un seul disque physique ou ils peuvent être placés sur des disques physiques distincts. Étant donné que le serveur stocke des données importantes pour votre client, il est recommandé d’utiliser plusieurs disques physiques et de protéger vos données client à l’aide de matériel RAID ou des espaces de stockage.  
  
## <a name="configuring-your-server-backup"></a>Configuration de la sauvegarde de votre serveur  
 En plus des disques durs internes sur le serveur, les utilisateurs doivent envisager l’utilisation de disques durs externes USB pour les sauvegardes. Dans l’idéal, le client doit disposer d’au moins deux disques durs externes avec une capacité suffisante pour sauvegarder toutes les données sur le serveur. Si les disques durs externes sont utilisés, le client peut prendre un disque hors site chaque nuit afin de mieux protéger les données.  
  
## <a name="partition-configuration"></a>Configuration de la partition  
 Lors de la Configuration initiale du serveur, un ensemble de dossiers de serveur par défaut qui incluent des dossiers partagés et le dossier de sauvegarde d’ordinateur client sont créés dans la plus grande partition de données sur le disque 0.  
  
## <a name="see-also"></a>Voir aussi  

 [Prise en main du kit ADK WindowsServerEssentials](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)

 [Prise en main du kit ADK WindowsServerEssentials](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Création et personnalisation de l’Image](../install/Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](../install/Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](../install/Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](../install/Testing-the-Customer-Experience.md)

