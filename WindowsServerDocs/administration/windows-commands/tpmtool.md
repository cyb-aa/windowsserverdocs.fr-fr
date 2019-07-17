---
title: tpmtool
description: Rubrique de commandes de Windows pour tpmtool - Obtient des informations sur le Module de plateforme sécurisée.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
author: ashleytqy
ms.author: ashleytqy
manager: ronaldai
ms.date: 05/07/2019
ms.openlocfilehash: e125dbf6127b92c91e041c431f1e462e1f884168
ms.sourcegitcommit: 0ff812a80f654fa2c35b1632524e27841eca75c7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68230866"
---
# <a name="tpmtool"></a>tpmtool

Cet utilitaire peut être utilisé pour obtenir des informations le [du Module de plateforme sécurisée (TPM)](https://docs.microsoft.com/windows/security/information-protection/tpm/trusted-platform-module-overview).

>[!IMPORTANT]
>Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#tpmtool_examples).

## <a name="syntax"></a>Syntaxe

```
tpmtool /parameter [<arguments>]
```
## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|getdeviceinformation|Affiche les informations de base du module TPM. Vous pouvez trouver la signification des valeurs d’indicateur d’informations [ici](https://docs.microsoft.com/windows/desktop/SecProv/win32-tpm-isreadyinformation#parameters).|
|GatherLogs [chemin d’accès de répertoire de sortie]|Collecte des journaux de module de plateforme sécurisée et les place dans le répertoire spécifié. Si ce répertoire n’existe pas, il est créé. Par défaut, ils sont placés dans le répertoire actif. Les fichiers possible générés sont : </br>-TpmEvents.evtx</br>-TpmInformation.txt</br>-SRTMBoot.dat</br>-SRTMResume.dat</br>-DRTMBoot.dat</br>-DRTMResume.dat</br>|
|drivertracing [Démarrer / arrêter]|Démarrer / arrêter la collecte des traces de pilote de module de plateforme sécurisée. Le journal des traces, TPMTRACE.etl, sera généré et placé dans le répertoire actif.|
|parsetcglogs [-valider (-v)]|Affiche le journal TCG analysé, également connu sous le Windows Boot Configuration journal (WBCL). Vous trouverez les descriptions d’événement plus récentes sur le [site Web TCG](https://trustedcomputinggroup.org/resource/pc-client-specific-platform-firmware-profile-specification/), sous **Descriptions d’événement**. Si le `-validate` l’indicateur est défini, valide le fait que les valeurs de Registre de Configuration plateforme (PCR) sur le module de plateforme sécurisée correspondent aux valeurs dans le journal.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="tpmtool_examples"></a>Exemples

Pour afficher les informations de base du module TPM, tapez :
```
tpmtool getdeviceinformation
```
Pour collecter les journaux du module de plateforme sécurisée et placez-les dans le répertoire actif, tapez :
```
tpmtool gatherlogs
```
Pour collecter les journaux de module de plateforme sécurisée et de les placer dans `C:\Users\Public`, type :
```
tpmtool gatherlogs C:\Users\Public
```
Pour collecter des traces de pilote de module de plateforme sécurisée, tapez :
```
tpmtool drivertracing start
# Run scenario
tpmtool drivertracing stop
```
Pour analyser le journal TCG :
```
tpmtool parsetcglogs
```
Pour analyser le journal TCG et valider les PCR :
```
tpmtool parsetcglogs -validate
```

## <a name="decoding-error-codes"></a>Décodage des Codes d’erreur

Codes d’erreur spécifique au module de plateforme sécurisée sont documentées [ici](https://docs.microsoft.com/windows/desktop/com/com-error-codes-6).
