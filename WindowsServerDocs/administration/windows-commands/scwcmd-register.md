---
title: Scwcmd register
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: cc8b4a06af519b0da01dfcab8de0139b12cc68f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834970"
---
# <a name="scwcmd-register"></a>Scwcmd: register

> S'applique à : Windows Server 2012 R2, Windows Server 2012

Étend ou personnalise la base de données de la Configuration de la sécurité Assistant de Configuration de sécurité (SCW) en enregistrant un fichier de base de données de Configuration de sécurité qui contient le rôle, tâches, service ou les définitions de port.

## <a name="syntax"></a>Syntaxe

```
scwcmd register /kbname:<MyApp> [/kbfile:<kb.xml>] [/kb:<path>] [/d]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/kbname :\<MyApp >|Spécifie le nom sous lequel l’extension de la base de données de Configuration de sécurité sera être enregistrée. Ce paramètre doit être spécifié.|
|/kbfile :\<Kb.xml >|Spécifie le chemin d’accès et le nom du fichier de base de données de Configuration de sécurité qui sera utilisé pour étendre ou personnaliser la base de données de Configuration de sécurité. Pour valider que le fichier de base de données de Configuration de sécurité est compatible avec le schéma de l’Assistant, utilisez le fichier de définition de schéma %windir%\security\KBRegistrationInfo.xsd. Cette option doit être fournie, sauf si le **/d** est précisé.|
|/KB :\<chemin d’accès >|Spécifie le chemin d’accès au répertoire qui contient les fichiers de base de données de Configuration de sécurité SCW à mettre à jour. Si cette option n’est pas spécifiée, %windir%\security\msscw\kbs est utilisé.|
|/d|Annule l’inscription d’une extension de la base de données de Configuration de sécurité à partir de la base de données de Configuration de sécurité. L’extension pour annuler l’inscription est spécifiée par le paramètre /kbname. (Le **/kbfile** paramètre ne doit pas être spécifié.) Spécifié par la base de données de Configuration de sécurité pour annuler l’inscription de l’extension à partir de la **/kb** paramètre.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Scwcmd.exe est uniquement disponible sur les ordinateurs exécutant Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003.

## <a name="BKMK_Examples"></a>Exemples

Pour inscrire le fichier de base de données de Configuration de sécurité nommé SCWKBForMyApp.xml sous le nom MyApp dans l’emplacement \\ \\kbserver\kb, type :
```
scwcmd register /kbfile:d:\SCWKBForMyApp.xml /kbname:MyApp /kb:\\kbserver\kb
```
Pour annuler l’inscription de la MyApp de base de données de Configuration de sécurité situé dans \\ \\kbserver\kb, type :
```
scwcmd register /d /kbname:MyApp /kb:\\kbserver\kb
```

#### <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)