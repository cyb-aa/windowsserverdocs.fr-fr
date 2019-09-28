---
title: Scwcmd transform
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 640dd892-0bb9-416d-8318-60a26605bcf4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 36ee3a99828c7fdd9d4fc0ca14cbc0e203b01ea0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384308"
---
# <a name="scwcmd-transform"></a>Scwcmd: transform

> S'applique à : Windows Server 2012 R2, Windows Server 2012

Transforme un fichier de stratégie de sécurité généré à l’aide de l’Assistant Configuration de la sécurité (SCW) en un nouvel objet de stratégie de groupe (GPO) dans Active Directory Domain Services. L’opération de transformation ne modifie pas les paramètres sur le serveur sur lequel elle est exécutée. Une fois l’opération de transformation terminée, un administrateur doit lier l’objet de stratégie de groupe aux unités d’organisation souhaitées pour déployer la stratégie sur les serveurs.

Les informations d’identification d’administrateur de domaine sont nécessaires pour terminer l’opération de transformation.

> [!IMPORTANT]
> Les paramètres de stratégie de sécurité Internet Information Services (IIS) ne peuvent pas être déployés à l’aide de stratégie de groupe.</br>> Stratégies de pare-feu qui répertorient les applications approuvées ne doivent pas être déployées sur les serveurs, sauf si le service pare-feu Windows a démarré automatiquement au dernier démarrage du serveur.

Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
scwcmd transform /p:<Policyfile.xml> /g:<GPODisplayName>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/p : @no__t -0Policyfile. Xml >|Spécifie le chemin d’accès et le nom de fichier du fichier de stratégie. XML à appliquer. Ce paramètre doit être spécifié.|
|/g : \<GPODisplayName >|Spécifie le nom complet de l’objet de stratégie de groupe. Ce paramètre doit être spécifié.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Scwcmd. exe est disponible uniquement sur les ordinateurs exécutant Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003.

## <a name="BKMK_Examples"></a>Illustre

Pour créer un objet de stratégie de groupe nommé FileServerSecurity à partir d’un fichier nommé FileServerPolicy. xml, tapez :
```
scwcmd transform /p:FileServerPolicy.xml /g:FileServerSecurity
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)