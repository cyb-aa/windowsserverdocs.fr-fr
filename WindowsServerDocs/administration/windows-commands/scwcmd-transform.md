---
title: Scwcmd transformation
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8a6a6e37c2c2a362f3aa0aeadef615ff5065713f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843810"
---
# <a name="scwcmd-transform"></a>Scwcmd: transform

> S'applique à : Windows Server 2012 R2, Windows Server 2012

Transforme un fichier de stratégie de sécurité généré à l’aide de l’Assistant de Configuration de sécurité (SCW) dans un nouvel objet (stratégie de groupe) dans les Services de domaine Active Directory. L’opération de transformation ne modifie pas les paramètres sur le serveur où elle est effectuée. Une fois l’opération de transformation terminée, un administrateur doit lier le GPO pour les unités d’organisation souhaitées à déployer la stratégie sur les serveurs.

Informations d’identification d’administrateur de domaine sont nécessaires pour terminer l’opération de transformation.

> [!IMPORTANT]
> Paramètres de stratégie de sécurité Internet Information Services (IIS) ne peut pas être déployés à l’aide de stratégie de groupe.</br>> Les stratégies de pare-feu que d’applications approuvées ne doivent pas déployées sur des serveurs, sauf si le service de pare-feu de Windows démarre automatiquement lorsque le serveur a été démarré.

Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
scwcmd transform /p:<Policyfile.xml> /g:<GPODisplayName>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/ p:\<Policyfile.xml >|Spécifie le chemin d’accès et le nom du fichier de stratégie .xml qui doit être appliqué. Ce paramètre doit être spécifié.|
|/ g:\<GPODisplayName >|Spécifie le nom complet de l’objet de stratégie de groupe. Ce paramètre doit être spécifié.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Scwcmd.exe est uniquement disponible sur les ordinateurs exécutant Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003.

## <a name="BKMK_Examples"></a>Exemples

Pour créer un objet de stratégie de groupe nommé FileServerSecurity à partir d’un fichier nommé FileServerPolicy.xml, tapez :
```
scwcmd transform /p:FileServerPolicy.xml /g:FileServerSecurity
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)