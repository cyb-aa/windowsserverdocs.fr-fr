---
title: Scripts et exemples DiskPart
description: La rubrique commandes Windows pour les scripts diskpart et des exemples sur l’automatisation des tâches liées aux disques, telles que la création de volumes ou la conversion de disques en disques dynamiques.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 319c0795-11df-47c8-b203-eadb0577ee0d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bdfa35e8597479f32bc9bd854549cb3f74e4c0d3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80845492"
---
# <a name="diskpart-scripts-and-examples"></a>Scripts et exemples DiskPart

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Utilisez DiskPart `/s` pour exécuter des scripts qui automatisent les tâches liées aux disques, telles que la création de volumes ou la conversion de disques en disques dynamiques. L’écriture de scripts pour ces tâches est utile si vous déployez Windows à l’aide d’une installation sans assistance ou de l’outil Sysprep, qui ne prend pas en charge la création de volumes autres que le volume de démarrage.  
  
-   Pour créer un script DiskPart, créez un fichier texte qui contient les commandes DiskPart que vous souhaitez exécuter, avec une commande par ligne et aucune ligne vide. Vous pouvez commencer une ligne par `rem` pour faire de la ligne un commentaire.  
  
    par exemple, voici un script qui efface un disque, puis crée une partition de 300 Mo pour l’environnement de récupération Windows :  
  
    ```  
    select disk 0  
    clean  
    convert gpt  
    create partition primary size=300  
    format quick fs=ntfs label=Windows RE tools  
    assign letter=T  
    ```  
  
-   Pour exécuter un script DiskPart, à l’invite de commandes, tapez la commande suivante, où *scriptname* est le nom du fichier texte qui contient votre script.  
  
    ```  
    diskpart /s scriptname.txt  
    ```  
  
-   Pour rediriger la sortie de script de DiskPart vers un fichier, tapez la commande suivante, où *logfile* est le nom du fichier texte dans lequel DiskPart écrit sa sortie.  
  
    ```  
    diskpart /s scriptname.txt > logfile.txt  
    ```  
  
> [!IMPORTANT]  
> Lorsque vous utilisez la commande **diskpart** dans le cadre d’un script, nous vous recommandons d’effectuer toutes les opérations DiskPart ensemble dans le cadre d’un script DiskPart unique. Vous pouvez exécuter des scripts DiskPart consécutifs, mais vous devez autoriser au moins 15 secondes entre chaque script pour un arrêt complet de l’exécution précédente avant d’exécuter à nouveau la commande **diskpart** dans des scripts successifs. Dans le cas contraire, les scripts successifs risquent d’échouer. Vous pouvez ajouter une pause entre des scripts DiskPart consécutifs en ajoutant la commande `timeout /t 15` à votre fichier de commandes avec vos scripts DiskPart.  
  
Au démarrage de DiskPart, la version de DiskPart et le nom de l’ordinateur s’affichent à l’invite de commandes. Par défaut, si DiskPart rencontre une erreur lors de la tentative d’exécution d’une tâche de script, DiskPart arrête le traitement du script et affiche un code d’erreur \(, sauf si vous avez spécifié le paramètre **noerr**\). Toutefois, DiskPart retourne toujours des erreurs lorsqu’il rencontre des erreurs de syntaxe, que vous ayez utilisé ou non le paramètre **noerr** . Le paramètre **noerr** vous permet d’effectuer des tâches utiles, telles que l’utilisation d’un seul script pour supprimer toutes les partitions sur tous les disques, quel que soit le nombre total de disques.  
  
## <a name="additional-references"></a>Références supplémentaires
  
- [Exemple : configurer UEFI\/des partitions de disque dur basées sur des\-GPT à l’aide de Windows PE et de DiskPart](https://technet.microsoft.com/library/hh825686.aspx)  
- [Exemple : configurer le BIOS\/les partitions de disque dur\-basées sur un disque dur à l’aide de Windows PE et de DiskPart](https://technet.microsoft.com/library/hh825677.aspx)  
- [Applets de commande de stockage dans Windows PowerShell](https://technet.microsoft.com/library/hh848705.aspx)  
  

