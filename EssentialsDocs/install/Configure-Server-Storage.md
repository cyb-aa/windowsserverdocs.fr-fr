---
title: Configuration du stockage du serveur
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: ef7ddcdd-3a74-40ca-9487-c3a6fc5155a5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b543e473c545f9446ab33b8f81d5b2e5fa4ed877
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817222"
---
# <a name="configure-server-storage"></a>Configuration du stockage du serveur

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

## <a name="sample-hard-disk-configurations"></a>Exemples de configurations de disques durs  
 Le tableau suivant suggère des exemples de configurations de disques durs. Les estimations sont basées sur des conditions d'utilisation et de fonctionnement normales, mais elles ne répondent pas aux critères de performances optimales. Vous pouvez utiliser tout type de disque dur pris en charge pour ces configurations (tel que SATA ou SCSI), selon les préférences et les besoins de votre client.  
  
> [!IMPORTANT]
>   Windows Server Essentials doit être installé en tant que volume C :, et la taille du volume doit être d’au moins 60 Go. Il est recommandé de créer deux partitions sur votre lecteur de système d’exploitation et de ne pas utiliser C: (partition système) pour stocker des données d’entreprise.  
  
|Niveau serveur|Configuration de disque|  
|------------------|------------------------|  
|Entrée|-Deux disques physiques<br /><br /> -Configuré en tant que jeu en miroir RAID 1 contenant les éléments suivants :<br /><br /> -C : volume ? 60 Go<br /><br /> -D : volume ? 1000 GO|  
|Moyenne|-Trois disques physiques<br /><br /> -Configuré comme un ensemble RAID 5 contenant les éléments suivants :<br /><br /> -C : volume ? 60 Go<br /><br /> -D : volume ? 1500 GO|  
|Élevée|-Au moins cinq disques physiques au total<br /><br /> -Deux disques dans un jeu en miroir RAID 1 contenant le volume C :? 100 GO<br /><br /> -Tous les disques restants dans un ensemble RAID 5 contenant les éléments suivants :<br /><br /> -D : volume ? 1500 GO<br /><br /> -E : volume ? 1500 GO|  
  
 Ces recommandations tiennent compte de la taille du système d’exploitation installé, de la taille moyenne du stockage de données que le serveur utilise et de la croissance du stockage de données prévue au cours de la durée de vie du serveur. Les volumes peuvent être des partitions sur un seul disque physique ou ils peuvent être placés sur des disques physiques distincts. Étant donné que le serveur stocke des données importantes pour votre client, il est recommandé d’utiliser plusieurs disques physiques et de protéger les données de vos clients à l’aide de RAID matériel ou d’espaces de stockage.  
  
## <a name="configuring-your-server-backup"></a>Configuration de la sauvegarde du serveur  
 En plus des disques durs internes du serveur, les clients devraient envisager l'utilisation de disques durs externes USB pour les sauvegardes. Dans l'idéal, le client doit disposer d'au moins deux disques durs externes avec une capacité suffisante pour sauvegarder l'ensemble des données sur le serveur. Si l'on utilise des disques durs externes sont utilisés, le client peut emporter chaque nuit un disque hors site afin de mieux protéger les données.  
  
## <a name="partition-configuration"></a>Configuration de partition  
 Lors de la configuration initiale du serveur, un ensemble de dossiers serveur par défaut incluant les dossiers partagés et le dossier de sauvegarde de l'ordinateur client ont été créés dans la plus grande partition de données sur le disque 0.  
  
## <a name="see-also"></a>Voir aussi  

 [Prise en main avec Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](Testing-the-Customer-Experience.md)

 [Prise en main avec Windows Server Essentials ADK](../install/Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Création et personnalisation de l’Image](../install/Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](../install/Additional-Customizations.md)   
 [Préparation de l’image pour le déploiement](../install/Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience utilisateur](../install/Testing-the-Customer-Experience.md)

