---
title: sfc
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c58c25da-e028-42a6-9e10-973484a4b953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1db0ab81c9469c88ddb64a367a9dc98a1fd9b70c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832390"
---
# <a name="sfc"></a>sfc

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Analyse et vérifie l’intégrité du système protégé tous les fichiers et remplace les versions incorrectes avec les versions correctes.
Pour obtenir des exemples montrant comment utiliser cette commande, consultez [exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe
```
sfc [/scannow] [/verifyonly] [/scanfile=<file>] [/verifyfile=<file>] [/offwindir=<offline windows directory> /offbootdir=<offline boot directory>]
```

### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/scannow|Analyse l’intégrité de tous les fichiers protégés du système et répare les fichiers lorsque cela est possible.|
|/VERIFYONLY|Analyse l’intégrité de tous les fichiers protégés du système. Aucune opération de réparation n’est effectuée.|
|/scanfile|Analyse l’intégrité du fichier spécifié et répare le fichier si des problèmes sont détectés, lorsque cela est possible.|
|\<file>|Nom de fichier et chemin d’accès complet spécifié|
|/verifyfile|vérifie l’intégrité du fichier spécifié. Aucune opération de réparation n’est effectuée.|
|/offwindir|Spécifie l’emplacement du répertoire windows hors connexion, de réparation hors connexion.|
|/offbootdir|Spécifie l’emplacement du répertoire de démarrage hors connexion en mode hors connexion|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
-   Vous devez être connecté en tant que membre du groupe Administrateurs pour exécuter **sfc.exe**.
-   Si **sfc** détecte qu’un fichier protégé a été remplacé, il récupère la version correcte du fichier à partir de la **systemroot\system32\dllcache** dossier, puis remplace le fichier incorrect.
-   Il existe des différences fonctionnelles entre **sfc** sur Windows Server 2003, Windows Server 2008 et Windows Server 2008 R2 :
-   Pour plus d’informations sur **sfc** sur Windows Server 2003, consultez [article 310747](https://go.microsoft.com/fwlink/?LinkId=227069) dans la Base de connaissances Microsoft.
-   Pour plus d’informations sur **sfc** sur Windows Server 2008 et Windows Server 2008 R2, consultez [Vérificateur des fichiers système](https://go.microsoft.com/fwlink/?LinkId=227071).

## <a name="BKMK_examples"></a>Exemples
Pour vérifier la **fichier kernel32.dll**, type :
```
sfc /verifyfile=c:\windows\system32\kernel32.dll
```
Pour la réparation hors connexion le programme d’installation de le **kernel32.dll** fichier avec un répertoire de démarrage hors connexion la valeur **d:** et le répertoire windows hors connexion la valeur **d:\windows**, type :
```
sfc /scanfile=d:\windows\system32\kernel32.dll /offbootdir=d:\ /offwindir=d:\windows
```

## <a name="additional-references"></a>Références supplémentaires
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

