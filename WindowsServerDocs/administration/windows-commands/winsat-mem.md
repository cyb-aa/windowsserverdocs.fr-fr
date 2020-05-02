---
title: mémoire WinSAT
description: Rubrique de référence pour le MEM WinSAT, qui teste la bande passante de mémoire système de manière à refléter une grande quantité de mémoire pour les copies de mémoire tampon, comme c’est le cas pour le traitement multimédia.
ms.prod: windows-server
ms.technology: manage-windows-commands
winms.topic: article
ms.assetid: cda017bf-6193-43c1-b71f-e379c23e1152
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 158757d4449b288469b52d2d62e7cfb53d57a7d2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720670"
---
# <a name="winsat-mem"></a>mémoire WinSAT



Teste la bande passante de la mémoire système de manière à refléter une grande quantité de mémoire pour les copies de mémoire tampon, comme c’est le cas pour le traitement multimédia.



## <a name="syntax"></a>Syntaxe

```
winsat mem <parameters>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|vers le haut|Forcer le test de la mémoire avec un seul thread. La valeur par défaut consiste à exécuter un thread par processeur physique ou noyau.|
|-RN|Spécifiez que les threads de l’évaluation doivent s’exécuter à la priorité normale. La valeur par défaut est exécuter à la priorité 15.|
|-NC|Spécifiez que l’évaluation doit allouer de la mémoire et la marquer comme non mise en cache. Cela signifie que les caches du processeur seront ignorés pour les opérations de copie. La valeur par défaut est l’exécution dans l’espace mis en cache.|
|-do \<n>|Spécifiez la distance, en octets, entre la fin de la mémoire tampon source et le début de la mémoire tampon de destination. La valeur par défaut est 64 octets. Le décalage de destination maximal autorisé est de 16 Mo. Si vous spécifiez un décalage de destination non valide, une erreur se produit.</br>Remarque : zéro est une valeur valide pour ** \<n>**, mais les nombres négatifs ne le sont pas.|
|-menthe \<n>|Spécifiez le temps d’exécution minimal, en secondes, pour l’évaluation. La valeur par défaut est 2,0. La valeur minimale est 1,0. La valeur maximale est 30,0.</br>Remarque : la spécification d’une valeur **-menthe** supérieure à la valeur **-maxt** lorsque les deux paramètres sont utilisés en association génère une erreur.|
|-maxt \<n>|Spécifiez la durée maximale d’exécution (en secondes) pour l’évaluation. La valeur par défaut est 5,0. La valeur minimale est 1,0. La valeur maximale est 30,0. S’il est utilisé en association avec le paramètre **-menthe** , l’évaluation commence à effectuer des contrôles statistiques périodiques de ses résultats après la période spécifiée dans **-menthe**. Si les contrôles statistiques réussissent, l’évaluation se termine avant la période spécifiée dans **-maxt** . Si l’évaluation s’exécute pendant la période spécifiée dans **-maxt** sans satisfaire aux contrôles statistiques, l’évaluation se termine à ce moment-là et retourne les résultats qu’elle a collectés.|
|-bufferSize \<n>|Spécifiez la taille de la mémoire tampon que le test de copie de mémoire doit utiliser. Deux fois ce montant est alloué par UC, ce qui détermine la quantité de données copiées d’une mémoire tampon à une autre. La valeur par défaut est 16 Mo. Cette valeur est arrondie à la limite de 4 Ko la plus proche. La valeur maximale est de 32 Mo. La valeur minimale est de 4 Ko. Si vous spécifiez une taille de mémoire tampon non valide, une erreur se produit.|
|-v|Envoie la sortie détaillée à STDOUT, y compris les informations d’État et de progression. Toutes les erreurs sont également écrites dans la fenêtre de commande.|
|-nom \<du fichier XML>|Enregistre la sortie de l’évaluation en tant que fichier XML spécifié. Si le fichier spécifié existe, il est remplacé.|
|-idiskinfo|Enregistrer les informations sur les volumes physiques et les disques logiques dans le cadre de la ** \<section SystemConfig>** de la sortie XML.|
|-iguid|Créez un identificateur global unique (GUID) dans le fichier de sortie XML.|
|-Remarque : texte|Ajoutez le texte de la note à la ** \<section note>** du fichier de sortie XML.|
|-icn|Incluez le nom de l’ordinateur local dans le fichier de sortie XML.|
|-EEF|Énumérer les informations système supplémentaires dans le fichier de sortie XML.|

## <a name="examples"></a>Exemples

- Pour exécuter l’évaluation pour un minimum de 4 secondes et pas plus de 12 secondes, en utilisant une taille de mémoire tampon de 32 Mo et en enregistrant les résultats au format XML dans le fichier **memtest. xml**.  
  ```
  winsat mem -mint 4.0 -maxt 12.0 -buffersize 32MB -xml memtest.xml
  ```

## <a name="remarks"></a>Notes 

-   L’appartenance au groupe Administrateurs local, ou équivalent, est la condition minimale requise pour utiliser **WinSAT**. La commande doit être exécutée à partir d’une fenêtre d’invite de commandes avec élévation de privilèges.
-   Pour ouvrir une fenêtre d’invite de commandes avec élévation de privilèges, cliquez sur **Démarrer**, sur **accessoires**, cliquez avec le bouton droit sur **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.

## <a name="additional-references"></a>Références supplémentaires

