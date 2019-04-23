---
title: WinSAT mem
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cda017bf-6193-43c1-b71f-e379c23e1152
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7dc041c8a6c85b46913904ce80494f54cb818ca8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878340"
---
# <a name="winsat-mem"></a>WinSAT mem



Copie de bande passante de la mémoire système de tests de manière réfléchissante de grande mémoire à la mémoire tampon, sont utilisés dans le traitement multimédia.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
winsat mem <parameters>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|-haut|Forcer le test avec un seul thread de mémoire. La valeur par défaut consiste à exécuter un thread par physique du processeur ou core.|
|-rn|Spécifier que les threads de l’évaluation doivent s’exécuter avec une priorité normale. La valeur par défaut consiste à exécuter à la priorité de 15.|
|-nc|Spécifier que l’évaluation doit allouer de la mémoire et le marquer comme non mis en cache. Cela signifie que les caches du processeur seront ignorés pour les opérations de copie. La valeur par défaut consiste à exécuter dans l’espace de mise en cache.|
|-do \<n>|Spécifiez la distance, en octets, entre la fin de la mémoire tampon source et le début de la mémoire tampon de destination. La valeur par défaut est de 64 octets. L’offset de destination autorisée maximale est de 16 Mo. Spécification d’un décalage de destination non valide entraîne une erreur.</br>Remarque: Zéro est une valeur valide pour  **\<n >**, mais les nombres négatifs ne sont pas.|
|-mint \<n>|Spécifiez la valeur minimale en secondes pour l’évaluation de la durée d’exécution. La valeur par défaut est 2.0. La valeur minimale est 1.0. La valeur maximale est de 30,0.</br>Remarque: En spécifiant un **-mint** valeur supérieure à la **- maxt** valeur lorsque les deux paramètres sont utilisés conjointement entraîne une erreur.|
|-maxt \<n>|Spécifiez la durée maximale d’exécution en secondes pour l’évaluation. La valeur par défaut est 5.0. La valeur minimale est 1.0. La valeur maximale est de 30,0. Si utilisé conjointement avec le **-mint** paramètre, l’évaluation commence à faire des statistiques des vérifications périodiques de ses résultats après la période de temps spécifiée dans **-mint**. Si les statistiques vérifications réussissent, l’évaluation se termine avant la période de temps spécifiée dans **- maxt** s’est écoulé. Si l’évaluation s’exécute pour la période de temps spécifiée dans **- maxt** sans qui satisfont les vérifications de statistiques, puis l’évaluation se terminer à ce moment-là et les résultats qu’il a collectées.|
|-buffersize \<n >|Spécifiez la taille de mémoire tampon que le test de copie de mémoire doit utiliser. Deux fois cette quantité sera allouée par UC, qui détermine la quantité de données copiées à partir d’une mémoire tampon à un autre. La valeur par défaut est de 16 Mo. Cette valeur est arrondie à la limite de 4 Ko le plus proche. La valeur maximale est de 32 Mo. La valeur minimale est de 4 Ko. En spécifiant une taille de tampon non valide entraîne une erreur.|
|-v|Envoyer la sortie détaillée dans STDOUT, y compris les informations d’état et la progression. Des erreurs seront également être écrites dans la fenêtre de commande.|
|xml - \<nom de fichier >|Enregistrez la sortie de l’évaluation en tant que le fichier XML spécifié. Si le fichier spécifié existe, il sera remplacé.|
|-idiskinfo|Enregistrer les informations sur les volumes physiques et les disques logiques dans le cadre de la  **\<SystemConfig >** section dans la sortie XML.|
|-iguid|Créer un identificateur global unique (GUID) dans le fichier de sortie XML.|
|-Notez « Remarque text »|Ajoutez le texte de note dans le  **\<Remarque >** section dans le fichier de sortie XML.|
|-icn|Inclure le nom de l’ordinateur local dans le fichier de sortie XML.|
|-eef|Énumérer les informations système supplémentaires dans le fichier de sortie XML.|

## <a name="BKMK_examples"></a>Exemples

-   L’exemple suivant exécute l’évaluation pour un minimum de 4 secondes et pas plus de 12 secondes, à l’aide d’une taille de mémoire tampon de 32 Mo et enregistrer les résultats au format XML dans le fichier **memtest.xml**.  
    ```
    winsat mem -mint 4.0 -maxt 12.0 -buffersize 32MB -xml memtest.xml
    ```

## <a name="remarks"></a>Notes

-   L’appartenance au groupe Administrateurs local, ou équivalent, est la condition minimale requise pour utiliser **winsat**. La commande doit être exécutée à partir d’une fenêtre d’invite de commandes avec élévation de privilèges.
-   Pour ouvrir une fenêtre d’invite de commandes avec élévation de privilèges, cliquez sur **Démarrer**, cliquez sur **Accessoires**, avec le bouton droit **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.

#### <a name="additional-references"></a>Références supplémentaires

