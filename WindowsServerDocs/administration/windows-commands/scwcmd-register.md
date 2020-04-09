---
title: Historique scwcmd
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fe4d126a-9f27-4076-b7b1-fbefa45f378a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5390b7d81efe8d807dd0b7d7a8c136a1d7092af3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835152"
---
# <a name="scwcmd-register"></a>Scwcmd: register

> S’applique à : Windows Server 2012 R2, Windows Server 2012

Étend ou personnalise la base de données de configuration de la sécurité de l’Assistant Configuration de la sécurité en inscrivant un fichier de base de données de configuration de sécurité qui contient des définitions de rôle, de tâche, de service ou de port.

## <a name="syntax"></a>Syntaxe

```
scwcmd register /kbname:<MyApp> [/kbfile:<kb.xml>] [/kb:<path>] [/d]
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/kbname :\<MyApp >|Spécifie le nom sous lequel l’extension de la base de données de configuration de sécurité sera inscrite. Ce paramètre doit être spécifié.|
|/kbfile :\<KB. Xml >|Spécifie le chemin d’accès et le nom de fichier du fichier de base de données de configuration de sécurité qui sera utilisé pour étendre ou personnaliser la base de données de configuration de sécurité de base. Pour confirmer que le fichier de base de données de configuration de sécurité est conforme au schéma de l’Assistant Configuration de la sécurité, utilisez le fichier de définition de schéma%windir%\security\KBRegistrationInfo.xsd. Cette option doit être fournie, sauf si le paramètre **/d** est spécifié.|
|/KB : chemin d’accès\<>|Spécifie le chemin d’accès au répertoire qui contient les fichiers de la base de données de configuration de sécurité SCW à mettre à jour. Si cette option n’est pas spécifiée,%windir%\security\msscw\kbs est utilisé.|
|/d|Annule l’inscription d’une extension de base de données de configuration de sécurité dans la base de données de configuration de sécurité. L’extension dont l’inscription doit être annulée est spécifiée par le paramètre/kbname. (Le paramètre **/kbfile** ne doit pas être spécifié.) La base de données de configuration de sécurité pour annuler l’inscription de l’extension est spécifiée par le paramètre **/KB** .|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

Scwcmd. exe est disponible uniquement sur les ordinateurs exécutant Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003.

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

Pour enregistrer le fichier de la base de données de configuration de sécurité nommé SCWKBForMyApp. XML sous le nom MyApp à l’emplacement \\\\kbserver\kb, tapez :
```
scwcmd register /kbfile:d:\SCWKBForMyApp.xml /kbname:MyApp /kb:\\kbserver\kb
```
Pour annuler l’inscription de la base de données de configuration de sécurité MyApp située dans \\\\kbserver\kb, tapez :
```
scwcmd register /d /kbname:MyApp /kb:\\kbserver\kb
```

## <a name="additional-references"></a>Références supplémentaires

-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)