---
title: mfmedia WinSAT
description: Référence pour WinSAT mfmedia, qui mesure les performances du décodage vidéo (lecture) à l’aide de l’infrastructure Media Foundation.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 09a3b3dd-f746-4e6e-b684-76a9bde0c78d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3a03304f4df27dc7fdf9bb0af5e2f4d6f2b9c98d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720664"
---
# <a name="winsat-mfmedia"></a>mfmedia WinSAT



Mesure les performances du décodage vidéo (lecture) à l’aide de l’infrastructure Media Foundation.



## <a name="syntax"></a>Syntaxe

```
winsat mfmedia <parameters>
```

### <a name="parameters"></a>Paramètres

|Paramètres|Description|
|----------|-----------|
|-nom \<du fichier d’entrée>|Obligatoire : spécifiez le fichier contenant le clip vidéo à lire ou à encoder. Le fichier peut être dans n’importe quel format qui peut être rendu par Media Foundation.|
|-dumpgraph|Spécifiez que le graphique de filtre doit être enregistré dans un fichier compatible GraphEdit avant le démarrage de l’évaluation.|
|-NS|Spécifiez que le graphique de filtre doit s’exécuter à la vitesse de lecture normale du fichier d’entrée. Par défaut, le graphique de filtre s’exécute aussi rapidement que possible, en ignorant les durées de présentation.|
|-Play|Exécutez l’évaluation en mode décode et lisez tout contenu audio fourni dans le fichier spécifié en **entrée** à l’aide de l’appareil DirectSound par défaut. Par défaut, la lecture audio est désactivée.|
|-nopmp|N’utilisez pas Media Foundation le processus MFPMP (Protected Media Pipeline) pendant l’évaluation.|
|-PMP|Utilisez toujours le processus MFPMP pendant l’évaluation.</br>Remarque : si **-PMP** ou **-nopmp** n’est pas spécifié, MFPMP est utilisé uniquement lorsque cela est nécessaire.|
|-v|Envoie la sortie détaillée à STDOUT, y compris les informations d’État et de progression. Toutes les erreurs sont également écrites dans la fenêtre de commande.|
|-nom \<du fichier XML>|Enregistre la sortie de l’évaluation en tant que fichier XML spécifié. Si le fichier spécifié existe, il est remplacé.|
|-idiskinfo|Enregistrer les informations sur les volumes physiques et les disques logiques dans le cadre de la ** \<section SystemConfig>** de la sortie XML.|
|-iguid|Créez un identificateur global unique (GUID) dans le fichier de sortie XML.|
|-Remarque : texte|Ajoutez le texte de la note à la ** \<section note>** du fichier de sortie XML.|
|-icn|Incluez le nom de l’ordinateur local dans le fichier de sortie XML.|
|-EEF|Énumérer les informations système supplémentaires dans le fichier de sortie XML.|

## <a name="examples"></a>Exemples

- Pour exécuter l’évaluation avec le fichier d’entrée qui est utilisé pendant une évaluation **formelle WinSAT** , sans utiliser le pipeline MFPMP protégé Media Foundation, sur un ordinateur où c:\Windows est l’emplacement du dossier Windows.  
  ```
  winsat mfmedia -input c:\windows\performance\winsat\winsat.wmv -nopmp
  ```

## <a name="remarks"></a>Notes 

-   L’appartenance au groupe Administrateurs local, ou équivalent, est la condition minimale requise pour utiliser **WinSAT**. La commande doit être exécutée à partir d’une fenêtre d’invite de commandes avec élévation de privilèges.
-   Pour ouvrir une fenêtre d’invite de commandes avec élévation de privilèges, cliquez sur **Démarrer**, sur **accessoires**, cliquez avec le bouton droit sur **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.

## <a name="additional-references"></a>Références supplémentaires

