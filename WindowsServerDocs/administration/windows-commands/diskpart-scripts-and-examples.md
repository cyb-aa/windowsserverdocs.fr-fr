---
title: Scripts et exemples DiskPart
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 9c40ce79664795297af4369e35cbda7422617e6e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377852"
---
# <a name="diskpart-scripts-and-examples"></a>Scripts et exemples DiskPart

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Utilisez DiskPart `/s` pour exécuter des scripts qui automatisent les tâches Disk @ no__t-1related, telles que la création de volumes ou la conversion de disques en disques dynamiques. L’écriture de scripts pour ces tâches est utile si vous déployez Windows à l’aide d’une installation sans assistance ou de l’outil Sysprep, qui ne prend pas en charge la création de volumes autres que le volume de démarrage.  
  
-   Pour créer un script DiskPart, créez un fichier texte qui contient les commandes DiskPart que vous souhaitez exécuter, avec une commande par ligne et aucune ligne vide. Vous pouvez démarrer une ligne avec `rem` pour faire de la ligne un commentaire.  
  
    par exemple, voici un script qui efface un disque, puis crée une partition de 300 Mo pour l’environnement de récupération Windows :  
  
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
  
-   Pour rediriger la sortie de script de DiskPart vers un fichier, tapez la commande suivante, où *logfile* est le nom du fichier texte dans lequel DiskPart écrit sa sortie.  
  
    ```  
    diskpart /s scriptname.txt > logfile.txt  
    ```  
  
> [!IMPORTANT]  
> Lorsque vous utilisez la commande **diskpart** dans le cadre d’un script, nous vous recommandons d’effectuer toutes les opérations DiskPart ensemble dans le cadre d’un script DiskPart unique. Vous pouvez exécuter des scripts DiskPart consécutifs, mais vous devez autoriser au moins 15 secondes entre chaque script pour un arrêt complet de l’exécution précédente avant d’exécuter à nouveau la commande **diskpart** dans des scripts successifs. Dans le cas contraire, les scripts successifs risquent d’échouer. Vous pouvez ajouter une pause entre des scripts DiskPart consécutifs en ajoutant la commande `timeout /t 15` à votre fichier de commandes avec vos scripts DiskPart.  
  
Au démarrage de DiskPart, la version de DiskPart et le nom de l’ordinateur s’affichent à l’invite de commandes. Par défaut, si DiskPart rencontre une erreur lors de la tentative d’exécution d’une tâche de script, DiskPart arrête le traitement du script et affiche un code d’erreur \(unless vous avez spécifié le paramètre **noerr** @ no__t-2. Toutefois, DiskPart retourne toujours des erreurs lorsqu’il rencontre des erreurs de syntaxe, que vous ayez utilisé ou non le paramètre **noerr** . Le paramètre **noerr** vous permet d’effectuer des tâches utiles, telles que l’utilisation d’un seul script pour supprimer toutes les partitions sur tous les disques, quel que soit le nombre total de disques.  
  
## <a name="see-also"></a>Voir aussi  
[Exemple Configurer UEFI @ no__t-0gpt @ no__t-1Based partitions de disque dur à l’aide de Windows PE et de DiskPart @ no__t-2  
[Exemple Configurer le BIOS @ no__t-0MBR @ no__t-1Based partitions de disque dur à l’aide de Windows PE et de DiskPart @ no__t-2  
[Applets de commande de stockage dans Windows PowerShell](https://technet.microsoft.com/library/hh848705.aspx)  
  

