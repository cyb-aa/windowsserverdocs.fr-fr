---
title: mfmedia WinSAT
description: Commandes Windows pour WinSAT mfmedia, qui mesure les performances du décodage vidéo (lecture) à l’aide de l’infrastructure Media Foundation.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 09a3b3dd-f746-4e6e-b684-76a9bde0c78d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ed3759c741b6f168bc67e8aef3e0b817595cfe4e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829062"
---
# <a name="winsat-mfmedia"></a>mfmedia WinSAT



Mesure les performances du décodage vidéo (lecture) à l’aide de l’infrastructure Media Foundation.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
winsat mfmedia <parameters>
```

### <a name="parameters"></a>Paramètres

|Paramètres|Description|
|----------|-----------|
|-nom du fichier \<d’entrée >|Obligatoire : spécifiez le fichier contenant le clip vidéo à lire ou à encoder. Le fichier peut être dans n’importe quel format qui peut être rendu par Media Foundation.|
|-dumpgraph|Spécifiez que le graphique de filtre doit être enregistré dans un fichier compatible GraphEdit avant le démarrage de l’évaluation.|
|-NS|Spécifiez que le graphique de filtre doit s’exécuter à la vitesse de lecture normale du fichier d’entrée. Par défaut, le graphique de filtre s’exécute aussi rapidement que possible, en ignorant les durées de présentation.|
|-Play|Exécutez l’évaluation en mode décode et lisez tout contenu audio fourni dans le fichier spécifié en **entrée** à l’aide de l’appareil DirectSound par défaut. Par défaut, la lecture audio est désactivée.|
|-nopmp|N’utilisez pas Media Foundation le processus MFPMP (Protected Media Pipeline) pendant l’évaluation.|
|-PMP|Utilisez toujours le processus MFPMP pendant l’évaluation.</br>Remarque : si **-PMP** ou **-nopmp** n’est pas spécifié, MFPMP est utilisé uniquement lorsque cela est nécessaire.|
|-v|Envoie la sortie détaillée à STDOUT, y compris les informations d’État et de progression. Toutes les erreurs sont également écrites dans la fenêtre de commande.|
|-nom de fichier \<XML >|Enregistre la sortie de l’évaluation en tant que fichier XML spécifié. Si le fichier spécifié existe, il est remplacé.|
|-idiskinfo|Enregistrer les informations sur les volumes physiques et les disques logiques dans le cadre de la section **\<SystemConfig >** dans la sortie XML.|
|-iguid|Créez un identificateur global unique (GUID) dans le fichier de sortie XML.|
|-Remarque : texte|Ajoutez le texte de note à la section **\<note >** dans le fichier de sortie XML.|
|-icn|Incluez le nom de l’ordinateur local dans le fichier de sortie XML.|
|-EEF|Énumérer les informations système supplémentaires dans le fichier de sortie XML.|

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

- L’exemple suivant exécute l’évaluation avec le fichier d’entrée utilisé pendant une évaluation **formelle WinSAT** , sans utiliser Media Foundation le pipeline MFPMP protégé, sur un ordinateur où c:\Windows est l’emplacement du dossier Windows.  
  ```
  winsat mfmedia -input c:\windows\performance\winsat\winsat.wmv -nopmp
  ```

## <a name="remarks"></a>Notes

-   L’appartenance au groupe Administrateurs local, ou équivalent, est la condition minimale requise pour utiliser **WinSAT**. La commande doit être exécutée à partir d’une fenêtre d’invite de commandes avec élévation de privilèges.
-   Pour ouvrir une fenêtre d’invite de commandes avec élévation de privilèges, cliquez sur **Démarrer**, sur **accessoires**, cliquez avec le bouton droit sur **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.

## <a name="additional-references"></a>Références supplémentaires

