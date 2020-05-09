---
title: scripts et exemples DiskPart
description: Rubrique de référence sur les scripts diskpart et des exemples sur l’automatisation des tâches liées aux disques, telles que la création de volumes ou la conversion de disques en disques dynamiques.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 319c0795-11df-47c8-b203-eadb0577ee0d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f3e781e49aa978288de45da90224a3f1c2b247b1
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992488"
---
# <a name="diskpart-scripts-and-examples"></a>scripts et exemples DiskPart

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Utilisez `diskpart /s` pour exécuter des scripts qui automatisent les tâches liées aux disques, telles que la création de volumes ou la conversion de disques en disques dynamiques. L’écriture de scripts pour ces tâches est utile si vous déployez Windows à l’aide d’une installation sans assistance ou de l’outil Sysprep, qui ne prend pas en charge la création de volumes autres que le volume de démarrage.

Pour créer un script DiskPart, créez un fichier texte qui contient les commandes DiskPart que vous souhaitez exécuter, avec une commande par ligne et aucune ligne vide. Vous pouvez démarrer une ligne avec `rem` pour transformer la ligne en commentaire. Par exemple, voici un script qui efface un disque, puis crée une partition de 300 Mo pour l’environnement de récupération Windows :

    ```
    select disk 0
    clean
    convert gpt
    create partition primary size=300
    format quick fs=ntfs label=Windows RE tools
    assign letter=T
    ```

## <a name="examples"></a>Exemples

- Pour exécuter un script DiskPart, à l’invite de commandes, tapez la commande suivante, où *scriptname* est le nom du fichier texte qui contient votre script :

    ```
    diskpart /s scriptname.txt
    ```

- Pour rediriger la sortie de script de DiskPart vers un fichier, tapez la commande suivante, où *logfile* est le nom du fichier texte dans lequel DiskPart écrit sa sortie :

    ```
    diskpart /s scriptname.txt > logfile.txt
    ```

### <a name="remarks"></a>Notes 

- Lorsque vous utilisez la commande **diskpart** dans le cadre d’un script, nous vous recommandons d’effectuer toutes les opérations DiskPart ensemble dans le cadre d’un script DiskPart unique. Vous pouvez exécuter des scripts DiskPart consécutifs, mais vous devez autoriser au moins 15 secondes entre chaque script pour un arrêt complet de l’exécution précédente avant d’exécuter à nouveau la commande **diskpart** dans des scripts successifs. Dans le cas contraire, les scripts successifs risquent d’échouer. Vous pouvez ajouter une pause entre des scripts DiskPart consécutifs en `timeout /t 15` ajoutant la commande à votre fichier de commandes et à vos scripts Diskpart.

- Au démarrage de DiskPart, la version de DiskPart et le nom de l’ordinateur s’affichent à l’invite de commandes. Par défaut, si DiskPart rencontre une erreur lors de la tentative d’exécution d’une tâche de script, DiskPart arrête le traitement du script et affiche un code d’erreur (sauf si vous avez spécifié le paramètre **noerr** ). Toutefois, DiskPart retourne toujours des erreurs lorsqu’il rencontre des erreurs de syntaxe, que vous ayez utilisé ou non le paramètre **noerr** . Le paramètre **noerr** vous permet d’effectuer des tâches utiles, telles que l’utilisation d’un seul script pour supprimer toutes les partitions sur tous les disques, quel que soit le nombre total de disques.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [Exemple : configurer des\/partitions\-de disque dur UEFI basés sur le modèle de configuration de disque à l’aide de Windows PE et de DiskPart](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh825686(v=win.10))

- [Exemple : configurer des\/partitions\-de disque dur basées sur un MBR BIOS à l’aide de Windows PE et de DiskPart](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh825677(v=win.10))

- [Applets de commande de stockage dans Windows PowerShell](https://docs.microsoft.com/powershell/module/storage/?view=win10-ps)
