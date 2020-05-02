---
title: typeperf
description: Rubrique de référence pour typeperf, qui écrit les données de performances dans la fenêtre commande ou dans un fichier journal.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0c7ca89a-03b3-4626-afcf-ef8565e90043
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a996abc1417fdb6aa50370a942433716d8df2cbe
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721206"
---
# <a name="typeperf"></a>typeperf

La commande **typeperf** écrit des données de performances dans la fenêtre commande ou dans un fichier journal. Pour arrêter **typeperf**, appuyez sur Ctrl + C.

## <a name="syntax"></a>Syntaxe

```
typeperf <counter [counter ...]> [options]
typeperf -cf <filename> [options]
typeperf -q [object] [options]
typeperf -qx [object] [options]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<compteur [compteur [...]] >|Spécifie les compteurs de performance à surveiller.|

> [!NOTE]
> >de compteur est le nom complet d’un compteur de performance au * \\ \\format Computer\Object (instance) \Counter* , tel que ** \\ \\Server1\Processor (\% 0) heure utilisateur**. ** \<**

## <a name="options"></a>Options

|                   Option                   |                                                         Description                                                          |
|--------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
|                     -?                     |                                               Affiche l’aide contextuelle.                                               |
| -f \<CSV&verbar;&verbar;bin&verbar;SQL> CSV |                                    Spécifie le format du fichier de sortie. La valeur par défaut est CSV.                                     |
|              -CF \<nom du fichier>               |              Spécifie un fichier contenant une liste de compteurs de performances à surveiller, avec un compteur par ligne.               |
|             -Si < [[hh :] mm :] SS>             |                                  Spécifie l’intervalle d’échantillonnage. La valeur par défaut est d’une seconde.                                   |
|               -o \<filename>               |     Spécifie le chemin d’accès pour le fichier de sortie ou la base de données SQL. La valeur par défaut est STDOUT (écrit dans la fenêtre commande).      |
|                -q [objet]                 | Affiche la liste des compteurs installés (aucune instance). Pour répertorier les compteurs d’un objet, incluez le nom de l’objet. \*\*\*TELS |
|                -QX [objet]                |        Affiche la liste des compteurs installés avec des instances de. Pour répertorier les compteurs d’un objet, incluez le nom de l’objet.        |
|               -SC \<samples>               |             Spécifie le nombre d’échantillons à collecter. La valeur par défaut consiste à collecter les données jusqu’à ce que CTRL + C soit enfoncé.              |
|            -nom \<de fichier de configuration>             |                                    Spécifie un fichier de paramètres contenant les options de commande.                                     |
|            -s \<computer_name>             |                   Spécifie un ordinateur distant à surveiller si aucun ordinateur n’est spécifié dans le chemin d’accès au compteur.                    |
|                     -y                     |                                        Répondez oui à toutes les questions sans demander confirmation.                                        |

## <a name="examples"></a>Exemples

- Pour écrire les valeurs du temps processeur du processeur de compteur ** \\ \\de performance (_Total\% )** de l’ordinateur local dans la fenêtre de commande à un intervalle d’échantillonnage par défaut de 1 seconde jusqu’à ce que CTRL + C soit enfoncé.  
  ```
  typeperf \Processor(_Total)\% Processor Time
  ```  
- Pour écrire les valeurs de la liste des compteurs dans le fichier **Counters. txt** dans le fichier délimité par des tabulations **domaine2. TSV** à un intervalle d’échantillonnage de 5 secondes jusqu’à ce que les exemples 50 aient été collectés.  
  ```
  typeperf -cf counters.txt -si 5 -sc 50 -f TSV -o domain2.tsv
  ```  
- Pour interroger les compteurs installés avec des instances de l’objet de compteur **PhysicalDisk** et écrit la liste résultante dans le fichier **Counters. txt**.  
  ```
  typeperf -qx PhysicalDisk -o counters.txt
  ```
