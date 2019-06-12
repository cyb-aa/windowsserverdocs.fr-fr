---
title: tskill
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 08986e6a-6900-4ece-85a1-8f73b14db1b3 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b582334d7b79b2badbb86818be1093b6a5f55080
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440817"
---
# <a name="tskill"></a>tskill

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Met fin à un processus en cours d’exécution dans une session sur un serveur hôte de Session Bureau à distance (hôte de Session Bureau à distance).
Pour obtenir des exemples montrant comment utiliser cette commande, consultez [exemples](#BKMK_examples).

> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour savoir quelles sont les nouveautés dans la version la plus récente, consultez [les nouveautés des Services Bureau à distance dans Windows Server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.

## <a name="syntax"></a>Syntaxe
```
tskill {<ProcessID> | <ProcessName>} [/server:<ServerName>] [/id:<SessionID> | /a] [/v]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|\<ProcessID>|Spécifie l’ID du processus que vous souhaitez mettre fin.|
|\<ProcessName>|Spécifie le nom du processus que vous souhaitez mettre fin. Ce paramètre peut inclure des caractères génériques.|
|/ Server :\<nom_serveur >|Spécifie le serveur terminal server qui contient le processus que vous souhaitez mettre fin. Si **/server** n’est pas spécifié, le serveur hôte de Session Bureau à distance en cours est utilisé.|
|/ID :\<SessionID >|Met fin au processus qui s’exécute dans la session spécifiée.|
|/a|Met fin au processus qui s’exécute dans toutes les sessions.|
|/v|Affiche des informations sur les actions en cours d’exécution.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
- Vous pouvez utiliser **tskill** à la fin uniquement les processus qui vous appartiennent, sauf si vous êtes un administrateur. Les administrateurs ont un accès complet à tous les **tskill** fonctionne et peut mettre fin aux processus qui sont exécutent dans d’autres sessions utilisateur.
- Mettre fin à tous les processus qui sont exécutent dans une session, la session se termine également.
- Si vous utilisez le *ProcessName* et **/Server :** <em>nom_serveur</em> paramètres, vous devez également spécifier le **/id :**  <em>SessionID</em> ou **/a** paramètre.

## <a name="BKMK_examples"></a>Exemples
- Pour terminer le processus 6543, tapez :
  ```
  tskill 6543
  ```
- Pour terminer le processus « explorer » en cours d’exécution de la session 5, tapez :
  ```
  tskill explorer /id:5
  ```
  #### <a name="additional-references"></a>Références supplémentaires
  [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
  [des Services Bureau à distance &#40;Services Terminal Server&#41; référence de la commande](remote-desktop-services-terminal-services-command-reference.md)
