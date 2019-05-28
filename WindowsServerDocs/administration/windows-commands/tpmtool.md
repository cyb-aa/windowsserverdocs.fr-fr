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
ms.openlocfilehash: d2939b693c3f9bd8d0d8c8e1fefdd5d8f892e4c9
ms.sourcegitcommit: 3582c38d87f23cc54467d63bf00c29ef07cdb7c8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65517192"
---
>[!IMPORTANT]
>Certaines informations concernent la version préliminaire de produits susceptibles d’être considérablement modifiés d’ici leur commercialisation. Microsoft ne donne aucune garantie, expresse ou implicite, concernant les informations fournies ici.

# <a name="tpmtool"></a>tpmtool

Cet utilitaire peut être utilisé pour obtenir des informations le [du Module de plateforme sécurisée (TPM)](https://docs.microsoft.com/windows/security/information-protection/tpm/trusted-platform-module-overview).

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#tpmtool_examples).

## <a name="syntax"></a>Syntaxe

```
tpmtool /parameter [<arguments>]
```
## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|getdeviceinformation|Affiche les informations de base du module TPM.|
|GatherLogs [chemin d’accès de répertoire de sortie]|Collecte des journaux de module de plateforme sécurisée et les place dans le répertoire spécifié. Par défaut, ils sont placés dans le répertoire actif.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="tpmtool_examples"></a>Exemples

Pour afficher les informations de base du module TPM, tapez :
```
tpmtool getdeviceinformation
```
Pour collecte des journaux de module de plateforme sécurisée et placez-les dans le répertoire actif, type :
```
tpmtool gatherlogs
```
Pour collecte les journaux de module de plateforme sécurisée et de les placer dans `C:\Users\Public`, type :
```
tpmtool gatherlogs C:\Users\Public
```
