---
title: tpmtool
description: Rubrique de référence pour tpmtool, qui obtient des informations sur la Module de plateforme sécurisée (TPM).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: ashleytqy
ms.author: ashleytqy
manager: ronaldai
ms.date: 05/07/2019
ms.openlocfilehash: f2e283dd20d22418416958686d77605976923eaf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721329"
---
# <a name="tpmtool"></a>tpmtool

Cet utilitaire peut être utilisé pour obtenir des informations sur le [module de plateforme sécurisée (TPM) (TPM)](https://docs.microsoft.com/windows/security/information-protection/tpm/trusted-platform-module-overview).

>[!IMPORTANT]
>Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft n’offre aucune garantie, expresse ou implicite, concernant les informations fournies ici.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#tpmtool_examples).

## <a name="syntax"></a>Syntaxe

```
tpmtool /parameter [<arguments>]
```
### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|getdeviceinformation|Affiche les informations de base du module de plateforme sécurisée. La signification des valeurs des indicateurs d’informations est disponible [ici](https://docs.microsoft.com/windows/desktop/SecProv/win32-tpm-isreadyinformation#parameters).|
|GatherLogs [chemin du répertoire de sortie]|Collecte les journaux TPM et les place dans le répertoire spécifié. Si ce répertoire n’existe pas, il est créé. Par défaut, ils sont placés dans le répertoire actif. Les fichiers générés sont les suivants : </br>-TpmEvents. evtx</br>-TpmInformation. txt</br>-SRTMBoot. dat</br>-SRTMResume. dat</br>-DRTMBoot. dat</br>-DRTMResume. dat</br>|
|drivertracing [démarrer/arrêter]|Démarrer/arrêter la collecte des traces du pilote du module de plateforme sécurisée. Le journal des traces, TPMTRACE. etl, sera généré et placé dans le répertoire actif.|
|parsetcglogs [-Validate (-v)]|Affiche le journal TCG analysé, également appelé journal de configuration de démarrage Windows (WBCL). Les descriptions d’événements les plus récentes se trouvent sur le [site Web TCG](https://trustedcomputinggroup.org/resource/pc-client-specific-platform-firmware-profile-specification/), sous **descriptions des événements**. Si l' `-validate` indicateur est défini, valide que les valeurs de registre de configuration de plateforme (PCR) sur le module de plateforme sécurisée correspondent aux valeurs du journal.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="examples"></a><a name=tpmtool_examples></a>Illustre

Pour afficher les informations de base du module de plateforme sécurisée, tapez :
```
tpmtool getdeviceinformation
```
Pour collecter les journaux TPM et les placer dans le répertoire actif, tapez :
```
tpmtool gatherlogs
```
Pour collecter les journaux TPM et les placer `C:\Users\Public`dans, tapez :
```
tpmtool gatherlogs C:\Users\Public
```
Pour collecter les traces du pilote du module de plateforme sécurisée, tapez :
```
tpmtool drivertracing start
# Run scenario
tpmtool drivertracing stop
```
Pour analyser le journal TCG :
```
tpmtool parsetcglogs
```
Pour analyser le journal TCG et valider le PCR :
```
tpmtool parsetcglogs -validate
```

## <a name="decoding-error-codes"></a>Décodage des codes d’erreur

Les codes d’erreur spécifiques au module de plateforme sécurisée sont décrits [ici](https://docs.microsoft.com/windows/desktop/com/com-error-codes-6).
