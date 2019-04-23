---
title: Exemples et Scripts de Diskpart
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 319c0795-11df-47c8-b203-eadb0577ee0d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a8bd0dfab98ca705cf64766d66285c69bd3d3129
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862790"
---
# <a name="diskpart-scripts-and-examples"></a>Exemples et Scripts de Diskpart

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Utilisez Diskpart `/s` pour exécuter des scripts qui automatisent disque\-tâches liées, telles que la création de volumes ou convertir les disques en disques dynamiques. Ces tâches de script est utile si vous déployez Windows en utilisant le programme d’installation sans assistance ou l’outil Sysprep, qui ne prennent pas en charge la création de volumes autres que le volume de démarrage.  
  
-   Pour créer un script Diskpart, créez un fichier texte qui contient les commandes Diskpart que vous souhaitez exécuter, avec une seule commande par ligne et aucune ligne vide. Vous pouvez démarrer une ligne avec `rem` afin que la ligne un commentaire.  
  
    par exemple, ici s un script qui permet d’effacer un disque, puis crée une partition de 300 Mo pour l’environnement de récupération Windows :  
  
    ```  
    select disk 0  
    clean  
    convert gpt  
    create partition primary size=300  
    format quick fs=ntfs label="Windows RE tools"  
    assign letter="T"  
    ```  
  
-   Pour exécuter un script DiskPart, à l’invite de commandes, tapez la commande suivante, où *scriptname* est le nom du fichier texte qui contient votre script.  
  
    ```  
    diskpart /s scriptname.txt  
    ```  
  
-   Pour rediriger la sortie du script DiskPart dans un fichier, tapez la commande suivante, où *logfile* est le nom du fichier texte où DiskPart écrit sa sortie.  
  
    ```  
    diskpart /s scriptname.txt > logfile.txt  
    ```  
  
> [!IMPORTANT]  
> Lorsque vous utilisez le **DiskPart** commande dans le cadre d’un script, nous vous recommandons d’effectuer toutes les opérations DiskPart ensemble au sein d’un seul script DiskPart. Vous pouvez exécuter des scripts DiskPart consécutifs, mais vous devez autoriser au moins 15 secondes entre chaque script pour un arrêt complet de l’exécution précédente avant d’exécuter le **DiskPart** commande à nouveau dans les scripts suivants. Sinon, les scripts successifs peuvent échouer. Vous pouvez ajouter une pause entre des scripts DiskPart en ajoutant la `timeout /t 15` commande à votre fichier de commandes, ainsi que vos scripts DiskPart.  
  
Lorsque DiskPart démarre, l’affichage DiskPart version et ordinateur nom à l’invite de commandes. Par défaut, si DiskPart rencontre une erreur lorsque vous tentez d’effectuer une tâche du script, DiskPart arrête le traitement du script et affiche un code d’erreur \(, sauf si vous avez spécifié le **noerr** paramètre\). Toutefois, DiskPart renvoie toujours les erreurs lorsqu’il rencontre des erreurs de syntaxe, indépendamment de si vous avez utilisé le **noerr** paramètre. Le **noerr** paramètre vous permet d’effectuer des tâches utiles telles que l’utilisation d’un seul script pour supprimer toutes les partitions sur tous les disques, quel que soit le nombre total de disques.  
  
## <a name="see-also"></a>Voir aussi  
[Exemple : Configurer l’interface UEFI\/gpt\-en fonction des Partitions de disque dur par l’utilisation de Windows PE et de DiskPart](https://technet.microsoft.com/library/hh825686.aspx)  
[Exemple : Configurer le BIOS\/MBR\-en fonction des Partitions de disque dur à l’aide de Windows PE et DiskPart](https://technet.microsoft.com/library/hh825677.aspx)  
[Applets de commande de stockage dans Windows PowerShell](https://technet.microsoft.com/library/hh848705.aspx)  
  

