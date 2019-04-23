---
title: WinSAT mfmedia
description: Commandes Windows
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 09a3b3dd-f746-4e6e-b684-76a9bde0c78d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 69ce4fac127a6af8a94f3800d62c45989cf7020b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845430"
---
# <a name="winsat-mfmedia"></a>WinSAT mfmedia



Mesure les performances de la vidéo de décodage (lecture) à l’aide de l’infrastructure de Media Foundation.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
winsat mfmedia <parameters>
```

## <a name="parameters"></a>Paramètres

|Paramètres|Description|
|----------|-----------|
|-d’entrée \<nom de fichier >|Obligatoire : Spécifiez le fichier contenant le clip vidéo pour être lu ou codé. Le fichier peut être dans n’importe quel format qui peut être affiché par Media Foundation.|
|-dumpgraph|Spécifiez que le graphique de filtre doit être enregistré dans un fichier GraphEdit-compatible avant le démarrage de l’évaluation.|
|-ns|Spécifier que le graphique de filtre doit s’exécuter à la vitesse de lecture normale du fichier d’entrée. Par défaut, le graphique de filtre s’exécute aussi rapidement que possible, en ignorant les durées de présentation.|
|-lire|Exécution de l’évaluation de décoder mode- and -play les fourni le contenu audio dans le fichier spécifié dans **-d’entrée** à l’aide de l’appareil de DirectSound par défaut. Par défaut, la lecture audio est désactivée.|
|-nopmp|Ne faites pas utiliser du Media Foundation Protected Media Pipeline (MFPMP) processus pendant l’évaluation.|
|-pmp|Toujours exploiter la MFPMP processus pendant l’évaluation.</br>Remarque: Si **- pmp** ou **- nopmp** n’est pas spécifié, MFPMP sera utilisé uniquement lorsque cela est nécessaire.|
|-v|Envoyer la sortie détaillée dans STDOUT, y compris les informations d’état et la progression. Des erreurs seront également être écrites dans la fenêtre de commande.|
|xml - \<nom de fichier >|Enregistrez la sortie de l’évaluation en tant que le fichier XML spécifié. Si le fichier spécifié existe, il sera remplacé.|
|-idiskinfo|Enregistrer les informations sur les volumes physiques et les disques logiques dans le cadre de la  **\<SystemConfig >** section dans la sortie XML.|
|-iguid|Créer un identificateur global unique (GUID) dans le fichier de sortie XML.|
|-Notez « Remarque text »|Ajoutez le texte de note dans le  **\<Remarque >** section dans le fichier de sortie XML.|
|-icn|Inclure le nom de l’ordinateur local dans le fichier de sortie XML.|
|-eef|Énumérer les informations système supplémentaires dans le fichier de sortie XML.|

## <a name="BKMK_examples"></a>Exemples

-   L’exemple suivant exécute l’évaluation avec le fichier d’entrée qui est utilisé pendant un **winsat formel** évaluation, sans employant le Media Foundation Protected Media Pipeline (MFPMP), sur un ordinateur où c:\windows est l’emplacement de le dossier Windows.  
    ```
    winsat mfmedia -input c:\windows\performance\winsat\winsat.wmv -nopmp
    ```

## <a name="remarks"></a>Notes

-   L’appartenance au groupe Administrateurs local, ou équivalent, est la condition minimale requise pour utiliser **winsat**. La commande doit être exécutée à partir d’une fenêtre d’invite de commandes avec élévation de privilèges.
-   Pour ouvrir une fenêtre d’invite de commandes avec élévation de privilèges, cliquez sur **Démarrer**, cliquez sur **Accessoires**, avec le bouton droit **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.

#### <a name="additional-references"></a>Références supplémentaires

