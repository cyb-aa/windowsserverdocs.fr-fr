---
title: Historique scwcmd
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe4d126a-9f27-4076-b7b1-fbefa45f378a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e892f7c08461e88d12a072dfb171f9523558ef7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371225"
---
# <a name="scwcmd-register"></a>Scwcmd: register

> S'applique à : Windows Server 2012 R2, Windows Server 2012

Étend ou personnalise la base de données de configuration de la sécurité de l’Assistant Configuration de la sécurité en inscrivant un fichier de base de données de configuration de sécurité qui contient des définitions de rôle, de tâche, de service ou de port.

## <a name="syntax"></a>Syntaxe

```
scwcmd register /kbname:<MyApp> [/kbfile:<kb.xml>] [/kb:<path>] [/d]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/kbname : \<MyApp >|Spécifie le nom sous lequel l’extension de la base de données de configuration de sécurité sera inscrite. Ce paramètre doit être spécifié.|
|/kbfile : @no__t -0Kb. Xml >|Spécifie le chemin d’accès et le nom de fichier du fichier de base de données de configuration de sécurité qui sera utilisé pour étendre ou personnaliser la base de données de configuration de sécurité de base. Pour confirmer que le fichier de base de données de configuration de sécurité est conforme au schéma de l’Assistant Configuration de la sécurité, utilisez le fichier de définition de schéma%windir%\security\KBRegistrationInfo.xsd. Cette option doit être fournie, sauf si le paramètre **/d** est spécifié.|
|/KB : \<Path >|Spécifie le chemin d’accès au répertoire qui contient les fichiers de la base de données de configuration de sécurité SCW à mettre à jour. Si cette option n’est pas spécifiée,%windir%\security\msscw\kbs est utilisé.|
|/d|Annule l’inscription d’une extension de base de données de configuration de sécurité dans la base de données de configuration de sécurité. L’extension dont l’inscription doit être annulée est spécifiée par le paramètre/kbname. (Le paramètre **/kbfile** ne doit pas être spécifié.) La base de données de configuration de sécurité pour annuler l’inscription de l’extension est spécifiée par le paramètre **/KB** .|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Scwcmd. exe est disponible uniquement sur les ordinateurs exécutant Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003.

## <a name="BKMK_Examples"></a>Illustre

Pour enregistrer le fichier de la base de données de configuration de sécurité nommé SCWKBForMyApp. XML sous le nom MyApp à l’emplacement \\ @ no__t-1kbserver\kb, tapez :
```
scwcmd register /kbfile:d:\SCWKBForMyApp.xml /kbname:MyApp /kb:\\kbserver\kb
```
Pour annuler l’inscription de la base de données de configuration de sécurité MyApp située à l’emplacement \\ @ no__t-1kbserver\kb, tapez :
```
scwcmd register /d /kbname:MyApp /kb:\\kbserver\kb
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)