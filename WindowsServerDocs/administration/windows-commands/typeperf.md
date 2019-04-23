---
title: typeperf
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c7ca89a-03b3-4626-afcf-ef8565e90043
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1337066f547367b381a531dbab6627ad78d280ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848450"
---
# <a name="typeperf"></a>typeperf



Le **typeperf** commande écrit les données de performances à la fenêtre de commande ou dans un fichier journal. Pour arrêter **typeperf**, appuyez sur CTRL + C.

Pour obtenir des exemples montrant comment utiliser **typeperf**, consultez [exemples](#BKMK_EXAMPLES).

## <a name="syntax"></a>Syntaxe

```
typeperf <counter [counter ...]> [options]
typeperf -cf <filename> [options]
typeperf -q [object] [options]
typeperf -qx [object] [options]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<compteur [compteur [...]] >|Spécifie les compteurs de performance à surveiller.|

> [!NOTE]
> **\<compteur >** est le nom complet d’un compteur de performances dans  *\\ \\Computer\Object (Instance) \Counter* mettre en forme, telles que  **\\ \\Server1\ Processor(0)\% temps utilisateur**.

## <a name="options"></a>Options

|Option|Description|
|---------|-----------|
|-?|Affiche l’aide contextuelle.|
|-f \<CSV&verbar;TSV&verbar;BIN&verbar;SQL>|Spécifie le format de fichier de sortie. La valeur par défaut est CSV.|
|-cf \<filename>|Spécifie un fichier contenant une liste des compteurs de performance à surveiller, avec un compteur par ligne.|
|-si < [[hh :] mm :] ss >|Spécifie l’intervalle d’échantillonnage. La valeur par défaut est une seconde.|
|-o \<filename>|Spécifie le chemin d’accès pour le fichier de sortie ou la base de données SQL. La valeur par défaut est STDOUT (écrite dans la fenêtre de commande).|
|-q [object]|Afficher la liste des compteurs installés (aucune instance). Pour répertorier les compteurs d’un objet, incluez le nom de l’objet. EXEMPLE|
|-qx [object]|Afficher la liste des compteurs installés avec des instances. Pour répertorier les compteurs d’un objet, incluez le nom de l’objet.|
|-sc \<samples>|Spécifie le nombre d’échantillons à collecter. La valeur par défaut consiste à collecter des données jusqu'à ce que vous appuyez sur CTRL + C.|
|-config \<filename>|Spécifie un fichier de paramètres contenant les options de commande.|
|-s \<computer_name>|Spécifie un ordinateur distant à surveiller si aucun ordinateur n’est spécifié dans le chemin de compteur.|
|-y|Répondez Oui à toutes les questions sans demander confirmation.|

## <a name="BKMK_EXAMPLES"></a>Exemples

-   L’exemple suivant écrit les valeurs de compteur de performance de l’ordinateur local  **\\ \\processeur (_Total)\% temps processeur** à la fenêtre de commande à un intervalle d’échantillonnage par défaut de 1 seconde jusqu'à ce que CTRL + C est enfoncé.  
    ```
    typeperf "\Processor)_Total)\% Processor Time"
    ```  
-   L’exemple suivant écrit les valeurs de la liste des compteurs dans le fichier **counters.txt** dans le fichier délimité par tabulation **domaine2.tsv** selon un intervalle d’échantillonnage de 5 secondes jusqu'à ce que 50 échantillons ont été collectés.  
    ```
    typeperf -cf counters.txt -si 5 -sc 50 -f TSV -o domain2.tsv
    ```  
-   L’exemple suivant interroge les compteurs installés avec des instances de l’objet de compteur **PhysicalDisk** et écrit la liste résultante dans le fichier **counters.txt**.  
    ```
    typeperf -qx PhysicalDisk -o counters.txt
    ```
